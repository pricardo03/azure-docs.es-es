---
title: Instalación de Azure IoT Edge en Windows con contenedores de Windows | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Windows con contenedores de Windows
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: 3d34628a5a47788bca8cdafcb6e199a0c2cb3bcc
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437848"
---
# <a name="install-azure-iot-edge-runtime-on-windows-to-use-with-windows-containers"></a>Instalación del entorno de ejecución de Azure IoT Edge en Windows para usar con contenedores de Windows

El entorno de ejecución de IoT Edge se implementa en todos los dispositivos IoT Edge. Tiene tres componentes. El **demonio de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo IoT Edge. El demonio se inicia en cada inicio y arranca el dispositivo iniciando el agente de IoT Edge. El **agente de IoT Edge** facilita la implementación y supervisión de los módulos en el dispositivo IoT Edge, incluido el centro de IoT Edge. El **centro de IoT Edge** administra las comunicaciones entre los módulos del dispositivo de IoT Edge y entre el dispositivo y la instancia de IoT Hub.

En este artículo se enumeran los pasos para instalar el entorno de ejecución de IoT Edge en el sistema Windows x64 (AMD e Intel). 

Esta compatibilidad con Windows se encuentra actualmente en versión preliminar.

## <a name="supported-windows-versions"></a>Versiones de Windows admitidas
Se puede utilizar Azure IoT Edge con contenedores de Windows con:
  * Windows 10/IoT Enterprise/IoT Core con la actualización de abril de 2018 (compilación 17134).
  * Windows Server 1803

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor 

>[!NOTE]
>Para la instalación del motor de contenedor en Windows IoT Core, siga los pasos descritos en el artículo [Aprovisionamiento de un dispositivo de IoT Core][lnk-iot-core] y, después, continúe con las instrucciones siguientes.

Azure IoT Edge se basa en un entorno de ejecución de contenedor [compatible con OCI][lnk-oci] (por ejemplo, Docker). Puede usar [Docker para Windows][lnk-docker-for-windows] para desarrollo y pruebas. 

**Asegúrese de Docker para Windows está [configurado para utilizar los contenedores de Windows][lnk-docker-config]**

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

Instale vcruntime mediante la siguiente acción (puede omitir este paso en un dispositivo IoT Core Edge):

```powershell
Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
.\vc_redist.exe /quiet /norestart
 ```

Cree e inicie el servicio **iotedge**:

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

El demonio se puede configurar mediante el archivo de configuración en `C:\ProgramData\iotedge\config.yaml`.

El dispositivo Edge se puede configurar manualmente mediante una [cadena de conexión de dispositivo][lnk-dcs] o [automáticamente a través del servicio aprovisionamiento de dispositivos][lnk-dps].

* Para la configuración manual, quite el comentario de modo de aprovisionamiento **manual**. Actualice el valor de **device_connection_string** con la cadena de conexión desde un dispositivo IoT Edge.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

* Para la configuración automática, quite el comentario de modo de aprovisionamiento **dps**. Actualice los valores de **scope_id** y **registration_id** con los valores de la instancia de DPS de IoT Hub y el dispositivo IoT Edge con TPM. 

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Obtenga el nombre del dispositivo Edge mediante el comando `hostname` de PowerShell y establézcalo como valor de **hostname:** en el archivo yaml de configuración. Por ejemplo: 

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

A continuación, proporcione la dirección IP y el puerto para **workload_uri** y **management_uri** en las secciones de configuración **connect:** y **listen:**.

Para recuperar la dirección IP, escriba `ipconfig` en la ventana de PowerShell y copie la dirección IP de la interfaz de **vEthernet (nat)** como se indica en el siguiente ejemplo (la dirección IP del sistema puede ser diferente):  

![NAT][img-nat]

Actualice las secciones **workload_uri** y **management_uri** en la sección **connect:** del archivo de configuración. Reemplace **\<GATEWAY_ADDRES\>** con la dirección IP que ha copiado. 

```yaml
connect:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

Escriba las mismas direcciones en la sección **listen:** de la configuración, con la dirección IP como la dirección de puerta de enlace.

```yaml
listen:
  management_uri: "http://<GATEWAY_ADDRESS>:15580"
  workload_uri: "http://<GATEWAY_ADDRESS>:15581"
```

En la ventana de PowerShell, cree una variable de entorno **IOTEDGE_HOST** con la dirección **management_uri**.

```powershell
[Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<GATEWAY_ADDRESS>:15580")
```

Conserve la variable de entorno entre los reinicios.

```powershell
SETX /M IOTEDGE_HOST "http://<GATEWAY_ADDRESS>:15580"
```

Finalmente, asegúrese de que a la configuración de **network:** en **moby_runtime:** se le ha quitado la marca de comentario y se ha establecido en **nat**

```yaml
moby_runtime:
  docker_uri: "npipe://./pipe/docker_engine"
  network: "nat"
```

Guarde el archivo de configuración y reinicie el servicio:

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si ha usado los pasos de **configuración manual** en la sección anterior, el tiempo de ejecución de IoT Edge debe haber sido aprovisionado correctamente y se debe ejecutar en el dispositivo. Si ha usado los pasos de **configuración automática**, deberá completar algunos pasos adicionales para que el tiempo de ejecución pueda registrar el dispositivo con IoT Hub en su nombre. Para los pasos siguientes, consulte el artículo [Creación y aprovisionamiento de un dispositivo TPM Edge simulado en Windows](how-to-auto-provision-simulated-device-windows.md#create-a-tpm-environment-variable).

Puede comprobar el estado del servicio IoT Edge mediante: 

```powershell
Get-Service iotedge
```

El examen de los registros de servicio de los últimos 5 minutos con:

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
[img-nat]: ./media/how-to-install-iot-edge-windows-with-windows/nat.png

<!-- Links -->
[lnk-docker-config]: https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-windows.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
[lnk-docker-for-windows]: https://www.docker.com/docker-windows
[lnk-iot-core]: how-to-install-iot-core.md

