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
ms.date: 06/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e47e8cbd209ea34317ca9b176a2c4b0fef10a2b2
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133604"
---
# <a name="oms-portal-moving-to-azure"></a>Traslado del portal de OMS a Azure
Gracias por usar el portal de OMS. Nos sentimos alentados por su apoyo y seguimos haciendo grandes inversiones en nuestros servicios de supervisión y administración. Uno de los comentarios que se oyen reiteradamente de los clientes es la necesidad de una sola experiencia de usuario para supervisar y administrar tanto las cargas de trabajo locales como las de Azure. Probablemente sepa que Azure Portal es el centro de todos los servicios de Azure y ofrece una rica experiencia de administración con funcionalidades tales como paneles de información para anclar recursos, búsqueda inteligente para encontrar recursos y etiquetado para la administración de recursos. Para consolidar y agilizar el flujo de trabajo de supervisión y administración, empezamos por agregar las funcionalidades del portal de OMS a Azure Portal. Nos complace anunciar que la mayoría de las características del portal de OMS forman parte ahora de Azure Portal. De hecho, algunas de las nuevas características, como Traffic Manager, solo están disponibles en Azure Portal. Solo quedan algunas lagunas pendientes, siendo las más impactantes cinco soluciones que aún están en proceso de ser trasladadas a Azure Portal. Si no utiliza estas características, podrá llevar a cabo todo lo que hacía en el portal de OMS y mucho más con Azure Portal. Si aún no lo ha hecho, le recomendamos que empiece a usar Azure Portal hoy mismo. 

Esperamos cerrar las lagunas pendientes entre los dos portales antes de agosto de 2018. En función de los comentarios de los clientes, comunicaremos la escala de tiempo para la desaparición del portal de OMS. Estamos entusiasmados con la mudanza a Azure Portal y esperamos que la transición sea fácil. Pero sabemos que los cambios son difíciles y pueden ser perturbadores. Envíe sus preguntas, comentarios o dudas a LAUpgradeFeedback@microsoft.com. En el resto de este artículo se repasan los principales escenarios, las lagunas actuales y la hoja de ruta para esta transición. 


## <a name="what-will-change"></a>¿Qué cambiará? 
Se anuncian los siguientes cambios al quedar en desuso el portal de OMS. Cada uno de estos cambios se describe con más detalle en las siguientes secciones.

- La nueva experiencia de Alert Management sustituirá a la solución Alert Management.
- La administración de acceso de los usuarios se realizará en Azure Portal con el control de acceso basado en rol de Azure.
- Application Insights Connector ya no se necesita, ya que puede habilitarse la misma funcionalidad a través de consultas entre áreas de trabajo.
- Dejará de utilizarse la aplicación móvil de OMS. 
- La solución NSG se reemplaza por las funcionalidades mejoradas disponibles a través de la solución de Análisis de tráfico.



## <a name="current-known-gaps"></a>Lagunas conocidas actuales 
Actualmente hay algunas lagunas de funcionalidad que le obligan a seguir usando el portal de OMS. Estas lagunas se están subsanando, y este documento se actualizará adecuadamente. Para ver anuncios continuos sobre extensiones y cambios, debe consultar también las [actualizaciones de Azure](https://azure.microsoft.com/updates/?product=log-analytics).

- Las siguientes soluciones aún no están plenamente operativas en Azure Portal. Debe seguir utilizando estas soluciones en el portal clásico hasta que se actualicen.

    - Las soluciones de Windows Analytics ([Upgrade Readiness](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [Estado del dispositivo](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) y [Update Compliance](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [DNS Analytics](log-analytics-dns.md) 
    - [Surface Hub](log-analytics-surface-hubs.md)

-  Para tener acceso a recursos de Log Analytics, el usuario debe tener acceso mediante el [acceso basado en roles de Azure](#user-access-and-role-migration).
- Es posible que las programaciones de actualizaciones que se crearon con el portal de OMS no se reflejen en las implementaciones de actualizaciones programadas o el historial de trabajos de actualización del panel de información de Update Management en Azure Portal. Se espera que esta laguna se haya subsanado a finales de junio de 2018.
- Característica en vista previa (GB) de los registros personalizados solo puede habilitarse mediante el portal de OMS. A finales de junio de 2018, se habilitará automáticamente para todas las áreas de trabajo.
 
## <a name="what-should-i-do-now"></a>¿Qué debo hacer a continuación?  
Debe consultar [Common questions for transition from OMS portal to Azure portal for Log Analytics users](../log-analytics/log-analytics-oms-portal-faq.md) (Preguntas frecuentes sobre la transición del portal de OMS a Azure Portal para usuarios de Log Analytics) a fin de obtener información sobre cómo realizar una transición a Azure Portal. Si las [lagunas descritas antes](#current-known-gaps) no se aplican a su entorno, debe plantearse empezar a utilizar Azure Portal como experiencia principal. Envíe sus comentarios, preguntas o dudas a LAUpgradeFeedback@microsoft.com.

## <a name="changes-to-alerts"></a>Cambios a las alertas 

### <a name="alert-extension"></a>Extensión de alerta  
Las alertas están en proceso de ser [extendidas a Azure Portal](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Cuando este proceso haya terminado, las acciones de administración en alertas solo estarán disponibles en Azure Portal. Las alertas existentes seguirán apareciendo en el portal de OMS. Si obtiene acceso a las alertas mediante programación con la API de REST para alertas de Log Analytics o la plantilla de recursos de alerta de Log Analytics, tendrá que usar grupos de acciones en lugar de acciones en las llamadas API, las plantillas de Azure Resource Manager y los comandos de PowerShell.

### <a name="alert-management-solution"></a>solución de administración de alertas
En lugar de la [solución Alert Management](log-analytics-solution-alert-management.md), puede usar la [interfaz de alertas unificadas de Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para visualizar y administrar las alertas. Esta nueva experiencia agrega alertas procedentes de varios orígenes a Azure, como, por ejemplo, las alertas de registro de Log Analytics. Puede ver las distribuciones de las alertas, aprovechar la agrupación automatizada de alertas relacionadas mediante grupos inteligentes y ver alertas en varias suscripciones mientras aplica filtros complejos. Todas estas características estarán disponibles en la versión preliminar a partir del 4 de junio de 2018. La solución Alert Management no estará disponible en Azure Portal. 

Los datos recopilados por la solución Alert Management (registros con un tipo de alerta) seguirán estando en Log Analytics siempre que la solución se instale en el área de trabajo. A partir de agosto de 2018, se habilitará el streaming de alertas procedentes de alertas unificadas en áreas de trabajo, lo que reemplaza esta funcionalidad. Se esperan algunos cambios de esquema que se anunciarán en una fecha posterior.

## <a name="user-access-and-role-migration"></a>Migración de roles y acceso de usuario
La administración de acceso de Azure Portal es más completa y eficaz que la administración de acceso del portal de OMS, pero exige algunas conversiones. Vea [Administración de áreas de trabajo](log-analytics-manage-access.md#manage-accounts-and-users) para obtener más información de administración de acceso en Log Analytics.

A partir del 25 de junio y durante julio, se iniciará la conversión automática de los permisos de control de acceso del portal de OMS en permisos de Azure Portal. Cuando la conversión haya terminado, la sección de administración de usuarios del portal de OMS enrutará a los usuarios al control de acceso (IAM) en Azure. 

Durante la conversión, el sistema comprobará cada usuario o grupo de seguridad que tenga permisos en el portal OMS y determinará si tiene el mismo nivel o permisos en Azure. Si faltan permisos, se asignarán los siguientes roles a las áreas de trabajo y soluciones relevantes.

| Permiso del portal de OMS | Rol de Azure |
|:---|:---|
| ReadOnly | Lector de Log Analytics |
| Colaborador | Colaborador de Log Analytics |
| Administrador | Propietario |

Para asegurarse de que no se asignan permisos excesivos a los usuarios, el sistema no asignará automáticamente estos permisos en el nivel de grupo de recursos. Por consiguiente, los administradores de áreas de trabajo deben asignarse a sí mismos roles de _propietario_ o _colaborador_ en el nivel de suscripción o grupo de recursos para realizar las siguientes acciones.

- Agregar o quitar soluciones
- Definir nuevas vistas personalizadas
- Administrar alertas 

En algunos casos, la conversión automática no puede aplicar permisos y pedirá al administrador que los asigne manualmente.

## <a name="oms-mobile-app"></a>Aplicación móvil de OMS
La aplicación móvil de OMS desaparecerá con el portal de OMS. En lugar de la aplicación móvil de OMS, para el acceso a información sobre la infraestructura de TI, los paneles de información y las consultas guardadas, tiene acceso a Azure Portal directamente desde el explorador del dispositivo móvil. Para obtener alertas, debe configurar [grupos de acciones de Azure](../monitoring-and-diagnostics/monitoring-action-groups.md) para recibir notificaciones en forma de SMS o llamada de voz

## <a name="application-insights-connector-and-solution"></a>Application Insights Connector y solución
[Application Insights Connector](log-analytics-app-insights-connector.md) ofrece una forma de incorporar datos de Application Insights a un área de trabajo de Log Analytics. Esta duplicación de datos era necesaria para permitir la visibilidad de los datos de la infraestructura y las aplicaciones.

Con el soporte de [consultas entre recursos](log-analytics-cross-workspace-search.md), ya no hay necesidad de duplicar los datos. Por lo tanto, la solución Application Insights existente quedará en desuso. A partir de julio, no podrán vincularse los nuevos recursos de Application Insights a áreas de trabajo de Log Analytics. Los paneles de información y los vínculos existentes seguirán funcionando hasta noviembre de 2018.


## <a name="azure-network-security-group-analytics"></a>Azure Network Security Group Analytics
La [solución de Azure Network Security Group Analytics](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) se reemplazará con la solución [Análisis de tráfico](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) lanzada recientemente, que ofrece visibilidad de la actividad de usuario y de la aplicación en redes en la nube. Análisis de tráfico le ayuda a auditar la actividad de red de la organización, a proteger las aplicaciones y los datos, a optimizar el rendimiento de la carga de trabajo y a satisfacer los requisitos de cumplimiento. 

Esta solución analiza los registros de flujo de NSG y ofrece información sobre lo siguiente.

- Flujo de tráfico a través de las redes entre regiones de la nube pública, redes virtuales y subredes de Azure e Internet.
- Aplicaciones y protocolos de la red, sin necesidad de rastreadores ni aparatos de recopilación de flujo dedicados.
- Top Talkers, aplicaciones comunicativas, conversaciones de máquinas virtuales en la nube, zonas activas de tráfico.
- Orígenes y destinos del tráfico entre redes virtuales, interrelaciones entre servicios y aplicaciones críticos para el negocio.
- Seguridad, que incluye tráfico malintencionado, puertos abiertos a Internet, aplicaciones o máquinas virtuales que intentan tener acceso a Internet.
- Utilización de la capacidad, que ayuda a eliminar problemas de sobreaprovisionamiento o infrautilización.

Aún puede contar con la configuración de diagnóstico para enviar los registros de NSG a Log Analytics para que las búsquedas, las alertas y los paneles de información guardados continúen funcionando. Los clientes que ya tengan instalada la solución pueden seguir utilizándola hasta nuevo aviso. A partir del 20 de junio, la solución Network Security Group Analytics se quitará de Marketplace y estará disponible a través de la comunidad como [plantilla de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="next-steps"></a>Pasos siguientes
- Vea [Common questions for transition from OMS portal to Azure portal for Log Analytics users](log-analytics-oms-portal-faq.md) (Preguntas frecuentes sobre la transición del portal de OMS a Azure Portal para usuarios de Log Analytics) a fin de obtener instrucciones sobre cómo trasladarse del portal de OMS a Azure Portal.