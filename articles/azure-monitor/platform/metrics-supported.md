---
title: Métricas compatibles de Azure Monitor por tipo de recurso
description: Lista de métricas disponibles para cada tipo de recurso con Azure Monitor.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/20/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 49780ec342ea168d27ab8a029c41a1c18a6ffcc4
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69019045"
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
|qpu_metric|QPU|Count|Media|QPU. Intervalo de 0-100 para S1, 0-200 para S2 y 0-400 para S4|ServerResourceType|
|memory_metric|Memoria|Bytes|Media|Memoria. Intervalo de 0-25 GB para S1, 0-50 GB para S2 y 0-100 GB para S4|ServerResourceType|
|TotalConnectionRequests|Número total de solicitudes de conexión|Count|Media|Número total de solicitudes de conexión. Se trata de llegadas.|ServerResourceType|
|SuccessfullConnectionsPerSec|Conexiones correctas por segundo|CountPerSecond|Media|Tasa de finalizaciones de conexión correctas.|ServerResourceType|
|TotalConnectionFailures|Número total de errores de conexión|Count|Media|Número total de intentos de conexión con error.|ServerResourceType|
|CurrentUserSessions|Sesiones de usuario actuales|Count|Media|Número actual de sesiones de usuario establecidas.|ServerResourceType|
|QueryPoolBusyThreads|Subprocesos ocupados de grupo de consultas|Count|Media|Número de subprocesos ocupados del grupo de subprocesos de consulta.|ServerResourceType|
|CommandPoolJobQueueLength|Longitud de cola de trabajos de grupo de comandos|Count|Media|Número de trabajos en la cola del grupo de subprocesos de comandos.|ServerResourceType|
|ProcessingPoolJobQueueLength|Longitud de cola de trabajos de grupo de procesamiento|Count|Media|Número de trabajos que no son de E/S en la cola del grupo de subprocesos de procesamiento.|ServerResourceType|
|CurrentConnections|Conexión: Conexiones actuales|Count|Media|Número actual de conexiones de cliente establecidas.|ServerResourceType|
|CleanerCurrentPrice|Memoria: precio actual de limpieza|Count|Media|Precio actual de memoria, USD/byte/tiempo, normalizado a 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Memoria: memoria de limpieza reducible|Bytes|Media|Cantidad de memoria, en bytes, que purgará el limpiador en segundo plano.|ServerResourceType|
|CleanerMemoryNonshrinkable|Memoria: memoria de limpieza no reducible|Bytes|Media|Cantidad de memoria, en bytes, que no purgará el limpiador en segundo plano.|ServerResourceType|
|MemoryUsage|Memoria: Uso de la memoria|Bytes|Media|Uso de memoria del proceso de servidor tal como se usa para calcular el precio de la memoria del limpiador. Es igual al contador Proceso\Bytes privados más el tamaño de los datos asignados a la memoria; se ignora la memoria que ha asignado VertiPaq superior al límite de memoria de VertiPaq.|ServerResourceType|
|MemoryLimitHard|Memoria: límite de memoria física|Bytes|Media|Límite de memoria física del archivo de configuración.|ServerResourceType|
|MemoryLimitHigh|Memoria: límite alto de memoria|Bytes|Media|Límite alto de memoria del archivo de configuración.|ServerResourceType|
|MemoryLimitLow|Memoria: límite bajo de memoria|Bytes|Media|Límite bajo de memoria del archivo de configuración.|ServerResourceType|
|MemoryLimitVertiPaq|Memoria: VertiPaq de límite de memoria|Bytes|Media|Límite en memoria del archivo de configuración.|ServerResourceType|
|Quota|Memoria: Quota|Bytes|Media|Cuota de memoria actual, en bytes. La cuota de memoria también se denomina concesión de memoria o reserva de memoria.|ServerResourceType|
|QuotaBlocked|Memoria: cuota bloqueada|Count|Media|Número actual de solicitudes de cuota que están bloqueadas hasta que se liberen otras cuotas de memoria.|ServerResourceType|
|VertiPaqNonpaged|Memoria: VertiPaq no paginado|Bytes|Media|Bytes de memoria bloqueados en el espacio de trabajo para que los use el motor en memoria.|ServerResourceType|
|VertiPaqPaged|Memoria: VertiPaq paginado|Bytes|Media|Bytes de memoria paginada en uso para datos en memoria.|ServerResourceType|
|RowsReadPerSec|Procesamiento: filas leídas por segundo|CountPerSecond|Media|Velocidad de filas leídas de todas las bases de datos relacionales.|ServerResourceType|
|RowsConvertedPerSec|Procesamiento: filas convertidas por segundo|CountPerSecond|Media|Velocidad de filas convertidas durante el procesamiento.|ServerResourceType|
|RowsWrittenPerSec|Procesamiento: filas escritas por segundo|CountPerSecond|Media|Velocidad de filas escritas durante el procesamiento.|ServerResourceType|
|CommandPoolBusyThreads|Subprocesos: subprocesos ocupados del grupo de comandos|Count|Media|Número de subprocesos ocupados del grupo de subprocesos de comandos.|ServerResourceType|
|CommandPoolIdleThreads|Subprocesos: subprocesos inactivos del grupo de comandos|Count|Media|Número de subprocesos inactivos del grupo de subprocesos de comandos.|ServerResourceType|
|LongParsingBusyThreads|Subprocesos: subprocesos ocupados en análisis largos|Count|Media|Número de subprocesos ocupados del grupo de subprocesos de análisis largos.|ServerResourceType|
|LongParsingIdleThreads|Subprocesos: subprocesos inactivos en análisis largos|Count|Media|Número de subprocesos inactivos del grupo de subprocesos en análisis largos.|ServerResourceType|
|LongParsingJobQueueLength|Subprocesos: longitud de cola de trabajos en análisis largos|Count|Media|Número de trabajos en la cola del grupo de subprocesos en análisis largos.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Subprocesos: subprocesos de trabajo de E/S ocupados del grupo de procesamiento|Count|Media|Número de subprocesos que ejecutan trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Subprocesos: subprocesos de trabajo ocupados que no son de E/S del grupo de procesamiento|Count|Media|Número de subprocesos que ejecutan trabajos que no son de E/S del grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Subprocesos: Longitud de cola de trabajos de E/S de grupo de procesamiento|Count|Media|Número de trabajos de E/S en la cola del grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Subprocesos: subprocesos de trabajo de E/S inactivos del grupo de procesamiento|Count|Media|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Subprocesos: subprocesos de trabajo inactivos que no son de E/S del grupo de procesamiento|Count|Media|Número de subprocesos inactivos del grupo de subprocesos de procesamiento dedicado a trabajos que no son de E/S.|ServerResourceType|
|QueryPoolIdleThreads|Subprocesos: subprocesos inactivos del grupo de consultas|Count|Media|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|QueryPoolJobQueueLength|Subprocesos: longitud de cola de trabajos del grupo de consultas|Count|Media|Número de trabajos en la cola del grupo de subprocesos de consultas.|ServerResourceType|
|ShortParsingBusyThreads|Subprocesos: subprocesos ocupados en análisis cortos|Count|Media|Número de subprocesos ocupados del grupo de subprocesos de análisis cortos.|ServerResourceType|
|ShortParsingIdleThreads|Subprocesos: subprocesos inactivos en análisis cortos|Count|Media|Número de subprocesos inactivos del grupo de subprocesos de análisis cortos.|ServerResourceType|
|ShortParsingJobQueueLength|Subprocesos: longitud de cola de trabajos en análisis cortos|Count|Media|Número de trabajos en la cola del grupo de subprocesos en análisis cortos.|ServerResourceType|
|memory_thrashing_metric|Paginación excesiva de memoria|Percent|Media|Paginación excesiva media de memoria.|ServerResourceType|
|mashup_engine_qpu_metric|QPU de motor M|Count|Media|Uso de QPU por los procesos del motor de mashup|ServerResourceType|
|mashup_engine_memory_metric|Memoria del motor M|Bytes|Media|Uso de memoria por los procesos del motor de mashup|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Requests|Requests|Count|Total|Número total de solicitudes de puerta de enlace durante un período determinado. Se puede segmentar por varias dimensiones para ayudar a diagnosticar problemas. |Location, BackendResponseCode, LastErrorReason, GatewayResponseCode|
|TotalRequests|Solicitudes de puerta de enlace en total|Count|Total|Número total de solicitudes de puerta de enlace durante un período determinado. Esta métrica está en desuso; se recomienda usar la nueva métrica `Requests`. |Ubicación, Nombre de host|
|SuccessfulRequests|Solicitudes de puerta de enlace correctas|Count|Total|Número total de solicitudes de puerta de enlace correctas durante un período determinado. Esta métrica está en desuso; se recomienda usar la nueva métrica `Requests`.|Ubicación, Nombre de host|
|UnauthorizedRequests|Solicitudes de puerta de enlace no autorizadas|Count|Total| Número total de solicitudes de puerta de enlace no autorizadas durante un período determinado. Esta métrica está en desuso; se recomienda usar la nueva métrica `Requests`.|Ubicación, Nombre de host|
|FailedRequests|Solicitudes de puerta de enlace con error|Count|Total|Número total de solicitudes de puerta de enlace con errores durante un período determinado. Esta métrica está en desuso; se recomienda usar la nueva métrica `Requests`.|Ubicación, Nombre de host|
|OtherRequests|Otras solicitudes de puerta de enlace|Count|Total|Número total de solicitudes de puerta de enlace durante un período determinado que no se pueden clasificar como correctas, no autorizadas o con errores. Esta métrica está en desuso; se recomienda usar la nueva métrica `Requests`. |Ubicación, Nombre de host|
|Duration|Duración total de las solicitudes de puerta de enlace|Milisegundos|Media|Tiempo transcurrido entre el momento en que API Management recibe una solicitud de un cliente y cuando devuelve una respuesta al cliente.|Ubicación, Nombre de host|
|Capacity|Capacity|Percent|Media|Indicador de carga de una instancia de API Management para tomar decisiones fundamentadas sobre si dicha instancia se debe escalar para acomodar más carga.|Location|
|EventHubTotalEvents|Eventos totales de EventHub|Count|Total|Número total de eventos enviados a EventHub desde API Management durante un período determinado.|Location|
|EventHubSuccessfulEvents|Eventos EventHub correctos|Count|Total|Número total de eventos EventHub correctos durante un período determinado.|Location|
|EventHubTotalFailedEvents|Eventos EventHub con errores|Count|Total|Número total de eventos EventHub con errores durante un período determinado.|Location|
|EventHubRejectedEvents|Eventos EventHub rechazados|Count|Total|Número total de eventos EventHub rechazados (configuración incorrecta o no autorizados) durante un período determinado.|Location|
|EventHubThrottledEvents|Eventos EventHub limitados|Count|Total|Número total de eventos EventHub limitados durante un período determinado.|Location|
|EventHubTimedoutEvents|Eventos EventHub con tiempo de espera agotado|Count|Total|Número total de eventos EventHub que han agotado el tiempo de espera durante un período determinado.|Location|
|EventHubDroppedEvents|Eventos EventHub quitados|Count|Total|Número total de eventos omitidos por haber alcanzado el límite del tamaño de la cola durante un período determinado.|Location|
|EventHubTotalBytesSent|Tamaño de los eventos EventHub|Bytes|Total|Tamaño total de los eventos EventHub en bytes durante un período determinado.|Location|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TotalJob|Número total de trabajos|Count|Total|El número total de trabajos (jobs)|Runbook, Estado|
|TotalUpdateDeploymentRuns|Ejecuciones de implementación de actualizaciones totales|Count|Total|Ejecuciones de implementación de actualizaciones de software totales|SoftwareUpdateConfigurationName, Status|
|TotalUpdateDeploymentMachineRuns|Ejecuciones de máquina de implementación de actualizaciones totales|Count|Total|Ejecuciones de máquina de implementación de actualizaciones de software totales en una ejecución de implementación de actualizaciones de software|SoftwareUpdateConfigurationName, Status, TargetComputer, SoftwareUpdateConfigurationRunId|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CoreCount|Recuento de núcleos dedicados|Count|Total|Número total de núcleos dedicados de la cuenta de Batch|Sin dimensiones|
|TotalNodeCount|Recuento de nodos dedicados|Count|Total|Número total de nodos dedicados de la cuenta de Batch|Sin dimensiones|
|LowPriorityCoreCount|Recuento de núcleos de baja prioridad|Count|Total|Número total de núcleos de baja prioridad de la cuenta de Batch|Sin dimensiones|
|TotalLowPriorityNodeCount|Recuento de nodos de baja prioridad|Count|Total|Número total de nodos de baja prioridad de la cuenta de Batch|Sin dimensiones|
|CreatingNodeCount|Recuento de nodos creados|Count|Total|Número de nodos que se van a crear|Sin dimensiones|
|StartingNodeCount|Recuento de nodos de inicio|Count|Total|Número de nodos que se van a iniciar|Sin dimensiones|
|WaitingForStartTaskNodeCount|Recuento de nodos esperando a la tarea de inicio|Count|Total|Número de nodos que esperan a que se complete la tarea de inicio|Sin dimensiones|
|StartTaskFailedNodeCount|Recuento de nodos con error de la tarea de inicio|Count|Total|Número de nodos con error en la tarea de inicio|Sin dimensiones|
|IdleNodeCount|Recuento de nodos inactivos|Count|Total|Número de nodos inactivos|Sin dimensiones|
|OfflineNodeCount|Recuento de nodos sin conexión|Count|Total|Número de nodos sin conexión|Sin dimensiones|
|RebootingNodeCount|Recuento de nodos de reinicio|Count|Total|Número de nodos de reinicio|Sin dimensiones|
|ReimagingNodeCount|Recuento de nodos de restablecimiento de imagen inicial|Count|Total|Número de nodos de restablecimiento de imagen inicial|Sin dimensiones|
|RunningNodeCount|Recuento de nodos en ejecución|Count|Total|Número de nodos en ejecución|Sin dimensiones|
|LeavingPoolNodeCount|Recuento de nodos que salen del grupo|Count|Total|Número de nodos que abandonan el grupo|Sin dimensiones|
|UnusableNodeCount|Recuento de nodos no utilizables|Count|Total|Número de nodos inutilizables|Sin dimensiones|
|PreemptedNodeCount|Recuento de nodos con prioridad|Count|Total|Número de nodos con prioridad|Sin dimensiones|
|TaskStartEvent|Eventos de inicio de tarea|Count|Total|Número total de tareas que se han iniciado|Sin dimensiones|
|TaskCompleteEvent|Eventos de tarea completada|Count|Total|Número total de tareas que se han completado|Sin dimensiones|
|TaskFailEvent|Eventos de error en tareas|Count|Total|Número total de tareas que se han completado con errores|Sin dimensiones|
|PoolCreateEvent|Eventos de creación de grupo|Count|Total|Número total de grupos que se han creado|Sin dimensiones|
|PoolResizeStartEvent|Eventos de inicio de cambio de tamaño de grupo|Count|Total|Número total de cambios de tamaño de grupo que se han iniciado|Sin dimensiones|
|PoolResizeCompleteEvent|Eventos de finalización de cambio de tamaño de grupo|Count|Total|Número total de cambios de tamaño de grupo completados|Sin dimensiones|
|PoolDeleteStartEvent|Eventos de inicio de eliminación de grupo|Count|Total|Número total de eliminaciones de grupo que se han iniciado|Sin dimensiones|
|PoolDeleteCompleteEvent|Eventos de finalización de eliminaciones de grupo|Count|Total|Número total de eliminaciones de grupo que se han completado|Sin dimensiones|
|JobDeleteCompleteEvent|Eventos de finalización de eliminaciones de trabajo|Count|Total|Número total de trabajos que se han eliminado correctamente.|Sin dimensiones|
|JobDeleteStartEvent|Eventos de inicio de eliminaciones de trabajo|Count|Total|Número total de trabajos cuya eliminación se ha solicitado.|Sin dimensiones|
|JobDisableCompleteEvent|Eventos de finalización de deshabilitación de trabajo|Count|Total|Número total de trabajos que se han deshabilitado correctamente.|Sin dimensiones|
|JobDisableStartEvent|Eventos de inicio de deshabilitación de trabajo|Count|Total|Número total de trabajos cuya deshabilitación se ha solicitado.|Sin dimensiones|
|JobStartEvent|Eventos de inicio de trabajo|Count|Total|Número total de trabajos que se han iniciado correctamente.|Sin dimensiones|
|JobTerminateCompleteEvent|Eventos de finalización de terminaciones de trabajo|Count|Total|Número total de trabajos que se han finalizado correctamente.|Sin dimensiones|
|JobTerminateStartEvent|Eventos de inicio de terminaciones de trabajo|Count|Total|Número total de trabajos cuya terminación se ha solicitado.|Sin dimensiones|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|connectedclients|Clientes conectados|Count|Máxima||ShardId|
|totalcommandsprocessed|Total de operaciones|Count|Total||ShardId|
|cachehits|Aciertos de caché|Count|Total||ShardId|
|cachemisses|Errores de caché|Count|Total||ShardId|
|getcommands|Gets|Count|Total||ShardId|
|setcommands|Sets|Count|Total||ShardId|
|operationsPerSecond|Operaciones por segundo|Count|Máxima||ShardId|
|evictedkeys|Claves expulsadas|Count|Total||ShardId|
|totalkeys|Total de claves|Count|Máxima||ShardId|
|expiredkeys|Claves expiradas|Count|Total||ShardId|
|usedmemory|Memoria usada|Bytes|Máxima||ShardId|
|usedmemorypercentage|Porcentaje de memoria usado|Percent|Máxima||ShardId|
|usedmemoryRss|Memoria RSS usada|Bytes|Máxima||ShardId|
|serverLoad|Carga de servidor|Percent|Máxima||ShardId|
|cacheWrite|Escritura de caché|BytesPerSecond|Máxima||ShardId|
|cacheRead|Lectura de caché|BytesPerSecond|Máxima||ShardId|
|percentProcessorTime|CPU|Percent|Máxima||ShardId|
|cacheLatency|Microsegundos de latencia de la memoria caché (versión preliminar)|Count|Media||ShardId, SampleType|
|errors|Errors|Count|Máxima||ShardId, ErrorType|
|connectedclients0|Clientes conectados (partición 0)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed0|Total de operaciones (partición 0)|Count|Total||Sin dimensiones|
|cachehits0|Aciertos de caché (partición 0)|Count|Total||Sin dimensiones|
|cachemisses0|Errores de caché (partición 0)|Count|Total||Sin dimensiones|
|getcommands0|Obtenciones (partición 0)|Count|Total||Sin dimensiones|
|setcommands0|Definiciones (partición 0)|Count|Total||Sin dimensiones|
|operationsPerSecond0|Operaciones por segundo (partición 0)|Count|Máxima||Sin dimensiones|
|evictedkeys0|Claves expulsadas (partición 0)|Count|Total||Sin dimensiones|
|totalkeys0|Total de claves (partición 0)|Count|Máxima||Sin dimensiones|
|expiredkeys0|Claves expiradas (partición 0)|Count|Total||Sin dimensiones|
|usedmemory0|Memoria usada (partición 0)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss0|Memoria RSS usada (partición 0)|Bytes|Máxima||Sin dimensiones|
|serverLoad0|Carga de servidor (partición 0)|Percent|Máxima||Sin dimensiones|
|cacheWrite0|Escritura de caché (partición 0)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead0|Lectura de caché (partición 0)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime0|CPU (partición 0)|Percent|Máxima||Sin dimensiones|
|connectedclients1|Clientes conectados (partición 1)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed1|Total de operaciones (partición 1)|Count|Total||Sin dimensiones|
|cachehits1|Aciertos de caché (partición 1)|Count|Total||Sin dimensiones|
|cachemisses1|Errores de caché (partición 1)|Count|Total||Sin dimensiones|
|getcommands1|Obtenciones (partición 1)|Count|Total||Sin dimensiones|
|setcommands1|Definiciones (partición 1)|Count|Total||Sin dimensiones|
|operationsPerSecond1|Operaciones por segundo (partición 1)|Count|Máxima||Sin dimensiones|
|evictedkeys1|Claves expulsadas (partición 1)|Count|Total||Sin dimensiones|
|totalkeys1|Total de claves (partición 1)|Count|Máxima||Sin dimensiones|
|expiredkeys1|Claves expiradas (partición 1)|Count|Total||Sin dimensiones|
|usedmemory1|Memoria usada (partición 1)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss1|Memoria RSS usada (partición 1)|Bytes|Máxima||Sin dimensiones|
|serverLoad1|Carga de servidor (partición 1)|Percent|Máxima||Sin dimensiones|
|cacheWrite1|Escritura de caché (partición 1)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead1|Lectura de caché (partición 1)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime1|CPU (partición 1)|Percent|Máxima||Sin dimensiones|
|connectedclients2|Clientes conectados (partición 2)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed2|Total de operaciones (partición 2)|Count|Total||Sin dimensiones|
|cachehits2|Aciertos de caché (partición 2)|Count|Total||Sin dimensiones|
|cachemisses2|Errores de caché (partición 2)|Count|Total||Sin dimensiones|
|getcommands2|Obtenciones (partición 2)|Count|Total||Sin dimensiones|
|setcommands2|Definiciones (partición 2)|Count|Total||Sin dimensiones|
|operationsPerSecond2|Operaciones por segundo (partición 2)|Count|Máxima||Sin dimensiones|
|evictedkeys2|Claves expulsadas (partición 2)|Count|Total||Sin dimensiones|
|totalkeys2|Total de claves (partición 2)|Count|Máxima||Sin dimensiones|
|expiredkeys2|Claves expiradas (partición 2)|Count|Total||Sin dimensiones|
|usedmemory2|Memoria usada (partición 2)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss2|Memoria RSS usada (partición 2)|Bytes|Máxima||Sin dimensiones|
|serverLoad2|Carga de servidor (partición 2)|Percent|Máxima||Sin dimensiones|
|cacheWrite2|Escritura de caché (partición 2)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead2|Lectura de caché (partición 2)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime2|CPU (partición 2)|Percent|Máxima||Sin dimensiones|
|connectedclients3|Clientes conectados (partición 3)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed3|Total de operaciones (partición 3)|Count|Total||Sin dimensiones|
|cachehits3|Aciertos de caché (partición 3)|Count|Total||Sin dimensiones|
|cachemisses3|Errores de caché (partición 3)|Count|Total||Sin dimensiones|
|getcommands3|Obtenciones (partición 3)|Count|Total||Sin dimensiones|
|setcommands3|Definiciones (partición 3)|Count|Total||Sin dimensiones|
|operationsPerSecond3|Operaciones por segundo (partición 3)|Count|Máxima||Sin dimensiones|
|evictedkeys3|Claves expulsadas (partición 3)|Count|Total||Sin dimensiones|
|totalkeys3|Total de claves (partición 3)|Count|Máxima||Sin dimensiones|
|expiredkeys3|Claves expiradas (partición 3)|Count|Total||Sin dimensiones|
|usedmemory3|Memoria usada (partición 3)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss3|Memoria RSS usada (partición 3)|Bytes|Máxima||Sin dimensiones|
|serverLoad3|Carga de servidor (partición 3)|Percent|Máxima||Sin dimensiones|
|cacheWrite3|Escritura de caché (partición 3)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead3|Lectura de caché (partición 3)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime3|CPU (partición 3)|Percent|Máxima||Sin dimensiones|
|connectedclients4|Clientes conectados (partición 4)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed4|Total de operaciones (partición 4)|Count|Total||Sin dimensiones|
|cachehits4|Aciertos de caché (partición 4)|Count|Total||Sin dimensiones|
|cachemisses4|Errores de caché (partición 4)|Count|Total||Sin dimensiones|
|getcommands4|Obtenciones (partición 4)|Count|Total||Sin dimensiones|
|setcommands4|Definiciones (partición 4)|Count|Total||Sin dimensiones|
|operationsPerSecond4|Operaciones por segundo (partición 4)|Count|Máxima||Sin dimensiones|
|evictedkeys4|Claves expulsadas (partición 4)|Count|Total||Sin dimensiones|
|totalkeys4|Total de claves (partición 4)|Count|Máxima||Sin dimensiones|
|expiredkeys4|Claves expiradas (partición 4)|Count|Total||Sin dimensiones|
|usedmemory4|Memoria usada (partición 4)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss4|Memoria RSS usada (partición 4)|Bytes|Máxima||Sin dimensiones|
|serverLoad4|Carga de servidor (partición 4)|Percent|Máxima||Sin dimensiones|
|cacheWrite4|Escritura de caché (partición 4)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead4|Lectura de caché (partición 4)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime4|CPU (partición 4)|Percent|Máxima||Sin dimensiones|
|connectedclients5|Clientes conectados (partición 5)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed5|Total de operaciones (partición 5)|Count|Total||Sin dimensiones|
|cachehits5|Aciertos de caché (partición 5)|Count|Total||Sin dimensiones|
|cachemisses5|Errores de caché (partición 5)|Count|Total||Sin dimensiones|
|getcommands5|Obtenciones (partición 5)|Count|Total||Sin dimensiones|
|setcommands5|Definiciones (partición 5)|Count|Total||Sin dimensiones|
|operationsPerSecond5|Operaciones por segundo (partición 5)|Count|Máxima||Sin dimensiones|
|evictedkeys5|Claves expulsadas (partición 5)|Count|Total||Sin dimensiones|
|totalkeys5|Total de claves (partición 5)|Count|Máxima||Sin dimensiones|
|expiredkeys5|Claves expiradas (partición 5)|Count|Total||Sin dimensiones|
|usedmemory5|Memoria usada (partición 5)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss5|Memoria RSS usada (partición 5)|Bytes|Máxima||Sin dimensiones|
|serverLoad5|Carga de servidor (partición 5)|Percent|Máxima||Sin dimensiones|
|cacheWrite5|Escritura de caché (partición 5)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead5|Lectura de caché (partición 5)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime5|CPU (partición 5)|Percent|Máxima||Sin dimensiones|
|connectedclients6|Clientes conectados (partición 6)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed6|Total de operaciones (partición 6)|Count|Total||Sin dimensiones|
|cachehits6|Aciertos de caché (partición 6)|Count|Total||Sin dimensiones|
|cachemisses6|Errores de caché (partición 6)|Count|Total||Sin dimensiones|
|getcommands6|Obtenciones (partición 6)|Count|Total||Sin dimensiones|
|setcommands6|Definiciones (partición 6)|Count|Total||Sin dimensiones|
|operationsPerSecond6|Operaciones por segundo (partición 6)|Count|Máxima||Sin dimensiones|
|evictedkeys6|Claves expulsadas (partición 6)|Count|Total||Sin dimensiones|
|totalkeys6|Total de claves (partición 6)|Count|Máxima||Sin dimensiones|
|expiredkeys6|Claves expiradas (partición 6)|Count|Total||Sin dimensiones|
|usedmemory6|Memoria usada (partición 6)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss6|Memoria RSS usada (partición 6)|Bytes|Máxima||Sin dimensiones|
|serverLoad6|Carga de servidor (partición 6)|Percent|Máxima||Sin dimensiones|
|cacheWrite6|Escritura de caché (partición 6)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead6|Lectura de caché (partición 6)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime6|CPU (partición 6)|Percent|Máxima||Sin dimensiones|
|connectedclients7|Clientes conectados (partición 7)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed7|Total de operaciones (partición 7)|Count|Total||Sin dimensiones|
|cachehits7|Aciertos de caché (partición 7)|Count|Total||Sin dimensiones|
|cachemisses7|Errores de caché (partición 7)|Count|Total||Sin dimensiones|
|getcommands7|Obtenciones (partición 7)|Count|Total||Sin dimensiones|
|setcommands7|Definiciones (partición 7)|Count|Total||Sin dimensiones|
|operationsPerSecond7|Operaciones por segundo (partición 7)|Count|Máxima||Sin dimensiones|
|evictedkeys7|Claves expulsadas (partición 7)|Count|Total||Sin dimensiones|
|totalkeys7|Total de claves (partición 7)|Count|Máxima||Sin dimensiones|
|expiredkeys7|Claves expiradas (partición 7)|Count|Total||Sin dimensiones|
|usedmemory7|Memoria usada (partición 7)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss7|Memoria RSS usada (partición 7)|Bytes|Máxima||Sin dimensiones|
|serverLoad7|Carga de servidor (partición 7)|Percent|Máxima||Sin dimensiones|
|cacheWrite7|Escritura de caché (partición 7)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead7|Lectura de caché (partición 7)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime7|CPU (partición 7)|Percent|Máxima||Sin dimensiones|
|connectedclients8|Clientes conectados (partición 8)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed8|Total de operaciones (partición 8)|Count|Total||Sin dimensiones|
|cachehits8|Aciertos de caché (partición 8)|Count|Total||Sin dimensiones|
|cachemisses8|Errores de caché (partición 8)|Count|Total||Sin dimensiones|
|getcommands8|Obtenciones (partición 8)|Count|Total||Sin dimensiones|
|setcommands8|Definiciones (partición 8)|Count|Total||Sin dimensiones|
|operationsPerSecond8|Operaciones por segundo (partición 8)|Count|Máxima||Sin dimensiones|
|evictedkeys8|Claves expulsadas (partición 8)|Count|Total||Sin dimensiones|
|totalkeys8|Total de claves (partición 8)|Count|Máxima||Sin dimensiones|
|expiredkeys8|Claves expiradas (partición 8)|Count|Total||Sin dimensiones|
|usedmemory8|Memoria usada (partición 8)|Bytes|Máxima||Sin dimensiones|
|usedmemoryRss8|Memoria RSS usada (partición 8)|Bytes|Máxima||Sin dimensiones|
|serverLoad8|Carga de servidor (partición 8)|Percent|Máxima||Sin dimensiones|
|cacheWrite8|Escritura de caché (partición 8)|BytesPerSecond|Máxima||Sin dimensiones|
|cacheRead8|Lectura de caché (partición 8)|BytesPerSecond|Máxima||Sin dimensiones|
|percentProcessorTime8|CPU (partición 8)|Percent|Máxima||Sin dimensiones|
|connectedclients9|Clientes conectados (partición 9)|Count|Máxima||Sin dimensiones|
|totalcommandsprocessed9|Total de operaciones (partición 9)|Count|Total||Sin dimensiones|
|cachehits9|Aciertos de caché (partición 9)|Count|Total||Sin dimensiones|
|cachemisses9|Errores de caché (partición 9)|Count|Total||Sin dimensiones|
|getcommands9|Obtenciones (partición 9)|Count|Total||Sin dimensiones|
|setcommands9|Definiciones (partición 9)|Recuento|Total||Sin dimensiones|
|operationsPerSecond9|Operaciones por segundo (partición 9)|Count|Máxima||Sin dimensiones|
|evictedkeys9|Claves expulsadas (partición 9)|Count|Total||Sin dimensiones|
|totalkeys9|Total de claves (partición 9)|Count|Máxima||Sin dimensiones|
|expiredkeys9|Claves expiradas (partición 9)|Count|Total||Sin dimensiones|
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
|TotalCalls|Total de llamadas|Count|Total|Número total de llamadas.|ApiName, OperationName, Region|
|SuccessfulCalls|Llamadas correctas|Count|Total|Número de llamadas correctas.|ApiName, OperationName, Region|
|TotalErrors|Total de errores|Count|Total|Número total de llamadas con respuesta de error (código de respuesta HTTP 4xx o 5xx).|ApiName, OperationName, Region|
|BlockedCalls|Llamadas bloqueadas|Count|Total|Número de llamadas que han superado la tasa o el límite de cuota.|ApiName, OperationName, Region|
|ServerErrors|Errores del servidor|Count|Total|Número de llamadas con error interno del servicio (código de respuesta HTTP 5xx).|ApiName, OperationName, Region|
|ClientErrors|Errores de cliente|Count|Total|Número de llamadas con error interno del lado cliente (código de respuesta HTTP 4xx).|ApiName, OperationName, Region|
|DataIn|Entrada de datos|Bytes|Total|Tamaño de los datos de entrada en bytes.|ApiName, OperationName, Region|
|DataOut|Salida de datos|Bytes|Total|Tamaño de los datos de salida en bytes.|ApiName, OperationName, Region|
|Latencia|Latencia|MilliSeconds|Media|Latencia en milisegundos.|ApiName, OperationName, Region|
|CharactersTranslated|Caracteres traducidos|Count|Total|Número total de caracteres de la solicitud entrante de texto.|ApiName, OperationName, Region|
|CharactersTrained|Caracteres entrenados|Count|Total|Número total de caracteres entrenados.|ApiName, OperationName, Region|
|SpeechSessionDuration|Duración de la sesión de voz|Segundos|Total|Duración total de la sesión de voz en segundos.|ApiName, OperationName, Region|
|TotalTransactions|Transacciones totales|Count|Total|Número total de transacciones.|Sin dimensiones|
|TotalTokenCalls|Llamadas de token totales|Count|Total|Número total de llamadas de token.|ApiName, OperationName, Region|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Media|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|Sin dimensiones|
|Red interna|Red interna facturable|Bytes|Total|El número de bytes facturables recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|Sin dimensiones|
|Red interna|Red externa facturable|Bytes|Total|El número de bytes facturables emitidos en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|Sin dimensiones|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|Sin dimensiones|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Media|E/S por segundo de lectura de disco|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Media|E/S por segundo de escritura en disco|Sin dimensiones|
|Créditos de CPU restantes|Créditos de CPU restantes|Count|Media|Número total de créditos disponibles para irrumpir|Sin dimensiones|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Count|Media|Número total de créditos consumidos por la máquina virtual|Sin dimensiones|
|Bytes de lectura de discos por segundo|Bytes de lectura de discos de datos por segundo (en desuso)|CountPerSecond|Media|Bytes por segundo de lectura de un único disco durante el período de supervisión|SlotId|
|Bytes de escritura de discos por segundo|Bytes de escritura de discos de datos por segundo (en desuso)|CountPerSecond|Media|Bytes por segundo escritos en un único disco durante el período de supervisión|SlotId|
|Operaciones de lectura de discos por segundo|Operaciones de lectura de discos de datos por segundo (en desuso)|CountPerSecond|Media|IOPS de lectura de un único disco durante el período de supervisión|SlotId|
|Operaciones de escritura de discos por segundo|Operaciones de escritura de discos de datos por segundo (en desuso)|CountPerSecond|Media|IOPS de escritura de un único disco durante el período de supervisión|SlotId|
|Por disco QD|Profundidad de la cola de disco de datos (en desuso)|Count|Media|Profundidad de la cola del disco de datos (o longitud de la cola)|SlotId|
|Sistema operativo por bytes de lectura de discos por segundo|Bytes de lectura de discos de SO por segundo (en desuso)|CountPerSecond|Media|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por bytes de escritura de disco por segundo|Bytes de escritura de discos de SO por segundo (en desuso)|CountPerSecond|Media|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por operaciones de lectura de discos por segundo|Operaciones de lectura de discos de SO por segundo (en desuso)|CountPerSecond|Media|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Sistema operativo operaciones de escritura de discos por segundo|Operaciones de escritura de discos de SO por segundo (en desuso)|CountPerSecond|Media|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Sistema operativo por disco QD|Profundidad de la cola de disco de SO (en desuso)|Count|Media|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|Sin dimensiones|
|Bytes de lectura de discos de datos por segundo|Bytes de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Bytes por segundo de lectura de un único disco durante el período de supervisión|LUN|
|Bytes de escritura de discos de datos por segundo|Bytes de escritura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Bytes por segundo escritos en un único disco durante el período de supervisión|LUN|
|Operaciones de lectura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS de lectura de un único disco durante el período de supervisión|LUN|
|Operaciones de escritura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS de escritura de un único disco durante el período de supervisión|LUN|
|Profundidad de cola de discos de datos|Profundidad de cola de discos de datos (versión preliminar)|Count|Media|Profundidad de la cola del disco de datos (o longitud de la cola)|LUN|
|Bytes de lectura de discos del sistema operativo por segundo|Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Bytes de escritura en discos del SO por segundo|Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Operaciones de lectura de discos de SO por segundo|Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Operaciones de escritura en discos de SO por segundo|Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Profundidad de cola de discos de SO|Profundidad de cola de discos de SO (versión preliminar)|Count|Media|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|Sin dimensiones|
|Flujos de entrada|Flujos de entrada (versión preliminar)|Count|Media|Los flujos de entrada son el número de flujos actuales en la dirección entrante (el tráfico que va a la máquina virtual)|Sin dimensiones|
|Flujos de salida|Flujos de salida (versión preliminar)|Count|Media|Los flujos de salida son el número de flujos actuales en la dirección saliente (el tráfico que sale de la máquina virtual)|Sin dimensiones|
|Tasa de creación máxima de flujos de entrada|Tasa de creación máxima de flujos de entrada (versión preliminar)|CountPerSecond|Media|La tasa de creación máxima de flujos de entrada (el tráfico que va a la máquina virtual)|Sin dimensiones|
|Tasa de creación máxima de flujos de salida|Tasa de creación máxima de flujos de salida (versión preliminar)|CountPerSecond|Media|La tasa de creación máxima de flujos de salida (el tráfico que sale de la máquina virtual)|Sin dimensiones|
|Acierto de lectura de la caché de discos de datos premium|Acierto de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Media|Acierto de lectura de la caché de discos de datos premium|LUN|
|Error de lectura de la caché de discos de datos premium|Error de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Media|Error de lectura de la caché de discos de datos premium|LUN|
|Acierto de lectura de la caché de discos de SO premium|Acierto de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Media|Acierto de lectura de la caché de discos de SO premium|Sin dimensiones|
|Error de lectura de la caché de discos de SO premium|Error de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Media|Error de lectura de la caché de discos de SO premium|Sin dimensiones|
|Red entrante total|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|Sin dimensiones|
|Red saliente total|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|Sin dimensiones|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Media|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|VMName|
|Red interna|Red interna facturable|Bytes|Total|El número de bytes facturables recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|VMName|
|Red interna|Red externa facturable|Bytes|Total|El número de bytes facturables emitidos en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|VMName|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|VMName|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|VMName|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Media|E/S por segundo de lectura de disco|VMName|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Media|E/S por segundo de escritura en disco|VMName|
|Créditos de CPU restantes|Créditos de CPU restantes|Count|Media|Número total de créditos disponibles para irrumpir|Sin dimensiones|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Count|Media|Número total de créditos consumidos por la máquina virtual|Sin dimensiones|
|Bytes de lectura de discos por segundo|Bytes de lectura de discos de datos por segundo (en desuso)|CountPerSecond|Media|Bytes por segundo de lectura de un único disco durante el período de supervisión|SlotId|
|Bytes de escritura de discos por segundo|Bytes de escritura de discos de datos por segundo (en desuso)|CountPerSecond|Media|Bytes por segundo escritos en un único disco durante el período de supervisión|SlotId|
|Operaciones de lectura de discos por segundo|Operaciones de lectura de discos de datos por segundo (en desuso)|CountPerSecond|Media|IOPS de lectura de un único disco durante el período de supervisión|SlotId|
|Operaciones de escritura de discos por segundo|Operaciones de escritura de discos de datos por segundo (en desuso)|CountPerSecond|Media|IOPS de escritura de un único disco durante el período de supervisión|SlotId|
|Por disco QD|Profundidad de la cola de disco de datos (en desuso)|Count|Media|Profundidad de la cola del disco de datos (o longitud de la cola)|SlotId|
|Sistema operativo por bytes de lectura de discos por segundo|Bytes de lectura de discos de SO por segundo (en desuso)|CountPerSecond|Media|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por bytes de escritura de disco por segundo|Bytes de escritura de discos de SO por segundo (en desuso)|CountPerSecond|Media|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Sistema operativo por operaciones de lectura de discos por segundo|Operaciones de lectura de discos de SO por segundo (en desuso)|CountPerSecond|Media|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Sistema operativo operaciones de escritura de discos por segundo|Operaciones de escritura de discos de SO por segundo (en desuso)|CountPerSecond|Media|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Sistema operativo por disco QD|Profundidad de la cola de disco de SO (en desuso)|Count|Media|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|Sin dimensiones|
|Bytes de lectura de discos de datos por segundo|Bytes de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Bytes por segundo de lectura de un único disco durante el período de supervisión|LUN, VMName|
|Bytes de escritura de discos de datos por segundo|Bytes de escritura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|Bytes por segundo escritos en un único disco durante el período de supervisión|LUN, VMName|
|Operaciones de lectura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS de lectura de un único disco durante el período de supervisión|LUN, VMName|
|Operaciones de escritura de discos de datos por segundo|Operaciones de lectura de discos de datos por segundo (versión preliminar)|CountPerSecond|Media|IOPS de escritura de un único disco durante el período de supervisión|LUN, VMName|
|Profundidad de cola de discos de datos|Profundidad de cola de discos de datos (versión preliminar)|Count|Media|Profundidad de la cola del disco de datos (o longitud de la cola)|LUN, VMName|
|Bytes de lectura de discos del sistema operativo por segundo|Bytes de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|VMName|
|Bytes de escritura en discos del SO por segundo|Bytes de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|VMName|
|Operaciones de lectura de discos de SO por segundo|Operaciones de lectura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|VMName|
|Operaciones de escritura en discos de SO por segundo|Operaciones de escritura de discos del sistema operativo por segundo (versión preliminar)|CountPerSecond|Media|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|VMName|
|Profundidad de cola de discos de SO|Profundidad de cola de discos de SO (versión preliminar)|Count|Media|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|VMName|
|Flujos de entrada|Flujos de entrada (versión preliminar)|Count|Media|Los flujos de entrada son el número de flujos actuales en la dirección entrante (el tráfico que va a la máquina virtual)|VMName|
|Flujos de salida|Flujos de salida (versión preliminar)|Count|Media|Los flujos de salida son el número de flujos actuales en la dirección saliente (el tráfico que sale de la máquina virtual)|VMName|
|Tasa de creación máxima de flujos de entrada|Tasa de creación máxima de flujos de entrada (versión preliminar)|CountPerSecond|Media|La tasa de creación máxima de flujos de entrada (el tráfico que va a la máquina virtual)|VMName|
|Tasa de creación máxima de flujos de salida|Tasa de creación máxima de flujos de salida (versión preliminar)|CountPerSecond|Media|La tasa de creación máxima de flujos de salida (el tráfico que sale de la máquina virtual)|VMName|
|Acierto de lectura de la caché de discos de datos premium|Acierto de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Media|Acierto de lectura de la caché de discos de datos premium|LUN, VMName|
|Error de lectura de la caché de discos de datos premium|Error de lectura de la caché de discos de datos premium (versión preliminar)|Percent|Media|Error de lectura de la caché de discos de datos premium|LUN, VMName|
|Acierto de lectura de la caché de discos de SO premium|Acierto de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Media|Acierto de lectura de la caché de discos de SO premium|VMName|
|Error de lectura de la caché de discos de SO premium|Error de lectura de la caché de discos de SO premium (versión preliminar)|Percent|Media|Error de lectura de la caché de discos de SO premium|VMName|
|Red entrante total|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|VMName|
|Red saliente total|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|VMName|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CpuUsage|Uso de CPU|Count|Media|Uso de la CPU en todos los núcleos en millares de núcleos.|containerName|
|MemoryUsage|Uso de la memoria|Bytes|Media|Uso de memoria total en bytes.|containerName|
|NetworkBytesReceivedPerSecond|Bytes de red recibidos por segundo|Bytes|Media|Bytes de red recibidos por segundo.|Sin dimensiones|
|NetworkBytesTransmittedPerSecond|Bytes de red transmitidos por segundo|Bytes|Media|Bytes de red transmitidos por segundo.|Sin dimensiones|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TotalPullCount|Recuento total de extracciones|Count|Media|Número de extracciones de imágenes en total|Sin dimensiones|
|SuccessfulPullCount|Recuento de extracciones correctas|Count|Media|Número de extracciones de imágenes correctas|Sin dimensiones|
|TotalPushCount|Recuento total de inserciones|Count|Media|Número de inserciones de imágenes en total|Sin dimensiones|
|SuccessfulPushCount|Recuento de inserciones correctas|Count|Media|Número de inserciones de imágenes correctas|Sin dimensiones|
|RunDuration|Duración de la ejecución|Milisegundos|Total|Duración de la ejecución en milisegundos|Sin dimensiones|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Número total de núcleos de CPU disponibles en un clúster administrado|Count|Total|Número total de núcleos de CPU disponibles en un clúster administrado|Sin dimensiones|
|kube_node_status_allocatable_memory_bytes|Cantidad total de memoria disponible en un clúster administrado|Bytes|Total|Cantidad total de memoria disponible en un clúster administrado|Sin dimensiones|
|kube_pod_status_ready|Número de pods con estado Listo|Count|Total|Número de pods con estado Listo|espacio de nombres, pod|
|kube_node_status_condition|Estados de diversas condiciones de nodo|Count|Total|Estados de diversas condiciones de nodo|condition, status, status2, node|
|kube_pod_status_phase|Número de pods por fase|Count|Total|Número de pods por fase|fase, espacio de nombres, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|DCIApiCalls|Llamadas API Customer Insights|Count|Total||Sin dimensiones|
|DCIMappingImportOperationSuccessfulLines|Líneas correctas de la operación de importación de asignaciones|Count|Total||Sin dimensiones|
|DCIMappingImportOperationFailedLines|Líneas con error de la operación de importación de asignaciones|Count|Total||Sin dimensiones|
|DCIMappingImportOperationTotalLines|Líneas totales de la operación de importación de asignaciones|Count|Total||Sin dimensiones|
|DCIMappingImportOperationRuntimeInSeconds|Tiempo de ejecución en segundos de la operación de importación de asignaciones|Segundos|Total||Sin dimensiones|
|DCIOutboundProfileExportSucceeded|Exportación correcta del perfil saliente|Count|Total||Sin dimensiones|
|DCIOutboundProfileExportFailed|Error de exportación del perfil saliente|Count|Total||Sin dimensiones|
|DCIOutboundProfileExportDuration|Duración de la exportación del perfil saliente|Segundos|Total||Sin dimensiones|
|DCIOutboundKpiExportSucceeded|Exportación correcta de KPI saliente|Count|Total||Sin dimensiones|
|DCIOutboundKpiExportFailed|Error de exportación de KPI saliente|Count|Total||Sin dimensiones|
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
|ImportASAValuesFailed|Error de recuento de valores ASA de importación|Count|Total||Sin dimensiones|
|ImportASAValuesSucceeded|Recuento correcto de valores ASA de importación|Count|Total||Sin dimensiones|
|DCIProfilesCount|Recuento de instancias del perfil|Count|Último||Sin dimensiones|
|DCIInteractionsPerMonthCount|Recuento de interacciones por mes|Count|Último||Sin dimensiones|
|DCIKpisCount|Recuento de KPI|Count|Último||Sin dimensiones|
|DCISegmentsCount|Recuento de segmentos|Count|Último||Sin dimensiones|
|DCIPredictiveMatchPoliciesCount|Recuento de coincidencias predictivo|Count|Último||Sin dimensiones|
|DCIPredictionsCount|Recuento de predicción|Count|Último||Sin dimensiones|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|NICReadThroughput|Rendimiento de lectura (red)|BytesPerSecond|Media|El rendimiento de lectura de la interfaz de red en el dispositivo en el período de informe para todos los volúmenes en la puerta de enlace.|InstanceName|
|NICWriteThroughput|Rendimiento de escritura (red)|BytesPerSecond|Media|El rendimiento de escritura de la interfaz de red en el dispositivo en el período de informe para todos los volúmenes en la puerta de enlace.|InstanceName|
|CloudReadThroughputPerShare|Rendimiento de descarga en la nube (recurso compartido)|BytesPerSecond|Media|El rendimiento de descarga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|CloudUploadThroughputPerShare|Rendimiento de carga en la nube (recurso compartido)|BytesPerSecond|Media|El rendimiento de carga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|BytesUploadedToCloudPerShare|Bytes cargados en la nube (recurso compartido)|Bytes|Media|El número total de bytes que se carga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|TotalCapacity|Capacidad total|Bytes|Media|Capacidad total|Sin dimensiones|
|AvailableCapacity|Capacidad disponible|Bytes|Media|La capacidad disponible en bytes durante el período de informe.|Sin dimensiones|
|CloudUploadThroughput|Rendimiento de carga en la nube|BytesPerSecond|Media|El rendimiento de carga en la nube en Azure durante el período de informe.|Sin dimensiones|
|CloudReadThroughput|Rendimiento de descarga en la nube|BytesPerSecond|Media|El rendimiento de descarga en la nube en Azure durante el período de informe.|Sin dimensiones|
|BytesUploadedToCloud|Bytes cargados en la nube (dispositivo)|Bytes|Media|El número total de bytes que se carga en Azure desde un dispositivo durante el período de informe.|Sin dimensiones|
|HyperVVirtualProcessorUtilization|Proceso perimetral: porcentaje de CPU|Percent|Media|Porcentaje de uso de CPU|InstanceName|
|HyperVMemoryUtilization|Proceso perimetral: uso de memoria|Percent|Media|Cantidad de RAM en uso|InstanceName|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|FailedRuns|Ejecuciones con error|Count|Total||pipelineName, activityName|
|SuccessfulRuns|Ejecuciones correctas|Count|Total||pipelineName, activityName|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|PipelineFailedRuns|Métricas de ejecuciones de canalización erróneas|Count|Total||FailureType, Name|
|PipelineSucceededRuns|Las métricas de ejecuciones de canalización se realizaron correctamente|Count|Total||FailureType, Name|
|ActivityFailedRuns|Métricas de ejecuciones de actividad erróneas|Count|Total||ActivityType, PipelineName, FailureType, Name|
|ActivitySucceededRuns|Métricas de ejecución de actividad realizadas correctamente|Count|Total||ActivityType, PipelineName, FailureType, Name|
|TriggerFailedRuns|Métricas de ejecuciones de desencadenador erróneas|Count|Total||Name, FailureType|
|TriggerSucceededRuns|Métricas de ejecuciones de desencadenador realizadas correctamente|Count|Total||Name, FailureType|
|IntegrationRuntimeCpuPercentage|Uso de la CPU de entorno de ejecución de integración|Percent|Media||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableMemory|Memoria disponible de entorno de ejecución de integración|Bytes|Media||IntegrationRuntimeName, NodeName|
|MaxAllowedResourceCount|Recuento del máximo de entidades permitidas|Count|Máxima||Sin dimensiones|
|MaxAllowedFactorySizeInGbUnits|Tamaño de fábrica máximo permitido (unidad de GB)|Count|Máxima||Sin dimensiones|
|ResourceCount|Recuento total de entidades|Count|Máxima||Sin dimensiones|
|FactorySizeInGbUnits|Tamaño total de fábrica (unidad de GB)|Count|Máxima||Sin dimensiones|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|JobEndedSuccess|Trabajos correctos|Count|Total|Recuento de trabajos realizados correctamente|Sin dimensiones|
|JobEndedFailure|Trabajos con error|Count|Total|Recuento de trabajos con error|Sin dimensiones|
|JobEndedCancelled|Trabajos cancelados|Count|Total|Recuento de trabajos cancelados.|Sin dimensiones|
|JobAUEndedSuccess|Tiempo AU correcto|Segundos|Total|Tiempo total AU para trabajos realizados correctamente|Sin dimensiones|
|JobAUEndedFailure|Error de tiempo AU|Segundos|Total|Tiempo total AU para trabajos con error|Sin dimensiones|
|JobAUEndedCancelled|Tiempo de AU cancelado|Segundos|Total|Tiempo total AU para trabajos cancelados.|Sin dimensiones|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TotalStorage|Almacenamiento total|Bytes|Máxima|Cantidad total de datos almacenados en la cuenta.|Sin dimensiones|
|DataWritten|Datos escritos|Bytes|Total|Cantidad total de datos escritos en la cuenta.|Sin dimensiones|
|DataRead|Datos leídos|Bytes|Total|Cantidad total de datos leídos de la cuenta.|Sin dimensiones|
|WriteRequests|Solicitudes de escritura|Count|Total|Número de solicitudes de escritura de datos en la cuenta.|Sin dimensiones|
|ReadRequests|Solicitudes de lectura|Count|Total|Número de solicitudes de lectura de datos de la cuenta.|Sin dimensiones|

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
|active_connections|Conexiones activas|Count|Media|Conexiones activas|Sin dimensiones|
|connections_failed|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|seconds_behind_master|Intervalo de replicación en segundos|Count|Media|Intervalo de replicación en segundos|Sin dimensiones|
|backup_storage_used|Almacenamiento de copia de seguridad utilizado|Bytes|Media|Almacenamiento de copia de seguridad utilizado|Sin dimensiones|
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
|active_connections|Conexiones activas|Count|Media|Conexiones activas|Sin dimensiones|
|connections_failed|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|seconds_behind_master|Intervalo de replicación en segundos|Count|Media|Intervalo de replicación en segundos|Sin dimensiones|
|backup_storage_used|Almacenamiento de copia de seguridad utilizado|Bytes|Media|Almacenamiento de copia de seguridad utilizado|Sin dimensiones|
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
|active_connections|Conexiones activas|Count|Media|Conexiones activas|Sin dimensiones|
|connections_failed|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|backup_storage_used|Almacenamiento de copia de seguridad utilizado|Bytes|Media|Almacenamiento de copia de seguridad utilizado|Sin dimensiones|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|
|pg_replica_log_delay_in_seconds|Retraso entre réplicas|Segundos|Máxima|Intervalo de replicación en segundos|Sin dimensiones|
|pg_replica_log_delay_in_bytes|Retraso máximo entre réplicas|Bytes|Máxima|Retardo en bytes de la réplica con más retraso|Sin dimensiones|

## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Sin dimensiones|
|memory_percent|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Sin dimensiones|
|iops|E/S|Count|Media|Operaciones de E/S por segundo|Sin dimensiones|
|storage_percent|Porcentaje de almacenamiento|Percent|Media|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Almacenamiento utilizado|Bytes|Media|Almacenamiento utilizado|Sin dimensiones|
|active_connections|Conexiones activas|Count|Media|Conexiones activas|Sin dimensiones|
|network_bytes_egress|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Intentos de envío de mensajes de telemetría|Count|Total|Número de mensajes de telemetría de dispositivo a la nube para enviar a IoT Hub|Sin dimensiones|
|d2c.telemetry.ingress.success|Mensajes de telemetría enviados|Count|Total|Número de mensajes de telemetría de dispositivo a la nube enviados correctamente a IoT Hub|Sin dimensiones|
|c2d.commands.egress.complete.success|Comandos completados|Count|Total|Número de comandos de la nube al dispositivo que el dispositivo ha completado correctamente|Sin dimensiones|
|c2d.commands.egress.abandon.success|Comandos abandonados|Count|Total|Número de comandos de la nube al dispositivo que el dispositivo ha abandonado|Sin dimensiones|
|c2d.commands.egress.reject.success|Comandos rechazados|Count|Total|Número de comandos de la nube al dispositivo que el dispositivo ha rechazado|Sin dimensiones|
|devices.totalDevices|Total de dispositivos (en desuso)|Count|Total|Número de dispositivos registrados en IoT Hub|Sin dimensiones|
|devices.connectedDevices.allProtocol|Dispositivos conectados (en desuso) |Count|Total|Número de dispositivos conectados a IoT Hub|Sin dimensiones|
|d2c.telemetry.egress.success|Enrutamiento: mensajes de telemetría entregados|Count|Total|Número de veces que los mensajes se han entregado correctamente a todos los puntos de conexión mediante el enrutamiento de IoT Hub. Si un mensaje se enruta a varios puntos de conexión, este valor aumenta en uno por cada entrega correcta. Si un mensaje se enruta al mismo punto de conexión varias veces, este valor aumenta en uno por cada entrega correcta.|Sin dimensiones|
|d2c.telemetry.egress.dropped|Enrutamiento: mensajes de telemetría quitados |Count|Total|Número de veces que se han quitado los mensajes con el enrutamiento de IoT Hub a causa de puntos de conexión inactivos. Este valor no cuenta los mensajes entregados a la ruta de reserva porque los mensajes quitados no se entregan allí.|Sin dimensiones|
|d2c.telemetry.egress.orphaned|Enrutamiento: mensajes de telemetría huérfanos |Count|Total|Número de veces que los mensajes eran huérfanos a causa del enrutamiento de IoT Hub porque no coincidían con las reglas de enrutamiento (incluida la regla de reserva). |Sin dimensiones|
|d2c.telemetry.egress.invalid|Enrutamiento: mensajes de telemetría incompatibles|Count|Total|Número de veces que el enrutamiento de IoT Hub no ha podido entregar mensajes debido a una incompatibilidad con el punto de conexión. Este valor no incluye los reintentos.|Sin dimensiones|
|d2c.telemetry.egress.fallback|Enrutamiento: mensajes entregados a la reserva|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes al punto de conexión asociado con la ruta de reserva.|Sin dimensiones|
|d2c.endpoints.egress.eventHubs|Enrutamiento: mensajes entregados al centro de eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del centro de eventos.|Sin dimensiones|
|d2c.endpoints.latency.eventHubs|Enrutamiento: latencia de mensajes para el centro de eventos|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión del centro de eventos.|Sin dimensiones|
|d2c.endpoints.egress.serviceBusQueues|Enrutamiento: mensajes entregados a la cola de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de la cola de Service Bus.|Sin dimensiones|
|d2c.endpoints.latency.serviceBusQueues|Enrutamiento: latencia de mensajes de la cola de Service Bus|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de la cola de Service Bus.|Sin dimensiones|
|d2c.endpoints.egress.serviceBusTopics|Enrutamiento: mensajes entregados al tema de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del tema de Service Bus.|Sin dimensiones|
|d2c.endpoints.latency.serviceBusTopics|Enrutamiento: latencia de mensajes del tema de Service Bus|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión del tema de Service Bus.|Sin dimensiones|
|d2c.endpoints.egress.builtIn.events|Enrutamiento: mensajes entregados a mensajes/eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente al punto de conexión integrado (mensajes/eventos). Esta métrica solo comienza a funcionar cuando se habilita el enrutamiento (https://aka.ms/iotrouting) para el centro de IoT.|Sin dimensiones|
|d2c.endpoints.latency.builtIn.events|Enrutamiento: latencia de mensajes para mensajes/eventos|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría al punto de conexión integrado (mensajes/eventos). Esta métrica solo comienza a funcionar cuando se habilita el enrutamiento (https://aka.ms/iotrouting) para el centro de IoT.|Sin dimensiones|
|d2c.endpoints.egress.storage|Enrutamiento: mensajes entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.latency.storage|Enrutamiento: latencia de mensajes para almacenamiento|Milisegundos|Media|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.egress.storage.bytes|Enrutamiento: datos entregados al almacenamiento|Bytes|Total|Cantidad de datos (bytes) que el enrutamiento de IoT Hub ha entregado a los punto de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.egress.storage.blobs|Enrutamiento: blobs entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado blobs a los puntos de conexión de almacenamiento.|Sin dimensiones|
|EventGridDeliveries|Entregas de Event Grid (versión preliminar)|Count|Total|El número de eventos de IoT Hub publicados en Event Grid. Use la dimensión de resultado para ver el número de solicitudes correctas e incorrectas. La dimensión EventType muestra el tipo de evento (https://aka.ms/ioteventgrid).|Result, EventType|
|EventGridLatency|La latencia media (milisegundos) desde el momento en que se generó el evento de IoT Hub hasta el momento en que se publicó el evento en Event Grid. Este número es un promedio entre todos los tipos de evento. Use la dimensión EventType para ver la latencia de un tipo específico de evento.|EventType|
|d2c.twin.read.success|Lecturas gemelas correctas de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.read.failure|Lecturas gemelas con error de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.read.size|Tamaño de la respuesta de las lecturas gemelas de dispositivos|Bytes|Media|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.success|Actualizaciones gemelas correctas de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.failure|Actualizaciones gemelas con error de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.size|Tamaño de las actualizaciones gemelas de los dispositivos|Bytes|Media|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|c2d.methods.success|Invocaciones correctas al método directo|Count|Total|El número de todas las llamadas correctas al método directo.|Sin dimensiones|
|c2d.methods.failure|Invocaciones al método directo con error|Count|Total|El número de todas las llamadas al método directo con error.|Sin dimensiones|
|c2d.methods.requestSize|Tamaño de la solicitud de las invocaciones a métodos directos|Bytes|Media|El valor medio, mínimo y máximo de todas las solicitudes correctas de método directo.|Sin dimensiones|
|c2d.methods.responseSize|Tamaño de la respuesta de las invocaciones a métodos directos|Bytes|Media|El valor medio, mínimo y máximo de todas las respuestas correctas de método directo.|Sin dimensiones|
|c2d.twin.read.success|Lecturas gemelas correctas del back-end|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.read.failure|Lecturas gemelas con error del back-end|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.read.size|Tamaño de la respuesta de las lecturas gemelas del back-end|Bytes|Media|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.success|Actualizaciones gemelas correctas del back-end|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.failure|Actualizaciones gemelas con error del back-end|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.size|Tamaño de las actualizaciones gemelas del back-end|Bytes|Media|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|twinQueries.success|Consultas gemelas correctas|Count|Total|El recuento de todas las consultas gemelas correctas.|Sin dimensiones|
|twinQueries.failure|Consultas gemelas con error|Count|Total|El recuento de todas las consultas gemelas con error.|Sin dimensiones|
|twinQueries.resultSize|Tamaño de resultado de consultas gemelas|Bytes|Media|El valor medio, mínimo y máximo del tamaño del resultado de todas las consultas gemelas correctas.|Sin dimensiones|
|jobs.createTwinUpdateJob.success|Creaciones correctas de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones correctas de trabajos de actualización gemelos.|Sin dimensiones|
|jobs.createTwinUpdateJob.failure|Creaciones con error de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones con error de trabajos de actualización gemelos.|Sin dimensiones|
|jobs.createDirectMethodJob.success|Creaciones correctas de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones correctas de los trabajos de invocación de método directos.|Sin dimensiones|
|jobs.createDirectMethodJob.failure|Creaciones con error de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones con error de los trabajos de invocación de método directos.|Sin dimensiones|
|jobs.listJobs.success|Llamadas correctas para enumerar trabajos|Count|Total|El recuento de todas las llamadas correctas para enumerar trabajos.|Sin dimensiones|
|jobs.listJobs.failure|Llamadas con error para enumerar trabajos|Count|Total|El recuento de todas las llamadas con error para enumerar trabajos.|Sin dimensiones|
|jobs.cancelJob.success|Cancelaciones de trabajos correctas|Count|Total|El recuento de todas las llamadas correctas para cancelar un trabajo.|Sin dimensiones|
|jobs.cancelJob.failure|Cancelaciones de trabajos con error|Count|Total|El recuento de todas las llamadas con error para cancelar un trabajo.|Sin dimensiones|
|jobs.queryJobs.success|Consultas de trabajo correctas|Count|Total|El recuento de todas las llamadas correctas para consultar trabajos.|Sin dimensiones|
|jobs.queryJobs.failure|Consultas de trabajo con error|Count|Total|El recuento de todas las llamadas con error para consultar trabajos.|Sin dimensiones|
|jobs.completed|Trabajos completados|Count|Total|El recuento de todos los trabajos completados.|Sin dimensiones|
|jobs.failed|Trabajos con error|Count|Total|El recuento de todos los trabajos con error.|Sin dimensiones|
|d2c.telemetry.ingress.sendThrottle|Número de errores de limitación|Count|Total|Número de errores de limitación debido a las limitaciones de rendimiento del dispositivo|Sin dimensiones|
|dailyMessageQuotaUsed|Número total de mensajes usados|Count|Media|Número de mensajes totales usados hoy. Se trata de un valor acumulado que se restablece en cero a las 00:00 UTC cada día.|Sin dimensiones|
|deviceDataUsage|Uso total de datos del dispositivo|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|Sin dimensiones|
|totalDeviceCount|Total de dispositivos (versión preliminar)|Count|Media|Número de dispositivos registrados en IoT Hub|Sin dimensiones|
|connectedDeviceCount|Dispositivos conectados (versión preliminar)|Count|Media|Número de dispositivos conectados a IoT Hub|Sin dimensiones|
|configuraciones|Métricas de configuración|Count|Total|Métricas de las operaciones de configuración|Sin dimensiones|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|RegistrationAttempts|Intentos de registro|Count|Total|Número de intentos de registro de dispositivos|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Dispositivos asignados|Count|Total|Número de dispositivos asignados a un IoT Hub|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Intentos de atestación|Count|Total|Número de intentos de atestación de dispositivos|ProvisioningServiceName, Status, Protocol|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|AvailableStorage|Almacenamiento disponible|Bytes|Total|Almacenamiento total disponible notificado a una granularidad de cinco minutos|CollectionName, DatabaseName, Region|
|CassandraConnectionClosures|Cierres de conexión de Cassandra|Count|Total|Número de conexiones de Cassandra que se han cerrado, notificadas en una granularidad de 1 minuto|Region, ClosureReason|
|CassandraRequestCharges|Cargos de solicitud de Cassandra|Count|Total|Unidades de solicitud consumidas para las solicitudes de Cassandra realizadas|DatabaseName, CollectionName, Region, OperationType, ResourceType|
|CassandraRequests|Solicitudes de Cassandra|Count|Count|Número de solicitudes de Cassandra realizadas|DatabaseName, CollectionName, Region, OperationType, ResourceType, ErrorCode|
|DataUsage|Uso de datos|Bytes|Total|Uso total de datos notificado a una granularidad de cinco minutos|CollectionName, DatabaseName, Region|
|DocumentCount|Recuento de documentos|Count|Total|Recuento total de documentos notificado a una granularidad de cinco minutos|CollectionName, DatabaseName, Region|
|DocumentQuota|Cuota de documentos|Bytes|Total|Cuota total de almacenamiento notificada a una granularidad de cinco minutos|CollectionName, DatabaseName, Region|
|IndexUsage|Uso de índice|Bytes|Total|Uso total del índice notificado a una granularidad de cinco minutos|CollectionName, DatabaseName, Region|
|MetadataRequests|Solicitudes de metadatos|Count|Count|Recuento de las solicitudes de metadatos. Cosmos DB mantiene la colección de metadatos del sistema para cada cuenta, lo que le permite enumerar las colecciones, las bases de datos y sus configuraciones de forma gratuita.|DatabaseName, CollectionName, Region, StatusCode, |
|MongoRequestCharge|Cargo de la solicitud de Mongo|Count|Total|Unidades de la solicitud de Mongo consumidas|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequests|Solicitudes de Mongo|Count|Count|Número de solicitudes de Mongo realizadas|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|ProvisionedThroughput|Rendimiento aprovisionado|Count|Máxima|Rendimiento aprovisionado|DatabaseName, CollectionName|
|ReplicationLatency|Latencia de replicación P99|MilliSeconds|Media|Latencia de replicación P99 entre regiones de origen y destino para la cuenta habilitada geográficamente|SourceRegion, TargetRegion|
|ServiceAvailability|Disponibilidad del servicio|Percent|Media|Disponibilidad de solicitudes de cuenta en una granularidad por hora, día o mes|Sin dimensiones|
|TotalRequestUnits|Unidades de solicitud totales|Count|Total|Unidades de solicitud consumidas|DatabaseName, CollectionName, Region, StatusCode, OperationType|
|TotalRequests|Total de solicitudes|Count|Count|Número de solicitudes realizadas|DatabaseName, CollectionName, Region, StatusCode, OperationType|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishFailCount|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|UnmatchedEventCount|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Count|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|MatchedEventCount|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|Sin dimensiones|
|DeliveryAttemptFailCount|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType|
|DeliverySuccessCount|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|Sin dimensiones|
|DestinationProcessingDurationInMs|Duración del procesamiento de destino|Milisegundos|Media|Duración del procesamiento de destino en milisegundos|Sin dimensiones|
|DroppedEventCount|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason|
|DeadLetteredCount|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishFailCount|Eventos erróneos|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|UnmatchedEventCount|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Latencia de publicación correcta|Count|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas|Count|Total|Solicitudes correctas de Microsoft.EventHub.|EntityName, |
|ServerErrors|Errores del servidor.|Count|Total|Errores del servidor de Microsoft.EventHub.|EntityName, |
|UserErrors|Errores de usuario.|Count|Total|Errores de usuario de Microsoft.EventHub.|EntityName, |
|QuotaExceededErrors|Cuota de errores superada.|Count|Total|Cuota de errores superada de Microsoft.EventHub.|EntityName, |
|ThrottledRequests|Solicitudes limitadas.|Count|Total|Solicitudes limitadas de Microsoft.EventHub.|EntityName, |
|IncomingRequests|Solicitudes entrantes|Count|Total|Solicitudes entrantes de Microsoft.EventHub.|EntityName|
|IncomingMessages|Mensajes entrantes|Count|Total|Mensajes entrantes de Microsoft.EventHub.|EntityName|
|OutgoingMessages|Mensajes salientes|Count|Total|Mensajes salientes de Microsoft.EventHub.|EntityName|
|IncomingBytes|Bytes entrantes.|Bytes|Total|Bytes entrantes de Microsoft.EventHub.|EntityName|
|OutgoingBytes|Bytes salientes.|Bytes|Total|Bytes salientes de Microsoft.EventHub.|EntityName|
|ActiveConnections|ActiveConnections|Count|Media|Total de conexiones activas de Microsoft.EventHub.|Sin dimensiones|
|ConnectionsOpened|Conexiones abiertas.|Count|Media|Conexiones abiertas de Microsoft.EventHub.|EntityName|
|ConnectionsClosed|Conexiones cerradas.|Count|Media|Conexiones cerradas de Microsoft.EventHub.|EntityName|
|CaptureBacklog|Capturar el trabajo pendiente.|Count|Total|Capturar el trabajo pendiente de Microsoft.EventHub.|EntityName|
|CapturedMessages|Mensajes capturados.|Count|Total|Mensajes capturados de Microsoft.EventHub.|EntityName|
|CapturedBytes|Bytes capturados.|Bytes|Total|Bytes capturados de Microsoft.EventHub.|EntityName|
|Size|Size|Bytes|Media|Tamaño de EventHub en bytes.|EntityName|
|INREQS|Solicitudes entrantes (en desuso)|Count|Total|Total de solicitudes de envío entrantes de un espacio de nombres (en desuso)|Sin dimensiones|
|SUCCREQ|Solicitudes correctas (en desuso)|Count|Total|Total de solicitudes correctas para un espacio de nombres (en desuso)|Sin dimensiones|
|FAILREQ|Solicitudes con error (en desuso)|Count|Total|Total de solicitudes con error para un espacio de nombres (en desuso)|Sin dimensiones|
|SVRBSY|Errores de servidor ocupado (en desuso)|Count|Total|Errores totales de servidor ocupado para un espacio de nombres (en desuso)|Sin dimensiones|
|INTERR|Errores internos del servidor (en desuso)|Count|Total|Errores totales de servidor interno para un espacio de nombres (en desuso)|Sin dimensiones|
|MISCERR|Otros errores (en desuso)|Count|Total|Total de solicitudes con error para un espacio de nombres (en desuso)|Sin dimensiones|
|INMSGS|Mensajes entrantes (en desuso) (en desuso)|Count|Total|Total de mensajes entrantes para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Mensajes entrantes (en desuso)|Sin dimensiones|
|EHINMSGS|Mensajes entrantes (en desuso)|Count|Total|Total de mensajes entrantes para un espacio de nombres (en desuso)|Sin dimensiones|
|OUTMSGS|Mensajes salientes (en desuso) (en desuso)|Count|Total|Total de mensajes salientes para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Mensajes salientes (en desuso)|Sin dimensiones|
|EHOUTMSGS|Mensajes salientes (en desuso)|Count|Total|Total de mensajes salientes para un espacio de nombres (en desuso)|Sin dimensiones|
|EHINMBS|Bytes de entrada (en desuso) (en desuso)|Bytes|Total|Rendimiento de mensajes entrantes del centro de eventos para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Bytes de entrada (en desuso)|Sin dimensiones|
|EHINBYTES|Bytes de entrada (en desuso)|Bytes|Total|Rendimiento de mensajes entrantes del centro de eventos para un espacio de nombres (en desuso)|Sin dimensiones|
|EHOUTMBS|Bytes de salida (en desuso) (en desuso)|Bytes|Total|Rendimiento de mensajes salientes del centro de eventos para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Bytes de salida (en desuso)|Sin dimensiones|
|EHOUTBYTES|Bytes de salida (en desuso)|Bytes|Total|Rendimiento de mensajes salientes del centro de eventos para un espacio de nombres (en desuso)|Sin dimensiones|
|EHABL|Mensajes de trabajo pendiente de archivado (en desuso)|Count|Total|Mensajes de archivado del centro de eventos en el trabajo pendiente para un espacio de nombres (en desuso)|Sin dimensiones|
|EHAMSGS|Mensajes de archivado (en desuso)|Count|Total|Mensajes de archivado del centro de eventos en un espacio de nombres (en desuso)|Sin dimensiones|
|EHAMBS|Rendimiento de mensajes de archivado (en desuso)|Bytes|Total|Rendimiento de mensajes de archivado del centro de eventos en un espacio de nombres (en desuso)|Sin dimensiones|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas (versión preliminar)|Count|Total|Solicitudes correctas de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|ServerErrors|Errores del servidor. (versión preliminar)|Count|Total|Errores del servidor de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|UserErrors|Errores de usuario. (versión preliminar)|Count|Total|Errores de usuario de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|QuotaExceededErrors|Cuota de errores superada. (versión preliminar)|Count|Total|Cuota de errores superada de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|ThrottledRequests|Solicitudes limitadas. (versión preliminar)|Count|Total|Solicitudes limitadas de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|IncomingRequests|Solicitudes entrantes (versión preliminar)|Count|Total|Solicitudes entrantes de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|IncomingMessages|Mensajes entrantes (versión preliminar)|Count|Total|Mensajes entrantes de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|OutgoingMessages|Mensajes salientes (versión preliminar)|Count|Total|Mensajes salientes de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|IncomingBytes|Bytes entrantes. (versión preliminar)|Bytes|Total|Bytes entrantes de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|OutgoingBytes|Bytes salientes. (versión preliminar)|Bytes|Total|Bytes salientes de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|ActiveConnections|ActiveConnections (versión preliminar)|Count|Media|Total de conexiones activas de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|ConnectionsOpened|Conexiones abiertas. (versión preliminar)|Count|Media|Conexiones abiertas de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|ConnectionsClosed|Conexiones cerradas. (versión preliminar)|Count|Media|Conexiones cerradas de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|CaptureBacklog|Capturar el trabajo pendiente. (versión preliminar)|Count|Total|Capturar el trabajo pendiente de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|CapturedMessages|Mensajes capturados. (versión preliminar)|Count|Total|Mensajes capturados de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|CapturedBytes|Bytes capturados. (versión preliminar)|Bytes|Total|Bytes capturados de Microsoft.EventHub. (versión preliminar)|Sin dimensiones|
|CPU|CPU (versión preliminar)|Percent|Máxima|Uso de la CPU para el clúster del centro de eventos como porcentaje|Role|
|AvailableMemory|Memoria disponible (versión preliminar)|Count|Máxima|Memoria disponible para el clúster del centro de eventos en bytes|Role|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|GatewayRequests|Solicitudes de puerta de enlace|Count|Total|Número de solicitudes de puerta de enlace|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Solicitudes de puerta de enlace categorizadas|Count|Total|Número de solicitudes de puerta de enlace por categorías (1xx/2xx/3xx/4xx/5xx)|ClusterDnsName, HttpStatus|
|NumActiveWorkers|Número de trabajos activos|Count|Máxima|Número de trabajos activos|ClusterDnsName, MetricName|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ObservedMetricValue|Valor de métrica observado|Count|Media|El valor calculado por el escalado automático cuando se ejecuta|MetricTriggerSource|
|MetricThreshold|Umbral de métrica|Count|Media|El umbral configurado de escalado automático cuando se ejecutó el escalado automático.|MetricTriggerRule|
|ObservedCapacity|Capacidad observada|Count|Media|La capacidad informada al escalado automático cuando se ejecutó.|Sin dimensiones|
|ScaleActionsInitiated|Acciones de escalado iniciadas|Count|Total|La dirección de la operación de escalado.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

(Vista previa pública)

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Disponibilidad|Percent|Media|Porcentaje de pruebas de disponibilidad completadas correctamente|availabilityResult/name, availabilityResult/location|
|availabilityResults/count|Pruebas de disponibilidad|Count|Count|Recuento de pruebas de disponibilidad|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|availabilityResults/duration|Duración de la prueba de disponibilidad|MilliSeconds|Media|Duración de la prueba de disponibilidad|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|browserTimings/networkDuration|Tiempo de conexión de red de carga de página|MilliSeconds|Media|Tiempo transcurrido entre la solicitud del usuario y la conexión de red. Incluye la búsqueda de DNS y la conexión de transporte.|Sin dimensiones|
|browserTimings/processingDuration|Tiempo de procesamiento del cliente|MilliSeconds|Media|Tiempo transcurrido entre la recepción del último byte de un documento hasta que se carga el DOM. Todavía se pueden procesar solicitudes asincrónicas.|Sin dimensiones|
|browserTimings/receiveDuration|Tiempo de recepción de respuesta|MilliSeconds|Media|Tiempo transcurrido entre el primer y el último byte, o hasta la desconexión.|Sin dimensiones|
|browserTimings/sendDuration|Tiempo de solicitud de envío|MilliSeconds|Media|Tiempo transcurrido entre la conexión de red y la recepción del primer byte.|Sin dimensiones|
|browserTimings/totalDuration|Tiempo de carga de página del explorador|MilliSeconds|Media|Tiempo que transcurre entre la solicitud del usuario hasta que se cargan el DOM, las hojas de estilo, los scripts y las imágenes.|Sin dimensiones|
|dependencies/count|Llamadas de dependencia|Count|Count|Número de llamadas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, dependency/success, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/duration|Duración de la dependencia|MilliSeconds|Media|Duración de las llamadas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, dependency/success, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/failed|Errores de llamadas de dependencia|Count|Count|Número de llamadas de dependencia erróneas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, operation/synthetic, cloud/roleInstance, cloud/roleName|
|pageViews/count|Vistas de página|Count|Count|Número de vistas de página.|operation/synthetic|
|pageViews/duration|Tiempo de carga de la vista de página|MilliSeconds|Media|Tiempo de carga de la vista de página|operation/synthetic|
|performanceCounters/requestExecutionTime|Tiempo de ejecución de solicitud HTTP|MilliSeconds|Media|Tiempo de ejecución de la solicitud más reciente.|cloud/roleInstance|
|performanceCounters/requestsInQueue|Solicitudes HTTP en la cola de la aplicación|Count|Media|Longitud de la cola de solicitudes de aplicación.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|Tasa de solicitudes HTTP|CountPerSecond|Media|Velocidad de todas las solicitudes a la aplicación por segundo desde ASP.NET.|cloud/roleInstance|
|performanceCounters/exceptionsPerSecond|Velocidad de excepciones|CountPerSecond|Media|Recuento de excepciones controladas y no controladas de las cuales se informó a Windows, incluidas las excepciones de .NET y las excepciones no administradas que se convierten en excepciones de .NET.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|Velocidad de E/S del proceso|BytesPerSecond|Media|Número total de bytes por segundo leídos y escritos en los archivos, la red y los dispositivos.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|CPU de procesos|Percent|Media|Porcentaje de tiempo transcurrido que todos los subprocesos del proceso han usado el procesador para ejecutar instrucciones. Puede variar entre 0 y 100. Esta métrica indica el rendimiento solo del proceso w3wp.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Tiempo de procesador|Percent|Media|Porcentaje de tiempo que invierte el procesador en subprocesos no inactivos.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Memoria disponible|Bytes|Media|Memoria física disponible de inmediato para su asignación a un proceso o para uso del sistema.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Bytes privados del proceso|Bytes|Media|Memoria asignada exclusivamente a los procesos de la aplicación supervisada.|cloud/roleInstance|
|requests/duration|Tiempo de respuesta del servidor|MilliSeconds|Media|Tiempo entre que se recibe una solicitud HTTP y se termina de enviar la respuesta.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/count|Solicitudes de servidor|Count|Count|Número de solicitudes HTTP completadas.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/failed|Error en las solicitudes|Count|Count|Número de solicitudes HTTP marcadas como erróneas. En la mayoría de los casos, se trata de solicitudes con un código de respuesta > = 400 y no igual a 401.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|requests/rate|Tasa de solicitudes del servidor|CountPerSecond|Media|Tasa de solicitudes del servidor por segundo|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|exceptions/count|Excepciones|Count|Count|Recuento combinado de todas las excepciones no detectadas.|cloud/roleName, cloud/roleInstance, client/type|
|exceptions/browser|Excepciones de explorador|Count|Count|Recuento de excepciones no detectadas en el explorador.|Sin dimensiones|
|exceptions/server|Excepciones de servidor|Count|Count|Número de excepciones no detectadas producidas en la aplicación de servidor.|cloud/roleName, cloud/roleInstance|
|traces/count|Seguimientos|Count|Count|Número de documentos de seguimiento|trace/severityLevel, operation/synthetic, cloud/roleName, cloud/roleInstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ServiceApiHit|Número total de visitas de la API de servicio|Count|Count|Número total de visitas de la API de servicio|ActivityType, ActivityName|
|ServiceApiLatency|Latencia general de la API de servicio|Milisegundos|Media|Latencia general de las solicitudes de la API de servicio|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Número total de resultados de la API de servicio|Count|Count|Número total de resultados de la API de servicio|ActivityType, ActivityName, StatusCode|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CacheUtilization|Uso de la caché|Percent|Media|Nivel de uso en el ámbito de clúster|None|
|QueryDuration|Duración de la consulta|Milisegundos|Media|Duración de las consultas en segundos|Consultar el estado|
|IngestionUtilization|Uso de la ingesta|Percent|Media|Proporción de ranuras de ingesta usadas en el clúster|None|
|KeepAlive|Mantener conexión|Count|Media|La comprobación de integridad indica las respuestas del clúster a las consultas|None|
|IngestionVolumeInMB|Volumen de ingesta (en MB)|Count|Total|Volumen global de la ingesta de datos del clúster (en MB)|Base de datos|
|IngestionLatencyInSeconds|Latencia de la ingesta (en segundos)|Segundos|Media|Tiempo de ingesta del origen (p. ej., el mensaje está en el centro de eventos) al clúster en segundos|None|
|EventProcessedForEventHubs|Eventos procesados (para Event Hubs)|Count|Total|Número de eventos que ha procesado el clúster en la ingesta desde el centro de eventos|None|
|IngestionResult|Resultado de la ingesta|Count|Count|Número de operaciones de ingesta|Status|
|CPU|CPU|Percent|Media|Nivel de uso de CPU|None|
| ContinuousExportNumOfRecordsExported | Número de registros exportados en la exportación continua | Count | Total | Número de registros exportados para cada artefacto de almacenamiento escrito durante la operación de exportación  | None |
| ExportUtilization | Utilización de la exportación | Percent | Máxima | Utilización de la exportación | None |
| ContinuousExportPendingCount | Recuento pendiente de exportación continua | Count | Máxima | El número de trabajos de exportación continua pendientes listos para su ejecución | None |
| ContinuousExportMaxLatenessMinutes | Minutos de retraso máximos de exportación continua | Count | Máxima | El tiempo máximo en minutos de todas las exportaciones continuas pendientes y listas para su ejecución | None |

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Uso|Uso|Count|Count|Recuento de llamadas a la API|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|RunsStarted|Ejecuciones iniciadas|Count|Total|Número de ejecuciones de flujo de trabajo iniciadas|Sin dimensiones|
|RunsCompleted|Ejecuciones completadas|Count|Total|Número de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunsSucceeded|Ejecuciones correctas|Count|Total|Número de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunsFailed|Ejecuciones con errores|Count|Total|Número de ejecuciones de flujo de trabajo con errores|Sin dimensiones|
|RunsCancelled|Ejecuciones canceladas|Count|Total|Número de ejecuciones de flujo de trabajo canceladas.|Sin dimensiones|
|RunLatency|Latencia de ejecución|Segundos|Media|Latencia de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunSuccessLatency|Latencia de ejecuciones correctas|Segundos|Media|Latencia de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunThrottledEvents|Ejecución de eventos limitados|Count|Total|Número de eventos limitados de desencadenador o acciones de flujo de trabajo|Sin dimensiones|
|RunFailurePercentage|Porcentaje de errores de ejecución|Percent|Total|Porcentaje de ejecuciones de flujo de trabajo con errores.|Sin dimensiones|
|ActionsStarted|Acciones iniciadas |Count|Total|Número de acciones de flujo de trabajo iniciadas|Sin dimensiones|
|ActionsCompleted|Acciones completadas |Count|Total|Número de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionsSucceeded|Acciones correctas |Count|Total|Número de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionsFailed|Acciones con errores|Count|Total|Número de acciones de flujo de trabajo con errores|Sin dimensiones|
|ActionsSkipped|Acciones omitidas |Count|Total|Número de acciones de flujo de trabajo omitidas|Sin dimensiones|
|ActionLatency|Latencia de acciones |Segundos|Media|Latencia de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionSuccessLatency|Latencia de acciones correctas |Segundos|Media|Latencia de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionThrottledEvents|Eventos limitados de acciones|Count|Total|Número de eventos limitados de acciones de flujo de trabajo|Sin dimensiones|
|TriggersStarted|Desencadenadores iniciados |Count|Total|Número de desencadenadores de flujo de trabajo iniciados|Sin dimensiones|
|TriggersCompleted|Desencadenadores completados |Count|Total|Número de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggersSucceeded|Desencadenadores correctos |Count|Total|Número de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggersFailed|Desencadenadores con errores |Count|Total|Número de desencadenadores de flujo de trabajo con errores|Sin dimensiones|
|TriggersSkipped|Desencadenadores omitidos|Count|Total|Número de desencadenadores de flujo de trabajo omitidos|Sin dimensiones|
|TriggersFired|Desencadenadores activados |Count|Total|Número de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggerLatency|Latencia de desencadenador |Segundos|Media|Latencia de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggerFireLatency|Latencia de desencadenador activado |Segundos|Media|Latencia de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggerSuccessLatency|Latencia de desencadenadores correctos |Segundos|Media|Latencia de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggerThrottledEvents|Eventos limitados de desencadenadores|Count|Total|Número de eventos limitados de desencadenador de flujo de trabajo|Sin dimensiones|
|BillableActionExecutions|Ejecuciones de acciones facturables|Count|Total|Número de ejecuciones de acciones de flujo de trabajo que se facturan.|Sin dimensiones|
|BillableTriggerExecutions|Ejecuciones del desencadenador facturable|Count|Total|Número de ejecuciones del desencadenador de flujo de trabajo que se factura.|Sin dimensiones|
|TotalBillableExecutions|Total de ejecuciones facturables|Count|Total|Número de ejecuciones de flujo de trabajo que se factura.|Sin dimensiones|
|BillingUsageNativeOperation|Uso de facturación para las ejecuciones de la operación nativa|Count|Total|Número de ejecuciones de la operación nativa que se facturan.|Sin dimensiones|
|BillingUsageStandardConnector|Uso de facturación para las ejecuciones del conector estándar|Count|Total|Número de ejecuciones del conector estándar que se facturan.|Sin dimensiones|
|BillingUsageStorageConsumption|Uso de facturación para las ejecuciones de consumo de almacenamiento|Count|Total|Número de ejecuciones del consumo de almacenamiento que se facturan.|Sin dimensiones|
|BillingUsageNativeOperation|Uso de facturación para las ejecuciones de la operación nativa|Count|Total|Número de ejecuciones de la operación nativa que se facturan.|Sin dimensiones|
|BillingUsageStandardConnector|Uso de facturación para las ejecuciones del conector estándar|Count|Total|Número de ejecuciones del conector estándar que se facturan.|Sin dimensiones|
|BillingUsageStorageConsumption|Uso de facturación para las ejecuciones de consumo de almacenamiento|Count|Total|Número de ejecuciones del consumo de almacenamiento que se facturan.|Sin dimensiones|

## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/integrationServiceEnvironments

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|RunsStarted|Ejecuciones iniciadas|Count|Total|Número de ejecuciones de flujo de trabajo iniciadas|Sin dimensiones|
|RunsCompleted|Ejecuciones completadas|Count|Total|Número de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunsSucceeded|Ejecuciones correctas|Count|Total|Número de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunsFailed|Ejecuciones con errores|Count|Total|Número de ejecuciones de flujo de trabajo con errores|Sin dimensiones|
|RunsCancelled|Ejecuciones canceladas|Count|Total|Número de ejecuciones de flujo de trabajo canceladas.|Sin dimensiones|
|RunLatency|Latencia de ejecución|Segundos|Media|Latencia de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunSuccessLatency|Latencia de ejecuciones correctas|Segundos|Media|Latencia de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunThrottledEvents|Ejecución de eventos limitados|Count|Total|Número de eventos limitados de desencadenador o acciones de flujo de trabajo|Sin dimensiones|
|RunStartThrottledEvents|Ejecución de eventos limitados de inicio|Count|Total|Número de eventos limitados de inicio de ejecución de flujo de trabajo.|Sin dimensiones|
|RunFailurePercentage|Porcentaje de errores de ejecución|Percent|Total|Porcentaje de ejecuciones de flujo de trabajo con errores.|Sin dimensiones|
|ActionsStarted|Acciones iniciadas |Count|Total|Número de acciones de flujo de trabajo iniciadas|Sin dimensiones|
|ActionsCompleted|Acciones completadas |Count|Total|Número de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionsSucceeded|Acciones correctas |Count|Total|Número de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionsFailed|Acciones con errores |Count|Total|Número de acciones de flujo de trabajo con errores|Sin dimensiones|
|ActionsSkipped|Acciones omitidas |Count|Total|Número de acciones de flujo de trabajo omitidas|Sin dimensiones|
|ActionLatency|Latencia de acciones |Segundos|Media|Latencia de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionSuccessLatency|Latencia de acciones correctas |Segundos|Media|Latencia de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionThrottledEvents|Eventos limitados de acciones|Count|Total|Número de eventos limitados de acciones de flujo de trabajo|Sin dimensiones|
|TriggersStarted|Desencadenadores iniciados |Count|Total|Número de desencadenadores de flujo de trabajo iniciados|Sin dimensiones|
|TriggersCompleted|Desencadenadores completados |Count|Total|Número de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggersSucceeded|Desencadenadores correctos |Count|Total|Número de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggersFailed|Desencadenadores con errores |Count|Total|Número de desencadenadores de flujo de trabajo con errores|Sin dimensiones|
|TriggersSkipped|Desencadenadores omitidos|Count|Total|Número de desencadenadores de flujo de trabajo omitidos|Sin dimensiones|
|TriggersFired|Desencadenadores activados |Count|Total|Número de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggerLatency|Latencia de desencadenador |Segundos|Media|Latencia de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggerFireLatency|Latencia de desencadenador activado |Segundos|Media|Latencia de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggerSuccessLatency|Latencia de desencadenadores correctos |Segundos|Media|Latencia de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggerThrottledEvents|Eventos limitados de desencadenadores|Count|Total|Número de eventos limitados de desencadenador de flujo de trabajo|Sin dimensiones|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Uso del procesador de flujo de trabajo para el entorno del servicio de integración|Percent|Media|Uso del procesador de flujo de trabajo para el entorno de servicio de integración.|Sin dimensiones|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Uso del memoria de flujo de trabajo para el entorno del servicio de integración|Percent|Media|Uso del memoria de flujo de trabajo para el entorno del servicio de integración.|Sin dimensiones|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Uso del procesador del conector para el entorno del servicio de integración|Percent|Media|Uso del procesador del conector para el entorno del servicio de integración.|Sin dimensiones|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Uso de la memoria del conector para el entorno del servicio de integración|Percent|Media|Uso de la memoria del conector para el entorno del servicio de integración.|Sin dimensiones|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Ejecuciones finalizadas|Ejecuciones finalizadas|Count|Total|Número de ejecuciones finalizadas correctamente para esta área de trabajo|Escenario|
|Ejecuciones iniciadas|Ejecuciones iniciadas|Count|Total|Número de ejecuciones iniciadas para esta área de trabajo|Escenario|
|Ejecuciones con error|Ejecuciones con error|Count|Total|Número de ejecuciones con error para esta área de trabajo|Escenario|

## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Uso|Uso|Count|Count|Recuento de llamadas a la API|ApiCategory, ApiName, ResultType, ResponseCode|
|Disponibilidad|Disponibilidad|Percent|Media|Disponibilidad de las API|ApiCategory, ApiName|

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
|BytesSentRate|Bytes enviados|Count|Total|Número de bytes de la interfaz de red enviados|Sin dimensiones|
|BytesReceivedRate|Bytes recibidos|Count|Total|Número de bytes de la interfaz de red recibidos|Sin dimensiones|
|PacketsSentRate|Paquetes enviados|Count|Total|Número de paquetes de la interfaz de red enviados|Sin dimensiones|
|PacketsReceivedRate|Paquetes recibidos|Count|Total|Número de paquetes de la interfaz de red recibidos|Sin dimensiones|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|VipAvailability|Disponibilidad de la ruta de acceso de datos|Count|Media|Disponibilidad media de la ruta de acceso de datos de Load Balancer por duración|FrontendIPAddress, FrontendPort|
|DipAvailability|Estado del sondeo de mantenimiento|Count|Media|Estado del sondeo de mantenimiento medio de Load Balancer por duración|ProtocolType, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|ByteCount|Recuento de bytes|Count|Total|Número total de bytes transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|PacketCount|Recuento de paquetes|Count|Total|Número total de paquetes transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|SYNCount|Recuento de SYN|Count|Total|Número total de paquetes SYN transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|SnatConnectionCount|Recuento de conexiones SNAT|Count|Total|Número total de conexiones SNAT nuevas creadas en un período|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Puertos SNAT asignados (versión preliminar)|Count|Total|Número total de puertos SNAT asignados dentro del período de tiempo|FrontendIPAddress, BackendIPAddress, ProtocolType|
|UsedSnatPorts|Puertos SNAT usados (versión preliminar)|Count|Total|Número total de puertos SNAT usados dentro del período de tiempo|FrontendIPAddress, BackendIPAddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|QueryVolume|Volumen de consulta|Count|Total|Número de consultas que se sirven para una zona DNS|Sin dimensiones|
|RecordSetCount|Recuento de conjunto de registros|Count|Máxima|Número de conjuntos de registros de una zona DNS|Sin dimensiones|
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
|IfUnderDDoSAttack|Bajo ataque de DDoS o no|Count|Máxima|Bajo ataque de DDoS o no|Sin dimensiones|
|DDoSTriggerTCPPackets|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|Sin dimensiones|
|DDoSTriggerUDPPackets|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|Sin dimensiones|
|DDoSTriggerSYNPackets|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|Sin dimensiones|
|VipAvailability|Disponibilidad de la ruta de acceso de datos|Count|Media|Disponibilidad media de la dirección IP por duración|Port|
|ByteCount|Recuento de bytes|Count|Total|Número total de bytes transmitidos en un período|Port, Direction|
|PacketCount|Recuento de paquetes|Count|Total|Número total de paquetes transmitidos en un período|Port, Direction|
|SynCount|Recuento de SYN|Count|Total|Número total de paquetes SYN transmitidos en un período|Port, Direction|

## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ApplicationRuleHit|Recuento de llamadas de las reglas de aplicación|Count|Total|Número de veces que se llamó a las reglas de aplicación|Status, Reason, Protocol|
|NetworkRuleHit|Número de llamadas de reglas de red|Count|Total|Número de veces que se llamó a las reglas de red|Status, Reason, Protocol|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Throughput|Throughput|BytesPerSecond|Total|Número de bytes por segundo que ha ofrecido Application Gateway|Sin dimensiones|
|UnhealthyHostCount|Recuento de hosts con estado incorrecto|Count|Media|Número de hosts de back-end con estado incorrecto|BackendSettingsPool|
|HealthyHostCount|Recuento de hosts con estado correcto|Count|Media|Número de hosts de back-end con estado correcto|BackendSettingsPool|
|TotalRequests|Total de solicitudes|Count|Total|Recuento de solicitudes correctas que ha servido Application Gateway|BackendSettingsPool|
|FailedRequests|Solicitudes con error|Count|Total|Recuento de solicitudes con error que ha servido Application Gateway|BackendSettingsPool|
|ResponseStatus|Estado de respuesta|Count|Total|Estado de respuesta HTTP devuelto por Application Gateway|HttpStatusGroup|
|CurrentConnections|Conexiones actuales|Count|Total|Recuento de conexiones actuales establecidas con Application Gateway|Sin dimensiones|
|CapacityUnits|Unidades de capacidad actuales|Count|Media|Unidades de capacidad consumidas|Sin dimensiones|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|AverageBandwidth|Ancho de banda S2S de puerta de enlace|BytesPerSecond|Media|Ancho de banda de sitio a sitio medio de una puerta de enlace en bytes por segundo|Sin dimensiones|
|P2SBandwidth|Ancho de banda P2S de puerta de enlace|BytesPerSecond|Media|Ancho de banda de punto a sitio medio de una puerta de enlace en bytes por segundo|Sin dimensiones|
|P2SConnectionCount|Recuento de conexiones P2S|Count|Máxima|Recuento de conexiones de punto a sitio de una puerta de enlace|Protocolo|
|TunnelAverageBandwidth|Ancho de banda de túnel|BytesPerSecond|Media|Media de ancho de banda de un túnel en bytes por segundo|ConnectionName, RemoteIP|
|TunnelEgressBytes|Bytes de salida de túnel|Bytes|Total|Bytes de salida de un túnel|ConnectionName, RemoteIP|
|TunnelIngressBytes|Bytes de salida de túnel|Bytes|Total|Bytes de entrada de un túnel|ConnectionName, RemoteIP|
|TunnelEgressPackets|Paquetes de salida de túnel|Count|Total|Recuento de paquetes salientes de un túnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|Paquetes de entrada de túnel|Count|Total|Recuento de paquetes entrantes de un túnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de salida|Count|Total|Recuento de paquetes de salida con error de coincidencia del selector de tráfico de un túnel|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de entrada|Count|Total|Recuento de paquetes entrantes con error de coincidencia del selector de tráfico de un túnel|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Media|Bits que ingresan a Azure por segundo|PeeringType|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Media|Bits que egresan de Azure por segundo|PeeringType|

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
|QpsByEndpoint|Consultas por punto de conexión devueltas|Count|Total|Número de veces que se devolvió un punto de conexión de Traffic Manager en el período dado|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Estado de punto de conexión por punto de conexión|Count|Máxima|1 si el estado de sondeo de un punto de conexión es “Habilitado”, 0 en caso contrario.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ProbesFailedPercent|% de sondeos con error|Percent|Media|% de sondeos de supervisión de conectividad con error|Sin dimensiones|
|AverageRoundtripMs|Prom. Tiempo de ida y vuelta (ms)|MilliSeconds|Media|Promedio de tiempo de ida y vuelta de red (ms) para los sondeos de supervisión de conectividad que se envían entre el origen y el destino|Sin dimensiones|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|RequestCount|Recuento de solicitudes|Count|Total|Número de solicitudes de cliente que atiende el proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Tamaño de la solicitud|Bytes|Total|Número de bytes enviados como solicitudes de clientes al proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Tamaño de la respuesta|Bytes|Total|Número de bytes enviados como respuestas del proxy HTTP/S a los clientes|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Recuento de solicitudes de back-end|Count|Total|Número de solicitudes enviadas del proxy HTTP/S a los back-end|HttpStatus, HttpStatusGroup, Backend|
|BackendRequestLatency|Latencia de las solicitudes de back-end|MilliSeconds|Media|Tiempo calculado desde que el proxy HTTP/S ha enviado la solicitud al back-end hasta que el proxy HTTP/S ha recibido el último byte de respuesta del back-end|Back-end|
|TotalLatency|Latencia total|MilliSeconds|Media|Tiempo calculado desde que el proxy HTTP/S ha recibido la solicitud de cliente hasta que el cliente ha reconocido el último byte de respuesta del proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Porcentaje de estado del back-end|Percent|Media|El porcentaje de sondeos de estado correctos del proxy HTTP/S a los back-ends|Backend, BackendPool|
|WebApplicationFirewallRequestCount|Recuento de solicitudes del firewall de aplicaciones web|Count|Total|Número de solicitudes de cliente procesadas por el firewall de aplicaciones web|PolicyName, RuleName, Action|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|registration.all|Operaciones de registro|Count|Total|El recuento de todas las operaciones de registro correctas (creaciones, actualizaciones, consultas y eliminaciones). |Sin dimensiones|
|registration.create|Operaciones de creación de registros|Count|Total|El recuento de todas las creaciones de registros correctas.|Sin dimensiones|
|registration.update|Operaciones de actualización de registros|Count|Total|El recuento de todas las actualizaciones de registros correctas.|Sin dimensiones|
|registration.get|Operaciones de lectura de registros|Count|Total|El recuento de todas las consultas de registros correctas.|Sin dimensiones|
|registration.delete|Operaciones de eliminación de registros|Count|Total|El recuento de todas las eliminaciones de registros correctas.|Sin dimensiones|
|incoming|Mensajes entrantes|Count|Total|El recuento de todas las llamadas a la API de envío. |Sin dimensiones|
|incoming.scheduled|Notificaciones push programadas enviadas|Count|Total|Notificaciones push programadas canceladas|Sin dimensiones|
|incoming.scheduled.cancel|Notificaciones push programadas canceladas|Count|Total|Notificaciones push programadas canceladas|Sin dimensiones|
|scheduled.pending|Notificaciones programadas pendientes|Count|Total|Notificaciones programadas pendientes|Sin dimensiones|
|installation.all|Operaciones de administración de instalaciones|Count|Total|Operaciones de administración de instalaciones|Sin dimensiones|
|installation.get|Obtener operaciones de instalación|Count|Total|Obtener operaciones de instalación|Sin dimensiones|
|installation.upsert|Crear o actualizar operaciones de instalación|Count|Total|Crear o actualizar operaciones de instalación|Sin dimensiones|
|installation.patch|Aplicar revisión a operaciones de instalación|Count|Total|Aplicar revisión a operaciones de instalación|Sin dimensiones|
|installation.delete|Eliminar operaciones de instalación|Count|Total|Eliminar operaciones de instalación|Sin dimensiones|
|outgoing.allpns.success|Notificaciones correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.allpns.invalidpayload|Errores de carga útil|Count|Total|El recuento de las inserciones que provocaron un error porque el PNS devolvió un error de carga útil incorrecta.|Sin dimensiones|
|outgoing.allpns.pnserror|Errores del sistema de notificación externo|Count|Total|El recuento de inserciones que provocaron un error porque se produjo un problema al comunicarse con el PNS (excluye los problemas de autenticación).|Sin dimensiones|
|outgoing.allpns.channelerror|Errores de canal|Count|Total|El recuento de inserciones que provocaron un error porque el canal no era válido no está asociado a la aplicación correcta limitada o expirada.|Sin dimensiones|
|outgoing.allpns.badorexpiredchannel|Errores de canal incorrecto o expirado|Count|Total|El recuento de inserciones que provocaron un error porque el canal/token/registrationId del registro ha expirado o no es válido.|Sin dimensiones|
|outgoing.wns.success|Notificaciones de WNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.wns.invalidcredentials|Errores de autorización de WNS (credenciales no válidas)|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas. (Windows Live no reconoce las credenciales).|Sin dimensiones|
|outgoing.wns.badchannel|Error de canal incorrecto de WNS|Count|Total|El recuento de inserciones que provocaron un error porque no se reconoció ChannelURI en el registro (estado de WNS: 404 No encontrado).|Sin dimensiones|
|outgoing.wns.expiredchannel|Error de canal expirado de WNS|Count|Total|El recuento de inserciones que provocaron un error porque ChannelURI expiró (estado de WNS: 410 Ya no existe).|Sin dimensiones|
|outgoing.wns.throttled|Notificaciones limitadas de WNS|Count|Total|El recuento de inserciones que provocaron un error porque WNS limita esta aplicación (estado de WNS: 406 No aceptable).|Sin dimensiones|
|outgoing.wns.tokenproviderunreachable|Errores de autorización de WNS (inaccesible)|Count|Total|No se puede acceder a Windows Live.|Sin dimensiones|
|outgoing.wns.invalidtoken|Errores de autorización de WNS (token no válido)|Count|Total|El token que se proporciona a WNS no es válido (estado de WNS: 401 No autorizado).|Sin dimensiones|
|outgoing.wns.wrongtoken|Errores de autorización de WNS (token erróneo)|Count|Total|El token que se proporciona a WNS es válido, pero para otra aplicación (estado de WNS: 403 Prohibido). Esto puede ocurrir si la instancia de ChannelURI en el registro está asociada a otra aplicación. Compruebe que la aplicación cliente está asociada a la misma aplicación cuyas credenciales están en el centro de notificaciones.|Sin dimensiones|
|outgoing.wns.invalidnotificationformat|Formato de notificación no válido de WNS|Count|Total|El formato de la notificación no es válido (estado de WNS: 400). Tenga en cuenta que WNS no rechaza todas las cargas útiles no válidas.|Sin dimensiones|
|outgoing.wns.invalidnotificationsize|Error de tamaño de notificación no válido de WNS|Count|Total|La carga útil de la notificación es demasiado grande (estado de WNS: 413).|Sin dimensiones|
|outgoing.wns.channelthrottled|Canal de WNS limitado|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (encabezado de respuesta WNS: X-WNS-NotificationStatus:channelThrottled).|Sin dimensiones|
|outgoing.wns.channeldisconnected|Canal de WNS desconectado|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (encabezado de respuesta WNS: X-WNS-DeviceConnectionStatus: desconectado).|Sin dimensiones|
|outgoing.wns.dropped|Notificaciones descartadas de WNS|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (X-WNS-NotificationStatus: descartado pero no X-WNS-DeviceConnectionStatus: desconectado).|Sin dimensiones|
|outgoing.wns.pnserror|Errores de WNS|Count|Total|La notificación no se entregó debido a errores de comunicación con WNS.|Sin dimensiones|
|outgoing.wns.authenticationerror|Errores de autenticación de WNS|Count|Total|La notificación no se entregó debido a errores de comunicación con Windows Live, credenciales no válidas o token erróneo.|Sin dimensiones|
|outgoing.apns.success|Notificaciones de APNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.apns.invalidcredentials|Errores de autorización de APNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.apns.badchannel|Error de canal incorrecto de APNS|Count|Total|Recuento de inserciones que han generado errores porque el token no es válido (código de estado de protocolo binario de APNS: 8. Código de estado del protocolo HTTP de APNS: 400 con "BadDeviceToken").|Sin dimensiones|
|outgoing.apns.expiredchannel|Error de canal expirado de APNS|Count|Total|El recuento de tokens invalidados por el canal de comentarios de APNS.|Sin dimensiones|
|outgoing.apns.invalidnotificationsize|Error de tamaño de notificación no válido de APNS|Count|Total|Recuento de inserciones que han generado errores porque la carga útil era demasiado grande (código de estado de protocolo binario de APNS: 7).|Sin dimensiones|
|outgoing.apns.pnserror|Errores de APNS|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con APNS.|Sin dimensiones|
|outgoing.gcm.success|Notificaciones de GCM correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.gcm.invalidcredentials|Errores de autorización de GCM (credenciales no válidas)|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.gcm.badchannel|Error de canal incorrecto de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro no se ha reconocido (resultado de GCM: registro no válido).|Sin dimensiones|
|outgoing.gcm.expiredchannel|Error de canal expirado de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro ha caducado (resultado de GCM: NotRegistered).|Sin dimensiones|
|outgoing.gcm.throttled|Notificaciones limitadas de GCM|Count|Total|El recuento de inserciones que provocaron un error porque GCM limitó esta aplicación (código de estado de GCM: 501-599 o resultado: no disponible).|Sin dimensiones|
|outgoing.gcm.invalidnotificationformat|Formato de notificación no válido de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil no tenía el formato correcto (resultado de GCM: InvalidDataKey o InvalidTtl).|Sin dimensiones|
|outgoing.gcm.invalidnotificationsize|Error de tamaño de notificación no válido de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil era demasiado grande (resultado de GCM: MessageTooBig).|Sin dimensiones|
|outgoing.gcm.wrongchannel|Error de canal incorrecto de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro no está asociado a la aplicación actual (resultado de GCM: InvalidPackageName).|Sin dimensiones|
|outgoing.gcm.pnserror|Errores de GCM|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con GCM.|Sin dimensiones|
|outgoing.gcm.authenticationerror|Errores de autenticación de GCM|Count|Total|El recuento de inserciones que provocaron un error debido a que el PNS no aceptó las credenciales proporcionadas, las credenciales están bloqueadas o el Id. del remitente no está configurado correctamente en la aplicación (resultado de GCM: MismatchedSenderId).|Sin dimensiones|
|outgoing.mpns.success|Notificaciones de MPNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.mpns.invalidcredentials|Credenciales no válidas de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.mpns.badchannel|Error de canal incorrecto de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque no se reconoció ChannelURI en el registro (estado de MPNS: 404 No encontrado).|Sin dimensiones|
|outgoing.mpns.throttled|Notificaciones limitadas de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque MPNS limita esta aplicación (MPNS de WNS: 406 No aceptable).|Sin dimensiones|
|outgoing.mpns.invalidnotificationformat|Formato de notificación no válido de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil de la notificación era demasiado grande.|Sin dimensiones|
|outgoing.mpns.channeldisconnected|Canal de MPNS desconectado|Count|Total|El recuento de inserciones que provocaron un error porque se desconectó la instancia ChannelURI en el registro (estado de MPNS: 412 No encontrado).|Sin dimensiones|
|outgoing.mpns.dropped|Notificaciones descartadas de MPNS|Count|Total|El recuento de inserciones que eliminó MPNS (encabezado de respuesta MPNS: X-NotificationStatus: QueueFull o Suprimido).|Sin dimensiones|
|outgoing.mpns.pnserror|Errores de MPNS|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con MPNS.|Sin dimensiones|
|outgoing.mpns.authenticationerror|Errores de autenticación de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|notificationhub.pushes|Todas las notificaciones salientes|Count|Total|Todas las notificaciones salientes del Centro de notificaciones|Sin dimensiones|
|incoming.all.requests|Todas las solicitudes entrantes|Count|Total|Solicitudes entrantes totales de un centro de notificaciones|Sin dimensiones|
|incoming.all.failedrequests|Todas las solicitudes entrantes con error|Count|Total|Solicitudes entrantes totales con error de un centro de notificaciones|Sin dimensiones|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Average_% Free Inodes|% de Inodes libres|Count|Media|Average_% Free Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|% de espacio libre|Count|Media|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Inodes|% de Inodes usados|Count|Media|Average_% Used Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Space|% espacio usado|Count|Media|Average_% Used Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Read Bytes/sec|Bytes de lectura de disco/s|Count|Media|Average_Disk Read Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Lecturas de disco/s|Count|Media|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Transferencias de disco/s|Count|Media|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Write Bytes/sec|Bytes de escritura en disco/s|Count|Media|Average_Disk Write Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Escrituras en disco/s|Count|Media|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Megabytes libres|Count|Media|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Logical Disk Bytes/sec|Bytes de disco lógico/s|Count|Media|Average_Logical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Memory|% de memoria disponible|Count|Media|Average_% Available Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Swap Space|% de espacio de intercambio disponible|Count|Media|Average_% Available Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Memory|% de memoria usada|Count|Media|Average_% Used Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Swap Space|% de espacio de intercambio usado|Count|Media|Average_% Used Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Memory|MB de memoria disponibles|Count|Media|Average_Available MBytes Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Swap|Intercambio de MB disponibles|Count|Media|Average_Available MBytes Swap|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Reads/sec|Lecturas de página/s|Count|Media|Average_Page Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Writes/sec|Escrituras de página/s|Count|Media|Average_Page Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pages/sec|Páginas/s|Count|Media|Average_Pages/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used MBytes Swap Space|Espacio de intercambio de MB usado|Count|Media|Average_Used MBytes Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory MBytes|MB de memoria usados|Count|Media|Average_Used Memory MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Transmitted|Número total de bytes transmitidos|Count|Media|Average_Total Bytes Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Received|Número total de bytes recibidos|Count|Media|Average_Total Bytes Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes|Número total de bytes|Count|Media|Average_Total Bytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Transmitted|Número total de paquetes transmitidos|Count|Media|Average_Total Packets Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Received|Número total de paquetes recibidos|Count|Media|Average_Total Packets Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx Errors|Errores de Rx totales|Count|Media|Average_Total Rx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Tx Errors|Errores de Tx totales|Count|Media|Average_Total Tx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Collisions|Colisiones totales|Count|Media|Average_Total Collisions|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/lecturas|Prom. Segundos de disco/lecturas|Count|Media|Average_Avg. Segundos de disco/lecturas|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/transferencias|Prom. Segundos de disco/transferencias|Count|Media|Average_Avg. Segundos de disco/transferencias|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/escrituras|Prom. Segundos de disco/escrituras|Count|Media|Average_Avg. Segundos de disco/escrituras|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Physical Disk Bytes/sec|Bytes de disco físico/s|Count|Media|Average_Physical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct Privileged Time|Porcentaje de tiempo con privilegios|Count|Media|Average_Pct Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct User Time|Porcentaje de tiempo de usuario|Count|Media|Average_Pct User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory kBytes|Kilobytes de memoria usados|Count|Media|Average_Used Memory kBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Virtual Shared Memory|Memoria compartida virtual|Count|Media|Average_Virtual Shared Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% DPC Time|% de tiempo de DPC|Count|Media|Average_% DPC Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Idle Time|% de tiempo de inactividad|Count|Media|Average_% Idle Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Interrupt Time|% de tiempo de interrupción|Count|Media|Average_% Interrupt Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% IO Wait Time|% de tiempo de espera de E/S|Count|Media|Average_% IO Wait Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Nice Time|% de tiempo bueno|Count|Media|Average_% Nice Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Privileged Time|% de tiempo con privilegios|Count|Media|Average_% Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|% de tiempo de procesador|Count|Media|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% User Time|% de tiempo de usuario|Count|Media|Average_% User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Physical Memory|Memoria física libre|Count|Media|Average_Free Physical Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Space in Paging Files|Espacio libre en archivos de paginación|Count|Media|Average_Free Space in Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Virtual Memory|Memoria virtual libre|Count|Media|Average_Free Virtual Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processes|Procesos|Count|Media|Average_Processes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Size Stored In Paging Files|Tamaño almacenado en archivos de paginación|Count|Media|Average_Size Stored In Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Uptime|Tiempo de actividad|Count|Media|Average_Uptime|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Users|Usuarios|Count|Media|Average_Users|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/lecturas|Prom. Segundos de disco/lecturas|Count|Media|Average_Avg. Segundos de disco/lecturas|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/escrituras|Prom. Segundos de disco/escrituras|Count|Media|Average_Avg. Segundos de disco/escrituras|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Current Disk Queue Length|Longitud actual de cola de disco|Count|Media|Average_Current Disk Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Lecturas de disco/s|Count|Media|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Transferencias de disco/s|Count|Media|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Escrituras en disco/s|Count|Media|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Megabytes libres|Count|Media|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|% de espacio libre|Count|Media|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes|MB disponibles|Count|Media|Average_Available MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Committed Bytes In Use|% de bytes confirmados en uso|Count|Media|Average_% Committed Bytes In Use|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Received/sec|Bytes recibidos por segundo|Count|Media|Average_Bytes Received/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Sent/sec|Bytes enviados por segundo|Count|Media|Average_Bytes Sent/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Total/sec|Bytes totales por segundo|Count|Media|Average_Bytes Total/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|% de tiempo de procesador|Count|Media|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processor Queue Length|Longitud de cola del procesador|Count|Media|Average_Processor Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Latido|Latido|Count|Total|Latido|Computer, OSType, Version, SourceComputerId|
|Actualizar|Actualizar|Count|Media|Actualizar|Computer, Product, Classification, UpdateState, Optional, Approved|
|Evento|Evento|Count|Media|Evento|Source, EventLog, Computer, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|QueryDuration|Duración de la consulta|Milisegundos|Media|Duración de la consulta DAX en el último intervalo|Sin dimensiones|
|QueryPoolJobQueueLength|Subprocesos: longitud de cola de trabajos del grupo de consultas|Count|Media|Número de trabajos en la cola del grupo de subprocesos de consultas.|Sin dimensiones|
|qpu_high_utilization_metric|Uso intensivo de QPU|Count|Total|Uso intensivo de QPU en el último minuto: 1 para un uso intensivo de QPU y 0 en caso contrario.|Sin dimensiones|
|memory_metric|Memoria|Bytes|Media|Memoria. Intervalo de 0-3 GB para A1, 0-5 GB para A2, 0-10 GB para A3, 0-25 GB para A4, 0-50 GB para A5 y 0-100 GB para A6|Sin dimensiones|
|memory_thrashing_metric|Paginación excesiva de memoria|Percent|Media|Paginación excesiva media de memoria.|Sin dimensiones|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Count|Total|Valor correcto de ListenerConnections de Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Count|Total|ClientError en ListenerConnections para Microsoft.Relay.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Count|Total|ServerError en ListenerConnections para Microsoft.Relay.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Count|Total|Valor correcto de SenderConnections para Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Count|Total|ClientError en SenderConnections para Microsoft.Relay.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Count|Total|ServerError en SenderConnections para Microsoft.Relay.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Count|Total|Valor total de ListenerConnections para Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Count|Total|Solicitudes totales de SenderConnections para Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Count|Total|Valor total de ActiveConnections para Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Count|Total|Valor total de ActiveListeners para Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Count|Total|Valor total de BytesTransferred para Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Count|Total|Valor total de ListenerDisconnects para Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Count|Total|Valor total de SenderDisconnects para Microsoft.Relay.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|SearchLatency|Latencia de búsqueda|Segundos|Media|Promedio de latencia de búsqueda para el servicio de búsqueda|Sin dimensiones|
|SearchQueriesPerSecond|Consultas de búsqueda por segundo|CountPerSecond|Media|Consultas de búsqueda por segundo para el servicio de búsqueda|Sin dimensiones|
|ThrottledSearchQueriesPercentage|Porcentaje de consultas de búsqueda limitadas|Percent|Media|Porcentaje de consultas de búsqueda limitadas para el servicio de búsqueda|Sin dimensiones|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitudes correctas (versión preliminar)|Count|Total|Número total de solicitudes correctas para un espacio de nombres (versión preliminar)|EntityName|
|ServerErrors|Errores del servidor. (versión preliminar)|Count|Total|Errores del servidor para Microsoft.ServiceBus. (versión preliminar)|EntityName|
|UserErrors|Errores de usuario. (versión preliminar)|Count|Total|Errores de usuario para Microsoft.ServiceBus. (versión preliminar)|EntityName|
|ThrottledRequests|Solicitudes limitadas. (versión preliminar)|Count|Total|Solicitudes limitadas para Microsoft.ServiceBus. (versión preliminar)|EntityName|
|IncomingRequests|Solicitudes entrantes (versión preliminar)|Count|Total|Solicitudes entrantes para Microsoft.ServiceBus. (versión preliminar)|EntityName|
|IncomingMessages|Mensajes entrantes (versión preliminar)|Count|Total|Mensajes entrantes para Microsoft.ServiceBus. (versión preliminar)|EntityName|
|OutgoingMessages|Mensajes salientes (versión preliminar)|Count|Total|Mensajes salientes para Microsoft.ServiceBus. (versión preliminar)|EntityName|
|ActiveConnections|ActiveConnections (versión preliminar)|Count|Total|Total de conexiones activas para Microsoft.ServiceBus. (versión preliminar)|Sin dimensiones|
|Size|Tamaño (vista previa)|Bytes|Media|Tamaño de cola/tema en bytes. (versión preliminar)|EntityName|
|error de Hadoop|Recuento de mensajes de una cola/tema. (versión preliminar)|Count|Media|Recuento de mensajes de una cola/tema. (versión preliminar)|EntityName|
|ActiveMessages|Recuento de mensajes activos de una cola/tema. (versión preliminar)|Count|Media|Recuento de mensajes activos de una cola/tema. (versión preliminar)|EntityName|
|DeadletteredMessages|Recuento de mensajes fallidos de una cola/tema. (versión preliminar)|Count|Media|Recuento de mensajes fallidos de una cola/tema. (versión preliminar)|EntityName|
|ScheduledMessages|Recuento de mensajes programados de una cola/tema. (versión preliminar)|Count|Media|Recuento de mensajes programados de una cola/tema. (versión preliminar)|EntityName|
|CPUXNS|Uso de CPU por espacio de nombres|Percent|Máxima|Métrica de uso de CPU de espacio de nombres premium de Service Bus|Sin dimensiones|
|WSXNS|Uso de tamaño de memoria por espacio de nombres|Percent|Máxima|Métrica de uso de memoria de espacio de nombres premium de Service Bus|Sin dimensiones|

## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/applications

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|AllocatedCpu|AllocatedCpu|Count|Media|CPU asignada a este contenedor en millares de núcleos|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|AllocatedMemory|AllocatedMemory|Bytes|Media|Memoria asignada a este contenedor en MB|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualCpu|ActualCpu|Count|Media|Uso de CPU real en millares de núcleos|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualMemory|ActualMemory|Bytes|Media|Uso de memoria real en MB|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|CpuUtilization|CpuUtilization|Percent|Media|Utilización de la CPU para este contenedor como porcentaje de AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|MemoryUtilization|MemoryUtilization|Percent|Media|Utilización de la CPU para este contenedor como porcentaje de AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ApplicationStatus|ApplicationStatus|Count|Media|Estado de una aplicación Service Fabric Mesh|ApplicationName, Status|
|ServiceStatus|ServiceStatus|Count|Media|Estado de mantenimiento de un servicio en una aplicación Service Fabric Mesh|ApplicationName, Status, ServiceName|
|ServiceReplicaStatus|ServiceReplicaStatus|Count|Media|Estado de mantenimiento de una réplica de servicio en una aplicación Service Fabric Mesh|ApplicationName, Status, ServiceName, ServiceReplicaName|
|ContainerStatus|ContainerStatus|Count|Media|Estado del contenedor en una aplicación Service Fabric Mesh|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName, Status|
|RestartCount|RestartCount|Count|Media|Número de reinicios de un contenedor en una aplicación Service Fabric Mesh|ApplicationName, Status, ServiceName, ServiceReplicaName, CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ConnectionCount|Número de conexiones|Count|Máxima|Cantidad de conexiones de usuario.|Punto de conexión|
|MessageCount|Número de mensajes|Count|Total|Cantidad total de mensajes.|Sin dimensiones|
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
|storage|Espacio de datos usado|Bytes|Máxima|Tamaño total de base de datos|Sin dimensiones|
|connection_successful|Conexiones correctas|Count|Total|Conexiones correctas|Sin dimensiones|
|connection_failed|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|blocked_by_firewall|Bloqueado por el firewall|Count|Total|Bloqueado por el firewall|Sin dimensiones|
|deadlock|Interbloqueos|Count|Total|Interbloqueos|Sin dimensiones|
|storage_percent|Porcentaje de espacio de datos usado|Percent|Máxima|Porcentaje de tamaño de base de datos|Sin dimensiones|
|xtp_storage_percent|Porcentaje de almacenamiento de OLTP en memoria|Percent|Media|Porcentaje de almacenamiento de OLTP en memoria|Sin dimensiones|
|workers_percent|Porcentaje de trabajos|Percent|Media|Porcentaje de trabajos|Sin dimensiones|
|sessions_percent|Porcentaje de sesiones|Percent|Media|Porcentaje de sesiones|Sin dimensiones|
|dtu_limit|Límite de DTU|Count|Media|Límite de DTU|Sin dimensiones|
|dtu_used|DTU utilizada|Count|Media|DTU utilizada|Sin dimensiones|
|cpu_limit|Límite de CPU|Count|Media|Límite de CPU|Sin dimensiones|
|cpu_used|CPU utilizada|Count|Media|CPU utilizada|Sin dimensiones|
|dwu_limit|Límite de DWU|Count|Máxima|Límite de DWU|Sin dimensiones|
|dwu_consumption_percent|Porcentaje de DWU|Percent|Máxima|Porcentaje de DWU|Sin dimensiones|
|dwu_used|DWU utilizada|Count|Máxima|DWU utilizada|Sin dimensiones|
|dw_cpu_percent|Porcentaje de CPU de nivel de nodo de almacenamiento de datos|Percent|Media|Porcentaje de CPU de nivel de nodo de almacenamiento de datos|DwLogicalNodeId|
|dw_physical_data_read_percent|Porcentaje de E/S de datos de nivel de nodo de almacenamiento de datos|Percent|Media|Porcentaje de E/S de datos de nivel de nodo de almacenamiento de datos|DwLogicalNodeId|
    |cache_hit_percent|Porcentaje de aciertos de caché|Percent|Máxima|Porcentaje de aciertos de caché|Sin dimensiones|
|cache_used_percent|Porcentaje de caché usada|Percent|Máxima|Porcentaje de caché usada|Sin dimensiones|
|local_tempdb_usage_percent|Porcentaje de tempdb local|Percent|Media|Porcentaje de tempdb local|Sin dimensiones|
|app_cpu_billed|CPU de aplicación facturada|Count|Total|CPU de aplicación facturada|Sin dimensiones|
|app_cpu_percent|Porcentaje de CPU de la aplicación|Percent|Media|Porcentaje de CPU de la aplicación|Sin dimensiones|
|app_memory_percent|Porcentaje utilizado de memoria de la aplicación|Percent|Media|Porcentaje utilizado de memoria de la aplicación|Sin dimensiones|
|allocated_data_storage|Espacio de datos asignado|Bytes|Media|Espacio de datos asignado|Sin dimensiones|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Sin dimensiones|
|physical_data_read_percent|Porcentaje de E/S de datos|Percent|Media|Porcentaje de E/S de datos|Sin dimensiones|
|log_write_percent|Porcentaje de E/S de registro|Percent|Media|Porcentaje de E/S de registro|Sin dimensiones|
|dtu_consumption_percent|Porcentaje de DTU|Percent|Media|Porcentaje de DTU|Sin dimensiones|
|storage_percent|Porcentaje de espacio de datos usado||Percent|Media|Porcentaje de almacenamiento|Sin dimensiones|
|workers_percent|Porcentaje de trabajos|Percent|Media|Porcentaje de trabajos|Sin dimensiones|
|sessions_percent|Porcentaje de sesiones|Percent|Media|Porcentaje de sesiones|Sin dimensiones|
|eDTU_limit|Límite de eDTU|Count|Media|Límite de eDTU|Sin dimensiones|
|storage_limit|Tamaño máximo de datos|Bytes|Media|Límite de almacenamiento|Sin dimensiones|
|eDTU_used|eDTU utilizada|Count|Media|eDTU utilizada|Sin dimensiones|
|storage_used|Espacio de datos usado|Bytes|Media|Almacenamiento utilizado|Sin dimensiones|
|xtp_storage_percent|Porcentaje de almacenamiento de OLTP en memoria|Percent|Media|Porcentaje de almacenamiento de OLTP en memoria|Sin dimensiones|
|cpu_limit|Límite de CPU|Count|Media|Límite de CPU|Sin dimensiones|
|cpu_used|CPU utilizada|Count|Media|CPU utilizada|Sin dimensiones|
|allocated_data_storage|Espacio de datos asignado|Bytes|Media|Espacio de datos asignado|Sin dimensiones|
|allocated_data_storage_percent|Porcentaje asignado de espacio de datos|Percent|Máxima|Porcentaje asignado de espacio de datos|Sin dimensiones|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|virtual_core_count|Recuento de núcleos virtuales|Count|Media|Recuento de núcleos virtuales|Sin dimensiones|
|avg_cpu_percent|Porcentaje de CPU medio|Percent|Media|Porcentaje de CPU medio|Sin dimensiones|
|reserved_storage_mb|Espacio de almacenamiento reservado|Count|Media|Espacio de almacenamiento reservado|Sin dimensiones|
|storage_space_used_mb|Espacio de almacenamiento usado|Count|Media|Espacio de almacenamiento usado|Sin dimensiones|
|io_requests|Recuento de solicitudes de E/S|Count|Media|Recuento de solicitudes de E/S|Sin dimensiones|
|io_bytes_read|Bytes de E/S leídos|Bytes|Media|Bytes de E/S leídos|Sin dimensiones|
|io_bytes_written|Bytes de E/S escritos|Bytes|Media|Bytes de E/S escritos|Sin dimensiones|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|UsedCapacity|Capacidad usada|Bytes|Media|Capacidad usada de la cuenta|Sin dimensiones|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BlobCapacity|Capacidad de Blob|Bytes|Media|La cantidad de almacenamiento que ha utilizado el Blob service de la cuenta de almacenamiento, en bytes.|BlobType, Tier|
|BlobCount|Recuento de blobs|Count|Total|El número de blobs en el Blob service de la cuenta de almacenamiento.|BlobType|       |BlobCount|Recuento de blobs|Count|Media|El número de blobs en el Blob service de la cuenta de almacenamiento.|BlobType, Tier|
|ContainerCount|Recuento de contenedores de blobs|Count|Media|El número de contenedores en el Blob service de la cuenta de almacenamiento.|Sin dimensiones|
|IndexCapacity|Capacidad de índice|Bytes|Media|Cantidad de almacenamiento que usa el índice (jerárquico) de ADLS Gen2 en bytes.|Sin dimensiones|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|FileCapacity|Capacidad de File|Bytes|Media|La cantidad de almacenamiento que ha utilizado el servicio File de la cuenta de almacenamiento, en bytes.|Sin dimensiones|
|FileCount|Recuento de archivos|Count|Media|El número de archivos en el servicio File de la cuenta de almacenamiento.|Sin dimensiones|
|FileShareCount|Recuento de recursos compartidos de archivos|Count|Media|El número de recursos compartidos de archivos en el servicio File de la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|QueueCapacity|Capacidad de Queue|Bytes|Media|La cantidad de almacenamiento que ha utilizado el Queue service de la cuenta de almacenamiento, en bytes.|Sin dimensiones|
|QueueCount|Recuento de colas|Count|Media|El número de colas en el Queue service de la cuenta de almacenamiento.|Sin dimensiones|
|QueueMessageCount|Recuento de mensajes de Queue|Count|Media|El número aproximado de mensajes de cola en el Queue service de la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|TableCapacity|Capacidad de Table|Bytes|Media|La cantidad de almacenamiento que ha utilizado el servicio Table de la cuenta de almacenamiento, en bytes.|Sin dimensiones|
|TableCount|Recuento de tablas|Count|Media|El número de tablas en el servicio Table de la cuenta de almacenamiento.|Sin dimensiones|
|TableEntityCount|Recuento de entidades de Table|Count|Media|El número de entidades de tabla en el servicio Table de la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|Salida|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latencia del servidor correcta|Milisegundos|Media|La latencia media utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latencia de E2E correcta|Milisegundos|Media|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|Disponibilidad|Disponibilidad|Percent|Media|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Resultado de la sesión de sincronización|Count|Media|Métrica que registra un valor de 1 cada vez que el punto de conexión del servidor completa correctamente una sesión de sincronización con el punto de conexión en la nube|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Archivos sincronizados|Count|Total|Número de archivos sincronizados|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Archivos que no se están sincronizando|Count|Total|No se pudo sincronizar el recuento de archivos|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Bytes sincronizados|Bytes|Total|Tamaño total del archivo transferido para las sesiones de sincronización|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncServerHeartbeat|Estado en línea del servidor|Count|Máxima|Métrica que registra un valor de 1 cada vez que el servidor registrado registra correctamente un latido con el punto de conexión en la nube|nombreDeServidor|
|StorageSyncRecallIOTotalSizeBytes|Recuperación de niveles de la nube|Bytes|Total|Tamaño total de datos recuperados por el servidor|nombreDeServidor|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|ResourceUtilization|SU % uso|Percent|Máxima|SU % uso|LogicalName, PartitionId|
|InputEvents|Eventos de entrada|Count|Total|Eventos de entrada|LogicalName, PartitionId|
|InputEventBytes|Bytes del evento de entrada|Bytes|Total|Bytes del evento de entrada|LogicalName, PartitionId|
|LateInputEvents|Eventos de entrada retrasada|Count|Total|Eventos de entrada retrasada|LogicalName, PartitionId|
|OutputEvents|Eventos de salida|Count|Total|Eventos de salida|LogicalName, PartitionId|
|ConversionErrors|Errores de conversión de datos|Count|Total|Errores de conversión de datos|LogicalName, PartitionId|
|Errors|Errores de tiempo de ejecución|Count|Total|Errores de tiempo de ejecución|LogicalName, PartitionId|
|DroppedOrAdjustedEvents|Eventos de fuera de orden|Count|Total|Eventos de fuera de orden|LogicalName, PartitionId|
|AMLCalloutRequests|Solicitudes de función|Count|Total|Solicitudes de función|LogicalName, PartitionId|
|AMLCalloutFailedRequests|Solicitudes de función con errores|Count|Total|Solicitudes de función con errores|LogicalName, PartitionId|
|AMLCalloutInputEvents|Eventos de función|Count|Total|Eventos de función|LogicalName, PartitionId|
|DeserializationError|Errores de deserialización de entrada|Count|Total|Errores de deserialización de entrada|LogicalName, PartitionId|
|EarlyInputEvents|Primeros eventos de entrada|Count|Total|Primeros eventos de entrada|LogicalName, PartitionId|
|OutputWatermarkDelaySeconds|Retraso de la marca de agua|Segundos|Máxima|Retraso de la marca de agua|LogicalName, PartitionId|
|InputEventsSourcesBacklogged|Eventos de entrada pendientes|Count|Máxima|Eventos de entrada pendientes|LogicalName, PartitionId|
|InputEventsSourcesPerSecond|Orígenes de entrada recibidos|Count|Total|Orígenes de entrada recibidos|LogicalName, PartitionId|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|IngressReceivedMessages|Mensajes de entrada recibidos|Count|Total|Número de mensajes leídos desde todos los orígenes de eventos de Event Hubs o IoT Hub|Sin dimensiones|
|IngressReceivedInvalidMessages|Mensajes no válidos de entrada recibidos|Count|Total|Número de mensajes no válidos leídos desde todos los orígenes de eventos de Event Hubs o IoT Hub|Sin dimensiones|
|IngressReceivedBytes|Bytes de entrada recibidos|Bytes|Total|Número de bytes leídos desde todos los orígenes de eventos|Sin dimensiones|
|IngressStoredBytes|Bytes de entrada almacenados|Bytes|Total|Tamaño total de los eventos almacenados correctamente y disponibles para su consulta|Sin dimensiones|
|IngressStoredEvents|Eventos de entrada almacenados|Count|Total|Número de eventos acoplados que se procesaron correctamente y que están disponibles para su consulta|Sin dimensiones|
|IngressReceivedMessagesTimeLag|Tiempo de retardo de los mensajes de entrada recibidos|Segundos|Máxima|Diferencia entre el momento en el que el mensaje se pone en la cola en el origen del evento y el momento en que se procesa en la entrada|Sin dimensiones|
|IngressReceivedMessagesCountLag|Intervalo de recuento de los mensajes de entrada recibidos|Count|Media|Diferencia entre el número de secuencia del último mensaje en cola en la partición del origen del evento y el del mensaje que se está procesando en la entrada|Sin dimensiones|
|WarmStorageMaxProperties|Propiedades máximas del almacenamiento intermedio|Count|Máxima|Número máximo de propiedades utilizadas que permite el entorno para la SKU S1/S2 y número máximo de propiedades admitido por el almacenamiento intermedio para la SKU de pago por uso|Sin dimensiones|
|WarmStorageUsedProperties|Propiedades utilizadas de almacenamiento intermedio |Count|Máxima|Número de propiedades utilizadas por el entorno para la SKU S1/S2 y número de propiedades que utiliza el almacenamiento intermedio para la SKU de pago por uso|Sin dimensiones|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|IngressReceivedMessages|Mensajes de entrada recibidos|Count|Total|Número de mensajes leídos desde el origen del evento|Sin dimensiones|
|IngressReceivedInvalidMessages|Mensajes no válidos de entrada recibidos|Count|Total|Número de mensajes leídos no válidos desde el origen del evento|Sin dimensiones|
|IngressReceivedBytes|Bytes de entrada recibidos|Bytes|Total|Número de bytes leídos desde el origen del evento|Sin dimensiones|
|IngressStoredBytes|Bytes de entrada almacenados|Bytes|Total|Tamaño total de los eventos almacenados correctamente y disponibles para su consulta|Sin dimensiones|
|IngressStoredEvents|Eventos de entrada almacenados|Count|Total|Número de eventos acoplados que se procesaron correctamente y que están disponibles para su consulta|Sin dimensiones|
|IngressReceivedMessagesTimeLag|Tiempo de retardo de los mensajes de entrada recibidos|Segundos|Máxima|Diferencia entre el momento en el que el mensaje se pone en la cola en el origen del evento y el momento en que se procesa en la entrada|Sin dimensiones|
|IngressReceivedMessagesCountLag|Intervalo de recuento de los mensajes de entrada recibidos|Count|Media|Diferencia entre el número de secuencia del último mensaje en cola en la partición del origen del evento y el del mensaje que se está procesando en la entrada|Sin dimensiones|
|WarmStorageMaxProperties|Propiedades máximas del almacenamiento intermedio|Count|Máxima|Número máximo de propiedades utilizadas que permite el entorno para la SKU S1/S2 y número máximo de propiedades admitido por el almacenamiento intermedio para la SKU de pago por uso|Sin dimensiones|
|WarmStorageUsedProperties|Propiedades utilizadas de almacenamiento intermedio |Count|Máxima|Número de propiedades utilizadas por el entorno para la SKU S1/S2 y número de propiedades que utiliza el almacenamiento intermedio para la SKU de pago por uso|Sin dimensiones|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk Read Bytes/Sec|BytesPerSecond|Media|Rendimiento de disco promedio debido a las operaciones de lectura durante el período de ejemplo.|Sin dimensiones|
|DiskWriteBytesPerSecond|Disk Write Bytes/Sec|BytesPerSecond|Media|Rendimiento de disco promedio debido a las operaciones de escritura durante el período de ejemplo.|Sin dimensiones|
|Bytes de lectura de disco|Bytes de lectura de disco|Bytes|Total|Rendimiento de disco total debido a las operaciones de lectura durante el período de ejemplo.|Sin dimensiones|
|Bytes de escritura de disco|Bytes de escritura de disco|Bytes|Total|Rendimiento de disco total debido a las operaciones de escritura durante el período de ejemplo.|Sin dimensiones|
|DiskReadOperations|Operaciones de lectura en disco|Count|Total|El número de operaciones de lectura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|Sin dimensiones|
|DiskWriteOperations|Operaciones de escritura en disco|Count|Total|El número de operaciones de escritura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Operaciones de lectura de disco por segundo|CountPerSecond|Media|El número promedio de operaciones de lectura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Operaciones de escritura por segundo en disco|CountPerSecond|Media|El número promedio de operaciones de escritura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|Sin dimensiones|
|DiskReadLatency|Latencia de lectura en disco|Milisegundos|Media|Latencia de lectura total. La suma de las latencias de lectura del kernel y el dispositivo.|Sin dimensiones|
|DiskWriteLatency|Latencia de escritura en disco|Milisegundos|Media|Latencia de escritura total. La suma de las latencias de escritura del kernel y el dispositivo.|Sin dimensiones|
|NetworkInBytesPerSecond|Red en bytes/seg|BytesPerSecond|Media|Rendimiento de red promedio para el tráfico recibido.|Sin dimensiones|
|NetworkOutBytesPerSecond|Bytes/seg de red externa|BytesPerSecond|Media|Rendimiento de red promedio para el tráfico transmitido.|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|Rendimiento de red total para el tráfico recibido.|Sin dimensiones|
|Red interna|Red interna|Bytes|Total|Rendimiento de red total para el tráfico transmitido.|Sin dimensiones|
|MemoryUsed|Memoria usada|Bytes|Media|La cantidad de memoria de la máquina que está usando la máquina virtual.|Sin dimensiones|
|MemoryGranted|Memoria concedida|Bytes|Media|La cantidad de memoria que concedió el host a la máquina virtual. No se concede memoria al host hasta que se toque una vez, y la memoria concedida se puede intercambiar o aumentar en caso de que el kernel de la VM necesite la memoria.|Sin dimensiones|
|MemoryActive|Memoria activa|Bytes|Media|La cantidad de memoria utilizada por la máquina virtual en el último período de tiempo reducido. Esta es la cantidad de memoria "verdadera" que requiere la máquina virtual. La memoria adicional que no se utiliza se puede intercambiar o aumentar sin afectar al rendimiento del invitado.|Sin dimensiones|
|Porcentaje de CPU|Porcentaje de CPU|Percent|Media|El uso de la CPU. Este valor se notifica con el 100 %, que representa todos los núcleos de procesador del sistema. Por ejemplo, una máquina virtual bidireccional que use el 50 % de un sistema de cuatro núcleos estará usando por completo dos núcleos.|Sin dimensiones|
|PercentageCpuReady|Preparación de la CPU en porcentaje|Milisegundos|Total|El tiempo de preparación es el tiempo que se tuvo que esperar a que las CPU estuvieran disponibles en el último intervalo de actualización.|Sin dimensiones|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CpuPercentage|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Instance|
|MemoryPercentage|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Instance|
|DiskQueueLength|Longitud de la cola de disco|Count|Media|Longitud de la cola de disco|Instance|
|HttpQueueLength|Longitud de la cola HTTP|Count|Media|Longitud de la cola HTTP|Instance|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (se excluye Functions)

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CpuTime|Tiempo de CPU|Segundos|Total|Tiempo de CPU|Instance|
|Requests|Requests|Count|Total|Requests|Instance|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|
|Http101|Http 101|Count|Total|Http 101|Instance|
|Http2xx|Http 2xx|Count|Total|Http 2xx|Instance|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instance|
|Http401|Http 401|Count|Total|Http 401|Instance|
|Http403|Http 403|Count|Total|Http 403|Instance|
|Http404|Http 404|Count|Total|Http 404|Instance|
|Http406|Http 406|Count|Total|Http 406|Instance|
|Http4xx|Http 4xx|Count|Total|Http 4xx|Instance|
|Http5xx|Errores de servidor HTTP|Count|Total|Errores de servidor HTTP|Instance|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Media|Espacio de trabajo de memoria|Instance|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Media|Espacio de trabajo de memoria promedio|Instance|
|AverageResponseTime|Tiempo de respuesta promedio|Segundos|Media|Tiempo de respuesta promedio|Instance|
|AppConnections|Conexiones|Count|Media|Conexiones|Instance|
|Identificadores|Recuento de identificadores|Count|Media|Recuento de identificadores|Instance|
|Subprocesos|Número de subprocesos|Count|Media|Número de subprocesos|Instance|
|PrivateBytes|Bytes privados|Bytes|Media|Bytes privados|Instance|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instance|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instance|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instance|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instance|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instance|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instance|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Count|Media|Solicitudes en cola de la aplicación|Instance|
|CurrentAssemblies|Ensamblados actuales|Count|Media|Ensamblados actuales|Instance|
|TotalAppDomains|Dominios de aplicación totales|Count|Media|Dominios de aplicación totales|Instance|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Count|Media|Dominios de aplicación totales descargados|Instance|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Count|Total|Recolección de elementos no utilizados de Gen 0|Instance|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Count|Total|Recolección de elementos no utilizados de Gen 1|Instance|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Count|Total|Recolección de elementos no utilizados de Gen 2|Instance|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (Functions)

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|
|Http5xx|Errores de servidor HTTP|Count|Total|Errores de servidor HTTP|Instance|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Media|Espacio de trabajo de memoria|Instance|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Media|Espacio de trabajo de memoria promedio|Instance|
|FunctionExecutionUnits|Unidades de ejecución de función|MB/milisegundos|Total|[Unidades de ejecución de función](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Instance|
|FunctionExecutionCount|Recuento de ejecución de funciones|Count|Total|Recuento de ejecución de funciones|Instance|
|PrivateBytes|Bytes privados|Bytes|Media|Bytes privados|Instance|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instance|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instance|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instance|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instance|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instance|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instance|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Count|Media|Solicitudes en cola de la aplicación|Instance|
|CurrentAssemblies|Ensamblados actuales|Count|Media|Ensamblados actuales|Instance|
|TotalAppDomains|Dominios de aplicación totales|Count|Media|Dominios de aplicación totales|Instance|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Count|Media|Dominios de aplicación totales descargados|Instance|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Count|Total|Recolección de elementos no utilizados de Gen 0|Instance|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Count|Total|Recolección de elementos no utilizados de Gen 1|Instance|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Count|Total|Recolección de elementos no utilizados de Gen 2|Instance|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|CpuTime|Tiempo de CPU|Segundos|Total|Tiempo de CPU|Instance|
|Requests|Requests|Count|Total|Requests|Instance|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|
|Http101|Http 101|Count|Total|Http 101|Instance|
|Http2xx|Http 2xx|Count|Total|Http 2xx|Instance|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instance|
|Http401|Http 401|Count|Total|Http 401|Instance|
|Http403|Http 403|Count|Total|Http 403|Instance|
|Http404|Http 404|Count|Total|Http 404|Instance|
|Http406|Http 406|Count|Total|Http 406|Instance|
|Http4xx|Http 4xx|Count|Total|Http 4xx|Instance|
|Http5xx|Errores de servidor HTTP|Count|Total|Errores de servidor HTTP|Instance|
|MemoryWorkingSet|Espacio de trabajo de memoria|Bytes|Media|Espacio de trabajo de memoria|Instance|
|AverageMemoryWorkingSet|Espacio de trabajo de memoria promedio|Bytes|Media|Espacio de trabajo de memoria promedio|Instance|
|AverageResponseTime|Tiempo de respuesta promedio|Segundos|Media|Tiempo de respuesta promedio|Instance|
|FunctionExecutionUnits|Unidades de ejecución de función|Count|Total|Unidades de ejecución de función|Instance|
|FunctionExecutionCount|Recuento de ejecución de funciones|Count|Total|Recuento de ejecución de funciones|Instance|
|AppConnections|Conexiones|Count|Media|Conexiones|Instance|
|Identificadores|Recuento de identificadores|Count|Media|Recuento de identificadores|Instance|
|Subprocesos|Número de subprocesos|Count|Media|Número de subprocesos|Instance|
|PrivateBytes|Bytes privados|Bytes|Media|Bytes privados|Instance|
|IoReadBytesPerSecond|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Bytes de lectura de E/S por segundo|Instance|
|IoReadBytesPerSecond|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Bytes de escritura de E/S por segundo|Instance|
|IoOtherBytesPerSecond|Otros bytes de E/S por segundo|BytesPerSecond|Total|Otros bytes de E/S por segundo|Instance|
|IoReadOperationsPerSecond|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Operaciones de lectura de E/S por segundo|Instance|
|IoWriteOperationsPerSecond|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Operaciones de escritura de E/S por segundo|Instance|
|IoOtherOperationsPerSecond|Otras operaciones de E/S por segundo|BytesPerSecond|Total|Otras operaciones de E/S por segundo|Instance|
|RequestsInApplicationQueue|Solicitudes en cola de la aplicación|Count|Media|Solicitudes en cola de la aplicación|Instance|
|CurrentAssemblies|Ensamblados actuales|Count|Media|Ensamblados actuales|Instance|
|TotalAppDomains|Dominios de aplicación totales|Count|Media|Dominios de aplicación totales|Instance|
|TotalAppDomainsUnloaded|Dominios de aplicación totales descargados|Count|Media|Dominios de aplicación totales descargados|Instance|
|Gen0Collections|Recolección de elementos no utilizados de Gen 0|Count|Total|Recolección de elementos no utilizados de Gen 0|Instance|
|Gen1Collections|Recolección de elementos no utilizados de Gen 1|Count|Total|Recolección de elementos no utilizados de Gen 1|Instance|
|Gen2Collections|Recolección de elementos no utilizados de Gen 2|Count|Total|Recolección de elementos no utilizados de Gen 2|Instance|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|Requests|Requests|Count|Total|Requests|Instance|
|BytesReceived|Entrada de datos|Bytes|Total|Entrada de datos|Instance|
|BytesSent|Salida de datos|Bytes|Total|Salida de datos|Instance|
|Http101|Http 101|Count|Total|Http 101|Instance|
|Http2xx|Http 2xx|Count|Total|Http 2xx|Instance|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instance|
|Http401|Http 401|Count|Total|Http 401|Instance|
|Http403|Http 403|Count|Total|Http 403|Instance|
|Http404|Http 404|Count|Total|Http 404|Instance|
|Http406|Http 406|Count|Total|Http 406|Instance|
|Http4xx|Http 4xx|Count|Total|Http 4xx|Instance|
|Http5xx|Errores de servidor HTTP|Count|Total|Errores de servidor HTTP|Instance|
|AverageResponseTime|Tiempo de respuesta promedio|Segundos|Media|Tiempo de respuesta promedio|Instance|
|CpuPercentage|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Instance|
|MemoryPercentage|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Instance|
|DiskQueueLength|Longitud de la cola de disco|Count|Media|Longitud de la cola de disco|Instance|
|HttpQueueLength|Longitud de la cola HTTP|Count|Media|Longitud de la cola HTTP|Instance|
|ActiveRequests|Solicitudes activas|Count|Total|Solicitudes activas|Instance|
|TotalFrontEnds|Número total de front-ends|Count|Media|Número total de front-ends|Sin dimensiones|
|SmallAppServicePlanInstances|Trabajos pequeños del plan de App Service|Count|Media|Trabajos pequeños del plan de App Service|Sin dimensiones|
|MediumAppServicePlanInstances|Trabajos medianos del plan de App Service|Count|Media|Trabajos medianos del plan de App Service|Sin dimensiones|
|LargeAppServicePlanInstances|Trabajos grandes del plan de App Service|Count|Media|Trabajos grandes del plan de App Service|Sin dimensiones|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Métrica|Nombre de métrica para mostrar|Unidad|Tipo de agregación|DESCRIPCIÓN|Dimensiones|
|---|---|---|---|---|---|
|WorkersTotal|Número total de trabajos|Count|Media|Número total de trabajos|Sin dimensiones|
|WorkersAvailable|Trabajos disponibles|Count|Media|Trabajos disponibles|Sin dimensiones|
|WorkersUsed|Trabajos usados|Count|Media|Trabajos usados|Sin dimensiones|
|CpuPercentage|Porcentaje de CPU|Percent|Media|Porcentaje de CPU|Instance|
|MemoryPercentage|Porcentaje de memoria|Percent|Media|Porcentaje de memoria|Instance|

## <a name="next-steps"></a>Pasos siguientes
* [Lea información sobre las métricas en Azure Monitor](data-platform.md)
* [Creación de alertas basadas en métricas](alerts-overview.md)
* [Exportación de métricas a cuentas de almacenamiento, Event Hubs o Log Analytics](diagnostic-logs-overview.md)
