---
title: Preguntas más frecuentes sobre Azure Security Center | Microsoft Docs
description: Estas preguntas más frecuentes responden a las preguntas sobre el Centro de seguridad de Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: v-mohabe
ms.openlocfilehash: 6e41453a096f4812b9ba541a2693b9c9b975b7c9
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779069"
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Preguntas más frecuentes sobre el Centro de seguridad de Azure
Estas preguntas más frecuentes responden a preguntas sobre Azure Security Center, un servicio que le ayuda a evitar y detectar amenazas y a responder a las mismas con mayor visibilidad y control sobre la seguridad de los recursos de Microsoft Azure.

> [!NOTE]
> Desde primeros de junio de 2017, Security Center usará Microsoft Monitoring Agent para recopilar y almacenar datos. Para obtener más información, consulte [Migración de la plataforma de Azure Security Center](security-center-platform-migration.md). La información de este artículo representa la funcionalidad de Security Center después de la transición a Microsoft Monitoring Agent.
>
>

## <a name="general-questions"></a>Preguntas generales
### <a name="what-is-azure-security-center"></a>¿Qué es el Centro de seguridad de Azure?
El Centro de seguridad de Azure ayuda a evitar, detectar y responder a amenazas con más visibilidad y control de la seguridad en sus recursos de Azure. Proporciona administración de directivas y supervisión de la seguridad integrada en las suscripciones, ayuda a detectar las amenazas que podrían pasar desapercibidas y funciona con un amplio ecosistema de soluciones de seguridad.

### <a name="how-do-i-get-azure-security-center"></a>¿Cómo puedo obtener el Centro de seguridad de Azure?
Azure Security Center se habilita con la suscripción de Microsoft Azure y se accede a él desde el [Portal de Azure](https://azure.microsoft.com/features/azure-portal/). ([Inicie sesión en el portal](https://portal.azure.com), seleccione **Examinar** y desplácese al **Security Center**).  

## <a name="billing"></a>Facturación
### <a name="how-does-billing-work-for-azure-security-center"></a>¿Cómo funciona la facturación para el Centro de seguridad de Azure?
Security Center se ofrece en dos niveles:

El nivel **Gratis** permite ver el estado de seguridad de los recursos de Azure, la directiva de seguridad básica, las recomendaciones de seguridad, y la integración con los productos y servicios de seguridad de los asociados.

En el **nivel Estándar** se agregan funcionalidades de detección de amenazas avanzada, como inteligencia de amenazas, análisis del comportamiento, detección de anomalías, incidentes de seguridad e informes de atribución de amenazas. Puede iniciar una evaluación gratuita de nivel Estándar. Para actualizarlo, seleccione el [plan de tarifa](https://docs.microsoft.com/azure/security-center/security-center-pricing) de la directiva de seguridad. Para más información, consulte la [página de precios](https://azure.microsoft.com/pricing/details/security-center/).

### <a name="how-can-i-track-who-in-my-organization-performed-pricing-tier-changes-in-azure-security-center"></a>¿Cómo puedo realizar un seguimiento de las personas de mi organización que realizaron cambios en el plan de tarifa en Azure Security Center?
Dado que una suscripción de Azure puede tener varios administradores con permisos para cambiar el plan de tarifa, es posible que un usuario quiera saber quién realizó el cambio en el plan de tarifa. Para ello, se puede usar el registro de actividad de Azure. Para ver más instrucciones, vaya [aquí](https://techcommunity.microsoft.com/t5/Security-Identity/Tracking-Changes-in-the-Pricing-Tier-for-Azure-Security-Center/td-p/390832).

## <a name="permissions"></a>Permisos
Azure Security Center usa el [control de acceso basado en roles (RBAC)](../role-based-access-control/role-assignments-portal.md), que proporciona [roles integrados](../role-based-access-control/built-in-roles.md) que se pueden asignar a usuarios, grupos y servicios de Azure.

Security Center evalúa la configuración de los recursos para identificar problemas de seguridad y vulnerabilidades. En Security Center, solo se muestra información relacionada con un recurso cuando tiene asignado el rol de Propietario, Colaborador o Lector a la suscripción o grupo de recursos al que pertenece un recurso.

Consulte [Permisos en Azure Security Center](security-center-permissions.md) para más información sobre los roles y las acciones permitidas en Security Center.

## <a name="data-collection-agents-and-workspaces"></a>Recopilación de datos, agentes y áreas de trabajo
Security Center recopila datos de las máquinas virtuales de Azure (VM), los conjuntos de escalado de máquinas virtuales (VMSS), los contenedores de IaaS y de los equipos que no son de Azure (incluidos los equipos locales) para supervisar las amenazas y vulnerabilidades de seguridad. Los datos se recopilan con Microsoft Monitoring Agent, que lee distintas configuraciones relacionadas con la seguridad y distintos registros de eventos de la máquina y copia los datos en el área de trabajo para analizarlos.

### <a name="am-i-billed-for-azure-monitor-logs-on-the-workspaces-created-by-security-center"></a>¿Se me facturan los registros de Azure Monitor en las áreas de trabajo que creada Security Center?
No. Las áreas de trabajo que crea Security Center, mientras estén configuradas para los registros de Azure Monitor por facturación de nodo, no incurren en gastos de registros de Azure Monitor. La facturación de Security Center siempre se basa en la directiva de seguridad de Security Center y en las soluciones instaladas en un área de trabajo:

- **Nivel Gratis**: Security Center habilita la solución "SecurityCenterFree" en el área de trabajo predeterminada. No se le facturará por el nivel Gratis.
- **Nivel Estándar**: Security Center habilita la solución "Security" en el área de trabajo predeterminada.

Para más información, vea [Precios de Security Center ](https://azure.microsoft.com/pricing/details/security-center/). La página de precios trata los cambios en el almacenamiento de datos de seguridad y la facturación prorrateada a partir de junio de 2017.

> [!NOTE]
> El plan de tarifa de Log Analytics de áreas de trabajo creadas por Security Center no afecta a la facturación de Security Center.
>
>

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

### <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>¿Qué hace que una máquina virtual sea apta para el aprovisionamiento automático de la instalación de Microsoft Monitoring Agent?
Las máquinas virtuales Linux o Windows de IaaS son aptas si:

- La extensión Microsoft Monitoring Agent no está instalada actualmente en la máquina virtual.
- El estado de la máquina virtual es en ejecución.
- El [agente de máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) de Windows o Linux está instalado.
- La máquina virtual no se utiliza como dispositivo, es decir, firewall de aplicaciones web o de próxima generación.

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>¿Puedo eliminar las áreas de trabajo predeterminadas creadas por Security Center?
**La eliminación del área de trabajo predeterminada no es recomendable.** Security Center utiliza las áreas de trabajo predeterminadas para almacenar los datos de seguridad de las VM.  Si elimina un área de trabajo, Security Center no podrá recopilar estos datos y algunas recomendaciones de seguridad y alertas dejarán de estar disponibles.

Para realizar la recuperación, quite Microsoft Monitoring Agent de las VM conectadas al área de trabajo eliminado. Security Center vuelve a instalar el agente y crea nuevas áreas de trabajo predeterminadas.

### <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>¿Cómo puedo usar mi área de trabajo de Log Analytics existente?

Puede seleccionar un área de trabajo de Log Analytics existente para almacenar los datos recopilados por Security Center. Para usar el área de trabajo de Log Analytics existente:

- El área de trabajo debe estar asociada con la suscripción de Azure seleccionada.
- Como mínimo, el usuario debe tener permisos de lectura para obtener acceso al área de trabajo.

Para seleccionar un área de trabajo de Log Analytics existente:

1. En **Security policy – Data Collection** (Directiva de seguridad: recopilación de datos), seleccione **Use another workspace** (Usar otro área de trabajo).

   ![Usar otro área de trabajo][5]

2. En el menú desplegable, seleccione un área de trabajo para almacenar los datos recopilados.

   > [!NOTE]
   > En el menú desplegable solo se muestran las áreas de trabajo a las que tiene acceso y las que están en su suscripción a Azure.
   >
   >

3. Seleccione **Guardar**.
4. Tras hacer clic en **Guardar**, se le preguntará si quiere reconfigurar las máquinas virtuales supervisadas.

   - Haga clic en **No** si quiere que la nueva configuración del área de trabajo **solo se aplique a las máquinas virtuales nuevas**. La nueva configuración del área de trabajo solo se aplica a las nuevas instalaciones de agente, aquellas máquinas virtuales recién detectadas que no tengan instalado Microsoft Monitoring Agent.
   - Haga clic en **Sí** si quiere que la nueva configuración del área de trabajo **se aplique a todas las máquinas virtuales**. Además, cada máquina virtual conectada a un área de trabajo creada por Security Center se reconecta a la nueva área de trabajo de destino.

   > [!NOTE]
   > Si hace clic en Sí, no debe eliminar las áreas de trabajo creadas por Security Center hasta que todas las máquinas virtuales se hayan reconectado al nuevo área de trabajo de destino. Esta operación no se lleva a cabo si se elimina un área de trabajo demasiado pronto.
   >
   >

   - Haga clic en **Cancelar** para cancelar la operación.

### ¿Qué ocurre si Microsoft Monitoring Agent ya estaba instalado como una extensión en la VM?<a name="mmaextensioninstalled"></a>
Cuando se instala Monitoring Agent como una extensión, la configuración de extensión permite enviar informes a una sola área de trabajo. Security Center no invalida las conexiones existentes con áreas de trabajo de usuario. Security Center almacenará datos de seguridad de una máquina virtual en un área de trabajo que ya esté conectada, siempre que se haya instalado la solución "Security" o "SecurityCenterFree". Durante este proceso, Security Center puede actualizar la versión de extensión a la versión más reciente.

Para obtener más información, consulte [Aprovisionamiento automático en los casos de una instalación de agente ya existente](security-center-enable-data-collection.md#preexisting).


### ¿Qué ocurre si tenía una instancia de Microsoft Monitoring Agent instalada directamente en la máquina, pero no como una extensión (agente directo)?<a name="directagentinstalled"></a>
Si Microsoft Monitoring Agent se instala directamente en la máquina virtual (no como una extensión de Azure), Security Center instalará la extensión de Microsoft Monitoring Agent y puede que la actualice a la versión más reciente.
El agente instalado continuará generando informes para las áreas de trabajo ya configuradas y, además, los generará para el área de trabajo configurada en Security Center (el hospedaje múltiple se admite en las máquinas Windows).
Si el área de trabajo configurada es un área de trabajo de usuario (no el área de trabajo predeterminada de Security Center), deberá instalar la solución "Security" o "SecurityCenterFree" para que Security Center empiece a procesar eventos desde máquinas virtuales y equipos que generan informes para esa área de trabajo.

El hospedaje múltiple del agente todavía no es compatible con las máquinas Linux; por lo tanto, si se detecta una instalación de agente existente, no se producirá el aprovisionamiento automático y no se modificará la configuración de la máquina.

Para las máquinas existentes en suscripciones incorporadas a Security Center antes del 17-03-2019, cuando se detecte un agente existente, no se instalará la extensión de Microsoft Monitoring Agent y la máquina no se modificará. Para estas máquinas, consulte la recomendación "Resolver incidencias de supervisión de estado del agente en las máquinas" con el fin de resolver las incidencias de instalación del agente en estas máquinas.

 Para más información, consulte la sección siguiente [¿Qué ocurre si ya hay un agente directo de SCOM o OMS instalado en mi máquina virtual?](#scomomsinstalled)

### ¿Qué ocurre si un agente de System Center Operations Manager (SCOM) ya está instalado en mi máquina virtual?<a name="scomomsinstalled"></a>
Security Center instalará la extensión de Microsoft Monitoring Agent en paralelo a la versión del agente existente de System Center Operations Manager. El agente existente de SCOM continuará generando informes con normalidad para el servidor de System Center Operations Manager. Tenga en cuenta que el agente de System Center Operations Manager y Microsoft Monitoring Agent comparten bibliotecas en tiempo de ejecución, las cuales se actualizarán a la versión más reciente durante este proceso. Nota: Si se instala la versión 2012 del agente de System Center Operations Manager, no active el aprovisionamiento automático (las funcionalidades de administración pueden perderse cuando el servidor de System Center Operations Manager también es de la versión 2012).

### <a name="what-is-the-impact-of-removing-these-extensions"></a>¿Qué impacto tiene quitar estas extensiones?
Si quita la extensión de Microsoft Monitoring, Security Center no será capaz de recopilar datos de seguridad de la VM y algunas recomendaciones de seguridad y las alertas no estarán disponibles. En un plazo de 24 horas, Security Center determina que la VM no tiene la extensión y vuelve a instalarla.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>¿Cómo detengo la instalación automática del agente y la creación del área de trabajo?
Puede desactivar el aprovisionamiento automático en las suscripciones en la directiva de seguridad, pero no es recomendable. La desactivación del aprovisionamiento automático limita las recomendaciones y las alertas de Security Center. Para deshabilitar el aprovisionamiento automático:

1. Si la suscripción está configurada para el nivel Estándar, abra la directiva de seguridad de esa suscripción y seleccione el nivel **Gratis**.

   ![Plan de tarifa][1]

2. Después, desactive el aprovisionamiento automático seleccionando **Desactivar** en la hoja **Security policy - Data collection** (Directiva de seguridad: Recopilación de datos).
   ![Recopilación de datos][2]

### <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>¿Debo rechazar la instalación del agente y la creación del área de trabajo automáticas?

> [!NOTE]
> Si decide rechazar el aprovisionamiento automático, no olvide revisar las secciones sobre [qué implica rechazarlo](#what-are-the-implications-of-opting-out-of-automatic-provisioning) y [los pasos recomendados al rechazarlo](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).
>
>

Quizá desee rechazar el aprovisionamiento automático si lo siguiente es aplicable en su caso:

- La instalación automática del agente de Security Center se aplica a toda la suscripción.  No se puede aplicar la instalación automática a un subconjunto de máquinas virtuales. Si hay máquinas virtuales críticas en las que no se puede instalar Microsoft Monitoring Agent, debe rechazar el aprovisionamiento automático.
- La instalación de la extensión Microsoft Monitoring Agent (MMA) actualiza la versión del agente. Esto se aplica a un agente directo y a un agente SCOM (en este último, SCOM y MMA comparten bibliotecas de tiempo de ejecución comunes, que se actualizarán en el proceso). Si el agente de SCOM instalado es la versión 2012 y se actualiza, las funcionalidades de administración pueden perderse cuando la versión del servidor SCOM sea también 2012. Considere rechazar el aprovisionamiento automático en caso de que la versión instalada del agente de SCOM sea 2012.
- Si tiene un área de trabajo personalizada externa a la suscripción (centralizada) debe rechazar el aprovisionamiento automático. Puede instalar manualmente la extensión Microsoft Monitoring Agent y conectarla al área de trabajo sin que Security Center invalide la conexión.
- Si desea evitar la creación de varias áreas de trabajo por suscripción y tiene su propia área de trabajo personalizada en la suscripción, tiene dos opciones:

   1. Puede rechazar el aprovisionamiento automático. Tras la migración, establezca la configuración predeterminada del área de trabajo como se describe en [¿Cómo puedo usar mi área de trabajo de Log Analytics existente?](#how-can-i-use-my-existing-log-analytics-workspace)
   2. O bien, puede permitir que la migración se complete, que Microsoft Monitoring Agent se instale en las máquinas virtuales y que estas se conecten al área de trabajo creada. A continuación, seleccione su propia área de trabajo personalizada; para ello, establezca la configuración predeterminada del área de trabajo con la opción de reconfigurar los agentes ya instalados activada. Para más información, consulte [¿Cómo puedo usar mi área de trabajo de Log Analytics existente?](#how-can-i-use-my-existing-log-analytics-workspace)

### <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>¿Qué implica rechazar el aprovisionamiento automático?
Una vez terminada la migración, Security Center no será capaz de recopilar datos de seguridad de la máquina virtual y algunas recomendaciones de seguridad y las alertas no estarán disponibles. Si lo rechaza, deberá instalar Microsoft Monitoring Agent manualmente. Consulte los [pasos recomendados al rechazarlo](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).

### <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>¿Cuáles son los pasos recomendados al rechazar el aprovisionamiento automático?

Debe instalar la extensión Microsoft Monitoring Agent manualmente para que Security Center pueda recopilar los datos de seguridad de las máquinas virtuales y proporcionar recomendaciones y alertas. Consulte la [instalación del agente de máquina virtual Windows](../virtual-machines/extensions/oms-windows.md) o la [instalación del agente de máquina virtual Linux](../virtual-machines/extensions/oms-linux.md) para obtener instrucciones sobre la instalación.

Puede conectar al agente a cualquier área de trabajo personalizada existente o a la que creara Security Center. Si el área de trabajo personalizada no tiene las soluciones "Security" o "SecurityCenterFree" habilitadas, será necesario aplicar una solución. Para la aplicación, seleccione el área de trabajo personalizada o la suscripción y aplique un plan de tarifa desde la hoja **Directiva de seguridad: plan de tarifa**.

   ![Plan de tarifa][1]

Security Center habilitará la solución correcta en el área de trabajo en función del plan de tarifa seleccionado.

### ¿Cómo quito extensiones OMS instaladas por Security Center?<a name="remove-oms"></a>
También puede quitar manualmente Microsoft Monitoring Agent. Sin embargo, no es recomendable porque limita las recomendaciones y las alertas de Security Center.

> [!NOTE]
> Si la recopilación de datos está habilitada, Security Center volverá a instalar el agente después de quitarlo.  Debe deshabilitar la recopilación de datos antes de quitar manualmente el agente. Consulte ¿Cómo detengo la instalación automática del agente y la creación del área de trabajo? para obtener instrucciones acerca de la deshabilitación de una colección de datos.
>
>

Para quitar manualmente el agente:

1.  En el portal, abra **Log Analytics**.
2.  En la hoja Log Analytics, seleccione un área de trabajo:
3.  Seleccione las VM que no desea supervisar y seleccione **Desconectar**.

   ![Eliminación del agente][3]

> [!NOTE]
> Si una VM Linux ya tiene un agente OMS que no es una extensión, la eliminación de la extensión también quita el agente y el cliente tiene que volver a instalarlo.
>
>
### <a name="how-do-i-disable-data-collection"></a>¿Cómo se puede deshabilitar la recolección de datos?
El aprovisionamiento automático está desactivado de manera predeterminada. Puede deshabilitar en cualquier momento el aprovisionamiento automático de los recursos desactivando esta opción en la directiva de seguridad. El aprovisionamiento automático es muy recomendable para poder obtener alertas de seguridad y recomendaciones sobre las actualizaciones del sistema, vulnerabilidades del sistema operativo y la protección de puntos de conexión.

Para deshabilitar la recopilación de datos, [inicie sesión en Azure Portal](https://portal.azure.com), seleccione **Examinar**, **Security Center** y, luego, **Seleccionar directiva**. Seleccione la suscripción en la que quiere deshabilitar el aprovisionamiento automático. Al seleccionar una suscripción, se abre **Directiva de seguridad: recopilación de datos**. En **Autoaprovisionamiento**, seleccione **Desactivar**.

### <a name="how-do-i-enable-data-collection"></a>¿Cómo se puede habilitar la recolección de datos?
Puede habilitar la colección de datos de la suscripción de Azure en la directiva de seguridad. Para habilitar la recopilación de datos, [Inicie sesión en Azure Portal](https://portal.azure.com), seleccione **Examinar**, **Security Center** y, luego, **Seleccionar directiva**. Seleccione la suscripción en la que quiere habilitar el aprovisionamiento automático. Al seleccionar una suscripción, se abre **Directiva de seguridad: recopilación de datos**. En **Autoaprovisionamiento**, seleccione **Activar**.

### <a name="what-happens-when-data-collection-is-enabled"></a>¿Qué sucede cuando se habilita la colección de datos?
Si el aprovisionamiento automático está habilitado, Security Center aprovisiona Microsoft Monitoring Agent en todas las máquinas virtuales de Azure compatibles y en las que se creen. El aprovisionamiento automático está muy recomendado, pero la instalación manual del agente también está disponible. [Aprenda a instalar la extensión Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

El agente habilita el evento 4688 de creación de procesos y el campo *CommandLine* dentro del evento 4688. El registro de eventos registra los nuevos procesos creados en la VM y los servicios de detección de Security Center los supervisan. Para obtener información sobre los detalles que se registran para cada nuevo proceso, consulte los [campos de descripción en 4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). El agente también recopila los eventos 4688 creados en la máquina virtual y los almacena en la búsqueda.

El agente también habilita la recopilación de datos para los [controles de aplicación adaptables](security-center-adaptive-application.md), Security Center configura una directiva de AppLocker local en el modo de auditoría para permitir todas las aplicaciones. Con esto, AppLocker generará eventos que luego Security Center recopilará y aprovechará. Es importante tener en cuenta que esta directiva no se configurará en las máquinas en las que ya se haya configurado una directiva de AppLocker. 

Cuando Security Center detecta actividad sospechosa en la máquina virtual, el cliente recibe una notificación por correo electrónico si se ha proporcionado [información de contacto de seguridad](security-center-provide-security-contact-details.md). También se puede ver una alerta en el panel de alertas de seguridad de Security Center.

### <a name="will-security-center-work-using-an-oms-gateway"></a>¿Security Center funcionará con una puerta de enlace de OMS?
Sí. Azure Security Center aprovecha Azure Monitor para recopilar datos de máquinas virtuales y servidores de Azure mediante Microsoft Monitoring Agent.
Para recopilar los datos, cada máquina virtual y servidor deben conectarse a Internet mediante HTTPS. La conexión puede ser directa, a través de un proxy o a través de la [puerta de enlace de OMS](../azure-monitor/platform/gateway.md).

### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>¿Microsoft Monitoring Agent afecta al rendimiento de mis servidores?
El agente utiliza una cantidad simbólica de recursos del sistema y apenas tiene impacto en el rendimiento. Para obtener más información sobre el impacto en el rendimiento, y el agente y la extensión, vea la [guía de planeación y las operaciones](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>¿Dónde se almacenan los datos?
Los datos que recopila este agente se almacenan en un área de trabajo de Log Analytics asociada con la suscripción o en una nueva área de trabajo. Para obtener más información, consulte [Seguridad de datos de Azure Security Center](security-center-data-security.md).

## Clientes de registros de Azure Monitor existentes<a name="existingloganalyticscust"></a>

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>¿Invalida Security Center las conexiones existentes entre VM y áreas de trabajo?
Si una VM ya tiene Microsoft Monitoring Agent instalado como una extensión de Azure, Security Center no invalida la conexión de área de trabajo existente. En su lugar, Security Center usa el área de trabajo existente. La máquina virtual se protegerá siempre que se haya instalado la solución "Security" o "SecurityCenterFree" en el área de trabajo para la que genera informes. 

Una solución de Security Center se instala en el área de trabajo seleccionada en la pantalla Colección de datos si todavía no está presente y la solución solo se aplica a las máquinas virtuales pertinentes. Cuando se agrega una solución, se implementa automáticamente de forma predeterminada en todos los agentes de Windows y Linux conectados al área de trabajo de Log Analytics. La [selección de destino de solución](../operations-management-suite/operations-management-suite-solution-targeting.md) le permite aplicar un ámbito a sus soluciones.

Si Microsoft Monitoring Agent se instala directamente en la VM (no como una extensión de Azure), Security Center no instala Microsoft Monitoring Agent y la supervisión de seguridad estará limitada.

### <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>¿Instala Security Center soluciones en mis áreas de trabajo de Log Analytics existentes? ¿Qué implica desde el punto de vista de la facturación?
Cuando Security Center identifica que una VM ya está conectada a un área de trabajo creado por el usuario, habilita las soluciones en esta área de trabajo según el plan de tarifa. Las soluciones se aplican solo a las VM de Azure pertinentes a través de la [selección de destino de solución](../operations-management-suite/operations-management-suite-solution-targeting.md), por lo que la facturación sigue siendo la misma.

- **Nivel Gratis**: Security Center instala la solución “SecurityCenterFree” en el área de trabajo. No se le facturará por el nivel Gratis.
- **Nivel Estándar**: Security Center instala la solución "Security" en el área de trabajo.

   ![Soluciones del área de trabajo predeterminada][4]

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Ya tengo áreas de trabajo en mi entorno, ¿puedo utilizarlas para recopilar datos de seguridad?
Si una VM ya tiene Microsoft Monitoring Agent instalado como una extensión de Azure, Security Center usa el área de trabajo conectada existente. Una solución de Security Center se instala en el área de trabajo si todavía no está presente y la solución solo se aplica a VM relevantes mediante la [selección de destino de solución](../operations-management-suite/operations-management-suite-solution-targeting.md).

Cuando Security Center instala Microsoft Monitoring Agent en VM, utiliza los espacios de trabajo predeterminados creados por Security Center.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Ya tengo una solución de seguridad en mis áreas de trabajo. ¿Qué implica desde el punto de vista de la facturación?
La solución Seguridad y auditoría se utiliza para habilitar las características del nivel Estándar de Security Center para VM de Azure. Si la solución Seguridad y auditoría ya está instalada en un área de trabajo, Security Center utiliza la solución existente. No hay ningún cambio en la facturación.

## <a name="using-azure-security-center"></a>Uso del Centro de seguridad de Azure
### <a name="what-is-a-security-policy"></a>¿Qué es una directiva de seguridad?
Una directiva de seguridad define el conjunto de controles recomendados para los recursos de la suscripción específica. En Azure Security Center, se definen las directivas para las suscripciones de Azure de acuerdo con los requisitos de seguridad de la compañía y el tipo de aplicaciones o la confidencialidad de los datos de cada suscripción.

Las directivas de seguridad habilitadas en Azure Security Center generan la supervisión y recomendaciones de seguridad. Para obtener más información sobre las directivas de seguridad, consulte [Supervisión del estado de seguridad en el Centro de seguridad de Azure](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>¿Quién puede modificar una directiva de seguridad?
Para modificar una directiva de seguridad, debe ser administrador de seguridad o propietario/colaborador de esa suscripción.

Para obtener información sobre cómo configurar una directiva de seguridad, consulte [Establecimiento de directivas de seguridad en el Centro de seguridad de Azure](tutorial-security-policy.md).

### <a name="what-is-a-security-recommendation"></a>¿Qué es una recomendación de seguridad?
El Centro de seguridad de Azure analiza el estado de seguridad de los recursos de Azure. Las recomendaciones se crean una vez que se identifican las posibles vulnerabilidades de seguridad. Las recomendaciones le guían en el proceso de configurar el control necesario. Algunos ejemplos son:

* Aprovisionamiento de antimalware para ayudar a identificar y eliminar software malintencionado.
* [Grupos de seguridad de red](../virtual-network/security-overview.md) y reglas para controlar el tráfico a las máquinas virtuales.
* Aprovisionamiento de un firewall de aplicaciones web para ayudar a defenderse contra ataques dirigidos a las aplicaciones web.
* Implementación de actualizaciones del sistema que faltan.
* Resolución de las configuraciones de sistema operativo que no coinciden con las líneas base recomendadas.

Aquí solo se muestran las recomendaciones habilitadas en las directivas de seguridad.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>¿Cómo puedo ver el estado de seguridad actual de los recursos de Azure?
La hoja **Security Center Overview** (Introducción a Security Center) muestra la posición de seguridad general del entorno con la información desglosada por Compute, Network, Storage y datos, y aplicaciones. Cada tipo de recurso tiene un indicador que muestra si se ha identificado alguna posible vulnerabilidad de seguridad. Al hacer clic en cada icono, se muestra una lista de problemas de seguridad que ha identificado Security Center, junto con un inventario de los recursos de la suscripción.

### <a name="what-triggers-a-security-alert"></a>¿Qué desencadena una alerta de seguridad?
Azure Security Center recopila, analiza y combina automáticamente los datos de registro de los recursos de Azure, la red y soluciones de asociados como firewalls y antimalware. Cuando se detecten amenazas, se creará una alerta de seguridad. Como ejemplos se incluye la detección de:

* Máquinas virtuales en peligro que se comunican con direcciones IP malintencionadas conocidas.
* Malware avanzado detectado mediante la generación de informes de errores de Windows.
* Ataques por fuerza bruta contra máquinas virtuales.
* Alertas de seguridad de soluciones de seguridad integradas de socio, como antimalware o Firewall de aplicaciones web.

### ¿Por qué los valores de puntuación segura cambian? <a name="secure-score-faq"></a>
A partir de febrero de 2019, Security Center ajustó la puntuación de algunas recomendaciones para adaptarse mejor a su gravedad. Como resultado de este ajuste, puede haber cambios en los valores generales de la puntuación segura.  Para más información sobre la puntuación segura, consulte [Cálculo de puntuación segura](security-center-secure-score.md).

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>¿Qué diferencia hay entre las amenazas detectadas y advertidas por el Centro de respuestas de seguridad de Microsoft y las comunicadas por Azure Security Center?
El Centro de respuestas de seguridad de Microsoft (MSRC) lleva a cabo una selecta supervisión de seguridad de la red e infraestructura de Azure y recibe información sobre amenazas y quejas sobre abusos de terceros. Cuando MSRC se da cuenta de que un usuario ilegítimo o no autorizado ha tenido acceso a los datos del cliente o de que el uso de Azure por parte del cliente no cumple las condiciones de uso aceptable, un administrador de incidentes de seguridad notifica al cliente. La notificación suele consistir en el envío de un correo electrónico a los contactos de seguridad especificados en Azure Security Center o al propietario de la suscripción de Azure en caso de no especificarse un contacto de seguridad.

Azure Security Center es un servicio de Azure que no deja de supervisar el entorno de Azure del cliente y aplica análisis para detectar de forma automática una amplia variedad de actividades potencialmente malintencionadas. Estas detecciones aparecen como alertas de seguridad en el panel de Azure Security Center.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>¿Qué recursos de Azure supervisa Azure Security Center?
Azure Security Center supervisa los siguientes recursos de Azure:

* Máquinas virtuales (se incluyen [Cloud Services](../cloud-services/cloud-services-choose-me.md))
* Conjunto de escalado de máquinas virtuales (VMSS)
* Instancias de Azure Virtual Network
* Servicio de SQL Azure
* Cuenta de Azure Storage
* Azure Web Apps ([en App Service Environment](../app-service/environment/intro.md))
* Soluciones de asociados integradas en su suscripción de Azure, como un firewall de aplicaciones web en las máquinas virtuales y en App Service Environment

Además, Azure Security Center también puede supervisar los equipos que no son de Azure (incluidos los locales) (se admiten tanto los [equipos Windows ](./quick-onboard-windows-computer.md) como los [equipos Linux ](./quick-onboard-linux-computer.md)).

## <a name="virtual-machines"></a>Virtual Machines
### <a name="what-types-of-virtual-machines-are-supported"></a>¿Qué tipos de máquinas virtuales se admiten?
La funcionalidad de supervisión y las recomendaciones están disponibles para las máquinas virtuales creadas con los [modelos de implementación clásica y de Resource Manager](../azure-classic-rm.md).

Consulte [Plataformas compatibles con Azure Security Center](security-center-os-coverage.md) para ver una lista de plataformas compatibles.

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>¿Por qué Azure Security Center no reconoce la solución antimalware que se ejecuta en mi máquina virtual de Azure?
Azure Security Center solo tiene visibilidad del antimalware instalado mediante extensiones de Azure. Por ejemplo, Security Center no puede detectar antimalware instalado previamente en una imagen que haya proporcionado o si instaló este tipo de herramienta en máquinas virtuales con sus propios procesos (como sistemas de administración de configuración).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>¿Por qué recibo el mensaje Faltan los datos de detección en mi máquina virtual?
Este mensaje aparece cuando no hay datos de examen de una máquina virtual. Los datos de detección pueden tardar algún tiempo (menos de una hora) en rellenarse después de habilitar la recolección de datos en Azure Security Center. Tras el rellenado inicial de los datos de examen, se puede recibir este mensaje porque no hay ningún dato de examen o porque no hay ninguno que sea reciente. Estos análisis no se rellenan para las máquinas virtuales que estén detenidas. Este mensaje también podría aparecer si no se han rellenado datos de examen recientemente (de acuerdo con la directiva de retención del agente de Windows, que tiene un valor predeterminado de 30 días).

### <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>¿Con qué frecuencia Security Center analiza las vulnerabilidades del sistema operativo, las actualizaciones del sistema y los problemas de Endpoint Protection?
La latencia de los análisis de Security Center en busca de vulnerabilidades, actualizaciones y problemas es:

- Configuraciones de seguridad del sistema operativo: los datos se actualizan en un plazo de 48 horas
- Actualizaciones del sistema: los datos se actualizan en un plazo de 24 horas.
- Problemas de Endpoint Protection: los datos se actualizan en un plazo de 8 horas.

Normalmente, Security Center busca nuevos datos cada hora y actualiza las recomendaciones en consecuencia. 

> [!NOTE]
> Desde primeros de junio de 2017, Security Center usará Microsoft Monitoring Agent para recopilar y almacenar datos. Para obtener más información, consulte [Migración de la plataforma de Azure Security Center](security-center-platform-migration.md). La información de este artículo representa la funcionalidad de Security Center después de la transición a Microsoft Monitoring Agent.
>
>

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>¿Por qué recibo el mensaje Falta el agente de máquina virtual?
El agente de VM debe instalarse en VM para habilitar la recopilación de datos. De manera predeterminada, el agente de máquina virtual está instalado en las máquinas virtuales que se implementan desde Azure Marketplace. La entrada de blog sobre [las extensiones y el agente de máquina virtual](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)proporciona información sobre cómo instalar el Agente de máquina virtual.


<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
[5]: ./media/security-center-platform-migration-faq/use-another-workspace.png
