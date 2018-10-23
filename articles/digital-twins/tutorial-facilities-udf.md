---
title: Supervisión de un espacio con Azure Digital Twins | Microsoft Docs
description: Si sigue los pasos de este tutorial, aprenderá a aprovisionar los recursos espaciales y a supervisar las condiciones de trabajo con Azure Digital Twins.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1e5cb18b4e526cd0a0607f5bc93788fcf07430e1
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364242"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins"></a>Tutorial: Aprovisionamiento del edificio y supervisión de las condiciones de trabajo con Azure Digital Twins

En este tutorial se muestra cómo usar Azure Digital Twins para supervisar las condiciones de temperatura y el nivel de comodidad deseados en los distintos espacios. Una vez que haya [configurado el edificio de ejemplo](tutorial-facilities-setup.md), con los pasos de este tutorial puede aprovisionarlo y ejecutar funciones personalizadas en los datos de los sensores.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Definir las condiciones que se van a supervisar
> * Crear una función definida por el usuario
> * Simular los datos de los sensores
> * Obtener los resultados de una función definida por el usuario

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se supone que ha [configurado Azure Digital Twins](tutorial-facilities-setup.md). Antes de continuar, asegúrese de que:
- tiene una [cuenta de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F),
- hay una instancia de Digital Twins en ejecución, 
- los [ejemplos en C# de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) se han descargado y extraído en su equipo de trabajo, 
- tiene la [versión 2.1.403 de SDK de .NET Core, o cualquier versión superior,](https://www.microsoft.com/net/download) en el equipo de desarrollo para compilar y ejecutar el ejemplo. Ejecute `dotnet --version` para comprobar si está instalada la versión correcta. 
- Tiene [Visual Studio Code](https://code.visualstudio.com/) para explorar el código de ejemplo. 

## <a name="define-conditions-to-monitor"></a>Definición de las condiciones que se van a supervisar
Puede definir el conjunto de condiciones específicas que se supervisan en el dispositivo o en los datos de los sensores. Dicho conjunto se denomina **buscador de coincidencias**. Luego, puede definir funciones, llamadas las *funciones definidas por el usuario*, que ejecutan una lógica personalizada en los datos procedentes de los espacios y dispositivos cuando se dan las condiciones que se producen en el buscadores de coincidencias. Para más información, lea [Data processing and user-defined functions](concepts-user-defined-functions.md) (Procesamiento de datos y funciones definidas por el usuario). 

En el proyecto de ejemplo **_occupancy-quickstart_**, abra el archivo **_src\actions\provisionSample.yaml_** en Visual Studio Code. Observe la sección que comienza con el tipo **matchers**. Cada una de las entradas de este tipo crea un buscador de coincidencias con el valor de **Name** especificado que supervisará un sensor del tipo **dataTypeValue**. Observe cómo se relaciona con el espacio denominado *Focus Room A1*, que tiene un nodo **devices**, que contiene algunos **sensores**. Para aprovisionar un buscador de coincidencias que realice el seguimiento de uno de estos sensores, el valor de **dataTypeValue** debe coincidir con el valor **dataType** de dicho sensor. 

Agregue el siguiente buscador de coincidencias debajo de los buscadores de coincidencias existentes y asegúrese de que las claves están alineadas y espacios no se sustituyen por caracteres de tabulación:

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Se realizará el seguimiento del sensor *SAMPLE_SENSOR_TEMPERATURE* que agregó en [el primer tutorial](tutorial-facilities-setup.md). Tenga en cuenta que estas líneas también se encuentran en el archivo *provisionSample.yaml* como líneas con comentarios; para que dejen de ser comentarios no tiene más que quitar el carácter '#' que hay al principio de cada línea. 

<a id="udf" />

## <a name="create-a-user-defined-function"></a>Creación de una función definida por el usuario
Las funciones definidas por el usuario, o UDF, permiten personalizar el procesamiento de los datos de los sensores. Son código JavaScript personalizado que se puede ejecutar en una instancia de Digital Twins cuando se producen determinadas condiciones que describen los buscadores de coincidencias. Se pueden crear *buscadores de coincidencias* y *funciones definidas por el usuario* para todos los sensor que se deseen supervisar. Para más información al respecto, lea [Procesamiento de datos y funciones definidas por el usuario](concepts-user-defined-functions.md). 

En el archivo *provisionSample.yaml* del ejemplo, busque un sección que comience por el tipo **userdefinedfunctions**. En esta sección se aprovisiona una función definida por el usuario con un valor de **Name** dado, que actúa en la lista de buscadores de coincidencias de **matcherNames**. Tenga en cuenta que puede especificar su propio archivo de JavaScript para la UDF como **script**. Fíjese también en la sección denominada **roleassignments**. Asigna el rol de *administrador de espacio* a la función definida por el usuario, lo que le permite acceder a los eventos procedentes de cualquiera de los espacios aprovisionados. 

1. Configure la UDF para incluir el buscador de coincidencias de temperatura. Para ello, debe agregar o quitar el comentario de la línea siguiente del nodo `matcherNames` del archivo *provisionSample.yaml*:

    ```yaml
            - Matcher Temperature
    ```

1. Abra el archivo **_src\actions\userDefinedFunctions\availability.js_** en el editor. Este es el archivo al que se hace referencia en el elemento **script** de *provisionSample.yaml*. La función definida por el usuario de este archivo busca condiciones cuando no se detecta movimiento en la sala, así como cuando los niveles de dióxido de carbono están por debajo de 1000 ppm. Modifique el archivo de JavaScript para supervisar la temperatura, además de otras condiciones. Agregue las siguientes líneas de código para buscar condiciones cuando no se detecte movimiento en la sala, cuando los niveles de dióxido de carbono no lleguen a 1000 ppm y la temperatura sea inferior a 78 ºF.

   > [!NOTE]
   > En esta sección se modifica el archivo *src\actions\userDefinedFunctions\availability.js* para explicar detalladamente una manera de escribir una función definida por el usuario. Sin embargo, puede elegir utilizar directamente el archivo [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) en su configuración. Dicho archivo contiene todos los cambios necesarios para este tutorial. Si lo utiliza, asegúrese de usar el nombre de archivo correcto para la clave del **_script_** en [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

    1. Al principio del archivo, agregue las siguientes líneas para la temperatura debajo del comentario `// Add your sensor type here`:

        ```JavaScript
            var temperatureType = "Temperature";
            var temperatureThreshold = 78;
        ```
   
    1. Agregue las líneas siguientes después de la instrucción que define `var motionSensor`, debajo del comentario `// Add your sensor variable here`:

        ```JavaScript
            var temperatureSensor = otherSensors.find(function(element) {
                return element.DataType === temperatureType;
            });
        ```
    
    1. Agregue la línea siguiente después de la instrucción que define `var carbonDioxideValue`, debajo del comentario `// Add your sensor latest value here`:

        ```JavaScript
            var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
        ```
    
    1. Quite las siguientes líneas de código de debajo del comentario `// Modify this line to monitor your sensor value`: 

        ```JavaScript
            if(carbonDioxideValue === null || motionValue === null) {
                sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
                return;
            }
        ```
       
       Reemplácelas por estas:

        ```JavaScript
            if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
                sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
                return;
            }
        ```
    
    1. Quite las siguientes líneas de código de debajo del comentario `// Modify these lines as per your sensor`:

        ```JavaScript
            var availableFresh = "Room is available and air is fresh";
            var noAvailableOrFresh = "Room is not available or air quality is poor";
        ```

       Reemplácelas por estas:

        ```JavaScript
            var alert = "Room with fresh air and comfortable temperature is available.";
            var noAlert = "Either room is occupied, or working conditions are not right.";
        ```
    
    1. Quite el siguiente bloque de código *if-else* después del comentario `// Modify this code block for your sensor`:

        ```JavaScript
            // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
            if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
                log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
                setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);

                // Set up custom notification for air quality
                parentSpace.Notify(JSON.stringify(availableFresh));
            }
            else {
                log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
                setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

                // Set up custom notification for air quality
                parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
            }
        ```
      
       Y sustitúyala por el siguiente bloque *if-else*:

        ```JavaScript
            // If sensor values are within range and room is available
            if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
                log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

                // log, notify and set parent space computed value
                setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

                // Set up notification for this alert
                parentSpace.Notify(JSON.stringify(alert));
            }
            else {
                log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);
    
                // log, notify and set parent space computed value
                setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
            }
        ```
        
        La UDF modificada buscará una condición en la que una sala esté disponible y tenga el dióxido de carbono y la temperatura dentro de unos límites tolerables. Cuando dicha condición se cumpla, generará una notificación con la instrucción `parentSpace.Notify(JSON.stringigy(alert));`. Establece el valor del espacio supervisado, independientemente de si se cumple la condición, con el mensaje correspondiente.
    
    1. Guarde el archivo. 
    
1. Abra la ventana de comandos y vaya a la carpeta **_occupancy-quickstart\src_**. Ejecute el siguiente comando para aprovisionar el grafo de inteligencia espacial y la función definida por el usuario. 

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   > [!IMPORTANT]
   > Para evitar el acceso no autorizado a API de administración de Digital Twins, la aplicación **_occupancy-quickstart_** requiere que inicie sesión con las credenciales de su cuenta de Azure. Guarda las credenciales durante un breve periodo, así que es posible que no tenga que iniciar sesión cada vez que lo ejecute. Tanto la primera vez que se ejecute el programa como cuándo expiren las credenciales guardadas, se le dirigirá a una página de inicio de sesión y se le proporcionará un código específico de la sesión para entrar en la página. Siga las indicaciones para iniciar sesión con su cuenta de Azure.


1. Una vez que se autentique la cuenta, la aplicación empezará a crear un grafo espacial de ejemplo como está configurado en *provisionSample.yaml*. Espere hasta que se complete el aprovisionamiento; tardará unos minutos. Cuando se haya completado, observe los mensajes de la ventana de comandos y cómo se crea el grafo espacial. Observe cómo crea una instancia de IoT Hub en el nodo raíz o en `Venue`. 

1. En la salida en la ventana de comandos, copie el valor de `ConnectionString`, en la sección `Devices`, en el Portapapeles. Dicho valor se necesitará para simular la conexión del dispositivo en la sección siguiente.

    ![Ejemplo de aprovisionamiento](./media/tutorial-facilities-udf/run-provision-sample.png)

> [!TIP]
> Si recibe un mensaje de error similar a "The I/O operation has been aborted because of either a thread exit or an application request" (La operación de E/S se anuló debido a una salida de subproceso o una solicitud de aplicación) en medio del aprovisionamiento, pruebe a volver a ejecutar el comando. Esto puede ocurrir si el cliente HTTP ha agotado el tiempo de espera debido a algún problema en la red.

<a id="simulate" />

## <a name="simulate-sensor-data"></a>Simulación de los datos de los sensores
En esta sección, usará el proyecto llamado *device-connectivity* del ejemplo, para simular los datos de los sensores para detectar movimiento, temperatura y dióxido de carbono. Este proyecto genera valores aleatorios para los sensores y los envía a la instancia de IoT Hub mediante la cadena de conexión del dispositivo.

1. En una ventana de comandos independiente, vaya al ejemplo de Digital Twins y, después, a la carpeta  **_device-connectivity_**.

1. Ejecute este comando para asegurarse de que las dependencias del proyecto son correctas:

    ```cmd/sh
    dotnet restore
    ```

1. Abra el archivo *appSettings.json* en el editor y edite los valores siguientes:
    1. *DeviceConnectionString*: asigne el valor de `ConnectionString` en la ventana de salida de la sección anterior. Asegúrese de copiar estas cadena completamente, entre comillas, para que el simulador se conecte correctamente con la instancia de IoT Hub.

    1. *HardwareId* en la matriz *Sensors*: dado que simula eventos procedentes de sensores aprovisionados en una instancia de Digital Twins, tanto el identificador de hardware como los nombres de los sensores de este archivo deben coincidir con el nodo `sensors` del archivo *provisionSample.yaml*. Agregar una nueva entrada para el sensor de temperatura; el nodo **Sensors** nodo de *appSettings.json* debe ser como este:

        ```JSON
        "Sensors": [{
          "DataType": "Motion",
          "HardwareId": "SAMPLE_SENSOR_MOTION"
        },{
          "DataType": "CarbonDioxide",
          "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
        },{
          "DataType": "Temperature",
          "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
        }]
        ```

1. Ejecute este comando para iniciar la simulación de eventos de temperatura, movimiento y dióxido de carbono del de dispositivo:

    ```cmd/sh
    dotnet run
    ```

   > [!NOTE] 
   > Como el ejemplo de simulación no se comunica directamente con la instancia de Digital Twin, no requiere autenticación.

## <a name="get-results-of-user-defined-function"></a>Obtención de los resultados de una función definida por el usuario
La función definida por el usuario se ejecuta cada vez que una instancia recibe datos del dispositivo y de los sensores. Esta sección consulta su instancia de Digital Twins para obtener los resultados de la función definida por el usuario. Verá casi en tiempo real cuando una sala está disponible, el aire es puro y la temperatura es la adecuada. 

1. Abra la ventana de comandos que usó para aprovisionar el ejemplo o una nueva y vaya otra vez a la carpeta **_occupancy-quickstart\src_** del ejemplo. 

1. Ejecute el comando siguiente e inicie sesión cuando se le solicite:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

La ventana de salida mostrará la ejecución de la función definida por el usuario e interceptará los eventos de la simulación del dispositivo. 

   ![Ejecutar UDF](./media/tutorial-facilities-udf/udf-running.png)

En función de que se cumpla la condición supervisada, la función definida por el usuario establece el valor del espacio con el mensaje pertinente, tal como se ha visto en [la sección anterior](#udf), que la función `GetAvailableAndFreshSpaces` imprime en la consola. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea dejar de explorar Azure Digital Twins aquí, elimine los recursos creados en este tutorial:

1. En el menú izquierdo de [Azure Portal](http://portal.azure.com), haga clic en **Todos los recursos**, seleccione el grupo de recurso de Digital Twins y haga clic en **Eliminar**.
2. Si lo necesita, también puede eliminar las aplicaciones de ejemplo de su equipo de trabajo. 


## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprovisionado los espacios y creado un marco para desencadenar notificaciones personalizadas, puede continuar con cualquiera de los siguientes tutoriales. 

> [!div class="nextstepaction"]
> [Tutorial: Recepción de notificaciones de los espacios de Azure Digital Twins mediante Logic Apps](tutorial-facilities-events.md)

O bien,

> [!div class="nextstepaction"]
> [Tutorial: Visualización y análisis de eventos de los espacios de Azure Digital Twins mediante Time Series Insights](tutorial-facilities-analyze.md)
