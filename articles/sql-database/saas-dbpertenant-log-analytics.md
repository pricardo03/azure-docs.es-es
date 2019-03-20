---
title: Usar registros de Azure Monitor con una aplicación multiinquilino de SQL Database | Microsoft Docs
description: Configurar y usar los registros de Azure Monitor con una aplicación SaaS de Azure SQL Database multiinquilino
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 6380488faa9a4554df5df5ea67e11dbeb8853fff
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57455928"
---
# <a name="set-up-and-use-azure-monitor-logs-with-a-multitenant-sql-database-saas-app"></a>Configurar y usar los registros de Azure Monitor con una aplicación SaaS de base de datos SQL multiinquilino

En este tutorial, configurar y usar [registros de Azure Monitor](/azure/log-analytics/log-analytics-overview) para supervisar los grupos elásticos y bases de datos. Este tutorial se basa en el [tutorial de administración y supervisión del rendimiento](saas-dbpertenant-performance-monitoring.md). Muestra cómo usar los registros de Azure Monitor para intensificar la supervisión y alerta proporcionadas en el portal de Azure. Azure Monitor registra admite miles de grupos elásticos y cientos de miles de bases de datos de supervisión. Registros de Azure Monitor proporciona una única solución de supervisión, que puede integrar la supervisión de distintas aplicaciones y servicios de Azure a través de varias suscripciones de Azure.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Instale y configure los registros de Azure Monitor.
> * Usar registros de Azure Monitor para la supervisión de grupos y bases de datos.

Para completar este tutorial, asegúrese de cumplir estos requisitos previos:

* Aplicación Wingtip Tickets SaaS Database Per Tenant implementada. Para implementarla en menos de cinco minutos, consulte el artículo sobre [implementación y exploración de la aplicación Wingtip Tickets SaaS Database Per Tenant](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell está instalado. Para más información, consulte el artículo de [introducción a Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Consulte el [tutorial de administración y supervisión del rendimiento](saas-dbpertenant-performance-monitoring.md) para una descripción de los patrones y escenarios de SaaS, y ver cómo afectan a los requisitos de una solución de supervisión.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-azure-monitor-logs"></a>Supervisión y administración de base de datos de rendimiento y grupos elásticos con registros de Azure Monitor

En Azure SQL Database, la supervisión y alerta están disponibles en bases de datos y grupos de Azure Portal. Esta supervisión y alerta integrada resulta práctica, pero también es específica del recurso. Esto significa que resulta menos adecuada para supervisar grandes instalaciones o para proporcionar una vista unificada entre recursos y suscripciones.

Para escenarios de gran volumen, puede usar los registros de Azure Monitor para la supervisión y alertas. Azure Monitor es un servicio de Azure independiente que permite el análisis sobre registros de diagnóstico y telemetría recopilados en un área de trabajo de potencialmente muchos servicios. Registros de Azure Monitor proporciona una consulta integrada de herramientas de visualización de datos y lenguaje que permiten el análisis de datos operativos. La solución SQL Analytics proporciona varias vistas y consultas predefinidas de supervisión y alerta de bases de datos y grupos elásticos. Registros de Monitor de Azure también proporciona un diseñador de vistas personalizadas.

Las áreas de trabajo de OMS ahora se conocen como áreas de trabajo de Log Analytics. Las áreas de trabajo y las soluciones de análisis de Log Analytics se abren en Azure Portal. Azure Portal es el punto de acceso más reciente, pero, en ciertos aspectos, podría ir por detrás del portal de Operations Management Suite.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Generación de datos de diagnóstico de rendimiento mediante la simulación de una carga de trabajo en los inquilinos 

1. En PowerShell ISE, abra *..\\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\Performance Monitoring and Management\\Demo-PerformanceMonitoringAndManagement.ps1*. Mantenga este script abierto, ya que quizá quiera ejecutar varios de los escenarios de generación de carga durante este tutorial.
1. Si aún no lo ha hecho, aprovisione un lote de inquilinos para que el contexto de supervisión sea más interesante. Este proceso tarda unos minutos.

    a. Establezca **$DemoScenario = 1**, _Aprovisionamiento de un lote de inquilinos_.

   b. Para ejecutar el script e implementar 17 inquilinos adicionales, presione F5.

1. Ahora inicie el generador de carga para ejecutar una carga simulada en todos los inquilinos.

     a. Establecer **$DemoScenario = 2**, _generar una carga de intensidad normal (aproximadamente 30 DTU)_.

    b. Presione F5 para ejecutar el script.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Obtención de los scripts de la aplicación Wingtip Tickets SaaS Database Per Tenant

Los scripts y el código fuente de la aplicación SaaS de base de datos multiinquilino Wingtip Tickets están disponibles en el repositorio de GitHub [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Consulte las [instrucciones generales](saas-tenancy-wingtip-app-guidance-tips.md) para saber cuáles son los pasos para descargar y desbloquear los scripts de PowerShell de Wingtip Tickets.

## <a name="install-and-configure-log-analytics-workspace-and-the-azure-sql-analytics-solution"></a>Instalar y configurar el área de trabajo de Log Analytics y la solución Azure SQL Analytics

Azure Monitor es un servicio independiente que se debe configurar. Registros de Azure Monitor recopila datos de registro, datos de telemetría y métricas en un área de trabajo de Log Analytics. Al igual que otros recursos de Azure, el área de trabajo de Log Analytics se debe crear, pero no en el mismo grupo de recursos que las aplicaciones que supervisa. Si bien, hacer esto es a menudo lo que tiene más sentido. Para la aplicación Wingtip Tickets, use un grupo de recursos único para garantizar que el área de trabajo se elimine con la aplicación.

1. En PowerShell ISE, abra *..\\WingtipTicketsSaaS-MultiTenantDb-master\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\Demo-LogAnalytics.ps1*.
1. Presione F5 para ejecutar el script.

Ahora puede abrir los registros de Azure Monitor en Azure portal. Se tarda unos minutos en recopilar la telemetría en el área de trabajo de Log Analytics y para que sea visible. Cuanto más tiempo deje que el sistema recopile datos de diagnóstico, más interesante será la experiencia. 

## <a name="use-log-analytics-workspace-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Use el área de trabajo de Log Analytics y la solución SQL Analytics para supervisar grupos y bases de datos


En este ejercicio, abra el área de trabajo de Log Analytics en Azure portal para buscar la telemetría recopilada para las bases de datos y los grupos.

1. Vaya a [Azure Portal](https://portal.azure.com). Seleccione **todos los servicios** para abrir el área de trabajo de Log Analytics. A continuación, busque Log Analytics.

   ![Abrir área de trabajo de Log Analytics](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Seleccione el área de trabajo denominada _wtploganalytics-&lt;user&gt;_.

1. Seleccione **Introducción** para abrir la solución de log analytics en Azure portal.

   ![Información general](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > La solución puede tardar unos minutos en activarse. 

1. Seleccione el icono de **Azure SQL Analytics** para abrirlo.

    ![Icono de información general](media/saas-dbpertenant-log-analytics/overview.png)

1. Las vistas en la solución se desplazan lateralmente, con su propia barra de desplazamiento interna en la parte inferior. Si es necesario, actualice la página.

1. Para explorar la página de resumen, seleccione los iconos o bases de datos individuales para abrir un explorador de exploración en profundidad.

    ![Panel de análisis de registro](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. Cambie la configuración de filtro para modificar el intervalo de tiempo. En este tutorial, seleccione **Last 1 hour** (Última hora).

    ![Filtro de tiempo](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Seleccione una base de datos individual para explorar las métricas y el uso de consultas para esa base de datos.

    ![Análisis de base de datos](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Para ver las métricas de uso, desplace la página de análisis a la derecha.
 
     ![Métricas de base de datos](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Desplace la página de análisis a la izquierda y seleccione el icono de servidor en la lista de **información de recursos**.  

    ![Lista de información de recursos](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    Se abre una página que muestra los grupos y las bases de datos en el servidor.

    ![Servidor con grupos y bases de datos](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Seleccione un grupo. En la página de grupo que se abre, desplácese hacia la derecha para ver las métricas del grupo. 

    ![Métricas de grupo](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


1. De nuevo en el área de trabajo de Log Analytics, seleccione el **portal de OMS** para abrir ahí el área de trabajo.

    ![Área de trabajo de Log Analytics](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

En el área de trabajo de Log Analytics, puede explorar aún más los datos de métricas y de registro. 

Supervisión y alertas en Azure Monitor registros se basan en las consultas a través de los datos del área de trabajo, a diferencia de las alertas definidas en cada recurso en el portal de Azure. Al basar las alertas en consultas, puede definir una alerta única que busque en todas las bases de datos, en lugar de definir una por cada base de datos. Las consultas solo se ven limitadas por los datos disponibles en el área de trabajo.

Para obtener más información sobre cómo usar los registros de Azure Monitor para consultar y establecer alertas, consulte [trabajar con reglas de alerta en Azure Monitor registra](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Registros de Azure Monitor para SQL Database se cobra según el volumen de datos del área de trabajo. En este tutorial, ha creado un área de trabajo gratuita, que está limitada a 500 MB al día. Una vez alcanzado ese límite, ya no se agregan más datos al área de trabajo.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Instale y configure los registros de Azure Monitor.
> * Usar registros de Azure Monitor para la supervisión de grupos y bases de datos.

Pruebe el [tutorial de análisis de inquilinos](saas-dbpertenant-log-analytics.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Otros tutoriales basados en la implementación inicial de la aplicación Wingtip Tickets SaaS Database Per Tenant](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Registros de Azure Monitor](../azure-monitor/insights/azure-sql.md)
