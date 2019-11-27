---
title: Consulta de datos desde un entorno de GA mediante código de C# (Azure Time Series Insights) | Microsoft Docs
description: Aprenda a consultar datos desde un entorno de Azure Time Series Insights mediante una aplicación personalizada escrita en C#.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/03/2019
ms.custom: seodec18
ms.openlocfilehash: 5946a1484a62b041772f388b0629d131afe37b92
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74012548"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Consulta de datos desde el entorno de Azure Time Series Insights GA mediante C#

En este ejemplo de C# se muestra cómo consultar datos desde el entorno de Azure Time Series Insights GA.

El ejemplo muestra varios ejemplos básicos del uso de la API de consulta:

1. Como paso de preparación, se adquiere el token de acceso mediante la API de Azure Active Directory. Pase este token en el encabezado `Authorization` de todas las solicitudes de Query API. Para configurar las aplicaciones no interactivas, consulte [Autenticación y autorización](time-series-insights-authentication-and-authorization.md). Asegúrese también de que todas las constantes definidas al principio del ejemplo se han definido correctamente.
1. Se obtiene la lista de entornos a los que el usuario tiene acceso. Uno de los entornos se elige como entorno de interés y se consultan datos adicionales para este entorno.
1. Como ejemplo de solicitud de HTTPS, se solicitan datos de disponibilidad del entorno de interés.
1. Como ejemplo de solicitud de web socket, se solicitan datos de agregación de eventos del entorno de interés. Se solicitan datos de todo el intervalo de tiempo de disponibilidad.

> [!NOTE]
> El código de ejemplo está disponible en [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample).

## <a name="project-dependencies"></a>Dependencias del proyecto

Agregue los paquetes NuGet `Microsoft.IdentityModel.Clients.ActiveDirectory` y `Newtonsoft.Json`.

## <a name="c-example"></a>Ejemplo de C#

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la consulta, vea la [referencia sobre las API de consulta](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api).

- Lea cómo [conectar una aplicación de JavaScript mediante el SDK de cliente](https://github.com/microsoft/tsiclient) a Time Series Insights.