---
title: Retirada del centro de seguridad de las características (julio de 2019) | Microsoft Docs
description: En este artículo se describe características de Security Center que se retirará el 31 de julio de 2019.
services: security-center
author: yoavfrancis
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.date: 4/16/2019
ms.author: yoafr
ms.openlocfilehash: 614dabe842c7fe99da3ddb486c27003c79382ea1
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231397"
---
# <a name="retirement-of-security-center-features-july-2019"></a>Retirada de las características de Security Center (julio de 2019)

Hemos realizado varias [mejoras](https://azure.microsoft.com/updates/?product=security-center) a Azure Security Center durante los últimos 6 meses.  
Con las capacidades mejoradas quitaremos una serie de características redundantes, así como las API relacionadas, desde el centro de seguridad en el 31 de julio de 2019.  

La mayoría de las características retiradas puede reemplazarse por las nuevas características de Azure Security Center o Log Analytics; y algunas características pueden implementarse mediante [Centinela de Azure (versión preliminar)](https://azure.microsoft.com/services/azure-sentinel/), anunciada recientemente.

La lista de características que se va a retirar desde el centro de seguridad incluye:

- [Panel eventos](#menu_events)
- [Entrada de menú de búsqueda](#menu_search)
- [Vínculos clásico de identidad y acceso de vista sobre la identidad y acceso (versión preliminar)](#menu_classicidentity)
- [Botón en el mapa de las alertas de seguridad (versión preliminar) asignan los eventos de seguridad](#menu_securityeventsmap)
- [Reglas de alerta personalizadas (versión preliminar)](#menu_customalerts)
- [Investigar el botón en las alertas de seguridad de protección contra amenazas](#menu_investigate)
- [Un subconjunto de las soluciones de seguridad](#menu_solutions)
- [Editar configuraciones de seguridad para las directivas de seguridad](#menu_securityconfigurations)
- [Seguridad y auditoría panel (usado originalmente en el portal de OMS) para las áreas de trabajo de Log Analytics.](#menu_securityomsdashboard)

El a continuación se proporciona información detallada para cada característica retirada y los pasos que puede seguir usando las características de reemplazo.

## Panel eventos<a name="menu_events"></a>
Security Center usa Microsoft Monitoring Agent para recopilar distintos eventos y configuraciones relacionados con la seguridad en los equipos y almacena esos eventos en los espacios de trabajo. El [panel eventos](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard) permite ver los datos y básicamente proporciona otro punto de entrada a Log Analytics.

A partir de ahora el panel de eventos se va a retirar:

![Pantalla de selección del área de trabajo de eventos][1]

El panel de eventos que aparece una vez que un usuario hace clic en un área de trabajo, también se va a retirar:

![Panel Eventos][2]

### <a name="events-dashboard---new-experience"></a>Panel de eventos - nueva experiencia

Se recomienda utilizar las capacidades nativas de Log Analytics para ver los eventos importantes en sus áreas de trabajo de los clientes.
Si ya ha creado los eventos importantes personalizados desde el centro de seguridad, estos serán accesibles a través de Log analytics -> seleccione área de trabajo -> búsquedas guardadas. Los datos no se pierden ni se puede modificar. Eventos importantes nativos también están disponibles en la misma pantalla.

![Las búsquedas guarda del área de trabajo][3]

## Entrada de menú de búsqueda<a name="menu_search"></a>
Azure Security Center usa actualmente la búsqueda de registros de Azure Monitor para recuperar y analizar los datos de seguridad. Esta pantalla sirve básicamente como una fachada para Log Analytics "[búsqueda](https://docs.microsoft.com/azure/security-center/security-center-search)" página, lo que permite a los usuarios ejecutar consultas de búsqueda en su área de trabajo seleccionada. A partir de ahora esta ventana de búsqueda se va a retirar:

![Página de búsqueda][4]

### <a name="search-menu-entry---new-experience"></a>Entrada de menú: nueva experiencia de búsqueda

Se recomienda usar los clientes **Log Analytics** capacidades nativas para realizar consultas de búsqueda en sus áreas de trabajo. Para ello, puede ir a Log analytics en Azure y seleccione: "Registros":

![Página de registros de log Analytics][5]

## Clásico identidad y acceso (versión preliminar)<a name="menu_classicidentity"></a>
La experiencia de identidad y acceso de "Clásico" de Security Center ha proporcionado una forma para que los clientes ver su identidad y acceso a información relacionada en log analytics. Esto se realiza siguiendo los clics a continuación:

Haga clic en "Vista clásica Identity & Access"

![Página de identidad][6]

Junto con la pantalla de esta página se abriría "panel identidad y acceso":

![Página de identidad: selección del área de trabajo][7]

Al hacer clic en el área de trabajo, abre el panel de análisis de registro "Identity and access" donde los clientes pueden ver identidad & tener acceso a información en su área de trabajo:

![Página de identidad: panel][8]

A partir de ahora todas las tres pantallas anteriores se va a retirar. Los datos seguirán estando disponibles en la solución de seguridad de log analytics y no se puede modificar ni quitar.
Las consultas de Log Analytics originales utilizadas para rellenar el panel identidad y acceso están disponibles en el centro de seguridad [repositorio de GitHub](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards).

### <a name="classic-identity--access-preview---new-experience"></a>Clásica identidad y acceso (versión preliminar): nueva experiencia
Mientras el panel de análisis de registro ha proporcionado información en solo un área de trabajo determinado, las capacidades nativas de Security Center proporcionan visibilidad en todas las suscripciones y todas las áreas de trabajo asociados a ellos, en una forma sencilla de-para usar la vista que le permite centrarse en lo que de importante, según la puntuación segura de sus en las recomendaciones de identidad y acceso.
Pueden dirigirse a todas las características de la identidad y el panel de análisis de registro de acceso seleccionando "Identidad y acceso (versión preliminar)" en el centro de seguridad:

![Retirada de la experiencia de página identidad - clásico][9]

## Mapa de eventos de seguridad<a name="menu_securityeventsmap"></a>
Security Center proporciona un [mapa](https://docs.microsoft.com/azure/security-center/security-center-threat-intel) que le ayuda a identificar las amenazas de seguridad en el entorno. El botón 'Ir a mapa de eventos de seguridad' en la que se asignan conduce a un panel que permite ver los eventos de seguridad sin procesar en el área de trabajo seleccionada.
El botón, junto con el panel por área de trabajo, se quitará después de la degradación.

![Botón de mapa: las alertas de seguridad][10]

Hoy en día al hacer clic en "Ir a la asignación de eventos de seguridad" se abre el panel de inteligencia de amenazas. El panel de inteligencia sobre amenazas que se va a retirar.  

![Panel Inteligencia de amenazas][11]

Al elegir un área de trabajo para ver su panel de inteligencia de amenazas, la seguridad de alertas pantalla map(Preview) *en Log Analytics* se abre. Esta pantalla se va a retirar.

![Mapa de las alertas de seguridad en Log Analytics][12]

Los datos existentes seguirán estando disponibles en la solución de seguridad de log analytics y no se puede modificar ni quitar.
Las consultas de Log Analytics originales utilizadas para rellenar el panel de inteligencia de amenazas están disponibles en el centro de seguridad [repositorio de GitHub](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards).

### <a name="security-events-map---new-experience"></a>Mapa de eventos de seguridad: nueva experiencia
Le animamos a nuestros clientes para usar la funcionalidad de asignación de las alertas integrada en el centro de seguridad: "(versión preliminar) asignan las alertas de seguridad". Esto proporciona una experiencia optimizada y funciona en todas las suscripciones y áreas de trabajo asociados, lo que permite una macro ver a través de su entorno y se centra en un área de trabajo.

## Reglas de alerta personalizadas (versión preliminar)<a name="menu_customalerts"></a>
La experiencia de alertas personalizadas será [retirado](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) el 30 de junio de 2019, debido a la retirada de la infraestructura subyacente se basa en. En el período de tiempo hasta el desuso, los usuarios podrán editar reglas de alerta personalizadas existentes pero no podrá agregar otros nuevos. Se aconseja a los usuarios para habilitar [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) con la incorporación de un solo clic para migrar sus alertas existentes y crear nuevos o volver a crear también sus alertas con Azure Monitor automáticamente alertas de registro.

Para mantener las alertas existentes y migrarlas a Azure Sentinel, inicie Azure Sentinel. Como primer paso, seleccione el área de trabajo donde se almacenan las alertas personalizadas y, a continuación, seleccione el elemento de menú 'Analytics' migrar automáticamente las alertas.

![Alertas personalizadas][13]

Los clientes que no le interesados la incorporación a Azure Sentinel se anima a volver a crear sus alertas con alertas de registro de Azure Monitor. Para obtener instrucciones, consulte: [crear, ver y administrar alertas de registro mediante Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log). Para obtener instrucciones sobre cómo crear alertas del registro, consulte: [Alertas de registro en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

Para obtener más información sobre la retirada de alertas personalizadas, consulte el [documentación de alertas de Security Center personalizado](https://docs.microsoft.com/azure/security-center/security-center-custom-alert).

## Investigación de alertas de seguridad<a name="menu_investigate"></a>
[La característica de investigación](https://docs.microsoft.com/azure/security-center/security-center-investigation) en Security Center le permite evaluar, entender el ámbito y realizar un seguimiento de la causa raíz de un incidente de seguridad potenciales. Esta característica se quitará del centro de seguridad tal como se ha reemplazado con una experiencia mejorada en [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/).

![Incidente de seguridad][14]

Al hacer clic en el botón "Investigar" anterior, "investigación (versión preliminar)" en Log Analytics se abre el panel. El panel de investigación que se va a retirar.  
Los datos existentes seguirán estando disponibles en la solución de seguridad de Log Analytics y no se puede modificar ni quitar.

![Panel de investigación en Log Analytics][15]

### <a name="investigation---new-experience"></a>Investigación: nueva experiencia

Le animamos a nuestros clientes para incorporar [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) para una experiencia enriquecida de investigación, con tecnología de las alertas a la caza de característica. Sentinel Azure proporciona herramientas de búsqueda y consulta de búsqueda eficaz buscando amenazas de seguridad a través de orígenes de datos de su organización.  

## Subconjunto de las soluciones de seguridad<a name="menu_solutions"></a>

Security Center tiene la capacidad de habilitar [soluciones de seguridad de Azure integradas](https://docs.microsoft.com/azure/security-center/security-center-partner-integration). Las siguientes soluciones de asociados se quitan y se admite en [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/), junto con incluso más orígenes de datos.

- Firewall de próxima generación y soluciones de Firewall de aplicaciones de Web (Azure Sentinel [documentación](https://docs.microsoft.com/azure/sentinel/connect-data-sources))
- Integración de soluciones de seguridad que admiten el formato de evento común (CEF) (Azure Sentinel [documentación](https://docs.microsoft.com/azure/sentinel/connect-common-event-format))
- Microsoft Advanced Threat Analytics (centinela Azure [documentación](https://docs.microsoft.com/azure/sentinel/connect-azure-atp))
- Azure AD Identity Protection (centinela Azure [documentación](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection))

Después de la degradación, los usuarios no puedan agregar nuevas o modificar los existentes soluciones conectadas de los tipos mencionados anteriormente, desde la interfaz de usuario y la API.
Las soluciones conectadas existentes, se recomienda mover a Azure Sentinel al final de este período.

![Soluciones de centros de seguridad][16]

## Editar configuraciones de seguridad para las directivas de seguridad<a name="menu_securityconfigurations"></a>
Azure Security Center supervisa las configuraciones de seguridad mediante un conjunto de [más de 150 reglas recomendadas](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) para proteger el sistema operativo, incluidas, entre otras, las relacionadas con firewalls, auditoría y directivas de contraseña. Si se detecta que la configuración de una maquina es vulnerable, Security Center genera una recomendación de seguridad. El [pantalla de configuración de seguridad de edición](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config) permite a los clientes personalizar la configuración de seguridad predeterminada del sistema operativo en Security Center.

Esta característica ha estado en versión preliminar y se va a retirar.

![Edición de las opciones de configuración de seguridad][17]

### <a name="edit-security-configurations---new-experience"></a>Editar configuraciones de seguridad: nueva experiencia

Security Center admitirá el [agente invitado de la configuración](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration) en un futuro próximo, lo que permite un conjunto de características mucho más completo - incluida la compatibilidad para la integración con la configuración de invitado con las directivas de Azure (y sistemas operativos adicionales directivas de invitado). También proporcionará la capacidad de controlar a escala y aplicar automáticamente en nuevos recursos.

## Panel seguridad y auditoría (usado originalmente en el portal de OMS) para las áreas de trabajo de Log Analytics<a name="menu_securityomsdashboard"></a>

El panel de seguridad en Log analytics proporciona una introducción al área de trabajo de eventos de seguridad importantes y las amenazas, un mapa de inteligencia sobre amenazas y evaluación de la identidad y acceso de eventos de seguridad que se guardan en el área de trabajo. El panel se quitará en el futuro. Como ya se han recomendado en el panel de interfaz de usuario, los usuarios se recomienda usar Azure Security Center a partir de ahora.

![Panel de seguridad de log analytics][18]

### <a name="security--audit-dashboard---new-experience"></a>Panel seguridad y auditoría: nueva experiencia
Nuestros clientes se recomienda usar Azure Security Center, que proporciona la misma información general de seguridad a través de varias suscripciones y áreas de trabajo asociados con ellos, junto con un conjunto de características más completo.

Las consultas de Log Analytics originales utilizadas para rellenar la seguridad y auditoría panel están disponibles en el centro de seguridad [repositorio de GitHub](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards).

## <a name="next-steps"></a>Pasos siguientes
- Más información acerca de [Azure Security Center](https://docs.microsoft.com/azure/security-center/)
- Obtenga más información sobre [Centinela de Azure](https://docs.microsoft.com/azure/sentinel)

<!--Image references - events-->
[1]: ./media/security-center-features-retirement-july2019/asc_events_dashboard.png
[2]: ./media/security-center-features-retirement-july2019/asc_events_dashboard_inner.png
[3]: ./media/security-center-features-retirement-july2019/workspace_saved_searches.png
<!--Image references - search-->
[4]: ./media/security-center-features-retirement-july2019/asc_search.png
[5]: ./media/security-center-features-retirement-july2019/workspace_logs.png
<!--Image references - classic identity and access-->
[6]: ./media/security-center-features-retirement-july2019/asc_identity.png
[7]: ./media/security-center-features-retirement-july2019/asc_identity_workspace_selection.png
[8]: ./media/security-center-features-retirement-july2019/loganalytics_dashboard_identity.png
[9]: ./media/security-center-features-retirement-july2019/asc_identity_nobuttonhighlight.png
<!--Image references - alerts map-->
[10]: ./media/security-center-features-retirement-july2019/asc_security_alerts_map.png
[11]: ./media/security-center-features-retirement-july2019/asc_threat_intellignece_dashboard.png
[12]: ./media/security-center-features-retirement-july2019/loganalytics_security_alerts_map.png
<!--Image references - custom alerts-->
[13]: ./media/security-center-features-retirement-july2019/asc_custom_alerts.png
<!--Image references - Investigation-->
[14]: ./media/security-center-features-retirement-july2019/asc-security-incident.png
[15]: ./media/security-center-features-retirement-july2019/loganalytics_investigation_dashboard.png
<!--Image references - Solutions-->
[16]: ./media/security-center-features-retirement-july2019/asc_security_solutions.png
<!--Image references - Edit security configurations-->
[17]: ./media/security-center-features-retirement-july2019/asc_edit_security_configurations.png
<!--Image references - Security dashboard in log analytics-->
[18]: ./media/security-center-features-retirement-july2019/loganalytics_security_dashboard.png
