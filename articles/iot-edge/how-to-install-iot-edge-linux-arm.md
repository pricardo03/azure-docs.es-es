---
title: Instalación de Azure IoT Edge en Linux | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Linux en ARM32
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: e814785af7041ba762f7c383a0cfdc434fce9dce
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213879"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)

El entorno de ejecución de Azure IoT Edge se implementa en todos los dispositivos de IoT Edge. Tiene tres componentes. El **demonio de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo Edge. El demonio se inicia en cada inicio e inicia el agente de IoT Edge para arrancar el dispositivo. El **agente de IoT Edge** facilita la implementación y supervisión de los módulos en el dispositivo IoT Edge, incluido el centro de IoT Edge. El **centro de IoT Edge** administra las comunicaciones entre los módulos del dispositivo de IoT Edge y entre el dispositivo y la instancia de IoT Hub.

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge en un dispositivo Edge de Linux ARM32v7/armhf (por ejemplo, Raspberry Pi).

>[!NOTE]
>Los paquetes en los repositorios de software de Linux están sujetos a los términos de licencia que se encuentran en cada paquete (/usr/share/doc/*nombre-de-paquete*). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor

Azure IoT Edge utiliza un entorno de ejecución de contenedores [compatible con OCI][lnk-oci]. Para escenarios de producción, se recomienda encarecidamente utilizar el motor [basado en Moby][lnk-moby] que se proporciona a continuación. Es el único motor de contenedor compatible oficialmente con Azure IoT Edge. Las imágenes de los contenedores Docker CE/EE son compatibles con el entorno de ejecución basado en Moby.

Los siguientes comandos instalan tanto el motor basado en Moby como la interfaz de la línea de comandos (CLI). La CLI es útil para el desarrollo pero opcional para implementaciones de producción.

```cmd/sh

# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f

```

## <a name="install-the-iot-edge-security-daemon"></a>Instalación del demonio de seguridad de IoT Edge

```cmd/sh
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configuración del demonio de seguridad de Azure IoT Edge


El demonio se puede configurar mediante el archivo de configuración en `/etc/iotedge/config.yaml`. De forma predeterminada el archivo está protegido contra escritura, puede que necesite permisos elevados para editarlo.

```bash
sudo nano /etc/iotedge/config.yaml
```

El dispositivo Edge se puede configurar manualmente mediante una [cadena de conexión de dispositivo][lnk-dcs] o [automáticamente a través de Device Provisioning Service][lnk-dps].

* Para la configuración manual, quite el comentario del modo de aprovisionamiento **manual**. Actualice el valor de **device_connection_string** con la cadena de conexión desde un dispositivo IoT Edge.

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

* Para la configuración automática, quite el comentario del modo de aprovisionamiento **dps**. Actualice los valores de **scope_id** y **registration_id** con los valores de la instancia de DPS de IoT Hub y el dispositivo IoT Edge con TPM. 

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

Guarde y cierre el archivo. 

   `CTRL + X`, `Y`, `Enter`

Después de introducir la información de aprovisionamiento en la configuración, reinicie el demonio:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si uso los pasos de **configuración manual** de la sección anterior, el entorno de ejecución de IoT Edge debe haberse aprovisionado correctamente y estar en ejecución en el dispositivo. Si ha usado los pasos de **configuración automática**, deberá completar algunos pasos adicionales para que el entorno de ejecución pueda registrar el dispositivo con IoT Hub en su nombre. Para los pasos siguientes, consulte la sección de [Create and provision a simulated TPM Edge device on a Linux virtual machine](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm) (Creación y aprovisionamiento de un dispositivo TPM Edge en una máquina virtual Linux).

Puede comprobar el estado del demonio de IoT Edge mediante lo siguiente:

```cmd/sh
systemctl status iotedge
```

Examine los registros del demonio con:

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Y, enumere los módulos en ejecución con:

```cmd/sh
sudo iotedge list
```
>[!NOTE]
>En dispositivos con recursos limitados como RaspberryPi, se recomienda encarecidamente que la variable de entorno *OptimizeForPerformance* se establezca en *false* según las instrucciones de la [guía de resolución de problemas][lnk-trouble].


## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas con la instalación correcta del entorno de ejecución de IoT Edge, consulte la página de [solución de problemas][lnk-trouble].

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-trouble]: https://docs.microsoft.com/azure/iot-edge/troubleshoot#stability-issues-on-resource-constrained-devices
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
