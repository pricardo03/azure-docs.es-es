---
title: Instalar Azure IoT Edge para Linux en Windows | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge para contenedores de Linux en Windows 10, Windows Server y Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: kgremban
ms.openlocfilehash: b7386cbbe18d7e05c2fbffb96f6214b468956192
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151701"
---
# <a name="use-iot-edge-on-windows-to-run-linux-containers"></a>Uso de IoT Edge en Windows para ejecutar contenedores de Linux

Pruebe los módulos de IoT Edge para dispositivos de Linux mediante una máquina de Windows. 

En un escenario de producción, los dispositivos de Windows solo deben ejecutar contenedores de Windows. Sin embargo, un escenario de desarrollo común es usar un equipo de Windows para crear módulos para los dispositivos de Linux de IoT Edge. El tiempo de ejecución de IoT Edge para Windows le permite ejecutar contenedores de Linux para **desarrollo y pruebas** fines. 

Este artículo enumeran los pasos para instalar el runtime de Azure IoT Edge con contenedores de Linux en el Windows x64 (AMD e Intel) sistema. Para obtener más información sobre el instalador IoT Edge en tiempo de ejecución, incluidos los detalles acerca de todos los parámetros de instalación, consulte [instalar el runtime de Azure IoT Edge en Windows](how-to-install-iot-edge-windows.md).

## <a name="prerequisites"></a>Requisitos previos

Utilice esta sección para revisar si el dispositivo Windows puede admitir IoT Edge y para prepararlo para un motor de contenedor antes de la instalación. 

### <a name="supported-windows-versions"></a>Versiones de Windows admitidas

Puede ejecutar Azure IoT Edge con contenedores de Linux en las siguientes versiones de Windows: 
* Actualización de aniversario de Windows 10 (compilación 14393) o posterior
* Windows Server 2016 o posterior

Para obtener más información sobre qué se incluye en la versión más reciente de IoT Edge, vea las [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Si desea instalar IoT Edge en una máquina virtual, habilitar la virtualización anidada y asigne al menos 2 GB de memoria. Cómo habilitar la virtualización anidada es diferente dependiendo de su uso en el hipervisor. Para Hyper-V, máquinas virtuales de generación 2 anidadas virtualización habilitada de forma predeterminada. Para VMWare, hay un botón de alternancia para habilitar la característica en la máquina virtual. 

### <a name="prepare-the-container-engine"></a>Preparar el motor de contenedor 

Azure IoT Edge se basa en un motor de contenedor [compatible con OCI](https://www.opencontainers.org/). La principal diferencia de la configuración entre contenedores de Windows y Linux en ejecución en un equipo es que la instalación de IoT Edge incluye un tiempo de ejecución del contenedor de Windows, pero deberá proporcionar su propio tiempo de ejecución para contenedores de Linux antes de instalar IoT Edge de Windows. 

Para configurar una máquina de Windows para desarrollar y probar contenedores para los dispositivos de Linux, puede usar [Docker Desktop](https://www.docker.com/docker-windows) como motor de contenedor. Deberá instalar Docker y configurarlo de modo que [usar contenedores de Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers) antes de instalar IoT Edge.  

Si su dispositivo IoT Edge es un equipo Windows, compruebe que cumple los [requisitos del sistema](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) para Hyper-V.

## <a name="install-iot-edge-on-a-new-device"></a>Instalación de IoT Edge en un dispositivo nuevo

>[!NOTE]
>Los paquetes de software de Azure IoT Edge están sujetos a los términos de licencia ubicados en los paquetes (en el directorio LICENSE). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

Un script de PowerShell descarga e instala el demonio de seguridad de Azure IoT Edge. Después, el demonio de seguridad inicia el primero de los dos módulos en tiempo de ejecución, el agente de IoT Edge, que permite implementaciones remotas de otros módulos. 

Cuando instale por primera vez el tiempo de ejecución de IoT Edge en un dispositivo, debe aprovisionar el dispositivo con una identidad de un IoT hub. Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexiones de dispositivo proporcionada por IoT hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para configurar. 

Puede leer más sobre las distintas opciones de instalación y los parámetros en el artículo [instalar el runtime de Azure IoT Edge en Windows](how-to-install-iot-edge-windows.md). Una vez que el escritorio de Docker instalado y configurado para contenedores de Linux, la diferencia de la instalación principal está declarando Linux con la **ContainerOs -** parámetro. Por ejemplo: 

1. Si no lo ha hecho ya, registre un nuevo dispositivo de IoT Edge y recuperar la cadena de conexión del dispositivo. Copie la cadena de conexión para usarlo más adelante en esta sección. Puede completar este paso con las siguientes herramientas:

   * [Azure Portal](how-to-register-device-portal.md)
   * [CLI de Azure](how-to-register-device-cli.md)
   * [Visual Studio Code](how-to-register-device-vscode.md)

2. Ejecute PowerShell como administrador.

   >[!NOTE]
   >Utilice una sesión AMD64 de PowerShell para instalar IoT Edge, no PowerShell (x86). Si no está seguro de qué tipo de sesión que está usando, ejecute el siguiente comando:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. El **implementar IoTEdge** comando comprueba que el equipo de Windows está en una versión compatible, activa la característica de contenedores y, a continuación, descarga el tiempo de ejecución moby (que no se usa para contenedores de Linux) y el tiempo de ejecución de IoT Edge. Los valores predeterminados de comando a contenedores de Windows, así que declare Linux como sistema operativo de contenedor que desee. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Linux
   ```

4. En este momento, los dispositivos de IoT Core pueden reiniciarse automáticamente. Otros dispositivos Windows 10 o Windows Server pueden pedirle que reinicie. Si es así, reinicie ahora el dispositivo. Una vez que el dispositivo está listo, ejecute PowerShell como administrador de nuevo.

5. El comando **Initialize-IoTEdge** configura el entorno de ejecución de Azure IoT Edge en el equipo. El comando tiene como valor predeterminado para el aprovisionamiento manual con una cadena de conexión del dispositivo. Declarar Linux como sistema operativo deseado contenedor nuevo. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ContainerOs Linux
   ```

6. Cuando se le solicite, proporcione la cadena de conexión de dispositivo que recuperó en el paso 1. La cadena de conexión del dispositivo, asocia el dispositivo físico con un identificador de dispositivo en IoT Hub. 

   La cadena de conexión de dispositivo tiene el formato siguiente y no debe incluir entre comillas: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Compruebe el estado del servicio IoT Edge. Esta debe aparecer como en ejecución.  

```powershell
Get-Service iotedge
```

Examine los registros de servicio de los últimos 5 minutos. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Enumere los módulos en ejecución. Después de una instalación nueva, el módulo solo debería ver la ejecución es **edgeAgent**. Después de [implementar módulos IoT Edge](how-to-deploy-modules-portal.md), verá que otros usuarios. 

```powershell
iotedge list
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge aprovisionado con el entorno de ejecución instalado, puede [implementar módulos de IoT Edge](how-to-deploy-modules-portal.md).

Si tiene problemas con la instalación correcta de IoT Edge, vea la página de [solución de problemas](troubleshoot.md).

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).
