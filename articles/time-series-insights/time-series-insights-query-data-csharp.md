---
title: Consultar datos en un entorno de disponibilidad general de Azure tiempo Series Insights mediante C# código | Microsoft Docs
description: En este artículo se describe cómo consultar datos de un entorno de Azure Time Series Insights mediante la creación de una aplicación personalizada escrita en el lenguaje de .NET C# (c-sharp).
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
reviewer: jasonwhowell, kfile, tsidocs
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: 250dd691c3ef3146d6768123de52bf0628b10e42
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66728965"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Consultar datos desde el entorno de disponibilidad general de Azure tiempo Series Insights medianteC#

Esto C# en el ejemplo se muestra cómo consultar datos desde el entorno de disponibilidad general de Azure tiempo Series Insights.

El ejemplo muestra varios ejemplos básicos del uso de la API de consulta:

1. Como paso de preparación, se adquiere el token de acceso mediante la API de Azure Active Directory. Pase este token en el encabezado `Authorization` de todas las solicitudes de Query API. Para configurar las aplicaciones no interactivas, consulte [Autenticación y autorización](time-series-insights-authentication-and-authorization.md). Asegúrese también de que todas las constantes definidas al principio del ejemplo se han definido correctamente.
1. Se obtiene la lista de entornos a los que el usuario tiene acceso. Uno de los entornos se elige como entorno de interés y se consultan datos adicionales para este entorno.
1. Como ejemplo de solicitud de HTTPS, se solicitan datos de disponibilidad del entorno de interés.
1. Como ejemplo de solicitud de web socket, se solicitan datos de agregación de eventos del entorno de interés. Se solicitan datos de todo el intervalo de tiempo de disponibilidad.

> [!NOTE]
> El código de ejemplo está disponible en [ https://github.com/Azure-Samples/Azure-Time-Series-Insights ](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample).

## <a name="project-dependencies"></a>Dependencias del proyecto

Agregar paquetes de NuGet `Microsoft.IdentityModel.Clients.ActiveDirectory` y `Newtonsoft.Json`.

## <a name="c-example"></a>Ejemplo de C#

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de la consulta, lea el [referencia de API de consulta](/rest/api/time-series-insights/ga-query-api).

- Lea cómo a [conectar una aplicación de página única de JavaScript](tutorial-create-tsi-sample-spa.md) a Time Series Insights.