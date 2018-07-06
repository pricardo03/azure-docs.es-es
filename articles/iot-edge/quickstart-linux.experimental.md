---
title: Simulación de Azure IoT Edge en Linux | Microsoft Docs
description: En esta guía de inicio rápido, aprenda a implementar código creado previamente de manera remota en un dispositivo IoT Edge.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0e0d22b3363b00c81be5091fd12773f9e486c09e
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099192"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Guía de inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo Linux x64

Azure IoT Edge le permite realizar análisis y procesamiento de datos en los dispositivos, en lugar de tener que insertar todos los datos en la nube. En los tutoriales de IoT Edge se muestra cómo implementar diferentes tipos de módulos, pero primero se necesita un dispositivo para probarlo. 

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

1. Cree un centro de IoT Hub.
2. Registre un dispositivo IoT Edge en su instancia de IoT Hub.
3. Iniciar el entorno de ejecución de IoT Edge.
4. Implemente un módulo de manera remota en un dispositivo IoT Edge.

![Arquitectura del tutorial][2]

Esta guía de inicio rápido convierte su equipo o maquina virtual Linux en un dispositivo IoT Edge. Después, puede implementar un módulo desde Azure Portal en el dispositivo. El módulo que se implementa en esta guía de inicio rápido es un sensor simulado que genera datos de temperatura, humedad y presión. Los otros tutoriales de Azure IoT Edge se basan en el trabajo que se realiza aquí mediante la implementación de módulos que analizan los datos simulados para obtener información empresarial. 

Si no tiene una suscripción activa a Azure, cree una [cuenta gratuita][lnk-account] antes de comenzar.

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Para comenzar la guía de inicio rápido, cree su instancia de IoT Hub en Azure Portal.
![Creación de una instancia de IoT Hub][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registro de un dispositivo de IoT Edge

Registre un dispositivo de IoT Edge con la instancia de IoT Hub recién creada.
![Registro de un dispositivo][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]


## <a name="install-and-start-the-iot-edge-runtime"></a>Instale e inicie el runtime de IoT Edge

Instale e inicie el runtime de Azure IoT Edge en el dispositivo. 
![Registro de un dispositivo][5]

El runtime de IoT Edge se implementa en todos los dispositivos de IoT Edge. Tiene tres componentes. El **demonio de seguridad de IoT Edge** se inicia cada vez que se inicia un dispositivo perimetral y arranca el dispositivo mediante el inicio del agente de IoT Edge. El **agente de IoT Edge** facilita la implementación y supervisión de los módulos en el dispositivo IoT Edge, incluido el centro de IoT Edge. El **centro de IoT Edge** administra las comunicaciones entre los módulos del dispositivo de IoT Edge y entre el dispositivo y la instancia de IoT Hub. 

### <a name="register-your-device-to-use-the-software-repository"></a>Registro del dispositivo para que use el repositorio de software

Los paquetes necesarios para ejecutar el entorno de ejecución de Azure IoT Edge se administran en un repositorio de software. Configure el dispositivo IoT Edge para acceder a este repositorio. 

Los pasos de esta sección son para dispositivos que ejecutan **Ubuntu 16.04**. Para acceder al repositorio de software en otras versiones de Linux, consulte [Install the Azure IoT Edge runtime on Linux (x64)](how-to-install-iot-edge-linux.md) [Instalación del entorno de ejecución de Azure IoT Edge en Linux (x64)] o [Install Azure IoT Edge runtime on Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) [Instalación del entorno de ejecución de Azure IoT Edge en Linux (ARM32v7/armhf)].

1. En la máquina que está utilizando como dispositivo IoT Edge, instale la configuración del repositorio.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Instale una clave pública para acceder al repositorio.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Instalación de un entorno de ejecución del contenedor

El entorno de ejecución de IoT Edge es un conjunto de contenedores y la lógica que implementa en el dispositivo IoT Edge se empaqueta en forma de contenedores. Prepare el dispositivo para estos componentes mediante la instalación de un entorno de ejecución del contenedor.

Actualice **apt-get**.

   ```bash
   sudo apt-get update
   ```

Instale Moby, un entorno de ejecución del contenedor y sus comandos de la CLI. 

   ```bash
   sudo apt-get install moby-engine
   sudo apt-get install moby-cli   
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>Instalación y configuración del demonio de seguridad de IoT Edge

El demonio de seguridad se instala como un servicio del sistema para que el entorno de ejecución de IoT Edge se inicie cada vez que arranca el dispositivo. La instalación también incluye una versión de **hsmlib** que permite que el demonio de seguridad interactúe con la seguridad del hardware del dispositivo. 

1. Descargue e instale el demonio de seguridad de IoT Edge. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Abra el archivo de configuración de IoT Edge. Es un archivo protegido por lo que puede que tenga que usar privilegios elevados para acceder a él.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Agregue la cadena de conexión del dispositivo IoT Edge que copió cuando registró el dispositivo. Reemplace el valor de la variable **device_connection_string** que copió anteriormente en esta guía de inicio rápido.

4. Reinicie el demonio de seguridad de Edge:

   ```bash
   sudo systemctl restart iotedge
   ```

5. Compruebe que el demonio de seguridad de Edge se ejecuta como un servicio del sistema:

   ```bash
   sudo systemctl status iotedge
   ```

   ![Comprobación de cómo el demonio de Edge se ejecuta con un servicio del sistema](./media/quickstart-linux/iotedged-running.png)

   También puede ver los registros del demonio de seguridad de Edge ejecutando el comando siguiente:

   ```bash
   journalctl -u iotedge
   ```

6. Vea los módulos que se ejecutan en el dispositivo: 

   ```bash
   sudo iotedge list
   ```
Después de un cierre de sesión y el inicio de sesión, *sudo* no es necesario para el comando anterior.

   ![Visualización de un módulo en el dispositivo](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Implementación de un módulo

Administre el dispositivo Azure IoT Edge desde la nube para implementar un módulo que enviará datos de telemetría a IoT Hub.
![Registro de un dispositivo][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Visualización de datos generados

En esta guía de inicio rápido, ha creado un nuevo dispositivo de IoT Edge y ha instalado el runtime de IoT Edge en él. Luego, ha usado Azure Portal para insertar un módulo de IoT Edge para que se ejecute en el dispositivo sin tener que realizar cambios en el propio dispositivo. En este caso, el módulo que ha insertado crea datos del entorno que se pueden usar para los tutoriales. 

Vuelva a abrir el símbolo del sistema en el equipo que ejecuta el dispositivo simulado. Confirme que el módulo implementado desde la nube se está ejecutando en el dispositivo IoT Edge:

   ```bash
   sudo iotedge list
   ```
Después de un cierre de sesión y el inicio de sesión, *sudo* no es necesario para el comando anterior.

   ![Ver tres módulos en el dispositivo](./media/quickstart-linux/iotedge-list-2.png)

Vea los mensajes que se envían desde el módulo tempSensor:

  ```bash
   sudo iotedge logs tempSensor -f 
   ```

Después de un cierre de sesión y el inicio de sesión, *sudo* no es necesario para el comando anterior.

![Ver los datos desde el módulo](./media/quickstart-linux/iotedge-logs.png)

Puede que el módulo del sensor de temperatura esté esperando para conectarse al centro de IoT Edge si la última línea que se ve en el registro es `Using transport Mqtt_Tcp_Only`. Intente terminar el módulo y dejar que el agente de Edge lo reinicie. Puede terminarlo mediante el comando `sudo docker stop tempSensor`.

Puede ver también la telemetría que el dispositivo envía mediante la [herramienta del explorador de IoT Hub][lnk-iothub-explorer] o la [extensión de Azure IoT Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea continuar con los tutoriales de IoT Edge, puede usar el dispositivo que ha registrado y configurado en esta guía de inicio rápido. Si desea quitar las instalaciones desde su dispositivo, use los siguientes comandos.  

Quite el entorno de ejecución de Azure IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Elimine los contenedores que se crearon en el dispositivo. 

   ```bash
   sudo docker rm -f $(sudo docker ps -aq)
   ```

Quite el entorno de ejecución del contenedor.

   ```bash
   sudo apt-get remove --purge moby
   ```

Cuando ya no necesite el centro de IoT o el dispositivo IoT Edge que creó en esta guía de inicio rápido, puede eliminarlos en Azure Portal. Vaya a la página de información general del centro de IoT y seleccione **Eliminar**. 

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un nuevo dispositivo IoT Edge y ha usado la interfaz en la nube de Azure IoT Edge para implementar el código en el dispositivo. Ahora tiene un dispositivo simulado que genera datos sin procesar acerca de su entorno. 

Esta guía de inicio rápido es el requisito previo para todos los demás tutoriales de IoT Edge. Puede continuar con cualquiera de los demás tutoriales para obtener información acerca de la forma en que Azure IoT Edge puede ayudarle a transformar estos datos en información empresarial en el perímetro.

> [!div class="nextstepaction"]
> [Filtrado de los datos del sensor mediante una función de Azure](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
