---
title: Consulta de datos desde un entorno de versión preliminar de Azure Time Series Insights mediante código de C# | Microsoft Docs
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
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: da4e9626b2bf5b180cddba012af81059dd8b67fe
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883284"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Consulta de datos desde el entorno de versión preliminar de Azure Time Series Insights mediante C#

En este ejemplo de C# se muestra cómo consultar datos desde el entorno de versión preliminar de Azure Time Series Insights.

El ejemplo muestra varios ejemplos básicos del uso de la API de consulta:

1. Como paso de preparación, se adquiere el token de acceso mediante la API de Azure Active Directory. Pase este token en el encabezado `Authorization` de todas las solicitudes de Query API. Para configurar las aplicaciones no interactivas, consulte [Autenticación y autorización](time-series-insights-authentication-and-authorization.md). Asegúrese también de que todas las constantes definidas al principio del ejemplo se han definido correctamente.
1. Se obtiene la lista de entornos a los que el usuario tiene acceso. Uno de los entornos se elige como entorno de interés y se consultan datos adicionales para este entorno.
1. Como ejemplo de solicitud de HTTPS, se solicitan datos de disponibilidad del entorno de interés.
1. Como ejemplo de solicitud de web socket, se solicitan datos de agregación de eventos del entorno de interés. Se solicitan datos de todo el intervalo de tiempo de disponibilidad.

> [!NOTE]
> Este código de ejemplo también está disponible en [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample).

## <a name="c-example"></a>Ejemplo de C#

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> El ejemplo de código anterior se puede ejecutar sin modificar los valores del entorno predeterminados.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la consulta, vea la [referencia sobre las API de consulta](https://docs.microsoft.com/rest/api/time-series-insights/preview-query).

- Lea cómo [conectar una aplicación de página única de JavaScript](tutorial-create-tsi-sample-spa.md) a Time Series Insights.