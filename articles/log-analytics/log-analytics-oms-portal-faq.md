---
title: Preguntas comunes para la transición desde el portal de OMS a Azure Portal para los usuarios de Log Analytics| Microsoft Docs
description: Respuestas a preguntas comunes para los usuarios de Log Analytics que realizan la transición desde el portal de OMS a Azure Portal.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: bwren
ms.openlocfilehash: 1e0fd56b6e420103b4f786985f71a84737db642d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333639"
---
# <a name="common-questions-for-transition-from-oms-portal-to-azure-portal-for-log-analytics-users"></a>Preguntas comunes para la transición desde el portal de OMS a Azure Portal para los usuarios de Log Analytics
Log Analytics inicialmente usaba su propio portal denominado portal de OMS para administrar su configuración y analizar los datos recopilados.  Toda la funcionalidad de este portal se ha movido a Azure Portal, donde continuará desarrollándose.

En este artículo se ofrecen respuestas a preguntas comunes para los usuarios que realizan esta transición.  Si usó Log Analytics en el portal de OMS, puede encontrar respuestas aquí sobre cómo puede realizar las mismas tareas en Azure Portal.

## <a name="where-do-i-find-log-analytics-in-azure"></a>¿Dónde encuentro Log Analytics en Azure?
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).  Haga clic en **Todos los servicios** y, en la lista de recursos, escriba **Log Analytics**. Seleccione **Log Analytics** y luego elija su área de trabajo. Se muestra la página de resumen para el área de trabajo.

![Área de trabajo de Log Analytics](media/log-analytics-new-portal/log-analytics.png)

## <a name="how-do-i-manage-permissions"></a>¿Cómo administro los permisos?
Si no tiene acceso a su área de trabajo de Log Analytics en Azure Portal, debe configurar los permisos mediante el [acceso basado en rol de Azure](../active-directory/role-based-access-control-configure.md). Para detalles sobre la administración de permisos de área de trabajo, consulte [Administración de áreas de trabajo](../log-analytics/log-analytics-manage-access.md#manage-accounts-and-users). Para información sobre cómo administrar permisos para alertas, consulte [Introducción a roles, permisos y seguridad con Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md).

## <a name="how-do-i-create-a-new-workspace"></a>¿Cómo se crea una nueva área de trabajo? 
En la lista de áreas de trabajo en Azure Portal, haga clic en **Agregar**.  Para detalles completos, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../log-analytics/log-analytics-quick-create-workspace.md).

![Página de información general](media/log-analytics-new-portal/new-workspace.png)

## <a name="where-is-my-overview-page"></a>¿Dónde está mi página de introducción?
La pantalla principal del portal de OMS muestra los iconos para todas las soluciones de administración instaladas en el área de trabajo y todas las vistas personalizadas que haya creado. Esta misma vista está disponible en Azure Portal. En el área de trabajo, seleccione **Workspace Summary** (Resumen del área de trabajo).

![Página de información general](media/log-analytics-new-portal/overview.png)

## <a name="how-do-i-open-log-search-and-view-designer"></a>¿Cómo se abre la búsqueda de registros y el diseñador de vistas?
Tanto la **búsqueda de registros** como el **diseñador de vistas** están disponibles en la página principal y en el menú de la izquierda del área de trabajo de Azure Portal, a la derecha de **Introducción**.

## <a name="where-do-i-find-settings"></a>¿Dónde puedo encontrar la configuración?
Muchas de las configuraciones de la sección **Configuración** del portal de OMS están disponibles en el menú **Configuración avanzada** de Azure Portal para el área de trabajo.

![Configuración avanzada](media/log-analytics-new-portal/advanced-settings.png)

En las secciones siguientes se proporciona una lista completa de cómo puede acceder a la configuración que estaba previamente disponible en la sección **Configuración** del portal de OMS.

### <a name="accounts"></a>Cuentas 
La configuración de cuentas se administra en distintos lugares en Azure Portal, tal y como se describe en la tabla siguiente.

| Configuración en el portal de OMS | Equivalente en Azure Portal |
|:---|:---|
| Cuenta de Automation | Menú **Cuenta de automatización** para el área de trabajo. |
| Suscripción y plan de datos de Azure | Menú **Nivel de precios** para el área de trabajo. |
| Administrar usuarios | Use el acceso basado en rol de Azure para [administrar permisos para el área de trabajo](#how-do-i-manage-permissions). |
| Información del área de trabajo | La información disponible en el menú **Área de trabajo de OMS** para el área de trabajo. |

### <a name="alerts"></a>Alertas
Las reglas de alerta basadas en consultas de Log Analytics ahora se administran en la [experiencia de alertas unificada](#how-do-i-create-and-manage-alerts). 

### <a name="computer-groups"></a>Grupos de equipos
Administre grupos de equipos en el menú **Configuración avanzada** para el área de trabajo. 

### <a name="connected-sources"></a>Orígenes conectados
Administre la mayoría de las configuraciones de orígenes conectados en el menú **Configuración avanzada** para el área de trabajo. En la tabla siguiente se proporcionan detalles para cada sección de este menú.

| Configuración en el portal de OMS | Equivalente en Azure Portal |
|:---|:---|
| Servidores Windows   | Menú **Configuración avanzada** para el área de trabajo. |
| Servidores Linux   | Menú **Configuración avanzada** para el área de trabajo. |
| Azure Storage     | Menú **Configuración avanzada** para el área de trabajo. |
| System Center     | Menú **Configuración avanzada** para el área de trabajo. |
| Office 365        | Consulte la [documentación de solución de administración de Office 365](../operations-management-suite/oms-solution-office-365.md) para obtener detalles de configuración. |
| Telemetría de Windows | Todavía no está disponible en Azure Portal. |
| Conector ITSM    | Consulte [Conectar productos o servicios de ITSM con el Conector de Administración de servicios de TI](../log-analytics/log-analytics-itsmc-connections.md) para instrucciones sobre cómo conectar el servicio ITSM con Log Analytics. |

### <a name="data"></a>Datos
Administre la mayoría de las configuraciones de datos en el menú **Configuración avanzada** para el área de trabajo. En la tabla siguiente se proporcionan detalles para cada sección de este menú.

| Configuración en el portal de OMS | Equivalente en Azure Portal |
|:---|:---|
| Registros de eventos de Windows           | Menú **Configuración avanzada** para el área de trabajo. |
| Contadores de rendimiento de Windows | Menú **Configuración avanzada** para el área de trabajo. |
| Contadores de rendimiento de Linux   | Menú **Configuración avanzada** para el área de trabajo. |
| Registros IIS                     | Menú **Configuración avanzada** para el área de trabajo. |
| Campos personalizados                | Menú **Configuración avanzada** para el área de trabajo. |
| Registros personalizados                  | Menú **Configuración avanzada** para el área de trabajo. |
| syslog                       | Menú **Configuración avanzada** para el área de trabajo. |
| Application Insights         | Esta solución está en desuso ahora que Log Analytics y Application Insights comparten el mismo motor de datos.  |
| Seguimiento de archivos de Windows        | Menú **Seguimiento de cambios** en Azure Automation. Consulte [Seguimiento de cambios en el entorno con la solución Change Tracking](../automation/automation-change-tracking.md) para detalles. |
| Seguimiento del Registro de Windows        | Menú **Seguimiento de cambios** en Azure Automation. Consulte [Seguimiento de cambios en el entorno con la solución Change Tracking](../automation/automation-change-tracking.md) para detalles. |
| Seguimiento de archivos de Linux          | Menú **Seguimiento de cambios** en Azure Automation. Consulte [Seguimiento de cambios en el entorno con la solución Change Tracking](../automation/automation-change-tracking.md) para detalles. |

### <a name="solutions"></a>Soluciones
Administre las soluciones del menú **Soluciones** para el área de trabajo. 

## <a name="how-do-i-install-and-remove-management-solutions"></a>¿Cómo instalo y quito las soluciones de administración?
En el portal de OMS, las soluciones de administración se instalan desde la Galería de soluciones y se quitan de **Configuración**. En Azure Portal, [instale soluciones de administrador](../monitoring/monitoring-solutions.md#install-a-management-solution) desde Azure Marketplace. [Quite soluciones](../monitoring/monitoring-solutions.md#remove-a-management-solution) de la lista de soluciones instaladas.

## <a name="how-do-i-create-and-manage-alerts"></a>¿Cómo creo y administro alertas?
Las reglas de alerta basadas en consultas de Log Analytics ahora se administran en la [experiencia de alertas unificada](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). Consulte [cómo extender alertas desde Log Analytics en alertas de Azure](../monitoring-and-diagnostics/monitoring-alerts-extend-tool.md) para detalles sobre cómo configurar y usar las alertas en Azure Portal.

## <a name="how-do-i-access-my-dashboards"></a>¿Cómo accedo a mis paneles?
Los [paneles](../log-analytics/log-analytics-dashboards.md) de Log Analytics han quedado en desuso.  Puede visualizar datos de Log Analytics mediante el [Diseñador de vistas](../log-analytics/log-analytics-view-designer.md), que tiene una funcionalidad adicional, y anclar la consulta y las vistas en los paneles de Azure.

## <a name="how-do-i-check-my-usage"></a>¿Cómo puedo comprobar el uso?
Ahora puede ver y administrar fácilmente su uso y el costo de Log Analytics seleccionando **Uso y costos estimados** en el área de trabajo.

![Uso y costos estimados](media/log-analytics-new-portal/usage.png)


## <a name="can-i-still-use-the-classic-portal"></a>¿Todavía puedo usar el portal clásico?
Durante un tiempo limitado, es posible acceder al portal a través de esta dirección URL, con su propio nombre de área de trabajo: https://\<su nombre de área de trabajo\>.portal.mms.microsoft.com. Sin embargo, se recomienda usar Azure Portal y que nos proporcione los comentarios en LAUpgradeFeedback@microsoft.com sobre cualquier problema de bloqueo.

## <a name="next-steps"></a>Pasos siguientes

- [Buscar e instalar soluciones de administración](../monitoring/monitoring-solutions.md) mediante Azure Portal.
- Más información sobre la [búsqueda de registros en Azure Portal](log-analytics-log-search-portals.md).