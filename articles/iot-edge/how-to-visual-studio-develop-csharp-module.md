---
title: Desarrollo y depuración de módulos de C# para Azure IoT Edge en Visual Studio 2017 | Microsoft Docs
description: Uso de Visual Studio 2017 para desarrollar y depurar módulos de C# para Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/24/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: ab4dd1186715fde51fbf188ace902c8092d192d0
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647194"
---
# <a name="use-visual-studio-2017-to-develop-and-debug-c-modules-for-azure-iot-edge-preview"></a>Uso de Visual Studio 2017 para desarrollar y depurar módulos de C# para Azure IoT Edge (versión preliminar)

Puede convertir la lógica de negocios en módulos de Azure IoT Edge. En este artículo se muestra cómo usar Visual Studio 2017 como herramienta principal para desarrollar y depurar módulos de C#.

Las herramientas de Azure IoT Edge para Visual Studio proporcionan las siguientes ventajas:

- Creación, edición, compilación y ejecución de soluciones y módulos de Azure IoT Edge en la máquina de desarrollo local.
- Implementación de la solución de Azure IoT Edge en un dispositivo de Azure IoT Edge gracias a Azure IoT Hub.
- Programación de los módulos de Azure IoT en C# disfrutando de las ventajas de desarrollo en Visual Studio.
- Administración de dispositivos de Azure IoT Edge y módulos sin interfaz de usuario. 

En este artículo se muestra cómo usar las herramientas de Azure IoT Edge para Visual Studio 2017 para desarrollar módulos de IoT Edge en C#. También aprenderá a implementar el proyecto en el dispositivo de Azure IoT Edge.

## <a name="prerequisites"></a>Requisitos previos
En este artículo se da por hecho que usa una máquina virtual que ejecuta Windows como máquina de desarrollo. El dispositivo IoT Edge puede ser otro dispositivo físico.

En este artículo se usa Visual Studio 2017 como herramienta de desarrollo principal, así que debe instalar Visual Studio. Y asegúrese de incluir la **carga de trabajo de desarrollo de Azure** en la instalación de Visual Studio 2017. También puede [modificar Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) y agregar la carga de trabajo de desarrollo de Azure.

Una vez listo Visual Studio 2017, también necesitará:

- Descargue e instale [extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) desde Visual Studio Marketplace para crear el proyecto de IoT Edge en Visual Studio 2017.
- [Docker Community Edition](https://docs.docker.com/install/) en la máquina de desarrollo para compilar y ejecutar las imágenes del módulo. Deberá establecer correctamente la versión de Docker CE que se ejecute en el modo de contenedor de Linux o Windows.
- Para configurar el entorno local de desarrollo para depurar, ejecutar y probar la solución de IoT Edge, necesita la [herramienta de desarrollo de Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Instale [Python (2.7/3.6) y Pip](https://www.python.org/). A continuación, instale **iotedgehubdev** mediante la ejecución del siguiente comando en el terminal. Asegúrese de que la versión de la herramienta de desarrollo de Azure IoT EdgeHub es posterior a la 0.3.0.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   - Puede usar un registro de Docker local con fines de prototipo y prueba en lugar de un registro en la nube. 

- Para probar el módulo necesita un centro de IoT activo con al menos un identificador de dispositivo de IoT Edge creado. Si ejecuta el demonio de seguridad de IoT Edge en una máquina de desarrollo, es posible que deba detener EdgeHub y EdgeAgent para empezar el desarrollo en Visual Studio. 

### <a name="check-your-tools-version"></a>Comprobación de la versión de herramientas

1. En el menú **Herramientas**, seleccione **Extensiones y actualizaciones**. Expanda **Instalado > Herramientas** y encontrará **Azure IoT Edge** y **Cloud Explorer**.

2. Observe la versión instalada. Puede comparar esta versión con la más reciente de Visual Studio Marketplace ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.azureiotedgetools))

3. Si su versión es anterior, actualice las herramientas en Visual Studio como se muestra en la siguiente sección.

### <a name="update-your-tools"></a>Actualización de las herramientas

1. En el cuadro de diálogo **Extensiones y actualizaciones**, expanda **Actualizaciones > Visual Studio Marketplace**, elija **Azure IoT Edge** o **Cloud Explorer** y seleccione **Actualizar**.

2. Después de descargar la actualización de las herramientas, cierre Visual Studio para desencadenar la actualización de las herramientas mediante el programa de instalación de VSIX.

3. En el instalador, elija **Aceptar** para iniciar y Modificar para actualizar las herramientas.

4. Una vez completada la actualización, elija Cerrar y reinicie Visual Studio.

### <a name="create-an-azure-iot-edge-project"></a>Creación de un proyecto de Azure IoT Edge

La plantilla de proyecto de Azure IoT Edge en Visual Studio crea un proyecto que se puede implementar en dispositivos de Azure IoT Edge en Azure IoT Hub. En primer lugar, cree una solución de Azure IoT Edge y, luego, genere el primer módulo de C# en esa solución. Cada solución de IoT Edge puede contener más de un módulo. 

1. En Visual Studio, seleccione **Nuevo** > **proyecto** en el menú **Archivo**.

2. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Instalado**, expanda **Visual C# > Nube**, seleccione **Azure IoT Edge**, escriba un **Nombre** de proyecto y especifique la ubicación; después, haga clic en **Aceptar**. El nombre de proyecto predeterminado es **AzureIoTEdgeApp1**. 

   ![Nuevo proyecto](./media/how-to-visual-studio-develop-csharp-module/create-new.jpg)

3. En la ventana **IoT Edge Module Configuration** (Configuración del módulo de IoT Edge), seleccione **C# Module** (Módulo de C#), y escriba y especifique el nombre del módulo y el repositorio de imágenes de este.  VS rellena automáticamente el nombre del módulo con **localhost:5000**. Reemplácelo por su propia información de registro. Si usa un registro de Docker local para realizar pruebas, localhost puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a **<registry name>.azurecr.io**. Reemplace solo la parte de localhost de la cadena, no elimine el nombre del módulo. El nombre de módulo predeterminado es **IoTEdgeModule1**

4. Haga clic en **Aceptar** para crear el proyecto de Azure IoT Edge con un módulo de C#.

Ahora tiene un proyecto **AzureIoTEdgeApp1** y uno **IoTEdgeModule1** en nuestra solución. El proyecto **AzureIoTEdgeApp1** tiene un archivo **deployment.template.json**. Este archivo define los módulos que desea compilar e implementar para la solución de IoT Edge y define las rutas entre los módulos. La solución predeterminada tiene un módulo **tempSensor** y otro **IoTEdgeModule1**. **tempSensor** genera datos simulados para **IoTEdgeModule1**, mientras que el código predeterminado de **IoTEdgeModule1** es un módulo de mensajes de la canalización, que los recibe directamente en IoT Hub.

El proyecto **IoTEdgeModule1** es una aplicación de consola de .Net Core 2.1. Contiene los archivos **Dockerfile** que necesita para que el dispositivo de IoT Edge se ejecute con el contenedor de Windows o Linux. El archivo **module.json** describe los metadatos de un módulo. **program.cs** es el código del módulo real, que toma el SDK de dispositivo IoT de Azure como dependencia.

## <a name="develop-your-module"></a>Desarrollo de su módulo

El código del módulo de C# predeterminado que se incluye con la solución se encuentra en **IoTEdgeModule1** > **Program.cs**. El módulo y el archivo deployment.template.json se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo para iniciar la prueba sin tocar el código. El módulo se compila para tomar simplemente la entrada de un origen (en este caso, el módulo tempSensor que simula datos) y canalizarlo a IoT Hub. 

Cuando esté listo para personalizar la plantilla de C# con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como seguridad, administración de dispositivos y confiabilidad. 

## <a name="initialize-iotegehubdev-with-iot-edge-device-connection-string"></a>Inicialización de **iotegehubdev** con la cadena de conexión de dispositivo IoT Edge

1. Para obtener la cadena de conexión de cualquier dispositivo IoT Edge, puede copiar el valor de "Cadena de conexión principal" de Cloud Explorer en Visual Studio 2017 de la manera siguiente. No copie la cadena de conexión del dispositivo que no es Edge, el icono de dispositivo IoT Edge es diferente al del dispositivo que no es Edge.

   ![Copia de la cadena de conexión del dispositivo Edge](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

2. Tiene que hacer clic con el botón derecho en el proyecto **AzureIoTEdgeApp1** para abrir el menú contextual y luego hacer clic en **Set Edge Device Connection String** (Establecer cadena de conexión del dispositivo Edge). Se mostrará la ventana de configuración de Azure IoT Edge.

   ![Apertura de la ventana de la cadena de conexión del dispositivo Edge establecida](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

3. En la ventana de configuración, especifique la cadena de conexión que obtuvo en el primer paso y haga clic en el botón **Aceptar**.

>[!NOTE]
>Se trata de un trabajo de aplicación única, tan solo hay que ejecutar este paso una vez en un equipo y todas las soluciones posteriores de Azure IoT Edge la obtendrán de manera gratuita. Por supuesto puede volver a ejecutar este paso si la cadena de conexión no es válida o si tiene que cambiar a otra cadena de conexión.

## <a name="build-and-debug-single-c-module"></a>Compilación y depuración de un solo módulo de C#

Por lo general, queremos probar y depurar los módulos antes de que se ejecuten en una solución entera con varios de ellos.

1. Seleccione **IoTEdgeModule1** como proyecto de inicio en el menú contextual.

   ![Establecimiento del proyecto de inicio](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

2. Presione **F5** o haga clic en el botón siguiente para ejecutar el módulo, puede tardar unos 10-20 segundos la primera vez.

   ![Ejecución del módulo](./media/how-to-visual-studio-develop-csharp-module/run-module.png)


3. Si el módulo se inicializa correctamente, verá una aplicación de consola de .Net Core iniciada.

   ![Módulo en ejecución](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

4. Ahora puede establecer un punto de interrupción en **PipeMessage** en **Program.cs** y enviar el mensaje al ejecutar el siguiente comando en **Git Bash** o **Bash WSL** (no lo ejecute en CMD ni Powershell) (también puede encontrar este comando en la ventana de salida de VS):

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Depuración de un solo módulo](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Debe desencadenarse el punto de interrupción. Puede inspeccionar las variables en la ventana de variables locales de Visual Studio.

   > [!TIP]
   > También puede usar [PostMan](https://www.getpostman.com/) u otras herramientas de API para enviar mensajes en lugar de `curl`.

5. Presione **Ctrl + F5** o haga clic en el botón Detener para detener la depuración.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Compilación y depuración de la solución IoT Edge con varios módulos

Cuando hayamos terminado de desarrollar el módulo, querremos ejecutar y depurar toda la solución de varios módulos.

1. Agregue el segundo módulo de C# a la solución. Haga clic con el botón derecho en **AzureIoTEdgeApp1** y seleccione **Agregar** -> **New IoT Edge Module** (Nuevo módulo de IoT Edge). El nombre predeterminado del segundo módulo es **IoTEdgeModule2** y sigue siendo un módulo de la canalización.

2. Vaya a **deployment.template.json**. Verá que **IoTEdgeModule2** se ha agregado a la sección **modules**. Reemplace la sección **routes** por lo siguiente. Si ha personalizado los nombres de módulo, asegúrese de actualizarlos a continuación tras el reemplazo.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

3. Establezca el proyecto **AzureIoTEdgeApp1** proyecto de inicio en el menú contextual.

4. Establezca los puntos de interrupción y presione F5 para poder ejecutar y depurar varios módulos a la vez. Verá varias ventanas de aplicación de consola de .Net Core, cada una de ellas indica un módulo de C#. 

   ![Depuración de varios módulos](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

5. Presione **Ctrl + F5** o haga clic en el botón Detener para detener la depuración.

## <a name="build-and-push-images"></a>Compilación e inserción de imágenes

1. Asegúrese de que **AzureIoTEdgeApp1** es el proyecto de inicio. Seleccione **Depurar** o la configuración de la **Versión** donde compilar las imágenes del módulo.

    > [!NOTE]
    > Al elegir **Depurar**, VS usará `Dockerfile.debug` para compilar las imágenes de Docker. Esto incluye el depurador de la línea de comandos de .NET Core VSDBG en la imagen de contenedor al realizar la compilación. Se recomienda usar la configuración de **Versión** que usa `Dockerfile` sin VSDBG para los módulos de IoT Edge listos para producción.

2. Si usa un registro privado como Azure Container Registry, ejecute el registro de Docker con el siguiente comando en el terminal. Si usa un registro local, lea [Run a local registry](https://docs.docker.com/registry/deploying/#run-a-local-registry) (Ejecutar un registro local).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server> 
    ```

3. Si usa un registro privado tipo Azure Container Registry, deberá escribir el nombre de usuario del registro y la contraseña en `deployment.template.json` en la configuración de Runtime con el siguiente contenido. No olvide reemplazar el marcador de posición por el nombre de usuario administrador real y la contraseña.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

4. Haga clic con el botón derecho en **AzureIoTEdgeApp1** y elija el elemento de menú contextual **Build and Push Edge Solution** (Compilar e insertar solución de Edge), compilará e insertará imágenes de Docker para cada módulo.

   ![Compilación e inserción de imágenes](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)


## <a name="deploy-the-solution"></a>Implementación de la solución

En el artículo de la guía de inicio rápido que siguió para configurar el dispositivo de IoT Edge, implementó un módulo mediante Azure Portal. También puede implementar módulos mediante Cloud Explorer para Visual Studio. Ya tiene un manifiesto de implementación preparado para su escenario, el archivo `deployment.json`. Ahora todo lo que necesita hacer es seleccionar un dispositivo que reciba la implementación.

1. Abra **Cloud Explorer** al hacer clic en **Ver** > **Cloud Explorer**. Asegúrese de haber iniciado sesión en Visual Studio 2017 con su cuenta.

2. En **Cloud Explorer**, expanda su suscripción, busque Azure IoT Hub y el dispositivo de Azure IoT Edge que desea implementar.

3. Haga clic con el botón derecho en el dispositivo de IoT Edge que crear e implementar; debe elegir el archivo de manifiesto de implementación de `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`.

>>[!NOTE]
>>No debe seleccionar `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json`.

4. Haga clic en el botón Actualizar. Debería ver los nuevos módulos en ejecución junto con el módulo **TempSensor**, **$edgeAgent** y **$edgeHub**.

## <a name="view-generated-data"></a>Visualización de datos generados

1. Para supervisar el mensaje de D2C de un determinado dispositivo, haga clic en el dispositivo en la lista y en **Start Monitoring D2C Messages** (Iniciar la supervisión de mensajes de D2C) en la ventana **Acción**.

2. Para detener la supervisión de datos, haga clic en el dispositivo en la lista y seleccione **Stop Monitoring D2C Messages** (Detener la supervisión de mensajes de D2C) en la ventana **Acción**.

## <a name="next-steps"></a>Pasos siguientes

Para desarrollar módulos para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
