---
title: Envío de eventos de Azure Time Series Insights - Envío de eventos al entorno de Azure Time Series Insights | Microsoft Docs
description: Aquí aprenderá a configurar el centro de eventos y a ejecutar una aplicación de ejemplo para insertar eventos que se muestren en Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 69d16292f5b71179ee66fb5f7d6c4a6f11cbb9de
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276152"
---
# <a name="send-events-to-a-time-series-insights-environment-by-using-an-event-hub"></a>Enviar eventos a un entorno de Time Series Insights mediante un centro de eventos

En este artículo se explica cómo crear y configurar un centro de eventos en Azure Event Hubs, y cómo ejecutar una aplicación de ejemplo para insertar eventos. Si tiene un centro de eventos con eventos en formato JSON, pase por alto este tutorial y consulte su entorno en [Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Configuración de un centro de eventos

1. Para saber cómo crear un centro de eventos, consulte la [documentación de Event Hubs](https://docs.microsoft.com/azure/event-hubs/).
1. En el cuadro de búsqueda, busque **Event Hubs**. Haga clic en **Event Hubs** en la lista devuelta.
1. Seleccione su centro de eventos.
1. Cuando se crea un centro de eventos, en realidad está creando un espacio de nombres del centro de eventos. Si todavía no ha creado un centro de eventos en el espacio de nombres, vaya al menú y, en **Entities** (Entidades), cree un centro de eventos.  

    ![Lista de Event Hubs][1]

1. Después de crear un centro de eventos, selecciónelo en la lista de centros de eventos.
1. En el menú, en **Entities** (Entidades), seleccione **Event Hubs**.
1. Seleccione el nombre del centro de eventos para configurarlo.
1. En **Entities** (Entidades), seleccione **Consumer groups** (Grupos de consumidores) y, a continuación, seleccione **Consumer group** (Grupo de consumidores).

    ![Creación de un grupo de consumidores][2]

1. Asegúrese de crear un grupo de consumidores que sea utilizado exclusivamente por el origen de eventos de Time Series Insights.

    > [!IMPORTANT]
    > Asegúrese de que ningún otro servicio usa el grupo de consumidores (como un trabajo de Azure Stream Analytics u otro entorno de Time Series Insights). Si otros servicios usan el grupo de consumidores, las operaciones de lectura se ven afectadas negativamente en este entorno y en los otros servicios. Si usa **$Default** como grupo de consumidores, otros lectores podrían volver a usar el grupo de consumidores.

1. En el menú, en **Settings** (Configuración), seleccione **Shared access policies** (Directivas de acceso compartido) y, a continuación, seleccione **Add** (Agregar).

    ![Seleccione Shared access policies (Directivas de acceso compartido) y, a continuación, pulse el botón Add (Agregar).][3]

1. En el panel **Add new shared access policy** (Agregar nueva directiva de acceso compartido), cree un acceso compartido denominado **MySendPolicy**. Usará esta directiva de acceso compartido para enviar eventos en los ejemplos de C# más adelante en este artículo.

    ![En el cuadro Policy name (Nombre de la directiva), escriba un nombre.][4]

1. En **Claim** (Reclamar), seleccione la casilla **Send** (Enviar).

## <a name="add-a-time-series-insights-instance"></a>Agregar una instancia de Time Series Insights

La actualización de Time Series Insights usa instancias para agregar datos contextuales a los datos de telemetría entrantes. Los datos se unen en el momento de la consulta mediante un **id. de serie temporal**. El **id. de serie temporal** del proyecto de los molinos de viento de muestra que usaremos más adelante en este artículo es **Id**. Para obtener más información sobre las instancias de Time Series Insight y el **id. de serie temporal**, consulte el artículo [Time Series Models](./time-series-insights-update-tsm.md) (Modelos de serie temporal).

### <a name="create-a-time-series-insights-event-source"></a>Crear un origen de eventos de Time Series Insights

1. Complete los pasos para [crear un origen de eventos](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub) si aún no ha creado uno.

1. Establezca el valor de `timeSeriesId`. Para obtener más información sobre el **id. de serie temporal**, consulte [Time Series Models](./time-series-insights-update-tsm.md) (Modelos de serie temporal).

### <a name="push-events"></a>Eventos push (ejemplo de los molinos de viento)

1. Busque **Event Hubs** en la barra de búsqueda. Haga clic en **Event Hubs** en la lista devuelta.

1. Seleccione su centro de eventos.

1. Vaya a **Shared Access Policies (Directivas de acceso compartido)** > **RootManageSharedAccessKey**. Copie el valor de la **clave principal de la cadena de conexión** y guárdelo.

    ![Copie el valor de la cadena de conexión de la clave principal][5]

1. Vaya a https://tsiclientsample.azurewebsites.net/windFarmGen.html. La dirección URL ejecuta dispositivos simulados de molinos de viento.
1. En el cuadro **Cadena de conexión del centro de eventos** de la página web, pegue la cadena de conexión que copió en [Eventos push](#push-events).
  
    ![Pegue la cadena de conexión de la clave principal en el cuadro Cadena de conexión del centro de eventos][6]

1. Seleccione **Click to start** (Haga clic para iniciar). El simulador genera la instancia JSON que puede usar directamente.

1. Vuelva a su centro de eventos en Azure Portal. En la página **Descripción general**, debería ver los nuevos eventos que recibió el centro de eventos:

    ![Una página de información general del centro de eventos que muestra las métricas del centro de eventos][7]

<a id="json"></a>

## <a name="supported-json-shapes"></a>Formas de JSON admitidas

### <a name="sample-1"></a>Ejemplo 1

#### <a name="input"></a>Entrada

Un objeto JSON simple:

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```

#### <a name="output-one-event"></a>Salida: Un evento

|id| timestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Ejemplo 2

#### <a name="input"></a>Entrada

Una matriz JSON con dos objetos JSON. Cada objeto JSON se convierte en un evento.

```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```

#### <a name="output-two-events"></a>Salida: Dos eventos

|id| timestamp|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Ejemplo 3

#### <a name="input"></a>Entrada

Un objeto JSON con una matriz JSON anidada que contiene dos objetos JSON:

```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}
```

#### <a name="output-two-events"></a>Salida: Dos eventos

La propiedad **location** se copia en cada uno de los eventos.

|location|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Ejemplo 4

#### <a name="input"></a>Entrada

Un objeto JSON con una matriz JSON anidada que contiene dos objetos JSON. Esta entrada muestra que las propiedades globales se pueden representar mediante el objeto JSON complejo.

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```

#### <a name="output-two-events"></a>Salida: Dos eventos

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ver el entorno en el explorador de Time Series Insights](https://insights.timeseries.azure.com)

<!-- Images -->
[1]: media/send-events/updated.png
[2]: media/send-events/consumer-group.png
[3]: media/send-events/shared-access-policy.png
[4]: media/send-events/shared-access-policy-2.png
[5]: media/send-events/sample-code-connection-string.png
[6]: media/send-events/updated_two.png
[7]: media/send-events/telemetry.png
