---
title: Instalación de Azure IoT Edge en Linux | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Linux
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: kgremban
ms.openlocfilehash: 56223b2ed8e9d9b1a08f5313940920113a650bfe
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128339"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Instalar el entorno de ejecución de Azure IoT Edge en Linux (x64)

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeñas como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. 

Para más información acerca de cómo funciona el entorno de ejecución de IoT Edge y los componentes que se incluyen, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge en el dispositivo Edge de Linux x64 (Intel o AMD). Consulte [Compatibilidad de Azure IoT Edge](support.md#operating-systems) para obtener una lista de sistemas operativos AMD64 que se admiten actualmente. 

>[!NOTE]
>Los paquetes en los repositorios de software de Linux están sujetos a los términos de licencia que se encuentran en cada paquete (/usr/share/doc/*nombre-de-paquete*). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Registro de la clave y la fuente del repositorio de software de Microsoft

Según el sistema operativo, elija los scripts adecuados para preparar el dispositivo para la instalación del entorno de ejecución de IoT Edge. 

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/

# Perform apt upgrade
sudo apt-get upgrade
```

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor 

Azure IoT Edge se basa en un entorno de ejecución de contenedor [compatible con OCI][lnk-oci]. Para escenarios de producción, se recomienda encarecidamente utilizar el motor [basado en Moby][lnk-moby] que se proporciona a continuación. Es el único motor de contenedor compatible oficialmente con Azure IoT Edge. Las imágenes de contenedor de Docker CE/EE son totalmente compatibles con el entorno de ejecución de Moby.

Actualice apt-get.

```bash
sudo apt-get update
```

Instale el motor de Moby. 

```bash
sudo apt-get install moby-engine
```

Instale la interfaz de la línea de comandos (CLI) de Moby. La CLI es útil para el desarrollo pero opcional para implementaciones de producción.

```bash
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>Instalación del demonio de seguridad de Azure IoT Edge

El **demonio de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo Edge. El demonio se inicia en cada arranque e inicia el resto del entorno de ejecución de IoT Edge para arrancar el dispositivo. 

El comando de instalación también instala la versión estándar de **iothsmlib** si no existe.

Actualice apt-get.

```bash
sudo apt-get update
```

Instale el demonio de seguridad. El paquete se instala en `/etc/iotedge/`.

```bash
sudo apt-get install iotedge
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configuración del demonio de seguridad de Azure IoT Edge

Configure el entorno de ejecución de Azure IoT Edge para vincular el dispositivo físico con una identidad de dispositivo que exista en un centro de Azure IoT. 

El demonio se puede configurar mediante el archivo de configuración en `/etc/iotedge/config.yaml`. De forma predeterminada el archivo está protegido contra escritura, puede que necesite permisos elevados para editarlo.

Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexiones de dispositivo proporcionada por IoT Hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para aprovisionar. Según la elección de aprovisionamiento, elija el script de instalación adecuado. 

### <a name="option-1-manual-provisioning"></a>Opción 1: Aprovisionamiento manual

Para aprovisionar manualmente un dispositivo, debe proporcionarle una [cadena de conexión de dispositivo][lnk-dcs] que puede crear mediante el registro de un nuevo dispositivo en su instancia de IoT Hub.


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

Para aprovisionar automáticamente un dispositivo, [configure el servicio Device Provisioning y recupere el identificador de registro del dispositivo][lnk-dps] (DPS). El aprovisionamiento automático solo funciona con dispositivos que tienen un chip de Módulo de plataforma segura (TPM). Por ejemplo, los dispositivos Raspberry Pi no incluyen TPM de forma predeterminada. 

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

En dispositivos con recursos limitados, se recomienda encarecidamente que establezca la variable de entorno *OptimizeForPerformance* en *false* según las instrucciones de la [guía de resolución de problemas][lnk-trouble].

## <a name="next-steps"></a>Pasos siguientes

Si tiene problemas con la instalación correcta del entorno de ejecución de Edge, consulte la página de [solución de problemas][lnk-trouble].

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
