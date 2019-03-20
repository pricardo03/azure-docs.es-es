---
title: Métricas compatibles de Azure Monitor por tipo de recurso
description: Lista de métricas disponibles para cada tipo de recurso con Azure Monitor.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 09/14/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: f7bfb4f403104bb91fb1a9ba4b70cb164e0738b4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58113306"
---
# <a name="supported-metrics-with-azure-monitor"></a>Métricas compatibles con Azure Monitor
Azure Monitor proporciona varias maneras de interactuar con las métricas, como la representación en gráficos en el portal, el acceso a ellas a través de la API de REST o consultarlas con PowerShell o la CLI. A continuación se muestra una lista completa de todas las métricas disponibles actualmente en la canalización de métricas de Azure Monitor. Otras métricas pueden estar disponibles en el portal o mediante las API heredadas. La siguiente lista incluye solo las métricas disponibles con la canalización de métricas consolidada de Azure Monitor. Para consultar estas métricas y acceder a ellas, use [2018-01-01 api-version](https://docs.microsoft.com/rest/api/monitor/metricdefinitions).

> [!NOTE]
> Actualmente no se admite el envío de métricas de varias dimensiones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
>
> *Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representará con todos los mensajes entrantes de todas las colas de Event Hub.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|qpu_metric|QPU|Número|Media|QPU. Intervalo de 0-100 para S1, 0-200 para S2 y 0-400 para S4|ServerResourceType|
|memory_metric|Memoria|Bytes|Media|Memoria. Intervalo de 0-25 GB para S1, 0-50 GB para S2 y 0-100 GB para S4|ServerResourceType|
|TotalConnectionRequests|Número total de solicitudes de conexión|Número|Media|Número total de solicitudes de conexión. Se trata de llegadas.|ServerResourceType|
|SuccessfullConnectionsPerSec|Conexiones correctas por segundo|CountPerSecond|Media|Tasa de finalizaciones de conexión correctas.|ServerResourceType|
|TotalConnectionFailures|Número total de errores de conexión|Número|Media|Número total de intentos de conexión con error.|ServerResourceType|
|CurrentUserSessions|Sesiones de usuario actuales|Número|Media|Número actual de sesiones de usuario establecidas.|ServerResourceType|
|QueryPoolBusyThreads|Subprocesos ocupados de grupo de consultas|Número|Media|Número de subprocesos ocupados del grupo de subprocesos de consulta.|ServerResourceType|
|CommandPoolJobQueueLength|Longitud de cola de trabajos de grupo de comandos|Número|Media|Número de trabajos en la cola del grupo de subprocesos de comandos.|ServerResourceType|
|ProcessingPoolJobQueueLength|Longitud de cola de trabajos de grupo de procesamiento|Número|Media|Número de trabajos que no son de E/S en la cola del grupo de subprocesos de procesamiento.|ServerResourceType|
|CurrentConnections|Conexión: Conexiones actuales|Número|Media|Número actual de conexiones de cliente establecidas.|ServerResourceType|
|CleanerCurrentPrice|Memoria: precio actual de limpieza|Número|Media|Precio actual de memoria, USD/byte/tiempo, normalizado a 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Memoria: memoria de limpieza reducible|Bytes|Media|Cantidad de memoria, en bytes, que purgará el limpiador en segundo plano.|ServerResourceType|
|CleanerMemoryNonshrinkable|Memoria: memoria de limpieza no reducible|Bytes|Media|Cantidad de memoria, en bytes, que no purgará el limpiador en segundo plano.|ServerResourceType|
|MemoryUsage|Memoria: Uso de la memoria|Bytes|Media|Uso de memoria del proceso de servidor tal como se usa para calcular el precio de la memoria del limpiador. Es igual al contador Proceso\Bytes privados más el tamaño de los datos asignados a la memoria; se ignora la memoria que ha asignado VertiPaq superior al límite de memoria de VertiPaq.|ServerResourceType|
|MemoryLimitHard|Memoria: límite de memoria física|Bytes|Media|Límite de memoria física del archivo de configuración.|ServerResourceType|
|MemoryLimitHigh|Memoria: límite alto de memoria|Bytes|Media|Límite alto de memoria del archivo de configuración.|ServerResourceType|
|MemoryLimitLow|Memoria: límite bajo de memoria|Bytes|Media|Límite bajo de memoria del archivo de configuración.|ServerResourceType|
|MemoryLimitVertiPaq|Memoria: VertiPaq de límite de memoria|Bytes|Media|Límite en memoria del archivo de configuración.|ServerResourceType|
|Quota|Memoria: Quota|Bytes|Media|Cuota de memoria actual, en bytes. La cuota de memoria también se denomina concesión de memoria o reserva de memoria.|ServerResourceType|
|QuotaBlocked|Memoria: cuota bloqueada|Número|Media|Número actual de solicitudes de cuota que están bloqueadas hasta que se liberen otras cuotas de memoria.|ServerResourceType|
|VertiPaqNonpaged|Memoria: VertiPaq no paginado|Bytes|Media|Bytes de memoria bloqueados en el espacio de trabajo para que los use el motor en memoria.|ServerResourceType|
|VertiPaqPaged|Memoria: VertiPaq paginado|Bytes|Media|Bytes de memoria paginada en uso para datos en memoria.|ServerResourceType|
|RowsReadPerSec|Procesamiento: filas leídas por segundo|CountPerSecond|Media|Velocidad de filas leídas de todas las bases de datos relacionales.|ServerResourceType|
|RowsConvertedPerSec|Procesamiento: filas convertidas por segundo|CountPerSecond|Media|Velocidad de filas convertidas durante el procesamiento.|ServerResourceType|
|RowsWrittenPerSec|Procesamiento: filas escritas por segundo|CountPerSecond|Media|Velocidad de filas escritas durante el procesamiento.|ServerResourceType|
|CommandPoolBusyThreads|Subprocesos: subprocesos ocupados del grupo de comandos|Número|Media|Número de subprocesos ocupados del grupo de subprocesos de comandos.|ServerResourceType|
|CommandPoolIdleThreads|Subprocesos: subprocesos inactivos del grupo de comandos|Número|Media|Número de subprocesos inactivos del grupo de subprocesos de comandos.|ServerResourceType|
|LongParsingBusyThreads|Subprocesos: subprocesos ocupados en análisis largos|Número|Media|Número de subprocesos ocupados del grupo de subprocesos de análisis largos.|ServerResourceType|
|LongParsingIdleThreads|Subprocesos: subprocesos inactivos en análisis largos|Número|Media|Número de subprocesos inactivos del grupo de subprocesos en análisis largos.|ServerResourceType|
|LongParsingJobQueueLength|Subprocesos: longitud de cola de trabajos en análisis largos|Número|Media|Número de trabajos en la cola del grupo de subprocesos en análisis largos.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Subprocesos: subprocesos de trabajo de E/S ocupados del grupo de procesamiento|Número|Media|Número de subprocesos que ejecutan trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Subprocesos: subprocesos de trabajo ocupados que no son de E/S del grupo de procesamiento|Número|Media|Número de subprocesos que ejecutan trabajos que no son de E/S del grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Subprocesos: Longitud de cola de trabajos de E/S de grupo de procesamiento|Número|Media|Número de trabajos de E/S en la cola del grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Subprocesos: subprocesos de trabajo de E/S inactivos del grupo de procesamiento|Número|Media|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Subprocesos: subprocesos de trabajo inactivos que no son de E/S del grupo de procesamiento|Número|Media|Número de subprocesos inactivos del grupo de subprocesos de procesamiento dedicado a trabajos que no son de E/S.|ServerResourceType|
|QueryPoolIdleThreads|Subprocesos: subprocesos inactivos del grupo de consultas|Número|Media|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|QueryPoolJobQueueLength|Subprocesos: longitud de cola de trabajos del grupo de consultas|Número|Media|Número de trabajos en la cola del grupo de subprocesos de consultas.|ServerResourceType|
|ShortParsingBusyThreads|Subprocesos: subprocesos ocupados en análisis cortos|Número|Media|Número de subprocesos ocupados del grupo de subprocesos de análisis cortos.|ServerResourceType|
|ShortParsingIdleThreads|Subprocesos: subprocesos inactivos en análisis cortos|Número|Media|Número de subprocesos inactivos del grupo de subprocesos de análisis cortos.|ServerResourceType|
|ShortParsingJobQueueLength|Subprocesos: longitud de cola de trabajos en análisis cortos|Número|Media|Número de trabajos en la cola del grupo de subprocesos en análisis cortos.|ServerResourceType|
|memory_thrashing_metric|Paginación excesiva de memoria|Percent|Media|Paginación excesiva media de memoria.|ServerResourceType|
|mashup_engine_qpu_metric|QPU de motor M|Número|Media|Uso de QPU por los procesos del motor de mashup|ServerResourceType|
|mashup_engine_memory_metric|Memoria del motor M|Bytes|Media|Uso de memoria por los procesos del motor de mashup|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TotalRequests|Solicitudes de puerta de enlace en total|Número|Total|Número de solicitudes de puerta de enlace|Ubicación, Nombre de host|
|SuccessfulRequests|Solicitudes de puerta de enlace correctas|Número|Total|Número de solicitudes de puerta de enlace correctas|Ubicación, Nombre de host|
|UnauthorizedRequests|Solicitudes de puerta de enlace no autorizadas|Número|Total|Número de solicitudes de puerta de enlace no autorizadas|Ubicación, Nombre de host|
|FailedRequests|Solicitudes de puerta de enlace con error|Número|Total|Número de errores en solicitudes de puerta de enlace|Ubicación, Nombre de host|
|OtherRequests|Otras solicitudes de puerta de enlace|Número|Total|Número de otras solicitudes de puerta de enlace|Ubicación, Nombre de host|
|Duration|Duración total de las solicitudes de puerta de enlace|Milisegundos|Media|Duración total de las solicitudes de puerta de enlace en milisegundos|Ubicación, Nombre de host|
|Capacity|Capacity|Percent|Media|Métrica de uso para el servicio ApiManagement|Ubicación|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TotalJob|Número total de trabajos|Número|Total|El número total de trabajos (jobs)|Runbook, Estado|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CoreCount|Recuento de núcleos dedicados|Número|Total|Número total de núcleos dedicados de la cuenta de Batch|Sin dimensiones|
|TotalNodeCount|Recuento de nodos dedicados|Número|Total|Número total de nodos dedicados de la cuenta de Batch|Sin dimensiones|
|LowPriorityCoreCount|Recuento de núcleos de baja prioridad|Número|Total|Número total de núcleos de baja prioridad de la cuenta de Batch|Sin dimensiones|
|TotalLowPriorityNodeCount|Recuento de nodos de baja prioridad|Número|Total|Número total de nodos de baja prioridad de la cuenta de Batch|Sin dimensiones|
|CreatingNodeCount|Recuento de nodos creados|Número|Total|Número de nodos que se van a crear|Sin dimensiones|
|StartingNodeCount|Recuento de nodos de inicio|Número|Total|Número de nodos que se van a iniciar|Sin dimensiones|
|WaitingForStartTaskNodeCount|Recuento de nodos esperando a la tarea de inicio|Número|Total|Número de nodos que esperan a que se complete la tarea de inicio|Sin dimensiones|
|StartTaskFailedNodeCount|Recuento de nodos con error de la tarea de inicio|Número|Total|Número de nodos con error en la tarea de inicio|Sin dimensiones|
|IdleNodeCount|Recuento de nodos inactivos|Número|Total|Número de nodos inactivos|Sin dimensiones|
|OfflineNodeCount|Recuento de nodos sin conexión|Número|Total|Número de nodos sin conexión|Sin dimensiones|
|RebootingNodeCount|Recuento de nodos de reinicio|Número|Total|Número de nodos de reinicio|Sin dimensiones|
|ReimagingNodeCount|Recuento de nodos de restablecimiento de imagen inicial|Número|Total|Número de nodos de restablecimiento de imagen inicial|Sin dimensiones|
|RunningNodeCount|Recuento de nodos en ejecución|Número|Total|Número de nodos en ejecución|Sin dimensiones|
|LeavingPoolNodeCount|Recuento de nodos que salen del grupo|Número|Total|Número de nodos que abandonan el grupo|Sin dimensiones|
|UnusableNodeCount|Recuento de nodos no utilizables|Número|Total|Número de nodos inutilizables|Sin dimensiones|
|PreemptedNodeCount|Recuento de nodos con prioridad|Número|Total|Número de nodos con prioridad|Sin dimensiones|
|TaskStartEvent|Eventos de inicio de tarea|Número|Total|Número total de tareas que se han iniciado|Sin dimensiones|
|TaskCompleteEvent|Eventos de tarea completada|Número|Total|Número total de tareas que se han completado|Sin dimensiones|
|TaskFailEvent|Eventos de error en tareas|Número|Total|Número total de tareas que se han completado con errores|Sin dimensiones|
|PoolCreateEvent|Eventos de creación de grupo|Número|Total|Número total de grupos que se han creado|Sin dimensiones|
|PoolResizeStartEvent|Eventos de inicio de cambio de tamaño de grupo|Número|Total|Número total de cambios de tamaño de grupo que se han iniciado|Sin dimensiones|
|PoolResizeCompleteEvent|Eventos de finalización de cambio de tamaño de grupo|Número|Total|Número total de cambios de tamaño de grupo completados|Sin dimensiones|
|PoolDeleteStartEvent|Eventos de inicio de eliminación de grupo|Número|Total|Número total de eliminaciones de grupo que se han iniciado|Sin dimensiones|
|PoolDeleteCompleteEvent|Eventos de finalización de eliminaciones de grupo|Número|Total|Número total de eliminaciones de grupo que se han completado|Sin dimensiones|
|JobDeleteCompleteEvent|Eventos de finalización de eliminaciones de trabajo|Número|Total|Número total de trabajos que se han eliminado correctamente.|Sin dimensiones|
|JobDeleteStartEvent|Eventos de inicio de eliminaciones de trabajo|Número|Total|Número total de trabajos cuya eliminación se ha solicitado.|Sin dimensiones|
|JobDisableCompleteEvent|Eventos de finalización de deshabilitación de trabajo|Número|Total|Número total de trabajos que se han deshabilitado correctamente.|Sin dimensiones|
|JobDisableStartEvent|Eventos de inicio de deshabilitación de trabajo|Número|Total|Número total de trabajos cuya deshabilitación se ha solicitado.|Sin dimensiones|
|JobStartEvent|Eventos de inicio de trabajo|Número|Total|Número total de trabajos que se han iniciado correctamente.|Sin dimensiones|
|JobTerminateCompleteEvent|Eventos de finalización de terminaciones de trabajo|Número|Total|Número total de trabajos que se han finalizado correctamente.|Sin dimensiones|
|JobTerminateStartEvent|Eventos de inicio de terminaciones de trabajo|Número|Total|Número total de trabajos cuya terminación se ha solicitado.|Sin dimensiones|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|connectedclients|Clientes conectados|Número|Máxima||ShardId|
|totalcommandsprocessed|Total de operaciones|Número|Total||ShardId|
|cachehits|Aciertos de caché|Número|Total||ShardId|
|cachemisses|Errores de caché|Número|Total||ShardId|
|getcommands|Gets|Número|Total||ShardId|
|setcommands|Sets|Número|Total||ShardId|
|operationsPerSecond|Operaciones por segundo|Número|Máxima||ShardId|
|evictedkeys|Claves expulsadas|Número|Total||ShardId|
|totalkeys|Total de claves|Número|Máxima||ShardId|
|expiredkeys|Claves expiradas|Número|Total||ShardId|
|usedmemory|Memoria usada|Bytes|Máxima||ShardId|
|usedmemorypercentage|Porcentaje de memoria usado|Percent|Máxima||ShardId|
|usedmemoryRss|Memoria RSS usada|Bytes|Máxima||ShardId|
|serverLoad|Carga de servidor|Percent|Máxima||ShardId|
|cacheWrite|Escritura de caché|BytesPerSecond|Máxima||ShardId|
|cacheRead|Lectura de caché|BytesPerSecond|Máxima||ShardId|
|percentProcessorTime|CPU|Percent|Máxima||ShardId|
|cacheLatency|Microsegundos de latencia de la memoria caché (versión preliminar)|Número|Media||ShardId, SampleType|
|errors|Errors|Número|Máxima||ShardId, ErrorType|
|connectedclients0|Clientes conectados (partición 0)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed0|Total de operaciones (partición 0)|Número|Total||Sin dimensiones|
|cachehits0|Aciertos de caché (partición 0)|Número|Total||Sin dimensiones|
|cachemisses0|Errores de caché (partición 0)|Número|Total||Sin dimensiones|
|getcommands0|Obtenciones (partición 0)|Número|Total||Sin dimensiones|
|setcommands0|Definiciones (partición 0)|Número|Total||Sin dimensiones|
|operationsPerSecond0|Operaciones por segundo (partición 0)|Número|Máxima||Sin dimensiones|
|evictedkeys0|Claves expulsadas (partición 0)|Número|Total||Sin dimensiones|
|totalkeys0|Total de claves (partición 0)|Número|Máxima||Sin dimensiones|
|expiredkeys0|Claves expiradas (partición 0)|Número|Total||Sin dimensiones|
|usedmemory0|Memoria usada (partición 0)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss0|Memoria RSS usada (partición 0)|Bytes|Máxima||Sin dimensiones|
|serverLoad0|Carga de servidor (partición 0)|Percent|Máxima||Sin dimensiones|
|cacheWrite0|Escritura de caché (partición 0)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead0|Lectura de caché (partición 0)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime0|CPU (partición 0)|Percent|Máxima||Sin dimensiones|
|connectedclients1|Clientes conectados (partición 1)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed1|Total de operaciones (partición 1)|Número|Total||Sin dimensiones|
|cachehits1|Aciertos de caché (partición 1)|Número|Total||Sin dimensiones|
|cachemisses1|Errores de caché (partición 1)|Número|Total||Sin dimensiones|
|getcommands1|Obtenciones (partición 1)|Número|Total||Sin dimensiones|
|setcommands1|Definiciones (partición 1)|Número|Total||Sin dimensiones|
|operationsPerSecond1|Operaciones por segundo (partición 1)|Número|Máxima||Sin dimensiones|
|evictedkeys1|Claves expulsadas (partición 1)|Número|Total||Sin dimensiones|
|totalkeys1|Total de claves (partición 1)|Número|Máxima||Sin dimensiones|
|expiredkeys1|Claves expiradas (partición 1)|Número|Total||Sin dimensiones|
|usedmemory1|Memoria usada (partición 1)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss1|Memoria RSS usada (partición 1)|Bytes|Máxima||Sin dimensiones|
|serverLoad1|Carga de servidor (partición 1)|Percent|Máxima||Sin dimensiones|
|cacheWrite1|Escritura de caché (partición 1)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead1|Lectura de caché (partición 1)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime1|CPU (partición 1)|Percent|Máxima||Sin dimensiones|
|connectedclients2|Clientes conectados (partición 2)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed2|Total de operaciones (partición 2)|Número|Total||Sin dimensiones|
|cachehits2|Aciertos de caché (partición 2)|Número|Total||Sin dimensiones|
|cachemisses2|Errores de caché (partición 2)|Número|Total||Sin dimensiones|
|getcommands2|Obtenciones (partición 2)|Número|Total||Sin dimensiones|
|setcommands2|Definiciones (partición 2)|Número|Total||Sin dimensiones|
|operationsPerSecond2|Operaciones por segundo (partición 2)|Número|Máxima||Sin dimensiones|
|evictedkeys2|Claves expulsadas (partición 2)|Número|Total||Sin dimensiones|
|totalkeys2|Total de claves (partición 2)|Número|Máxima||Sin dimensiones|
|expiredkeys2|Claves expiradas (partición 2)|Número|Total||Sin dimensiones|
|usedmemory2|Memoria usada (partición 2)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss2|Memoria RSS usada (partición 2)|Bytes|Máxima||Sin dimensiones|
|serverLoad2|Carga de servidor (partición 2)|Percent|Máxima||Sin dimensiones|
|cacheWrite2|Escritura de caché (partición 2)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead2|Lectura de caché (partición 2)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime2|CPU (partición 2)|Percent|Máxima||Sin dimensiones|
|connectedclients3|Clientes conectados (partición 3)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed3|Total de operaciones (partición 3)|Número|Total||Sin dimensiones|
|cachehits3|Aciertos de caché (partición 3)|Número|Total||Sin dimensiones|
|cachemisses3|Errores de caché (partición 3)|Número|Total||Sin dimensiones|
|getcommands3|Obtenciones (partición 3)|Número|Total||Sin dimensiones|
|setcommands3|Definiciones (partición 3)|Número|Total||Sin dimensiones|
|operationsPerSecond3|Operaciones por segundo (partición 3)|Número|Máxima||Sin dimensiones|
|evictedkeys3|Claves expulsadas (partición 3)|Número|Total||Sin dimensiones|
|totalkeys3|Total de claves (partición 3)|Número|Máxima||Sin dimensiones|
|expiredkeys3|Claves expiradas (partición 3)|Número|Total||Sin dimensiones|
|usedmemory3|Memoria usada (partición 3)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss3|Memoria RSS usada (partición 3)|Bytes|Máxima||Sin dimensiones|
|serverLoad3|Carga de servidor (partición 3)|Percent|Máxima||Sin dimensiones|
|cacheWrite3|Escritura de caché (partición 3)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead3|Lectura de caché (partición 3)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime3|CPU (partición 3)|Percent|Máxima||Sin dimensiones|
|connectedclients4|Clientes conectados (partición 4)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed4|Total de operaciones (partición 4)|Número|Total||Sin dimensiones|
|cachehits4|Aciertos de caché (partición 4)|Número|Total||Sin dimensiones|
|cachemisses4|Errores de caché (partición 4)|Número|Total||Sin dimensiones|
|getcommands4|Obtenciones (partición 4)|Número|Total||Sin dimensiones|
|setcommands4|Definiciones (partición 4)|Número|Total||Sin dimensiones|
|operationsPerSecond4|Operaciones por segundo (partición 4)|Número|Máxima||Sin dimensiones|
|evictedkeys4|Claves expulsadas (partición 4)|Número|Total||Sin dimensiones|
|totalkeys4|Total de claves (partición 4)|Número|Máxima||Sin dimensiones|
|expiredkeys4|Claves expiradas (partición 4)|Número|Total||Sin dimensiones|
|usedmemory4|Memoria usada (partición 4)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss4|Memoria RSS usada (partición 4)|Bytes|Máxima||Sin dimensiones|
|serverLoad4|Carga de servidor (partición 4)|Percent|Máxima||Sin dimensiones|
|cacheWrite4|Escritura de caché (partición 4)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead4|Lectura de caché (partición 4)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime4|CPU (partición 4)|Percent|Máxima||Sin dimensiones|
|connectedclients5|Clientes conectados (partición 5)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed5|Total de operaciones (partición 5)|Número|Total||Sin dimensiones|
|cachehits5|Aciertos de caché (partición 5)|Número|Total||Sin dimensiones|
|cachemisses5|Errores de caché (partición 5)|Número|Total||Sin dimensiones|
|getcommands5|Obtenciones (partición 5)|Número|Total||Sin dimensiones|
|setcommands5|Definiciones (partición 5)|Número|Total||Sin dimensiones|
|operationsPerSecond5|Operaciones por segundo (partición 5)|Número|Máxima||Sin dimensiones|
|evictedkeys5|Claves expulsadas (partición 5)|Número|Total||Sin dimensiones|
|totalkeys5|Total de claves (partición 5)|Número|Máxima||Sin dimensiones|
|expiredkeys5|Claves expiradas (partición 5)|Número|Total||Sin dimensiones|
|usedmemory5|Memoria usada (partición 5)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss5|Memoria RSS usada (partición 5)|Bytes|Máxima||Sin dimensiones|
|serverLoad5|Carga de servidor (partición 5)|Percent|Máxima||Sin dimensiones|
|cacheWrite5|Escritura de caché (partición 5)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead5|Lectura de caché (partición 5)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime5|CPU (partición 5)|Percent|Máxima||Sin dimensiones|
|connectedclients6|Clientes conectados (partición 6)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed6|Total de operaciones (partición 6)|Número|Total||Sin dimensiones|
|cachehits6|Aciertos de caché (partición 6)|Número|Total||Sin dimensiones|
|cachemisses6|Errores de caché (partición 6)|Número|Total||Sin dimensiones|
|getcommands6|Obtenciones (partición 6)|Número|Total||Sin dimensiones|
|setcommands6|Definiciones (partición 6)|Número|Total||Sin dimensiones|
|operationsPerSecond6|Operaciones por segundo (partición 6)|Número|Máxima||Sin dimensiones|
|evictedkeys6|Claves expulsadas (partición 6)|Número|Total||Sin dimensiones|
|totalkeys6|Total de claves (partición 6)|Número|Máxima||Sin dimensiones|
|expiredkeys6|Claves expiradas (partición 6)|Número|Total||Sin dimensiones|
|usedmemory6|Memoria usada (partición 6)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss6|Memoria RSS usada (partición 6)|Bytes|Máxima||Sin dimensiones|
|serverLoad6|Carga de servidor (partición 6)|Percent|Máxima||Sin dimensiones|
|cacheWrite6|Escritura de caché (partición 6)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead6|Lectura de caché (partición 6)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime6|CPU (partición 6)|Percent|Máxima||Sin dimensiones|
|connectedclients7|Clientes conectados (partición 7)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed7|Total de operaciones (partición 7)|Número|Total||Sin dimensiones|
|cachehits7|Aciertos de caché (partición 7)|Número|Total||Sin dimensiones|
|cachemisses7|Errores de caché (partición 7)|Número|Total||Sin dimensiones|
|getcommands7|Obtenciones (partición 7)|Número|Total||Sin dimensiones|
|setcommands7|Definiciones (partición 7)|Número|Total||Sin dimensiones|
|operationsPerSecond7|Operaciones por segundo (partición 7)|Número|Máxima||Sin dimensiones|
|evictedkeys7|Claves expulsadas (partición 7)|Número|Total||Sin dimensiones|
|totalkeys7|Total de claves (partición 7)|Número|Máxima||Sin dimensiones|
|expiredkeys7|Claves expiradas (partición 7)|Número|Total||Sin dimensiones|
|usedmemory7|Memoria usada (partición 7)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss7|Memoria RSS usada (partición 7)|Bytes|Máxima||Sin dimensiones|
|serverLoad7|Carga de servidor (partición 7)|Percent|Máxima||Sin dimensiones|
|cacheWrite7|Escritura de caché (partición 7)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead7|Lectura de caché (partición 7)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime7|CPU (partición 7)|Percent|Máxima||Sin dimensiones|
|connectedclients8|Clientes conectados (partición 8)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed8|Total de operaciones (partición 8)|Número|Total||Sin dimensiones|
|cachehits8|Aciertos de caché (partición 8)|Número|Total||Sin dimensiones|
|cachemisses8|Errores de caché (partición 8)|Número|Total||Sin dimensiones|
|getcommands8|Obtenciones (partición 8)|Número|Total||Sin dimensiones|
|setcommands8|Definiciones (partición 8)|Número|Total||Sin dimensiones|
|operationsPerSecond8|Operaciones por segundo (partición 8)|Número|Máxima||Sin dimensiones|
|evictedkeys8|Claves expulsadas (partición 8)|Número|Total||Sin dimensiones|
|totalkeys8|Total de claves (partición 8)|Número|Máxima||Sin dimensiones|
|expiredkeys8|Claves expiradas (partición 8)|Número|Total||Sin dimensiones|
|usedmemory8|Memoria usada (partición 8)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss8|Memoria RSS usada (partición 8)|Bytes|Máxima||Sin dimensiones|
|serverLoad8|Carga de servidor (partición 8)|Percent|Máxima||Sin dimensiones|
|cacheWrite8|Escritura de caché (partición 8)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead8|Lectura de caché (partición 8)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime8|CPU (partición 8)|Percent|Máxima||Sin dimensiones|
|connectedclients9|Clientes conectados (partición 9)|Número|Máxima||Sin dimensiones|
|totalcommandsprocessed9|Total de operaciones (partición 9)|Número|Total||Sin dimensiones|
|cachehits9|Aciertos de caché (partición 9)|Número|Total||Sin dimensiones|
|cachemisses9|Errores de caché (partición 9)|Número|Total||Sin dimensiones|
|getcommands9|Obtenciones (partición 9)|Número|Total||Sin dimensiones|
|setcommands9|Definiciones (partición 9)|Recuento|Total||Sin dimensiones|
|operationsPerSecond9|Operaciones por segundo (partición 9)|Número|Máxima||Sin dimensiones|
|evictedkeys9|Claves expulsadas (partición 9)|Número|Total||Sin dimensiones|
|totalkeys9|Total de claves (partición 9)|Número|Máxima||Sin dimensiones|
|expiredkeys9|Claves expiradas (partición 9)|Número|Total||Sin dimensiones|
|usedmemory9|Memoria usada (partición 9)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss9|Memoria RSS usada (partición 9)|Bytes|Máxima||Sin dimensiones|
|serverLoad9|Carga de servidor (partición 9)|Percent|Máxima||Sin dimensiones|
|cacheWrite9|Escritura de caché (partición 9)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead9|Lectura de caché (partición 9)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime9|CPU (partición 9)|Percent|Máxima||Sin dimensiones|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Media|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente.|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante).|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente).|Sin dimensiones|
|Disk Read Bytes/Sec|Lectura de disco|BytesPerSecond|Media|Promedio de bytes que se leen desde el disco durante el período de supervisión.|Sin dimensiones|
|Disk Write Bytes/Sec|Escritura de disco|BytesPerSecond|Media|Promedio de bytes que se escriben en el disco durante el período de supervisión.|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Media|E/S por segundo de lectura de disco.|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Media|E/S por segundo de escritura en disco.|Sin dimensiones|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Media|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente.|RoleInstanceId|
|Red interna|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante).|RoleInstanceId|
|Red interna|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente).|RoleInstanceId|
|Disk Read Bytes/Sec|Lectura de disco|BytesPerSecond|Media|Promedio de bytes que se leen desde el disco durante el período de supervisión.|RoleInstanceId|
|Disk Write Bytes/Sec|Escritura de disco|BytesPerSecond|Media|Promedio de bytes que se escriben en el disco durante el período de supervisión.|RoleInstanceId|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Media|E/S por segundo de lectura de disco.|RoleInstanceId|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Media|E/S por segundo de escritura en disco.|RoleInstanceId|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TotalCalls|Total de llamadas|Número|Total|Número total de llamadas.|Sin dimensiones|
|SuccessfulCalls|Llamadas correctas|Número|Total|Número de llamadas correctas.|Sin dimensiones|
|TotalErrors|Total de errores|Número|Total|Número total de llamadas con respuesta de error (código de respuesta HTTP 4xx o 5xx).|Sin dimensiones|
|BlockedCalls|Llamadas bloqueadas|Número|Total|Número de llamadas que han superado la tasa o el límite de cuota.|Sin dimensiones|
|ServerErrors|Errores del servidor|Número|Total|Número de llamadas con error interno del servicio (código de respuesta HTTP 5xx).|Sin dimensiones|
|ClientErrors|Errores de cliente|Número|Total|Número de llamadas con error interno del lado cliente (código de respuesta HTTP 4xx).|Sin dimensiones|
|DataIn|Entrada de datos|Bytes|Total|Tamaño de los datos de entrada en bytes.|Sin dimensiones|
|DataOut|Salida de datos|Bytes|Total|Tamaño de los datos de salida en bytes.|Sin dimensiones|
|Latencia|Latencia|MilliSeconds|Media|Latencia en milisegundos.|Sin dimensiones|
|CharactersTranslated|Caracteres traducidos|Número|Total|Número total de caracteres de la solicitud entrante de texto.|Sin dimensiones|
|SpeechSessionDuration|Duración de la sesión de voz|Segundos|Total|Duración total de la sesión de voz en segundos.|Sin dimensiones|
|TotalTransactions|Transacciones totales|Número|Total|Número total de transacciones.|Sin dimensiones|
|TotalTokenCalls|Llamadas de token totales|Número|Total|Número total de llamadas de token.|Sin dimensiones|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Media|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|Sin dimensiones|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Número total de bytes que se leen desde el disco durante el período de supervisión|Sin dimensiones|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Número total de bytes que se escriben en el disco durante el período de supervisión|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Media|E/S por segundo de lectura de disco|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Media|E/S por segundo de escritura en disco|Sin dimensiones|
|Créditos de CPU restantes|Créditos de CPU restantes|Número|Media|Número total de créditos disponibles para irrumpir|Sin dimensiones|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Número|Media|Número total de créditos consumidos por la máquina virtual|Sin dimensiones|
|Bytes de lectura de discos por segundo|Bytes de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo de lectura de un único disco durante el período de supervisión|SlotId|
|Bytes de escritura de discos por segundo|Bytes de escritura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo escritos en un único disco durante el período de supervisión|SlotId|
|Operaciones de lectura de discos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la lectura de un único disco durante el período de supervisión|SlotId|
|Operaciones de escritura de discos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la escritura en un único disco durante el período de supervisión|SlotId|
|Por disco QD|Disco de datos QD (versión preliminar)|Número|Media|Profundidad de la cola del disco de datos (o longitud de la cola)|SlotId|
|Sistema operativo por bytes de lectura de discos por segundo|Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por bytes de escritura de disco por segundo|Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por operaciones de lectura de discos por segundo|Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo operaciones de escritura de discos por segundo|Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la escritura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por disco QD|Disco QD del sistema operativo (versión preliminar)|Número|Media|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|Sin dimensiones|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Media|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|Sin dimensiones|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Número total de bytes que se leen desde el disco durante el período de supervisión|Sin dimensiones|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Número total de bytes que se escriben en el disco durante el período de supervisión|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Media|E/S por segundo de lectura de disco|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Media|E/S por segundo de escritura en disco|Sin dimensiones|
|Créditos de CPU restantes|Créditos de CPU restantes|Número|Media|Número total de créditos disponibles para irrumpir|Sin dimensiones|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Número|Media|Número total de créditos consumidos por la máquina virtual|Sin dimensiones|
|Bytes de lectura de discos por segundo|Bytes de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo de lectura de un único disco durante el período de supervisión|SlotId|
|Bytes de escritura de discos por segundo|Bytes de escritura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo escritos en un único disco durante el período de supervisión|SlotId|
|Operaciones de lectura de discos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la lectura de un único disco durante el período de supervisión|SlotId|
|Operaciones de escritura de discos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la escritura en un único disco durante el período de supervisión|SlotId|
|Por disco QD|Disco de datos QD (versión preliminar)|Número|Media|Profundidad de la cola del disco de datos (o longitud de la cola)|SlotId|
|Sistema operativo por bytes de lectura de discos por segundo|Bytes de lectura de discos del sistema operativo por segundo|CountPerSecond|Media|Total de bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por bytes de escritura de disco por segundo|Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por operaciones de lectura de discos por segundo|Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo operaciones de escritura de discos por segundo|Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la escritura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por disco QD|Disco QD del sistema operativo (versión preliminar)|Número|Media|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|Sin dimensiones|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Media|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|Sin dimensiones|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Número total de bytes que se leen desde el disco durante el período de supervisión|Sin dimensiones|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Número total de bytes que se escriben en el disco durante el período de supervisión|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Media|E/S por segundo de lectura de disco|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Media|E/S por segundo de escritura en disco|Sin dimensiones|
|Créditos de CPU restantes|Créditos de CPU restantes|Número|Media|Número total de créditos disponibles para irrumpir|Sin dimensiones|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Número|Media|Número total de créditos consumidos por la máquina virtual|Sin dimensiones|
|Bytes de lectura de discos por segundo|Bytes de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo de lectura de un único disco durante el período de supervisión|SlotId|
|Bytes de escritura de discos por segundo|Bytes de escritura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo escritos en un único disco durante el período de supervisión|SlotId|
|Operaciones de lectura de discos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la lectura de un único disco durante el período de supervisión|SlotId|
|Operaciones de escritura de discos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la escritura en un único disco durante el período de supervisión|SlotId|
|Por disco QD|Disco de datos QD (versión preliminar)|Número|Media|Profundidad de la cola del disco de datos (o longitud de la cola)|SlotId|
|Sistema operativo por bytes de lectura de discos por segundo|Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por bytes de escritura de disco por segundo|Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Total de bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por operaciones de lectura de discos por segundo|Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo operaciones de escritura de discos por segundo|Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS total durante la escritura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por disco QD|Disco QD del sistema operativo (versión preliminar)|Número|Media|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|Sin dimensiones|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CpuUsage|Uso de CPU|Número|Media|Uso de la CPU en todos los núcleos en millares de núcleos.|containerName|
|MemoryUsage|Uso de la memoria|Bytes|Media|Uso de memoria total en bytes.|containerName|
|NetworkBytesReceivedPerSecond|Bytes de red recibidos por segundo|Bytes|Media|Bytes de red recibidos por segundo.|Sin dimensiones|
|NetworkBytesTransmittedPerSecond|Bytes de red transmitidos por segundo|Bytes|Media|Bytes de red transmitidos por segundo.|Sin dimensiones|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Número total de núcleos de CPU disponibles en un clúster administrado|Número|Total|Número total de núcleos de CPU disponibles en un clúster administrado|Sin dimensiones|
|kube_node_status_allocatable_memory_bytes|Cantidad total de memoria disponible en un clúster administrado|Bytes|Total|Cantidad total de memoria disponible en un clúster administrado|Sin dimensiones|
|kube_pod_status_ready|Número de pods con estado Listo|Número|Total|Número de pods con estado Listo|espacio de nombres, pod|
|kube_node_status_condition|Estados de diversas condiciones de nodo|Número|Total|Estados de diversas condiciones de nodo|condición, estado, nodo|
|kube_pod_status_phase|Número de pods por fase|Número|Total|Número de pods por fase|fase, espacio de nombres, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|DCIApiCalls|Llamadas API Customer Insights|Número|Total||Sin dimensiones|
|DCIMappingImportOperationSuccessfulLines|Líneas correctas de la operación de importación de asignaciones|Número|Total||Sin dimensiones|
|DCIMappingImportOperationFailedLines|Líneas con error de la operación de importación de asignaciones|Número|Total||Sin dimensiones|
|DCIMappingImportOperationTotalLines|Líneas totales de la operación de importación de asignaciones|Número|Total||Sin dimensiones|
|DCIMappingImportOperationRuntimeInSeconds|Tiempo de ejecución en segundos de la operación de importación de asignaciones|Segundos|Total||Sin dimensiones|
|DCIOutboundProfileExportSucceeded|Exportación correcta del perfil saliente|Número|Total||Sin dimensiones|
|DCIOutboundProfileExportFailed|Error de exportación del perfil saliente|Número|Total||Sin dimensiones|
|DCIOutboundProfileExportDuration|Duración de la exportación del perfil saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundKpiExportSucceeded|Exportación correcta de KPI saliente|Número|Total||Sin dimensiones|
|DCIOutboundKpiExportFailed|Error de exportación de KPI saliente|Número|Total||Sin dimensiones|
|DCIOutboundKpiExportDuration|Duración de exportación de KPI saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundKpiExportStarted|Inicio de exportación de KPI saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundKpiRecordCount|Recuento de registros de KPI saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundProfileExportCount|Recuento de exportación de perfil saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundInitialProfileExportFailed|Recuento de exportación de perfil inicial saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundInitialProfileExportSucceeded|Exportación correcta de perfil inicial saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundInitialKpiExportFailed|Error de exportación de KPI inicial saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundInitialKpiExportSucceeded|Exportación correcta de KPI inicial saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundInitialProfileExportDurationInSeconds|Duración en segundos de exportación de perfil inicial saliente|Segundos|Total||Sin dimensiones|
|AdlaJobForStandardKpiFailed|Error en segundos de trabajo de Adla para KPI estándar|Segundos|Total||Sin dimensiones|
|AdlaJobForStandardKpiTimeOut|Tiempo de espera en segundos del trabajo de Adla para KPI estándar|Segundos|Total||Sin dimensiones|
|AdlaJobForStandardKpiCompleted|Tiempo de finalización en segundos del trabajo de Adla para KPI estándar|Segundos|Total||Sin dimensiones|
|ImportASAValuesFailed|Error de recuento de valores ASA de importación|Número|Total||Sin dimensiones|
|ImportASAValuesSucceeded|Recuento correcto de valores ASA de importación|Número|Total||Sin dimensiones|
|DCIProfilesCount|Recuento de instancias del perfil|Número|Último||Sin dimensiones|
|DCIInteractionsPerMonthCount|Recuento de interacciones por mes|Número|Último||Sin dimensiones|
|DCIKpisCount|Recuento de KPI|Número|Último||Sin dimensiones|
|DCISegmentsCount|Recuento de segmentos|Número|Último||Sin dimensiones|
|DCIPredictiveMatchPoliciesCount|Recuento de coincidencias predictivo|Número|Último||Sin dimensiones|
|DCIPredictionsCount|Recuento de predicción|Número|Último||Sin dimensiones|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|FailedRuns|Ejecuciones con error|Número|Total||pipelineName, activityName, windowEnd, windowStart|
|SuccessfulRuns|Ejecuciones correctas|Número|Total||pipelineName, activityName, windowEnd, windowStart|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|PipelineFailedRuns|Métricas de ejecuciones de canalización erróneas|Número|Total||FailureType, Name|
|PipelineSucceededRuns|Las métricas de ejecuciones de canalización se realizaron correctamente|Número|Total||FailureType, Name|
|ActivityFailedRuns|Métricas de ejecuciones de actividad erróneas|Número|Total||ActivityType, PipelineName, FailureType, Name|
|ActivitySucceededRuns|Métricas de ejecución de actividad realizadas correctamente|Número|Total||ActivityType, PipelineName, FailureType, Name|
|TriggerFailedRuns|Métricas de ejecuciones de desencadenador erróneas|Número|Total||Name, FailureType|
|TriggerSucceededRuns|Métricas de ejecuciones de desencadenador realizadas correctamente|Número|Total||Name, FailureType|
|IntegrationRuntimeCpuPercentage|Uso de la CPU de entorno de ejecución de integración|Percent|Media||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableMemory|Memoria disponible de entorno de ejecución de integración|Bytes|Media||IntegrationRuntimeName, NodeName|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|JobEndedSuccess|Trabajos correctos|Número|Total|Recuento de trabajos realizados correctamente|Sin dimensiones|
|JobEndedFailure|Trabajos con error|Número|Total|Recuento de trabajos con error|Sin dimensiones|
|JobEndedCancelled|Trabajos cancelados|Número|Total|Recuento de trabajos cancelados|Sin dimensiones|
|JobAUEndedSuccess|Tiempo AU correcto|Segundos|Total|Tiempo total AU para trabajos realizados correctamente|Sin dimensiones|
|JobAUEndedFailure|Error de tiempo AU|Segundos|Total|Tiempo total AU para trabajos con error|Sin dimensiones|
|JobAUEndedCancelled|Tiempo AU para cancelados|Segundos|Total|Tiempo total AU para trabajos cancelados|Sin dimensiones|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TotalStorage|Almacenamiento total|Bytes|Máxima|Cantidad total de datos almacenados en la cuenta.|Sin dimensiones|
|DataWritten|Datos escritos|Bytes|Total|Cantidad total de datos escritos en la cuenta.|Sin dimensiones|
|DataRead|Datos leídos|Bytes|Total|Cantidad total de datos leídos de la cuenta.|Sin dimensiones|
|WriteRequests|Solicitudes de escritura|Número|Total|Número de solicitudes de escritura de datos en la cuenta.|Sin dimensiones|
|ReadRequests|Solicitudes de lectura|Número|Total|Número de solicitudes de lectura de datos de la cuenta.|Sin dimensiones|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Sin dimensiones|
|memory_percent|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Sin dimensiones|
|io_consumption_percent|Porcentaje de E/S|Percent|Media|Porcentaje de E/S|Sin dimensiones|
|storage_percent|Porcentaje de almacenamiento|Percent|Media|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Almacenamiento utilizado|Bytes|Media|Almacenamiento utilizado|Sin dimensiones|
|storage_limit|Límite de almacenamiento|Bytes|Media|Límite de almacenamiento|Sin dimensiones|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|Media|Porcentaje de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|Media|Almacenamiento del registro del servidor usado|Sin dimensiones|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|Media|Límite de almacenamiento del registro del servidor|Sin dimensiones|
|active_connections|Conexiones activas|Número|Media|Conexiones activas|Sin dimensiones|
|connections_failed|Conexiones con errores|Número|Total|Conexiones con errores|Sin dimensiones|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Sin dimensiones|
|memory_percent|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Sin dimensiones|
|io_consumption_percent|Porcentaje de E/S|Percent|Media|Porcentaje de E/S|Sin dimensiones|
|storage_percent|Porcentaje de almacenamiento|Percent|Media|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Almacenamiento utilizado|Bytes|Media|Almacenamiento utilizado|Sin dimensiones|
|storage_limit|Límite de almacenamiento|Bytes|Media|Límite de almacenamiento|Sin dimensiones|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|Media|Porcentaje de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|Media|Almacenamiento del registro del servidor usado|Sin dimensiones|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|Media|Límite de almacenamiento del registro del servidor|Sin dimensiones|
|active_connections|Conexiones activas|Número|Media|Conexiones activas|Sin dimensiones|
|connections_failed|Conexiones con errores|Número|Total|Conexiones con errores|Sin dimensiones|
|seconds_behind_master|Intervalo de replicación en segundos|Número|Media|Intervalo de replicación en segundos|Sin dimensiones|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Sin dimensiones|
|memory_percent|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Sin dimensiones|
|io_consumption_percent|Porcentaje de E/S|Percent|Media|Porcentaje de E/S|Sin dimensiones|
|storage_percent|Porcentaje de almacenamiento|Percent|Media|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Almacenamiento utilizado|Bytes|Media|Almacenamiento utilizado|Sin dimensiones|
|storage_limit|Límite de almacenamiento|Bytes|Media|Límite de almacenamiento|Sin dimensiones|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|Media|Porcentaje de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|Media|Almacenamiento del registro del servidor usado|Sin dimensiones|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|Media|Límite de almacenamiento del registro del servidor|Sin dimensiones|
|active_connections|Conexiones activas|Número|Media|Conexiones activas|Sin dimensiones|
|connections_failed|Conexiones con errores|Número|Total|Conexiones con errores|Sin dimensiones|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Intentos de envío de mensajes de telemetría|Número|Total|Número de mensajes de telemetría de dispositivo a la nube para enviar a IoT Hub|Sin dimensiones|
|d2c.telemetry.ingress.success|Mensajes de telemetría enviados|Número|Total|Número de mensajes de telemetría de dispositivo a la nube enviados correctamente a IoT Hub|Sin dimensiones|
|c2d.commands.egress.complete.success|Comandos completados|Número|Total|Número de comandos de la nube al dispositivo que el dispositivo ha completado correctamente|Sin dimensiones|
|c2d.commands.egress.abandon.success|Comandos abandonados|Número|Total|Número de comandos de la nube al dispositivo que el dispositivo ha abandonado|Sin dimensiones|
|c2d.commands.egress.reject.success|Comandos rechazados|Número|Total|Número de comandos de la nube al dispositivo que el dispositivo ha rechazado|Sin dimensiones|
|devices.totalDevices|Total de dispositivos (en desuso)|Número|Total|Número de dispositivos registrados en IoT Hub|Sin dimensiones|
|devices.connectedDevices.allProtocol|Dispositivos conectados (en desuso) |Número|Total|Número de dispositivos conectados a IoT Hub|Sin dimensiones|
|d2c.telemetry.egress.success|Enrutamiento: mensajes de telemetría entregados|Número|Total|Número de veces que los mensajes se han entregado correctamente a todos los puntos de conexión mediante el enrutamiento de IoT Hub. Si un mensaje se enruta a varios puntos de conexión, este valor aumenta en uno por cada entrega correcta. Si un mensaje se enruta al mismo punto de conexión varias veces, este valor aumenta en uno por cada entrega correcta.|Sin dimensiones|
|d2c.telemetry.egress.dropped|Enrutamiento: mensajes de telemetría quitados |Número|Total|Número de veces que se han quitado los mensajes con el enrutamiento de IoT Hub a causa de puntos de conexión inactivos. Este valor no cuenta los mensajes entregados a la ruta de reserva porque los mensajes quitados no se entregan allí.|Sin dimensiones|
|d2c.telemetry.egress.orphaned|Enrutamiento: mensajes de telemetría huérfanos |Número|Total|Número de veces que los mensajes eran huérfanos a causa del enrutamiento de IoT Hub porque no coincidían con las reglas de enrutamiento (incluida la regla de reserva). |Sin dimensiones|
|d2c.telemetry.egress.invalid|Enrutamiento: mensajes de telemetría incompatibles|Número|Total|Número de veces que el enrutamiento de IoT Hub no ha podido entregar mensajes debido a una incompatibilidad con el punto de conexión. Este valor no incluye los reintentos.|Sin dimensiones|
|d2c.telemetry.egress.fallback|Enrutamiento: mensajes entregados a la reserva|Número|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes al punto de conexión asociado con la ruta de reserva.|Sin dimensiones|
|d2c.endpoints.egress.eventHubs|Enrutamiento: mensajes entregados al centro de eventos|Número|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del centro de eventos.|Sin dimensiones|
|d2c.endpoints.latency.eventHubs|Enrutamiento: latencia de mensajes para el centro de eventos|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión del centro de eventos.|Sin dimensiones|
|d2c.endpoints.egress.serviceBusQueues|Enrutamiento: mensajes entregados a la cola de Service Bus|Número|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de la cola de Service Bus.|Sin dimensiones|
|d2c.endpoints.latency.serviceBusQueues|Enrutamiento: latencia de mensajes de la cola de Service Bus|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de la cola de Service Bus.|Sin dimensiones|
|d2c.endpoints.egress.serviceBusTopics|Enrutamiento: mensajes entregados al tema de Service Bus|Número|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del tema de Service Bus.|Sin dimensiones|
|d2c.endpoints.latency.serviceBusTopics|Enrutamiento: latencia de mensajes del tema de Service Bus|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión del tema de Service Bus.|Sin dimensiones|
|d2c.endpoints.egress.builtIn.events|Enrutamiento: mensajes entregados a mensajes/eventos|Número|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente al punto de conexión integrado (mensajes/eventos).|Sin dimensiones|
|d2c.endpoints.latency.builtIn.events|Enrutamiento: latencia de mensajes para mensajes/eventos|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría al punto de conexión integrado (mensajes/eventos).|Sin dimensiones|
|d2c.endpoints.egress.storage|Enrutamiento: mensajes entregados al almacenamiento|Número|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.latency.storage|Enrutamiento: latencia de mensajes para almacenamiento|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.egress.storage.bytes|Enrutamiento: datos entregados al almacenamiento|Bytes|Total|Cantidad de datos (bytes) que el enrutamiento de IoT Hub ha entregado a los punto de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.egress.storage.blobs|Enrutamiento: blobs entregados al almacenamiento|Número|Total|Número de veces que el enrutamiento de IoT Hub ha entregado blobs a los puntos de conexión de almacenamiento.|Sin dimensiones|
|d2c.twin.read.success|Lecturas gemelas correctas de los dispositivos|Número|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.read.failure|Lecturas gemelas con error de los dispositivos|Número|Total|El recuento de todas las lecturas gemelas con error iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.read.size|Tamaño de la respuesta de las lecturas gemelas de dispositivos|Bytes|Media|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.success|Actualizaciones gemelas correctas de los dispositivos|Número|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.failure|Actualizaciones gemelas con error de los dispositivos|Número|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.size|Tamaño de las actualizaciones gemelas de los dispositivos|Bytes|Media|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|c2d.methods.success|Invocaciones correctas al método directo|Número|Total|El número de todas las llamadas correctas al método directo.|Sin dimensiones|
|c2d.methods.failure|Invocaciones al método directo con error|Número|Total|El número de todas las llamadas al método directo con error.|Sin dimensiones|
|c2d.methods.requestSize|Tamaño de la solicitud de las invocaciones a métodos directos|Bytes|Media|El valor medio, mínimo y máximo de todas las solicitudes correctas de método directo.|Sin dimensiones|
|c2d.methods.responseSize|Tamaño de la respuesta de las invocaciones a métodos directos|Bytes|Media|El valor medio, mínimo y máximo de todas las respuestas correctas de método directo.|Sin dimensiones|
|c2d.twin.read.success|Lecturas gemelas correctas del back-end|Número|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.read.failure|Lecturas gemelas con error del back-end|Número|Total|El recuento de todas las lecturas gemelas con error iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.read.size|Tamaño de la respuesta de las lecturas gemelas del back-end|Bytes|Media|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.success|Actualizaciones gemelas correctas del back-end|Número|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.failure|Actualizaciones gemelas con error del back-end|Número|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.size|Tamaño de las actualizaciones gemelas del back-end|Bytes|Media|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|twinQueries.success|Consultas gemelas correctas|Número|Total|El recuento de todas las consultas gemelas correctas.|Sin dimensiones|
|twinQueries.failure|Consultas gemelas con error|Número|Total|El recuento de todas las consultas gemelas con error.|Sin dimensiones|
|twinQueries.resultSize|Tamaño de resultado de consultas gemelas|Bytes|Media|El valor medio, mínimo y máximo del tamaño del resultado de todas las consultas gemelas correctas.|Sin dimensiones|
|jobs.createTwinUpdateJob.success|Creaciones correctas de trabajos de actualización gemelos|Número|Total|El recuento de todas las creaciones correctas de trabajos de actualización gemelos.|Sin dimensiones|
|jobs.createTwinUpdateJob.failure|Creaciones con error de trabajos de actualización gemelos|Número|Total|El recuento de todas las creaciones con error de trabajos de actualización gemelos.|Sin dimensiones|
|jobs.createDirectMethodJob.success|Creaciones correctas de trabajos de invocación de método|Número|Total|El recuento de todas las creaciones correctas de los trabajos de invocación de método directos.|Sin dimensiones|
|jobs.createDirectMethodJob.failure|Creaciones con error de trabajos de invocación de método|Número|Total|El recuento de todas las creaciones con error de los trabajos de invocación de método directos.|Sin dimensiones|
|jobs.listJobs.success|Llamadas correctas para enumerar trabajos|Número|Total|El recuento de todas las llamadas correctas para enumerar trabajos.|Sin dimensiones|
|jobs.listJobs.failure|Llamadas con error para enumerar trabajos|Número|Total|El recuento de todas las llamadas con error para enumerar trabajos.|Sin dimensiones|
|jobs.cancelJob.success|Cancelaciones de trabajos correctas|Número|Total|El recuento de todas las llamadas correctas para cancelar un trabajo.|Sin dimensiones|
|jobs.cancelJob.failure|Cancelaciones de trabajos con error|Número|Total|El recuento de todas las llamadas con error para cancelar un trabajo.|Sin dimensiones|
|jobs.queryJobs.success|Consultas de trabajo correctas|Número|Total|El recuento de todas las llamadas correctas para consultar trabajos.|Sin dimensiones|
|jobs.queryJobs.failure|Consultas de trabajo con error|Número|Total|El recuento de todas las llamadas con error para consultar trabajos.|Sin dimensiones|
|jobs.completed|Trabajos completados|Número|Total|El recuento de todos los trabajos completados.|Sin dimensiones|
|jobs.failed|Trabajos con error|Número|Total|El recuento de todos los trabajos con error.|Sin dimensiones|
|d2c.telemetry.ingress.sendThrottle|Número de errores de limitación|Número|Total|Número de errores de limitación debido a las limitaciones de rendimiento del dispositivo|Sin dimensiones|
|dailyMessageQuotaUsed|Número total de mensajes usados|Número|Media|Número de mensajes totales usados hoy. Se trata de un valor acumulado que se restablece en cero a las 00:00 UTC cada día.|Sin dimensiones|
|deviceDataUsage|Uso total de datos del dispositivo (en desuso)|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|Sin dimensiones|
|deviceDataUsageV2|Uso total de datos del dispositivo (versión preliminar)|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|Sin dimensiones|
|totalDeviceCount|Total de dispositivos (versión preliminar)|Número|Media|Número de dispositivos registrados en IoT Hub|Sin dimensiones|
|connectedDeviceCount|Dispositivos conectados (versión preliminar)|Número|Media|Número de dispositivos conectados a IoT Hub|Sin dimensiones|
|configuraciones|Métricas de configuración|Número|Total|Métricas de las operaciones de configuración|Sin dimensiones|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|RegistrationAttempts|Intentos de registro|Número|Total|Número de intentos de registro de dispositivos|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Dispositivos asignados|Número|Total|Número de dispositivos asignados a un IoT Hub|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Intentos de atestación|Número|Total|Número de intentos de atestación de dispositivos|ProvisioningServiceName, Status, Protocol|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

### <a name="request-metrics"></a>Solicitud de métricas

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---|---|---| ---| ---| ---|
| TotalRequests |   Total de solicitudes| Número   | Número | Número de solicitudes realizadas|  DatabaseName, CollectionName, Region, StatusCode|   Todo |   TotalRequests, Http 2xx, Http 3xx, Http 400, Http 401, error interno del servidor, servicio no disponible, solicitudes limitadas, promedio de solicitudes por segundo |    Se usa para supervisar las solicitudes por código de estado, la colección en una granularidad por minuto. Para obtener el promedio de solicitudes por segundo, utilice la agregación Count en un minuto y divida por 60. |
| MetadataRequests |    Solicitudes de metadatos   |Número| Número   | Recuento de las solicitudes de metadatos. Azure Cosmos DB mantiene la colección de metadatos del sistema para cada cuenta, lo que le permite enumerar las colecciones, las bases de datos y sus configuraciones de forma gratuita.    | DatabaseName, CollectionName, Region, StatusCode| Todo|  |Se usa para supervisar las limitaciones debido a las solicitudes de metadatos.|
| MongoRequests |   Solicitudes de Mongo| Número | Número|  Número de solicitudes de Mongo realizadas   | DatabaseName, CollectionName, Region, CommandName, ErrorCode| Todo |Tasa de solicitudes de consultas de Mongo, tasa de solicitudes de actualización de Mongo, tasa de solicitudes de eliminación de Mongo, tasa de solicitudes de inserción de Mongo y tasa de solicitudes de recuento de Mongo|   Se usa para supervisar los errores de solicitud de Mongo y los usos por de comando. |


### <a name="request-unit-metrics"></a>Métricas de unidad de solicitud

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---|---|---| ---| ---| ---|
| MongoRequestCharge|   Cargo de la solicitud de Mongo |  Número   |Total  |Unidades de la solicitud de Mongo consumidas|  DatabaseName, CollectionName, Region, CommandName, ErrorCode|   Todo |Cargo de solicitudes de consultas de Mongo, cargo de solicitudes de actualización de Mongo, cargo de solicitudes de eliminación de Mongo, cargo de solicitudes de inserción de Mongo y cargo de solicitudes de recuento de Mongo| Se usa para supervisar las RU de recursos de Mongo en un minuto.|
| TotalRequestUnits |Unidades de solicitud totales|   Número|  Total|  Unidades de solicitud consumidas| DatabaseName, CollectionName, Region, StatusCode    |Todo|   TotalRequestUnits|  Se usa para supervisar el uso de RU total en una granularidad por minuto. Para obtener el promedio de RU consumidas por segundo, utilice la agregación Total en un minuto y divida por 60.|
| ProvisionedThroughput |Rendimiento aprovisionado|    Número|  Máxima |Rendimiento aprovisionado en granularidad de la colección|  DatabaseName, CollectionName|   5M| |   Se usa para supervisar el rendimiento aprovisionado por colección.|

### <a name="storage-metrics"></a>Métricas de almacenamiento

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---|---|---| ---| ---| ---|
| AvailableStorage| Almacenamiento disponible   |Bytes| Total|  Almacenamiento total disponible notificado en una granularidad de cinco minutos por región|   DatabaseName, CollectionName, Region|   5M| Almacenamiento disponible|   Se usa para supervisar la capacidad de almacenamiento disponible (aplicable solo para las colecciones de almacenamiento fijo). La granularidad mínima debe ser de cinco minutos.| 
| DataUsage |Uso de datos |Bytes| Total   |Uso de datos total notificado en una granularidad de cinco minutos por región|    DatabaseName, CollectionName, Region|   5M  |Tamaño de los datos  | Se utiliza para supervisar el uso total de los datos en la colección y la región, la granularidad mínima debe ser cinco minutos.|
| IndexUsage|   Uso de índice|    Bytes|  Total   |Uso del índice total notificado en una granularidad de cinco minutos por región|    DatabaseName, CollectionName, Region|   5M| Tamaño de índice| Se utiliza para supervisar el uso total de los datos en la colección y la región, la granularidad mínima debe ser cinco minutos. |
| DocumentQuota|    Cuota de documentos| Bytes|  Total|  Cuota de almacenamiento total notificado en una granularidad de cinco minutos por región. Aplicable a las colecciones de almacenamiento fijo| DatabaseName, CollectionName, Region|   5M  |Capacidad de almacenamiento|  Se utiliza para supervisar la cuota total en la colección y la región. La granularidad mínima debe ser cinco minutos.|
| DocumentCount|    Recuento de documentos| Número   |Total  |Recuento total de documentos notificado en una granularidad de cinco minutos por región|  DatabaseName, CollectionName, Region|   5M  |Recuento de documentos|Se utiliza para supervisar el recuento de documentos en la colección y la región. La granularidad mínima debe ser cinco minutos.|

### <a name="latency-metrics"></a>Métricas de latencia

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Uso |
|---|---|---|---|---|---| ---| ---|
| ReplicationLatency    | Latencia de replicación|  MilliSeconds|   Mínima, máxima y promedio | Latencia de replicación P99 entre regiones de origen y destino para la cuenta habilitada geográficamente| SourceRegion, TargetRegion| Todo | Se usa para supervisar la latencia de replicación P99 entre dos regiones cualesquiera para una cuenta replicada geográficamente. |

### <a name="availability-metrics"></a>Métricas de disponibilidad

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Asignación de métricas heredadas | Uso |
|---|---|---|---|---|---| ---| ---| ---|
| ServiceAvailability   | Disponibilidad del servicio| Percent |Mínimo y máximo|   Solicitudes de cuenta en una granularidad por hora|  |   1H  | Disponibilidad del servicio  | Se trata del porcentaje de solicitudes pasadas en total. Se considera que una solicitud ha dado error debido a un error del sistema si el código de estado es 410, 500 o 503. Se utiliza para supervisar la disponibilidad de la cuenta en una granularidad por hora. |

### <a name="cassandra-api-metrics"></a>Métricas de Cassandra API

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones| Granularidades de tiempo| Uso |
|---|---|---|---|---|---| ---| ---|
| CassandraRequests | Solicitudes de Cassandra |  Número|  Número|  Número de solicitudes de Cassandra API realizadas|  DatabaseName, CollectionName, ErrorCode, Region, OperationType, ResourceType|   Todo| Se usa para supervisar las solicitudes de Cassandra en una granularidad por minuto. Para obtener el promedio de solicitudes por segundo, utilice la agregación Count en un minuto y divida por 60.|
| CassandraRequestCharges|  Cargos de solicitud de Cassandra| Número|   Suma, mínimo, máximo y promedio| Unidades de solicitud consumidas por solicitudes de Cassandra API|   DatabaseName, CollectionName, Region, OperationType, ResourceType|  Todo| Se usa para supervisar las RU por minuto por una cuenta de Cassandra API.|
| CassandraConnectionClosures   | Cierres de conexión de Cassandra |Número| Número   |Número de conexiones de Cassandra cerradas|    ClosureReason, Region|  Todo | Se usa para supervisar la conectividad entre los clientes y Cassandra API de Azure Cosmos DB.|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Número|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishFailCount|Eventos erróneos|Número|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|UnmatchedEventCount|Eventos no coincidentes|Número|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|MatchedEventCount|Eventos coincidentes|Número|Total|Número total de eventos que coinciden con esta suscripción de eventos|Sin dimensiones|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Número|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType|
|DeliverySuccessCount|Eventos entregados|Número|Total|Número total de eventos que se han entregado a esta suscripción de eventos|Sin dimensiones|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Media|Duración del procesamiento de destino en milisegundos|Sin dimensiones|
|DroppedEventCount|Eventos quitados|Número|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|Sin dimensiones|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Número|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Número|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishFailCount|Eventos erróneos|Número|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|UnmatchedEventCount|Eventos no coincidentes|Número|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas (versión preliminar)|Número|Total|Solicitudes correctas de Microsoft.EventHub.  (versión preliminar)|EntityName|
|ServerErrors|Errores del servidor.  (versión preliminar)|Número|Total|Errores del servidor de Microsoft.EventHub.  (versión preliminar)|EntityName|
|UserErrors|Errores de usuario.  (versión preliminar)|Número|Total|Errores de usuario de Microsoft.EventHub.  (versión preliminar)|EntityName|
|QuotaExceededErrors|Cuota de errores superada.  (versión preliminar)|Número|Total|Cuota de errores superada de Microsoft.EventHub.  (versión preliminar)|EntityName|
|ThrottledRequests|Solicitudes limitadas.  (versión preliminar)|Número|Total|Solicitudes limitadas de Microsoft.EventHub.  (versión preliminar)|EntityName|
|IncomingRequests|Solicitudes entrantes (versión preliminar)|Número|Total|Solicitudes entrantes de Microsoft.EventHub.  (versión preliminar)|EntityName|
|IncomingMessages|Mensajes entrantes (versión preliminar)|Número|Total|Mensajes entrantes de Microsoft.EventHub.  (versión preliminar)|EntityName|
|OutgoingMessages|Mensajes salientes (versión preliminar)|Número|Total|Mensajes salientes de Microsoft.EventHub.  (versión preliminar)|EntityName|
|IncomingBytes|Bytes entrantes.  (versión preliminar)|Bytes|Total|Bytes entrantes de Microsoft.EventHub.  (versión preliminar)|EntityName|
|OutgoingBytes|Bytes salientes.  (versión preliminar)|Bytes|Total|Bytes salientes de Microsoft.EventHub.  (versión preliminar)|EntityName|
|ActiveConnections|ActiveConnections (versión preliminar)|Número|Media|Total de conexiones activas de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|ConnectionsOpened|Conexiones abiertas.  (versión preliminar)|Número|Media|Conexiones abiertas de Microsoft.EventHub.  (versión preliminar)|EntityName|
|ConnectionsClosed|Conexiones cerradas.  (versión preliminar)|Número|Media|Conexiones cerradas de Microsoft.EventHub.  (versión preliminar)|EntityName|
|CaptureBacklog|Capturar el trabajo pendiente.  (versión preliminar)|Número|Total|Capturar el trabajo pendiente de Microsoft.EventHub.  (versión preliminar)|EntityName|
|CapturedMessages|Mensajes capturados.  (versión preliminar)|Número|Total|Mensajes capturados de Microsoft.EventHub.  (versión preliminar)|EntityName|
|CapturedBytes|Bytes capturados.  (versión preliminar)|Bytes|Total|Bytes capturados de Microsoft.EventHub.  (versión preliminar)|EntityName|
|Tamaño|Tamaño (vista previa)|Bytes|Media|Tamaño de EventHub en bytes.  (versión preliminar)|EntityName|
|INREQS|Solicitudes entrantes|Número|Total|Total de solicitudes de envío entrantes de un espacio de nombres|Sin dimensiones|
|SUCCREQ|Solicitudes correctas|Número|Total|Número total de solicitudes correctas para un espacio de nombres|Sin dimensiones|
|FAILREQ|Solicitudes con error|Número|Total|Número total de solicitudes erróneas para un espacio de nombres|Sin dimensiones|
|SVRBSY|Errores de servidor ocupado|Número|Total|Errores totales de servidor ocupado para un espacio de nombres|Sin dimensiones|
|INTERR|Errores internos del servidor|Número|Total|Errores totales de servidor interno para un espacio de nombres|Sin dimensiones|
|MISCERR|Otros errores|Número|Total|Número total de solicitudes erróneas para un espacio de nombres|Sin dimensiones|
|INMSGS|Mensajes entrantes (en desuso)|Número|Total|Total de mensajes entrantes para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Mensajes entrantes|Sin dimensiones|
|EHINMSGS|Mensajes entrantes|Número|Total|Total de mensajes entrantes para un espacio de nombres|Sin dimensiones|
|OUTMSGS|Mensajes salientes (en desuso)|Número|Total|Total de mensajes salientes para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Mensajes salientes|Sin dimensiones|
|EHOUTMSGS|Mensajes salientes|Número|Total|Total de mensajes salientes para un espacio de nombres|Sin dimensiones|
|EHINMBS|Bytes de entrada (en desuso)|Bytes|Total|Rendimiento de mensajes entrantes del centro de eventos para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Bytes de entrada|Sin dimensiones|
|EHINBYTES|Bytes de entrada|Bytes|Total|Rendimiento de mensajes entrantes del centro de eventos para un espacio de nombres|Sin dimensiones|
|EHOUTMBS|Bytes de salida (en desuso)|Bytes|Total|Rendimiento de mensajes salientes del centro de eventos para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Bytes de salida|Sin dimensiones|
|EHOUTBYTES|Bytes de salida|Bytes|Total|Rendimiento de mensajes salientes del centro de eventos para un espacio de nombres|Sin dimensiones|
|EHABL|Mensajes de trabajo pendiente de archivado|Número|Total|Mensajes de archivo del centro de eventos en el trabajo pendiente para un espacio de nombres|Sin dimensiones|
|EHAMSGS|Mensajes de archivado|Número|Total|Mensajes archivados del centro de eventos en un espacio de nombres|Sin dimensiones|
|EHAMBS|Rendimiento de mensajes de archivado|Bytes|Total|Rendimiento de mensajes archivados del centro de eventos en un espacio de nombres|Sin dimensiones|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas (versión preliminar)|Número|Total|Solicitudes correctas de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|ServerErrors|Errores del servidor.  (versión preliminar)|Número|Total|Errores del servidor de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|UserErrors|Errores de usuario.  (versión preliminar)|Número|Total|Errores de usuario de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|QuotaExceededErrors|Cuota de errores superada.  (versión preliminar)|Número|Total|Cuota de errores superada de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|ThrottledRequests|Solicitudes limitadas.  (versión preliminar)|Número|Total|Solicitudes limitadas de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|IncomingRequests|Solicitudes entrantes (versión preliminar)|Número|Total|Solicitudes entrantes de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|IncomingMessages|Mensajes entrantes (versión preliminar)|Número|Total|Mensajes entrantes de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|OutgoingMessages|Mensajes salientes (versión preliminar)|Número|Total|Mensajes salientes de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|IncomingBytes|Bytes entrantes.  (versión preliminar)|Bytes|Total|Bytes entrantes de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|OutgoingBytes|Bytes salientes.  (versión preliminar)|Bytes|Total|Bytes salientes de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|ActiveConnections|ActiveConnections (versión preliminar)|Número|Media|Total de conexiones activas de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|ConnectionsOpened|Conexiones abiertas.  (versión preliminar)|Número|Media|Conexiones abiertas de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|ConnectionsClosed|Conexiones cerradas.  (versión preliminar)|Número|Media|Conexiones cerradas de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|CaptureBacklog|Capturar el trabajo pendiente.  (versión preliminar)|Número|Total|Capturar el trabajo pendiente de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|CapturedMessages|Mensajes capturados.  (versión preliminar)|Número|Total|Mensajes capturados de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|CapturedBytes|Bytes capturados.  (versión preliminar)|Bytes|Total|Bytes capturados de Microsoft.EventHub.  (versión preliminar)|Sin dimensiones|
|CPU|CPU (versión preliminar)|Percent|Máxima|Uso de la CPU para el clúster del centro de eventos como porcentaje|Rol|
|AvailableMemory|Memoria disponible (versión preliminar)|Número|Máxima|Memoria disponible para el clúster del centro de eventos en bytes|Rol|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|GatewayRequests|Solicitudes de puerta de enlace|Número|Total|Número de solicitudes de puerta de enlace|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Solicitudes de puerta de enlace categorizadas|Número|Total|Número de solicitudes de puerta de enlace por categorías (1xx/2xx/3xx/4xx/5xx)|ClusterDnsName, HttpStatus|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ObservedMetricValue|Valor de métrica observado|Número|Media|El valor calculado por el escalado automático cuando se ejecuta|MetricTriggerSource|
|MetricThreshold|Umbral de métrica|Número|Media|El umbral configurado de escalado automático cuando se ejecutó el escalado automático.|MetricTriggerRule|
|ObservedCapacity|Capacidad observada|Número|Media|La capacidad informada al escalado automático cuando se ejecutó.|Sin dimensiones|
|ScaleActionsInitiated|Acciones de escalado iniciadas|Número|Total|La dirección de la operación de escalado.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components
(Vista previa pública)

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|availabilityResults/duration|Duración de la prueba|MilliSeconds|Media|Duración de la prueba|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|billingMeters/telemetryCount|Recuento de puntos de datos|Número|Total|Número de puntos de datos enviados a este recurso de Application Insights. Esta métrica se procesa con una latencia de hasta dos horas.|billing/telemetryItemType, billing/telemetryItemSource|
|billingMeters/telemetrySize|Volumen de puntos de datos|Bytes|Total|Volumen de los datos enviados a este recurso de Application Insights. Esta métrica se procesa con una latencia de hasta dos horas.|billing/telemetryItemType, billing/telemetryItemSource|
|browserTimings/networkDuration|Tiempo de conexión de red de carga de página|MilliSeconds|Media|Tiempo transcurrido entre la solicitud del usuario y la conexión de red. Incluye la búsqueda de DNS y la conexión de transporte.|Sin dimensiones|
|browserTimings/processingDuration|Tiempo de procesamiento del cliente|MilliSeconds|Media|Tiempo transcurrido entre la recepción del último byte de un documento hasta que se carga el DOM. Todavía se pueden procesar solicitudes asincrónicas.|Sin dimensiones|
|browserTimings/receiveDuration|Tiempo de recepción de respuesta|MilliSeconds|Media|Tiempo transcurrido entre el primer y el último byte, o hasta la desconexión.|Sin dimensiones|
|browserTimings/sendDuration|Tiempo de solicitud de envío|MilliSeconds|Media|Tiempo transcurrido entre la conexión de red y la recepción del primer byte.|Sin dimensiones|
|browserTimings/totalDuration|Tiempo de carga de página del explorador|MilliSeconds|Media|Tiempo que transcurre entre la solicitud del usuario hasta que se cargan el DOM, las hojas de estilo, los scripts y las imágenes.|Sin dimensiones|
|dependencies/count|Llamadas de dependencia|Número|Número|Número de llamadas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, dependency/success, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/duration|Duración de la dependencia|MilliSeconds|Media|Duración de las llamadas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, dependency/success, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/failed|Errores de dependencia|Número|Número|Número de llamadas de dependencia erróneas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, operation/synthetic, cloud/roleInstance, cloud/roleName|
|pageViews/count|Vistas de página|Número|Número|Número de vistas de página.|operation/synthetic|
|pageViews/duration|Tiempo de carga de la vista de página|MilliSeconds|Media|Tiempo de carga de la vista de página|operation/synthetic|
|performanceCounters/requestExecutionTime|Tiempo de ejecución de solicitud HTTP|MilliSeconds|Media|Tiempo de ejecución de la solicitud más reciente.|cloud/roleInstance|
|performanceCounters/requestsInQueue|Solicitudes HTTP en la cola de la aplicación|Número|Media|Longitud de la cola de solicitudes de aplicación.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|Tasa de solicitudes HTTP|CountPerSecond|Media|Velocidad de todas las solicitudes a la aplicación por segundo desde ASP.NET.|cloud/roleInstance|
|performanceCounters/exceptionsPerSecond|Velocidad de excepciones|CountPerSecond|Media|Recuento de excepciones controladas y no controladas de las cuales se informó a Windows, incluidas las excepciones de .NET y las excepciones no administradas que se convierten en excepciones de .NET.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|Velocidad de E/S del proceso|BytesPerSecond|Media|Número total de bytes por segundo leídos y escritos en los archivos, la red y los dispositivos.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|CPU de procesos|Percent|Media|Porcentaje de tiempo transcurrido que todos los subprocesos del proceso han usado el procesador para ejecutar instrucciones. Puede variar entre 0 y 100. Esta métrica indica el rendimiento solo del proceso w3wp.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Tiempo de procesador|Percent|Media|Porcentaje de tiempo que invierte el procesador en subprocesos no inactivos.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Memoria disponible|Bytes|Media|Memoria física disponible de inmediato para su asignación a un proceso o para uso del sistema.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Bytes privados del proceso|Bytes|Media|Memoria asignada exclusivamente a los procesos de la aplicación supervisada.|cloud/roleInstance|
|requests/duration|Tiempo de respuesta del servidor|MilliSeconds|Media|Tiempo entre que se recibe una solicitud HTTP y se termina de enviar la respuesta.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/count|Solicitudes de servidor|Número|Número|Número de solicitudes HTTP completadas.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/failed|Error en las solicitudes|Número|Número|Número de solicitudes HTTP marcadas como erróneas. En la mayoría de los casos, se trata de solicitudes con un código de respuesta > = 400 y no igual a 401.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|exceptions/count|Excepciones|Número|Total|Recuento combinado de todas las excepciones no detectadas.|cloud/roleName, cloud/roleInstance, client/type|
|exceptions/browser|Excepciones de explorador|Número|Total|Recuento de excepciones no detectadas en el explorador.|Sin dimensiones|
|exceptions/server|Excepciones de servidor|Número|Total|Número de excepciones no detectadas producidas en la aplicación de servidor.|cloud/roleName, cloud/roleInstance|
|traces/count|Número de seguimientos|Número|Total|Número de documentos de seguimiento|trace/severityLevel, operation/synthetic, cloud/roleName, cloud/roleInstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ServiceApiHit|Número total de visitas de la API de servicio|Número|Número|Número total de visitas de la API de servicio|ActivityType, ActivityName|
|ServiceApiLatency|Latencia general de la API de servicio|Milisegundos|Media|Latencia general de las solicitudes de la API de servicio|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Número total de resultados de la API de servicio|Número|Número|Número total de resultados de la API de servicio|ActivityType, ActivityName, StatusCode|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ClusterDataCapacityFactor|Uso de la caché|Percent|Media|Nivel de uso en el ámbito de clúster|Sin dimensiones|
|QueryDuration|Duración de la consulta|Milisegundos|Media|Duración de las consultas en segundos|QueryStatus|
|IngestionsLoadFactor|Uso de la ingesta|Percent|Media|Proporción de ranuras de ingesta usadas en el clúster|Sin dimensiones|
|IsEngineAnsweringQuery|Mantener conexión|Número|Media|La comprobación de integridad indica las respuestas del clúster a las consultas|Sin dimensiones|
|IngestCommandOriginalSizeInMb|Volumen de ingesta (en MB)|Número|Total|Volumen global de la ingesta de datos del clúster (en MB)|Sin dimensiones|
|EventAgeSeconds|Latencia de la ingesta (en segundos)|Segundos|Media|Tiempo de ingesta del origen (p. ej., el mensaje está en el centro de eventos) al clúster en segundos|Sin dimensiones|
|EventReceivedFromEventHub|Eventos procesados (para Event Hubs)|Número|Total|Número de eventos que ha procesado el clúster en la ingesta desde el centro de eventos|Sin dimensiones|
|IngestionResult|Resultado de la ingesta|Número|Número|Número de operaciones de ingesta|IngestionResultDetails|
|EngineCPU|CPU|Percent|Media|Nivel de uso de CPU|Sin dimensiones|

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Uso|Uso|Número|Número|Recuento de llamadas a la API|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|RunsStarted|Ejecuciones iniciadas|Número|Total|Número de ejecuciones de flujo de trabajo iniciadas|Sin dimensiones|
|RunsCompleted|Ejecuciones completadas|Número|Total|Número de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunsSucceeded|Ejecuciones correctas|Número|Total|Número de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunsFailed|Ejecuciones con errores|Número|Total|Número de ejecuciones de flujo de trabajo con errores|Sin dimensiones|
|RunsCancelled|Ejecuciones canceladas|Número|Total|Número de ejecuciones de flujo de trabajo canceladas|Sin dimensiones|
|RunLatency|Latencia de ejecución|Segundos|Media|Latencia de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunSuccessLatency|Latencia de ejecuciones correctas|Segundos|Media|Latencia de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunThrottledEvents|Ejecución de eventos limitados|Número|Total|Número de eventos limitados de desencadenador o acciones de flujo de trabajo|Sin dimensiones|
|RunFailurePercentage|Porcentaje de errores de ejecución|Percent|Total|Porcentaje de ejecuciones de flujo de trabajo con errores.|Sin dimensiones|
|ActionsStarted|Acciones iniciadas |Número|Total|Número de acciones de flujo de trabajo iniciadas|Sin dimensiones|
|ActionsCompleted|Acciones completadas |Número|Total|Número de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionsSucceeded|Acciones correctas |Número|Total|Número de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionsFailed|Acciones con errores|Número|Total|Número de acciones de flujo de trabajo con errores|Sin dimensiones|
|ActionsSkipped|Acciones omitidas |Número|Total|Número de acciones de flujo de trabajo omitidas|Sin dimensiones|
|ActionLatency|Latencia de acciones |Segundos|Media|Latencia de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionSuccessLatency|Latencia de acciones correctas |Segundos|Media|Latencia de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionThrottledEvents|Eventos limitados de acciones|Número|Total|Número de eventos limitados de acciones de flujo de trabajo|Sin dimensiones|
|TriggersStarted|Desencadenadores iniciados |Número|Total|Número de desencadenadores de flujo de trabajo iniciados|Sin dimensiones|
|TriggersCompleted|Desencadenadores completados |Número|Total|Número de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggersSucceeded|Desencadenadores correctos |Número|Total|Número de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggersFailed|Desencadenadores con errores |Número|Total|Número de desencadenadores de flujo de trabajo con errores|Sin dimensiones|
|TriggersSkipped|Desencadenadores omitidos|Número|Total|Número de desencadenadores de flujo de trabajo omitidos|Sin dimensiones|
|TriggersFired|Desencadenadores activados |Número|Total|Número de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggerLatency|Latencia de desencadenador |Segundos|Media|Latencia de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggerFireLatency|Latencia de desencadenador activado |Segundos|Media|Latencia de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggerSuccessLatency|Latencia de desencadenadores correctos |Segundos|Media|Latencia de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggerThrottledEvents|Eventos limitados de desencadenadores|Número|Total|Número de eventos limitados de desencadenador de flujo de trabajo|Sin dimensiones|
|BillableActionExecutions|Ejecuciones de acciones facturables|Número|Total|Número de ejecuciones de acciones de flujo de trabajo que se facturan.|Sin dimensiones|
|BillableTriggerExecutions|Ejecuciones del desencadenador facturable|Número|Total|Número de ejecuciones del desencadenador de flujo de trabajo que se factura.|Sin dimensiones|
|TotalBillableExecutions|Total de ejecuciones facturables|Número|Total|Número de ejecuciones de flujo de trabajo que se factura.|Sin dimensiones|
|BillingUsageNativeOperation|Uso de facturación para las ejecuciones de la operación nativa|Número|Total|Número de ejecuciones de la operación nativa que se facturan.|Sin dimensiones|
|BillingUsageStandardConnector|Uso de facturación para las ejecuciones del conector estándar|Número|Total|Número de ejecuciones del conector estándar que se facturan.|Sin dimensiones|
|BillingUsageStorageConsumption|Uso de facturación para las ejecuciones de consumo de almacenamiento|Número|Total|Número de ejecuciones del consumo de almacenamiento que se facturan.|Sin dimensiones|
|BillingUsageNativeOperation|Uso de facturación para las ejecuciones de la operación nativa|Número|Total|Número de ejecuciones de la operación nativa que se facturan.|Sin dimensiones|
|BillingUsageStandardConnector|Uso de facturación para las ejecuciones del conector estándar|Número|Total|Número de ejecuciones del conector estándar que se facturan.|Sin dimensiones|
|BillingUsageStorageConsumption|Uso de facturación para las ejecuciones de consumo de almacenamiento|Número|Total|Número de ejecuciones del consumo de almacenamiento que se facturan.|Sin dimensiones|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/Volumes

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|AverageOtherLatency|Otra latencia media|ms/op|Media|Otra latencia media (que no sea de lectura ni escritura) en milisegundos por operación|Sin dimensiones|
|AverageReadLatency|Latencia de lectura media|ms/op|Media|Latencia de lectura media en milisegundos por operación|Sin dimensiones|
|AverageTotalLatency|Latencia total media|ms/op|Media|Latencia total media en milisegundos por operación|Sin dimensiones|
|AverageWriteLatency|Latencia de escritura media|ms/op|Media|Latencia de escritura media en milisegundos por operación|Sin dimensiones|
|FilesystemOtherOps|Otras operaciones del sistema de archivos|ops|Media|Número de otras operaciones del sistema de archivos (que no sean de lectura ni escritura)|Sin dimensiones|
|FilesystemReadOps|Operaciones de lectura del sistema de archivos|ops|Media|Número de operaciones de lectura del sistema de archivos|Sin dimensiones|
|FilesystemTotalOps|Total de operaciones del sistema de archivos|ops|Media|Suma de todas las operaciones del sistema de archivos|Sin dimensiones|
|FilesystemWriteOps|Operaciones de escritura del sistema de archivos|ops|Media|Número de operaciones de escritura del sistema de archivos|Sin dimensiones|
|IoBytesPerOtherOps|Bytes de E/S por otras operaciones|bytes/op|Media|Número de bytes de entrada/salida para otras operaciones (que no sean de lectura ni escritura)|Sin dimensiones|
|IoBytesPerReadOps|Bytes de E/S por operación de lectura|bytes/op|Media|Número de bytes de entrada/salida por operación de lectura|Sin dimensiones|
|IoBytesPerTotalOps|Bytes de E/S por operación en todas las operaciones|bytes/op|Media|Suma de toda la operación de bytes de entrada/salida|Sin dimensiones|
|IoBytesPerWriteOps|Bytes de E/S por operación de escritura|bytes/op|Media|Número de bytes de entrada/salida por operación de lectura|Sin dimensiones|
|OtherIops|Otras IOPS|operations/second|Media|Otra operación de entrada/salida por segundo|Sin dimensiones|
|OtherThroughput|Otro rendimiento|MBps|Media|Otro rendimiento (que no sea de lectura ni escritura) en megabytes por segundo|Sin dimensiones|
|ReadIops|IOPS de lectura|operations/second|Media|Operaciones de entrada/salida de lectura por segundo|Sin dimensiones|
|ReadThroughput|Rendimiento de lectura|MBps|Media|Rendimiento de lectura en megabytes por segundo|Sin dimensiones|
|TotalIops|Total de IOPS|operations/second|Media|Suma de todas las operaciones de entrada/salida por segundo|Sin dimensiones|
|TotalThroughput|Rendimiento total|MBps|Media|Suma de todo el rendimiento en megabytes por segundo|Sin dimensiones|
|VolumeAllocatedSize|Tamaño asignado del volumen|bytes|Media|Tamaño asignado del volumen (no los bytes usados reales)|Sin dimensiones|
|VolumeLogicalSize|Tamaño lógico del volumen|bytes|Media|Tamaño lógico del volumen (bytes usados)|Sin dimensiones|
|VolumeSnapshotSize|Tamaño de instantánea de volumen|bytes|Media|Tamaño de todas las instantáneas del volumen|Sin dimensiones|
|WriteIops|IOPS de escritura|operations/second|Media|Operaciones de entrada/salida de escritura por segundo|Sin dimensiones|
|WriteThroughput|Rendimiento de escritura|MBps|Media|Rendimiento de escritura en megabytes por segundo|Sin dimensiones|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Tamaño asignado del grupo de volumen|bytes|Media|Tamaño asignado del grupo (no los bytes usados reales)|Sin dimensiones|
|VolumePoolAllocatedUsed|Uso asignado del grupo de volumen|bytes|Media|Tamaño usado asignado del grupo|Sin dimensiones|
|VolumePoolTotalLogicalSize|Tamaño lógico total del grupo de volumen|bytes|Media|Suma del tamaño lógico de todos los volúmenes que pertenecen al grupo|Sin dimensiones|
|VolumePoolTotalSnapshotSize|Tamaño de instantánea total del grupo de volumen|bytes|Media|Suma de todas las instantáneas del grupo|Sin dimensiones|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BytesSentRate|Bytes enviados|Número|Total|Número de bytes de la interfaz de red enviados|Sin dimensiones|
|BytesReceivedRate|Bytes recibidos|Número|Total|Número de bytes de la interfaz de red recibidos|Sin dimensiones|
|PacketsSentRate|Paquetes enviados|Número|Total|Número de paquetes de la interfaz de red enviados|Sin dimensiones|
|PacketsReceivedRate|Paquetes recibidos|Número|Total|Número de paquetes de la interfaz de red recibidos|Sin dimensiones|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|VipAvailability|Disponibilidad de la ruta de acceso de datos|Número|Media|Disponibilidad media de la ruta de acceso de datos de Load Balancer por duración|FrontendIPAddress, FrontendPort|
|DipAvailability|Estado del sondeo de mantenimiento|Número|Media|Estado del sondeo de mantenimiento medio de Load Balancer por duración|ProtocolType, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|ByteCount|Recuento de bytes|Número|Total|Número total de bytes transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|PacketCount|Recuento de paquetes|Número|Total|Número total de paquetes transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|SYNCount|Recuento de SYN|Número|Total|Número total de paquetes SYN transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|SnatConnectionCount|Recuento de conexiones SNAT|Número|Total|Número total de conexiones SNAT nuevas creadas en un período|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Puertos SNAT asignados (versión preliminar)|Número|Total|Número total de puertos SNAT asignados dentro del período de tiempo|FrontendIPAddress, BackendIPAddress, ProtocolType|
|UsedSnatPorts|Puertos SNAT usados (versión preliminar)|Número|Total|Número total de puertos SNAT usados dentro del período de tiempo|FrontendIPAddress, BackendIPAddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|QueryVolume|Volumen de consulta|Número|Total|Número de consultas que se sirven para una zona DNS|Sin dimensiones|
|RecordSetCount|Recuento de conjunto de registros|Número|Máxima|Número de conjuntos de registros de una zona DNS|Sin dimensiones|
|RecordSetCapacityUtilization|Uso de capacidad de conjunto de registros|Percent|Máxima|Porcentaje de capacidad de conjunto de registros que usa una zona DNS|Sin dimensiones|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|PacketsInDDoS|Paquetes entrantes de DDoS|CountPerSecond|Máxima|Paquetes entrantes de DDoS|Sin dimensiones|
|PacketsDroppedDDoS|Paquetes entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes entrantes de DDoS descartados|Sin dimensiones|
|PacketsForwardedDDoS|Paquetes entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes entrantes de DDoS reenviados|Sin dimensiones|
|TCPPacketsInDDoS|Paquetes TCP entrantes de DDoS|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS|Sin dimensiones|
|TCPPacketsDroppedDDoS|Paquetes TCP entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS descartados|Sin dimensiones|
|TCPPacketsForwardedDDoS|Paquetes TCP entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS reenviados|Sin dimensiones|
|UDPPacketsInDDoS|Paquetes UDP entrantes de DDoS|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS|Sin dimensiones|
|UDPPacketsDroppedDDoS|Paquetes UDP entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS descartados|Sin dimensiones|
|UDPPacketsForwardedDDoS|Paquetes UDP entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS reenviados|Sin dimensiones|
|BytesInDDoS|Bytes de entrada de DDoS|BytesPerSecond|Máxima|Bytes de entrada de DDoS|Sin dimensiones|
|BytesDroppedDDoS|Bytes de entrada de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada de DDoS descartados|Sin dimensiones|
|BytesForwardedDDoS|Bytes de entrada de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada de DDoS reenviados|Sin dimensiones|
|TCPBytesInDDoS|Bytes de entrada TCP de DDoS|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS|Sin dimensiones|
|TCPBytesDroppedDDoS|Bytes de entrada TCP de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS descartados|Sin dimensiones|
|TCPBytesForwardedDDoS|Bytes de entrada TCP de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS reenviados|Sin dimensiones|
|UDPBytesInDDoS|Bytes de entrada UDP de DDoS|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS|Sin dimensiones|
|UDPBytesDroppedDDoS|Bytes de entrada UDP de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS descartados|Sin dimensiones|
|UDPBytesDroppedDDoS|Bytes de entrada UDP de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS reenviados|Sin dimensiones|
|IfUnderDDoSAttack|Bajo ataque de DDoS o no|Número|Máxima|Bajo ataque de DDoS o no|Sin dimensiones|
|DDoSTriggerTCPPackets|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|Sin dimensiones|
|DDoSTriggerUDPPackets|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|Sin dimensiones|
|DDoSTriggerSYNPackets|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|Sin dimensiones|
|VipAvailability|Disponibilidad de la ruta de acceso de datos|Número|Media|Disponibilidad media de la dirección IP por duración|Port|
|ByteCount|Recuento de bytes|Número|Total|Número total de bytes transmitidos en un período|Port, Direction|
|PacketCount|Recuento de paquetes|Número|Total|Número total de paquetes transmitidos en un período|Port, Direction|
|SynCount|Recuento de SYN|Número|Total|Número total de paquetes SYN transmitidos en un período|Port, Direction|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Throughput|Throughput|BytesPerSecond|Total|Número de bytes por segundo que ha ofrecido Application Gateway|Sin dimensiones|
|UnhealthyHostCount|Recuento de hosts con estado incorrecto|Número|Media|Número de hosts de back-end con estado incorrecto|BackendSettingsPool|
|HealthyHostCount|Recuento de hosts con estado correcto|Número|Media|Número de hosts de back-end con estado correcto|BackendSettingsPool|
|TotalRequests|Total de solicitudes|Número|Total|Recuento de solicitudes correctas que ha servido Application Gateway|BackendSettingsPool|
|FailedRequests|Solicitudes con error|Número|Total|Recuento de solicitudes con error que ha servido Application Gateway|BackendSettingsPool|
|ResponseStatus|Estado de respuesta|Número|Total|Estado de respuesta HTTP devuelto por Application Gateway|HttpStatusGroup|
|CurrentConnections|Conexiones actuales|Número|Total|Recuento de conexiones actuales establecidas con Application Gateway|Sin dimensiones|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|AverageBandwidth|Ancho de banda S2S de puerta de enlace|BytesPerSecond|Media|Ancho de banda de sitio a sitio medio de una puerta de enlace en bytes por segundo|Sin dimensiones|
|P2SBandwidth|Ancho de banda P2S de puerta de enlace|BytesPerSecond|Media|Ancho de banda de punto a sitio medio de una puerta de enlace en bytes por segundo|Sin dimensiones|
|P2SConnectionCount|Recuento de conexiones P2S|Número|Máxima|Recuento de conexiones de punto a sitio de una puerta de enlace|Protocolo|
|TunnelAverageBandwidth|Ancho de banda de túnel|BytesPerSecond|Media|Media de ancho de banda de un túnel en bytes por segundo|ConnectionName, RemoteIP|
|TunnelEgressBytes|Bytes de salida de túnel|Bytes|Total|Bytes de salida de un túnel|ConnectionName, RemoteIP|
|TunnelIngressBytes|Bytes de salida de túnel|Bytes|Total|Bytes de entrada de un túnel|ConnectionName, RemoteIP|
|TunnelEgressPackets|Paquetes de salida de túnel|Número|Total|Recuento de paquetes salientes de un túnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|Paquetes de entrada de túnel|Número|Total|Recuento de paquetes entrantes de un túnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de salida|Número|Total|Recuento de paquetes de salida con error de coincidencia del selector de tráfico de un túnel|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de entrada|Número|Total|Recuento de paquetes entrantes con error de coincidencia del selector de tráfico de un túnel|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Media|Bits que ingresan a Azure por segundo|Sin dimensiones|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Media|Bits que egresan de Azure por segundo|Sin dimensiones|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Media|Bits que ingresan a Azure por segundo|Sin dimensiones|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Media|Bits que egresan de Azure por segundo|Sin dimensiones|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Media|Bits que ingresan a Azure por segundo|Sin dimensiones|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Media|Bits que egresan de Azure por segundo|Sin dimensiones|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|QpsByEndpoint|Consultas por punto de conexión devueltas|Número|Total|Número de veces que se devolvió un punto de conexión de Traffic Manager en el período dado|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Estado de punto de conexión por punto de conexión|Número|Máxima|1 si el estado de sondeo de un punto de conexión es “Habilitado”, 0 en caso contrario.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ProbesFailedPercent|% de sondeos con error|Percent|Media|% de sondeos de supervisión de conectividad con error|Sin dimensiones|
|AverageRoundtripMs|Prom. Tiempo de ida y vuelta (ms)|MilliSeconds|Media|Promedio de tiempo de ida y vuelta de red (ms) para los sondeos de supervisión de conectividad que se envían entre el origen y el destino|Sin dimensiones|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|RequestCount|Recuento de solicitudes|Número|Total|Número de solicitudes de cliente que atiende el proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Tamaño de la solicitud|Bytes|Total|Número de bytes enviados como solicitudes de clientes al proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Tamaño de la respuesta|Bytes|Total|Número de bytes enviados como respuestas del proxy HTTP/S a los clientes|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Recuento de solicitudes de back-end|Número|Total|Número de solicitudes enviadas del proxy HTTP/S a los back-end|HttpStatus, HttpStatusGroup, Backend|
|BackendRequestLatency|Latencia de las solicitudes de back-end|MilliSeconds|Media|Tiempo calculado desde que el proxy HTTP/S ha enviado la solicitud al back-end hasta que el proxy HTTP/S ha recibido el último byte de respuesta del back-end|Back-end|
|TotalLatency|Latencia total|MilliSeconds|Media|Tiempo calculado desde que el proxy HTTP/S ha recibido la solicitud de cliente hasta que el cliente ha reconocido el último byte de respuesta del proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Porcentaje de estado del back-end|Percent|Media|El porcentaje de sondeos de estado correctos del proxy HTTP/S a los back-ends|Backend, BackendPool|
|WebApplicationFirewallRequestCount|Recuento de solicitudes del firewall de aplicaciones web|Número|Total|Número de solicitudes de cliente procesadas por el firewall de aplicaciones web|PolicyName, RuleName, Action|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|registration.all|Operaciones de registro|Número|Total|El recuento de todas las operaciones de registro correctas (creaciones, actualizaciones, consultas y eliminaciones). |Sin dimensiones|
|registration.create|Operaciones de creación de registros|Número|Total|El recuento de todas las creaciones de registros correctas.|Sin dimensiones|
|registration.update|Operaciones de actualización de registros|Número|Total|El recuento de todas las actualizaciones de registros correctas.|Sin dimensiones|
|registration.get|Operaciones de lectura de registros|Número|Total|El recuento de todas las consultas de registros correctas.|Sin dimensiones|
|registration.delete|Operaciones de eliminación de registros|Número|Total|El recuento de todas las eliminaciones de registros correctas.|Sin dimensiones|
|incoming|Mensajes entrantes|Número|Total|El recuento de todas las llamadas a la API de envío. |Sin dimensiones|
|incoming.scheduled|Notificaciones push programadas enviadas|Número|Total|Notificaciones push programadas canceladas|Sin dimensiones|
|incoming.scheduled.cancel|Notificaciones push programadas canceladas|Número|Total|Notificaciones push programadas canceladas|Sin dimensiones|
|scheduled.pending|Notificaciones programadas pendientes|Número|Total|Notificaciones programadas pendientes|Sin dimensiones|
|installation.all|Operaciones de administración de instalaciones|Número|Total|Operaciones de administración de instalaciones|Sin dimensiones|
|installation.get|Obtener operaciones de instalación|Número|Total|Obtener operaciones de instalación|Sin dimensiones|
|installation.upsert|Crear o actualizar operaciones de instalación|Número|Total|Crear o actualizar operaciones de instalación|Sin dimensiones|
|installation.patch|Aplicar revisión a operaciones de instalación|Número|Total|Aplicar revisión a operaciones de instalación|Sin dimensiones|
|installation.delete|Eliminar operaciones de instalación|Número|Total|Eliminar operaciones de instalación|Sin dimensiones|
|outgoing.allpns.success|Notificaciones correctas|Número|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.allpns.invalidpayload|Errores de carga útil|Número|Total|El recuento de las inserciones que provocaron un error porque el PNS devolvió un error de carga útil incorrecta.|Sin dimensiones|
|outgoing.allpns.pnserror|Errores del sistema de notificación externo|Número|Total|El recuento de inserciones que provocaron un error porque se produjo un problema al comunicarse con el PNS (excluye los problemas de autenticación).|Sin dimensiones|
|outgoing.allpns.channelerror|Errores de canal|Número|Total|El recuento de inserciones que provocaron un error porque el canal no era válido no está asociado a la aplicación correcta limitada o expirada.|Sin dimensiones|
|outgoing.allpns.badorexpiredchannel|Errores de canal incorrecto o expirado|Número|Total|El recuento de inserciones que provocaron un error porque el canal/token/registrationId del registro ha expirado o no es válido.|Sin dimensiones|
|outgoing.wns.success|Notificaciones de WNS correctas|Número|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.wns.invalidcredentials|Errores de autorización de WNS (credenciales no válidas)|Número|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas. (Windows Live no reconoce las credenciales).|Sin dimensiones|
|outgoing.wns.badchannel|Error de canal incorrecto de WNS|Número|Total|El recuento de inserciones que provocaron un error porque no se reconoció ChannelURI en el registro (estado de WNS: 404 No encontrado).|Sin dimensiones|
|outgoing.wns.expiredchannel|Error de canal expirado de WNS|Número|Total|El recuento de inserciones que provocaron un error porque ChannelURI expiró (estado de WNS: 410 Ya no existe).|Sin dimensiones|
|outgoing.wns.throttled|Notificaciones limitadas de WNS|Número|Total|El recuento de inserciones que provocaron un error porque WNS limita esta aplicación (estado de WNS: 406 No aceptable).|Sin dimensiones|
|outgoing.wns.tokenproviderunreachable|Errores de autorización de WNS (inaccesible)|Número|Total|No se puede acceder a Windows Live.|Sin dimensiones|
|outgoing.wns.invalidtoken|Errores de autorización de WNS (token no válido)|Número|Total|El token que se proporciona a WNS no es válido (estado de WNS: 401 No autorizado).|Sin dimensiones|
|outgoing.wns.wrongtoken|Errores de autorización de WNS (token erróneo)|Número|Total|El token que se proporciona a WNS es válido, pero para otra aplicación (estado de WNS: 403 Prohibido). Esto puede ocurrir si la instancia de ChannelURI en el registro está asociada a otra aplicación. Compruebe que la aplicación cliente está asociada a la misma aplicación cuyas credenciales están en el centro de notificaciones.|Sin dimensiones|
|outgoing.wns.invalidnotificationformat|Formato de notificación no válido de WNS|Número|Total|El formato de la notificación no es válido (estado de WNS: 400). Tenga en cuenta que WNS no rechaza todas las cargas útiles no válidas.|Sin dimensiones|
|outgoing.wns.invalidnotificationsize|Error de tamaño de notificación no válido de WNS|Número|Total|La carga útil de la notificación es demasiado grande (estado de WNS: 413).|Sin dimensiones|
|outgoing.wns.channelthrottled|Canal de WNS limitado|Número|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (encabezado de respuesta WNS: X-WNS-NotificationStatus:channelThrottled).|Sin dimensiones|
|outgoing.wns.channeldisconnected|Canal de WNS desconectado|Número|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (encabezado de respuesta WNS: X-WNS-DeviceConnectionStatus: desconectado).|Sin dimensiones|
|outgoing.wns.dropped|Notificaciones descartadas de WNS|Número|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (X-WNS-NotificationStatus: descartado pero no X-WNS-DeviceConnectionStatus: desconectado).|Sin dimensiones|
|outgoing.wns.pnserror|Errores de WNS|Número|Total|La notificación no se entregó debido a errores de comunicación con WNS.|Sin dimensiones|
|outgoing.wns.authenticationerror|Errores de autenticación de WNS|Número|Total|La notificación no se entregó debido a errores de comunicación con Windows Live, credenciales no válidas o token erróneo.|Sin dimensiones|
|outgoing.apns.success|Notificaciones de APNS correctas|Número|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.apns.invalidcredentials|Errores de autorización de APNS|Número|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.apns.badchannel|Error de canal incorrecto de APNS|Número|Total|El recuento de inserciones que provocaron un error porque el token no es válido (código de estado de APNS: 8).|Sin dimensiones|
|outgoing.apns.expiredchannel|Error de canal expirado de APNS|Número|Total|El recuento de tokens invalidados por el canal de comentarios de APNS.|Sin dimensiones|
|outgoing.apns.invalidnotificationsize|Error de tamaño de notificación no válido de APNS|Número|Total|El recuento de inserciones que provocaron un error porque la carga útil era demasiado grande (código de estado de APNS: 7).|Sin dimensiones|
|outgoing.apns.pnserror|Errores de APNS|Número|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con APNS.|Sin dimensiones|
|outgoing.gcm.success|Notificaciones de GCM correctas|Número|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.gcm.invalidcredentials|Errores de autorización de GCM (credenciales no válidas)|Número|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.gcm.badchannel|Error de canal incorrecto de GCM|Número|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro no se ha reconocido (resultado de GCM: registro no válido).|Sin dimensiones|
|outgoing.gcm.expiredchannel|Error de canal expirado de GCM|Número|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro ha caducado (resultado de GCM: NotRegistered).|Sin dimensiones|
|outgoing.gcm.throttled|Notificaciones limitadas de GCM|Número|Total|El recuento de inserciones que provocaron un error porque GCM limitó esta aplicación (código de estado de GCM: 501-599 o resultado: no disponible).|Sin dimensiones|
|outgoing.gcm.invalidnotificationformat|Formato de notificación no válido de GCM|Número|Total|El recuento de inserciones que provocaron un error porque la carga útil no tenía el formato correcto (resultado de GCM: InvalidDataKey o InvalidTtl).|Sin dimensiones|
|outgoing.gcm.invalidnotificationsize|Error de tamaño de notificación no válido de GCM|Número|Total|El recuento de inserciones que provocaron un error porque la carga útil era demasiado grande (resultado de GCM: MessageTooBig).|Sin dimensiones|
|outgoing.gcm.wrongchannel|Error de canal incorrecto de GCM|Número|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro no está asociado a la aplicación actual (resultado de GCM: InvalidPackageName).|Sin dimensiones|
|outgoing.gcm.pnserror|Errores de GCM|Número|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con GCM.|Sin dimensiones|
|outgoing.gcm.authenticationerror|Errores de autenticación de GCM|Número|Total|El recuento de inserciones que provocaron un error debido a que el PNS no aceptó las credenciales proporcionadas, las credenciales están bloqueadas o el Id. del remitente no está configurado correctamente en la aplicación (resultado de GCM: MismatchedSenderId).|Sin dimensiones|
|outgoing.mpns.success|Notificaciones de MPNS correctas|Número|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.mpns.invalidcredentials|Credenciales no válidas de MPNS|Número|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.mpns.badchannel|Error de canal incorrecto de MPNS|Número|Total|El recuento de inserciones que provocaron un error porque no se reconoció ChannelURI en el registro (estado de MPNS: 404 No encontrado).|Sin dimensiones|
|outgoing.mpns.throttled|Notificaciones limitadas de MPNS|Número|Total|El recuento de inserciones que provocaron un error porque MPNS limita esta aplicación (MPNS de WNS: 406 No aceptable).|Sin dimensiones|
|outgoing.mpns.invalidnotificationformat|Formato de notificación no válido de MPNS|Número|Total|El recuento de inserciones que provocaron un error porque la carga útil de la notificación era demasiado grande.|Sin dimensiones|
|outgoing.mpns.channeldisconnected|Canal de MPNS desconectado|Número|Total|El recuento de inserciones que provocaron un error porque se desconectó la instancia ChannelURI en el registro (estado de MPNS: 412 No encontrado).|Sin dimensiones|
|outgoing.mpns.dropped|Notificaciones descartadas de MPNS|Número|Total|El recuento de inserciones que eliminó MPNS (encabezado de respuesta MPNS: X-NotificationStatus: QueueFull o Suprimido).|Sin dimensiones|
|outgoing.mpns.pnserror|Errores de MPNS|Número|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con MPNS.|Sin dimensiones|
|outgoing.mpns.authenticationerror|Errores de autenticación de MPNS|Número|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|notificationhub.pushes|Todas las notificaciones salientes|Número|Total|Todas las notificaciones salientes del Centro de notificaciones|Sin dimensiones|
|incoming.all.requests|Todas las solicitudes entrantes|Número|Total|Solicitudes entrantes totales de un centro de notificaciones|Sin dimensiones|
|incoming.all.failedrequests|Todas las solicitudes entrantes con error|Número|Total|Solicitudes entrantes totales con error de un centro de notificaciones|Sin dimensiones|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Average_% Free Inodes|% de Inodes libres|Número|Media|Average_% Free Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|% de espacio libre|Número|Media|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Inodes|% de Inodes usados|Número|Media|Average_% Used Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Space|% espacio usado|Número|Media|Average_% Used Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Read Bytes/sec|Bytes de lectura de disco/s |Número|Media|Average_Disk Read Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Lecturas de disco/s |Número|Media|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Transferencias de disco/s|Número|Media|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Write Bytes/sec|  Bytes de escritura en disco/s|Número|Media|Average_Disk Write Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec| Escrituras en disco/s|Número|Media|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Megabytes libres|Número|Media|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Logical Disk Bytes/sec|Bytes de disco lógico/s|Número|Media|Average_Logical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Memory|% de memoria disponible|Número|Media|Average_% Available Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Swap Space|% de espacio de intercambio disponible|Número|Media|Average_% Available Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Memory|% de memoria usada|Número|Media|Average_% Used Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Swap Space|% de espacio de intercambio usado|Número|Media|Average_% Used Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Memory|MB de memoria disponibles|Número|Media|Average_Available MBytes Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Swap|Intercambio de MB disponibles|Número|Media|Average_Available MBytes Swap|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Reads/sec|Lecturas de página/s|Número|Media|Average_Page Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Writes/sec|Escrituras de página/s|Número|Media|Average_Page Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pages/sec|Páginas/s|Número|Media|Average_Pages/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used MBytes Swap Space|Espacio de intercambio de MB usado|Número|Media|Average_Used MBytes Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory MBytes|MB de memoria usados|Número|Media|Average_Used Memory MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Transmitted|Número total de bytes transmitidos|Número|Media|Average_Total Bytes Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Received|Número total de bytes recibidos|Número|Media|Average_Total Bytes Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes|Número total de bytes|Número|Media|Average_Total Bytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Transmitted|Número total de paquetes transmitidos|Número|Media|Average_Total Packets Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Received|Número total de paquetes recibidos|Número|Media|Average_Total Packets Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx Errors|Errores de Rx totales|Número|Media|Average_Total Rx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Tx Errors|Errores de Tx totales|Número|Media|Average_Total Tx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Collisions|Colisiones totales|Número|Media|Average_Total Collisions|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/lecturas|Prom. Segundos de disco/lecturas|Número|Media|Average_Avg. Segundos de disco/lecturas|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/transferencias|Prom. Segundos de disco/transferencias|Número|Media|Average_Avg. Segundos de disco/transferencias|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/escrituras|Prom. Segundos de disco/escrituras|Número|Media|Average_Avg. Segundos de disco/escrituras|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Physical Disk Bytes/sec|Bytes de disco físico/s|Número|Media|Average_Physical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct Privileged Time|Porcentaje de tiempo con privilegios|Número|Media|Average_Pct Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct User Time|Porcentaje de tiempo de usuario|Número|Media|Average_Pct User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory kBytes|Kilobytes de memoria usados|Número|Media|Average_Used Memory kBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Virtual Shared Memory|Memoria compartida virtual|Número|Media|Average_Virtual Shared Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% DPC Time|% de tiempo de DPC|Número|Media|Average_% DPC Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Idle Time|% de tiempo de inactividad|Número|Media|Average_% Idle Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Interrupt Time|% de tiempo de interrupción|Número|Media|Average_% Interrupt Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% IO Wait Time|% de tiempo de espera de E/S|Número|Media|Average_% IO Wait Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Nice Time|% de tiempo bueno|Número|Media|Average_% Nice Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Privileged Time|% de tiempo con privilegios|Número|Media|Average_% Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|% de tiempo de procesador|Número|Media|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% User Time|% de tiempo de usuario|Número|Media|Average_% User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Physical Memory|Memoria física libre|Número|Media|Average_Free Physical Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Space in Paging Files|Espacio libre en archivos de paginación|Número|Media|Average_Free Space in Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Virtual Memory|Memoria virtual libre|Número|Media|Average_Free Virtual Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processes|Procesos|Número|Media|Average_Processes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Size Stored In Paging Files|Tamaño almacenado en archivos de paginación|Número|Media|Average_Size Stored In Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Uptime|Tiempo de actividad|Número|Media|Average_Uptime|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Users|Usuarios|Número|Media|Average_Users|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/lecturas|Prom. Segundos de disco/lecturas|Número|Media|Average_Avg. Segundos de disco/lecturas|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/escrituras|Prom. Segundos de disco/escrituras|Número|Media|Average_Avg. Segundos de disco/escrituras|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Current Disk Queue Length|Longitud actual de cola de disco|Número|Media|Average_Current Disk Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Lecturas de disco/s |Número|Media|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Transferencias de disco/s|Número|Media|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec| Escrituras en disco/s|Número|Media|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Megabytes libres|Número|Media|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|% de espacio libre|Número|Media|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes|MB disponibles|Número|Media|Average_Available MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Committed Bytes In Use|% de bytes confirmados en uso|Número|Media|Average_% Committed Bytes In Use|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Received/sec|Bytes recibidos por segundo|Número|Media|Average_Bytes Received/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Sent/sec|Bytes enviados por segundo|Número|Media|Average_Bytes Sent/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Total/sec|Bytes totales por segundo|Número|Media|Average_Bytes Total/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|% de tiempo de procesador|Número|Media|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processor Queue Length|Longitud de cola del procesador|Número|Media|Average_Processor Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Latido|Latido|Número|Total|Latido|Computer, OSType, Version, SourceComputerId|
|Actualizar|Actualizar|Número|Media|Actualizar|Computer, Product, Classification, UpdateState, Optional, Approved|
|Evento|Evento|Número|Media|Evento|Source, EventLog, Computer, EventCategory, EventLevel, EventLevelName, EventID|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|QueryDuration|Duración de la consulta|Milisegundos|Media|Duración de la consulta DAX en el último intervalo|Sin dimensiones|
|QueryPoolJobQueueLength|Subprocesos: longitud de cola de trabajos del grupo de consultas|Número|Media|Número de trabajos en la cola del grupo de subprocesos de consultas.|Sin dimensiones|
|qpu_high_utilization_metric|Uso intensivo de QPU|Número|Total|Uso intensivo de QPU en el último minuto: 1 para un uso intensivo de QPU y 0 en caso contrario.|Sin dimensiones|
|memory_metric|Memoria|Bytes|Media|Memoria. Intervalo de 0-3 GB para A1, 0-5 GB para A2, 0-10 GB para A3, 0-25 GB para A4, 0-50 GB para A5 y 0-100 GB para A6|Sin dimensiones|
|memory_thrashing_metric|Paginación excesiva de memoria|Percent|Media|Paginación excesiva media de memoria.|Sin dimensiones|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Número|Total|Valor correcto de ListenerConnections de Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Número|Total|ClientError en ListenerConnections para Microsoft.Relay.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Número|Total|ServerError en ListenerConnections para Microsoft.Relay.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Número|Total|Valor correcto de SenderConnections para Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Número|Total|ClientError en SenderConnections para Microsoft.Relay.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Número|Total|ServerError en SenderConnections para Microsoft.Relay.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Número|Total|Valor total de ListenerConnections para Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Número|Total|Solicitudes totales de SenderConnections para Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Número|Total|Valor total de ActiveConnections para Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Número|Total|Valor total de ActiveListeners para Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Número|Total|Valor total de BytesTransferred para Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Número|Total|Valor total de ListenerDisconnects para Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Número|Total|Valor total de SenderDisconnects para Microsoft.Relay.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|SearchLatency|Latencia de búsqueda|Segundos|Media|Promedio de latencia de búsqueda para el servicio de búsqueda|Sin dimensiones|
|SearchQueriesPerSecond|Consultas de búsqueda por segundo|CountPerSecond|Media|Consultas de búsqueda por segundo para el servicio de búsqueda|Sin dimensiones|
|ThrottledSearchQueriesPercentage|Porcentaje de consultas de búsqueda limitadas|Percent|Media|Porcentaje de consultas de búsqueda limitadas para el servicio de búsqueda|Sin dimensiones|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas (versión preliminar)|Número|Total|Número total de solicitudes correctas para un espacio de nombres (versión preliminar)|EntityName|
|ServerErrors|Errores del servidor.  (versión preliminar)|Número|Total|Errores del servidor para Microsoft.ServiceBus.  (versión preliminar)|EntityName|
|UserErrors|Errores de usuario.  (versión preliminar)|Número|Total|Errores de usuario para Microsoft.ServiceBus.  (versión preliminar)|EntityName|
|ThrottledRequests|Solicitudes limitadas.  (versión preliminar)|Número|Total|Solicitudes limitadas para Microsoft.ServiceBus.  (versión preliminar)|EntityName|
|IncomingRequests|Solicitudes entrantes (versión preliminar)|Número|Total|Solicitudes entrantes para Microsoft.ServiceBus.  (versión preliminar)|EntityName|
|IncomingMessages|Mensajes entrantes (versión preliminar)|Número|Total|Mensajes entrantes para Microsoft.ServiceBus.  (versión preliminar)|EntityName|
|OutgoingMessages|Mensajes salientes (versión preliminar)|Número|Total|Mensajes salientes para Microsoft.ServiceBus.  (versión preliminar)|EntityName|
|ActiveConnections|ActiveConnections (versión preliminar)|Número|Total|Total de conexiones activas para Microsoft.ServiceBus.  (versión preliminar)|Sin dimensiones|
|Tamaño|Tamaño (vista previa)|Bytes|Media|Tamaño de cola/tema en bytes.  (versión preliminar)|EntityName|
|error de Hadoop|Recuento de mensajes de una cola/tema.  (versión preliminar)|Número|Media|Recuento de mensajes de una cola/tema.  (versión preliminar)|EntityName|
|ActiveMessages|Recuento de mensajes activos de una cola/tema.  (versión preliminar)|Número|Media|Recuento de mensajes activos de una cola/tema.  (versión preliminar)|EntityName|
|CPUXNS|Uso de CPU por espacio de nombres|Percent|Máxima|Métrica de uso de CPU de espacio de nombres premium de Service Bus|Sin dimensiones|
|WSXNS|Uso de tamaño de memoria por espacio de nombres|Percent|Máxima|Métrica de uso de memoria de espacio de nombres premium de Service Bus|Sin dimensiones|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ConnectionCount|Número de conexiones|Número|Máxima|Cantidad de conexiones de usuario.|Punto de conexión|
|MessageCount|Número de mensajes|Número|Total|Cantidad total de mensajes.|Sin dimensiones|
|InboundTraffic|Tráfico entrante|Bytes|Total|Tráfico entrante del servicio|Sin dimensiones|
|OutboundTraffic|Tráfico saliente|Bytes|Total|Tráfico saliente del servicio|Sin dimensiones|
|UserErrors|Errores de usuario|Percent|Máxima|Porcentaje de errores de usuario|Sin dimensiones|
|SystemErrors|Errores de sistema|Percent|Máxima|Porcentaje de errores del sistema|Sin dimensiones|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Sin dimensiones|
|physical_data_read_percent|Porcentaje de E/S de datos|Percent|Media|Porcentaje de E/S de datos|Sin dimensiones|
|log_write_percent|Porcentaje de E/S de registro|Percent|Media|Porcentaje de E/S de registro|Sin dimensiones|
|dtu_consumption_percent|Porcentaje de DTU|Percent|Media|Porcentaje de DTU|Sin dimensiones|
|storage|Tamaño total de base de datos|Bytes|Máxima|Tamaño total de base de datos|Sin dimensiones|
|connection_successful|Conexiones correctas|Número|Total|Conexiones correctas|Sin dimensiones|
|connection_failed|Conexiones con errores|Número|Total|Conexiones con errores|Sin dimensiones|
|blocked_by_firewall|Bloqueado por el firewall|Número|Total|Bloqueado por el firewall|Sin dimensiones|
|deadlock|Interbloqueos|Número|Total|Interbloqueos|Sin dimensiones|
|storage_percent|Porcentaje de tamaño de base de datos|Percent|Máxima|Porcentaje de tamaño de base de datos|Sin dimensiones|
|xtp_storage_percent|Porcentaje de almacenamiento de OLTP en memoria|Percent|Media|Porcentaje de almacenamiento de OLTP en memoria|Sin dimensiones|
|workers_percent|Porcentaje de trabajos|Percent|Media|Porcentaje de trabajos|Sin dimensiones|
|sessions_percent|Porcentaje de sesiones|Percent|Media|Porcentaje de sesiones|Sin dimensiones|
|dtu_limit|Límite de DTU|Número|Media|Límite de DTU|Sin dimensiones|
|dtu_used|DTU utilizada|Número|Media|DTU utilizada|Sin dimensiones|
|dwu_limit|Límite de DWU|Número|Máxima|Límite de DWU|Sin dimensiones|
|dwu_consumption_percent|Porcentaje de DWU|Percent|Máxima|Porcentaje de DWU|Sin dimensiones|
|dwu_used|DWU utilizada|Número|Máxima|DWU utilizada|Sin dimensiones|
|dw_cpu_percent|Porcentaje de CPU de nivel de nodo de almacenamiento de datos|Percent|Media|Porcentaje de CPU de nivel de nodo de almacenamiento de datos|DwLogicalNodeId|
|dw_physical_data_read_percent|Porcentaje de E/S de datos de nivel de nodo de almacenamiento de datos|Percent|Media|Porcentaje de E/S de datos de nivel de nodo de almacenamiento de datos|DwLogicalNodeId|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Sin dimensiones|
|physical_data_read_percent|Porcentaje de E/S de datos|Percent|Media|Porcentaje de E/S de datos|Sin dimensiones|
|log_write_percent|Porcentaje de E/S de registro|Percent|Media|Porcentaje de E/S de registro|Sin dimensiones|
|dtu_consumption_percent|Porcentaje de DTU|Percent|Media|Porcentaje de DTU|Sin dimensiones|
|storage_percent|Porcentaje de almacenamiento|Percent|Media|Porcentaje de almacenamiento|Sin dimensiones|
|workers_percent|Porcentaje de trabajos|Percent|Media|Porcentaje de trabajos|Sin dimensiones|
|sessions_percent|Porcentaje de sesiones|Percent|Media|Porcentaje de sesiones|Sin dimensiones|
|eDTU_limit|Límite de eDTU|Número|Media|Límite de eDTU|Sin dimensiones|
|storage_limit|Límite de almacenamiento|Bytes|Media|Límite de almacenamiento|Sin dimensiones|
|eDTU_used|eDTU utilizada|Número|Media|eDTU utilizada|Sin dimensiones|
|storage_used|Almacenamiento utilizado|Bytes|Media|Almacenamiento utilizado|Sin dimensiones|
|xtp_storage_percent|Porcentaje de almacenamiento de OLTP en memoria|Percent|Media|Porcentaje de almacenamiento de OLTP en memoria|Sin dimensiones|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|virtual_core_count|Recuento de núcleos virtuales|Número|Media|Recuento de núcleos virtuales|Sin dimensiones|
|avg_cpu_percent|Porcentaje de CPU medio|Percent|Media|Porcentaje de CPU medio|Sin dimensiones|
|reserved_storage_mb|Espacio de almacenamiento reservado|Número|Media|Espacio de almacenamiento reservado|Sin dimensiones|
|storage_space_used_mb|Espacio de almacenamiento usado|Número|Media|Espacio de almacenamiento usado|Sin dimensiones|
|io_requests|Recuento de solicitudes de E/S|Número|Media|Recuento de solicitudes de E/S|Sin dimensiones|
|io_bytes_read|Bytes de E/S leídos|Bytes|Media|Bytes de E/S leídos|Sin dimensiones|
|io_bytes_written|Bytes de E/S escritos|Bytes|Media|Bytes de E/S escritos|Sin dimensiones|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|UsedCapacity|Capacidad usada|Bytes|Total|Capacidad usada de la cuenta|Sin dimensiones|
|Transacciones|Transacciones|Número|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BlobCapacity|Capacidad de Blob|Bytes|Total|La cantidad de almacenamiento que ha utilizado el Blob service de la cuenta de almacenamiento, en bytes.|BlobType|
|BlobCount|Recuento de blobs|Número|Total|El número de blobs en el Blob service de la cuenta de almacenamiento.|BlobType|
|ContainerCount|Recuento de contenedores de blobs|Número|Media|El número de contenedores en el Blob service de la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Transacciones|Número|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|FileCapacity|Capacidad de File|Bytes|Media|La cantidad de almacenamiento que ha utilizado el servicio File de la cuenta de almacenamiento, en bytes.|Sin dimensiones|
|FileCount|Recuento de archivos|Número|Media|El número de archivos en el servicio File de la cuenta de almacenamiento.|Sin dimensiones|
|FileShareCount|Recuento de recursos compartidos de archivos|Número|Media|El número de recursos compartidos de archivos en el servicio File de la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Transacciones|Número|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|QueueCapacity|Capacidad de Queue|Bytes|Media|La cantidad de almacenamiento que ha utilizado el Queue service de la cuenta de almacenamiento, en bytes.|Sin dimensiones|
|QueueCount|Recuento de colas|Número|Media|El número de colas en el Queue service de la cuenta de almacenamiento.|Sin dimensiones|
|QueueMessageCount|Recuento de mensajes de Queue|Número|Media|El número aproximado de mensajes de cola en el Queue service de la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Transacciones|Número|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TableCapacity|Capacidad de Table|Bytes|Media|La cantidad de almacenamiento que ha utilizado el servicio Table de la cuenta de almacenamiento, en bytes.|Sin dimensiones|
|TableCount|Recuento de tablas|Número|Media|El número de tablas en el servicio Table de la cuenta de almacenamiento.|Sin dimensiones|
|TableEntityCount|Recuento de entidades de Table|Número|Media|El número de entidades de tabla en el servicio Table de la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Transacciones|Número|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ResourceUtilization|SU % uso|Percent|Máxima|SU % uso|Sin dimensiones|
|InputEvents|Eventos de entrada|Número|Total|Eventos de entrada|Sin dimensiones|
|InputEventBytes|Bytes del evento de entrada|Bytes|Total|Bytes del evento de entrada|Sin dimensiones|
|LateInputEvents|Eventos de entrada retrasada|Número|Total|Eventos de entrada retrasada|Sin dimensiones|
|OutputEvents|Eventos de salida|Número|Total|Eventos de salida|Sin dimensiones|
|ConversionErrors|Errores de conversión de datos|Número|Total|Errores de conversión de datos|Sin dimensiones|
|Errors|Errores de tiempo de ejecución|Número|Total|Errores de tiempo de ejecución|Sin dimensiones|
|DroppedOrAdjustedEvents|Eventos de fuera de orden|Número|Total|Eventos de fuera de orden|Sin dimensiones|
|AMLCalloutRequests|Solicitudes de función|Número|Total|Solicitudes de función|Sin dimensiones|
|AMLCalloutFailedRequests|Solicitudes de función con errores|Número|Total|Solicitudes de función con errores|Sin dimensiones|
|AMLCalloutInputEvents|Eventos de función|Número|Total|Eventos de función|Sin dimensiones|
|DeserializationError|Errores de deserialización de entrada|Número|Total|Errores de deserialización de entrada|Sin dimensiones|
|EarlyInputEvents|Primeros eventos de entrada|Número|Total|Primeros eventos de entrada|Sin dimensiones|
|OutputWatermarkDelaySeconds|Retraso de la marca de agua|Segundos|Máxima|Retraso de la marca de agua|Sin dimensiones|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|IngressReceivedMessages|Mensajes de entrada recibidos|Número|Total|Número de mensajes leídos desde todos los orígenes de eventos de Event Hubs o IoT Hub|Sin dimensiones|
|IngressReceivedInvalidMessages|Mensajes no válidos de entrada recibidos|Número|Total|Número de mensajes no válidos leídos desde todos los orígenes de eventos de Event Hubs o IoT Hub|Sin dimensiones|
|IngressReceivedBytes|Bytes de entrada recibidos|Bytes|Total|Número de bytes leídos desde todos los orígenes de eventos|Sin dimensiones|
|IngressStoredBytes|Bytes de entrada almacenados|Bytes|Total|Tamaño total de los eventos almacenados correctamente y disponibles para su consulta|Sin dimensiones|
|IngressStoredEvents|Eventos de entrada almacenados|Número|Total|Número de eventos acoplados que se procesaron correctamente y que están disponibles para su consulta|Sin dimensiones|
|IngressReceivedMessagesTimeLag|Tiempo de retardo de los mensajes de entrada recibidos|Segundos|Máxima|Diferencia entre el momento en el que el mensaje se pone en la cola en el origen del evento y el momento en que se procesa en la entrada|Sin dimensiones|
|IngressReceivedMessagesCountLag|Intervalo de recuento de los mensajes de entrada recibidos|Número|Media|Diferencia entre el número de secuencia del último mensaje en cola en la partición del origen del evento y el del mensaje que se está procesando en la entrada|Sin dimensiones|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|IngressReceivedMessages|Mensajes de entrada recibidos|Número|Total|Número de mensajes leídos desde el origen del evento|Sin dimensiones|
|IngressReceivedInvalidMessages|Mensajes no válidos de entrada recibidos|Número|Total|Número de mensajes leídos no válidos desde el origen del evento|Sin dimensiones|
|IngressReceivedBytes|Bytes de entrada recibidos|Bytes|Total|Número de bytes leídos desde el origen del evento|Sin dimensiones|
|IngressStoredBytes|Bytes de entrada almacenados|Bytes|Total|Tamaño total de los eventos almacenados correctamente y disponibles para su consulta|Sin dimensiones|
|IngressStoredEvents|Eventos de entrada almacenados|Número|Total|Número de eventos acoplados que se procesaron correctamente y que están disponibles para su consulta|Sin dimensiones|
|IngressReceivedMessagesTimeLag|Tiempo de retardo de los mensajes de entrada recibidos|Segundos|Máxima|Diferencia entre el momento en el que el mensaje se pone en la cola en el origen del evento y el momento en que se procesa en la entrada|Sin dimensiones|
|IngressReceivedMessagesCountLag|Intervalo de recuento de los mensajes de entrada recibidos|Número|Media|Diferencia entre el número de secuencia del último mensaje en cola en la partición del origen del evento y el del mensaje que se está procesando en la entrada|Sin dimensiones|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CpuPercentage|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Instance|
|MemoryPercentage|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Instance|
|DiskQueueLength|Longitud de la cola de disco|Número|Media|Longitud de la cola de disco|Instance|
|HttpQueueLength|Longitud de la cola HTTP|Número|Media|Longitud de la cola HTTP|Instance|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (se excluye Functions)

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CpuTime|Tiempo de CPU|Segundos|Total|Tiempo de CPU|Instance|
|Requests|Requests|Número|Total|Requests|Instance|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|
|Http101|Http 101|Número|Total|Http 101|Instance|
|Http2xx|Http 2xx|Número|Total|Http 2xx|Instance|
|Http3xx|Http 3xx|Número|Total|Http 3xx|Instance|
|Http401|Http 401|Número|Total|Http 401|Instance|
|Http403|Http 403|Número|Total|Http 403|Instance|
|Http404|Http 404|Número|Total|Http 404|Instance|
|Http406|Http 406|Número|Total|Http 406|Instance|
|Http4xx|Http 4xx|Número|Total|Http 4xx|Instance|
|Http5xx|Errores de servidor HTTP|Número|Total|Errores de servidor HTTP|Instance|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Media|Espacio de trabajo de memoria|Instance|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Media|Espacio de trabajo de memoria promedio|Instance|
|AverageResponseTime|Tiempo de respuesta promedio|Segundos|Media|Tiempo de respuesta promedio|Instance|
|AppConnections|Conexiones|Número|Media|Conexiones|Instance|
|Identificadores|Recuento de identificadores|Número|Media|Recuento de identificadores|Instance|
|Subprocesos|Número de subprocesos|Número|Media|Número de subprocesos|Instance|
|PrivateBytes|Bytes privados|Bytes|Media|Bytes privados|Instance|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instance|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instance|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instance|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instance|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instance|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instance|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Número|Media|Solicitudes en cola de la aplicación|Instance|
|CurrentAssemblies|Ensamblados actuales|Número|Media|Ensamblados actuales|Instance|
|TotalAppDomains|Dominios de aplicación totales|Número|Media|Dominios de aplicación totales|Instance|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Número|Media|Dominios de aplicación totales descargados|Instance|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Número|Total|Recolección de elementos no utilizados de Gen 0|Instance|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Número|Total|Recolección de elementos no utilizados de Gen 1|Instance|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Número|Total|Recolección de elementos no utilizados de Gen 2|Instance|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (Functions)

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|
|Http5xx|Errores de servidor HTTP|Número|Total|Errores de servidor HTTP|Instance|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Media|Espacio de trabajo de memoria|Instance|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Media|Espacio de trabajo de memoria promedio|Instance|
|FunctionExecutionUnits|Unidades de ejecución de función|Número|Total|Unidades de ejecución de función|Instance|
|FunctionExecutionCount|Recuento de ejecución de funciones|Número|Total|Recuento de ejecución de funciones|Instance|
|PrivateBytes|Bytes privados|Bytes|Media|Bytes privados|Instance|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instance|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instance|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instance|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instance|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instance|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instance|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Número|Media|Solicitudes en cola de la aplicación|Instance|
|CurrentAssemblies|Ensamblados actuales|Número|Media|Ensamblados actuales|Instance|
|TotalAppDomains|Dominios de aplicación totales|Número|Media|Dominios de aplicación totales|Instance|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Número|Media|Dominios de aplicación totales descargados|Instance|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Número|Total|Recolección de elementos no utilizados de Gen 0|Instance|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Número|Total|Recolección de elementos no utilizados de Gen 1|Instance|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Número|Total|Recolección de elementos no utilizados de Gen 2|Instance|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CpuTime|Tiempo de CPU|Segundos|Total|Tiempo de CPU|Instance|
|Requests|Requests|Número|Total|Requests|Instance|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|
|Http101|Http 101|Número|Total|Http 101|Instance|
|Http2xx|Http 2xx|Número|Total|Http 2xx|Instance|
|Http3xx|Http 3xx|Número|Total|Http 3xx|Instance|
|Http401|Http 401|Número|Total|Http 401|Instance|
|Http403|Http 403|Número|Total|Http 403|Instance|
|Http404|Http 404|Número|Total|Http 404|Instance|
|Http406|Http 406|Número|Total|Http 406|Instance|
|Http4xx|Http 4xx|Número|Total|Http 4xx|Instance|
|Http5xx|Errores de servidor HTTP|Número|Total|Errores de servidor HTTP|Instance|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Media|Espacio de trabajo de memoria|Instance|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Media|Espacio de trabajo de memoria promedio|Instance|
|AverageResponseTime|Tiempo de respuesta promedio|Segundos|Media|Tiempo de respuesta promedio|Instance|
|FunctionExecutionUnits|Unidades de ejecución de función|Número|Total|Unidades de ejecución de función|Instance|
|FunctionExecutionCount|Recuento de ejecución de funciones|Número|Total|Recuento de ejecución de funciones|Instance|
|AppConnections|Conexiones|Número|Media|Conexiones|Instance|
|Identificadores|Recuento de identificadores|Número|Media|Recuento de identificadores|Instance|
|Subprocesos|Número de subprocesos|Número|Media|Número de subprocesos|Instance|
|PrivateBytes|Bytes privados|Bytes|Media|Bytes privados|Instance|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instance|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instance|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instance|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instance|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instance|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instance|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Número|Media|Solicitudes en cola de la aplicación|Instance|
|CurrentAssemblies|Ensamblados actuales|Número|Media|Ensamblados actuales|Instance|
|TotalAppDomains|Dominios de aplicación totales|Número|Media|Dominios de aplicación totales|Instance|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Número|Media|Dominios de aplicación totales descargados|Instance|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Número|Total|Recolección de elementos no utilizados de Gen 0|Instance|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Número|Total|Recolección de elementos no utilizados de Gen 1|Instance|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Número|Total|Recolección de elementos no utilizados de Gen 2|Instance|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Requests|Requests|Número|Total|Requests|Instance|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|
|Http101|Http 101|Número|Total|Http 101|Instance|
|Http2xx|Http 2xx|Número|Total|Http 2xx|Instance|
|Http3xx|Http 3xx|Número|Total|Http 3xx|Instance|
|Http401|Http 401|Número|Total|Http 401|Instance|
|Http403|Http 403|Número|Total|Http 403|Instance|
|Http404|Http 404|Número|Total|Http 404|Instance|
|Http406|Http 406|Número|Total|Http 406|Instance|
|Http4xx|Http 4xx|Número|Total|Http 4xx|Instance|
|Http5xx|Errores de servidor HTTP|Número|Total|Errores de servidor HTTP|Instance|
|AverageResponseTime|Tiempo de respuesta promedio|Segundos|Media|Tiempo de respuesta promedio|Instance|
|CpuPercentage|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Instance|
|MemoryPercentage|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Instance|
|DiskQueueLength|Longitud de la cola de disco|Número|Media|Longitud de la cola de disco|Instance|
|HttpQueueLength|Longitud de la cola HTTP|Número|Media|Longitud de la cola HTTP|Instance|
|ActiveRequests|Solicitudes activas|Número|Total|Solicitudes activas|Instance|
|TotalFrontEnds|Número total de front-ends|Número|Media|Número total de front-ends|Sin dimensiones|
|SmallAppServicePlanInstances|Trabajos pequeños del plan de App Service|Número|Media|Trabajos pequeños del plan de App Service|Sin dimensiones|
|MediumAppServicePlanInstances|Trabajos medianos del plan de App Service|Número|Media|Trabajos medianos del plan de App Service|Sin dimensiones|
|LargeAppServicePlanInstances|Trabajos grandes del plan de App Service|Número|Media|Trabajos grandes del plan de App Service|Sin dimensiones|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|WorkersTotal|Número total de trabajos|Número|Media|Número total de trabajos|Sin dimensiones|
|WorkersAvailable|Trabajos disponibles|Número|Media|Trabajos disponibles|Sin dimensiones|
|WorkersUsed|Trabajos usados|Número|Media|Trabajos usados|Sin dimensiones|
|CpuPercentage|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Instance|
|MemoryPercentage|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Instance|

## <a name="next-steps"></a>Pasos siguientes
* [Lea información sobre las métricas en Azure Monitor](../../azure-monitor/platform/data-collection.md)
* [Creación de alertas basadas en métricas](../../azure-monitor/platform/alerts-overview.md)
* [Exportación de métricas a cuentas de almacenamiento, Event Hubs o Log Analytics](../../azure-monitor/platform/diagnostic-logs-overview.md)

