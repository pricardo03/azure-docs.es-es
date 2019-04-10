---
title: Nivel de compatibilidad de los trabajos de Azure Stream Analytics
description: Obtenga información acerca de cómo establecer un nivel de compatibilidad para un trabajo de análisis de Azure Stream Analytics y de los principales cambios en el nivel de compatibilidad más reciente.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/08/2019
ms.openlocfilehash: 6fb93152263d253de983b17d25f02f4c68a172fd
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361391"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Nivel de compatibilidad de los trabajos de Azure Stream Analytics
 
El nivel de compatibilidad se refiere a los comportamientos específicos de la versión de un servicio de Azure Stream Analytics. Azure Stream Analytics es un servicio administrado, con actualizaciones regulares de características y mejoras de rendimiento. Normalmente, las actualizaciones estarán disponibles automáticamente para los usuarios finales. Sin embargo, algunas características nuevas pueden introducir cambios importantes, como cambios en el comportamiento de un trabajo existente, cambios en los procesos que consumen datos de estos trabajos, etc. Un nivel de compatibilidad se utiliza para representar un cambio introducido en Stream Analytics. Los cambios importantes siempre se introducen con un nuevo nivel de compatibilidad. 

Nivel de compatibilidad garantiza que los trabajos existentes se ejecuten sin errores. Cuando se crea un nuevo trabajo de Stream Analytics, es una práctica recomendada para crearlo mediante el nivel de compatibilidad más reciente. 
 
## <a name="set-a-compatibility-level"></a>Establecer un nivel de compatibilidad 

Los niveles de compatibilidad controlan el comportamiento en tiempo de ejecución de un trabajo de Stream Analytics. Puede establecer el nivel de compatibilidad de un trabajo de Stream Analytics desde el portal o con la [llamada de API de REST para crear un trabajo](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Actualmente, Azure Stream Analytics admite dos niveles de compatibilidad "1.0" y "1.1". De forma predeterminada, el nivel de compatibilidad se establece en "1.0", que se introdujo durante la disponibilidad general de Azure Stream Analytics. Para actualizar el valor predeterminado, vaya a su trabajo de Stream Analytics > seleccione la opción **Nivel de compatibilidad** en la sección **Configurar** y cambie el valor. 

Asegúrese de detener el trabajo antes de actualizar el nivel de compatibilidad. No se puede actualizar el nivel de compatibilidad si el trabajo está en un estado de ejecución. 

![Nivel de compatibilidad de Stream Analytics en Azure Portal](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

 
Cuando se actualiza el nivel de compatibilidad, el compilador de T-SQL valida el trabajo con la sintaxis que se corresponde con el nivel de compatibilidad seleccionado. 

## <a name="major-changes-in-the-latest-compatibility-level-12"></a>Cambios importantes en el nivel de compatibilidad más reciente (1.2)

Se introducen los siguientes cambios principales en el nivel de compatibilidad 1.2:

### <a name="geospatial-functions"></a>Funciones geoespaciales 

**Versiones anteriores:** Azure Stream Analytics usa los cálculos geográficos.

**Versión actual:** Azure Stream Analytics permite calcular las coordenadas geográficas proyectado geométrica. No hay ningún cambio en la firma de las funciones geoespaciales. Sin embargo, su semántica es ligeramente diferente, lo que permite un cálculo más preciso que antes.

Azure Stream Analytics admite la indexación de datos de referencia geoespacial. Datos de referencia que contiene elementos geoespaciales se pueden indizar para un cálculo de combinación más rápido.

Las funciones geoespaciales actualizada traer la expresividad del formato de Well Known Text (WKT) geoespaciales completa. Puede especificar otros componentes geoespaciales que anteriormente no eran compatibles con GeoJson.

Para obtener más información, consulte [actualiza a funciones geoespaciales en Azure Stream Analytics: en la nube y IoT Edge](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/).

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>Ejecución de consultas en paralelo para los orígenes de entrada con varias particiones 

**Versiones anteriores:** Consultas de Azure Stream Analytics requerían el uso de la cláusula PARTITION BY para paralelizar el procesamiento de consultas entre particiones de origen de entrada.

**Versión actual:** Si la lógica de la consulta se puede ejecutar en paralelo entre las particiones de origen de entrada, Azure Stream Analytics crea instancias de consulta independiente y ejecuta los cálculos en paralelo.

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>Integración nativa de API de forma masiva con la salida de CosmosDB

**Versiones anteriores:** El comportamiento de upsert era *insertar o combinar*.

**Versión actual:** Integración nativa de API de forma masiva con la salida de CosmosDB maximiza el rendimiento y se controla con gran eficacia la limitación de solicitudes.

Es el comportamiento de upsert *insertar o reemplazar*.

### <a name="datetimeoffset-when-writing-to-sql-output"></a>DateTimeOffset al escribir en la salida de SQL

**Versiones anteriores:** [DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017) tipos se ajustan a la hora UTC.

**Versión actual:** Ya no se ajusta DateTimeOffset.

### <a name="strict-validation-of-prefix-of-functions"></a>Validación estricta de prefijo de funciones

**Versiones anteriores:** No se produjo ninguna validación estricta de prefijos de función.

**Versión actual:** Azure Stream Analytics tiene una validación estricta de prefijos de función. Agregar un prefijo a una función integrada, produce un error. Por ejemplo,`myprefix.ABS(…)` no se admite.

Agregar un prefijo a los agregados integrados también produce error. Por ejemplo, `myprefix.SUM(…)` no se admite.

Con el prefijo "system" para los resultados de las funciones definidas por el usuario en el error.

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>No permitir la matriz y objeto como propiedades de clave en el adaptador de salida de Cosmos DB

**Versiones anteriores:** Se admiten los tipos de matriz y objeto como una propiedad de clave.

**Versión actual:** Ya no se admiten los tipos de matriz y objeto como una propiedad de clave.


## <a name="next-steps"></a>Pasos siguientes
* [Solución de problemas de las entradas de Azure Stream Analytics](stream-analytics-troubleshoot-input.md)
* [Estado de los recursos de análisis Stream](stream-analytics-resource-health.md)
