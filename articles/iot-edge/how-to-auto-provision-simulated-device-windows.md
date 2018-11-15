---
title: 'Aprovisionamiento automático de dispositivos Azure IoT Edge con DPS: Windows | Microsoft Docs'
description: Use un dispositivo simulado en el equipo Windows para probar el aprovisionamiento automático de dispositivos para Azure IoT Edge con Device Provisioning Service
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9be790d9b512dc9338cf183240430ad0ada3bef4
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51565112"
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

Después de completar la sección anterior, debería ver el nuevo dispositivo mostrado como dispositivo de IoT Edge en IoT Hub. Ahora, necesita instalar el entorno de ejecución de Azure IoT Edge en el dispositivo. 

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Sus componentes se ejecutan en contenedores y permiten implementar contenedores adicionales en el dispositivo para que pueda ejecutar código en Edge. En los dispositivos que ejecutan Windows, puede usar contenedores Windows o contenedores Linux. Elija el tipo de contenedor que desea utilizar y siga los pasos. Asegúrese de configurar el entorno de ejecución de IoT Edge para el aprovisionamiento automático, no manual. 

Siga las instrucciones para instalar el entorno de ejecución de IoT Edge en el dispositivo que ejecuta el TPM simulado de la sección anterior. 

Averigüe el **Ámbito de id.** de DPS y el **Id. de registro** del dispositivo antes de comenzar estos artículos. 

* [Contenedores de Windows](how-to-install-iot-edge-windows-with-windows.md)
* [Contenedores de Linux](how-to-install-iot-edge-windows-with-linux.md)

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si el entorno de ejecución se inició correctamente, puede ir a IoT Hub y empezar a implementar módulos de IoT Edge en el dispositivo. Use los siguientes comandos en el dispositivo para comprobar que el entorno de ejecución está instalado e iniciado correctamente.  

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
