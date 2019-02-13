---
title: Ingesta de datos de ejemplo en el Explorador de datos de Azure
description: Obtenga información sobre cómo ingerir (cargar) datos de ejemplo relacionadas con el tiempo en el Explorador de datos de Azure.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: ebe362b89eb6ffcde006ff824fbee40b1a9175d3
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55733406"
---
# <a name="ingest-sample-data-into-azure-data-explorer"></a>Ingesta de datos de ejemplo en el Explorador de datos de Azure

En este artículo, se muestra cómo ingerir (cargar) datos de ejemplo en una base de datos del Explorador de datos de Azure. Hay [varias maneras de ingerir datos](ingest-data-overview.md); este artículo se centra en un enfoque básico que es adecuado para realizar pruebas.

> [!NOTE]
> Ya tiene estos datos si completó [Guía de inicio rápido: Ingesta de datos mediante la biblioteca de Python de Azure Data Explorer](python-ingest-data.md).

## <a name="prerequisites"></a>Requisitos previos

[Base de datos y clúster de prueba](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Introducción de datos

El conjunto de datos de ejemplo de **StormEvents** contiene datos relacionados con el tiempo de los [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

1. Inicie sesión en [https://dataexplorer.azure.com](https://dataexplorer.azure.com).

1. En la parte superior izquierda de la aplicación, haga clic en **Agregar clúster**.

1. En el cuadro de diálogo **Agregar clúster**, escriba la URL del clúster con el formato `https://<ClusterName>.<Region>.kusto.windows.net/` y después haga clic en **Agregar**.

1. Pegue el siguiente comando y haga clic en **Ejecutar**.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)

    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. Una vez finalizada la ingesta, pegue la siguiente consulta, seleccione la consulta en la ventana y haga clic en **Ejecutar**.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    La consulta devuelve los siguientes resultados de los datos de ejemplo ingeridos.

    ![Resultados de la consulta](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: Consulta de datos en Azure Data Explorer](web-query-data.md)

> [!div class="nextstepaction"]
> [Escritura de consultas](write-queries.md)

> [!div class="nextstepaction"]
> [Azure Data Explorer data ingestion](ingest-data-overview.md) (Ingesta de datos del Explorador de datos de Azure)