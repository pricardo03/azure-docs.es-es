---
title: 'Almacenamiento y entrada de datos en la versión preliminar: Azure Time Series Insights | Microsoft Docs'
description: Información acerca del almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights.
author: lyrana
ms.author: lyhughes
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/31/2019
ms.custom: seodec18
ms.openlocfilehash: f00529d00312fd6acb045de698590047f991bec7
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76714303"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights

En este artículo se describen las actualizaciones en el almacenamiento y la entrada de datos de la versión preliminar de Azure Time Series Insights. Se trata la estructura de almacenamiento subyacente, el formato de archivos y la propiedad Time Series ID. También se describen el proceso de entrada subyacente, los procedimientos recomendados y las limitaciones actuales de la versión preliminar.

## <a name="data-ingress"></a>Entrada de datos

El entorno de Azure Time Series Insights contiene un motor de ingesta para recopilar, procesar y almacenar datos de series temporales. Al planear el entorno, hay algunas consideraciones que se deben tener en cuenta para asegurarse de que se procesan todos los datos entrantes y para lograr una gran escala de entrada y minimizar la latencia de ingesta (el tiempo que tarda TSI en leer y procesar los datos del evento de origen). 

Las directivas de entrada de datos de la versión preliminar de Time Series Insights determinan dónde se pueden obtener los datos de origen y qué formato deben tener.

### <a name="ingress-policies"></a>Directivas de entrada

#### <a name="event-sources"></a>Orígenes de eventos

La versión preliminar de Time Series Insights admite los siguientes orígenes de eventos:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

La versión preliminar de Time Series Insights admite un máximo de dos orígenes de eventos por instancia.

> [!WARNING] 
> * Al conectar un origen del evento a un entorno de versión preliminar, puede experimentar una latencia inicial elevada. 
> La latencia del origen del evento depende del número de eventos que se encuentren actualmente en IoT Hub o en el Centro de eventos.
> * Tras la primera ingesta de datos del origen del evento la latencia elevada disminuirá. Póngase en contacto con nosotros enviando una incidencia de soporte técnico desde Azure Portal si sufre una latencia elevada de forma continuada.

#### <a name="supported-data-format-and-types"></a>Tipos y formatos de datos admitidos

Azure Time Series Insights admite JSON codificado en UTF8 enviado a través de Azure IoT Hub o Azure Event Hubs. 

A continuación se muestra la lista de los tipos de datos admitidos.

| Tipo de datos | Descripción |
|-----------|------------------|-------------|
| bool      |   Un tipo de datos que tiene uno de dos estados: true o false.       |
| dateTime    |   Representa un instante de tiempo, normalmente expresado en forma de fecha y hora del día. Los valores DateTime deben tener el formato ISO 8601.      |
| double    |   Un punto flotante IEEE 754 de doble precisión de 64 bits.
| string    |   Valores de texto, compuestos de caracteres Unicode.          |

#### <a name="objects-and-arrays"></a>Objetos y matrices

Puede enviar tipos complejos, como objetos y matrices, como parte de la carga del evento, pero los datos se someterán a un proceso de acoplamiento al almacenarse. Para obtener más información sobre cómo dar forma a los eventos JSON, así como detalles sobre el tipo complejo y el acoplamiento de objetos anidados, vea la página sobre [cómo dar forma a JSON para la entrada y la consulta](./time-series-insights-update-how-to-shape-events.md).


### <a name="ingress-best-practices"></a>Procedimientos recomendados de entrada

Se recomienda aplicar los siguientes procedimientos recomendados:

* Configure Time Series Insights y la instancia de IoT Hub o el centro de eventos en la misma región con el fin de reducir la latencia de ingesta de la red.
* Planee las necesidades de escala al calcular la tasa de ingesta prevista y comprobar que se encuentra dentro de la tasa admitida que se indica a continuación.
* Entienda cómo optimizar y dar forma a los datos JSON, así como las limitaciones actuales de la versión preliminar, al leer [Cómo dar forma a JSON para la entrada y la consulta](./time-series-insights-update-how-to-shape-events.md).

### <a name="ingress-scale-and-limitations-in-preview"></a>Escala y limitaciones de entrada de la versión preliminar

#### <a name="per-environment-limitations"></a>Limitaciones de cada entorno

En general, las tasas de entrada se ven como el factor del número de dispositivos que hay en la organización, la frecuencia de emisión de eventos y el tamaño de cada evento:

*  **Número de dispositivos** × **Frecuencia de emisión de eventos** × **Tamaño de cada evento**.

De forma predeterminada, la versión preliminar de Time Series Insights puede ingerir datos entrantes a una velocidad de hasta 1 MB por segundo (MBps) **por entorno de TSI**. Póngase en contacto con nosotros si esto no cumple sus requisitos, podemos admitir hasta 16 MBps para un entorno mediante el envío de una incidencia de soporte técnico en Azure Portal.
 
Ejemplo 1: Contoso Shipping tiene 100.000 dispositivos que emiten un evento tres veces por minuto. El tamaño de un evento es de 200 bytes. Están usando una instancia de Event Hubs con 4 particiones como origen de eventos de TSI.
La tasa de ingesta para su entorno de TSI sería: 100.000 dispositivos * 200 bytes/evento * (3/60 eventos por segundo) = 1 MBps.
La tasa de ingesta por partición sería de 0,25 MBps.
La tasa de ingesta de Contoso Shipping se encuentra dentro del límite de escala de la versión preliminar.
 
Ejemplo 2: Contoso Fleet Analytics tiene 60.000 dispositivos que emiten un evento cada segundo. Están utilizando un recuento de 24 particiones de IoT Hub de 4 como origen del evento de TSI. El tamaño de un evento es de 200 bytes.
La tasa de ingesta de entorno sería: 20.000 dispositivos * 200 bytes/evento * (1 evento por segundo) = 4 MBps.
La tasa de ingesta por partición sería de 1 MBps.
Contoso Fleet Analytics necesitaría enviar una solicitud a TSI a través de Azure Portal para que un entorno dedicado alcance esta escala.

#### <a name="hub-partitions-and-per-partition-limits"></a>Particiones del centro y límites por partición

Al planear el entorno de TSI, es importante tener en cuenta la configuración de los orígenes de eventos que se van a conectar a TSI. Tanto Azure IoT Hub como Event Hubs usan particiones para habilitar el escalado horizontal para el procesamiento de eventos.  Una partición es una secuencia ordenada de eventos que se mantiene en un centro. El número de particiones se establece durante la fase de creación de IoT o Event Hubs, y no es modificable. Para más información para determinar el número de particiones, consulte esta pregunta frecuente sobre Event Hubs: ¿Cuántas particiones necesito? En el caso de los entornos de TSI que usan IoT Hub, generalmente la mayoría de las instancias de IoT Hub solo necesitan 4 particiones. Tanto si está creando un nuevo centro para el entorno de TSI como si usa uno existente, deberá calcular la velocidad de ingesta por partición para determinar si se encuentra dentro de los límites de la versión preliminar. La versión preliminar de TSI actualmente tiene un límite **por partición** de 0,5 MB/s. Use los ejemplos siguientes como referencia y tenga en cuenta la siguiente consideración específica de IoT Hub si es un usuario de IoT Hub.

#### <a name="iot-hub-specific-considerations"></a>Consideraciones específicas de IoT Hub

Cuando se crea un dispositivo en IoT Hub, se asigna a una partición y la asignación de la partición no cambia. Al hacerlo, IoT Hub puede garantizar el orden de los eventos. Sin embargo, esto tiene implicaciones en TSI como lector inferior en ciertos escenarios. Cuando los mensajes de varios dispositivos se reenvían al centro con el mismo identificador de dispositivo de puerta de enlace, llegarán a la misma partición, lo que posiblemente haga superar el límite de escala por partición. 

**Impacto**: Si una sola partición experimenta una tasa sostenida de ingesta sobre la limitación de la versión preliminar, existe la posibilidad de que el lector de TSI no la alcance antes de que se haya superado el período de retención de datos de IoT Hub. Esto provocaría una pérdida de datos.

Se recomienda lo siguiente: 

* Calcule la tasa de ingesta por entorno y por partición antes de implementar la solución
* Asegúrese de que los dispositivos IoT Hub (y, por lo tanto, las particiones) tienen una carga equilibrada con la extensión más lejana posible.

> [!WARNING]
> En el caso de los entornos que usan IoT Hub como origen de eventos, calcule la tasa de ingesta usando el número de dispositivos concentradores en uso para asegurarse de que la velocidad esté por debajo de 0,5 MBps por límite de partición en la versión preliminar.

  ![Diagrama de particiones de IoT Hub](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Vea los vínculos siguientes para obtener más información sobre las unidades de procesamiento y las particiones:

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
> Durante la versión preliminar, es posible que experimente un período de hasta 60 segundos hasta que los datos estén disponibles. Si experimenta una latencia considerable de más de 60 segundos, envíe una incidencia de soporte técnico a través de Azure Portal.

## <a name="azure-storage"></a>Azure Storage

En esta sección se describen detalles de Azure Storage relevantes para la versión preliminar de Azure Time Series Insights.

Para obtener una descripción detallada de Azure Blob Storage, lea [Introducción a Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

### <a name="your-storage-account"></a>Cuenta de almacenamiento

Cuando se crea un entorno de pago por uso de la versión preliminar de Time Series Insights, se crea una cuenta de blob de uso general v1 de Azure Storage como almacenamiento en frío a largo plazo.  

La versión preliminar de Time Series Insights publica un máximo de dos copias de cada evento en su cuenta de Azure Storage. La copia inicial tiene eventos ordenados por hora de ingesta y siempre se conserva, por lo que se pueden usar otros servicios para acceder a ella. Puede usar Spark, Hadoop y otras herramientas conocidas para procesar los archivos de Parquet sin formato. 

La versión preliminar de Time Series Insights vuelve a crear particiones de los archivos de Parquet a fin de optimizarlos para la consulta de Time Series Insights. Esta copia reparticionada de los datos también se guarda.

Durante la versión preliminar pública, los datos se almacenan de forma indefinida en su cuenta de Azure Storage.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Escritura y edición de blobs de Time Series Insights

Para garantizar el rendimiento de las consultas y la disponibilidad de los datos, no edite ni elimine los blobs que crea la versión preliminar de Time Series Insights.

#### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Acceso a los datos de la versión preliminar de Time Series Insights y exportación

Puede que quiera acceder a los datos vistos en el explorador de la versión preliminar de Time Series Insights para usarlos con otros servicios. Por ejemplo, puede usar los datos para crear un informe en Power BI o para entrenar un modelo de aprendizaje automático mediante Azure Machine Learning Studio. También puede usar los datos para transformar, visualizar y modelar en los cuadernos de Jupyter Notebook.

En general, se puede acceder a los datos de tres maneras:

* Desde el explorador de la versión preliminar de Time Series Insights. Puede exportar datos como archivo CSV desde el explorador. Para más información, lea [Explorador de la versión preliminar de Time Series Insights](./time-series-insights-update-explorer.md).
* Desde las API de la versión preliminar de Time Series Insights con la consulta de obtención de eventos. Para más información sobre esta API, lea [Consultas en Time Series](./time-series-insights-update-tsq.md).
* Directamente desde una cuenta de Azure Storage. Necesita acceso de lectura a la cuenta que usa para acceder a los datos de la versión preliminar de Time Series Insights. Para más información, lea [Administración del acceso a los recursos de la cuenta de almacenamiento](../storage/blobs/storage-manage-access-to-resources.md).

#### <a name="data-deletion"></a>Eliminación de datos

No elimine los archivos de la versión preliminar de Time Series Insights. Los datos relacionados solo se administran desde la versión preliminar de Time Series Insights.

### <a name="parquet-file-format-and-folder-structure"></a>Formato de archivo de Parquet y estructura de carpetas

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

- Lea [Dar forma JSON para entradas y consultas](./time-series-insights-update-how-to-shape-events.md).

- Lea sobre el nuevo [modelo de datos](./time-series-insights-update-tsm.md).
