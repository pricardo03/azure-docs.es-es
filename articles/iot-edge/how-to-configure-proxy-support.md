---
title: 'Configuración de dispositivos para servidores proxy de red: Azure IoT Edge | Microsoft Docs'
description: Aprenda a configurar el entorno de ejecución de Azure IoT Edge y todos los módulos de IoT Edge a los que se pueda acceder desde Internet para que se comuniquen a través de un servidor proxy.
author: kgremban
manager: ''
ms.author: kgremban
ms.date: 03/20/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 4fa5402b87eea969a5a4093000dda06d3cb5675d
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58312995"
---
# <a name="configure-an-iot-edge-device-to-communicate-through-a-proxy-server"></a>Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy

Los dispositivos de IoT Edge envían solicitudes HTTPS para comunicarse con IoT Hub. Si el dispositivo está conectado a una red que usa un servidor proxy, deberá configurar el entorno de ejecución de IoT Edge para que se comunique a través del servidor. Los servidores proxy también pueden afectar a módulos de IoT Edge individuales si estos realizan solicitudes HTTP o HTTPS que no se enrutan a través del centro de IoT Edge. 

Para configurar un dispositivo de IoT Edge para que funcione con un servidor proxy siga estos pasos básicos: 

1. Instale el entorno de ejecución de Azure IoT Edge en el dispositivo. 
2. Configure los demonios de Docker y de IoT Edge en el dispositivo para que usen el servidor proxy.
3. Configure las propiedades de edgeAgent en el archivo config.yaml del dispositivo.
4. Establezca las variables de entorno del entorno de ejecución de Azure IoT Edge y de otros módulos de IoT Edge en el manifiesto de implementación.

## <a name="know-your-proxy-url"></a>Dirección URL de proxy

Para configurar tanto el demonio Docker como IoT Edge en el dispositivo, deberá saber la dirección URL del proxy.

Las direcciones URL de proxy tienen el formato siguiente: **protocolo**://**host_proxy**:**puerto_proxy**.

* El **protocolo** es HTTP o HTTPS. El demonio de Docker puede usar cualquier protocolo según la configuración del registro de contenedor, pero los contenedores en tiempo de ejecución y demonio de IoT Edge deben utilizar siempre HTTPS.

* El **host_proxy** es una dirección para el servidor proxy. Si el servidor proxy requiere autenticación, puede proporcionar sus credenciales como parte de host del proxy con el siguiente formato: **usuario**:**contraseña**\@**host_proxy** .

* El **puerto_proxy** es el puerto de red en el que el proxy responde al tráfico de red.

## <a name="install-the-runtime"></a>Instalación de la instancia en tiempo de ejecución

Si va a instalar el entorno de ejecución de Azure IoT Edge en un dispositivo Linux, configure el administrador de paquetes para que pase por el servidor proxy para acceder al paquete de instalación. Por ejemplo, [configure apt-get para usar un http-proxy](https://help.ubuntu.com/community/AptGet/Howto/#Setting_up_apt-get_to_use_a_http-proxy). Una vez que el administrador de paquetes esté configurado, siga las instrucciones de [Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) o [Instalar el entorno de ejecución de Azure IoT Edge en Linux (x64)](how-to-install-iot-edge-linux.md) como de costumbre.

Si va a instalar el runtime de IoT Edge en un dispositivo de Windows, deberá ir dos veces a través del servidor proxy. La primera conexión es para descargar el archivo de script del instalador y la segunda conexión es durante la instalación para descargar los componentes necesarios. Puede configurar la información del proxy en la configuración de Windows, o incluir la información del proxy directamente en el script de instalación. El siguiente script de PowerShell es un ejemplo de una instalación de Windows con el argumento `-proxy`:

```powershell
. {Invoke-WebRequest -proxy <proxy URL> -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -proxy <proxy URL>
```

Si tiene credenciales de servidor proxy demasiado complejas como para incluirlas en la dirección URL, use el parámetro `-ProxyCredential` en el comando `-InvokeWebRequestParameters`. Por ejemplo,

```powershell
$proxyCredential = (Get-Credential).GetNetworkCredential()
. {Invoke-WebRequest -proxy <proxy URL> -ProxyCredential $proxyCredential -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -InvokeWebRequestParameters @{ '-Proxy' = '<proxy URL>'; '-ProxyCredential' = $proxyCredential }
```

Para obtener más información acerca de los parámetros de proxy, consulte [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest). Para obtener más información acerca de las opciones de instalación, consulte [Install Azure IoT Edge runtime on Windows](how-to-install-iot-edge-windows.md) (Instalación del entorno de ejecución de Azure IoT Edge en Windows).

Una vez que el entorno de ejecución de Azure IoT Edge esté instalado, use la sección siguiente para configurarlo con la información del proxy. 

## <a name="configure-the-daemons"></a>Configuración de los demonios

Los demonios Moby y IoT Edge que se ejecuta en el dispositivo de IoT Edge deben configurarse para usar el servidor proxy. El demonio Moby realiza las solicitudes web en extraer imágenes de contenedor desde registros de contenedor. El demonio de IoT Edge realiza solicitudes web para comunicarse con IoT Hub.

### <a name="moby-daemon"></a>Demonio MOBY

Puesto que Moby se basa en Docker, consulte la documentación de Docker para configurar el demonio Moby con variables de entorno. La mayoría de los registros de contenedor (incluidos los de DockerHub y Azure Container) admiten solicitudes HTTPS, por lo que el parámetro que se debe establecer es **HTTPS_PROXY**. Si va a extraer imágenes de un registro que no admite la seguridad de la capa de transporte (TLS), debe establecer el parámetro **HTTP_PROXY**. 

Elija el artículo que se aplica a su sistema operativo del dispositivo IoT Edge: 

* [Configurar el demonio de Docker en Linux](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)
    * El demonio Moby en dispositivos Linux mantiene el nombre de Docker.
* [Configurar el demonio de Docker en Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon#proxy-configuration)
    * El demonio Moby en dispositivos de Windows se denomina iotedge moby. Los nombres son diferentes porque es posible ejecutar Docker Desktop y Moby en paralelo en un dispositivo de Windows. 

### <a name="iot-edge-daemon"></a>Demonio de IoT Edge

El demonio de IoT Edge está configurado de forma similar al demonio Moby. Todas las solicitudes que IoT Edge envía a IoT Hub usan HTTPS. Utilice los pasos siguientes para establecer una variable de entorno para el servicio según su sistema operativo. 

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

#### <a name="windows"></a> Windows

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

Abra el archivo config.yaml en el dispositivo de IoT Edge. En los sistemas Linux, este archivo se encuentra en **/etc/iotedge/config.yaml**. En los sistemas Windows, este archivo se encuentra en **C:\ProgramData\iotedge\config.yaml**. El archivo de configuración está protegido, por lo que necesita privilegios de administrador para acceder a él. En sistemas Linux, eso significa que hay que usar el comando `sudo` antes de abrir el archivo en el editor de texto que prefiera. En Windows, esto significa abrir un editor de texto como el Bloc de notas para ejecutarlo como administrador y, a continuación, abrir el archivo. 

En el archivo config.yaml, busque la sección **especificación del módulo de agente de Edge**. La definición del agente de IoT Edge incluye un parámetro **env** en el que puede agregar variables de entorno. 

<!--
![edgeAgent definition](./media/how-to-configure-proxy-support/edgeagent-unedited.png)
-->

Elimine las llaves que son marcadores de posición del parámetro env y agregue la nueva variable en una nueva línea. Recuerde que las sangrías de YAML son dos espacios. 

```yaml
https_proxy: "<proxy URL>"
```

El runtime de IoT Edge usa AMQP de forma predeterminada para comunicarse con IoT Hub. Algunos servidores proxy bloquean los puertos AMQP. Si es así, también debe configurar edgeAgent para que use AMQP sobre WebSocket. Agregue una segunda variable de entorno.

```yaml
UpstreamProtocol: "AmqpWs"
```

![definición de edgeAgent con variables de entorno](./media/how-to-configure-proxy-support/edgeagent-edited.png)

Guarde los cambios en config.yaml y cierre el editor. Reinicie IoT Edge para que los cambios surtan efecto. 

* Linux: 

   ```bash
   sudo systemctl restart iotedge
   ```

* Windows:

   ```powershell
   Restart-Service iotedge
   ```

## <a name="configure-deployment-manifests"></a>Configuración de manifiestos de implementación  

Una vez que el dispositivo de IoT Edge esté configurado para funcionar con el servidor proxy, deberá declarar las variables de entorno en los manifiestos de implementación futuros. Configure siempre los dos módulos en tiempo de ejecución (edgeAgent y edgeHub) para que se comuniquen a través del servidor proxy con el fin de que puedan mantener una conexión con IoT Hub. Otros módulos de IoT Edge que se conecten a internet deben configurarse para el servidor proxy. Sin embargo, los módulos que enrutar los mensajes a través del centro de Edge o que solo se comunican con otros módulos en el dispositivo no necesitan los detalles del servidor proxy. 

Puede crear manifiestos de implementación mediante Azure Portal o editando manualmente un archivo JSON. 

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

