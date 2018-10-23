---
title: Análisis de eventos desde la configuración de Azure Digital Twins | Microsoft Docs
description: Siga los pasos de este tutorial para aprender a visualizar y analizar eventos de los espacios de Azure Digital Twins con Time Series Insights.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1366cafe5d2c526e86905c108b9c7b865aac8f69
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323467"
---
# <a name="tutorial-visualize-and-analyze-events-from-your-azure-digital-twins-spaces-using-time-series-insights"></a>Tutorial: Visualización y análisis de eventos de los espacios de Azure Digital Twins mediante Time Series Insights

Una vez que ha implementado una instancia de Azure Digital Twins, aprovisionado los espacios e implementado una función personalizada para supervisar las condiciones concretas, puede visualizar los eventos y datos procedentes de los espacios para buscar tendencias y anomalías. 

En [el primer tutorial](tutorial-facilities-setup.md), configuró el grafo espacial de un edificio imaginario con una sala con sensores de movimiento, temperatura y dióxido de carbono. En [el segundo tutorial](tutorial-facilities-udf.md), aprovisionó el grafo y una función definida por el usuario. La función supervisa estos valores de los sensores y desencadena notificaciones cuando las condiciones son adecuadas, es decir, la sala está vacía y los niveles de temperatura y dióxido de carbono son normales. Este tutorial muestra cómo puede integrar las notificaciones y los datos procedentes de la configuración de Digital Twins en Azure Time Series Insights. Luego puede visualizar los valores de los sensores con el paso del tiempo y buscar tendencias como la sala que más se usa, la hora del día en que más se usan las salas, etc. También puede detectar anomalías como qué salas están más cargadas y calientes o, si un área del edificio está enviando constantemente valores de temperatura alta que indican que el aire acondicionado está defectuoso.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Streaming de datos mediante Event Hubs
> * Análisis con Time Series Insights

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se supone que ha [configurado](tutorial-facilities-setup.md) y [aprovisionado](tutorial-facilities-udf.md) Azure Digital Twins. Antes de continuar, asegúrese de que:
- Tiene una [cuenta de Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Hay una instancia de Digital Twins en ejecución.
- Los [ejemplos en C# de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) se han descargado y extraído en su equipo de trabajo.
- Tiene la [versión 2.1.403 de SDK de .NET Core, o cualquier versión superior](https://www.microsoft.com/net/download) en el equipo de desarrollo para ejecutar el ejemplo. Ejecute `dotnet --version` para comprobar si está instalada la versión correcta. 


## <a name="stream-data-using-event-hubs"></a>Streaming de datos mediante Event Hubs
El servicio [Event Hubs](../event-hubs/event-hubs-about.md) le permite crear una canalización para transmitir sus datos. En esta sección se muestra cómo crear un centro de eventos como conector entre Digital Twins y las instancias de TSI.

### <a name="create-an-event-hub"></a>Crear un centro de eventos

1. Inicie sesión en el [portal de Azure](https://portal.azure.com).

1. En el panel de navegación izquierdo, haga clic en **Crear un recurso**. 

1. Busque **Event Hubs** y selecciónelo. Haga clic en **Crear**.

1. Escriba el **nombre** del espacio de nombres de Event Hubs, elija *Estándar* **Plan de tarifa**, su **suscripción**, el **grupo de recursos** que usó para la instancia de Digital Twins y la **ubicación**. Haga clic en **Crear**. 

1. Una vez implementado, vaya a la *implementación* del espacio de nombres de Event Hubs y haga clic en el espacio de nombres que hay debajo de **RECURSOS**.

    ![Espacio de nombres del Centro de eventos](./media/tutorial-facilities-analyze/open-event-hub-ns.png)


1. En el panel **Información general** del espacio de nombres de Event Hubs, haga clic en el botón **Centro de eventos** de la parte superior. 
    ![Event Hubs](./media/tutorial-facilities-analyze/create-event-hub.png)

1. Escriba el **nombre** del centro de eventos y haga clic en **Crear**. Una vez implementado, aparecerá en el panel **Event Hubs** del espacio de nombres de Event Hubs y el valor de *ESTADO* será **Activo**. Haga clic en este centro de eventos para abrir su panel **Información general**.

1. Haga clic en el botón **Grupo de consumidores** de la parte superior y escriba un nombre como *tsievents* para el grupo de consumidores. Haga clic en **Crear**.
    ![Grupo de consumidores de Event Hubs](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)

   Una vez creado, el grupo de consumidores aparecerá en la lista de la parte inferior del panel **Información general** del centro de eventos. 

1. Abra el panel **Directivas de acceso compartido** del centro de eventos y haga clic en el botón **Agregar**. **Cree** una directiva denominada *ManageSend*y asegúrese de que todas las casillas están seleccionadas. 

    ![Cadenas de conexión de Event Hub](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)

1. Abra la directiva *ManageSend* que ha creado y copie los valores de **Cadena de conexión: clave principal** y **Cadena de conexión: clave secundaria** en un archivo temporal. Necesitará estos valores para crear un punto de conexión para el centro de eventos en la sección siguiente.

### <a name="create-endpoint-for-the-event-hub"></a>Creación de un punto de conexión para el centro de eventos

1. En la ventana de comandos, asegúrese de que se encuentra en la carpeta **_occupancy-quickstar\src** del ejemplo de Digital Twins.

1. Abra el archivo **_actions\createEndpoints.yaml_** en un editor. Reemplace el contenido por el siguiente:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    ```

1. Reemplace los marcadores de posición `Primary_connection_string_for_your_event_hub` por el valor de **Cadena de conexión: clave principal** del centro de eventos. Asegúrese de que el formato de esta cadena de conexión es como este:
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
```

1. Reemplace los marcadores de posición `Secondary_connection_string_for_your_event_hub` por el valor de **Cadena de conexión: clave secundaria** del centro de eventos. Asegúrese de que el formato de esta cadena de conexión es como este: 
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
```

1. Reemplace los marcadores de posición `Name_of_your_Event_Hubs_namespace` por el nombre del espacio de nombres de Event Hubs.

    > [!IMPORTANT]
    > Escriba todos los valores sin comillas. Asegúrese de que hay al menos un carácter de espacio después de los signos de dos puntos en el archivo *YAML*. El contenido del archivo *YAML* también se puede validar mediante cualquier control de servidor de validación de YAML en línea como [esta herramienta](https://onlineyamltools.com/validate-yaml).


1. Guarde y cierre el archivo. Ejecute el comando siguiente en la ventana de comandos, e inicie sesión con su cuenta de Azure cuando se le solicite.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```
   
   Crea dos puntos de conexión para el centro de eventos.

   ![Puntos de conexión de Event Hubs](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)

## <a name="analyze-with-time-series-insights"></a>Análisis con Time Series Insights

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso**. 

1. Busque un nuevo recurso de **Time Series Insights** y selecciónelo. Haga clic en **Crear**.

1. Escriba el **nombre** de la instancia de Time Series Insights seleccione su **suscripción**. Seleccione el **grupo de recursos** que usó o para su instancia de Digital Twins y su **ubicación**. Haga clic en **Crear**.

    ![Crear TSI](./media/tutorial-facilities-analyze/create-tsi.png)

1. Una vez implementado, abra el entorno de Time Series Insights y abra su panel **Orígenes de eventos**. Haga clic en el botón **Agregar** de la parte superior para agregar un grupo de consumidores.

1. En el panel **Nuevo origen de eventos**, escriba un **nombre**y asegúrese de que los demás valores se seleccionan correctamente. Seleccione *ManageSend* en **Nombre de directiva de centro de eventos** y, después, seleccione el *grupo de consumidores* que creó en la sección anterior en **Grupo de consumidores del centro de eventos**. Haga clic en **Crear**.

    ![Origen del evento de TSI](./media/tutorial-facilities-analyze/tsi-event-source.png)

1. Abra el panel **Información general** del entorno de Time Series Insights y haga clic en el botón **Ir al entorno** de la parte superior. Si aparece una *advertencia por el acceso a los datos*, abra el panel **Directivas de acceso de datos** de la instancia de TSI, haga clic en **Agregar**, seleccione **Colaborador** como rol y seleccione el usuario apropiado.

1. El botón **Ir al entorno** abre el [explorador de Time Series Insights](../time-series-insights/time-series-insights-explorer.md). Si no muestra ningún evento, simule eventos del dispositivo, para lo que debe ir al proyecto **_device-connectivity_** del ejemplo de Digital Twins y ejecutar `dotnet run`.

1. Cuando se hayan generado algunos eventos simulados, vuelva al explorador de Time Series Insights y haga clic en el botón Actualizar de la parte superior. Debería ver que se crean gráficos analíticos para los datos de los sensores simulados. 

    ![Explorador de TSI](./media/tutorial-facilities-analyze/tsi-explorer.png)

1. Luego, en el explorador de Time Series, puede generar gráficos y mapas térmicos de los diferentes eventos y datos de las salas, sensores y otros recursos. En el lado izquierdo, haga clic en los cuadros de lista desplegable **MEDIDA** y **DIVIDIR POR** para crear sus propias visualizaciones. Por ejemplo, seleccione *Eventos* en **MEDIDA** y *DigitalTwins SensorHardwareId* en **DIVIDIR POR** para generar un mapa térmico para cada uno de los sensores, similar al de la siguiente imagen:

    ![Explorador de TSI](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea dejar de explorar Azure Digital Twins aquí, elimine los recursos creados en este tutorial:

1. En el menú izquierdo de [Azure Portal](http://portal.azure.com), haga clic en **Todos los recursos**, seleccione el grupo de recurso de Digital Twins y haga clic en **Eliminar**.
2. Si lo necesita, también puede eliminar las aplicaciones de ejemplo de su equipo de trabajo. 


## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para obtener más información acerca de los grafos de inteligencia espacial y los modelos de objetos en Azure Digital Twins. 
> [!div class="nextstepaction"]
> [Modelos de objetos de Digital Twins y grafo de inteligencia espacial](concepts-objectmodel-spatialgraph.md)

