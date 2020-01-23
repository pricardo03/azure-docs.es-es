---
title: 'Almacenamiento y entrada de datos en la versión preliminar: Azure Time Series Insights | Microsoft Docs'
description: Información acerca del almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/31/2019
ms.custom: seodec18
ms.openlocfilehash: 1deca696ba576849701eb8719de7fbaa7895a26a
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75861411"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights

En este artículo se describen las actualizaciones en el almacenamiento y la entrada de datos de la versión preliminar de Azure Time Series Insights. Se trata la estructura de almacenamiento subyacente, el formato de archivos y la propiedad Time Series ID. También se describen el proceso de entrada subyacente, los procedimientos recomendados y las limitaciones actuales de la versión preliminar.

## <a name="data-ingress"></a>Entrada de datos

El entorno de Azure Time Series Insights contiene un motor de ingesta para recopilar, procesar y almacenar datos de series temporales. Al planear el entorno, hay algunas consideraciones que se deben tener en cuenta para asegurarse de que se procesan todos los datos entrantes y para lograr una gran escala de entrada y minimizar la latencia de ingesta (el tiempo que tarda TSI en leer y procesar los datos del evento de origen). 

Las directivas de entrada de datos de la versión preliminar de Time Series Insights determinan dónde se pueden obtener los datos de origen y qué formato deben tener.

### <a name="ingress-policies"></a>Directivas de entrada

La versión preliminar de Time Series Insights admite los siguientes orígenes de eventos:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

La versión preliminar de Time Series Insights admite un máximo de dos orígenes de eventos por instancia. Azure Time Series Insights admite JSON enviado a través de Azure IoT Hub o Azure Event Hubs.

> [!WARNING] 
> * Al conectar un origen del evento a un entorno de versión preliminar, puede experimentar una latencia inicial elevada. 
> La latencia del origen del evento depende del número de eventos que se encuentren actualmente en IoT Hub o en el Centro de eventos.
> * Tras la primera ingesta de datos del origen del evento la latencia elevada disminuirá. Póngase en contacto con nosotros enviando una incidencia de soporte técnico desde Azure Portal si sufre una latencia elevada de forma continuada.

## <a name="ingress-best-practices"></a>Procedimientos recomendados de entrada

Se recomienda aplicar los siguientes procedimientos recomendados:

* Configure Time Series Insights y un centro de IoT o un centro de eventos en la misma región. Esto reduce la latencia de ingesta debida a la red.
* Planee las necesidades de escala al calcular la tasa de ingesta prevista y comprobar que se encuentra dentro de la tasa admitida que se indica a continuación.
* Entienda cómo optimizar y dar forma a los datos JSON, así como las limitaciones actuales de la versión preliminar, al leer [Cómo dar forma a JSON para la entrada y la consulta](./time-series-insights-update-how-to-shape-events.md).

### <a name="ingress-scale-and-limitations-in-preview"></a>Escala y limitaciones de entrada de la versión preliminar

De forma predeterminada, los entornos de versión preliminar admiten tasas de entrada de hasta **1 megabyte por segundo (MB/s) por entorno**. Los clientes pueden escalar sus entornos de versión preliminar hasta un rendimiento de **16 MB/s** si es necesario.
Además, hay un límite por partición de **0,5 MB/s**. 

El límite por partición tiene implicaciones para los clientes que usan IoT Hub. En concreto, dada la afinidad entre un dispositivo de IoT Hub y una partición. En los escenarios en los que un dispositivo de puerta de enlace reenvía los mensajes al centro de conectividad con su propio identificador de dispositivo y cadena de conexión, existe el riesgo de alcanzar el límite de 0,5 MB/s, dado que los mensajes llegan en una sola partición, aunque la carga del evento especifique distintos identificadores de serie temporal. 

En general, las tasas de entrada se ven como el factor del número de dispositivos que hay en la organización, la frecuencia de emisión de eventos y el tamaño de cada evento:

*  **Número de dispositivos** × **Frecuencia de emisión de eventos** × **Tamaño de cada evento**.

> [!TIP]
> En el caso de los entornos que usan IoT Hub como origen del evento, para calcular la tasa de ingesta debe usar el número de conexiones en uso del centro de conectividad, en lugar del número total de dispositivos en uso o en la organización.

Para más información acerca de las unidades de procesamiento, los límites y las particiones:

* [Escala de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Escala del centro de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Particiones del centro de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>Almacenamiento de datos

Al crear un entorno de SKU de pago por uso de la versión preliminar de Time Series Insights, se crean dos recursos de Azure:

* Un entorno de versión preliminar de Time Series Insights que puede incluir opcionalmente capacidades de almacenamiento intermedio.
* Una cuenta de blob de uso general v1 de Azure Storage para el almacenamiento de datos en frío.

Los datos del almacenamiento intermedio solo están disponibles a través de [Consulta de Serie temporal](./time-series-insights-update-tsq.md) y el [explorador de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-explorer.md). 

La versión preliminar de Time Series Insights guarda los datos del almacenamiento intermedio en Azure Blob Storage, en [formato de archivo de Parquet](#parquet-file-format-and-folder-structure). La versión preliminar de Time Series Insights administra estos datos del almacenamiento intermedio de forma exclusiva, aunque están disponibles para leerse directamente como archivos estándar de Parquet.

> [!WARNING]
> Como propietario de la cuenta de Azure Blob Storage donde residen los datos del almacenamiento intermedio, tiene acceso total a todos los datos de la cuenta. Este acceso incluye permisos de escritura y eliminación. No modifique ni elimine los datos que escribe la versión preliminar de Time Series Insights, ya que esto puede provocar una pérdida de datos.

### <a name="data-availability"></a>Disponibilidad de los datos

La versión preliminar de Time Series Insights crea particiones de los datos y los indexa para lograr un rendimiento óptimo de las consultas. Los datos están disponibles para su consulta una vez indexados. La cantidad de datos que se va a ingerir puede afectar a esta disponibilidad.

> [!IMPORTANT]
> La próxima versión de disponibilidad general (GA) de Time Series Insights logra que los datos estén disponibles 60 segundos después de leerse en el origen de eventos. Durante la versión preliminar, es posible que este intervalo hasta que los datos estén disponibles sea más largo. Si experimenta una latencia considerable de más de 60 segundos, envíe una incidencia de soporte técnico a través de Azure Portal.

## <a name="azure-storage"></a>Azure Storage

En esta sección se describen detalles de Azure Storage relevantes para la versión preliminar de Azure Time Series Insights.

Para obtener una descripción detallada de Azure Blob Storage, lea [Introducción a Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

### <a name="your-storage-account"></a>Cuenta de almacenamiento

Cuando se crea un entorno de pago por uso de la versión preliminar de Time Series Insights, se crea una cuenta de blob de uso general v1 de Azure Storage como almacenamiento en frío a largo plazo.  

La versión preliminar de Time Series Insights publica un máximo de dos copias de cada evento en su cuenta de Azure Storage. La copia inicial tiene eventos ordenados por hora de ingesta y siempre se conserva, por lo que se pueden usar otros servicios para acceder a ella. Puede usar Spark, Hadoop y otras herramientas conocidas para procesar los archivos de Parquet sin formato. 

La versión preliminar de Time Series Insights vuelve a crear particiones de los archivos de Parquet a fin de optimizarlos para la consulta de Time Series Insights. Esta copia reparticionada de los datos también se guarda.

Durante la versión preliminar pública, los datos se almacenan de forma indefinida en su cuenta de Azure Storage.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Escritura y edición de blobs de Time Series Insights

Para garantizar el rendimiento de las consultas y la disponibilidad de los datos, no edite ni elimine los blobs que crea la versión preliminar de Time Series Insights.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Acceso a los datos de la versión preliminar de Time Series Insights y exportación

Puede que quiera acceder a los datos vistos en el explorador de la versión preliminar de Time Series Insights para usarlos con otros servicios. Por ejemplo, puede usar los datos para crear un informe en Power BI o para entrenar un modelo de aprendizaje automático mediante Azure Machine Learning Studio. También puede usar los datos para transformar, visualizar y modelar en los cuadernos de Jupyter Notebook.

En general, se puede acceder a los datos de tres maneras:

* Desde el explorador de la versión preliminar de Time Series Insights. Puede exportar datos como archivo CSV desde el explorador. Para más información, lea [Explorador de la versión preliminar de Time Series Insights](./time-series-insights-update-explorer.md).
* Desde las API de la versión preliminar de Time Series Insights. Puede alcanzar el punto de conexión de API en `/getRecorded`. Para más información sobre esta API, lea [Consultas en Time Series](./time-series-insights-update-tsq.md).
* Directamente desde una cuenta de Azure Storage. Necesita acceso de lectura a la cuenta que usa para acceder a los datos de la versión preliminar de Time Series Insights. Para más información, lea [Administración del acceso a los recursos de la cuenta de almacenamiento](../storage/blobs/storage-manage-access-to-resources.md).

### <a name="data-deletion"></a>Eliminación de datos

No elimine los archivos de la versión preliminar de Time Series Insights. Los datos relacionados solo se administran desde la versión preliminar de Time Series Insights.

## <a name="parquet-file-format-and-folder-structure"></a>Formato de archivo de Parquet y estructura de carpetas

Parquet es un formato de archivo en columnas de código abierto diseñado para lograr un almacenamiento y un rendimiento eficaces. La versión preliminar de Time Series Insights usa Parquet por estos motivos. Crea particiones de los datos por identificador de serie temporal para el rendimiento de las consultas a escala.  

Para más información sobre el tipo de archivo de Parquet, lea la [documentación de Parquet](https://parquet.apache.org/documentation/latest/).

La versión preliminar de Time Series Insights almacena copias de los datos de la siguiente manera:

* La primera copia inicial se particiona por hora de ingesta y almacena los datos más o menos en orden de llegada. Los datos residen en la carpeta `PT=Time`:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* La segunda copia reparticionada se particiona por una agrupación de identificadores de serie temporal y reside en la carpeta `PT=TsId`:

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

En ambos casos, los valores de hora corresponden a la hora de creación del blob. Los datos de la carpeta `PT=Time` se conservan. Los datos de la carpeta `PT=TsId` se optimizan para la consulta a lo largo del tiempo y no permanecen estáticos.

> [!NOTE]
> * `<YYYY>` se asigna a una representación de año de cuatro dígitos.
> * `<MM>` se asigna a una representación de mes de dos dígitos.
> * `<YYYYMMDDHHMMSSfff>` se asigna a una representación de marca de tiempo con un año de cuatro dígitos (`YYYY`), un mes de dos dígitos (`MM`), un día de dos dígitos (`DD`), una hora de dos dígitos (`HH`), un minuto de dos dígitos (`MM`), un segundo de dos dígitos (`SS`) y un milisegundo de tres dígitos (`fff`).

Los eventos de la versión preliminar de Time Series Insights se asignan al contenido de los archivos de Parquet de esta manera:

* Cada evento se asigna a una sola fila.
* Cada fila incluye la columna **timestamp** con una marca de tiempo del evento. La propiedad time-stamp nunca es null. Su valor predeterminado es **event enqueued time** si no se especifica en el origen del evento. La marca de tiempo siempre está en UTC.
* Cada fila incluye la columna de identificador de serie temporal como se define durante la creación del entorno de Time Series Insights. El nombre de la propiedad incluye el sufijo `_string`.
* Las demás propiedades enviadas como datos de telemetría se asignan a nombres de columna que terminan en `_string` (cadena), `_bool` (booleano), `_datetime` (fecha y hora) o `_double` (doble), en función del tipo de propiedad.
* Este esquema de asignación se aplica a la primera versión del formato de archivo, a la que se hace referencia como **V=1**. A medida que esta característica evoluciona, el nombre puede incrementarse.

## <a name="next-steps"></a>Pasos siguientes

- Lea [Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights](./time-series-insights-update-storage-ingress.md).

- Lea sobre el nuevo [modelo de datos](./time-series-insights-update-tsm.md).
