---
title: Cómo instalar Azure IoT Edge en Windows con contenedores de Linux | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Windows con contenedores de Linux
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: kgremban
ms.openlocfilehash: 86a24c2cdba890046f2cbb0ccf6b73decdac7235
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997529"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Instalación del entorno de ejecución de Azure IoT Edge en Windows para su uso con contenedores de Linux

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. 

Para más información acerca de cómo funciona el entorno de ejecución de Azure IoT Edge y los componentes que se incluyen, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge con contenedores Linux en el sistema Windows x64 (AMD e Intel). Esta compatibilidad con Windows se encuentra actualmente en versión preliminar.

>[!NOTE]
El uso de contenedores Linux en sistemas Windows no es una configuración de producción recomendada o compatible con Azure IoT Edge. Sin embargo, se puede usar con fines de desarrollo y pruebas.

## <a name="supported-windows-versions"></a>Versiones de Windows admitidas
Azure IoT Edge se puede usar para el desarrollo y pruebas en las siguientes versiones de Windows, si se usan contenedores de Linux.
  * Windows 10 o sistemas operativos de escritorio más recientes.
  * Windows Server 2016 o sistemas operativos de servidor más recientes.

Para más información acerca de qué sistemas operativos se admiten actualmente, consulte [Compatibilidad de Azure IoT Edge](support.md#operating-systems). 

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor 

Azure IoT Edge se basa en un entorno de ejecución de contenedor [compatible con OCI][lnk-oci] (por ejemplo, Docker). 

Se puede usar [Docker para Windows][lnk-docker-for-windows] para desarrollo y pruebas. Configure Docker para Windows [para utilizar los contenedores de Linux][lnk-docker-config].

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instalación del demonio de seguridad de Azure IoT Edge

>[!NOTE]
>Los paquetes de software de Azure IoT Edge están sujetos a los términos de licencia ubicados en los paquetes (en el directorio LICENSE). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexiones de dispositivo proporcionada por IoT Hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para aprovisionar. Según la elección de aprovisionamiento, elija el script de instalación adecuado. 

### <a name="option-1-install-and-manually-provision"></a>Opción 1: Instalación y aprovisionamiento manual

1. Siga los pasos de [Registro de un nuevo dispositivo Azure IoT Edge desde Azure Portal][lnk-dcs] para registrar el dispositivo y recuperar la cadena de conexión de dicho dispositivo. 

2. En el dispositivo IoT Edge, ejecute PowerShell como administrador. 

3. Descargue e instale el entorno de ejecución de IoT Edge. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Manual -ContainerOs Linux
   ```

4. Cuando se le pida **DeviceConnectionString**, proporcione la cadena de conexión que recuperó de IoT Hub. No incluya comillas para la cadena de conexión. 

### <a name="option-2-install-and-automatically-provision"></a>Opción 2: Instalación y aprovisionamiento automático

1. Siga los pasos de [Creación y aprovisionamiento de un dispositivo TPM Edge simulado en Windows][lnk-dps] para configurar el servicio Device Provisioning y recuperar su **identificador de ámbito**, simular un dispositivo TPM y recuperar su **identificador de registro**; luego, cree una inscripción individual. Una vez que el dispositivo esté registrado en IoT Hub, continúe con la instalación.  

   >[!TIP]
   >Mantenga abierta la ventana que se está ejecutando el simulador de TPM durante la instalación y prueba. 

2. En el dispositivo IoT Edge, ejecute PowerShell como administrador. 

3. Descargue e instale el entorno de ejecución de IoT Edge. 

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Install-SecurityDaemon -Dps -ContainerOs Linux
   ```

4. Cuando se le solicite, proporcione **ScopeID** y **RegistrationID** para su servicio de aprovisionamiento y el dispositivo.

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Puede comprobar el estado del servicio IoT Edge mediante: 

```powershell
Get-Service iotedge
```

Examine los registros de servicio de los últimos 5 minutos con:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Y, enumere los módulos en ejecución con:

```powershell
iotedge list
```

## <a name="tips-and-suggestions"></a>Recomendaciones y sugerencias

Si la red tiene un servidor proxy, siga los pasos descritos en [Configure your IoT Edge device to communicate through a proxy server](how-to-configure-proxy-support.md) (Configuración del dispositivo IoT Edge para comunicarse mediante un servidor proxy).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo de IoT Edge aprovisionado con el entorno de ejecución instalado, puede [implementar módulos de IoT Edge][lnk-modules].

Si tiene problemas con la instalación correcta del entorno de ejecución de Edge, consulte la página de [solución de problemas][lnk-trouble].


<!-- Images -->
[img-docker-nat]: ./media/how-to-install-iot-edge-windows-with-linux/dockernat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-modules]: how-to-deploy-modules-portal.md
