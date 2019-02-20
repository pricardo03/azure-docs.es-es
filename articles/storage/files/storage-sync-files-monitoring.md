---
title: Supervisión de Azure File Sync | Microsoft Docs
description: Cómo supervisar Azure File Sync.
services: storage
author: jeffpatt24
ms.service: storage
ms.topic: article
ms.date: 01/31/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 5a0d02768b0fbd23e33d13c5e5c3fe84a41cdc52
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243661"
---
# <a name="monitor-azure-file-sync"></a>Supervisión de Azure File Sync

Use Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de los recursos compartidos de archivos de Azure. Puede usar cualquier protocolo disponible en Windows Server para acceder a sus datos localmente, como SMB, NFS y FTPS. Puede tener todas las cachés que necesite en todo el mundo.

En este artículo se describe cómo supervisar la implementación de Azure File Sync mediante Azure Portal y Windows Server.

Las siguientes opciones de supervisión están disponibles actualmente:

## <a name="azure-portal"></a>Azure Portal

En Azure Portal, puede ver el estado del servidor registrado, el estado del punto de conexión del servidor (estado de sincronización) y las métricas.

### <a name="storage-sync-service"></a>Servicio de sincronización de almacenamiento

Para ver el estado del servidor registrado y del punto de conexión del servidor, así como las métricas, vaya al servicio de sincronización de almacenamiento en Azure Portal. El estado del servidor registrado se puede ver en la hoja Servidores registrados. El estado de punto de conexión del servidor se puede ver en la hoja Grupos de sincronización.

Estado del servidor registrado
- Si el estado de Servidor registrado es En línea, el servidor se está comunicando correctamente con el servicio.
- Si el estado de Servidor registrado es Aparece sin conexión, verifique que el proceso del supervisor de sincronización de almacenamiento (AzureStorageSyncMonitor.exe) en el servidor se esté ejecutando. Si el servidor está detrás de un firewall o proxy, configure el firewall y proxy según la [documentación](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy).

Estado del punto de conexión del servidor
- El estado de punto de conexión del servidor en el portal se basa en los eventos de sincronización que se registran en el registro de eventos de telemetría en el servidor (Id. 9102 y 9302). Si se produce un error en una sesión de sincronización debido a un error transitorio (por ejemplo, error cancelado), puede que la sincronización siga mostrando un estado correcto en el portal, siempre y cuando la sesión de sincronización actual esté progresando (el id. de evento 9302 se usa para determinar si se están aplicando los archivos). Para más información, consulte la siguiente documentación: [Estado de sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [Progreso de sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Si el portal muestra un error de sincronización debido a que la sincronización no avanza, compruebe la [documentación de solución de problemas](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) para obtener instrucciones.

Métricas
- Las siguientes métricas están visibles en el portal del servicio de sincronización de almacenamiento:

  | Nombre de métrica | DESCRIPCIÓN | Hojas del portal | 
  |-|-|-|
  | Bytes sincronizados | Tamaño de los datos transferidos (carga y descarga) | Grupo de sincronización,punto de conexión de servidor |
  | Recuperación de niveles de la nube | Tamaño de los datos que se recuperan | Servidores registrados |
  | Archivos que no se están sincronizando | Recuento de archivos que no se pueden sincronizar | Punto de conexión de servidor |
  | Archivos sincronizados | Recuento de los ficheros transferidos (carga y descarga) | Grupo de sincronización,punto de conexión de servidor |
  | Estado en línea del servidor | Recuento de latidos recibido del servidor | Servidores registrados |

- Para obtener más información, consulte la sección [Azure Monitor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor). 

  > [!Note]  
  > Los gráficos del portal del servicio de sincronización de almacenamiento tienen un intervalo de tiempo de 24 horas. Para ver los diferentes intervalos de tiempo o dimensiones, use Azure Monitor.


### <a name="azure-monitor"></a>Azure Monitor

Use [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) para supervisar la sincronización, la nube por niveles y la conectividad del servidor. Las métricas de Azure File Sync están habilitadas de forma predeterminada y se envían a Azure Monitor cada 15 minutos.

Para ver las métricas de Azure File Sync en Azure Monitor, seleccione el tipo de recurso de Servicios de sincronización de almacenamiento.

Las siguientes métricas para Azure File Sync están disponibles en Azure Monitor:

| Nombre de métrica | DESCRIPCIÓN |
|-|-|
| Bytes sincronizados | Tamaño de los datos transferidos (carga y descarga).<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Recuperación de niveles de la nube | Tamaño de los datos que se recuperan.<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensión aplicable: Nombre del servidor |
| Archivos que no se están sincronizando | Recuento de archivos que no se pueden sincronizar.<br><br>Unidad: Recuento<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Archivos sincronizados | Recuento de los ficheros transferidos (carga y descarga).<br><br>Unidad: Recuento<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Estado en línea del servidor | Recuento de latidos recibido del servidor.<br><br>Unidad: Recuento<br>Tipo de agregación: Máxima<br>Dimensión aplicable: Nombre del servidor |
| Sync session result (Resultado de la sesión de sincronización) | Resultado de la sesión de sincronización (1 = sesión de sincronización correcta; 0 = sesión de sincronización con errores)<br><br>Unidad: Recuento<br>Tipos de agregación: Máxima<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |

## <a name="windows-server"></a>Windows Server

En Windows Server, puede ver la nube por niveles, el servidor registrado y el estado de sincronización.

### <a name="event-logs"></a>Registros de eventos

Usar el registro Eventos de telemetría en el servidor para supervisar el servidor registrado, la sincronización y el estado de la nube por niveles. El registro Eventos de telemetría se encuentra en Applications and Services\Microsoft\FileSync\Agent en el Visor de eventos.

Estado de sincronización
- El id. de evento 9102 se registra una vez finalizada una sesión de sincronización. Este evento se debe usar para determinar si las sesiones de sincronización se completan correctamente (HResult = 0) y si hay errores de sincronización por elemento. Para más información, consulte la siguiente documentación: [Estado de sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [Errores por elemento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

  > [!Note]  
  > A veces, las sesiones de sincronización pueden producir un error general o tienen un valor de PerItemErrorCount distinto de cero, pero aún así progresan y algunos archivos se sincronizan correctamente. Esto puede verse en los campos Applied (AppliedFileCount, AppliedDirCount, AppliedTombstoneCount y AppliedSizeBytes), que indican qué parte de la sesión finalizará correctamente. Si ve varias sesiones de sincronización seguidas que tienen un error pero que tienen un número cada vez mayor de campos Applied, debe dar tiempo para que se vuelva a intentar la sincronización antes de abrir una incidencia de soporte técnico.

- El id. de evento 9302 se registra cada 5 a 10 minutos si hay una sesión de sincronización activa. Este evento se debe usar para determinar si la sesión de sincronización actual está en progreso (AppliedItemCount > 0). Si la sincronización no avanza, la sesión de sincronización finalmente generará un error y se registrará el id. de evento 9102 con el error. Para más información, consulte la siguiente documentación: [Progreso de sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session)

Estado del servidor registrado
- El id. de evento 9301 se registra cada 30 segundos, cuando un servidor consulta al servicio por los trabajos. Si GetNextJob se completa con estado = 0, el servidor puede comunicarse con el servicio. Si GetNextJob se completa con un error, compruebe la [documentación de solución de problemas](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) para obtener instrucciones.

Estado de la nube por niveles
- Para supervisar la actividad en niveles en un servidor, use el los id. de evento 9003, 9016 y 9029 en el registro Eventos de telemetría (ubicado en Applications and Services\Microsoft\FileSync\Agent en el Visor de eventos).

  - El identificador de evento 9003 proporciona una distribución de errores para un punto de conexión de servidor. Por ejemplo, el recuento total de errores, el código de error, entre otros. Tenga en cuenta que se registra un evento por código de error.
  - El identificador de evento 9016 proporciona resultados de conversión en fantasma para un volumen. Por ejemplo, el porcentaje de espacio libre, el número de archivos convertidos en fantasma en la sesión, el número de archivos no convertidos en fantasma, etc.
  - El id. de evento 9029 proporciona información de la sesión de conversión en fantasma para el punto de conexión de un servidor. Por ejemplo, el número de archivos que se han intentado en la sesión, el número de archivos organizados en niveles de la sesión, el número de archivos ya organizados en niveles, entre otros.
  
- Para supervisar la actividad de recuperación en un servidor, use los id. de evento 9005, 9006, 9009 y 9059 en el registro Eventos de telemetría (ubicado en Applications and Services\Microsoft\FileSync\Agent en el Visor de eventos).

  - El identificador de evento 9005 proporciona confiabilidad de recuperación a un punto de conexión de servidor. Por ejemplo, el total de archivos únicos a los que se puede acceder, el total de archivos único con acceso erróneo, entre otros.
  - El identificador de evento 9006 proporciona una distribución de errores de recuperación a un punto de conexión de servidor. Por ejemplo, el total de solicitudes erróneas o el código de error. Tenga en cuenta que se registra un evento por código de error.
  - El id. de evento 9009 proporciona información de la sesión de recuperación para un punto de conexión de servidor. Por ejemplo, DurationSeconds, CountFilesRecallSucceeded, CountFilesRecallFailed, etc.
  - El id. de evento 9059 proporciona una distribución de recuperación de aplicación para un punto de conexión de servidor. Por ejemplo, ShareId, Application Name y TotalEgressNetworkBytes.

### <a name="performance-counters"></a>contadores de rendimiento

Use los contadores de rendimiento de Azure File Sync en el servidor para supervisar la actividad de sincronización.

Para ver los contadores de rendimiento de Azure File Sync en el servidor, inicie el Monitor de rendimiento (Perfmon.exe), los contadores se pueden encontrar en los objetos AFS Bytes Transferred (Bytes transferidos de AFS) y AFS Sync Operations (Operaciones de sincronización de AFS).

Los siguientes contadores de rendimiento para Azure File Sync están disponibles en el Monitor de rendimiento:

| Objeto de rendimiento\Nombre del contador | DESCRIPCIÓN |
|-|-|
| AFS Bytes Transferred\Downloaded Bytes/sec (Bytes descargados/s) | Número de bytes descargados por segundo. |
| AFS Bytes Transferred\Uploaded Bytes/sec (Bytes cargados/s) | Número de bytes cargados por segundo. |
| AFS Bytes Transferred\Total Bytes/sec (Total de bytes/s) | Número total de bytes por segundo (carga y descarga). |
| AFS Sync Operations\Downloaded Sync Files/sec (Archivos de sincronización descargados/s) | Número de archivos descargados por segundo. |
| AFS Sync Operations\Uploaded Sync Files/sec (Archivos de sincronización cargados/s) | Número de archivos cargados por segundo. |
| AFS Sync Operations\Total Sync File Operations/sec (Total de operaciones de sincronización de archivos/s) | Número total de archivos sincronizados (carga y descarga). |

## <a name="next-steps"></a>Pasos siguientes
- [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
- [Tenga en cuenta los valores de proxy y firewall](storage-sync-files-firewall-and-proxy.md)
- [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
- [Solución de problemas de Azure File Sync](storage-sync-files-troubleshoot.md)
- [Preguntas más frecuentes de Azure Files](storage-files-faq.md)
