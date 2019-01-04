---
title: archivo de inclusión
description: archivo de inclusión
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 06/21/2018
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 1efddb605422f00575cc0e44592315b725c838e3
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "54002780"
---
Hay algunos límites en el número de métricas y eventos por aplicación (es decir, por clave de instrumentación). Los límites dependen del [plan de precios](https://azure.microsoft.com/pricing/details/application-insights/) que elija.

| Recurso | Límite predeterminado | Nota:
| --- | --- | --- |
| Total de datos por día | 100 GB | Se pueden reducir los datos si se establece un límite. Si necesita más datos, puede aumentar el límite en el portal, hasta 1000 GB. Para capacidades mayores de 1000 GB, envíe un correo electrónico a AIDataCap@microsoft.com.
| Limitaciones | 32 000 eventos por segundo | El límite se mide por minuto.
| Retención de datos | 90 días | Este recurso es para [Search](../articles/azure-monitor/app/diagnostic-search.md), [Analytics](../articles/azure-monitor/app/analytics.md) y el [Explorador de métricas](../articles/azure-monitor/app/metrics-explorer.md).
| Retención de resultados detallados para la [prueba de disponibilidad de varios pasos](../articles/azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests) | 90 días | Este recurso proporciona resultados detallados de cada paso.
| Tamaño máximo del evento | 64 K | 
| Longitud de nombres de propiedades y métricas | 150 | Consulte [esquemas de tipos](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Longitud de cadena del valor de propiedad | 8192 | Consulte [esquemas de tipos](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Longitud del mensaje de seguimiento y excepción | 10 K | Consulte [esquemas de tipos](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Número de [pruebas de disponibilidad](../articles/azure-monitor/app/monitor-web-app-availability.md) por aplicación | 100 |
| Retención de datos del [generador de perfiles](../articles/application-insights/app-insights-profiler.md) | 5 días |
| Datos enviados por día del [generador de perfiles](../articles/application-insights/app-insights-profiler.md) | 10 GB |

Para más información, consulte [Administración de precios y cuotas para Application Insights](../articles/azure-monitor/app/pricing.md).