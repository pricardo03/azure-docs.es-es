---
title: ¿Qué es Azure Security Center? | Microsoft Docs
description: Obtenga información sobre el Centro de seguridad de Azure, sus capacidades clave y cómo funciona.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: rkarlin
ms.openlocfilehash: bd0e517845b9cfcbe6090dff8d656edcca782c83
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126299"
---
# <a name="what-is-azure-security-center"></a>¿Qué es el Centro de seguridad de Azure?
Azure Security Center proporciona administración unificada de la seguridad y protección avanzada contra amenazas para cargas de trabajo en la nube híbrida. Con Security Center, puede aplicar directivas de seguridad en las cargas de trabajo, limitar la exposición a amenazas y detectar y responder a los ataques.

Razones para usar Security Center

- **Administración centralizada de directivas**: garantice el cumplimiento de los requisitos de seguridad normativos o de la empresa administrando las directivas de seguridad de forma centralizada en todas las cargas de trabajo en la nube híbridas.
- **Evaluación continua de la seguridad**: supervise la situación de seguridad de máquinas, redes, almacenamiento y servicios de datos y aplicaciones para detectar posibles problemas de seguridad.
- **Recomendaciones prácticas**: corrija las vulnerabilidades de seguridad antes de que puedan ser usadas por los atacantes mediante recomendaciones de seguridad prácticas y clasificadas en orden de prioridad.
- **Prioridad de alertas e incidentes**: céntrese primero en las amenazas más críticas con la clasificación en orden de prioridad de las alertas e incidentes de seguridad.
- **Defensas avanzadas en la nube**: reduzca las amenazas con acceso Just-In-Time a los puertos de administración y los controles de aplicaciones adaptables que se ejecutan en las máquinas virtuales.
- **Soluciones de seguridad integradas**: recopile, busque y analice los datos de seguridad desde diversos orígenes, incluidas las soluciones de asociados conectadas.

En **Security Center - Overview** (Información general de Security Center) se proporciona una vista rápida de la situación de seguridad de las cargas de trabajo de Azure y que no son de Azure, lo que le permite detectar y evaluar la seguridad de las cargas de trabajo e identificar y mitigar riesgos. El panel integrado proporciona información instantánea sobre alertas de seguridad y vulnerabilidades que requieren atención.

![Información general][1]

## <a name="centralized-policy-management"></a>Administración de directivas centralizada
En la sección **Directiva y cumplimiento** (descrita anteriormente) se proporciona información rápida sobre la cobertura de la suscripción, el cumplimiento de directivas y la situación de seguridad.

### <a name="subscription-coverage"></a>Cobertura de la suscripción
En esta sección se muestra el número total de suscripciones a las que tiene acceso (lectura o escritura) y el nivel de cobertura de Security Center (Estándar o Gratis) con el que se ejecuta una suscripción en:

- **Cobertura (estándar)**: las suscripciones cubiertas se ejecutan con el nivel máximo de protección que ofrece Security Center.
- **Cobertura (gratis)**: las suscripciones cubiertas se ejecutan con el nivel de protección limitado y gratuito que ofrece Security Center.
- **Sin cobertura**: Security Center no supervisa las suscripciones con este estado.

Si selecciona el gráfico, se abre la ventana **Cobertura**. Si selecciona una pestaña (**Sin cobertura**, **Cobertura básica** o **Cobertura estándar**), se proporciona una lista de suscripciones en cada estado. Si selecciona una suscripción en alguna de las pestañas, se ofrece información adicional sobre una suscripción. Esta información permite identificar al propietario de una suscripción y ponerse en contacto con él, a fin de habilitar Security Center o de aumentar la cobertura de la suscripción.

![Cobertura de Security Center][9]

### <a name="policy-compliance"></a>Cumplimiento de directivas
El cumplimiento de directivas lo determinan los factores de cumplimiento de todas las directivas asignadas. La puntuación general de cumplimiento de un grupo de administración, una suscripción o un área de trabajo es la media ponderada de las asignaciones. La media ponderada tiene en cuenta el número de directivas en una única asignación y el número de recursos al que se aplica la asignación.

Por ejemplo, si la suscripción tiene dos máquinas virtuales y tiene asignada una iniciativa con cinco directivas, entonces usted tiene diez valoraciones en la suscripción. Si una de las máquinas virtuales no cumple las dos directivas, la puntuación general de cumplimiento de la asignación de la suscripción es del 80 %.

Esta sección muestra la relación global de cumplimiento y las suscripciones con menor cumplimiento. Si selecciona **Mostrar el cumplimiento de directivas de su entorno**, se abre la ventana **Administración de directivas**. En **Administración de directivas** se muestra la estructura jerárquica de grupos de administración, suscripciones y áreas de trabajo. Aquí se pueden administrar las directivas de seguridad si elige una suscripción o un grupo de administración.

![Administración de directivas][10]

Una directiva de seguridad define la configuración deseada de las cargas de trabajo. Además, ayuda a garantizar el cumplimiento de los requisitos de seguridad normativos o de la empresa. En Security Center, se definen directivas y se adaptan a su tipo de carga de trabajo o la confidencialidad de los datos, al determinar qué controles supervisa y recomienda Security Center. Puede editar la directiva de seguridad en Security Center; para ello, haga clic en una suscripción o en un grupo de administración. También puede usar Azure Policy para crear definiciones, definir directivas adicionales y asignar directivas en los grupos de administración.

Seleccione **Editar configuración >** para editar la configuración siguiente de Security Center en la suscripción, el grupo de administración, el grupo de recursos o el área de trabajo:

- **Recopilación de datos**: determina el aprovisionamiento del agente y la configuración de seguridad de la [colección de datos](security-center-enable-data-collection.md).
- **Notificaciones por correo electrónico**: determina los contactos de seguridad y la configuración de [notificaciones por correo electrónico](security-center-provide-security-contact-details.md).
- **Plan de tarifa**: define la [selección de precios](security-center-pricing.md) gratuitos o estándar. El plan elegido determina qué características de Security Center están disponibles para los recursos del ámbito.
- **Edición de las opciones de configuración de seguridad**: permite consultar y modificar las opciones de configuración del sistema operativo que Security Center evalúa para identificar posibles vulnerabilidades de seguridad.

Para más información, consulte [Integración de las directivas de seguridad de Security Center con Azure Policy](security-center-azure-policy.md).

### <a name="manage-and-govern-your-security-posture"></a>Administración y control de la situación de seguridad
El lado derecho del panel de **Directiva y cumplimiento** proporciona información que puede servir inmediatamente para mejorar su situación de seguridad global. Algunos ejemplos son:

- Definir y asignar directivas de Security Center para revisar y realizar un seguimiento del cumplimiento de los estándares de seguridad
- Poner las alertas de seguridad de Security Center a disposición de un conector SIEM
- Cumplimiento de directivas a lo largo del tiempo

## <a name="continuous-security-assessment"></a>Evaluación continua de la seguridad
La sección Protección de seguridad de recursos de **Security Center - Información general** proporciona una vista rápida de la protección de seguridad de los recursos, mostrando el número de problemas identificados y el estado de seguridad de cada tipo de recurso. La evaluación continua ayuda a detectar posibles problemas de seguridad, por ejemplo, sistemas a los que les faltan actualizaciones de seguridad o puertos de red expuestos.

### <a name="secure-score"></a>Puntuación segura
La puntuación segura de Azure Security Center revisa las recomendaciones de seguridad y les asigna una prioridad, así podrá saber qué recomendaciones implementar primero y le permitirá conocer las vulnerabilidades de seguridad más graves para establecer prioridades en la investigación. Puntuación segura es una herramienta de medición que le ayuda a reforzar la seguridad para lograr una carga de trabajo segura. Para más información, vea [Puntuación segura en Azure Security Center](security-center-secure-score.md).

### <a name="health-monitoring"></a>Supervisión del estado
Si selecciona un tipo de recurso en **Supervisión del estado de los recursos**, se proporciona una lista de recursos y las vulnerabilidades que se han identificado. Los tipos de recursos son proceso y aplicaciones, redes, datos y almacenamiento e identidad y acceso.

Hemos seleccionado **Proceso y aplicaciones**. En **Procesos**, hay cuatro pestañas:

- **Información general**: supervisión y recomendaciones identificadas por Security Center.
- **VM y equipos**: muestra el estado de seguridad actual de todas las máquinas virtuales y equipos.
- **Cloud Services**: lista de todos los roles web y de trabajo que supervisa Security Center.
- **App Services (versión preliminar)**: lista de las aplicaciones web y los entornos de App Service y el estado de seguridad actual de cada uno.

![¿Qué es la supervisión del estado de seguridad?][3]

Para más información, vea [Supervisión del estado de seguridad](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Recomendaciones prácticas
Security Center analiza el estado de seguridad de los recursos de Azure y que no son de Azure para identificar posibles vulnerabilidades de seguridad. Si selecciona **Recomendaciones** en **Recursos**, se proporciona una lista de recomendaciones de seguridad clasificadas en orden de prioridad que le guía por el proceso de solucionar los problemas de seguridad.

![Recomendaciones][4]

Para más información, vea [Administración de recomendaciones de seguridad](security-center-recommendations.md).

### <a name="most-prevalent-recommendations"></a>Recomendaciones más frecuentes
En el lado derecho del panel de **Recursos** se proporciona una lista de las recomendaciones más frecuentes que existen para el mayor número de recursos. Las recomendaciones más frecuentes destacan dónde se debe centrar la atención. Si selecciona la flecha derecha, se proporciona la recomendación con mayor repercusión.

![Recomendaciones más frecuentes][11]

Se trata de la única recomendación con mayor repercusión disponible en su entorno. Resolver esta recomendación mejorará al máximo el nivel de cumplimiento. En este ejemplo, la recomendación es "Aplicar cifrado de discos". Si selecciona **Mejorar su cumplimiento**, se proporciona una descripción de la recomendación y una lista de los recursos afectados.

![Aplicación del cifrado de discos][12]

## <a name="threat-protection"></a>Protección contra amenazas
Esta área proporciona visibilidad de las alertas de seguridad detectadas en los recursos y el nivel de gravedad de las alertas.

### <a name="prioritized-alerts-and-incidents"></a>Alertas e incidentes clasificados por orden de prioridad
Security Center usa inteligencia análisis avanzados e inteligencia global sobre amenazas para detectar ataques entrantes y las actividades posteriores a la infracción. Las alertas se clasifican en orden de prioridad y se agrupan en incidentes, lo que ayuda a centrarse primero en las amenazas más críticas. También puede crear sus propias alertas de seguridad personalizadas.

Si selecciona **Alertas de seguridad por gravedad** o **Alertas de seguridad a lo largo del tiempo**, se proporciona información detallada sobre las alertas.

![Alertas e incidentes clasificados por orden de prioridad][7]

Rápidamente, puede evaluar el ámbito y el impacto de un ataque con una experiencia de investigación visual e interactiva, y usar consultas predefinidas o ad hoc para una exploración más profunda de los datos de seguridad.

Para más información, vea [Administración y respuesta a las alertas de seguridad](security-center-managing-and-responding-alerts.md).

En el lado derecho del panel se proporciona información para ayudarle a priorizar qué alertas debe solucionar primero y comprender dónde están sus vulnerabilidades más comunes:

- **Recursos con más ataques**: recursos específicos que tienen el mayor número de alertas.
- **Alertas más frecuentes**: tipos de alertas que afectan a la mayor cantidad de recursos.

## <a name="just-in-time-vm-access"></a>Acceso Just-In-Time a la máquina virtual
Reduzca la superficie expuesta a ataques de red con el acceso controlado Just-In-Time a los puertos de administración de las máquinas virtuales de Azure, lo que reducirá drásticamente la exposición a ataques por fuerza bruta y otros tipos de ataques de red.

![Acceso Just-In-Time a la máquina virtual][5]

Especifique reglas de cómo los usuarios pueden conectarse a las máquinas virtuales. Cuando sea necesario, se puede solicitar el acceso desde Security Center o a través de PowerShell. Siempre que la solicitud cumpla las reglas, se concede automáticamente acceso en la hora solicitada.

Para más información, consulte [Administrar el acceso a máquina virtual mediante Just-In-Time](security-center-just-in-time.md).

## <a name="adaptive-application-controls"></a>Controles de aplicación adaptables
Bloquee el malware y otras aplicaciones no deseadas aplicando recomendaciones de inclusión en lista blanca adaptadas a sus cargas de trabajo de Azure específicas y basadas en el aprendizaje automático.

![Controles de aplicación adaptables][6]

Revise las reglas de creación de listas blancas de aplicaciones recomendadas generadas por Security Center y haga clic en ellas para aplicarlas, o edite las reglas ya configuradas.

Para más información, vea [Controles de aplicación adaptables](security-center-adaptive-application.md).

## <a name="integrate-your-security-solutions"></a>Integración de las soluciones de seguridad
Recopile, busque y analice los datos de seguridad procedentes de diversos orígenes, incluidas las soluciones de asociados conectadas como firewalls de red y otros servicios de Microsoft, de Security Center.

![Integración de las soluciones de seguridad][8]

Para más información, vea [Integración de soluciones de seguridad](security-center-partner-integration.md).

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a trabajar con el Centro de seguridad, necesita una suscripción a Microsoft Azure. Si no tiene una suscripción, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/).
- Con la suscripción de Azure se habilita el plan de tarifa Gratis de Security Center. Para aprovechar las funcionalidades avanzadas de administración de seguridad y detección de amenazas, debe actualizar al plan de tarifa Estándar. El nivel Estándar es gratuito durante los primeros 60 días. Para más información, consulte la [página de precios de Security Center](https://azure.microsoft.com/pricing/details/security-center/).
- Si está listo para habilitar Security Center Estándar en este momento, la [Guía de inicio para incorporar sus suscripción de Azure en Security Center Estándar](security-center-get-started.md) le guía por los pasos necesarios.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
[9]: ./media/security-center-intro/coverage.png
[10]: ./media/security-center-intro/policy-management.png
[11]: ./media/security-center-intro/highest-impact.png
[12]: ./media/security-center-intro/apply-disk-encryption.png
