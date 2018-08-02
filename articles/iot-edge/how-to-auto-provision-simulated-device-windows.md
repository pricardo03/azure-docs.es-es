---
title: 'Aprovisionamiento automático de dispositivos Azure IoT Edge con DPS: Windows | Microsoft Docs'
description: Use un dispositivo simulado en el equipo Windows para probar el aprovisionamiento automático de dispositivos para Azure IoT Edge con Device Provisioning Service
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e149886e1ade80d7751f58eb1f77031c4e432b75
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39307950"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Creación y aprovisionamiento de un dispositivo TPM Edge simulado en Windows

Los dispositivos Azure IoT Edge pueden aprovisionarse automáticamente con [Device Provisioning Service](../iot-dps/index.yml), igual que los dispositivos que no están habilitados para Edge. Si no está familiarizado con el proceso de aprovisionamiento automático, revise los [conceptos sobre el aprovisionamiento automático](../iot-dps/concepts-auto-provisioning.md) antes de continuar. 

En este artículo se muestra cómo probar el aprovisionamiento automático en un dispositivo Edge simulado con los pasos siguientes: 

* Cree una instancia de IoT Hub Device Provisioning Service (DPS).
* Cree un dispositivo simulado en una máquina Windows con un Módulo de plataforma segura (TPM) simulado para la seguridad de hardware.
* Cree una inscripción individual para el dispositivo.
* Instale el entorno de ejecución de IoT Edge y conecte el dispositivo a IoT Hub.

## <a name="prerequisites"></a>Requisitos previos

* Una máquina de desarrollo Windows. En este artículo se usa Windows 10. 
* Una instancia de IoT Hub activa. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Configuración de IoT Hub Device Provisioning Service

Cree una nueva instancia de IoT Hub Device Provisioning Service en Azure y vincúlela a IoT Hub. Puede seguir las instrucciones de [Configuración de IoT Hub DPS](../iot-dps/quick-setup-auto-provision.md).

Cuando Device Provisioning Service esté en ejecución, copie el valor de **Ámbito de id.** de la página de información general. Use este valor cuando configure el entorno de ejecución de IoT Edge. 

## <a name="simulate-a-tpm-device"></a>Simulación del dispositivo de TPM

Cree un dispositivo TPM simulado en la máquina de desarrollo Windows. Recupere el **Id. de registro** y la **Clave de aprobación** para el dispositivo y úselos para crear una entrada de inscripción individual en DPS. 

Al crear una inscripción en DPS, tiene la oportunidad de declarar un **Estado inicial de dispositivo gemelo**. En el dispositivo gemelo, puede establecer etiquetas para agrupar dispositivos por cualquier métrica que necesite en su solución, como la región, el entorno, la ubicación o el tipo de dispositivo. Estas etiquetas se usan para crear [implementaciones automáticas](how-to-deploy-monitor.md). 

Elija el lenguaje del SDK que desea usar para crear el dispositivo simulado y siga los pasos hasta que se cree la inscripción individual. 

Cuando cree la inscripción individual, seleccione **Habilitar** para declarar que esta máquina virtual es un **dispositivo IoT Edge**.

Guías de dispositivos simulados e inscripción individual: 
* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Después de crear la inscripción individual, guarde el valor del **Id. de registro**. Use este valor cuando configure el entorno de ejecución de IoT Edge. 

## <a name="install-the-iot-edge-runtime"></a>Instalación del entorno de ejecución de IoT Edge

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Sus componentes se ejecutan en contenedores y permiten implementar contenedores adicionales en el dispositivo para que pueda ejecutar código en Edge. En los dispositivos que ejecutan Windows, puede usar contenedores Windows o contenedores Linux. Elija el tipo de contenedor que desea utilizar y siga los pasos. Asegúrese de configurar el entorno de ejecución de IoT Edge para el aprovisionamiento automático, no manual. 

Siga las instrucciones para instalar el entorno de ejecución de IoT Edge en el dispositivo que ejecuta el TPM simulado de la sección anterior. 

Averigüe el **Ámbito de id.** de DPS y el **Id. de registro** del dispositivo antes de comenzar estos artículos. 

* [Contenedores de Windows](how-to-install-iot-edge-windows-with-windows.md)
* [Contenedores de Linux](how-to-install-iot-edge-windows-with-linux.md)

## <a name="create-a-tpm-environment-variable"></a>Creación de una variable de entorno de TPM

En la máquina que ejecuta el dispositivo simulado, modifique el registro del servicio **iotedge** para establecer una variable de entorno.

1. Desde el menú **Inicio**, abra **regedit**. 
2. Vaya a **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\iotedge**. 
3. Seleccione **Edición** > **Nuevo** > **Valor de cadena múltiple**. 
4. Escriba el nombre **Environment**. 
5. Haga doble clic en la nueva variable y establezca el valor en **IOTEDGE_USE_TPM_DEVICE=ON**. 
6. Haga clic en **Aceptar** para guardar los cambios. 

## <a name="restart-the-iot-edge-runtime"></a>Reinicio del entorno de ejecución de IoT Edge

Reinicie el entorno de ejecución de IoT Edge para que aplique todos los cambios de configuración realizados en el dispositivo. 

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si el entorno de ejecución se inició correctamente, puede ir a IoT Hub y ver que el nuevo dispositivo se aprovisionó automáticamente y está listo para ejecutar módulos de IoT Edge. 

Compruebe el estado del servicio IoT Edge.

```powershell
Get-Service iotedge
```

Examine los registros de servicio de los últimos 5 minutos.

```powershell
# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Enumere los módulos en ejecución.

```powershell
iotedge list
```

## <a name="next-steps"></a>Pasos siguientes

El proceso de inscripción en Device Provisioning Service permite establecer la id. de dispositivo y las etiquetas del dispositivo gemelo al mismo tiempo que aprovisiona el nuevo dispositivo. Puede usar esos valores para dirigirse a dispositivos individuales o grupos de dispositivos con la administración automática de dispositivos. Obtenga información sobre la [Implementación y supervisión de módulos de IoT Edge a escala mediante Azure Portal](how-to-deploy-monitor.md) o [mediante la CLI de Azure](how-to-deploy-monitor-cli.md)