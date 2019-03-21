---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: 2f5b03dd0170da9a9869183d7a412688509525ef
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56741095"
---
La opción de configuración `ApplicationInsights` le permite agregar compatibilidad con los datos de telemetría de [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) al contenedor. Application Insights proporciona una supervisión detallada del contenedor. Puede supervisar fácilmente la disponibilidad, el rendimiento y el uso del contenedor. También puede identificar y diagnosticar errores en el contenedor rápidamente.

En la tabla siguiente se describen las opciones de configuración compatibles en la sección `ApplicationInsights`.

|Obligatorio| NOMBRE | Tipo de datos | DESCRIPCIÓN |
|--|------|-----------|-------------|
|Sin | `InstrumentationKey` | string | Clave de instrumentación de la instancia de Application Insights para la que se envían los datos de telemetría del contenedor. Para más información, consulte [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Ejemplo:<br>`InstrumentationKey=123456789`|

