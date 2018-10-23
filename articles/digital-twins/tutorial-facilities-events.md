---
title: Captura de eventos de un espacio de Azure Digital Twins | Microsoft Docs
description: Si sigue los pasos de este tutorial, aprenderá a recibir notificaciones de los espacios de mediante la integración de Azure Digital Twins con Logic Apps.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 91dd16938efbd1e24419352f66e3238646a77e8a
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323435"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-using-logic-apps"></a>Tutorial: Recepción de notificaciones de los espacios de Azure Digital Twins mediante Logic Apps

Una vez que haya implementado su instancia de Azure Digital Twins, aprovisionado los espacios e implementado las funciones personalizadas para supervisar condiciones específicas, puede enviar una notificación a la administración de las oficinas a través de correo electrónico cuando se produzcan las condiciones supervisadas. 

En [el primer tutorial](tutorial-facilities-setup.md), configuró el grafo espacial de un edificio imaginario con una sala con sensores de movimiento, temperatura y dióxido de carbono. En [el segundo tutorial](tutorial-facilities-udf.md), aprovisionó el grafo y una función personalizada llamada la función definida por el usuario para supervisar los valores de los sensores y desencadenar notificaciones cuando la sala esté vacía y tanto los valores de temperatura y dióxido de carbono se encuentren en un intervalo aceptable. Este tutorial muestra cómo se pueden integrar estas notificaciones con Azure Logic Apps para enviar mensajes de correo electrónico cuando una sala esté disponible. Cualquier administrador de las oficinas puede usar esta información para ayudar a los empleados a reservar la sala de reuniones más productiva. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Integrar eventos con Event Grid
> * Notificar eventos con Logic Apps
    
## <a name="prerequisites"></a>Requisitos previos

En este tutorial se supone que ha [configurado](tutorial-facilities-setup.md) y [aprovisionado](tutorial-facilities-udf.md) Azure Digital Twins. Antes de continuar, asegúrese de que:
- Tiene una [cuenta de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Hay una instancia de Digital Twins en ejecución.
- Los [ejemplos en C# de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) se han descargado y extraído en su equipo de trabajo.
- Tiene la [versión 2.1.403 de SDK de .NET Core, o cualquier versión superior](https://www.microsoft.com/net/download) en el equipo de desarrollo para ejecutar el ejemplo. Ejecute `dotnet --version` para comprobar si está instalada la versión correcta. 
- Una cuenta de Office 365 para enviar notificaciones por correo electrónico.

## <a name="integrate-events-with-event-grid"></a>Integración de eventos con Event Grid 
En esta sección, configurará [Event Grid](../event-grid/overview.md) para recopilar eventos de una instancia de Digital Twins y redirigirlos a un [controlador de eventos](../event-grid/event-handlers.md) como Logic Apps.

### <a name="create-event-grid-topic"></a>Creación de un tema de Event Grid
[Temas de Event Grid](../event-grid/concepts.md#topics) proporciona una interfaz para enrutar los eventos que genera la función definida por el usuario. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En el panel de navegación izquierdo, haga clic en **Crear un recurso**. 

1. Busque y seleccione **Event Grid Topic**. Haga clic en **Crear**.

1. Escriba el **nombre** del tema de Event Grid y elija la **suscripción**. Seleccione el **grupo de recursos** que usó o creó para su instancia de Digital Twins y la **ubicación**. Haga clic en **Crear**. 

    ![Creación de un tema de Event Grid](./media/tutorial-facilities-events/create-event-grid-topic.png)

1. Vaya al tema de Event Grid desde su grupo de recursos, haga clic en **Información general**y copie el valor de **Punto de conexión del tema** en un archivo temporal. Esta dirección URL la necesitará en la sección siguiente. 

1. Haga clic en **Claves de acceso**y copie **Clave 1** y **Clave 2** en un archivo temporal. Necesitará estos valores para crear el punto de conexión en la sección siguiente.

    ![Claves de Event Grid](./media/tutorial-facilities-events/event-grid-keys.png)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Creación de un punto de conexión para el tema de Event Grid

1. En la ventana de comandos, asegúrese de que se encuentra en la carpeta **_occupancy-quickstart\src_** del ejemplo de Digital Twins.

1. Abra el archivo **_actions\createEndpoints.yaml_** en el editor de Visual Studio Code. Asegúrese de que tiene el siguiente contenido:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_Event_Grid
      secondaryConnectionString: Secondary_connection_string_for_your_Event_Grid
      path: Event_Grid_Topic_Path
    ```

1. Reemplace el marcador de posición `Primary_connection_string_for_your_Event_Grid` por el valor de **Clave 1**. 

1. Reemplace el marcador de posición `Secondary_connection_string_for_your_Event_Grid` por el valor de **Clave 2**.

1. Reemplace el marcador de posición `Event_Grid_Topic_Path` por la ruta de acceso del tema de Event Grid. Para obtener dicha ruta de acceso, quite *https://* y las rutas de los recursos finales de la dirección URL de **Punto de conexión del tema**. El formato debería ser similar a este: **NombreDeSuEvent.SuUbicacion.eventgrid.azure.net**. 

    > [!IMPORTANT]
    > Escriba todos los valores sin comillas. Asegúrese de que hay al menos un carácter de espacio después de los signos de dos puntos en el archivo *YAML*. El contenido del archivo *YAML* también se puede validar mediante cualquier control de servidor de validación de YAML en línea como [esta herramienta](https://onlineyamltools.com/validate-yaml).

1. Guarde y cierre el archivo. En la ventana de comandos, ejecute el comando siguiente e inicie sesión cuando se le solicite. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Este comando crea el punto de conexión de Event Grid. 

   ![Puntos de conexión de Event Grid](./media/tutorial-facilities-events/dotnet-create-endpoints.png)


## <a name="notify-events-with-logic-app"></a>Notificación de eventos con Logic Apps
El servicio [Azure Logic Apps](../logic-apps/logic-apps-overview.md) permite crear tareas automatizadas para los eventos recibidos de otros servicios. En esta sección, configurará Logic Apps para crear notificaciones por correo electrónico para eventos enrutados desde los sensores espaciales, con la ayuda de un [tema de Event Grid](../event-grid/overview.md).

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso**.

1. Busque un recurso de **Logic App** nuevo y selecciónelo. Haga clic en **Crear**.

1. Escriba el **nombre** de la aplicación lógica y seleccione su **suscripción**, **grupo de recursos** y **ubicación**. Haga clic en **Crear**.

    ![Creación de la aplicación lógica](./media/tutorial-facilities-events/create-logic-app.png)

1. Abra la aplicación lógica cuando está implementada y, después, abra el panel **Diseñador de aplicación lógica**. 

1. Seleccione el desencadenador **Cuando se produzca un evento de Event Grid**. **Inicie sesión** en su inquilino con su cuenta de Azure cuando se le solicite. Confirme que **permite el acceso** a Event Grid cuando se le solicite. Haga clic en **Continue**.

1. En la ventana **Cuando se produzca un evento de recursos (versión preliminar)**, 
    1. Seleccione la **suscripción** que usó para crear Event Grid anteriormente,

    1. Seleccione **Microsoft.EventGrid.Topics** en **Tipo de recurso**,

    1. Seleccione el recurso de Event Grid en el cuadro de lista desplegable de **Nombre de recurso**.

    ![Creación de la aplicación lógica](./media/tutorial-facilities-events/logic-app-resource-event.png)

1. Haga clic en el botón **Nuevo paso**.

1. En la ventana **Elegir una acción**,
    1. Busque la frase *parse json*y seleccione la acción **Analizar JSON**.

    1. Haga clic en el campo **Contenido** y, después, seleccione **Cuerpo** en la lista **Contenido dinámico**.

    1. Seleccione **Use sample to payload to generate schema** (Usar carga de ejemplo para generar el esquema). Pegue la siguiente carga de JSON y haga clic en **Listo**.

        ```JSON
        {
        "id": "32162f00-a8f1-4d37-aee2-9312aabba0fd",
        "subject": "UdfCustom",
        "data": {
          "TopologyObjectId": "20efd3a8-34cb-4d96-a502-e02bffdabb14",
          "ResourceType": "Space",
          "Payload": "\"Air quality is poor.\"",
          "CorrelationId": "32162f00-a8f1-4d37-aee2-9312aabba0fd"
        },
        "eventType": "UdfCustom",
        "eventTime": "0001-01-01T00:00:00Z",
        "dataVersion": "1.0",
        "metadataVersion": "1",
        "topic": "/subscriptions/a382ee71-b48e-4382-b6be-eec7540cf271/resourceGroups/HOL/providers/Microsoft.EventGrid/topics/DigitalTwinEventGrid"
        }
        ```
    
    Esta carga tiene valores ficticios. Logic App usa esta carga de ejemplo para generar un **esquema**.
    
    ![Analizar JSON de Logic App para Event Grid](./media/tutorial-facilities-events/logic-app-parse-json.png)

1. Haga clic en el botón **Nuevo paso**.

1. En la ventana **Elegir una acción**,
    1. Busque y seleccione **Control de condiciones** en la lista **Acciones**. 

    1. Haga clic en el primer cuadro de texto **Elegir un valor** y seleccione **eventType** en la lista **Contenido dinámico** para la ventana **Analizar JSON**.

    1. Haga clic en el segundo cuadro de texto **Elegir un valor** y escriba *UdfCustom*.

    ![Analizar JSON de Logic App para Event Grid](./media/tutorial-facilities-events/logic-app-condition.png)

1. En la ventana **If true**,
    1. Haga clic en **Agregar una acción**y seleccione *Office 365 Outlook*.

    1. En la lista **Acciones**, seleccione **Enviar un correo electrónico**. Haga clic en **Iniciar sesión** y use las credenciales de su cuenta de correo electrónico. Haga clic en **Permitir acceso** cuando se le solicite.

    1. En el cuadro **Para**, escriba su identificador de correo electrónico para recibir notificaciones. En **Asunto**, escriba el texto *Digital Twins notification for poor air quality in space* (Notificaciones de Digital Twins por aire de baja calidad en el espacio) y seleccione **TopologyObjectId** en la lista **Contenido dinámico** de **Analizar JSON**.

    1. En el campo **Cuerpo** de la misma ventana, escriba un texto similar al siguiente: *Poor air quality detected in a room, and temperature needs to be adjusted* (Se ha detectado aire de baja calidad en la sala y la temperatura se debe ajustar). No dude en crearlo mediante elementos de la lista **Contenido dinámico** como se muestra a continuación.

    ![Enviar un correo electrónico de Logic App](./media/tutorial-facilities-events/logic-app-send-email.png)

1. Haga clic en **Guardar** en la parte superior del panel **Diseñador de aplicación lógica**.

1. Asegúrese de simular los datos de los sensores, para lo que debe ir a la carpeta **_device-connectivity_** del ejemplo de Digital Twin en una ventana de comandos y ejecutar `dotnet run`.

En pocos minutos, debería empezar a recibir notificaciones por correo electrónico de esta aplicación lógica. 

   ![Enviar un correo electrónico de Logic App](./media/tutorial-facilities-events/logic-app-notification.png)

Para dejar de recibir estos correos electrónicos, vaya a **Logic App** en el portal y seleccione el panel **Información general**. Haga clic en **Deshabilitar**.


## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea dejar de explorar Azure Digital Twins aquí, elimine los recursos creados en este tutorial:

1. En el menú izquierdo de [Azure Portal](http://portal.azure.com), haga clic en **Todos los recursos**, seleccione el grupo de recurso de Digital Twins y haga clic en **Eliminar**.
2. Si lo necesita, también puede eliminar las aplicaciones de ejemplo de su equipo de trabajo. 


## <a name="next-steps"></a>Pasos siguientes

Puede pasar al siguiente tutorial para aprender a visualizar los datos de los sensores, analizar tendencias y detectar anomalías. 
> [!div class="nextstepaction"]
> [Tutorial: Visualización y análisis de eventos de los espacios de Azure Digital Twins mediante Time Series Insights](tutorial-facilities-analyze.md)

También puede continuar para obtener más información acerca de los grafos de inteligencia espacial y los modelos de objetos en Azure Digital Twins. 
> [!div class="nextstepaction"]
> [Modelos de objetos de Digital Twins y grafo de inteligencia espacial](concepts-objectmodel-spatialgraph.md)