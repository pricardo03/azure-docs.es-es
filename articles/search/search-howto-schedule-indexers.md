---
title: Cómo programar los indizadores - Azure Search
description: Programar los indexadores de Azure Search para indexar el contenido periódicamente o a horas específicas.
ms.date: 05/31/2019
author: RobDixon22
manager: HeidiSteen
ms.author: v-rodixo
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 764fca8d3cb4cd9c40d7880043637f89ef1a8578
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755386"
---
# <a name="how-to-schedule-indexers-for-azure-search"></a>Cómo programar los indizadores para Azure Search
Normalmente, un indizador que se ejecuta una vez, inmediatamente después de crearlo. Puede ejecutar a petición mediante el portal, la API de REST o el SDK de .NET. También puede configurar un indizador se ejecute periódicamente según una programación.

Algunas situaciones donde la programación del indexador resulta útil:

* Origen de datos cambiará con el tiempo, y desea que los indizadores de Azure Search para procesar automáticamente los datos modificados.
* El índice se rellenarán desde varios orígenes de datos y desea asegurarse de que los indizadores se ejecuten en momentos diferentes para reducir los conflictos.
* El origen de datos es muy grande y desea distribuir el indexador con el tiempo de procesamiento. Para obtener más información sobre la indexación de grandes volúmenes de datos, vea [cómo indexar grandes conjuntos de datos en Azure Search](search-howto-large-index.md).

El programador es una característica integrada de Azure Search. Un programador externo que no puede usar para controlar los indizadores de búsqueda.

## <a name="define-schedule-properties"></a>Definir propiedades de programación

Una programación de indizador tiene dos propiedades:
* **Intervalo**, que define la cantidad de tiempo entre programa ejecuciones de indizador. El intervalo más pequeño permitido es de 5 minutos, y es el mayor de 24 horas.
* **Hora (UTC) de inicio**, lo que indica la primera vez en el que se debe ejecutar el indizador.

Puede especificar una programación cuando empiece a crear el indizador, o mediante la actualización de las propiedades del indizador más adelante. Se pueden establecer programaciones de indizador mediante el [portal](#portal), el [API de REST](#restApi), o el [.NET SDK](#dotNetSdk).

Solo se puede ejecutar a la vez una ejecución de un indexador. Si ya se está ejecutando un indizador cuando está programada su ejecución siguiente, dicha ejecución se pospone hasta la próxima hora programada.

Veamos un ejemplo entenderlo concretamente. Supongamos que se configure una programación de indizador con un **intervalo** de cada hora y un **Start Time** del 1 de junio de 2019 a las 8:00:00 A.M. UTC. Aquí es lo que podría ocurrir cuando una ejecución del indexador tarda más de una hora:

* La primera ejecución de indizador se inicia exactamente o alrededor del 1 de junio de 2019 a las 8:00 A.M. UTC. Suponga que esta ejecución tarda 20 minutos (o un período inferior a 1 hora).
* La segunda ejecución se inicia exactamente o alrededor del 1 de junio de 2019 9:00 A.M. UTC. Suponga que esta ejecución tarda 70 minutos - más de una hora, y no se completará hasta 10:10 A.M. UTC.
* La tercera ejecución está programada para empezar a 10:00 A.M. UTC, pero en ese momento todavía se está ejecutando la ejecución anterior. Este programa, a continuación, se ha omitido la ejecución. La siguiente ejecución del indizador no se iniciará hasta 11:00 A.M. UTC.

<a name="portal"></a>

## <a name="define-a-schedule-in-the-portal"></a>Definir una programación en el portal

El Asistente para importar datos en el portal le permite definir la programación de un indizador en tiempo de creación. La configuración de la programación predeterminada es **por hora**, lo que significa que el indizador se ejecuta una vez después de que se crea y se ejecuta de nuevo posteriormente cada hora.

Puede cambiar la configuración de programación para **una vez** si no desea que el indizador se ejecute de nuevo automáticamente, o a **diario** para ejecutar una vez al día. Establézcalo en **personalizado** si desea especificar un intervalo diferente o una hora de inicio futuras específico.

Cuando se establece la programación en **personalizado**, aparecen los campos que le permiten especificar el **intervalo** y el **hora de inicio (UTC)** . El menor tiempo de intervalo permitido es 5 minutos y el más largo es 1440 minutos (24 horas).

   ![Programación de indizador de configuración en el Asistente para importar datos](media/search-howto-schedule-indexers/schedule-import-data.png "programación del indexador de configuración en el Asistente para importar datos")

Una vez creado un indexador, puede cambiar la configuración de programación mediante el panel de edición del indizador. Los campos de programación son las mismas que en el Asistente para importar datos.

   ![Establecer la programación en el panel de edición de indizador](media/search-howto-schedule-indexers/schedule-edit.png "establecer la programación en el panel de edición de indizador")

<a name="restApi"></a>

## <a name="define-a-schedule-using-the-rest-api"></a>Definir una programación mediante la API de REST

Puede definir la programación de un indizador que usa la API de REST. Para ello, incluya el **programación** al crear o actualizar el indizador de propiedad. El ejemplo siguiente muestra una solicitud PUT para actualizar un indexador existente:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

El parámetro **interval** es obligatorio. El intervalo se refiere al tiempo entre el inicio de dos ejecuciones consecutivas de indizador. El intervalo mínimo permitido es de 5 minutos y el máximo de un día. Debe tener el formato de un valor "dayTimeDuration" XSD (subconjunto restringido de un valor de [duración ISO 8601](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). El patrón de este es: `P(nD)(T(nH)(nM))`. Ejemplos: `PT15M` para cada 15 minutos, `PT2H` para cada 2 horas.

El elemento opcional **startTime** indica cuándo deben comenzar las ejecuciones programadas. Si se omite, se usará la hora UTC actual. Este tiempo puede estar en el pasado, en el que está programada caso la primera ejecución como si el indizador se hubiera ejecutado continuamente desde el original **startTime**.

También puede ejecutar un indexador a petición en cualquier momento mediante la llamada de ejecución de indizador. Para obtener más información sobre cómo ejecutar los indizadores y establecer las programaciones de indizador, consulte [ejecutar indexador](https://docs.microsoft.com/rest/api/searchservice/run-indexer), [Get Indexer](https://docs.microsoft.com/rest/api/searchservice/get-indexer), y [actualización indizador](https://docs.microsoft.com/rest/api/searchservice/update-indexer) en la referencia de API de REST.

<a name="dotNetSdk"></a>

## <a name="define-a-schedule-using-the-net-sdk"></a>Definir una programación mediante el SDK de .NET

Puede definir la programación de un indizador que usa el SDK de .NET de Azure Search. Para ello, incluya el **programación** al crear o actualizar un indizador de propiedad.

La siguiente C# en el ejemplo se crea un indizador, con un origen de datos predefinido y un índice y establece su programación se ejecute una vez al día a partir de 30 minutos desde ahora:

```
    Indexer indexer = new Indexer(
        name: "azure-sql-indexer",
        dataSourceName: dataSource.Name,
        targetIndexName: index.Name,
        schedule: new IndexingSchedule(
                        TimeSpan.FromDays(1), 
                        new DateTimeOffset(DateTime.UtcNow.AddMinutes(30))
                    )
        );
    await searchService.Indexers.CreateOrUpdateAsync(indexer);
```
Si el **programación** parámetro se omite, el indizador solo se ejecutará una vez inmediatamente después de crearlo.

El **startTime** parámetro puede establecerse en un momento del pasado. En ese caso, se programa la primera ejecución como si el indizador se hubiera ejecutado continuamente desde el determinado **startTime**.

La programación se define utilizando el [IndexingSchedule](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingschedule?view=azure-dotnet) clase. El **IndexingSchedule** constructor requiere un **intervalo** los parámetros especificados mediante un **TimeSpan** objeto. El valor de intervalo más pequeño permitido es 5 minutos, y es el mayor de 24 horas. El segundo **startTime** parámetro, especificado como un **DateTimeOffset** de objetos, es opcional.

El SDK de .NET permite operaciones de indexador de control mediante la [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) clase y su [indizadores](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.indexers) propiedad, que implementa los métodos de la **IIndexersOperations**interfaz. 

Puede ejecutar un indexador a petición en cualquier momento mediante uno de los [ejecutar](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.run), [RunAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.runasync), o [RunWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations.runwithhttpmessagesasync) métodos.

Para obtener más información sobre la creación, actualización y la ejecución de indizadores, vea [IIindexersOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations?view=azure-dotnet).
