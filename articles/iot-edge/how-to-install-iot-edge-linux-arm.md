---
title: Instalación de Azure IoT Edge en ARM32 en Linux | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Linux en dispositivos ARM32, como Raspberry PI
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kgremban
ms.openlocfilehash: ad7e34110b0c6d047eb7454b7fac9f8c10df8be2
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316106"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. 

Para más información acerca de cómo funciona el entorno de ejecución de IoT Edge y los componentes que se incluyen, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumera los pasos para instalar el runtime de Azure IoT Edge en un dispositivo Linux ARM32v7/armhf IoT Edge. Por ejemplo, estos pasos podrían funcionar con dispositivos Raspberry Pi. Para obtener una lista de sistemas operativos de ARM32 compatibles, consulte [soporte técnico de Azure IoT Edge](support.md#operating-systems). 

>[!NOTE]
>Los paquetes en los repositorios de software de Linux están sujetos a los términos de licencia que se encuentran en cada paquete (/usr/share/doc/*nombre-de-paquete*). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor

Azure IoT Edge utiliza un runtime de contenedor [compatible con OCI](https://www.opencontainers.org/). En los escenarios de producción, se recomienda encarecidamente utilizar el motor [basado en Moby](https://mobyproject.org/) que se proporciona a continuación. Es el único motor de contenedor compatible oficialmente con Azure IoT Edge. Las imágenes de los contenedores Docker CE/EE son compatibles con el entorno de ejecución basado en Moby.

El siguiente comando instala el motor de Moby y la interfaz de línea de comandos (CLI). La CLI es útil para el desarrollo pero opcional para implementaciones de producción.

```bash
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

El **daemon de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo de IoT Edge. El demonio se inicia en cada arranque e inicia el resto del entorno de ejecución de IoT Edge para arrancar el dispositivo. 


```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

## <a name="connect-your-device-to-an-iot-hub"></a>Conexión del dispositivo a IoT Hub 

Configure el entorno de ejecución de Azure IoT Edge para vincular el dispositivo físico con una identidad de dispositivo que exista en un centro de Azure IoT. 

El demonio se puede configurar mediante el archivo de configuración en `/etc/iotedge/config.yaml`. El archivo está protegido contra escritura de forma predeterminada, por lo que es posible que necesite permisos elevados para editarlo.

Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexiones de dispositivo proporcionada por IoT Hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para aprovisionar. Según la elección de aprovisionamiento, elija el script de instalación adecuado. 

### <a name="option-1-manual-provisioning"></a>Opción 1: Aprovisionamiento manual

Para aprovisionar manualmente un dispositivo, deberá proporcionar con un [cadena de conexión de dispositivo](how-to-register-device-portal.md) que puede crear mediante el registro de un nuevo dispositivo de IoT Edge en IoT hub.

Abra el archivo de configuración. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Busque la sección del archivo del aprovisionamiento y quite la marca de comentario del modo de aprovisionamiento **manual**. Actualice el valor de **device_connection_string** con la cadena de conexión desde un dispositivo IoT Edge.

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

Guarde y cierre el archivo. 

`CTRL + X`, `Y`, `Enter`

Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Opción 2: Aprovisionamiento automático

Para aprovisionar automáticamente un dispositivo, [configure el servicio Device Provisioning y recupere el identificador de registro del dispositivo](how-to-auto-provision-simulated-device-linux.md). El aprovisionamiento automático solo funciona con dispositivos que tienen un chip de Módulo de plataforma segura (TPM). Por ejemplo, los dispositivos Raspberry Pi no incluyen TPM de forma predeterminada. 

Abra el archivo de configuración. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Busque la sección del archivo del aprovisionamiento y quite la marca de comentario del modo de aprovisionamiento **dps**. Actualice los valores de **scope_id** y **registration_id** con los valores de IoT Hub Device Provisioning Service y el dispositivo IoT Edge con TPM. 

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

Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si uso los pasos de **configuración manual** de la sección anterior, el entorno de ejecución de IoT Edge debe haberse aprovisionado correctamente y estar en ejecución en el dispositivo. O bien, si ha usado el **configuración automática** pasos, deberá completar algunos pasos adicionales para que el tiempo de ejecución puede registrar el dispositivo con IoT hub en su nombre. Para los pasos siguientes, vea [creación y aprovisionamiento de un dispositivo de TPM IoT Edge simulado en una máquina virtual Linux](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Puede comprobar el estado del demonio de IoT Edge mediante lo siguiente:

```bash
systemctl status iotedge
```

Examine los registros del demonio con:

```bash
journalctl -u iotedge --no-pager --no-full
```

Y, enumere los módulos en ejecución con:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>Recomendaciones y sugerencias

Necesita privilegios elevados para ejecutar comandos `iotedge`. Después de instalar el entorno de ejecución, cierre la sesión en la máquina e iníciela de nuevo para actualizar sus permisos automáticamente. Hasta entonces, use **sudo** delante de los comandos `iotedge`.

En dispositivos con recursos limitados, se recomienda encarecidamente establecer la variable de entorno *OptimizeForPerformance* en *false*, tal como se indica en las instrucciones de la [guía para la solución de problemas](troubleshoot.md#stability-issues-on-resource-constrained-devices).

Si la red tiene un servidor proxy, siga los pasos descritos en [Configure your IoT Edge device to communicate through a proxy server](how-to-configure-proxy-support.md) (Configuración del dispositivo IoT Edge para comunicarse mediante un servidor proxy).

## <a name="uninstall-iot-edge"></a>Desinstalación de IoT Edge

Si quiere quitar la instalación de IoT Edge del dispositivo de Linux, use los siguientes comandos de la línea de comandos. 

Quite el entorno de ejecución de Azure IoT Edge. 

```bash
sudo apt-get remove --purge iotedge
```

Cuando se quita el entorno de ejecución de Azure IoT Edge, el contenedor que se ha creado se detiene, pero sigue existiendo en el dispositivo. Observe todos los contenedores para ver cuáles permanecen. 

```bash
sudo docker ps -a
```

Elimine los contenedores del dispositivo, incluidos los dos contenedores del entorno en tiempo de ejecución. 

```bash
sudo docker rm -f <container name>
```

Por último, quite el entorno en tiempo de ejecución del contenedor del dispositivo. 

```bash 
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge aprovisionado con el entorno de ejecución instalado, puede [implementar módulos de IoT Edge](how-to-deploy-modules-portal.md).

Si tiene problemas con el tiempo de ejecución de IoT Edge que se instale correctamente, consulte el [solución de problemas](troubleshoot.md#stability-issues-on-resource-constrained-devices) página.

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).
