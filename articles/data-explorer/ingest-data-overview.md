---
title: Ingesta de datos de Azure Data Explorer
description: Información acerca de las diferentes maneras que puede usar para la ingesta (carga) de datos en Azure Data Explorer
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f1df22c505bffdfaf60bf9c6eec3ad4e698fff02
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139533"
---
# <a name="azure-data-explorer-data-ingestion"></a>Ingesta de datos de Azure Data Explorer

La ingesta de datos es el proceso que se usa para cargar los registros de datos de uno o más orígenes para crear o actualizar una tabla en Azure Data Explorer. Una vez ingeridos, los datos están disponibles para su consulta. El diagrama siguiente muestra el flujo de un extremo a otro para trabajar en Azure Data Explorer, incluida la ingesta de datos **(2)**.

![Flujo de datos global](media/ingest-data-overview/overall-data-flow.png)

El servicio de administración de datos Azure Data Explorer, que es el responsable de la ingesta de datos, proporciona las siguientes funcionalidades:

1. **Extracción de datos**: extracción de datos de orígenes externos (Event Hubs) o lectura de las solicitudes de ingesta de una cola de Azure.

1. **Procesamiento por lotes**: procesamiento por lotes de datos que fluyen a la misma base de datos y tabla para optimizar el rendimiento de la ingesta de datos.

1. **Validación**: validación preliminar y conversión de formato, si es necesario.

1. **Manipulación de datos**: esquema coincidente, organización, indexación, codificación y compresión de los datos.

1. **Punto de persistencia en el flujo de ingesta**: administración de la carga de ingesta en el motor y control de los reintentos tras los errores transitorios.

1. **Confirmación de los datos de ingesta**: hace que los datos estén disponibles para consulta.

> [!NOTE]
> La directiva de retención efectiva de los datos ingeridos se deriva de la directiva de retención de la base de datos. Para información más detallada consulte [Directiva de retención](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy). La ingesta de datos requiere permisos de **Agente de ingesta de tablas** o de **Agente de ingesta de base de datos**.

## <a name="ingestion-methods"></a>Métodos de ingesta

Azure Data Explorer admite varios métodos de ingesta, cada uno con sus propios escenarios de destino, ventajas y desventajas. Azure Data Explorer ofrece conectores para servicios comunes, ingesta mediante programación utilizando los SDK y acceso directo al motor para fines de exploración.

### <a name="ingestion-using-connectors"></a>Ingesta mediante conectores

En la actualidad Azure Data Explorer es compatible con el conector del centro de eventos, que se puede administrar utilizando el asistente para administración en Azure Portal. Para más información consulte [Guía de inicio rápido: Ingesta de datos desde el centro de eventos en Azure Data Explorer](ingest-data-event-hub.md).

### <a name="programmatic-ingestion"></a>Ingesta mediante programación

Azure Data Explorer proporciona SDK que pueden usarse para la consulta e ingesta de datos. La ingesta mediante programación está optimizada para reducir los costos de ingesta (COG), minimizando las transacciones de almacenamiento durante y después del proceso de ingesta.

**SDK disponibles y proyectos de código abierto**:

Kusto ofrece un cliente SDK que puede utilizarse para la ingesta y consulta de datos con:

* [SDK de Python](https://docs.microsoft.com/azure/kusto/api/python/kusto-python-client-library)

* [SDK de .NET](https://docs.microsoft.com/azure/kusto/api/netfx/about-the-sdk)

* [SDK de Java](https://docs.microsoft.com/azure/kusto/api/java/kusto-java-client-library)

* [SDK de Node]

* [API DE REST](https://docs.microsoft.com/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Técnicas de ingesta mediante programación**:

* Ingesta de datos mediante el servicio de administración de datos Azure Data Explorer (ingesta confiable y de alto rendimiento)

  * [**Ingesta de lotes**](https://docs.microsoft.com/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (proporcionada por el SDK): el cliente carga los datos en Azure Blob Storage (designado por el servicio de administración de datos Azure Data Explorer) y envía una notificación a una cola de Azure. Esta es la técnica recomendada para la ingesta confiable y económica de datos de gran volumen.

* Ingesta de datos directamente en el motor de Azure Data Explorer (más adecuado para la exploración y la creación de prototipos):

  * **Ingesta insertada**: comando de control (.ingest inline) que contiene los datos en banda, está pensado para fines de pruebas ad hoc.

  * **Ingesta de consulta**: comando de control (.set, .set-or-append, .set-or-replace) que señala a los resultados de la consulta, se usa para generar informes o pequeñas tablas temporales.

  * **Ingesta de almacenamiento**: comando de control (.ingest into) con datos almacenados externamente (por ejemplo, Azure Blob Storage) permite la ingesta en masa de datos de forma eficaz.

**Latencia de los distintos métodos**:

| Método | Latencia |
| --- | --- |
| **Ingesta insertada** | Inmediata |
| **Ingesta de consulta** | Tiempo de consulta + tiempo de procesamiento |
| **Ingesta de almacenamiento** | Tiempo de descarga + tiempo de procesamiento |
| **Ingesta en cola** | Tiempo del procesamiento por lotes + tiempo de procesamiento |
| |

El tiempo de procesamiento depende del tamaño de los datos, normalmente es menos de unos segundos. El tiempo del procesamiento por lotes tiene un valor predeterminado de 5 minutos.

## <a name="choosing-the-most-appropriate-ingestion-method"></a>Selección del método más adecuado de ingesta

Antes de empezar con la ingesta de datos, debe plantearse las siguientes preguntas.

* ¿Dónde residen mis datos? 
* ¿Cuál es el formato de los datos y es posible cambiarlo? 
* ¿Cuáles son los campos obligatorios que se van a consultar? 
* ¿Cuál es el volumen de datos y la velocidad que se esperan? 
* ¿Cuántos tipos de eventos se esperan (reflejados como número de tablas)? 
* ¿Con qué frecuencia se espera que cambie el esquema de eventos? 
* ¿Cuántos nodos generarán los datos? 
* ¿Cuál es el SO del origen? 
* ¿Cuáles son los requisitos de latencia? 
* ¿Puede utilizarse una de las canalizaciones de ingesta administrada existentes? 

Para organizaciones con una infraestructura existente basada en un servicio de mensajería, como Event Hub, el uso de un conector es probablemente la solución más adecuada. La ingesta en cola es apropiada para grandes volúmenes de datos.

## <a name="supported-data-formats"></a>Formatos de datos compatibles

Para todos los métodos de ingesta, salvo la ingesta de consulta, los datos tienen que tener uno de los formatos de datos compatibles para que Azure Data Explorer puede analizarlos.

* CSV, TSV, PSV, SCSV, SOH
* JSON (separados por línea, multilínea), Avro
* ZIP y GZIP 

> [!NOTE]
> Cuando se produce la ingesta de datos, los tipos de datos se infieren basándose en las columnas de tabla de destino. Si un registro está incompleto o un campo no se puede analizar como tipo el de datos necesarios, las columnas de tabla correspondientes se rellenará con valores nulos.

## <a name="schema-mapping"></a>Asignación de esquemas

La asignación de esquemas, ayuda a enlazar de forma determinista campos de datos de origen a columnas de tabla de destino.

* [Asignación de CSV](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#csv-mapping) (opcional) funciona con todos los formatos basados en ordinales y se puede pasar como el parámetro de comando de ingesta o se puede [crear previamente en la tabla](https://docs.microsoft.com/azure/kusto/management/tables?branch=master#create-ingestion-mapping) y hacerle referencia desde el parámetro de comando de ingesta.
* [Asignación de JSON](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#json-mapping) (obligatorio) y [asignación de Avro](https://docs.microsoft.com/azure/kusto/management/tables#create-ingestion-mapping) (obligatorio) se puede pasar como el parámetro de comando de ingesta o se puede [crear previamente en la tabla](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#avro-mapping) y hacerle referencia desde el parámetro de comando de ingesta.

## <a name="next-steps"></a>Pasos siguientes

[Guía de inicio rápido: Ingesta de datos desde el centro de eventos en Azure Data Explorer](ingest-data-event-hub.md)

[Guía de inicio rápido: Ingesta de datos mediante la biblioteca de Python de Azure Data Explorer](python-ingest-data.md)

