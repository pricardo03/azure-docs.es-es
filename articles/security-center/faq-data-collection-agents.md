---
title: 'Preguntas frecuentes sobre Azure Security Center: recopilación de datos y agentes'
description: Preguntas frecuentes sobre recopilación de datos, agentes y áreas de trabajo de Azure Security Center, un producto que ayuda a evitar amenazas, a detectarlas y a responder a ellas
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 8317a13b9ef87679836f55627268deefa4500dce
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599677"
---
# <a name="faq---questions-about-data-collection-agents-and-workspaces"></a>Preguntas frecuentes: preguntas sobre recopilación de datos, agentes y áreas de trabajo

Security Center recopila datos de las máquinas virtuales de Azure, los conjuntos de escalado de máquinas virtuales, los contenedores de IaaS y los equipos que no son de Azure (incluidos los equipos locales) a fin de supervisar las amenazas y vulnerabilidades de seguridad. Los datos se recopilan con Microsoft Monitoring Agent, que lee distintas configuraciones relacionadas con la seguridad y distintos registros de eventos de la máquina y copia los datos en el área de trabajo para analizarlos.


## <a name="am-i-billed-for-azure-monitor-logs-on-the-workspaces-created-by-security-center"></a>¿Se me facturan los registros de Azure Monitor en las áreas de trabajo que creada Security Center?

No. Las áreas de trabajo que crea Security Center, mientras estén configuradas para los registros de Azure Monitor por facturación de nodo, no incurren en gastos de registros de Azure Monitor. La facturación de Security Center siempre se basa en la directiva de seguridad de Security Center y en las soluciones instaladas en un área de trabajo:

- **Nivel Gratis**: Security Center habilita la solución "SecurityCenterFree" en el área de trabajo predeterminada. No se le facturará por el nivel Gratis.

- **Nivel Estándar**: Security Center habilita la solución "Security" en el área de trabajo predeterminada.

Para más información, vea [Precios de Security Center ](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> El plan de tarifa de Log Analytics de áreas de trabajo creadas por Security Center no afecta a la facturación de Security Center.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


## <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>¿Qué hace que una máquina virtual sea apta para el aprovisionamiento automático de la instalación de Microsoft Monitoring Agent?

Las máquinas virtuales Linux o Windows de IaaS son aptas si:

- La extensión Microsoft Monitoring Agent no está instalada actualmente en la máquina virtual.
- El estado de la máquina virtual es en ejecución.
- El [agente de máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) de Windows o Linux está instalado.
- La máquina virtual no se utiliza como dispositivo, es decir, firewall de aplicaciones web o de próxima generación.


## <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>¿Puedo eliminar las áreas de trabajo predeterminadas creadas por Security Center?

**La eliminación del área de trabajo predeterminada no es recomendable.** Security Center utiliza las áreas de trabajo predeterminadas para almacenar los datos de seguridad de las VM. Si elimina un área de trabajo, Security Center no podrá recopilar estos datos y algunas recomendaciones de seguridad y alertas dejarán de estar disponibles.

Para realizar la recuperación, quite Microsoft Monitoring Agent de las VM conectadas al área de trabajo eliminado. Security Center vuelve a instalar el agente y crea nuevas áreas de trabajo predeterminadas.

## <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>¿Cómo puedo usar mi área de trabajo de Log Analytics existente?

Puede seleccionar un área de trabajo de Log Analytics existente para almacenar los datos recopilados por Security Center. Para usar el área de trabajo de Log Analytics existente:

- El área de trabajo debe estar asociada con la suscripción de Azure seleccionada.
- Como mínimo, el usuario debe tener permisos de lectura para obtener acceso al área de trabajo.

Para seleccionar un área de trabajo de Log Analytics existente:

1. En **Security policy – Data Collection** (Directiva de seguridad: recopilación de datos), seleccione **Use another workspace** (Usar otro área de trabajo).

    ![Usar otro área de trabajo][4]

1. En el menú desplegable, seleccione un área de trabajo para almacenar los datos recopilados.

    > [!NOTE]
    > En el menú desplegable solo se muestran las áreas de trabajo a las que tiene acceso y las que están en su suscripción a Azure.

1. Seleccione **Guardar**. Se le pregunta si quiere volver a configurar las máquinas virtuales supervisadas.

    - Haga clic en **No** si quiere que la nueva configuración del área de trabajo **solo se aplique a las máquinas virtuales nuevas**. La nueva configuración del área de trabajo solo se aplica a las nuevas instalaciones de agente, aquellas máquinas virtuales recién detectadas que no tengan instalado Microsoft Monitoring Agent.
    - Haga clic en **Sí** si quiere que la nueva configuración del área de trabajo **se aplique a todas las máquinas virtuales**. Además, cada máquina virtual conectada a un área de trabajo creada por Security Center se reconecta a la nueva área de trabajo de destino.

    > [!NOTE]
    > Si selecciona **Sí**, no elimine las áreas de trabajo creadas por Security Center hasta que todas las máquinas virtuales se hayan vuelto a conectar a la nueva área de trabajo de destino. Esta operación no se lleva a cabo si se elimina un área de trabajo demasiado pronto.

    - Para cancelar la operación, seleccione **Cancelar**.

## ¿Qué ocurre si Microsoft Monitoring Agent ya estaba instalado como una extensión en la VM?<a name="mmaextensioninstalled"></a>

Cuando se instala Monitoring Agent como una extensión, la configuración de extensión permite enviar informes a una sola área de trabajo. Security Center no invalida las conexiones existentes con áreas de trabajo de usuario. Security Center almacenará datos de seguridad de una máquina virtual en un área de trabajo que ya esté conectada, siempre que se haya instalado la solución "Security" o "SecurityCenterFree". Durante este proceso, Security Center puede actualizar la versión de extensión a la versión más reciente.

Para obtener más información, consulte [Aprovisionamiento automático en los casos de una instalación de agente ya existente](security-center-enable-data-collection.md#preexisting).



## ¿Qué ocurre si se instala una instancia de Microsoft Monitoring Agent directamente en la máquina pero no como extensión (Agente directo)?<a name="directagentinstalled"></a>

Si Microsoft Monitoring Agent se instala directamente en la máquina virtual (no como una extensión de Azure), Security Center instalará la extensión de Microsoft Monitoring Agent y puede que la actualice a la versión más reciente.

El agente instalado continuará generando informes para las áreas de trabajo ya configuradas y, además, los generará para el área de trabajo configurada en Security Center (el hospedaje múltiple se admite en las máquinas Windows).

Si el área de trabajo configurada es un área de trabajo de usuario (no el área de trabajo predeterminada de Security Center), debe instalar la solución "Security" o "SecurityCenterFree" para que Security Center empiece a procesar eventos de máquinas virtuales y equipos que notifican a esa área de trabajo.

El hospedaje múltiple del agente todavía no es compatible con las máquinas Linux; por lo tanto, si se detecta una instalación de agente existente, no se producirá el aprovisionamiento automático y no se modificará la configuración de la máquina.

Para las máquinas existentes en suscripciones incorporadas a Security Center antes del 17 de marzo de 2019, cuando se detecte un agente existente, no se instalará la extensión de Microsoft Monitoring Agent y la máquina no se modificará. Para estas máquinas, consulte la recomendación "Resolver incidencias de supervisión de estado del agente en las máquinas" con el fin de resolver las incidencias de instalación del agente en estas máquinas.

Para más información, consulte la sección siguiente [¿Qué ocurre si ya hay un agente directo de System Center Operations Manager o OMS instalado en mi máquina virtual?](#scomomsinstalled)

## ¿Qué ocurre si ya hay un agente de System Center Operations Manager instalado en mi máquina virtual?<a name="scomomsinstalled"></a>

Security Center instalará la extensión de Microsoft Monitoring Agent en paralelo a la versión del agente existente de System Center Operations Manager. El agente existente continuará generando informes con normalidad para el servidor de System Center Operations Manager. Tenga en cuenta que el agente de Operations Manager y Microsoft Monitoring Agent comparten bibliotecas en tiempo de ejecución, las cuales se actualizarán a la versión más reciente durante este proceso. Nota: Si está instalada la versión 2012 del agente de Operations Manager, no active el aprovisionamiento automático (las funcionalidades de administración pueden perderse cuando el servidor de Operations Manager también es de la versión 2012).


## <a name="what-is-the-impact-of-removing-these-extensions"></a>¿Qué impacto tiene quitar estas extensiones?

Si quita la extensión de Microsoft Monitoring, Security Center no será capaz de recopilar datos de seguridad de la VM y algunas recomendaciones de seguridad y las alertas no estarán disponibles. En un plazo de 24 horas, Security Center determina que la VM no tiene la extensión y vuelve a instalarla.


## <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>¿Cómo detengo la instalación automática del agente y la creación del área de trabajo?

Puede desactivar el aprovisionamiento automático en las suscripciones en la directiva de seguridad, pero no es recomendable. La desactivación del aprovisionamiento automático limita las recomendaciones y las alertas de Security Center. Para deshabilitar el aprovisionamiento automático:

1. Si la suscripción está configurada para el nivel Estándar, abra la directiva de seguridad de esa suscripción y seleccione el nivel **Gratis**.

   ![Plan de tarifa][1]

1. Después, desactive el aprovisionamiento automático seleccionando **Desactivar** en la página **Directiva de seguridad: Recopilación de datos**.
   ![Recopilación de datos][2]


## <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>¿Debo rechazar la instalación del agente y la creación del área de trabajo automáticas?

> [!NOTE]
> Si decide rechazar el aprovisionamiento automático, no olvide revisar las secciones sobre [qué implica rechazarlo](#what-are-the-implications-of-opting-out-of-automatic-provisioning) y [los pasos recomendados al rechazarlo](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).

Quizá desee rechazar el aprovisionamiento automático si lo siguiente es aplicable en su caso:

- La instalación automática del agente de Security Center se aplica a toda la suscripción. No se puede aplicar la instalación automática a un subconjunto de máquinas virtuales. Si hay máquinas virtuales críticas en las que no se puede instalar Microsoft Monitoring Agent, debe rechazar el aprovisionamiento automático.
- La instalación de la extensión Microsoft Monitoring Agent (MMA) actualiza la versión del agente. Esto se aplica a un agente directo y a un agente de System Center Operations Manager (en este último, Operations Manager y MMA comparten bibliotecas de tiempo de ejecución comunes, que se actualizarán en el proceso). Si el agente de Operations Manager instalado es de la versión 2012 y se actualiza, las funcionalidades de administración pueden perderse si la versión del servidor de Operations Manager es también 2012. Plantéese rechazar el aprovisionamiento automático en caso de que la versión instalada del agente de Operations Manager sea 2012.
- Si tiene un área de trabajo personalizada externa a la suscripción (centralizada), debe rechazar el aprovisionamiento automático. Puede instalar manualmente la extensión Microsoft Monitoring Agent y conectarla al área de trabajo sin que Security Center invalide la conexión.
- Si desea evitar la creación de varias áreas de trabajo por suscripción y tiene su propia área de trabajo personalizada en la suscripción, tiene dos opciones:

   1. Puede rechazar el aprovisionamiento automático. Tras la migración, establezca la configuración predeterminada del área de trabajo como se describe en [¿Cómo puedo usar mi área de trabajo de Log Analytics existente?](#how-can-i-use-my-existing-log-analytics-workspace)

   1. O bien, puede permitir que la migración se complete, que Microsoft Monitoring Agent se instale en las máquinas virtuales y que estas se conecten al área de trabajo creada. A continuación, seleccione su propia área de trabajo personalizada; para ello, establezca la configuración predeterminada del área de trabajo con la opción de reconfigurar los agentes ya instalados activada. Para más información, consulte [¿Cómo puedo usar mi área de trabajo de Log Analytics existente?](#how-can-i-use-my-existing-log-analytics-workspace)


## <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>¿Qué implica rechazar el aprovisionamiento automático?

Cuando se completa la migración, Security Center no puede recopilar datos de seguridad de la VM y algunas recomendaciones y alertas de seguridad no están disponibles. Si lo rechaza, instale Microsoft Monitoring Agent manualmente. Consulte los [pasos recomendados al rechazarlo](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).


## <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>¿Cuáles son los pasos recomendados al rechazar el aprovisionamiento automático?

Instale la extensión Microsoft Monitoring Agent manualmente para que Security Center pueda recopilar los datos de seguridad de las máquinas virtuales y proporcionar recomendaciones y alertas. Consulte la [instalación del agente de máquina virtual Windows](../virtual-machines/extensions/oms-windows.md) o la [instalación del agente de máquina virtual Linux](../virtual-machines/extensions/oms-linux.md) para obtener instrucciones sobre la instalación.

Puede conectar al agente a cualquier área de trabajo personalizada existente o a la que creara Security Center. Si el área de trabajo personalizada no tiene las soluciones "Security" o "SecurityCenterFree" habilitadas, será necesario aplicar una solución. Para la aplicación, seleccione el área de trabajo personalizada o la suscripción y aplique un plan de tarifa desde la página **Directiva de seguridad: plan de tarifa**.

   ![Plan de tarifa][1]

Security Center habilitará la solución correcta en el área de trabajo en función del plan de tarifa seleccionado.


## ¿Cómo quito extensiones OMS instaladas por Security Center?<a name="remove-oms"></a>

También puede quitar manualmente Microsoft Monitoring Agent. Sin embargo, no es recomendable porque limita las recomendaciones y las alertas de Security Center.

> [!NOTE]
> Si la recopilación de datos está habilitada, Security Center volverá a instalar el agente después de quitarlo.  Debe deshabilitar la recopilación de datos antes de quitar manualmente el agente. Consulte ¿Cómo detengo la instalación automática del agente y la creación del área de trabajo? para obtener instrucciones acerca de la deshabilitación de una colección de datos.

Para quitar manualmente el agente:

1.  En el portal, abra **Log Analytics**.

1.  En la página Log Analytics, seleccione un área de trabajo:

1.  Seleccione las VM que no desea supervisar y seleccione **Desconectar**.

   ![Eliminación del agente][3]

> [!NOTE]
> Si una máquina virtual Linux ya tiene un agente OMS que no es una extensión, la eliminación de la extensión también quita el agente, con lo que es necesario volver a instalarlo.


## <a name="how-do-i-disable-data-collection"></a>¿Cómo se puede deshabilitar la recolección de datos?

El aprovisionamiento automático está desactivado de manera predeterminada. Puede deshabilitar en cualquier momento el aprovisionamiento automático de los recursos desactivando esta opción en la directiva de seguridad. El aprovisionamiento automático es muy recomendable para poder obtener alertas de seguridad y recomendaciones sobre las actualizaciones del sistema, vulnerabilidades del sistema operativo, y la protección de puntos de conexión.

Para deshabilitar la recopilación de datos, [inicie sesión en Azure Portal](https://portal.azure.com), seleccione **Examinar**, **Security Center** y, luego, **Seleccionar directiva**. Seleccione la suscripción en la que quiere deshabilitar el aprovisionamiento automático. Al seleccionar una suscripción, se abre **Directiva de seguridad: recopilación de datos**. En **Autoaprovisionamiento**, seleccione **Desactivar**.


## <a name="how-do-i-enable-data-collection"></a>¿Cómo se puede habilitar la recolección de datos?

Puede habilitar la colección de datos de la suscripción de Azure en la directiva de seguridad. Para habilitar la recopilación de datos, [Inicie sesión en Azure Portal](https://portal.azure.com), seleccione **Examinar**, **Security Center** y, luego, **Seleccionar directiva**. Seleccione la suscripción en la que quiere habilitar el aprovisionamiento automático. Al seleccionar una suscripción, se abre **Directiva de seguridad: recopilación de datos**. En **Autoaprovisionamiento**, seleccione **Activar**.


## <a name="what-happens-when-data-collection-is-enabled"></a>¿Qué sucede cuando se habilita la colección de datos?

Si el aprovisionamiento automático está habilitado, Security Center aprovisiona Microsoft Monitoring Agent en todas las máquinas virtuales de Azure compatibles y en las que se creen. Se recomienda el aprovisionamiento automático pero la instalación manual del agente también está disponible. [Aprenda a instalar la extensión Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

El agente habilita el evento 4688 de creación de procesos y el campo *CommandLine* dentro del evento 4688. El registro de eventos registra los nuevos procesos creados en la VM y los servicios de detección de Security Center los supervisan. Para obtener más información sobre los detalles que se registran para cada nuevo proceso, consulte los [campos de descripción en 4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). El agente también recopila los eventos 4688 creados en la máquina virtual y los almacena en la búsqueda.

El agente también habilita la recopilación de datos para los [controles de aplicación adaptables](security-center-adaptive-application.md), Security Center configura una directiva de AppLocker local en el modo de auditoría para permitir todas las aplicaciones. Esta directiva hará que AppLocker genere eventos que luego Security Center recopilará y aprovechará. Es importante tener en cuenta que esta directiva no se configurará en las máquinas en las que ya se haya configurado una directiva de AppLocker. 

Cuando Security Center detecta actividad sospechosa en la máquina virtual, el cliente recibe una notificación por correo electrónico si se ha proporcionado [información de contacto de seguridad](security-center-provide-security-contact-details.md). También se puede ver una alerta en el panel de alertas de seguridad de Security Center.


## <a name="will-security-center-work-using-an-oms-gateway"></a>¿Security Center funcionará con una puerta de enlace de OMS?

Sí. Azure Security Center aprovecha Azure Monitor para recopilar datos de máquinas virtuales y servidores de Azure mediante Microsoft Monitoring Agent.
Para recopilar los datos, cada máquina virtual y servidor deben conectarse a Internet mediante HTTPS. La conexión puede ser directa, a través de un proxy o a través de la [puerta de enlace de OMS](../azure-monitor/platform/gateway.md).


## <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>¿Microsoft Monitoring Agent afecta al rendimiento de mis servidores?

El agente utiliza una cantidad simbólica de recursos del sistema y apenas tiene impacto en el rendimiento. Para obtener más información sobre el impacto en el rendimiento, y el agente y la extensión, vea la [guía de planeación y las operaciones](security-center-planning-and-operations-guide.md#data-collection-and-storage).


## <a name="where-is-my-data-stored"></a>¿Dónde se almacenan los datos?

Los datos que recopila este agente se almacenan en un área de trabajo de Log Analytics asociada con la suscripción o en una nueva área de trabajo. Para obtener más información, consulte [Seguridad de datos de Azure Security Center](security-center-data-security.md).


<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/use-another-workspace.png