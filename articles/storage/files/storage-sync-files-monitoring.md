---
title: Supervisión de Azure File Sync | Microsoft Docs
description: Cómo supervisar Azure File Sync.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 01/31/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: c0f19e3ea4f5952ac96b589fa267a2136c85e4f3
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63759473"
---
# <a name="monitor-azure-file-sync"></a>Supervisión de Azure File Sync

Use Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de los recursos compartidos de archivos de Azure. Puede usar cualquier protocolo disponible en Windows Server para acceder a sus datos localmente, como SMB, NFS y FTPS. Puede tener todas las cachés que necesite en todo el mundo.

En este artículo se describe cómo supervisar la implementación de Azure File Sync mediante el portal de Azure y Windows Server.

Las siguientes opciones de supervisión están disponibles actualmente.

## <a name="azure-portal"></a>Azure Portal

En Azure Portal, puede ver el estado del servidor registrado, el estado del punto de conexión del servidor (estado de sincronización) y las métricas.

### <a name="storage-sync-service"></a>Servicio de sincronización de almacenamiento

Para ver el estado del servidor registrado, el estado del punto de conexión de servidor y las métricas, vaya al servicio de sincronización de almacenamiento en el portal de Azure. Se puede ver el estado de servidor registrado en el **servidores registrados** hoja y el servidor de estado de punto de conexión en el **grupos de sincronización** hoja.

Estado del servidor registrado:

- Si el **servidor registrado** estado es **Online**, el servidor se está comunicando correctamente con el servicio.
- Si el **servidor registrado** estado es **aparece sin conexión**, compruebe que se está ejecutando el proceso del Monitor de sincronización de almacenamiento (AzureStorageSyncMonitor.exe) en el servidor. Si el servidor está detrás de un firewall o proxy, consulte [en este artículo](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy) para configurar el firewall y proxy.

Estado de punto de conexión del servidor:

- El estado de punto de conexión del servidor en el portal se basa en los eventos de sincronización que se registran en el registro de eventos de telemetría en el servidor (Id. 9102 y 9302). Si una sesión de sincronización se produce un error debido a un error transitorio, como error cancelado, podría seguir apareciendo en buen estado en el portal de sincronización siempre y cuando la sesión de sincronización actual se observa algún progreso. Id. de evento 9302 se utiliza para determinar si se están aplicando los archivos. Para obtener más información, consulte [estado de sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) y [progreso de sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Si el portal muestra un error de sincronización porque la sincronización no está progresando, consulte el [solución de problemas de documentación](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) para obtener instrucciones.

Métricas:

- Las siguientes métricas están visibles en el portal del servicio de sincronización de almacenamiento:

  | Nombre de métrica | DESCRIPCIÓN | Nombre de hoja |
  |-|-|-|
  | Bytes sincronizados | Tamaño de los datos transferidos (carga y descarga) | Grupo de sincronización,punto de conexión de servidor |
  | Recuperación de niveles de la nube | Tamaño de los datos que se recuperan | Servidores registrados |
  | Archivos que no se están sincronizando | Recuento de archivos que no se pueden sincronizar | Punto de conexión de servidor |
  | Archivos sincronizados | Recuento de los ficheros transferidos (carga y descarga) | Grupo de sincronización,punto de conexión de servidor |
  | Estado en línea del servidor | Recuento de latidos recibido del servidor | Servidores registrados |

- Para obtener más información, consulte [Azure Monitor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor).

  > [!Note]  
  > Los gráficos del portal del servicio de sincronización de almacenamiento tienen un intervalo de tiempo de 24 horas. Para ver los diferentes intervalos de tiempo o dimensiones, use Azure Monitor.

### <a name="azure-monitor"></a>Azure Monitor

Use [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) para supervisar la sincronización, la nube por niveles y la conectividad del servidor. Las métricas de Azure File Sync están habilitadas de forma predeterminada y se envían a Azure Monitor cada 15 minutos.

Para ver las métricas de Azure File Sync en Azure Monitor, seleccione el **servicios de sincronización de almacenamiento** tipo de recurso.

Las siguientes métricas para Azure File Sync están disponibles en Azure Monitor:

| Nombre de métrica | DESCRIPCIÓN |
|-|-|
| Bytes sincronizados | Tamaño de los datos transferidos (carga y descarga).<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Recuperación de niveles de la nube | Tamaño de los datos que se recuperan.<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensión aplicable: Nombre del servidor |
| Archivos que no se están sincronizando | Recuento de archivos que no se pueden sincronizar.<br><br>Unidad: Número<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Archivos sincronizados | Recuento de los ficheros transferidos (carga y descarga).<br><br>Unidad: Número<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Estado en línea del servidor | Recuento de latidos recibido del servidor.<br><br>Unidad: Número<br>Tipo de agregación: Máxima<br>Dimensión aplicable: Nombre del servidor |
| Sync session result (Resultado de la sesión de sincronización) | Resultado de la sesión de sincronización (1 = sesión de sincronización correcta; 0 = sesión de sincronización con errores)<br><br>Unidad: Número<br>Tipos de agregación: Máxima<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |

## <a name="windows-server"></a>Windows Server

En Windows Server, puede ver los niveles de nube, servidor registrado y estado de sincronización.

### <a name="event-logs"></a>Registros de eventos

Usar el registro Eventos de telemetría en el servidor para supervisar el servidor registrado, la sincronización y el estado de la nube por niveles. El registro de eventos de telemetría se encuentra en el Visor de eventos en *Applications and Services\Microsoft\FileSync\Agent*.

Estado de sincronización:

- Id. de evento 9102 se registra una vez finalizada una sesión de sincronización. Use este evento para determinar si las sesiones de sincronización se realizó correctamente (**HResult = 0**) y si hay elementos de los errores de sincronización. Para obtener más información, consulte el [estado de sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) y [errores por elemento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing) documentación.

  > [!Note]  
  > A veces, las sesiones de sincronización se producirá un error general o tienen un PerItemErrorCount distinto de cero. Sin embargo, todavía hacen progresos, y algunos archivos de sincronización se realice correctamente. Puede ver esto en los campos como AppliedFileCount, AppliedDirCount, AppliedTombstoneCount y AppliedSizeBytes aplicados. Estos campos indican qué parte de la sesión se realizó correctamente. Si ve varias sesiones de sincronización producirá un error en una fila, y que tienen un recuento creciente aplicado, asigne a la hora de sincronización para intentarlo de nuevo antes de abrir una incidencia de soporte técnico.

- El id. de evento 9302 se registra cada 5 a 10 minutos si hay una sesión de sincronización activa. Use este evento para determinar si la sesión de sincronización actual está realizando un progreso (**AppliedItemCount > 0**). Si la sincronización no avanza, la sesión de sincronización debe producir un error y un 9102 de Id. de evento se registrará con el error. Para obtener más información, consulte el [documentación del progreso de sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).

Estado del servidor registrado:

- El id. de evento 9301 se registra cada 30 segundos, cuando un servidor consulta al servicio por los trabajos. Si finaliza GetNextJob con **estado = 0**, el servidor es capaz de comunicarse con el servicio. Si GetNextJob finaliza con un error, compruebe el [solución de problemas de documentación](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) para obtener instrucciones.

Estado de los niveles de nube:

- Para supervisar la actividad de organización en niveles en un servidor, use el Id. de evento 9003, 9016 y 9029 en el registro de eventos de telemetría, que se encuentra en el Visor de eventos en *Applications and Services\Microsoft\FileSync\Agent*.

  - El identificador de evento 9003 proporciona una distribución de errores para un punto de conexión de servidor. Por ejemplo:  Recuento total de errores y el código de error. Un evento se registra cada código de error.
  - El identificador de evento 9016 proporciona resultados de conversión en fantasma para un volumen. Por ejemplo:  Espacio libre por ciento, número de archivos fantasma en la sesión, y número de archivos no se pudo fantasma.
  - El id. de evento 9029 proporciona información de la sesión de conversión en fantasma para el punto de conexión de un servidor. Por ejemplo:  Número de archivos que se ha intentado en la sesión, el número de archivos en capas en la sesión y el número de archivos en capas ya.
  
- Para supervisar la actividad de recuperación en un servidor, use el Id. de evento 9005, 9006, 9009 y 9059 en el registro de eventos de telemetría, que se encuentra en el Visor de eventos en *Applications and Services\Microsoft\FileSync\Agent*.

  - El identificador de evento 9005 proporciona confiabilidad de recuperación a un punto de conexión de servidor. Por ejemplo:  Total de archivos únicos que ha tenido acceso y Total de archivos únicos con acceso con error.
  - El identificador de evento 9006 proporciona una distribución de errores de recuperación a un punto de conexión de servidor. Por ejemplo:  Total de solicitudes con error y el código de error. Un evento se registra cada código de error.
  - El id. de evento 9009 proporciona información de la sesión de recuperación para un punto de conexión de servidor. Por ejemplo:  DurationSeconds, CountFilesRecallSucceeded y CountFilesRecallFailed.
  - El id. de evento 9059 proporciona una distribución de recuperación de aplicación para un punto de conexión de servidor. Por ejemplo:  ShareId, nombre de la aplicación y TotalEgressNetworkBytes.

### <a name="performance-counters"></a>contadores de rendimiento

Use los contadores de rendimiento de Azure File Sync en el servidor para supervisar la actividad de sincronización.

Para ver los contadores de rendimiento de Azure File Sync en el servidor, abra el Monitor de rendimiento (Perfmon.exe). Puede encontrar los contadores en el **AFS Bytes transferidos** y **las operaciones de sincronización de AFS** objetos.

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
