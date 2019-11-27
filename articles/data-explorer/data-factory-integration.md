---
title: Integración de Azure Data Explorer con Azure Data Factory
description: En este tema, integre Azure Data Explorer con Azure Data Factory para usar las actividades de copia, búsqueda y comando
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: tomersh26
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/14/2019
ms.openlocfilehash: dd2b3bd584bb39810e0a5c9acde1a961330c273d
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74093230"
---
# <a name="integrate-azure-data-explorer-with-azure-data-factory"></a>Integración de Azure Data Explorer con Azure Data Factory

[Azure Data Factory](/azure/data-factory/) (ADF) es un servicio de integración de datos basado en la nube que le permite integrar diferentes almacenes de datos y realizar actividades en los datos. ADF le permite crear flujos trabajo controlados por datos y así orquestar y automatizar el movimiento y la transformación de los datos. Azure Data Explorer es uno de los [almacenes de datos admitidos](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) en Azure Data Factory. 

## <a name="azure-data-factory-activities-for-azure-data-explorer"></a>Actividades de Azure Data Factory para Azure Data Explorer

Varias integraciones disponibles con Azure Data Factory para los usuarios de Azure Data Explorer:

### <a name="copy-activity"></a>Actividad de copia
 
La actividad de copia de Azure Data Factory se usa para transferir datos entre almacenes de datos. Azure Data Explorer se admite como origen, donde los datos se copian desde Azure Data Explorer a cualquier almacén de datos compatible y un receptor, donde se copian los datos desde cualquier almacén de datos compatible con Azure Data Explorer. Para más información, consulte [copia de datos con Azure Data Explorer como origen o destino mediante Azure Data Factory](/azure/data-factory/connector-azure-data-explorer). y para ver un tutorial detallado, consulte [cargar datos de Azure Data Factory en Azure Data Explorer](data-factory-load-data.md).
Azure Data Explorer es compatible con Azure IR (Integration Runtime), se usa cuando se copian datos en Azure y se hospedan en modo IR autohospedado, que se usa cuando se copian datos desde o hacia almacenes de datos ubicados en un entorno local o en una red con control de acceso, como un Virtual Network de Azure. Para más información, consulte [cuál IR usar](/azure/data-factory/concepts-integration-runtime#determining-which-ir-to-use)
 
> [!TIP]
> Al utilizar la actividad de copia y crear un **servicio vinculado** o un **conjunto de datos**, seleccione el almacén de datos **Azure Data Explorer (Kusto)** y no el antiguo almacén de datos **Kusto**.  

### <a name="lookup-activity"></a>Actividad de búsqueda
 
La actividad de búsqueda se usa para ejecutar consultas en Azure Data Explorer. El resultado de la consulta se devolverá como la salida de la actividad de búsqueda y se puede usar en la siguiente actividad de canalización, tal y como se describe en la [documentación de búsqueda de ADF](/azure/data-factory/control-flow-lookup-activity#use-the-lookup-activity-result-in-a-subsequent-activity).  
Además del límite de tamaño de respuesta de 5.000 filas y 2 MB, la actividad también tiene un límite de tiempo de espera de consulta de 1 hora.

### <a name="command-activity"></a>Actividad de comando

La actividad de comando permite la ejecución de los [comandos de control](/azure/kusto/concepts/#control-commands) de Azure Data Explorer. A diferencia de las consultas, los comandos de control pueden modificar potencialmente datos o metadatos. Algunos de los comandos de control están destinados a ingerir datos en Azure Data Explorer, mediante comandos como `.ingest`o `.set-or-append`) o la copia de datos desde Azure Data Explorer a almacenes de datos externos mediante comandos como `.export`.
Para ver un tutorial detallado de la actividad de comandos, consulte [usar la actividad de comandos de Azure Data Factory para ejecutar comandos de control de Azure Data Explorer](data-factory-command-activity.md).  El uso de un comando de control para copiar datos puede, en ocasiones, ser una opción más rápida y más barata que la actividad de copia. Para determinar cuándo se debe utilizar la actividad de comando en lugar de la actividad de copia, consulte [seleccionar entre las actividades de copia y comando al copiar datos](#select-between-copy-and-azure-data-explorer-command-activities-when-copy-data).

### <a name="copy-in-bulk-from-a-database-template"></a>Copiar de forma masiva desde una plantilla de base de datos

La [Copia en bloque desde una base de datos a Azure Data Explorer mediante la plantilla de Azure Data Factory](data-factory-template.md) es una canalización de Azure Data Factory. La plantilla se usa para crear muchas canalizaciones por base de datos o por tabla para una copia de datos más rápida. 

## <a name="select-between-copy-and-azure-data-explorer-command-activities-when-copy-data"></a>Selección entre las actividades de copia y de comandos de Azure Data Explorer cuando se copian datos 

Esta sección le ayudará a seleccionar la actividad correcta para sus necesidades de copia de datos.

Al copiar datos desde o a Azure Data Explorer, hay dos opciones disponibles en Azure Data Factory:
* Actividad de copia.
* La actividad de comandos de Azure Data Explorer, que ejecuta uno de los comandos de control que transfieren datos en Azure Data Explorer. 

### <a name="copy-data-from-azure-data-explorer"></a>Obtención de datos de Azure Data Explorer
  
Puede copiar datos desde Azure Data Explorer mediante la actividad de copia o el comando [`.export`](/azure/kusto/management/data-export/). El comando `.export` ejecuta una consulta y, a continuación, exporta los resultados de la consulta. 

Vea la tabla siguiente para obtener una comparación de la actividad de copia y el comando `.export` para copiar datos desde Azure Data Explorer.

| | Actividad de copia | .export (comando) |
|---|---|---|
| **Descripción del flujo** | ADF ejecuta una consulta en Kusto, procesa el resultado y lo envía al almacén de datos de destino. <br>(**ADX > ADF > almacén de datos receptor**) | ADF envía un comando de control `.export` a Azure Data Explorer, que ejecuta el comando, y envía los datos directamente al almacén de datos de destino. <br>(**ADX > almacén de datos receptor**) |
| **Almacenes de datos de destino compatibles** | Una amplia variedad de [almacenes de datos compatibles](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) | ADLSv2, Blob de Azure, SQL Database |
| **Rendimiento** | Centralizado | <ul><li>Distribuido (predeterminado), exportación de datos de varios nodos simultáneamente</li><li>Más rápido y COGS eficientes.</li></ul> |
| **Límites del servidor** | [Límites de consulta](/azure/kusto/concepts/querylimits) pueden extenderse o deshabilitarse. De forma predeterminada, las consultas de ADF contienen: <ul><li>Límite de tamaño de 500.000 registros o 64 MB.</li><li>Límite de tiempo de 10 minutos.</li><li>`noTruncation` establecida en Falso.</li></ul> | De forma predeterminada, extiende o deshabilita los límites de la consulta: <ul><li>Los límites de tamaño están deshabilitados.</li><li>El tiempo de espera del servidor se extiende a 1 hora.</li><li>`MaxMemoryConsumptionPerIterator` y `MaxMemoryConsumptionPerQueryPerNode` se extienden a Max (5 GB, TotalPhysicalMemory/2).</li></ul>

> [!TIP]
> Si el destino de la copia es uno de los almacenes de datos admitidos por el comando `.export`, y si ninguna de las características de la actividad de copia es fundamental para sus necesidades, seleccione el comando `.export`.

### <a name="copying-data-to-azure-data-explorer"></a>Copia de datos a Azure Data Explorer

Puede copiar datos a Azure Data Explorer mediante los comandos de actividad de copia o ingesta, como [ingesta de consultas ](/azure/kusto/management/data-ingestion/ingest-from-query) (`.set-or-append`, `.set-or-replace`, `.set`, `.replace)`y [la ingesta](/azure/kusto/management/data-ingestion/ingest-from-storage) de almacenamiento (`.ingest`). 

Vea la tabla siguiente para obtener una comparación de la actividad de copia y los comandos de ingesta para copiar datos desde Azure Data Explorer.

| | Actividad de copia | Ingesta desde consulta<br> `.set-or-append` / `.set-or-replace` / `.set` / `.replace` | Ingesta desde almacenamiento <br> `.ingest` |
|---|---|---|---|
| **Descripción del flujo** | ADF obtiene los datos del almacén de datos de origen, los convierte en un formato tabular y realiza los cambios de asignación de esquema necesarios. Después, ADF carga los datos en blobs de Azure, los divide en fragmentos y luego descarga los blobs para ingerirlos en la tabla ADX. <br> (**Almacén de datos de origen > ADF > blobs de Azure > ADX**) | Estos comandos pueden ejecutar una consulta o un comando de `.show` e ingerir los resultados de la consulta en una tabla (**ADX > ADX**). | Este comando ingiere datos en una tabla mediante la "extracción" de los datos de uno o varios artefactos de almacenamiento en la nube. |
| **Almacenes de datos de origen admitidos** |  [variedad de opciones](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats) | ADLS Gen 2, Azure BLOB, SQL (con el complemento sql_request), Cosmos (con el complemento cosmosdb_sql_request) y cualquier otro almacén de datos que proporcione las API HTTP o Python. | Filesystem, Azure Blob Storage, ADLS gen 1, ADLS Gen 2 |
| **Rendimiento** | Las ingestas se ponen en cola y se administran, lo que garantiza la ingesta de pequeño tamaño y garantiza una alta disponibilidad al proporcionar equilibrio de carga, reintentos y control de errores. | <ul><li>Estos comandos no se diseñaron para la importación de datos de gran volumen.</li><li>Funciona según lo previsto y más barato. Pero para los escenarios de producción y cuando las tasas de tráfico y los tamaños de datos son grandes, use la actividad de copia.</li></ul> |
| **Límites del servidor** | <ul><li>Sin límite de tamaño.</li><li>Límite máximo de tiempo de espera: 1 hora por ingesta de blob. |<ul><li>Solo hay un límite de tamaño en la parte de la consulta, que se puede omitir especificando `noTruncation=true`.</li><li>Límite máximo de tiempo de espera: 1 hora.</li></ul> | <ul><li>Sin límite de tamaño.</li><li>Límite máximo de tiempo de espera: 1 hora.</li></ul>|

> [!TIP]
> * Al copiar datos desde ADF a Azure Data Explorer use los comandos `ingest from query`.  
> * Para conjuntos de datos grandes (>1 GB), use la actividad de copia.  

## <a name="required-permissions"></a>Permisos necesarios

En la tabla siguiente se enumeran los permisos necesarios para varios pasos de la integración con Azure Data Factory.

| Paso | Operación | Nivel mínimo de permisos | Notas |
|---|---|---|---|
| **Creación de un servicio vinculado** | Navegación de la base de datos | *Visor de base de datos* <br>El usuario que ha iniciado sesión con ADF debe estar autorizado para leer los metadatos de la base de datos. | El usuario puede proporcionar el nombre de la base de datos manualmente. |
| | Probar la conexión | *Monitor de base de datos* o *tabla de ingesta* <br>La entidad de servicio debe estar autorizada para ejecutar los comandos `.show` de nivel de base de datos o la ingesta en el nivel de tabla. | <ul><li>TestConnection comprueba la conexión al clúster y no a la base de datos. Puede realizarse incluso si la base de datos no existe.</li><li>Los permisos de administrador de tablas no son suficientes.</li></ul>|
| **Crear un conjunto de datos** | Navegación por tabla | *Supervisor de base de datos* <br>El usuario que ha iniciado sesión con ADF debe estar autorizado para ejecutar los comandos `.show` de nivel de base de datos. | El usuario puede proporcionar el nombre de la tabla manualmente.|
| **Crear un conjunto de datos** o una **Actividad de copia** | Vista previa de los datos | *Visor de base de datos* <br>La entidad de servicio debe estar autorizada para leer los metadatos de la base de datos. | | 
|   | Importar esquema | *Visor de base de datos* <br>La entidad de servicio debe estar autorizada para leer los metadatos de la base de datos. | Cuando ADX es el origen de un tabular a una copia tabular, ADF importará el esquema automáticamente, incluso si el usuario no ha importado el esquema explícitamente. |
| **ADX como receptor** | Crear de una asignación de columna por nombre | *Supervisor de base de datos* <br>La entidad de servicio debe estar autorizada para ejecutar los comandos `.show` nivel de base de datos. | <ul><li>Todas las operaciones obligatorias funcionarán con la *ingesta de tablas*.</li><li> Se pueden producir errores en algunas operaciones opcionales.</li></ul> |
|   | <ul><li>Crear una asignación de CSV en la tabla</li><li>Quitar la asignación</li></ul>| *ingesta de tablas* o *administrador de base de datos* <br>La entidad de servicio debe estar autorizada para realizar cambios en una tabla. | |
|   | Introducción de datos | *ingesta de tablas* o *administrador de base de datos* <br>La entidad de servicio debe estar autorizada para realizar cambios en una tabla. | | 
| **ADX como origen** | Ejecutar consulta | *Visor de base de datos* <br>La entidad de servicio debe estar autorizada para leer los metadatos de la base de datos. | |
| **Comando Kusto** | | Según el nivel de permisos de cada comando. |

## <a name="performance"></a>Rendimiento 

Si Azure Data Explorer es el origen y usa la actividad de búsqueda, copia o comando que contiene una consulta, consulte [prácticas recomendadas de consulta](/azure/kusto/query/best-practices) para obtener información sobre el rendimiento y [documentación de ADF para la actividad de copia](/azure/data-factory/copy-activity-performance).
  
En esta sección se aborda el uso de la actividad de copia donde Azure Data Explorer es el receptor. El rendimiento estimado del receptor de Azure Data Explorer es de 11-13 MBps. En la tabla siguiente se detallan los parámetros que influyen en el rendimiento del receptor de Azure Data Explorer.

| Parámetro | Notas |
|---|---|
| **Proximidad geográfica de componentes** | Colocar todos los componentes en la misma región:<ul><li>almacenes de datos de origen y receptor.</li><li>Entorno de ejecución de integración ADF</li><li>Su clúster de ADX.</li></ul>Asegúrese de que al menos su entorno de ejecución de integración está en la misma región que el clúster de ADX. |
| **Número de DIU** | 1 VM por cada 4 DIU que usa ADF. <br>El aumento de DIU solo le ayudará si el origen es un almacén basado en archivos con varios archivos. Cada VM procesará un archivo diferente en paralelo. Por lo tanto, la copia de un solo archivo grande tendrá una latencia mayor que copiar varios archivos más pequeños.|
|**Cantidad y SKU del clúster de ADX** | Un gran número de nodos ADX aumentará el tiempo de procesamiento de la ingesta.|
| **Paralelismo** | Para copiar una cantidad muy grande de datos de una base de datos, cree particiones de los datos y, a continuación, use un bucle ForEach que copie cada partición en paralelo o use la [Copia masiva desde la base de datos a la plantilla de Azure Data Explorer](data-factory-template.md). Nota: **La configuración** > **Grado de paralelismo** en la actividad de copia no es relevante para ADX. |
| **Complejidad del procesamiento de datos** | La latencia varía según el formato del archivo de origen, la asignación de columnas y la compresión.|
| **La VM que ejecuta su entorno de ejecución de integración** | <ul><li>En el caso de la copia de Azure, las máquinas virtuales de ADF y máquinas de SKU no se pueden cambiar.</li><li> En el caso de la copia local a Azure, determine que la VM que hospeda la instancia de IR autohospedada es suficientemente segura.</li></ul>|

## <a name="monitor-activity-progress"></a>Supervise el progreso de la actividad

* Al supervisar el progreso de la actividad, la propiedad *datos escritos* puede ser mucho más grande que la propiedad *lectura de datos* porque *la lectura de datos* se calcula de acuerdo con el tamaño del archivo binario, mientras que *los datos escritos* se calculan de acuerdo con el tamaño en memoria, después de que los datos se deserializan y descomprimen.

* Al supervisar el progreso de la actividad, puede ver que los datos se escriben en el receptor de Azure Data Explorer. Al consultar la tabla de Azure Data Explorer, verá que los datos no han llegado. Esto se debe a que hay dos fases al copiar a Azure Data Explorer. 
    * La primera fase lee los datos de origen, los divide en fragmentos de 900 MB y carga cada fragmento en un BLOB de Azure. La primera fase se ve en la vista progreso de la actividad de ADF. 
    * La segunda fase comienza una vez que todos los datos se cargan en los blobs de Azure. Los nodos del motor de Azure Data Explorer descargan los blobs e ingieren los datos en la tabla del receptor. Los datos se verán en la tabla de Azure Data Explorer.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [copiar datos en Azure Data Explorer mediante Azure Data Factory](data-factory-load-data.md).
* Más información sobre el uso de [plantillas de Azure Data Factory para la copia masiva de la base de datos en Azure Data Explorer](data-factory-template.md).
* Obtenga información sobre el uso de la [actividad de comandos de Azure Data Factory para ejecutar comandos de control de Azure Data Explorer](data-factory-command-activity.md).
* Obtenga más información sobre las [consultas de Azure Data Explorer](/azure/data-explorer/web-query-data) para las consultas de datos.



