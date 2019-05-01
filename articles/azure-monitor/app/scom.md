---
title: Integración de SCOM con Application Insights | Microsoft Docs
description: Si es usuario de SCOM, supervise el rendimiento y diagnostique problemas con Application Insights. Paneles integrales, alertas inteligentes, eficaces herramientas de diagnóstico y consultas de análisis.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: mbullwin
ms.openlocfilehash: a08a67a78362325e29b1002b44f390a94e78888a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60784884"
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Supervisión del rendimiento de la aplicación con Application Insights para SCOM
Si usa System Center Operations Manager (SCOM) para administrar los servidores, puede supervisar el rendimiento y diagnosticar problemas de rendimiento con la ayuda de [Azure Application Insights](../../azure-monitor/app/asp-net.md). Application Insights supervisa solicitudes entrantes de la aplicación web, llamadas SQL y REST salientes, excepciones y seguimientos de registros. Proporciona paneles con gráficos de métricas y alertas inteligentes, así como una eficaz búsqueda de diagnóstico y consultas analíticas sobre esta telemetría. 

Puede activar la supervisión de Application Insights mediante un módulo de administración de SCOM.

> [!IMPORTANT]
> Este paquete de administración de System Center Operations Manager está **en desuso**. No es compatible con el SDK más reciente de Application Insights y ya no se recomienda.

## <a name="before-you-start"></a>Antes de comenzar
Condiciones:

* Está familiarizado con SCOM y usa SCOM 2012 R2 o 2016 para administrar los servidores web de IIS.
* Ya ha instalado los servidores de una aplicación web que desea supervisar con Application Insights.
* La versión de framework de la aplicación es .NET 4.5 o posterior.
* Tiene acceso a una suscripción en [Microsoft Azure](https://azure.com) y puede iniciar sesión en [Azure Portal](https://portal.azure.com). Su organización puede tener una suscripción y puede agregar su cuenta de Microsoft a ella.

(El equipo de desarrollo puede generar el [SDK de Application Insights](../../azure-monitor/app/asp-net.md) en la aplicación web. Esta instrumentación en tiempo de compilación les proporciona mayor flexibilidad al escribir una telemetría personalizada. Sin embargo, no importa: puede seguir los pasos descritos aquí, con o sin el SDK integrado).

## <a name="one-time-install-application-insights-management-pack"></a>(Una vez) Instalación del módulo de administración de Application Insights
En el equipo donde se ejecuta Operations Manager:

1. Desinstale cualquier versión anterior del paquete de administración:
   1. En Operations Manager, abra Administración, Módulos de administración. 
   2. Elimine la versión anterior.
2. Descargue e instale el módulo de administración desde el catálogo.
3. Reinicie Operations Manager.

## <a name="create-a-management-pack"></a>Creación de un módulo de administración
1. En Operations Manager, abra **Creación**, **.NET... con Application Insights**, **Asistente para agregar monitores** y vuelva a elegir **.NET... con Application Insights**.
   
    ![](./media/scom/020.png)
2. Asigne un nombre a la configuración de acuerdo con la aplicación. (Tiene que instrumentar una aplicación cada vez).
   
    ![](./media/scom/030.png)
3. En la misma página del asistente, cree un módulo de administración, o bien seleccione un módulo que creó anteriormente para Application Insights.
   
     (El [módulo de administración](https://technet.microsoft.com/library/cc974491.aspx) de Application Insights es una plantilla, a partir de la cual se crea una instancia. Puede reutilizar la misma instancia más adelante).

    ![En la pestaña Propiedades generales, escriba el nombre de la aplicación. Haga clic en Nuevo y escriba un nombre para un módulo de administración. Haga clic en Aceptar y, luego, en Crear.](./media/scom/040.png)

1. Elija una aplicación que desea supervisar. La característica de búsqueda busca entre las aplicaciones instaladas en los servidores.
   
    ![En la pestaña What to Monitor (Qué supervisar), haga clic en Agregar, escriba parte del nombre de la aplicación, haga clic en Buscar, seleccione la aplicación y haga clic en Agregar y Aceptar.](./media/scom/050.png)
   
    El campo opcional Ámbito de supervisión puede utilizarse para especificar un subconjunto de los servidores, si no desea supervisar la aplicación en todos los servidores.
2. En la siguiente página del asistente, debe proporcionar las credenciales para iniciar sesión en Microsoft Azure.
   
    En esta página, elija el recurso de Application Insights donde desea que los datos de telemetría se analicen y muestren. 
   
   * Si la aplicación se configuró para Application Insights durante el desarrollo, seleccione el recurso existente.
   * De lo contrario, cree un recurso con un nombre que corresponda a la aplicación. Si hay otras aplicaciones que son componentes del mismo sistema, colóquelas en el mismo grupo de recursos, para que el acceso a los datos a la telemetría sea más fácil de administrar.
     
     Puede cambiar estas opciones aquí.
     
     ![En la pestaña de configuración de Application Insights, haga clic en 'Iniciar sesión' y proporcione las credenciales de la cuenta de Microsoft para Azure. A continuación, elija una suscripción, el grupo de recursos y el recurso.](./media/scom/060.png)
3. Realice los pasos del asistente.
   
    ![Haga clic en Crear.](./media/scom/070.png)

Repita este procedimiento para cada aplicación que desee supervisar.

Si necesita cambiar la configuración más adelante, vuelva a abrir las propiedades del monitor en la ventana Creación.

![En Creación, seleccione .NET Application Performance Monitoring (Supervisión del rendimiento de la aplicación de .NET) con Application Insights, seleccione al monitor y haga clic en Propiedades.](./media/scom/080.png)

## <a name="verify-monitoring"></a>Comprobación de la supervisión
El monitor que ha instalado busca la aplicación en todos los servidores. Cuando encuentra la aplicación, configura el Monitor de estado de Application Insights ahí para supervisarla. Si es necesario, instala primero el Monitor de estado en el servidor.

Puede comprobar qué instancias de la aplicación ha encontrado:

![En Supervisión, abra Application Insights](./media/scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Visualización de la telemetría en Application Insights
En el [Portal de Azure](https://portal.azure.com), busque el recurso de la aplicación. [Verá gráficos que muestran la telemetría](../../azure-monitor/app/app-insights-dashboards.md) de su aplicación. (Si aún no ha aparecido en la página principal, haga clic en Live Metrics Stream).

## <a name="next-steps"></a>Pasos siguientes
* [Configuración de un panel](../../azure-monitor/app/app-insights-dashboards.md) para reunir los gráficos más importantes que supervisan esta y otras aplicaciones.
* [Más información sobre las métricas](../../azure-monitor/app/metrics-explorer.md)
* [Configuración de alertas](../../azure-monitor/app/alerts.md)
* [Diagnóstico de problemas de rendimiento](../../azure-monitor/app/detect-triage-diagnose.md)
* [Consultas de Analytics eficaces](../../azure-monitor/app/analytics.md)
* [Pruebas web de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md)

