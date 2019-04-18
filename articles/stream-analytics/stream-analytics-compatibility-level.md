---
title: Nivel de compatibilidad de los trabajos de Azure Stream Analytics
description: Obtenga información acerca de cómo establecer un nivel de compatibilidad para un trabajo de análisis de Azure Stream Analytics y de los principales cambios en el nivel de compatibilidad más reciente.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2019
ms.openlocfilehash: b5c833798f8533e7c6fbe3595a726ac6ce56e2d2
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682821"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Nivel de compatibilidad de los trabajos de Azure Stream Analytics

En este artículo se describe la opción de nivel de compatibilidad en Azure Stream Analytics. Stream Analytics es un servicio administrado, con actualizaciones regulares de características y mejoras de rendimiento. La mayoría de las actualizaciones de los tiempos de ejecución del servicio automáticamente están disponible para los usuarios finales. 

Sin embargo, algunas funcionalidades nuevas en el servicio pueden introducir un cambio importante, como un cambio en el comportamiento de un trabajo existente, o un cambio en la forma que los datos se consume en la ejecución de trabajos. Puede mantener los trabajos de Stream Analytics existentes que se ejecutan sin cambios importantes dejando reducida de establecer el nivel de compatibilidad. Cuando esté listo para los comportamientos en tiempo de ejecución más recientes, puede participar en al aumentar el nivel de compatibilidad. 

## <a name="choose-a-compatibility-level"></a>Elija un nivel de compatibilidad

Los niveles de compatibilidad controlan el comportamiento en tiempo de ejecución de un trabajo de Stream Analytics. 

Azure Stream Analytics actualmente admite tres niveles de compatibilidad:

* 1.0 – nivel predeterminado
* 1.1: comportamiento de la versión actual
* 1.2 (versión preliminar): el comportamiento más reciente con las mejoras más recientes en la evaluación

El nivel de compatibilidad original 1.0 se introdujo durante la disponibilidad general de Azure Stream Analytics hace varios años.

Cuando se crea un nuevo trabajo de Stream Analytics, es una práctica recomendada para crearlo mediante el nivel de compatibilidad más reciente. Inicie el diseño de trabajo de usuario de confianza en los comportamientos más recientes, para evitar el cambio se ha agregado y la complejidad más adelante.

## <a name="set-the-compatibility-level"></a>Definición del nivel de compatibilidad

Puede establecer el nivel de compatibilidad para un trabajo de Stream Analytics en Azure portal o mediante el [crear llamada de API de REST de trabajo](/rest/api/streamanalytics/stream-analytics-job).

Para actualizar el nivel de compatibilidad del trabajo en el portal de Azure:

1. Use la [portal Azure](https://portal.azure.com) en busca de su trabajo de Stream Analytics.
2. **Detener** el trabajo antes de actualizar el nivel de compatibilidad. No se puede actualizar el nivel de compatibilidad si el trabajo está en un estado de ejecución.
3. En el **configurar** encabezado, seleccione **ivel**.
4. Elija el valor de nivel de compatibilidad que desee.
5. Seleccione **guardar** en la parte inferior de la página.

![Nivel de compatibilidad de Stream Analytics en Azure Portal](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

Cuando se actualiza el nivel de compatibilidad, el compilador de T-SQL valida el trabajo con la sintaxis que se corresponde con el nivel de compatibilidad seleccionado.

## <a name="compatibility-level-12"></a>Nivel de compatibilidad 1.2

Se introducen los siguientes cambios principales en el nivel de compatibilidad 1.2:

### <a name="geospatial-functions"></a>Funciones geoespaciales

**Niveles anteriores:** Azure Stream Analytics usa los cálculos geográficos.

**nivel 1.2:** Azure Stream Analytics permite calcular las coordenadas geográficas proyectado geométrica. No hay ningún cambio en la firma de las funciones geoespaciales. Sin embargo, su semántica es ligeramente diferente, lo que permite un cálculo más preciso que antes.

Azure Stream Analytics admite la indexación de datos de referencia geoespacial. Datos de referencia que contiene elementos geoespaciales se pueden indizar para un cálculo de combinación más rápido.

Las funciones geoespaciales actualizada traer la expresividad del formato de Well Known Text (WKT) geoespaciales completa. Puede especificar otros componentes geoespaciales que anteriormente no eran compatibles con GeoJson.

Para obtener más información, consulte [actualiza a funciones geoespaciales en Azure Stream Analytics: en la nube y IoT Edge](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/).

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>Ejecución de consultas en paralelo para los orígenes de entrada con varias particiones

**Niveles anteriores:** Consultas de Azure Stream Analytics requerían el uso de la cláusula PARTITION BY para paralelizar el procesamiento de consultas entre particiones de origen de entrada.

**nivel 1.2:** Si la lógica de la consulta se puede ejecutar en paralelo entre las particiones de origen de entrada, Azure Stream Analytics crea instancias de consulta independiente y ejecuta los cálculos en paralelo.

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>Integración nativa de API de forma masiva con la salida de CosmosDB

**Niveles anteriores:** El comportamiento de upsert era *insertar o combinar*.

**nivel 1.2:** Integración nativa de API de forma masiva con la salida de CosmosDB maximiza el rendimiento y se controla con gran eficacia la limitación de solicitudes.

Es el comportamiento de upsert *insertar o reemplazar*.

### <a name="datetimeoffset-when-writing-to-sql-output"></a>DateTimeOffset al escribir en la salida de SQL

**Niveles anteriores:** [DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017) tipos se ajustan a la hora UTC.

**nivel 1.2:** Ya no se ajusta DateTimeOffset.

### <a name="strict-validation-of-prefix-of-functions"></a>Validación estricta de prefijo de funciones

**Niveles anteriores:** No se produjo ninguna validación estricta de prefijos de función.

**nivel 1.2:** Azure Stream Analytics tiene una validación estricta de prefijos de función. Agregar un prefijo a una función integrada, produce un error. Por ejemplo,`myprefix.ABS(…)` no se admite.

Agregar un prefijo a los agregados integrados también produce error. Por ejemplo, `myprefix.SUM(…)` no se admite.

Con el prefijo "system" para los resultados de las funciones definidas por el usuario en el error.

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>No permitir la matriz y objeto como propiedades de clave en el adaptador de salida de Cosmos DB

**Niveles anteriores:** Se admiten los tipos de matriz y objeto como una propiedad de clave.

**nivel 1.2:** Ya no se admiten los tipos de matriz y objeto como una propiedad de clave.

## <a name="compatibility-level-11"></a>Nivel de compatibilidad 1.1

En el nivel de compatibilidad 1.1 se introdujeron los siguientes cambios principales:

### <a name="service-bus-xml-format"></a>Formato XML de Bus de servicio

**1.0 nivel:** Azure Stream Analytics usaba DataContractSerializer, por lo que el contenido del mensaje incluía etiquetas XML. Por ejemplo: 

`@\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ "SensorId":"1", "Temperature":64\}\u0001`

**nivel de 1.1:** El contenido del mensaje contiene directamente la secuencia sin ninguna etiqueta adicional. Por ejemplo: `{ "SensorId":"1", "Temperature":64}`

### <a name="persisting-case-sensitivity-for-field-names"></a>Conservar minúsculas para nombres de campo

**1.0 nivel:** Los nombres de campo se cambiaban a minúsculas cuando los procesaba el motor de Azure Stream Analytics.

**nivel 1.1:** minúsculas se mantiene para nombres de campo cuando se procesan por el motor de Azure Stream Analytics.

> [!NOTE]
> La distinción entre mayúsculas y minúsculas persistente aún no está disponible para trabajos de Stream Analytics hospedados mediante un entorno de Edge. En consecuencia, todos los nombres de campo se convierten a minúsculas si el trabajo se hospeda en Edge.

### <a name="floatnandeserializationdisabled"></a>FloatNaNDeserializationDisabled

**1.0 nivel:** El comando CREATE TABLE no filtraba eventos con NaN (por ejemplo, Infinity, -Infinity) en una columna de tipo FLOAT, porque están fuera del intervalo documentado para esos números.

**nivel de 1.1:** El comando CREATE TABLE le permite especificar un esquema seguro. El motor de Stream Analytics valida que los datos se ajustan a este esquema. Con este modelo, el comando puede filtrar eventos con valores NaN.

### <a name="disable-automatic-upcast-for-datetime-strings-in-json"></a>Deshabilitar upcast automática para las cadenas de fecha y hora en JSON

**1.0 nivel:** El analizador JSON convertía automáticamente los valores de cadena con información de fecha, hora y zona al tipo DateTime y, después, los convertía a UTC. Este comportamiento dieron lugar a pérdida de información de zona horaria.

**nivel de 1.1:** Ya no existe la conversión automática de los valores de cadenas con información de fecha, hora y zona al tipo DateTime. Como resultado, se mantiene la información de zona horaria.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de las conexiones de entrada de Azure Stream Analytics](stream-analytics-troubleshoot-input.md)
* [Estado de los recursos de análisis Stream](stream-analytics-resource-health.md)
