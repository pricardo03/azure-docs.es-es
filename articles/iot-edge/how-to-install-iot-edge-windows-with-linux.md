---
title: Cómo instalar Azure IoT Edge en Windows con contenedores de Linux | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Windows con contenedores de Linux
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: cd517d7e652b38c7ecf28a17657936698416413a
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035202"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-linux-containers"></a>Instalación del entorno de ejecución de Azure IoT Edge en Windows para su uso con contenedores de Linux

El entorno de ejecución de Azure IoT Edge se implementa en todos los dispositivos IoT Edge. Tiene tres componentes. El **demonio de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo Edge. El demonio se inicia en cada inicio e inicia el agente de IoT Edge para arrancar el dispositivo. El **agente de IoT Edge** facilita la implementación y supervisión de los módulos en el dispositivo Edge, incluido el centro de IoT Edge. El **centro de IoT Edge** administra las comunicaciones entre los módulos del dispositivo de IoT Edge y entre el dispositivo y la instancia de IoT Hub.

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge en el sistema Windows x64 (AMD e Intel). Esta compatibilidad con Windows se encuentra actualmente en versión preliminar.

>[!NOTE]
El uso de contenedores Linux en sistemas Windows no es una configuración de producción recomendada o compatible con Azure IoT Edge. Sin embargo, se puede usar con fines de desarrollo y pruebas.

## <a name="supported-windows-versions"></a>Versiones de Windows admitidas
Azure IoT Edge se puede usar para el desarrollo y pruebas en las siguientes versiones de Windows, si se usan contenedores de Linux.
  * Windows 10 o sistemas operativos de escritorio más recientes.
  * Windows Server 2016 o sistemas operativos de servidor más recientes.

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor 

Azure IoT Edge se basa en un entorno de ejecución de contenedor [compatible con OCI][lnk-oci] (por ejemplo, Docker). 

Se puede usar [Docker para Windows][lnk-docker-for-windows] para desarrollo y pruebas. Asegúrese de que Docker para Windows está [configurado para utilizar los contenedores de Linux][lnk-docker-config].

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instalación del demonio de seguridad de Azure IoT Edge

>[!NOTE]
>Los paquetes de software de Azure IoT Edge están sujetos a los términos de licencia ubicados en los paquetes (en el directorio LICENSE). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

### <a name="download-the-edge-daemon-package-and-install"></a>Descarga e instalación del paquete del demonio de Edge

En una ventana de PowerShell como administrador, ejecute los siguientes comandos:

```powershell
Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
rmdir C:\ProgramData\iotedge\iotedged-windows
$env:Path += ";C:\ProgramData\iotedge"
SETX /M PATH "$env:Path"
```

Instale vcruntime mediante:

```powershell
Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
.\vc_redist.exe /quiet /norestart
 ```

Cree e inicie el servicio *iotedge*:

```powershell
New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
Start-Service iotedge
```

Agregue las excepciones del firewall para los puertos que usa el servicio:

```powershell
New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
```

Cree un archivo **iotedge.reg** con el siguiente contenido e impórtelo en el Registro de Windows haciendo doble clic en él o mediante el comando `reg import iotedge.reg`:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configuración del demonio de seguridad de Azure IoT Edge

El demonio se puede configurar mediante el archivo de configuración en `C:\ProgramData\iotedge\config.yaml`. El dispositivo Edge se puede configurar <!--[automatically via Device Provisioning Service][lnk-dps] or--> manualmente mediante una [cadena de conexión del dispositivo][lnk-dcs].

Para realizar la configuración manual, escriba la cadena de conexión del dispositivo en la sección **provisioning:** de **config.yaml**

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

Obtenga el nombre del dispositivo Edge mediante el comando `hostname` de PowerShell y establézcalo como valor de **hostname:** en el archivo YAML de configuración. Por ejemplo: 

```yaml
  ###############################################################################
  # Edge device hostname
  ###############################################################################
  #
  # Configures the environment variable 'IOTEDGE_GATEWAYHOSTNAME' injected into
  # modules.
  #
  ###############################################################################

  hostname: "edgedevice-1"
```

A continuación, deberá proporcionar la dirección IP y el puerto para **workload_uri** y **management_uri** en la sección **connect:** de la configuración.

Para la dirección IP, escriba `ipconfig` en la ventana de PowerShell y seleccione la dirección IP de la interfaz de **vEthernet (DockerNAT)** como se indica en el siguiente ejemplo (la dirección IP del sistema puede ser diferente):

![DockerNat][img-docker-nat]

```yaml
connect:
  management_uri: "http://10.0.75.1:15580"
  workload_uri: "http://10.0.75.1:15581"
```

Escriba las mismas direcciones de la sección **listen** de la configuración. Por ejemplo: 

```yaml
listen:
  management_uri: "http://10.0.75.1:15580"
  workload_uri: "http://10.0.75.1:15581"
```

En la ventana de PowerShell, cree una variable de entorno **IOTEDGE_HOST** con la dirección **management_uri**. Por ejemplo:

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://10.0.75.1:15580")
```

Por último, asegúrese de haber quitado la marca de comentario de la configuración de **network:** en **moby_runtime:** y que se ha establecido en **azure-iot-edge**.

```yaml
moby_runtime:
  docker_uri: "npipe://./pipe/docker_engine"
  network: "azure-iot-edge"
```

Guarde el archivo de configuración y reinicie el servicio:

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

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
  sort-object @{Expression="TimeCreated";Descending=$false}
```

Y, enumere los módulos en ejecución con:

```powershell
iotedge list
```

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas con la instalación correcta del entorno de ejecución de IoT Edge, consulte la página de [solución de problemas][lnk-trouble].


<!-- Images -->
[img-docker-nat]: ./media/how-to-install-iot-edge-windows-with-linux/dockernat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows

