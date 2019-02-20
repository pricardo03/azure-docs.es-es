---
title: Alertas y supervisión unificadas de Azure Monitor reemplaza a alertas y supervisión clásicas
description: Información general acerca de la retirada de la funcionalidad y los servicios de supervisión clásicos, que antes se mostraban en Azure Portal en Alertas (clásico). Alertas y supervisión clásicas incluye alertas de métricas clásicas de recursos de Azure, alertas de métricas clásicas de Application Insights, alertas de WebTest clásicas de Application Insights, alertas clásicas basadas en métricas personalizadas de Application Insights y alertas clásicas alertas de Application Insights SmartDetection v1
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 2/7/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 7cca9d9c91f35c30bbd71ae82b9f02447051078d
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55983175"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Alertas y supervisión unificadas de Azure Monitor reemplaza a alertas y supervisión clásicas

Azure Monitor se ha convertido en un servicio de supervisión completo y unificado que ahora admite "One Metrics" (métricas unificadas) y "One Alerts" (alertas unificadas) en los recursos; para más información, consulte nuestra [entrada de blog acerca del nuevo servicio Azure Monitor](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/). Las nuevas plataformas de supervisión y generación de alertas de Azure se han creado para que sean más rápidas, inteligentes y extensibles (con lo que se sigue el ritmo de la creciente expansión de la informática en la nube y está en línea con la filosofía de Microsoft Intelligent Cloud). 

Con la implantación de la nueva plataforma de supervisión y generación de alertas de Azure, se retirará la plataforma "clásica" de supervisión y alertas (hospedada en la sección *ver alertas clásicas* de las alertas de Azure y **dejará de usarse en junio de 2019 en las nubes públicas de Azure**). [La nube Azure Government](../../azure-government/documentation-government-welcome.md) no se verá afectada.

 ![Alerta clásica en Azure Portal](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Le animamos a volver a crear las alertas en la nueva plataforma. Para los clientes que tienen un gran número de alertas, se está trabajando para proporcionar una forma automatizada de mover las alertas clásicas existentes al nuevo sistema sin interrupciones del servicio ni costos adicionales.

> [!IMPORTANT]
> Las reglas de alerta clásicas creadas en el registro de actividad no caerán en desuso ni se migrarán. Puede acceder a todas las reglas de alerta clásicas creadas en el registro de actividad y usarlas tal cual, desde la nueva Azure Monitor: Alertas. Para más información, vea [Crear, ver y administrar las alertas del registro de actividad mediante Azure Monitor](../../azure-monitor/platform/alerts-activity-log.md). De forma similar, puede acceder a alertas en Service Health y usarlas tal cual desde la nueva sección Service Health. Para más información, consulte las [alertas en las notificaciones de Service Health](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Métricas y alertas unificadas en Application Insights

La nueva plataforma de métricas de Azure Monitor alimentará la supervisión procedente de Application Insights. Este cambio significa que Application Insights se enlazará a Grupos de acciones, lo que aporta muchas más opciones que anteriores las llamadas de webhook y el correo electrónico. Ahora, las alertas pueden desencadenar llamadas de voz, Azure Functions, Logic Apps, SMS y las herramientas de ITSM, como ServiceNow y runbooks de Automation. Con la generación de alertas y la supervisión casi en tiempo real, la nueva plataforma permite a los usuarios de Application Insights sacar provecho de la misma tecnología que alimenta la supervisión en otros recursos de Azure y sustenta la supervisión de los productos de Microsoft.

Comprenden la nueva supervisión y generación de alertas unificadas de Application Insights englobará:

- **Métricas de la plataforma Application Insights**: que proporciona métricas populares creadas previamente del producto Application Insights. Para más información, consulte este artículo acerca del uso de [métricas de plataforma para Application Insights en el nuevo Azure Monitor](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics).
- **Disponibilidad de Application Insights y prueba web**: que proporciona la capacidad de evaluar la capacidad de respuesta y disponibilidad de una aplicación web o servidor. Para más información, consulte este artículo acerca del uso de [prueba de disponibilidad y alertas para Application Insights en el nuevo Azure Monitor](../../azure-monitor/app/monitor-web-app-availability.md).
- **Métricas personalizadas de Application Insights**: que permiten definir y emitir sus propias métricas de supervisión y alertas. Para más información, consulte este artículo acerca del uso de [métricas personalizadas para Application Insights en el nuevo Azure Monitor](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).
- **Anomalías en los errores de Application Insights (parte de Detección inteligente)**: que le notifica automáticamente casi en tiempo real si una aplicación web sufre un aumento anómalo en la frecuencia de solicitudes HTTP o llamadas de dependencia erróneas. Anomalías en los errores de Application Insights (parte de Detección inteligente), como parte del nuevo Azure Monitor, estará disponible pronto y este documento se actualizará con vínculos en la siguiente iteración, ya que se lanzará en los próximos meses.

## <a name="unified-metrics-and-alerts-for-other-azure-resources"></a>Métricas y alertas unificadas para otros recursos de Azure

Desde marzo de 2018, la próxima generación de alertas y supervisión multidimensional para recursos de Azure han estado en disponibilidad. Ahora la plataforma de métricas más reciente y la generación de alertas es más rápida con funcionalidades casi en tiempo real. Y lo que es más importante, las nuevas alertas de la plataforma de métricas proporcionan mayor granularidad, ya que la nueva plataforma incluye la opción de dimensiones, lo que le permiten segmentar y filtrar en una combinación de valores, condición u operación específicas. Al igual que todas las alertas del nuevo Azure Monitor, las nuevas alertas de métricas son más extensibles con el uso de ActionGroups (lo que permite que las notificaciones vayan más allá del correo electrónico o webhook a SMS, voz, Azure Functions, runbook de Automation, etc. Para más información, vea [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../../azure-monitor/platform/alerts-metric.md).
Las métricas más recientes de los recursos de Azure están disponibles como:

- **Métricas de la plataforma estándar de Azure Monitor**: que proporciona métricas populares rellenadas previamente de varios productos y servicios de Azure. Para más información, consulte este artículo acerca de las [métricas compatibles con Azure Monitor](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported) y [Compatibilidad con alertas de métricas en Azure Monitor](../../azure-monitor/platform/alerts-metric-overview.md#supported-resource-types-for-metric-alerts).
- **Métricas personalizadas de Azure Monitor**: que proporciona métricas de orígenes controlados por el usuario, lo que incluye el agente de Azure Diagnostics. Para más información, consulte este artículo acerca de las [métricas personalizadas en Azure Monitor](../../azure-monitor/platform/metrics-custom-overview.md). Con las métricas personalizadas también puede publicar las métricas recopiladas por el [agente de Windows Azure Diagnostics](../../azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm.md) y el [agente de InfluxData Telegraf](../../azure-monitor/platform/collect-custom-metrics-linux-telegraf.md).

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Retirada de la plataforma de alertas y supervisión clásica

Como se indicó anteriormente, la plataforma de alertas y supervisión clásica que se puede usar actualmente desde la [sección Alertas (clásico)](../../azure-monitor/platform/alerts-classic.overview.md) de Azure Portal se retirará el portal en los próximos meses, ya que se ha reemplazado por el nuevo sistema.
El 30 de junio de 2019 se retirarán la funcionalidad clásica de supervisión y alertas anterior, lo que incluye el cierre de las API relacionadas, la interfaz de Azure Portal y los servicios que contiene. En concreto, estas características quedarán en desuso:

- Métricas y alertas anteriores (clásicas) para recursos de Azure están actualmente disponibles en la [sección Alertas (clásico)](../../azure-monitor/platform/alerts-classic.overview.md) de Azure Portal y se puede acceder a ellas como el recurso [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules).
- Métricas personalizadas y de la plataforma anteriores (clásicas) para Application Insights, así como la generación de alertas sobre ellos, que actualmente está disponible en la [sección Alertas (clásico)](../../azure-monitor/platform/alerts-classic.overview.md) de Azure Portal y se puede acceder a ellas como el recurso [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules).
- Alerta de anomalías en los errores anteriores (clásicas), que actualmente están disponibles como [Detección inteligente en Application Insights](../../azure-monitor/app/proactive-diagnostics.md) en Azure Portal, con las alertas configuradas que se muestran en la sección [Alertas (clásico)](../../azure-monitor/platform/alerts-classic.overview.md) de Azure Portal.

Todos los sistemas clásicos de supervisión y generación de alertas, incluidos la [API](https://msdn.microsoft.com/library/azure/dn931945.aspx) correspondiente, [PowerShell](../../azure-monitor/platform/alerts-classic-portal.md), la [CLI](../../azure-monitor/platform/alerts-classic-portal.md), la [página de Azure Portal](../../azure-monitor/platform/alerts-classic-portal.md) y la [plantilla de recursos](../../azure-monitor/platform/alerts-enable-template.md), se podrán utilizar hasta finales de junio de 2019. 

A finales de junio de 2019, en Azure Monitor:

- El servicio de alertas y supervisión clásico se retirará y dejará de estar disponible para la creación de nuevas reglas de alerta.
- Todas las reglas de alerta que sigan en Alertas (clásico) después de junio de 2019 continuarán ejecutándose y enviando notificaciones, pero no estarán disponibles para su modificación.
- Microsoft migrará automáticamente todas las alertas que permanezcan en la plataforma de supervisión y generación de alertas a su equivalente en la nueva plataforma de Azure Monitor a partir de julio de 2019. El proceso será ininterrumpido, no habrá tiempo de inactividad y garantizará que los clientes no pierden cobertura de supervisión.
- Las reglas de alertas migradas a la nueva plataforma de alertas proporcionarán cobertura de supervisión como antes, pero activarán notificaciones con las nuevas cargas útiles. Todas las direcciones de correo electrónico, los puntos de conexión de webhook o los vínculos a aplicaciones lógicas asociados con la regla de alertas clásica se traspasarán al realizar la migración, pero puede que su comportamiento no sea correcto, ya que la carga útil de la alerta será distinta en la nueva plataforma.

> [!IMPORTANT]
> En breve, Microsoft Azure Monitor proporcionará herramientas para que los clientes puedan migrar automáticamente las reglas de alerta clásicas a la nueva plataforma. Además, a partir de julio de 2019, deberá ejecutarla de manera forzosa para todas las reglas de alertas clásicas que aún existan. Los clientes deberán garantizar la adaptación de la automatización que consume la carga útil de reglas de alertas clásicas para poder controlar la nueva carga útil siguiendo los procedimientos descritos en [Métricas y alertas unificadas en Application Insights](#unified-metrics-and-alerts-in-application-insights) o [Métricas y alertas unificadas para otros recursos de Azure](#unified-metrics-and-alerts-for-other-azure-resources) después de la migración de las reglas de alertas clásicas. 

Pronto se facilitarán herramientas que permitirán migrar voluntariamente las alertas de la [sección Alertas (clásico)](../../azure-monitor/platform/alerts-classic.overview.md) de Azure Portal a la nueva funcionalidad de alertas de Azure. Todas las reglas configuradas en Alertas (clásico) que se migren al nuevo Azure Monitor seguirá siendo gratuitos. Además, no se cobrará nada a las reglas de alertas clásicas migradas por la inserción de notificaciones a través de correo electrónico, webhook o LogicApp. Sin embargo, el uso de tipos de acciones o notificaciones nuevos (como SMS, llamada de voz, integración de ITSM, etc.) se cargará si se agrega a una alerta nueva o a una migrada. Para más información, consulte [Precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

Además, lo siguiente se cargará en el ámbito de [Precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/):

- Todas las nuevas reglas de alertas (no migradas) creadas después de las unidades gratuitas en la nueva plataforma de Azure Monitor
- Todos los datos ingeridos y conservados en unidades que no sean las gratuitas incluidas por Azure Monitor
- Todas las pruebas web que ejecuta Application Insights
- Todas las métricas personalizadas almacenadas en unidades que no sean las gratuitas que se incluyen en Azure Monitor

Este artículo se actualizará constantemente con vínculos y detalles relativos a la nueva funcionalidad de supervisión y generación de alertas de Azure, así como con la disponibilidad de las herramientas que ayudan a los usuarios a adoptar la nueva plataforma de Azure Monitor.


## <a name="next-steps"></a>Pasos siguientes

* Más información acerca del [nuevo Azure Monitor unificado](../../azure-monitor/overview.md).
* Más información acerca de las nuevas [alertas de Azure](../../azure-monitor/platform/alerts-overview.md).

