---
title: Visualización de datos en tiempo real de datos del sensor desde Azure IoT hub en una aplicación web | Microsoft Docs
description: Usar una aplicación web para visualizar datos de temperatura y humedad que se recopilan desde un sensor y se envían a Iot hub.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 05/31/2019
ms.author: robinsh
ms.openlocfilehash: 22b15a95e529d4f09560e9b7e59d9f78f70651bc
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476012"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-in-a-web-application"></a>Visualizar datos de sensor en tiempo real desde Azure IoT hub en una aplicación web

![Diagrama integral](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Conocimientos que adquirirá

En este tutorial, aprenderá a visualizar los datos de sensor en tiempo real que recibe su IoT hub con una aplicación web de node.js que se ejecutan en el equipo local. Después de ejecutar la aplicación web localmente, opcionalmente, puede seguir los pasos para hospedar la aplicación web en Azure App Service. Si quiere intentar visualizar los datos en su IoT Hub con Power BI, vea [Visualización de datos del sensor en tiempo real desde Azure IoT Hub mediante Power BI](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Qué debe hacer

* Agregar un grupo de consumidores a IoT hub que la aplicación web usará para leer datos del sensor
* Descargue el código de aplicación web de GitHub
* Examine el código de aplicación web
* Configurar las variables de entorno para contener artefactos necesarios para la aplicación web de IoT Hub
* Ejecutar la aplicación web en el equipo de desarrollo
* Abrir una página web para ver los datos de temperatura y humedad en tiempo real desde IoT hub
* (Opcional) Usar la CLI de Azure para hospedar la aplicación web en Azure App Service

## <a name="what-you-need"></a>Lo que necesita

* Completar la [simulador en línea de Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md) tutorial o uno de los tutoriales de dispositivo; por ejemplo, [Raspberry Pi con node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Estos cubren los siguientes requisitos:

  * Una suscripción de Azure activa.
  * Un IoT Hub en su suscripción.
  * Una aplicación cliente que envíe mensajes a su IoT Hub.

* [Descargar Git](https://www.git-scm.com/downloads)

* En este artículo se da un equipo de desarrollo de Windows; Sin embargo, puede realizar fácilmente estos pasos en un sistema Linux en el shell que prefiera.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ejecute el siguiente comando para agregar la extensión IoT de Microsoft Azure para la CLI de Azure a la instancia de Cloud Shell. La extensión IOT agrega comandos específicos de IoT Hub, IoT Edge e IoT Device Provisioning Service (DPS) a la CLI de Azure.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

## <a name="add-a-consumer-group-to-your-iot-hub"></a>Adición de un grupo de consumidores a IoT Hub

[Grupos de consumidores](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) proporcionan vistas independientes de la secuencia de eventos que permiten a las aplicaciones y servicios de Azure para consumir datos desde el mismo extremo de centro de eventos de forma independiente. En esta sección, agrega un grupo de consumidores a punto de conexión integrado del su IoT hub que va a usar para leer datos de la aplicación web.

Ejecute el siguiente comando para agregar un grupo de consumidores para el punto de conexión integrado de IoT hub:

```azurecli-interactive
az iot hub consumer-group create --hub-name YourIoTHubName --name YourConsumerGroupName
```

Tenga en cuenta el nombre que elija, necesitará más adelante en este tutorial.

## <a name="get-a-service-connection-string-for-your-iot-hub"></a>Obtiene una cadena de conexión de servicio del centro de IoT

Se crean instancias de IoT hub con varias directivas de acceso de forma predeterminada. Una directiva de este tipo es el **servicio** policy, que proporciona los permisos necesarios para un servicio leer y escribir los puntos de conexión del centro de IoT. Ejecute el siguiente comando para obtener una cadena de conexión de IoT hub que se adhiere a la directiva del servicio:

```azurecli-interactive
az iot hub show-connection-string --hub-name YourIotHub --policy-name service
```

La cadena de conexión debe ser similar al siguiente:

```javascript
"HostName={YourIotHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"
```

Anote la cadena de conexión de servicio, necesitará más adelante en este tutorial.

## <a name="download-the-web-app-from-github"></a>Descargue la aplicación web de GitHub

Abra una ventana de comandos y escriba los comandos siguientes para descargar el ejemplo de GitHub y cambie al directorio de ejemplo:

```cmd
git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
cd web-apps-node-iot-hub-data-visualization
```

## <a name="examine-the-web-app-code"></a>Examine el código de aplicación web

En el directorio web-apps-node-iot-hub-data-visualization, abra la aplicación web en su editor favorito. La continuación muestra la estructura de archivos que se ven en VS Code:

![Estructura de archivos de aplicación Web](./media/iot-hub-live-data-visualization-in-web-apps/web-app-files.png)

Dedique un momento a examinar los archivos siguientes:

* **Server.js** es un script del lado del servicio que inicializa el socket web y la clase contenedora de centro de eventos. Proporciona una devolución de llamada a la clase de contenedor del centro de eventos que utiliza la clase para difundir los mensajes entrantes al socket web.

* **Eventos-hub-reader.js** es un script del lado del servicio que se conecta a punto de conexión integrado la instancia de IoT hub con el grupo de consumidores y cadena de conexión especificada. Extrae el valor de DeviceId y EnqueuedTimeUtc de metadatos en los mensajes entrantes y, a continuación, transmite el mensaje mediante el método de devolución de llamada registrado por server.js.

* **Gráfico-dispositivo-data.js** es un script de cliente que escucha en el socket web, realiza un seguimiento de cada DeviceId y almacena los 50 últimos puntos de datos de entrada para cada dispositivo. A continuación, se enlaza los datos del dispositivo seleccionado para el objeto de gráfico.

* **Index.HTML** controla el diseño de interfaz de usuario para la página web y hace referencia a los scripts necesarios para la lógica del lado cliente.

## <a name="configure-environment-variables-for-the-web-app"></a>Configurar las variables de entorno para la aplicación web

Para leer datos desde IoT hub, la aplicación web necesita la cadena de conexión del IoT hub y el nombre del grupo de consumidor que debe leer. Estas cadenas obtiene desde el entorno de proceso en las siguientes líneas en server.js:

```javascript
const iotHubConnectionString = process.env.IotHubConnectionString;
const eventHubConsumerGroup = process.env.EventHubConsumerGroup;
```

Establecer las variables de entorno en la ventana de comandos con los siguientes comandos. Reemplace los valores de marcador de posición por la cadena de conexión de servicio para IoT hub y el nombre del grupo de consumidores que creó anteriormente. No las cadenas entre comillas.

```cmd
set IotHubConnectionString=YourIoTHubConnectionString
set EventHubConsumerGroup=YourConsumerGroupName
```

## <a name="run-the-web-app"></a>Ejecución de la aplicación web

1. Asegúrese de que el dispositivo está ejecutando y envío de datos.

2. En la ventana de comandos, ejecute las líneas siguientes para descargar e instalar paquetes con referencia e iniciar el sitio Web:

   ```cmd
   npm install
   npm start
   ```

3. Debería ver la salida en la consola que indica que la aplicación web se ha conectado correctamente a IoT hub y está escuchando en el puerto 3000:

   ![A en la consola de la aplicación Web](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-start.png)

## <a name="open-a-web-page-to-see-data-from-your-iot-hub"></a>Abrir una página web para ver los datos desde IoT hub

Abra un explorador en `http://localhost:3000`.

En el **seleccionar un dispositivo** lista, seleccione el dispositivo para ver un gráfico de ejecución de los 50 últimos puntos de datos de temperatura y humedad a IoT hub enviados el dispositivo.

![Página de aplicación web que muestra la temperatura y humedad en tiempo real](./media/iot-hub-live-data-visualization-in-web-apps/web-page-output.png)

También debería ver la salida en la consola que muestra los mensajes que se difunde su aplicación web en el explorador del cliente:  

![Aplicación Web de salida en la consola de difusión](./media/iot-hub-live-data-visualization-in-web-apps/web-app-console-broadcast.png)

## <a name="host-the-web-app-in-app-service"></a>Host de la aplicación web en App Service

El [característica Web Apps de Azure App Service](https://docs.microsoft.com/azure/app-service/overview) proporciona una plataforma como servicio (PAAS) para hospedar aplicaciones web. Las aplicaciones Web hospedadas en Azure App Service pueden beneficiarse de eficaces características de Azure, como seguridad adicional, equilibrio de carga y escalabilidad que así como Azure y de socios DevOps soluciones como la implementación continua, administración de paquetes y así sucesivamente. Azure App Service admite aplicaciones web desarrolladas en muchos lenguajes populares e implementado en la infraestructura de Windows o Linux.

En esta sección, aprovisionar una aplicación web en App Service e implementa el código en ella mediante comandos de CLI de Azure. Puede encontrar detalles de los comandos usados en el [az webapp](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest) documentación. Antes de comenzar, asegúrese de que ha completado los pasos necesarios para [agregar un grupo de recursos a su IoT hub](#add-a-consumer-group-to-your-iot-hub), [obtener una cadena de conexión de servicio del centro de IoT](#get-a-service-connection-string-for-your-iot-hub), y [descargar la aplicación web desde GitHub](#download-the-web-app-from-github).

1. Un [plan de App Service](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) define un conjunto de recursos de proceso para una aplicación hospedada en App Service que ejecute. En este tutorial, usamos el nivel gratis o Developer para hospedar la aplicación web. Con el nivel gratis, la aplicación web se ejecuta en recursos compartidos de Windows con otras aplicaciones de App Service, incluidas las aplicaciones de otros clientes. Azure también ofrece que planes de App Service para implementar web apps en Linux recursos de proceso. Puede omitir este paso si ya tiene un plan de App Service que se va a usar.

   Para crear un plan de App Service utiliza el nivel gratis de Windows, ejecute el siguiente comando. Use IoT hub está en el mismo grupo de recursos. Puede contener el nombre del plan de servicio superior y letras minúsculas, números y guiones.

   ```azurecli-interactive
   az appservice plan create --name <app service plan name> --resource-group <your resource group name> --sku FREE
   ```

2. Ahora puede aprovisionar una aplicación web en su plan de App Service. El `--deployment-local-git` parámetro permite que el código de aplicación web se cargan e implemente desde un repositorio de Git en el equipo local. Nombre de la aplicación web debe ser único globalmente y puede contener superior y letras minúsculas, números y guiones.

   ```azurecli-interactive
   az webapp create -n <your web app name> -g <your resource group name> -p <your app service plan name> --deployment-local-git
   ```

3. Ahora puede agregar configuración de la aplicación para las variables de entorno que especifica la cadena de conexión de IoT hub y el grupo de consumidores del centro de eventos. Opciones de configuración individuales son delimitado por espacios en el `-settings` parámetro. Utilice la cadena de conexión de servicio del centro de IoT y el grupo de consumidores que creó anteriormente en este tutorial. No los valores entre comillas.

   ```azurecli-interactive
   az webapp config appsettings set -n <your web app name> -g <your resource group name> --settings EventHubConsumerGroup=<your consumer group> IotHubConnectionString=<your IoT hub connection string>
   ```

4. Habilitar los Sockets Web de protocolo para la aplicación web y establecer que la aplicación web para recibir HTTPS solo solicita (redirige las solicitudes HTTP a HTTPS).

   ```azurecli-interactive
   az webapp config set -n <your web app name> -g <your resource group name> --web-sockets-enabled true
   az webapp update -n <your web app name> -g <your resource group name> --https-only true
   ```

5. Para implementar el código en App Service, deberá usar su [credenciales de implementación de nivel de usuario](https://docs.microsoft.com/azure/app-service/deploy-configure-credentials). Las credenciales de implementación de nivel de usuario son diferentes de las credenciales de Azure y se usan para las implementaciones de FTP en una aplicación web y Git local. Una vez establecido, son válidos en todas sus aplicaciones de App Service en todas las suscripciones de su cuenta de Azure. Si previamente ha configurado las credenciales de implementación de nivel de usuario, puede usarlos.

   Si anteriormente no ha establecido las credenciales de implementación de nivel de usuario o si no recuerda su contraseña, ejecute el siguiente comando. El nombre de usuario de implementación debe ser único dentro de Azure, y no debe contener el ' @' para inserciones de Git local de símbolos. Cuando se le pida, escriba y confirme la nueva contraseña. La contraseña debe tener al menos ocho caracteres y dos de los tres elementos siguientes: letras, números y símbolos.

   ```azurecli-interactive
   az webapp deployment user set --user-name <your deployment user name>
   ```

6. Obtenga la dirección URL de Git a usar para insertar el código hasta la aplicación de servicio.

   ```azurecli-interactive
   az webapp deployment source config-local-git -n <your web app name> -g <your resource group name>
   ```

7. Agregue un control remoto para el clon que hace referencia el repositorio de Git para la aplicación web en App Service. Para \<dirección URL de clonación de Git\>, use la dirección URL devuelta en el paso anterior. Ejecute el siguiente comando en la ventana de comandos.

   ```cmd
   git remote add webapp <Git clone URL>
   ```

8. Para implementar el código en App Service, escriba el siguiente comando en la ventana de comandos. Si le piden credenciales, escriba las credenciales de implementación de nivel de usuario que creó en el paso 5. Asegúrese de que inserte en la rama principal del control remoto de App Service.

    ```cmd
    git push webapp master:master
    ```

9. El progreso de la implementación se actualizará en la ventana de comandos. Una implementación correcta finalizará con líneas similares a lo siguiente:

    ```cmd
    remote:
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://contoso-web-app-3.scm.azurewebsites.net/contoso-web-app-3.git
    6b132dd..7cbc994  master -> master
    ```

10. Ejecute el comando siguiente para consultar el estado de la aplicación web y asegúrese de que se está ejecutando:

    ```azurecli-interactive
    az webapp show -n <your web app name> -g <your resource group name> --query state
    ```

11. Vaya a `https://<your web app name>.azurewebsites.net` en un explorador. Una página web similar a la que vio cuando ejecutó la aplicación web localmente muestra. Suponiendo que el dispositivo está ejecutando y envío de datos, debería ver un gráfico de ejecución de las 50 lecturas más reciente de temperatura y humedad enviados por el dispositivo.

## <a name="troubleshooting"></a>solución de problemas

Si encuentra algún problema con este ejemplo, pruebe los pasos descritos en las secciones siguientes. Si sigue teniendo problemas, envíenos comentarios en la parte inferior de este tema.

### <a name="client-issues"></a>Problemas del cliente

* Si un dispositivo no aparece en la lista, o ningún gráfico que se va a dibujar, asegúrese de que el código de dispositivo se está ejecutando en el dispositivo.

* En el explorador, abra las herramientas de desarrollo (en muchos exploradores presionar F12 clave abrirá) y busque la consola. Busque las advertencias o errores se imprime.

* Puede depurar el script de cliente en /js/chat-device-data.js.

### <a name="local-website-issues"></a>Problemas del sitio Web local

* Ver la salida en la ventana donde se inició el nodo de salida de la consola.

* Depurar el código de servidor, en concreto, server.js y /scripts/event-hub-reader.js.

### <a name="azure-app-service-issues"></a>Problemas de Azure App Service

* En Azure portal, vaya a la aplicación web. En **supervisión** en el panel izquierdo, seleccione **registros de App Service**. Activar **(sistema de archivos) de registro de la aplicación** , establecer **nivel** Error y, a continuación, seleccione **guardar**. A continuación, abra **secuencia de registro** (bajo **supervisión**).

* Desde la aplicación web en Azure portal, en **herramientas de desarrollo** seleccione **consola** y validar las versiones de node y npm con `node -v` y `npm -v`.

* Si ve un error de no encontrar un paquete, puede haber ejecutado los pasos desordenados. Cuando se implementa el sitio (con `git push`) se ejecuta el servicio de aplicación `npm install`, que se ejecuta según la versión actual del nodo que ha configurado. Si la que se cambia en la configuración más adelante, deberá realizar un cambio no tiene sentido en el código y vuelva a insertar.

## <a name="next-steps"></a>Pasos siguientes

Ha usado correctamente una aplicación web para visualizar datos del sensor en tiempo real desde su IoT Hub.

Para que conocer otra manera de visualizar datos desde Azure IoT Hub, consulte [Use Power BI para visualizar datos de sensor en tiempo real desde IoT hub](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
