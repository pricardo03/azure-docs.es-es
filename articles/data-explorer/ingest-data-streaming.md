---
title: Uso de ingesta de streaming para ingerir datos en Azure Data Explorer
description: Aprenda a ingerir (cargar) datos en Azure Data Explorer mediante la ingesta de streaming.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: b716cbf3efb044da68d4dd1dcb724369855d1ed1
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70174435"
---
# <a name="streaming-ingestion-preview"></a>Ingesta de streaming (versión preliminar)

La ingesta de streaming está destinada a escenarios que requieren una latencia baja con un tiempo de ingesta de menos de 10 segundos de datos de volumen variado. Se usa para la optimización del procesamiento operativo de muchas tablas, en una o varias bases de datos donde el flujo de datos de cada tabla es relativamente pequeño (pocos registros por segundo), pero el volumen de ingesta de datos global es alto (miles de registros por segundo).

Use la ingesta clásica (masiva) en lugar de la ingesta de streaming cuando la cantidad de datos crezca más de 1 MB por segundo y tabla. Lea [Introducción a la ingesta de datos](/azure/data-explorer/ingest-data-overview) para conocer más sobre los distintos métodos de ingesta.

> [!NOTE]
> La ingesta de streaming no admite las siguientes características:
> * [Cursores de base de datos](/azure/kusto/management/databasecursor).
> * [Asignación de datos](/azure/kusto/management/mappings). Solo se admite la asignación de datos [creada previamente](/azure/kusto/management/tables#create-ingestion-mapping). 

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* Inicie sesión en la [interfaz de usuario web](https://dataexplorer.azure.com/).
* Cree un [clúster y una base de datos de Azure Data Explorer](create-cluster-database-portal.md).

## <a name="enable-streaming-ingestion-on-your-cluster"></a>Habilitación de la ingesta de streaming en el clúster

1. En Azure Portal, vaya al clúster de Azure Data Explorer. En **Configuración**, seleccione **Configuraciones**. 
1. En el panel **Configuraciones**, seleccione **Activado** para habilitar la **ingesta de streaming**.
1. Seleccione **Guardar**.
 
    ![Ingesta de streaming activada](media/ingest-data-streaming/streaming-ingestion-on.png)
 
1. En la [interfaz de usuario web](https://dataexplorer.azure.com/), defina la [directiva de ingesta de streaming](/azure/kusto/concepts/streamingingestionpolicy) en las tablas o las bases de datos que van a recibir los datos de streaming. 

    > [!TIP]
    > Si la directiva está definida en el nivel de base de datos, todas las tablas de la base de datos están habilitadas para la ingesta de streaming.

## <a name="supported-streaming-ingestion-types"></a>Tipos de ingesta de streaming admitidos

Hay dos tipos de ingesta de streaming admitidos:

* [Event Hub](/azure/data-explorer/ingest-data-event-hub) usado como origen de datos
* La ingesta personalizada requiere que escriba una aplicación que use una de las bibliotecas cliente de Azure Data Explorer. Consulte el [ejemplo de ingesta de streaming](https://github.com/Azure/azure-kusto-samples-dotnet/tree/master/client/StreamingIngestionSample) para obtener una aplicación de ejemplo.

### <a name="choose-the-appropriate-streaming-ingestion-type"></a>Selección del tipo de ingesta de streaming adecuado

|   |Centro de eventos  |Ingesta personalizada  |
|---------|---------|---------|
|Retraso de datos entre el inicio de la ingesta y los datos disponibles para la consulta   |    retraso más largo     |   retraso más corto      |
|Sobrecarga de desarrollo    |   configuración rápida y sencilla, sin sobrecarga de desarrollo    |   gran sobrecarga de desarrollo por la aplicación para que controle los errores y garantice la coherencia de los datos     |

## <a name="disable-streaming-ingestion-on-your-cluster"></a>Deshabilitación de la ingesta de streaming en el clúster

> [!WARNING]
> La deshabilitación de la ingesta de streaming puede tardar unas horas.

1. Quite la [directiva de ingesta de streaming](/azure/kusto/concepts/streamingingestionpolicy) de todas las tablas y bases de datos pertinentes. La eliminación de la directiva de ingesta de streaming desencadena el movimiento de datos de ingesta de streaming desde el almacenamiento inicial hasta el almacenamiento permanente en el almacén de columnas (extensiones o particiones). El movimiento de datos puede durar entre unos segundos y algunas horas, según la cantidad de datos en el almacenamiento inicial y el uso de CPU y memoria del clúster.
1. En Azure Portal, vaya al clúster de Azure Data Explorer. En **Configuración**, seleccione **Configuraciones**. 
1. En el panel **Configuraciones**, seleccione **Desactivado** para deshabilitar la **ingesta de streaming**.
1. Seleccione **Guardar**.

    ![Ingesta de streaming desactivada](media/ingest-data-streaming/streaming-ingestion-off.png)

## <a name="limitations"></a>Limitaciones

* El rendimiento y la capacidad de la ingesta de streaming se escalan cuando aumentan los tamaños de las máquinas virtuales y los clústeres. En el caso de un nodo D11, la carga recomendada es de hasta 20 solicitudes por segundo. Para un nodo D14, la carga recomendada es de hasta 150 solicitudes por segundo.
* Actualmente, solo existe compatibilidad con las SKU de 8 y 16 núcleos (D13, D14, L8 y L16).
* El límite de tamaño de datos por solicitud de ingesta es de 4 MB.
* Las actualizaciones de esquema, como la creación y modificación de tablas y asignaciones de ingesta, pueden tardar hasta cinco minutos en el servicio de ingesta de streaming.
* Cuando se habilita la ingesta de streaming en un clúster, incluso cuando los datos no se ingieren a través de streaming, se usa parte del disco SSD local de las máquinas del clúster para los datos de ingesta de streaming y se reduce el almacenamiento disponible para la caché activa.

## <a name="next-steps"></a>Pasos siguientes

* [Consulta de datos en Azure Data Explorer](web-query-data.md)