---
title: Más información sobre Azure Application Insights | Microsoft Docs
description: Aquí se presentan algunas características que puede explorar una vez que sepa utilizar Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: 7e62efb0b64b0b882b36126af4a1f9d520d92475
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65230638"
---
# <a name="more-telemetry-from-application-insights"></a>Más telemetría desde Application Insights
Una vez que haya [agregado Application Insights a su código de ASP.NET](../../azure-monitor/app/asp-net.md), puede realizar varias acciones para obtener más datos de telemetría. 

| . | Lo que obtiene|
|---|---|
|(Servidores IIS) [Instale Monitor de estado](https://go.microsoft.com/fwlink/?LinkId=506648) en cada máquina del servidor.<br/>(Aplicaciones web de Azure) En el panel de control de Azure de la aplicación web, abra la hoja Application Insights.| [**Contadores de rendimiento**](../../azure-monitor/app/performance-counters.md).<br/>[**Excepciones**](asp-net-exceptions.md): seguimiento de la pila detallado.<br/>[**Dependencias**](../../azure-monitor/app/asp-net-dependencies.md).|
|[Agregue el fragmento de código de JavaScript en las páginas web](../../azure-monitor/app/javascript.md).|[Rendimiento de página](../../azure-monitor/app/usage-overview.md), excepciones de explorador y rendimiento de AJAX. Telemetría del lado cliente personalizada.|
|[Cree pruebas web de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md).|Obtención de alertas si el sitio deja de estar disponible.|
|[Asegúrese de que se genera buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) mediante MSBuild.|[Generación de anotaciones en gráficos de métricas](https://docs.microsoft.com/azure/azure-monitor/app/annotations)
|[Cree eventos y métricas personalizados](../../azure-monitor/app/api-custom-events-metrics.md).|Recuento de métricas y eventos empresariales, seguimiento de uso detallado y mucho más.|
|[Genere un perfil del sitio activo.](https://aka.ms/AIProfilerPreview)|Intervalos de función detallados de la aplicación web activa.|






