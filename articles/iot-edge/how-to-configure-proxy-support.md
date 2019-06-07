---
title: 'Configuración de dispositivos para servidores proxy de red: Azure IoT Edge | Microsoft Docs'
description: Aprenda a configurar el entorno de ejecución de Azure IoT Edge y todos los módulos de IoT Edge a los que se pueda acceder desde Internet para que se comuniquen a través de un servidor proxy.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 1c0da1a768b894f543b9089643622c31d6a8758d
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730144"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy

Los dispositivos de IoT Edge envían solicitudes HTTPS para comunicarse con IoT Hub. Si el dispositivo está conectado a una red que usa un servidor proxy, deberá configurar el entorno de ejecución de IoT Edge para que se comunique a través del servidor. Los servidores proxy también pueden afectar a módulos de IoT Edge individuales si estos realizan solicitudes HTTP o HTTPS que no se enrutan a través del centro de IoT Edge. 

En este artículo le guía a través de los cuatro pasos siguientes para configurar y administrar un dispositivo de IoT Edge detrás de un servidor proxy: 

1. **Instalar el runtime de IoT Edge en el dispositivo.**

   Los scripts de instalación de IoT Edge extraen paquetes y archivos de internet, por lo que el dispositivo necesita comunicarse a través del servidor proxy para realizar esas solicitudes. Para obtener instrucciones detalladas, consulte el [instalar el runtime a través de un proxy](#install-the-runtime-through-a-proxy) sección de este artículo. Para los dispositivos de Windows, el script de instalación también proporciona un [instalación sin conexión](how-to-install-iot-edge-windows.md#offline-installation) opción. 

   Este paso es un proceso único que se realizan en el dispositivo de IoT Edge cuando se establece. También se requieren las mismas conexiones cuando se actualiza el tiempo de ejecución de IoT Edge. 

2. **Configurar el demonio de Docker y el demonio de IoT Edge en el dispositivo.**

   IoT Edge utiliza dos daemons en el dispositivo, que necesitan para realizar solicitudes web a través del servidor proxy. El demonio de IoT Edge es responsable de las comunicaciones con IoT Hub. El demonio Moby es responsable de la administración de contenedores, por lo que se comunica con los registros de contenedor. Para obtener instrucciones detalladas, consulte el [configurar los demonios](#configure-the-daemons) sección de este artículo. 

   Este paso es un proceso único que se realizan en el dispositivo de IoT Edge cuando se establece.

3. **Configure las propiedades de agente de IoT Edge en el archivo config.yaml en el dispositivo.**

   El demonio de IoT Edge inicia el módulo edgeAgent inicialmente, pero, a continuación, el módulo de edgeAgent es responsable de recuperar el manifiesto de implementación de IoT Hub e iniciar todos los módulos. Para que el agente de IoT Edge realizar la conexión inicial a IoT Hub, configure las variables de entorno del módulo de edgeAgent manualmente en el propio dispositivo. Después de la conexión inicial, puede configurar el módulo de edgeAgent de forma remota. Para obtener instrucciones detalladas, consulte el [configurar el agente de IoT Edge](#configure-the-iot-edge-agent) sección de este artículo.

   Este paso es un proceso único que se realizan en el dispositivo de IoT Edge cuando se establece.

4. **Para todas las implementaciones futuras del módulo, establecer variables de entorno de cualquier módulo que se comunican a través del proxy.**

   Una vez que el dispositivo IoT Edge está configurado y conectado a IoT Hub a través del servidor proxy, debe mantener la conexión de todas las implementaciones de módulo futuras. Para obtener instrucciones detalladas, consulte el [configurar manifiestos de implementación](#configure-deployment-manifests) sección de este artículo. 

   Este paso es un proceso continuo que se realizan de forma remota para que cada nueva actualización de módulo o implementación mantiene la capacidad del dispositivo para comunicarse a través del servidor proxy. 

## <a name="know-your-proxy-url"></a>Dirección URL de proxy

Antes de comenzar cualquiera de los pasos descritos en este artículo, necesitará saber la dirección URL del proxy.

Las direcciones URL de proxy tienen el formato siguiente: **protocolo**://**host_proxy**:**puerto_proxy**.

* El **protocolo** es HTTP o HTTPS. El demonio de Docker puede usar cualquier protocolo según la configuración del registro de contenedor, pero los contenedores en tiempo de ejecución y demonio de IoT Edge deben utilizar siempre HTTPS.

* El **host_proxy** es una dirección para el servidor proxy. Si el servidor proxy requiere autenticación, puede proporcionar sus credenciales como parte de host del proxy con el siguiente formato: **usuario**:**contraseña**\@**host_proxy** .

* El **puerto_proxy** es el puerto de red en el que el proxy responde al tráfico de red.

## <a name="install-the-runtime-through-a-proxy"></a>Instalar el runtime a través de un servidor proxy

Si el dispositivo IoT Edge se ejecuta en Windows o Linux, necesite obtener acceso a los paquetes de instalación a través del servidor proxy. Dependiendo del sistema operativo, siga los pasos para instalar el runtime de IoT Edge a través de un servidor proxy. 

### <a name="linux"></a>Linux

Si va a instalar el entorno de ejecución de Azure IoT Edge en un dispositivo Linux, configure el administrador de paquetes para que pase por el servidor proxy para acceder al paquete de instalación. Por ejemplo, [configure apt-get para usar un http-proxy](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy). Una vez que el administrador de paquetes esté configurado, siga las instrucciones de [Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) o [Instalar el entorno de ejecución de Azure IoT Edge en Linux (x64)](how-to-install-iot-edge-linux.md) como de costumbre.

### <a name="windows"></a>Windows

Si va a instalar el runtime de IoT Edge en un dispositivo de Windows, deberá ir dos veces a través del servidor proxy. La primera conexión descarga el archivo de script de instalador y la segunda conexión es durante la instalación para descargar los componentes necesarios. Puede configurar la información de proxy en la configuración de Windows, o incluir la información de proxy directamente en los comandos de PowerShell. 

Los pasos siguientes muestran un ejemplo de una instalación de windows con el `-proxy` argumento:

1. El comando Invoke-WebRequest necesita información de proxy para tener acceso a la secuencia de comandos del instalador. A continuación, el comando implementar IoTEdge necesita la información de proxy para descargar los archivos de instalación. 

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Deploy-IoTEdge -proxy <proxy URL>
   ```

2. El comando Initialize IoTEdge no tiene que pasar por el servidor proxy, por lo que el segundo paso solo requiere información de proxy de Invoke-WebRequest.

   ```powershell
   . {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; Initialize-IoTEdge

If you have complicated credentials for the proxy server that can't be included in the URL, use the `-ProxyCredential` parameter within `-InvokeWebRequestParameters`. For example,

```powershell
$proxyCredential = (Get-Credential).GetNetworkCredential()
. {Invoke-WebRequest -proxy <proxy URL> -ProxyCredential $proxyCredential -useb aka.ms/iotedge-win} | Invoke-Expression; `
Deploy-IoTEdge -InvokeWebRequestParameters @{ '-Proxy' = '<proxy URL>'; '-ProxyCredential' = $proxyCredential }
```

Para obtener más información acerca de los parámetros de proxy, consulte [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest). Para obtener más información acerca de las opciones de instalación de Windows, incluida la instalación sin conexión, consulte [en tiempo de ejecución de instalación de Azure IoT Edge en Windows](how-to-install-iot-edge-windows.md).

## <a name="configure-the-daemons"></a>Configuración de los demonios

IoT Edge se basa en dos demonios que se ejecutan en el dispositivo de IoT Edge. El demonio Moby realiza las solicitudes web en extraer imágenes de contenedor desde registros de contenedor. El demonio de IoT Edge realiza solicitudes web para comunicarse con IoT Hub.

El Moby y los demonios de IoT Edge deben configurarse para usar el servidor proxy para la funcionalidad del dispositivo en curso. Este paso realiza en el dispositivo de IoT Edge durante la instalación inicial del dispositivo. 

### <a name="moby-daemon"></a>Demonio MOBY

Puesto que Moby se basa en Docker, consulte la documentación de Docker para configurar el demonio Moby con variables de entorno. La mayoría de los registros de contenedor (incluidos los de DockerHub y Azure Container) admiten solicitudes HTTPS, por lo que el parámetro que se debe establecer es **HTTPS_PROXY**. Si va a extraer imágenes de un registro que no admite la seguridad de la capa de transporte (TLS), debe establecer el parámetro **HTTP_PROXY**. 

Elija el artículo que se aplica a su sistema operativo del dispositivo IoT Edge: 

* [Configurar el demonio de Docker en Linux](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
    * El demonio Moby en dispositivos Linux mantiene el nombre de Docker.
* [Configurar el demonio de Docker en Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon#proxy-configuration)
    * El demonio Moby en dispositivos de Windows se denomina iotedge moby. Los nombres son diferentes porque es posible ejecutar Docker Desktop y Moby en paralelo en un dispositivo de Windows. 

### <a name="iot-edge-daemon"></a>Demonio de IoT Edge

El demonio de IoT Edge está configurado de forma similar al demonio Moby. Utilice los pasos siguientes para establecer una variable de entorno para el servicio según su sistema operativo. 

El demonio de IoT Edge siempre utiliza HTTPS para enviar solicitudes a IoT Hub.

#### <a name="linux"></a>Linux

Abra un editor en el terminal para configurar el demonio de IoT Edge. 

```bash
sudo systemctl edit iotedge
```

Escriba el texto siguiente, reemplazando **\<dirección URL del proxy >** por la dirección del servidor proxy y el puerto. A continuación, guarde y salga. 

```ini
[Service]
Environment="https_proxy=<proxy URL>"
```

Actualice el administrador de servicios para que incluya la nueva configuración de IoT Edge.

```bash
sudo systemctl daemon-reload
```

Reinicie IoT Edge para que los cambios surtan efecto.

```bash
sudo systemctl restart iotedge
```

Compruebe que se creó la variable de entorno y que se cargó la nueva configuración. 

```bash
systemctl show --property=Environment iotedge
```

#### <a name="windows"></a>Windows

Abra una ventana de PowerShell como administrador y ejecute el comando siguiente para editar el registro con la nueva variable de entorno. Reemplace **\<dirección URL del proxy >** por la dirección del servidor proxy y el puerto. 

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Services\iotedge /v Environment /t REG_MULTI_SZ /d https_proxy=<proxy URL>
```

Reinicie IoT Edge para que los cambios surtan efecto.

```powershell
Restart-Service iotedge
```

## <a name="configure-the-iot-edge-agent"></a>Configuración del agente de IoT Edge

El agente de IoT Edge es el primer módulo que se inicia en cualquier dispositivo de IoT Edge. Se inicia por primera vez en función de la información del archivo config.yaml de IoT Edge. El agente de IoT Edge se conecta posteriormente a IoT Hub para recuperar los manifiestos de implementación, los cuales declaran qué otros módulos deberían estar implementados en el dispositivo.

Este paso realiza una vez en el dispositivo de IoT Edge durante la instalación inicial del dispositivo. 

1. Abra el archivo config.yaml en el dispositivo de IoT Edge. En los sistemas Linux, este archivo se encuentra en **/etc/iotedge/config.yaml**. En los sistemas Windows, este archivo se encuentra en **C:\ProgramData\iotedge\config.yaml**. El archivo de configuración está protegido, por lo que necesita privilegios de administrador para acceder a él. En los sistemas Linux, use el `sudo` comando antes de abrir el archivo en el editor de texto que prefiera. En Windows, abra un editor de texto como el Bloc de notas como administrador y, a continuación, abra el archivo. 

2. En el archivo config.yaml, busque la sección **especificación del módulo de agente de Edge**. La definición del agente de IoT Edge incluye un parámetro **env** en el que puede agregar variables de entorno. 

3. Elimine las llaves que son marcadores de posición del parámetro env y agregue la nueva variable en una nueva línea. Recuerde que las sangrías de YAML son dos espacios. 

   ```yaml
   https_proxy: "<proxy URL>"
   ```

4. El runtime de IoT Edge usa AMQP de forma predeterminada para comunicarse con IoT Hub. Algunos servidores proxy bloquean los puertos AMQP. Si es así, también debe configurar edgeAgent para que use AMQP sobre WebSocket. Agregue una segunda variable de entorno.

   ```yaml
   UpstreamProtocol: "AmqpWs"
   ```

   ![definición de edgeAgent con variables de entorno](./media/how-to-configure-proxy-support/edgeagent-edited.png)

5. Guarde los cambios en config.yaml y cierre el editor. Reinicie IoT Edge para que los cambios surtan efecto. 

   * Linux: 

      ```bash
      sudo systemctl restart iotedge
      ```

   * Windows:

      ```powershell
      Restart-Service iotedge
      ```

## <a name="configure-deployment-manifests"></a>Configuración de manifiestos de implementación  

Una vez que el dispositivo de IoT Edge esté configurado para funcionar con el servidor proxy, deberá declarar las variables de entorno en los manifiestos de implementación futuros. Puede editar los manifiestos de implementación ya sea mediante el Asistente del portal de Azure o mediante la edición de una implementación de archivo JSON de manifiesto. 

Configure siempre los dos módulos en tiempo de ejecución (edgeAgent y edgeHub) para que se comuniquen a través del servidor proxy con el fin de que puedan mantener una conexión con IoT Hub. Si quita la información del proxy desde el módulo de edgeAgent, es la única manera de restablecer la conexión, edite el archivo config.yaml en el dispositivo, como se describe en la sección anterior. 

Otros módulos de IoT Edge que se conectan a internet deben configurarse para comunicarse a través del servidor proxy, demasiado. Sin embargo, los módulos que enrutar los mensajes a través del centro de Edge o que solo se comunican con otros módulos en el dispositivo no necesitan los detalles del servidor proxy. 

Este paso está en curso durante la vida del dispositivo IoT Edge. 

### <a name="azure-portal"></a>Azure Portal

Cuando usa el asistente de **Establecimiento de módulos** para crear implementaciones para dispositivos de IoT Edge, todos los módulos tienen una sección **Variables de entorno** que puede usar para configurar las conexiones con el servidor proxy. 

Para configurar el agente de IoT Edge y los módulos del centro de IoT Edge, seleccione **Configurar las opciones avanzadas del entorno en tiempo de ejecución de Edge** en el primer paso del asistente. 

![Configurar las opciones avanzadas del entorno en tiempo de ejecución de Edge](./media/how-to-configure-proxy-support/configure-runtime.png)

Agregue la variable de entorno **https_proxy** al agente de IoT Edge y a las definiciones de módulo del centro de IoT Edge. Si incluyó la variable de entorno **UpstreamProtocol** en el archivo config.yaml del dispositivo de IoT Edge, agréguela también a la definición del módulo del agente de IoT Edge. 

![Establecimiento de la variable de entorno https_proxy](./media/how-to-configure-proxy-support/edgehub-environmentvar.png)

Todos los demás módulos que agregue a un manifiesto de implementación seguirán el mismo patrón. En la página en la que estableció el nombre y la imagen del módulo hay una sección de variables de entorno.

### <a name="json-deployment-manifest-files"></a>Archivos JSON del manifiesto de implementación

Si va a crear implementaciones para dispositivos IoT Edge mediante las plantillas de Visual Studio Code o mediante la creación manual de archivos JSON, puede agregar las variables de entorno directamente a cada definición del módulo. 

Use el siguiente formato JSON: 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    }
}
```

Con las variables de entorno incluidas, la definición del módulo debería ser similar al siguiente ejemplo de edgeHub:

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": ""
    },
    "env": {
        "https_proxy": {
            "value": "https://proxy.example.com:3128"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Si incluyó la variable de entorno **UpstreamProtocol** en el archivo confige.yaml del dispositivo de IoT Edge, agréguela también a la definición del módulo del agente de IoT Edge. 

```json
"env": {
    "https_proxy": {
        "value": "<proxy URL>"
    },
    "UpstreamProtocol": {
        "value": "AmqpWs"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los roles del [entorno de ejecución de Azure IoT Edge](iot-edge-runtime.md).

Solución de errores de instalación y configuración con [Problemas habituales y soluciones para Azure IoT Edge](troubleshoot.md)

