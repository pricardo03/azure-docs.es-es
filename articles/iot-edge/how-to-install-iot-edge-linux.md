---
title: Instalación de Azure IoT Edge en Linux | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en dispositivos AMD64 con Ubuntu Linux
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: bbab0d8d0947c18cf8e6c178d12fdbd7b335d2b6
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485895"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Instalar el entorno de ejecución de Azure IoT Edge en Linux (x64)

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube.

Para obtener más información, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge en el dispositivo IoT Edge de Ubuntu Linux x64 (Intel o AMD). Para obtener una lista de los sistemas operativos AMD64 compatibles, consulte [Sistemas compatibles con Azure IoT Edge](support.md#operating-systems).

> [!NOTE]
> Los paquetes en los repositorios de software de Linux están sujetos a los términos de licencia que se encuentran en cada paquete (/usr/share/doc/*nombre-de-paquete*). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

## <a name="install-the-latest-version"></a>Instalación de la versión más reciente

Utilice las secciones siguientes para instalar la versión más reciente del servicio Azure IoT Edge en sus dispositivos. 

### <a name="register-microsoft-key-and-software-repository-feed"></a>Registro de la clave y la fuente del repositorio de software de Microsoft

Prepare el dispositivo para la instalación del entorno de ejecución de Azure IoT Edge.


Instale la configuración del repositorio. Elija el fragmento de código **16.04** o **18.04** según corresponda para la versión de Ubuntu:

> [!NOTE]
> Asegúrese de elegir el fragmento de código en el cuadro de código correcto para la versión de Ubuntu.

* Para **Ubuntu 16.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   ```

* Para **Ubuntu 18.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
   ```
   
Copie la lista generada.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Instalación de la clave pública de GPG de Microsoft

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor

Azure IoT Edge utiliza un runtime de contenedor [compatible con OCI](https://www.opencontainers.org/). En los escenarios de producción, se recomienda utilizar el motor [basado en Moby](https://mobyproject.org/) que se proporciona a continuación. Es el único motor de contenedor compatible oficialmente con Azure IoT Edge. Las imágenes de contenedor de Docker CE/EE son totalmente compatibles con el entorno de ejecución de Moby.

Realice una actualización adecuada.

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

#### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>Verificar si el kernel de Linux es compatible con Moby

Muchos fabricantes de dispositivos incrustados distribuyen imágenes de dispositivo que contienen kernels de Linux personalizados a los que tal vez faltan características necesarias para la compatibilidad del runtime del contenedor. Si tiene problemas al instalar el runtime de contenedor recomendado de [Moby](https://github.com/moby/moby), quizá pueda solucionar los problemas de configuración del kernel de Linux mediante el script [check-config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh) proporcionado en el [repositorio Moby de GitHub](https://github.com/moby/moby) oficial; para ello, deberá ejecutar los comandos siguientes en el dispositivo.

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Esto proporcionará un resultado detallado que contiene el estado de las características de kernel que utiliza el runtime de Moby. Debe asegurarse de que todos los elementos de `Generally Necessary` y `Network Drivers` están habilitados para garantizar que el kernel es totalmente compatible con el runtime de Moby.  Si ha identificado alguna característica que falta y quiere habilitarla, puede volver a generar el kernel a partir del origen y seleccionar los módulos asociados para incluirlos en el archivo .config de kernel adecuado.  Igualmente, si utiliza un generador de configuración de kernel como defconfig o menuconfig, deberá buscar y habilitar las características correspondientes y volver a generar el kernel como corresponda.  Una vez que haya implementado el kernel recién modificado, vuelva a ejecutar el script check-config para comprobar que se han habilitado correctamente las características identificadas.

### <a name="install-the-azure-iot-edge-security-daemon"></a>Instalación del demonio de seguridad de Azure IoT Edge

El **demonio de seguridad de IoT Edge** proporciona y mantiene los estándares de seguridad en el dispositivo IoT Edge. El demonio se inicia en cada arranque e inicia el resto del entorno de ejecución de IoT Edge para arrancar el dispositivo.

El comando de instalación también instala la versión estándar de **libiothsm** si no existe.

Realice una actualización adecuada.

   ```bash
   sudo apt-get update
   ```

Instale el demonio de seguridad. El paquete se instala en `/etc/iotedge/`.

   ```bash
   sudo apt-get install iotedge
   ```

Una vez que IoT Edge se ha instalado correctamente, se le solicitará que actualice el archivo de configuración. Siga los pasos descritos en la sección [Configuración del demonio de seguridad de Azure IoT Edge](#configure-the-azure-iot-edge-security-daemon) para terminar de aprovisionar el dispositivo. 

## <a name="install-a-specific-version"></a>Instalación de una versión específica

Si desea instalar una versión específica de Azure IoT Edge, puede dirigirse directamente a los archivos de componentes desde el repositorio de GitHub de IoT Edge. Utilice los pasos siguientes para obtener todos los componentes de IoT Edge en el dispositivo: el motor de Moby y CLI, libiothsm y, por último, el demonio de seguridad de IoT Edge.

1. Vaya a [Versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases) y busque la versión de lanzamiento que desea tener como destino. 

2. Expanda la sección **Recursos** para esa versión.

3. Puede que haya o no actualizaciones del motor de Moby en cualquier versión determinada. Si ve archivos que empiezan por **moby-engine** y **moby-cli**, use los comandos siguientes para actualizar esos componentes. Si no ve los archivos de Moby y aún no tiene Moby instalado en el dispositivo, vuelva a revisar los recursos de la versión anterior hasta que los encuentre. 

   1. Busque el archivo **moby-engine** que coincida con la arquitectura del dispositivo IoT Edge. Haga clic con el botón derecho en el vínculo del archivo y copie la dirección del vínculo.

   2. Use el vínculo copiado en el comando siguiente para instalar esa versión del motor de Moby: 

      ```bash
      curl -L <moby-engine link> -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb
      ```

   3. Busque el archivo **moby-cli** que coincida con la arquitectura del dispositivo IoT Edge. Moby CLI es un componente opcional, pero puede resultar útil durante el desarrollo. Haga clic con el botón derecho en el vínculo del archivo y copie la dirección del vínculo. 

   4. Use el vínculo copiado en el comando siguiente para instalar esa versión de la CLI de Moby: 

      ```bash
      curl -L <moby-cli link> -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb
      ```

4. Cada versión debe tener archivos nuevos para el demonio de seguridad de IoT Edge y hsmlib. Use los comandos siguientes para actualizar esos componentes. 

   1. Busque el archivo **libiothsm-std** que coincida con la arquitectura del dispositivo IoT Edge. Haga clic con el botón derecho en el vínculo del archivo y copie la dirección del vínculo. 

   2. Use el vínculo copiado en el comando siguiente para instalar esa versión de hsmlib:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```
   
   3. Busque el archivo **iotedge** que coincida con la arquitectura del dispositivo IoT Edge. Haga clic con el botón derecho en el vínculo del archivo y copie la dirección del vínculo. 

   4. Use el vínculo copiado en el comando siguiente para instalar esa versión del demonio de seguridad de IoT Edge. 

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

Una vez que IoT Edge se ha instalado correctamente, se le solicitará que actualice el archivo de configuración. Siga los pasos descritos en la sección siguiente para terminar de aprovisionar el dispositivo. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Configuración del demonio de seguridad de Azure IoT Edge

Configure el entorno de ejecución de Azure IoT Edge para vincular el dispositivo físico con una identidad de dispositivo que exista en un centro de Azure IoT.

El demonio se puede configurar mediante el archivo de configuración en `/etc/iotedge/config.yaml`. De forma predeterminada el archivo está protegido contra escritura, puede que necesite permisos elevados para editarlo.

Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexiones de dispositivo proporcionada por IoT Hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para aprovisionar. Según la elección de aprovisionamiento, elija el script de instalación adecuado.

### <a name="option-1-manual-provisioning"></a>Opción 1: Aprovisionamiento manual

Para aprovisionar manualmente un dispositivo, debe proporcionarle una [cadena de conexión](how-to-register-device-portal.md), que puede crear mediante el registro de un dispositivo nuevo en su instancia de IoT Hub.

Abra el archivo de configuración.

```bash
sudo nano /etc/iotedge/config.yaml
```

Busque la sección de aprovisionamiento del archivo. Quite la marca de comentario del modo de aprovisionamiento **manual** y asegúrese de que el modo de aprovisionamiento de dps está marcado como comentario. Actualice el valor de **device_connection_string** con la cadena de conexión desde un dispositivo IoT Edge.

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

Busque la sección de aprovisionamiento del archivo. Quite la marca de comentario del modo de aprovisionamiento **dps** y marque como comentario la sección manual. Actualice los valores de **scope_id** y **registration_id** con los valores de IoT Hub Device Provisioning Service y el dispositivo IoT Edge con TPM.

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

En dispositivos con recursos limitados, se recomienda encarecidamente establecer la variable de entorno *OptimizeForPerformance* en *false*, tal como se indica en las instrucciones de la [guía para la solución de problemas](troubleshoot.md).

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

Si tiene problemas con la instalación correcta del entorno de ejecución de Azure IoT Edge, consulte la página de [solución de problemas](troubleshoot.md).

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).
