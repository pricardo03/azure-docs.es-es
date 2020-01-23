---
title: Descripción de las métricas de Azure Spring Cloud
description: Aprenda a revisar las métricas en Azure Spring Cloud
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: brendm
ms.openlocfilehash: e6517f1a7374b3960c3b749e63a90fe9eb21e7b0
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2020
ms.locfileid: "76277936"
---
# <a name="understand-metrics-for-azure-spring-cloud"></a>Descripción de las métricas en Azure Spring Cloud

El explorador de métricas de Azure es un componente de Microsoft Azure Portal que permite trazar gráficos, correlacionar tendencias visualmente e investigar máximos y mínimos en las métricas. Utilice el Explorador de métricas para investigar el estado y la utilización de recursos. 

En Azure Spring Cloud, hay dos puntos de vista para las métricas.
* Gráficos en todas las páginas de información general de aplicaciones
* Página de métricas comunes

 ![Gráficos de métricas](media/metrics/metrics-1.png)

Los gráficos de la **Información general** de la aplicación proporcionan comprobaciones de estado rápidas para cada aplicación. La página de **Métricas** comunes contiene todas las métricas disponibles como referencia. Puede crear sus propios gráficos en la página de métricas comunes y anclarlos en el Panel.

## <a name="application-overview-page"></a>Página de información general de la aplicación
Seleccione una aplicación en **Administración de aplicaciones** para buscar gráficos en la página de información general.  

 ![Administración de métricas de aplicación](media/metrics/metrics-2.png)

La página **Información general de aplicación** de cada aplicación muestra un gráfico de métricas que le permite realizar una comprobación rápida del estado de la aplicación.  

 ![Información general de métricas de aplicación](media/metrics/metrics-3.png)

Azure Spring Cloud proporciona estos cinco gráficos con métricas que se actualizan cada minuto:

* **Errores de servidor HTTP**: recuento de errores en las solicitudes HTTP a su aplicación
* **Entrada de datos**: bytes que ha recibido su aplicación
* **Salida de datos**: bytes que ha enviado su aplicación
* **Solicitud**: solicitudes que ha recibido su aplicación
* **Tiempo promedio de respuesta**: tiempo promedio de respuesta de su aplicación

En el gráfico, puede seleccionar un intervalo de tiempo de entre una hora y siete días.

## <a name="common-metrics-page"></a>Página de métricas comunes

El vínculo **Métricas** en el panel de navegación izquierdo abre la página de métricas comunes.

Primero, seleccione las métricas que quiere visualizar:

![Seleccionar la vista de métricas](media/metrics/metrics-4.png)

A continuación, seleccione el tipo de agregación para cada métrica:

![Agregación de métricas](media/metrics/metrics-5.png)

El tipo de agregación indica cómo se agrega el tiempo. Hay un punto de métrica cada minuto.
* Total: suma todas las métricas como salida del destino.
* Promedio: use el valor promedio del período como salida del destino.
* Máx./mín.: Use el valor Máx./mín. del período como salida del destino.

El intervalo de tiempo que se va a mostrar también puede modificarse.  El intervalo de tiempo puede elegirse de entre los últimos 30 minutos y los últimos 30 días, o un intervalo de tiempo personalizado.

![Modificación de métricas](media/metrics/metrics-6.png)

La vista predeterminada reúne todas las métricas de las aplicaciones de un servicio de Azure Spring Cloud. Se pueden filtrar las métricas de una aplicación o instancia en la pantalla.  Haga clic en **Agregar filtro**, establezca la propiedad en **Aplicación** y seleccione la aplicación de destino que quiere supervisar en el cuadro de texto **Valores**. 

Puede usar dos tipos de filtros (propiedades):
* Aplicación: filtrar por nombre de la aplicación
* Instancia: filtrar por instancia de la aplicación

![Filtros de métricas](media/metrics/metrics-7.png)

También puede usar la opción **Aplicar división**, que dibujará varias líneas para una aplicación:

![División de métricas](media/metrics/metrics-8.png)

>[!TIP]
> Puede crear sus propios gráficos en la página de métricas y anclarlos a su **panel**. Empiece por asignar un nombre al gráfico.  Después, seleccione **Anclar al panel en la esquina superior derecha**. Ya puede comprobar la aplicación en el **panel** del portal.

## <a name="user-portal-metrics-options"></a>Opciones de métricas del portal del usuario

En la tabla siguiente se muestran las métricas y detalles disponibles.
>[!div class="mx-tdBreakAll"]
>| Nombre | Display Name (Nombre para mostrar) | Nombre de la métrica de Spring Actuator | Unidad | Detalles |
>|----|----|----|----|------------|
>| SystemCpuUsagePercentage | System CPU Usage Percentage (Porcentaje de uso de CPU del sistema) | system.cpu.usage | Percent | Uso de CPU reciente de todo el sistema. Este es un valor Double en el intervalo [0,0, 1,0]. Un valor de 0,0 indica que todas las CPU estuvieron inactivas durante el período de tiempo reciente observado, mientras que un valor de 1,0 indica que todas las CPU se estaban ejecutando activamente el 100 % del tiempo durante el último período observado. Todos los valores entre 0,0 y 1,0 son posibles en función de las actividades que se estén realizando en el sistema. Si el uso reciente de la CPU del sistema no está disponible, el método devuelve un valor negativo. |
>| AppCpuUsagePercentage | App CPU Usage Percentage (Porcentaje de uso de CPU de la aplicación) | App CPU Usage Percentage (Porcentaje de uso de CPU de la aplicación) | Percent | Uso reciente de la CPU para el proceso de Máquina virtual Java. Este es un valor Double en el intervalo [0,0, 1,0]. Un valor de 0,0 indica que ninguna de las CPU estaba ejecutando subprocesos del proceso de Máquina virtual Java durante el período de tiempo reciente observado, mientras que un valor de 1,0 indica que todas las CPU estaban ejecutando activamente subprocesos de la Máquina virtual Java el 100 % del tiempo durante el último período observado. Los subprocesos de la Máquina virtual Java incluyen los subprocesos de la aplicación, así como los subprocesos internos de Máquina virtual Java. Todos los valores entre 0,0 y 1,0 son posibles en función de las actividades que se estén realizando en el proceso de Máquina virtual Java y en todo el sistema. Si el uso reciente de la CPU de la Máquina virtual Java no está disponible, el método devuelve un valor negativo. |
>| AppMemoryCommitted | App Memory Assigned (Memoria de la aplicación asignada) | jvm.memory.committed | Bytes | Representa la cantidad de memoria (en bytes) cuya disponibilidad está garantizada para uso de la Máquina virtual Java. La cantidad de memoria asignada puede cambiar con el tiempo (aumento o disminución). Es posible que la Máquina virtual Java libere memoria para el sistema y que la memoria asignada sea menor que al inicio. La memoria asignada siempre será mayor o igual que la utilizada. |
>| AppMemoryUsed | App Memory Used (Memoria de la aplicación utilizada) | jvm.memory.used | Bytes | Representa la cantidad de memoria que se usa actualmente en bytes. |
>| AppMemoryMax | App Memory Max (Memoria máxima de la aplicación) | jvm.memory.max | Bytes | Representa la cantidad máxima de memoria (en bytes) que se puede usar para la administración de memoria. Su valor puede ser indefinido. La cantidad máxima de memoria puede cambiar con el tiempo si es un valor definido. La cantidad de memoria usada y asignada siempre será menor o igual al valor máximo, si se definió el máximo. Se puede producir un error de asignación de memoria si intenta aumentar la memoria usada de modo que la memoria usada sea mayor que la memoria asignada, incluso si la memoria usada es menor o igual que el valor máximo (por ejemplo, cuando el sistema tiene poca memoria virtual). |
>| MaxOldGenMemoryPoolBytes | Max Available Old Generation Data Size (Tamaño máximo de datos de generación anterior disponible) | jvm.gc.max.data.size | Bytes | Uso de memoria máximo del bloque de memoria de generación anterior desde que se inició la máquina virtual Java. |
>| OldGenMemoryPoolBytes | Old Generation Data Size (Tamaño de datos de generación anterior) | jvm.gc.live.data.size | Bytes | Tamaño del grupo de memoria de generación anterior después de una recolección de elementos no utilizados completa. |
>| OldGenPromotedBytes | Promote to Old Generation Data Size (Promoción al tamaño de datos de generación anterior) | jvm.gc.memory.promoted | Bytes | Recuento de aumentos positivos en el tamaño del grupo de memoria de generación anterior entre antes y después de una recolección de elementos no utilizados. |
>| YoungGenPromotedBytes | Promote to Young Generation Data Size (Promoción al tamaño de datos de nueva generación) | jvm.gc.memory.allocated | Bytes | Se incrementa por un aumento del tamaño del grupo de memoria de nueva generación después de una recolección de elementos no utilizados y antes de la siguiente. |
>| GCPauseTotalCount | GC Pause Count (Recuento de pausas de la GC) | jvm.gc.pause (recuento total) | Count | Recuento total de recolecciones de elementos no utilizados después del inicio de esta Máquina virtual Java, incluidas las recolecciones de elementos no utilizados anteriores y nuevas. |
>| GCPauseTotalTime | GC Pause Total Time (Tiempo total de pausas de la GC) | jvm.gc.pause (tiempo total) | Milisegundos | Recuento total de tiempo consumido en recolecciones de elementos no utilizados después del inicio de esta Máquina virtual Java, incluidas las recolecciones de elementos no utilizados anteriores y nuevas. |
>| TomcatSentBytes | Tomcat Total Sent Bytes (Total de bytes enviados de Tomcat) | tomcat.global.sent | Bytes | Cantidad de datos enviados por el servidor web de Tomcat, en bytes |
>| TomcatReceivedBytes | Tomcat Total Received Bytes (Total de bytes recibidos de Tomcat) | tomcat.global.received | Bytes | Cantidad de datos recibidos por el servidor web de Tomcat, en bytes |
>| TomcatRequestTotalTime | Tomcat Request Total Time (Tiempo total de la solicitud de Tomcat) | tomcat.global.request (tiempo total) | Milisegundos | Tiempo total del servidor web de Tomcat para procesar las solicitudes. |
>| TomcatRequestTotalCount | Tomcat Request Total Count (Recuento total de las solicitudes de Tomcat) | tomcat.global.request (recuento total) | Count | Recuento total de solicitudes procesadas por el servidor web de Tomcat |
>| TomcatRequestMaxTime | Tomcat Request Max Time (Tiempo máximo de las solicitudes de Tomcat) | tomcat.global.request.max | Milisegundos | Tiempo máximo del servidor web de Tomcat para procesar una solicitud |
>| TomcatErrorCount | Tomcat Global Error (Error global de Tomcat) | tomcat.global.error | Count | Número de errores de las solicitudes procesadas |
>| TomcatSessionActiveMaxCount | Tomcat Session Max Active Count (Recuento máximo de sesiones activas de Tomcat) | tomcat.sessions.active.max | Count | Número máximo de sesiones que han estado activas al mismo tiempo |
>| TomcatSessionAliveMaxTime | Tomcat Session Max Alive Time (Tiempo de conexión máximo de las sesiones de Tomcat) | tomcat.sessions.alive.max | Milisegundos | Período más largo (en segundos) que una sesión expirada ha estado conectada |
>| TomcatSessionCreatedCount | Tomcat Session Created Count (Recuento de sesiones creadas de Tomcat) | tomcat.sessions.created | Count | Número de sesiones que se han creado |
>| TomcatSessionExpiredCount | Tomcat Session Expired Count (Recuento de sesiones expiradas de Tomcat) | tomcat.sessions.expired | Count | Número de sesiones que han expirado |
>| TomcatSessionRejectedCount | Tomcat Session Rejected Count (Recuento de sesiones rechazadas de Tomcat) | tomcat.sessions.rejected | Count | Número de sesiones que no se crearon porque se alcanzó el número máximo de sesiones activas. |

## <a name="see-also"></a>Consulte también
* [Introducción al Explorador de métricas de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started)

* [Análisis de registros y métricas con la configuración de diagnóstico](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)

## <a name="next-steps"></a>Pasos siguientes
* [Tutorial: Supervisión de recursos de Spring Cloud mediante alertas y grupos de acciones](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)

* [Cuotas y planes de servicio de Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quotas)

