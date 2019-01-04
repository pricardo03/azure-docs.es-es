---
title: Patrones de ruta de acceso de DateTime para la salida de Blob Storage de Azure Stream Analytics (versión preliminar)
description: En este artículo se describe la característica de patrones de rutas de acceso de DateTime personalizada para la salida de Blob Storage desde trabajos de Azure Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: ba386539c3f3c6740b843575bbccd4b028b8a5a7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090797"
---
# <a name="custom-datetime-path-patterns-for-azure-stream-analytics-blob-storage-output-preview"></a>Patrones de la ruta de acceso de fecha y hora personalizados para la salida de Blob Storage de Azure Stream Analytics (versión preliminar)

Azure Stream Analytics admite especificadores de formato de fecha y hora personalizados en la ruta de acceso del archivo para salidas de Blob Storage. Los patrones de la ruta de acceso de fecha y hora personalizados le permiten especificar un formato de salida que se alinee con las convenciones de Hive Streaming, lo que le proporciona a Azure Stream Analytics la capacidad de enviar datos a Azure HDInsight y Azure Databricks para su procesamiento posterior. Los patrones de la ruta de acceso de fecha personalizados se implementan fácilmente utilizando la palabra clave `datetime` en el campo Prefijo de ruta de acceso de la salida de blob, así como el especificador de formato. Por ejemplo, `{datetime:yyyy}`.

Use este vínculo de [Azure Portal](https://portal.azure.com/?Microsoft_Azure_StreamAnalytics_bloboutputcustomdatetimeformats=true) para alternar la marca de característica que habilita los patrones de la ruta de acceso de fecha y hora personalizados para la versión preliminar de la salida de Blob Storage. Esta característica pronto estará habilitada en el portal principal.

## <a name="supported-tokens"></a>Tokens admitidos

Los siguientes tokens del especificador de formato se pueden utilizar solos o en combinación para conseguir formatos personalizados de fecha y hora:

|Especificador de formato   |DESCRIPCIÓN   |Resultados en la hora de ejemplo 2018-01-02T10: 06: 08|
|----------|-----------|------------|
|{datetime:yyyy}|El año como un número de cuatro dígitos|2018|
|{datetime:MM}|Mes desde 01 hasta 12|01|
|{datetime:M}|Mes desde 1 hasta 12|1|
|{datetime:dd}|Día desde 01 hasta 31|02|
|{datetime:d}|Día desde 1 hasta 12|2|
|{datetime:HH}|Hora con el formato de 24 horas, de 00 a 23|10|
|{datetime:mm}|Minutos de 00 a 24|06|
|{datetime:m}|Minutos de 0 a 24|6|
|{datetime:ss}|Segundos de 00 a 60|08|

Si no quiere usar patrones de fecha y hora personalizados, puede agregar el token {date} o {time} al prefijo de ruta de acceso para crear un menú desplegable con los formatos incorporados de fecha y hora.

![Formatos antiguos de fecha y hora de Stream Analytics](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-old-date-time-formats.png)

## <a name="extensibility-and-restrictions"></a>Extensibilidad y restricciones

Puede usar tantos tokens (`{datetime:<specifier>}`) como quiera en el patrón de la ruta de acceso hasta que alcance el límite de caracteres del prefijo de ruta de acceso. Los especificadores de formato no se pueden combinar en un solo token si van más allá de las combinaciones ya enumeradas en los menús desplegables de fecha y hora. 

Para una partición de ruta de `logs/MM/dd`:

|Expresión válida   |Expresión no válida   |
|----------|-----------|
|`logs/{datetime:MM}/{datetime:dd}`|`logs/{datetime:MM/dd}`|

Puede usar el mismo especificador de formato varias veces en el prefijo de ruta de acceso. El token debe repetirse cada vez.

## <a name="hive-streaming-conventions"></a>Convenciones de Hive Streaming

Los patrones de ruta de acceso personalizados para Blob Storage se pueden usar con la convención Hive Streaming, que espera que las carpetas estén etiquetadas con `column=` en el nombre de la carpeta.

Por ejemplo, `year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}/hour={datetime:HH}`.

La salida personalizada elimina la necesidad de tener que alterar las tablas y agregar particiones manualmente a los datos de puertos que estén entre Azure Stream Analytics y Hive. En su lugar, muchas carpetas se pueden agregar automáticamente mediante:

```SQL
MSCK REPAIR TABLE while hive.exec.dynamic.partition true
```

### <a name="example"></a>Ejemplo

Cree una cuenta de almacenamiento, un grupo de recursos, un trabajo de Stream Analytics y un origen de entrada de acuerdo con la guía de inicio rápido de [Azure Stream Analytics mediante Azure Portal ](stream-analytics-quick-create-portal.md). Use los mismos datos de muestra utilizados en la guía de inicio rápido, que también está disponible en [GitHub](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json).

Cree un receptor de salida de blob con la siguiente configuración:

![Stream Analytics crea un receptor de salida de blob](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-create-output-sink.png)

El patrón de la ruta de acceso completo es el siguiente:


`year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}`


Cuando inicie el trabajo, se creará una estructura de carpetas basada en el patrón de la ruta de acceso en el contenedor de blobs. Puede explorar en profundidad hasta llegar al nivel del día.

![Salida de blobs de Stream Analytics con patrón de ruta de acceso personalizado](./media/stream-analytics-custom-path-patterns-blob-storage-output/stream-analytics-blob-output-folder-structure.png)

## <a name="next-steps"></a>Pasos siguientes

* [Información sobre las salidas desde Azure Stream Analytics](stream-analytics-define-outputs.md)
