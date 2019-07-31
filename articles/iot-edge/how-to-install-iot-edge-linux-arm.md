---
title: Instalación de Azure IoT Edge en ARM32 en Linux | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Linux en dispositivos ARM32, como Raspberry PI
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: kgremban
ms.openlocfilehash: 72349597a2831a64cb11ff66b30e6948f34cc921
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68224709"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. 

Para más información acerca de cómo funciona el entorno de ejecución de IoT Edge y los componentes que se incluyen, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge en un dispositivo perimetral de IoT Edge de Linux ARM32v7/armhf. Por ejemplo, estos pasos podrían funcionar con dispositivos Raspberry Pi. Para obtener una lista de los sistemas operativos ARM32 compatibles, consulte [Sistemas compatibles con Azure IoT Edge](support.md#operating-systems). 

>[!NOTE]
>Los paquetes en los repositorios de software de Linux están sujetos a los términos de licencia que se encuentran en cada paquete (/usr/share/doc/*nombre-de-paquete*). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="install-the-latest-version"></a>Instalación de la versión más reciente

Utilice las secciones siguientes para instalar la versión más reciente del servicio Azure IoT Edge en sus dispositivos ARM de Linux. 

### <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor

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

### <a name="install-the-iot-edge-security-daemon"></a>Instalación del demonio de seguridad de IoT Edge

El **demonio de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo IoT Edge. El demonio se inicia en cada arranque e inicia el resto del entorno de ejecución de IoT Edge para arrancar el dispositivo. 


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

Una vez que IoT Edge se ha instalado correctamente, se le solicitará que actualice el archivo de configuración. Siga los pasos descritos en la sección [Configuración del demonio de seguridad de Azure IoT Edge](#configure-the-azure-iot-edge-security-daemon) para terminar de aprovisionar el dispositivo. 

## <a name="install-a-specific-version"></a>Instalación de una versión específica

Si desea instalar una versión específica de Azure IoT Edge, puede dirigirse directamente a los archivos de componentes desde el repositorio de GitHub de IoT Edge. Use los mismos comandos `curl` de las secciones anteriores para obtener todos los componentes de IoT Edge en el dispositivo: el motor de Moby y CLI, libiothsm y, por último, el demonio de seguridad de IoT Edge. La única diferencia es que se reemplazan las direcciones URL **aka.ms** por vínculos que apuntan directamente a la versión de cada componente que quiere usar.

Vaya a [Versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases) y busque la versión de lanzamiento que desea tener como destino. Expanda la sección **Recursos** correspondiente a la versión y elija los archivos que coincidan con la arquitectura del dispositivo IoT Edge. Cada versión de IoT Edge contiene los archivos **iotedge** y **libiothsm**. No todas las versiones incluyen **moby-engine** o **moby-cli**. Si todavía no tiene instalado el motor de contenedor de Moby, busque en las versiones anteriores hasta encontrar alguna que incluya los componentes de Moby. 

Una vez que IoT Edge se ha instalado correctamente, se le solicitará que actualice el archivo de configuración. Siga los pasos descritos en la sección siguiente para terminar de aprovisionar el dispositivo. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configuración del demonio de seguridad de Azure IoT Edge

Configure el entorno de ejecución de Azure IoT Edge para vincular el dispositivo físico con una identidad de dispositivo que exista en un centro de Azure IoT. 

El demonio se puede configurar mediante el archivo de configuración en `/etc/iotedge/config.yaml`. El archivo está protegido contra escritura de forma predeterminada, por lo que puede que necesite permisos elevados para editarlo.

Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexiones de dispositivo proporcionada por IoT Hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para aprovisionar. Según la elección de aprovisionamiento, elija el script de instalación adecuado. 

### <a name="option-1-manual-provisioning"></a>Opción 1: Aprovisionamiento manual

Para aprovisionar manualmente un dispositivo, debe proporcionarle una [cadena de conexión de dispositivo](how-to-register-device-portal.md), que puede crear mediante el registro de un dispositivo IoT Edge nuevo en su instancia de IoT Hub.

Abra el archivo de configuración. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Busque las configuraciones de aprovisionamiento del archivo y quite la marca de comentario de la sección **Manual provisioning configuration**. Actualice el valor de **device_connection_string** con la cadena de conexión desde un dispositivo IoT Edge. Asegúrese de que las demás secciones de aprovisionamiento estén comentadas.

```yaml
# Manual provisioning configuration
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
# DPS TPM provisioning configuration
# provisioning:
#   source: "dps"
#   global_endpoint: "https://global.azure-devices-provisioning.net"
#   scope_id: "{scope_id}"
#   attestation:
#     method: "tpm"
#     registration_id: "{registration_id}"
```

Guarde y cierre el archivo. 

`CTRL + X`, `Y`, `Enter`

Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Opción 2: Aprovisionamiento automático

Para aprovisionar automáticamente un dispositivo, [configure el servicio Device Provisioning y recupere el identificador de registro del dispositivo](how-to-auto-provision-simulated-device-linux.md). Hay una serie de mecanismos de atestación que IoT Edge admite al usar el aprovisionamiento automático, pero los requisitos de hardware también afectan a las elecciones. Por ejemplo, de forma predeterminada, los dispositivos Raspberry Pi no cuentan con un chip Módulo de plataforma segura (TPM).

Abra el archivo de configuración. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Busque las configuraciones de aprovisionamiento del archivo y quite la marca de comentario de la sección adecuada para el mecanismo de atestación. Cuando utilice atestación TPM, por ejemplo, actualice los valores de **scope_id** y **registration_id** con los valores del servicio IoT Hub Device Provisioning y del dispositivo IoT Edge con TPM, respectivamente.

```yaml
   # Manual provisioning configuration
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # DPS TPM provisioning configuration
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     attestation:
       method: "tpm"
       registration_id: "{registration_id}"
```

Guarde y cierre el archivo. 

`CTRL + X`, `Y`, `Enter`

Después de especificar la información de aprovisionamiento en el archivo de configuración, reinicie el demonio:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Si uso los pasos de **configuración manual** de la sección anterior, el entorno de ejecución de IoT Edge debe haberse aprovisionado correctamente y estar en ejecución en el dispositivo. Si ha usado los pasos de **configuración automática**, deberá completar algunos pasos adicionales para que el entorno de ejecución pueda registrar el dispositivo con IoT Hub en su nombre. Para los pasos siguientes, consulte [Creación y aprovisionamiento de un dispositivo de IoT Edge con un TPM virtual en una máquina virtual Linux](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

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

Si tiene problemas con la instalación correcta del entorno de ejecución de Azure IoT Edge, consulte la página de [solución de problemas](troubleshoot.md#stability-issues-on-resource-constrained-devices).

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).
