---
title: Información general sobre las alertas clásicas en Microsoft Azure y Azure Monitor
description: Las alertas clásicas están en desuso. Las alertas permiten supervisar los registros, los eventos o las métricas de recursos de Azure y recibir una notificación cuando se cumple una condición especificada.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 7046a0c6ac84ad5f156098a26dcef2b8accd50af
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987652"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>¿Qué son las alertas clásicas en Microsoft Azure?

> [!NOTE]
> En este artículo se describe cómo crear alertas de métrica clásicas más antiguas. Azure Monitor ahora es compatible con [una nueva experiencia de alertas y las más recientes alertas de métrica casi en tiempo real](monitoring-overview-alerts.md). 
>

Las alertas le permiten configurar las condiciones en los datos y recibir notificaciones cuando las condiciones coinciden con los datos de supervisión más actualizados.

## <a name="old-and-new-alerting-capabilities"></a>Funcionalidades antiguas y nuevas de generación de alertas

Anteriormente, Azure Monitor, Application Insights, Log Analytics y Service Health tenían funcionalidades independientes de generación de alertas. Con el tiempo, Azure ha mejorado y combinado la interfaz de usuario y los distintos métodos de alerta. La consolidación aún está en proceso. Alertas

Puede ver las alertas clásicas solo en la pantalla de usuario de alertas clásicas en Azure Portal. Para acceder a esta pantalla, debe seleccionar el botón **Ver alertas clásicas** en la pantalla de alertas. 

 ![Opciones de alertas en Azure Portal](./media/monitoring-overview-alerts-classic/monitor-alert-screen2.png) 

La nueva experiencia de usuario para alertas presenta las siguientes ventajas en comparación con la experiencia de alertas clásicas:
-   **Mejor sistema de notificaciones**: todas las nuevas alertas usan grupos de acciones, que se denominan grupos de notificaciones y acciones que pueden reutilizarse en varias alertas. Las alertas de métricas clásicas y las alertas de Log Analytics más antiguas no usan grupos de acciones.
-   **Una experiencia de creación unificada**: la creación de todas las alertas para las métricas, los registros y el registro de actividad en Azure Monitor, Log Analytics y Application Insights está disponible desde un solo lugar.
-   **Visualización de las alertas desencadenadas de Log Analytics en Azure Portal**: ahora también puede ver las alertas desencadenadas de Log Analytics en su suscripción. Anteriormente, estas se encontraban en un portal independiente.
-   **Separación de las alertas desencadenadas y las reglas de alertas**: las reglas de alertas (la definición de la condición que desencadena una alerta) y las alertas desencadenadas (una instancia de la activación de regla de alertas) están diferenciadas, es decir, que las vistas operativas y de configuración son independientes.
-   **Mejor flujo de trabajo**: la nueva experiencia de creación de Alertas guía al usuario en el proceso de configuración de una regla de alertas, lo que facilita la detección de las condiciones correctas para que se active una alerta.
-   **Consolidación de alertas inteligentes** y **configuración del estado de alerta**: las alertas más recientes incluyen la funcionalidad de agrupación automática, que muestra las alertas similares juntas para reducir la sobrecarga de la interfaz de usuario. 

Las alertas de métricas más recientes presentan estas ventajas en comparación con las alertas de métricas clásicas:
-   **Latencia mejorada**: las nuevas alertas de métricas se pueden ejecutar con una frecuencia de cada minuto. Las alertas de métricas anteriores siempre se ejecutan con una frecuencia de 5 minutos. Las alertas más recientes tienen un retraso cada vez menor desde el momento del problema hasta la notificación o la acción (de 3 a 5 minutos). Las alertas anteriores tardan entre 5 y 15 minutos, según el tipo.  Las alertas de registro suelen tener un retraso de 10 a 15 minutos debido al tiempo que se tarda en ingerir los registros, pero los métodos de procesamiento más recientes reducen ese tiempo. 
-   **Compatibilidad con métricas multidimensionales**: puede enviar alertas sobre las métricas dimensionales que le permiten supervisar un segmento interesante de la métrica.
-   **Más control sobre las condiciones de las métricas**: puede definir reglas de alerta más avanzadas. Las nuevas alertas admiten la supervisión de los valores máximos, mínimos, promedio y totales de las métricas.
-   **Supervisión combinada de varias métricas**: se pueden supervisar varias métricas (actualmente hasta dos métricas) con una sola regla. Si ambas métricas incumplen sus respectivos umbrales durante el período especificado, se desencadena una alerta.
-   **Mejor sistema de notificación**: todas las nuevas alertas utilizan [grupos de acciones](../monitoring-and-diagnostics/monitoring-action-groups.md), que se denominan grupos de notificaciones y acciones que pueden reutilizarse en varias alertas.  Las alertas de métricas clásicas y las alertas de Log Analytics más antiguas no usan grupos de acciones. 
-   **Métricas de registros** (versión preliminar pública): los datos de registro que van a Log Analytics ahora se pueden extraer y convertir en métricas de Azure Monitor y luego en alertas como cualquier otra métrica. Consulte el artículo sobre [alertas (clásicas)](monitoring-overview-alerts-classic.md) para conocer la terminología específica de las alertas clásicas. 


## <a name="classic-alerts-on-azure-monitor-data"></a>Alertas clásicas sobre datos de Azure Monitor
Hay dos tipos de alertas clásicas disponibles: alertas de métricas y alertas de registro de actividad.

* **Alertas de métricas clásicas**: la alerta se desencadena cuando el valor de una métrica específica cruza un umbral que se le haya asignado. La alerta en cuestión genera una notificación cuando se "activa" (si se sobrepasa el umbral y se cumple la condición de alerta). Genera otra notificación cuando se "resuelve" (esto es, cuando se sobrepasa el umbral de nuevo y ya no se cumple la condición).

* **Alertas clásicas de registro de actividad**: una alerta de registro de streaming que se desencadena cuando se genera un evento de registro de actividad que coincide con los criterios de filtro que se han asignado. Estas alertas tienen solo un estado "Activado", puesto que el motor de alertas simplemente aplica los criterios de filtro a cualquier evento nuevo. Se pueden utilizar estas alertas para recibir notificaciones cuando se produzca un incidente de Service Health o cuando un usuario o una aplicación realiza una operación en su suscripción, por ejemplo, "Eliminar máquina virtual".

Para los datos de registro de diagnóstico disponibles a través de Azure Monitor, enrute los datos a Log Analytics (anteriormente, OMS) y use una alerta de consulta de Log Analytics. Log Analytics utiliza ahora el [nuevo método de alertas](monitoring-overview-unified-alerts.md) 

El diagrama siguiente resume los orígenes de datos en Azure Monitor y, de forma conceptual, la manera en la que se puede se puede alertar sobre esos datos.

![Alertas explicadas](./media/monitoring-overview-alerts-classic/Alerts_Overview_Resource_v5.png)

## <a name="taxonomy-of-alerts-classic"></a>Taxonomía de alertas (clásicas)
Azure utiliza los siguientes términos para describir las alertas clásicas y sus funciones:
* **Alerta**: una definición de criterios (una o más reglas o condiciones) que se activa cuando se cumplen.
* **Activo**: estado en el que se cumplen los criterios definidos por una alerta clásica.
* **Resuelto**: estado en el que ya no se cumplen los criterios definidos por una alerta clásica, después de que se cumplieran con anterioridad.
* **Notificación**: acción que se realiza a partir de la activación de una alerta clásica.
* **Acción**: una llamada específica que se envían al receptor de una notificación (por ejemplo, envío de un correo electrónico a una dirección o una publicación en una dirección URL de webhook). Las notificaciones normalmente pueden desencadenar varias acciones.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>¿Cómo se puede recibir una notificación de una alerta clásica de Azure Monitor?
Históricamente, las alertas de Azure de distintos servicios usan sus propios métodos de notificación integrados. 

Azure Monitor creó una agrupación de notificaciones reutilizables denominada *grupos de acciones*. Los grupos de acciones especifican un conjunto de receptores para una notificación y, cada vez que se activa una alerta que hace referencia al Grupo de acciones, todos los receptores reciben esa notificación. Los grupo de acciones permiten reutilizar una agrupación de receptores (por ejemplo, la lista de ingenieros de guardia) a través de muchos objetos de alerta. Los grupos de acciones admiten la notificación mediante la publicación en una URL de webhook, además de direcciones de correo electrónico, números de SMS y otras acciones.  Para más información, consulte [Grupos de acciones](monitoring-action-groups.md). 

Las alertas clásicas anteriores del registro de actividad usan grupos de acciones.

Pero las anteriores alertas de métricas aún no los utilizan. En cambio, se pueden configurar las siguientes acciones: 
- Enviar notificaciones de correo electrónico al administrador de servicios o a los coadministradores, o a las direcciones de correo electrónico adicionales que especifique.
- Llamar a un webhook, que permite iniciar acciones de automatización adicionales.

Los webhooks permiten la automatización y corrección, por ejemplo, mediante:
    - Runbook de Azure Automation
    - Función de Azure
    - Aplicación lógica de Azure
    - un servicio de terceros

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre las reglas de alertas y su configuración mediante:

* Más información sobre las [métricas](monitoring-overview-metrics.md)
* Configuración de [alertas de métricas clásicas a través de Azure Portal](insights-alerts-portal.md)
* Configuración de [alertas de métricas clásicas a través de PowerShell](insights-alerts-powershell.md)
* Configuración de [alertas de métricas clásicas con la interfaz de línea de comandos (CLI)](insights-alerts-command-line-interface.md)
* Configuración de [alertas de métricas clásicas con la API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Más información sobre el [registro de actividad](monitoring-overview-activity-logs.md)
* Configuración de [alertas del registro de actividad a través de Azure Portal](monitoring-activity-log-alerts.md)
* Configuración de [alertas del registro de actividad a través de Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Revisión del [esquema de webhook de alertas del registro de actividad](monitoring-activity-log-alerts-webhook.md)
* Obtenga más información sobre los [grupos de acciones](monitoring-action-groups.md)
* Configuración de [alertas más recientes](monitor-alerts-unified-usage.md)