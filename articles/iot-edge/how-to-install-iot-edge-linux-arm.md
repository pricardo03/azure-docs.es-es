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
ms.openlocfilehash: ad70fcc6b9779cb33772a3fce2fb11b4cec804ee
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062605"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)

El entorno de ejecución de Azure IoT Edge se implementa en todos los dispositivos de IoT Edge. Tiene tres componentes. El **demonio de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo IoT Edge. El demonio se inicia en cada inicio y arranca el dispositivo iniciando el agente de IoT Edge. El **agente de IoT Edge** facilita la implementación y supervisión de los módulos en el dispositivo IoT Edge, incluido el centro de IoT Edge. El **centro de IoT Edge** administra las comunicaciones entre los módulos del dispositivo de IoT Edge y entre el dispositivo y la instancia de IoT Hub.

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge en un dispositivo Edge de Linux ARM32v7/armhf (por ejemplo, Raspberry Pi).

>[!NOTE]
>Los paquetes en los repositorios de software de Linux están sujetos a los términos de licencia que se encuentran en cada paquete (/usr/share/doc/*nombre-de-paquete*). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor

Azure IoT Edge se basa en un entorno de ejecución de contenedor [compatible con OCI][lnk-oci] (por ejemplo, Docker). Si ya tiene Docker CE/EE instalado en su dispositivo Edge, puede seguir utilizándolo para el desarrollo y pruebas con Azure IoT Edge. 

Para escenarios de producción, se recomienda encarecidamente utilizar el motor [basado en Moby][lnk-moby] que se proporciona a continuación. Es el único motor de contenedor compatible oficialmente con Azure IoT Edge. Las imágenes de los contenedores Docker CE/EE son totalmente compatibles con el entorno de ejecución de Moby.

Los siguientes comandos instalan tanto el motor moby como la interfaz de la línea de comandos (CLI). La CLI es útil para el desarrollo pero opcional para implementaciones de producción.

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

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configuración del demonio de seguridad de IoT Edge

El demonio se puede configurar mediante el archivo de configuración en `/etc/iotedge/config.yaml`. El dispositivo Edge se puede configurar <!--[automatically via Device Provisioning Service][lnk-dps] or--> manualmente mediante una [cadena de conexión de dispositivo][lnk-dcs].

Para realizar la configuración manual, escriba la cadena de conexión del dispositivo en la sección **provisioning** de **config.yaml**

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

*De forma predeterminada el archivo está protegido contra escritura, puede que necesite usar `sudo` para editarlo. Por ejemplo: `sudo nano /etc/iotedge/config.yaml`*

Después de introducir la información de aprovisionamiento en la configuración, reinicie el demonio:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

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
iotedge list
```

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas con la instalación correcta del entorno de ejecución de IoT Edge, consulte la página de [solución de problemas][lnk-trouble].

<!-- Links -->
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md