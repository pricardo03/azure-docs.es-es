---
title: 'Tutorial: Captura de eventos de un espacio de Azure Digital Twins | Microsoft Docs'
description: Si sigue los pasos de este tutorial, aprenderá a recibir notificaciones de los espacios de mediante la integración de Azure Digital Twins con Logic Apps.
services: digital-twins
author: alinamstanciu
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 12/18/2018
ms.author: alinast
ms.openlocfilehash: 2b84fa2fd8053ca4dc7ef0ad246d29b2bba3dae5
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484712"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-by-using-logic-apps"></a>Tutorial: Recepción de notificaciones de los espacios de Azure Digital Twins mediante Logic Apps

Después de que haya implementado su instancia de Azure Digital Twins, aprovisionado los espacios e implementado las funciones personalizadas para supervisar condiciones específicas, puede enviar una notificación a la administración de las oficinas a través de correo electrónico cuando se produzcan las condiciones supervisadas.

En [el primer tutorial](tutorial-facilities-setup.md), ha configurado el grafo espacial de un edificio imaginario. Una sala en el edificio contiene sensores de movimiento, dióxido de carbono y temperatura. En [el segundo tutorial](tutorial-facilities-udf.md), aprovisionó el grafo y una función definida por el usuario para supervisar los valores de los sensores y desencadenar notificaciones cuando la sala esté vacía y tanto los valores de temperatura y dióxido de carbono se encuentren en un intervalo aceptable. 

Este tutorial muestra cómo se pueden integrar estas notificaciones con Azure Logic Apps para enviar mensajes de correo electrónico cuando una sala esté disponible. Cualquier administrador de las oficinas puede usar esta información para ayudar a los empleados a reservar la sala de reuniones más productiva.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Integrar eventos con Azure Event Grid.
> * Notificar eventos con Logic Apps.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se supone que ha [configurado](tutorial-facilities-setup.md) y [aprovisionado](tutorial-facilities-udf.md) Azure Digital Twins. Antes de continuar, asegúrese de que:

- Una [cuenta de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Hay una instancia de Digital Twins en ejecución.
- Los [ejemplos en C# de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) se han descargado y extraído en su equipo de trabajo.
- [Versión 2.1.403 del SDK de .NET Core, o cualquier versión superior](https://www.microsoft.com/net/download) en la máquina de desarrollo para ejecutar el ejemplo. Ejecute `dotnet --version` para comprobar que está instalada la versión correcta.
- Una cuenta de Office 365 para enviar notificaciones por correo electrónico.

## <a name="integrate-events-with-event-grid"></a>Integración de eventos con Event Grid

En esta sección, va a configurar [Event Grid](../event-grid/overview.md) para recopilar eventos de una instancia de Azure Digital Twins y redirigirlos a un [controlador de eventos](../event-grid/event-handlers.md) como Logic Apps.

### <a name="create-an-event-grid-topic"></a>Creación de un tema de Event Grid

Un [tema de Event Grid](../event-grid/concepts.md#topics) proporciona una interfaz para enrutar los eventos que genera la función definida por el usuario. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. En el panel izquierdo, seleccione **Crear un recurso**. 

1. Busque y seleccione **Event Grid Topic**. Seleccione **Crear**.

1. Escriba el **nombre** del tema de Event Grid y elija la **suscripción**. Seleccione el **grupo de recursos** que usó o creó para su instancia de Digital Twins y la **ubicación**. Seleccione **Crear**. 

    ![Creación de un tema de Event Grid](./media/tutorial-facilities-events/create-event-grid-topic.png)

1. Vaya al tema de Event Grid desde el grupo de recursos, seleccione **Información general** y copie el valor de **Punto de conexión del tema** en un archivo temporal. Necesitará esta dirección URL en la siguiente sección. 

1. Seleccione **Claves de acceso** y copie **YOUR_KEY_1** y **YOUR_KEY_2** en un archivo temporal. Necesitará estos valores para crear el punto de conexión en la sección siguiente.

    ![Claves de Event Grid](./media/tutorial-facilities-events/event-grid-keys.png)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Creación de un punto de conexión para el tema de Event Grid

1. En la ventana de comandos, asegúrese de que se encuentra en la carpeta **occupancy-quickstart\src** del ejemplo de Digital Twins.

1. Abra el archivo **actions\createEndpoints.yaml** en el editor de Visual Studio Code. Asegúrese de que tiene el siguiente contenido:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: <Primary connection string for your Event Grid>
      secondaryConnectionString: <Secondary connection string for your Event Grid>
      path: <Event Grid Topic Name without https:// and /api/events, e.g. eventgridname.region.eventgrid.azure.net>
    ```

1. Reemplace el marcador de posición `<Primary connection string for your Event Grid>` por el valor de **YOUR_KEY_1**.

1. Reemplace el marcador de posición `<Secondary connection string for your Event Grid>` por el valor de **YOUR_KEY_2**.

1. Reemplace el marcador de posición para **path** por la ruta de acceso del tema de Event Grid. Para obtener dicha ruta de acceso, elimine **https://** y las rutas de los recursos finales de la dirección URL del **Punto de conexión del tema**. El formato debería ser similar a este: *NombreDeSuEvent.SuUbicacion.eventgrid.azure.net*.

    > [!IMPORTANT]
    > Escriba todos los valores sin comillas. Asegúrese de que hay al menos un carácter de espacio después de los signos de dos puntos en el archivo YAML. También puede validar el contenido del archivo YAML mediante cualquier control de servidor de validación de YAML en línea, como [esta herramienta](https://onlineyamltools.com/validate-yaml).

1. Guarde y cierre el archivo. En la ventana de comandos, ejecute el comando siguiente e inicie sesión cuando se le solicite. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Este comando crea el punto de conexión de Event Grid. 

   ![Puntos de conexión de Event Grid](./media/tutorial-facilities-events/dotnet-create-endpoints.png)

## <a name="notify-events-with-logic-apps"></a>Notificación de eventos con Logic Apps

Puede usar el servicio [Azure Logic Apps](../logic-apps/logic-apps-overview.md) para crear tareas automatizadas para los eventos recibidos de otros servicios. En esta sección, va a configurar Logic Apps para crear notificaciones por correo electrónico para eventos enrutados desde los sensores espaciales, con la ayuda de un [tema de Event Grid](../event-grid/overview.md).

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso**.

1. Busque un recurso de **Logic App** nuevo y selecciónelo. Seleccione **Crear**.

1. Escriba un **Nombre** para el recurso de la aplicación lógica y seleccione su **Suscripción**, **Grupo de recursos** y **Ubicación**. Seleccione **Crear**.

    ![Creación de un recurso de Logic Apps](./media/tutorial-facilities-events/create-logic-app.png)

1. Abra el recurso de Logic Apps cuando esté implementado y, después, abra el panel **Diseñador de aplicación lógica**. 

1. Seleccione el desencadenador **Cuando se produce un evento de recursos de Event Grid**. Inicie sesión en su inquilino con su cuenta de Azure cuando se le solicite. Seleccione **Permitir acceso** para el recurso de Event Grid si se le solicita. Seleccione **Continuar**.

1. En la ventana **When a resource event occurs (Preview)** (Cuando se produzca un evento de recursos [versión preliminar]): 
   
   a. Seleccione la **suscripción** que usó para crear el tema de Event Grid.

   b. Seleccione **Microsoft.EventGrid.Topics** en **Tipo de recurso**.

   c. Seleccione el recurso de Event Grid en el cuadro de lista desplegable de **Nombre de recurso**.

   ![Panel Diseñador de aplicación lógica](./media/tutorial-facilities-events/logic-app-resource-event.png)

1. Seleccione el botón **Nuevo paso**.

1. En la ventana **Elegir una acción**:

   a. Busque la frase **parse json**y seleccione la acción **Analizar JSON**.

   b. Haga clic en el campo **Contenido** y, después, seleccione **Cuerpo** en la lista **Contenido dinámico**.

   c. Seleccione **Usar una carga de ejemplo para generar el esquema**. Pegue la siguiente carga de JSON y seleccione **Listo**.

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

    Esta carga tiene valores ficticios. Logic Apps usa esta carga de ejemplo para generar un *esquema*.

    ![Ventana de análisis de JSON de Logic Apps para Event Grid](./media/tutorial-facilities-events/logic-app-parse-json.png)

1. Seleccione el botón **Nuevo paso**.

1. En la ventana **Elegir una acción**:

   a. Seleccione **Control > Condición** o busque **Condición** en la lista **Acciones**. 

   b. En el primer cuadro de texto **Elegir un valor**, seleccione **eventType** en la lista **Contenido dinámico** para la ventana **Análisis del archivo JSON**.

   c. En el segundo cuadro de texto **Elegir un valor**, escriba `UdfCustom`.

   ![Condiciones seleccionadas](./media/tutorial-facilities-events/logic-app-condition.png)

1. En la ventana **If true**:

   a. Seleccione **Add an action** (Agregar una acción) y luego **Office 365 Outlook**.

   b. En la lista **Acciones**, seleccione **Send an email** (Enviar un correo electrónico). Seleccione **Iniciar sesión** y use las credenciales de su cuenta de correo electrónico. Seleccione **Permitir acceso** si se le solicita.

   c. En el cuadro **Para**, escriba su identificador de correo electrónico para recibir notificaciones. En **Asunto**, escriba el texto **Notificación de Digital Twins sobre mala calidad del aire en un espacio**. A continuación, seleccione **TopologyObjectId** desde la lista **Contenido dinámico** para **Análisis del archivo JSON**.

   d. En **Cuerpo**, en la misma ventana, escriba un texto similar al siguiente: **Se ha detectado que la calidad del aire en una sala es deficiente y la temperatura debe ajustarse**. Si lo cree necesario utilice elementos de la lista **Contenido dinámico**.

   ![Selecciones para el envío de un correo electrónico de Logic Apps](./media/tutorial-facilities-events/logic-app-send-email.png)

1. Seleccione el botón **Guardar** en la parte superior del panel **Diseñador de aplicación lógica**.

1. Asegúrese de simular los datos de los sensores, para lo que debe ir a la carpeta **device-connectivity** del ejemplo de Digital Twin en una ventana de comandos y ejecutar `dotnet run`.

En pocos minutos, debería empezar a recibir notificaciones por correo electrónico de Logic Apps. 

   ![Notificación por correo electrónico](./media/tutorial-facilities-events/logic-app-notification.png)

Para dejar de recibir estos correos electrónicos, vaya al recurso de Logic Apps en el portal y seleccione el panel **Información general**. Seleccione **Deshabilitar**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere dejar de explorar Azure Digital Twins en este punto, elimine los recursos creados en este tutorial:

1. En el menú izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Todos los recursos**, seleccione el grupo de recurso de Digital Twins y haga clic en **Eliminar**.

    > [!TIP]
    > Si tiene problemas al eliminar una instancia de Digital Twins, se ha incorporado una actualización del servicio con la corrección. Vuelva a intentar eliminar la instancia.

2. Si es necesario, elimine las aplicaciones de ejemplo en la máquina de trabajo.

## <a name="next-steps"></a>Pasos siguientes

Para aprender a visualizar los datos de los sensores, analizar tendencias y detectar anomalías, acuda al siguiente tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Visualización y análisis de eventos de los espacios de Azure Digital Twins mediante Time Series Insights](tutorial-facilities-analyze.md)

También puede aprender más acerca de los grafos de inteligencia espacial y los modelos de objetos en Azure Digital Twins:

> [!div class="nextstepaction"]
> [Modelos de objetos de Digital Twins y grafo de inteligencia espacial](concepts-objectmodel-spatialgraph.md)
