---
title: Instalación de Azure IoT Edge en ARM32 en Linux | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Linux en dispositivos ARM32, como Raspberry PI
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: kgremban
ms.openlocfilehash: 874e72a0c76a5935f4a3451ba5ffbd6e9e3eecd6
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393854"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. 

Para más información acerca de cómo funciona el entorno de ejecución de Azure IoT Edge y los componentes que se incluyen, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge en un dispositivo perimetral de Linux ARM32v7/armhf. Por ejemplo, estos pasos podrían funcionar con dispositivos Raspberry Pi. Consulte [Compatibilidad de Azure IoT Edge](support.md#operating-systems) para obtener una lista de sistemas operativos ARM32 que se admiten actualmente. 

>[!NOTE]
>Los paquetes en los repositorios de software de Linux están sujetos a los términos de licencia que se encuentran en cada paquete (/usr/share/doc/*nombre-de-paquete*). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor

Azure IoT Edge utiliza un runtime de contenedor [compatible con OCI](https://www.opencontainers.org/). En los escenarios de producción, se recomienda encarecidamente utilizar el motor [basado en Moby](https://mobyproject.org/) que se proporciona a continuación. Es el único motor de contenedor compatible oficialmente con Azure IoT Edge. Las imágenes de los contenedores Docker CE/EE son compatibles con el entorno de ejecución basado en Moby.

Los siguientes comandos instalan tanto el motor basado en Moby como la interfaz de la línea de comandos (CLI). La CLI es útil para el desarrollo pero opcional para implementaciones de producción.

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

El **demonio de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo Edge. El demonio se inicia en cada arranque e inicia el resto del entorno de ejecución de IoT Edge para arrancar el dispositivo. 


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

El demonio se puede configurar mediante el archivo de configuración en `/etc/iotedge/config.yaml`. De forma predeterminada el archivo está protegido contra escritura, puede que necesite permisos elevados para editarlo.

Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexiones de dispositivo proporcionada por IoT Hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para aprovisionar. Según la elección de aprovisionamiento, elija el script de instalación adecuado. 

### <a name="option-1-manual-provisioning"></a>Opción 1: Aprovisionamiento manual

Para aprovisionar manualmente un dispositivo, debe proporcionarle una [cadena de conexión](how-to-register-device-portal.md), que puede crear mediante el registro de un dispositivo nuevo en su instancia de IoT Hub.


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

Si uso los pasos de **configuración manual** de la sección anterior, el entorno de ejecución de IoT Edge debe haberse aprovisionado correctamente y estar en ejecución en el dispositivo. Si ha usado los pasos de **configuración automática**, deberá completar algunos pasos adicionales para que el entorno de ejecución pueda registrar el dispositivo con IoT Hub en su nombre. Para los pasos siguientes, consulte la sección de [Create and provision a simulated TPM Edge device on a Linux virtual machine](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm) (Creación y aprovisionamiento de un dispositivo TPM Edge en una máquina virtual Linux).

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

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas con la instalación correcta del runtime de IoT Edge, consulte la página de [solución de problemas](troubleshoot.md#stability-issues-on-resource-constrained-devices).
