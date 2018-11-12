---
title: Traslado del portal de OMS a Azure | Microsoft Docs
description: El portal de OMS desaparecerá al trasladar toda la funcionalidad a Azure Portal. En este artículo se ofrecen detalles sobre esta transición.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: c731d0498c026a5d0659335550f6e280a031b1e4
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011364"
---
# <a name="oms-portal-moving-to-azure"></a>Traslado del portal de OMS a Azure

> [!NOTE]
> Este artículo se aplica a la nube pública de Azure y a la nube del gobierno excepto donde se indique lo contrario.

Azure Portal es el centro de todos los servicios de Azure y ofrece una rica experiencia de administración con funcionalidades tales como paneles de información de anclaje, búsqueda inteligente y etiquetado para la administración de recursos. Para consolidar y agilizar el flujo de trabajo de supervisión y administración, empezamos por agregar las funcionalidades del portal de OMS a Azure Portal. Todas las características del portal de OMS forman parte ahora de Azure Portal. De hecho, algunas de las nuevas características, como Análisis de tráfico, solo están disponibles en Azure Portal. Con Azure Portal podrá llevar a cabo todo lo que hacía en el portal de OMS y mucho más. Si aún no lo ha hecho, empiece a usar Azure Portal hoy mismo.

**El portal de OMS se retirará oficialmente el 15 de enero de 2019.** Estamos entusiasmados con la mudanza a Azure Portal y esperamos que la transición sea fácil. Pero sabemos que los cambios son difíciles y pueden ser perturbadores. Envíe sus preguntas, comentarios o dudas a **LAUpgradeFeedback@microsoft.com**. En el resto de este artículo se repasan los principales escenarios y la hoja de ruta para esta transición.

## <a name="what-is-changing"></a>¿Qué está cambiando? 
Se anuncian los siguientes cambios al quedar en desuso el portal de OMS. Cada uno de estos cambios se describe con más detalle en las siguientes secciones.

- Las [áreas de trabajo](#new-workspaces) nuevas solo se pueden crear en Azure Portal.
- La nueva experiencia de Alert Management [sustituye a la solución Alert Management](#changes-to-alerts).
- La [administración de acceso de los usuarios](#user-access-and-role-migration) se realiza en Azure Portal con el control de acceso basado en rol de Azure.
- [Application Insights Connector ya no se necesita](#application-insights-connector-and-solution), ya que la misma funcionalidad está habilitada mediante las consultas de un área de trabajo a otra.
- Se va a dejar de utilizar la [aplicación móvil de OMS](#oms-mobile-app). 
- La [solución NSG se reemplaza](#azure-network-security-group-analytics) por las funcionalidades mejoradas disponibles a través de la solución Análisis de tráfico.
- Las nuevas conexiones de System Center Operations Manager con Log Analytics requieren [módulos de administración actualizados](#system-center-operations-manager).
- Consulte [Migración de las implementaciones de actualizaciones de OMS a Azure](../automation/migrate-oms-update-deployments.md) para más información sobre los cambios de [Update Management](../automation/automation-update-management.md).


## <a name="what-should-i-do-now"></a>¿Qué debo hacer a continuación?
Aunque la mayoría de las características seguirán funcionando sin necesidad de migración, debe realizar las siguientes tareas:

- Deberá [migrar sus permisos de usuario](#user-access-and-role-migration) a Azure Portal.
- Consulte [Migración de las implementaciones de actualizaciones de OMS a Azure](../automation/migrate-oms-update-deployments.md) para más información sobre la transición de la solución Update Management.

Consulte [Preguntas comunes para la transición desde el portal de OMS a Azure Portal para los usuarios de Log Analytics](../log-analytics/log-analytics-oms-portal-faq.md) para información sobre cómo realizar la transición a Azure Portal. Envíe sus comentarios, preguntas o dudas a **LAUpgradeFeedback@microsoft.com**.

## <a name="user-access-and-role-migration"></a>Migración de roles y acceso de usuario
La administración de acceso a Azure Portal es más completa y eficaz que la administración de acceso al portal de OMS. Vea [Administración de áreas de trabajo](log-analytics-manage-access.md#manage-accounts-and-users) para obtener más información de administración de acceso en Log Analytics.

> [!NOTE]
> En las versiones anteriores de este artículo se indica que los permisos pasarían automáticamente del portal de OMS a Azure Portal. Esta transición automática ya no está prevista y debe realizarla el usuario.

Puede que ya tenga el acceso adecuado en Azure Portal, en cuyo caso no es necesario realizar ningún cambio. Hay un par de casos en los que es posible que no tenga el acceso adecuado, en cuyo caso el administrador debe asignar los permisos.

- Tiene permisos de usuario de solo lectura en el portal de OMS, pero ninguno en Azure Portal. 
- Tiene permisos de colaborador en el portal de OMS, pero de solo lectura en Azure Portal.
 
En ambos casos, el administrador debe asignar manualmente el rol adecuado de la tabla siguiente. Se recomienda asignar este rol en el nivel de suscripción o de grupo de recursos.  Se proporcionarán instrucciones más normativas en breve para ambos casos.

| Permiso del portal de OMS | Rol de Azure |
|:---|:---|
| ReadOnly | Lector de Log Analytics |
| Colaborador | Colaborador de Log Analytics |
| Administrador | Propietario | 
 

## <a name="new-workspaces"></a>Nuevas áreas de trabajo
Ya no podrá crear áreas de trabajo mediante el portal de OMS. Siga las instrucciones de [Creación de un área de trabajo de Log Analytics en Azure Portal](log-analytics-quick-create-workspace.md) para crear un área de trabajo en Azure Portal.

## <a name="changes-to-alerts"></a>Cambios a las alertas

### <a name="alert-extension"></a>Extensión de alerta  

> [!NOTE]
> Las alertas ahora se han ampliado por completo a Azure Portal para la nube pública. Las reglas de alertas existentes pueden verse en el portal de OMS, pero solo se pueden administrar en Azure Portal. La extensión de alertas en Azure Portal se iniciará para la nube del gobierno de Azure en octubre de 2018.

Las alertas se han [ampliado en Azure Portal](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Cuando este proceso haya terminado, las acciones de administración en alertas solo estarán disponibles en Azure Portal. Las alertas existentes seguirán apareciendo en el portal de OMS. Si obtiene acceso a las alertas mediante programación con la API de REST para alertas de Log Analytics o la plantilla de recursos de alerta de Log Analytics, tendrá que usar grupos de acciones en lugar de acciones en las llamadas API, las plantillas de Azure Resource Manager y los comandos de PowerShell.

### <a name="alert-management-solution"></a>solución de administración de alertas
En lugar de la [solución Alert Management](log-analytics-solution-alert-management.md), puede usar la [interfaz de alertas unificadas de Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para visualizar y administrar las alertas. Esta nueva experiencia agrega alertas procedentes de varios orígenes a Azure, como, por ejemplo, las alertas de registro de Log Analytics. Puede ver las distribuciones de las alertas, aprovechar la agrupación automatizada de alertas relacionadas mediante grupos inteligentes y ver alertas en varias suscripciones mientras aplica filtros complejos. Todas estas características estarán disponibles en la versión preliminar a partir del 4 de junio de 2018. La solución Alert Management no estará disponible en Azure Portal. 

Los datos recopilados por la solución Alert Management (registros con un tipo de alerta) seguirán estando en Log Analytics siempre que la solución se instale en el área de trabajo. A partir de agosto de 2018, se habilitará el streaming de alertas procedentes de alertas unificadas en áreas de trabajo, lo que reemplaza esta funcionalidad. Se esperan algunos cambios de esquema que se anunciarán en una fecha posterior.

## <a name="oms-mobile-app"></a>Aplicación móvil de OMS
La aplicación móvil de OMS desaparecerá con el portal de OMS. En lugar de la aplicación móvil de OMS, para el acceso a información sobre la infraestructura de TI, los paneles de información y las consultas guardadas, tiene acceso a Azure Portal directamente desde el explorador del dispositivo móvil. Para obtener alertas, debe configurar [grupos de acciones de Azure](../monitoring-and-diagnostics/monitoring-action-groups.md) para recibir notificaciones en forma de SMS o llamada de voz

## <a name="application-insights-connector-and-solution"></a>Application Insights Connector y solución
[Application Insights Connector](log-analytics-app-insights-connector.md) ofrece una forma de incorporar datos de Application Insights a un área de trabajo de Log Analytics. Esta duplicación de datos era necesaria para permitir la visibilidad de los datos de la infraestructura y las aplicaciones.

Con el soporte de [consultas entre recursos](log-analytics-cross-workspace-search.md), ya no hay necesidad de duplicar los datos. Por lo tanto, la solución Application Insights existente quedará en desuso. A partir de octubre no podrán vincularse los nuevos recursos de Application Insights a áreas de trabajo de Log Analytics. Los paneles de información y los vínculos existentes seguirán funcionando hasta el 15 de enero de 2019.


## <a name="azure-network-security-group-analytics"></a>Azure Network Security Group Analytics
La [solución de Azure Network Security Group Analytics](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) se reemplazará con la solución [Análisis de tráfico](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) lanzada recientemente, que ofrece visibilidad de la actividad de usuario y de la aplicación en redes en la nube. Análisis de tráfico le ayuda a auditar la actividad de red de la organización, a proteger las aplicaciones y los datos, a optimizar el rendimiento de la carga de trabajo y a satisfacer los requisitos de cumplimiento. 

Esta solución analiza los registros de flujo de NSG y ofrece información sobre lo siguiente.

- Flujo de tráfico a través de las redes entre regiones de la nube pública, redes virtuales y subredes de Azure e Internet.
- Aplicaciones y protocolos de la red, sin necesidad de rastreadores ni aparatos de recopilación de flujo dedicados.
- Top Talkers, aplicaciones comunicativas, conversaciones de máquinas virtuales en la nube, zonas activas de tráfico.
- Orígenes y destinos del tráfico entre redes virtuales, interrelaciones entre servicios y aplicaciones críticos para el negocio.
- Seguridad, que incluye tráfico malintencionado, puertos abiertos a Internet, aplicaciones o máquinas virtuales que intentan tener acceso a Internet.
- Utilización de la capacidad, que ayuda a eliminar problemas de sobreaprovisionamiento o infrautilización.

Aún puede contar con la configuración de diagnóstico para enviar los registros de NSG a Log Analytics para que las búsquedas, las alertas y los paneles de información guardados continúen funcionando. Los clientes que ya tengan instalada la solución pueden seguir utilizándola hasta nuevo aviso. A partir del 5 de septiembre, la solución Network Security Group Analytics se quitará de Marketplace y estará disponible en la comunidad como [plantilla de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Si ha [conectado el grupo de administración de Operations Manager a Log Analytics](log-analytics-om-agents.md), seguirá funcionando sin cambios. Aunque para las nuevas conexiones, debe seguir las instrucciones de [Microsoft System Center Operations Manager Management Pack to configure Operations Management Suite](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/) (Módulo de administración de Microsoft System Center Operations Manager para configurar Operations Management Suite).

## <a name="next-steps"></a>Pasos siguientes
- Vea [Common questions for transition from OMS portal to Azure portal for Log Analytics users](log-analytics-oms-portal-faq.md) (Preguntas frecuentes sobre la transición del portal de OMS a Azure Portal para usuarios de Log Analytics) a fin de obtener instrucciones sobre cómo trasladarse del portal de OMS a Azure Portal.
