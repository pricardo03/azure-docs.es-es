---
title: 'Consulta de datos desde un entorno de versión preliminar mediante C#: Azure Time Series Insights | Microsoft Docs'
description: Aprenda a consultar datos desde un entorno de Azure Time Series Insights mediante una aplicación escrita en C#.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 12/05/2019
ms.custom: seodec18
ms.openlocfilehash: 20c1f1f9a8b0b0ef105893e44c9daaeae68604db
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74889760"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Consulta de datos desde el entorno de versión preliminar de Azure Time Series Insights mediante C#

En este ejemplo de C# se muestra cómo consultar datos desde el entorno de versión preliminar de Azure Time Series Insights.

El ejemplo muestra varios ejemplos básicos del uso de la API de consulta:

1. Como paso de preparación, se adquiere el token de acceso mediante la API de Azure Active Directory. Pase este token en el encabezado `Authorization` de todas las solicitudes de Query API. Para configurar las aplicaciones no interactivas, consulte [Autenticación y autorización](time-series-insights-authentication-and-authorization.md). Asegúrese también de que todas las constantes definidas al principio del ejemplo se han definido correctamente.
1. Se obtiene la lista de entornos a los que el usuario tiene acceso. Uno de los entornos se elige como entorno de interés y se consultan datos adicionales para este entorno.
1. Como ejemplo de solicitud de HTTPS, se solicitan datos de disponibilidad del entorno de interés.
1. Proporciona un ejemplo de compatibilidad con la generación automática de SDK desde [Azure AutoRest](https://github.com/Azure/AutoRest).

> [!NOTE]
> El código de ejemplo, así como los pasos para su compilación y ejecución, están disponibles en [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample).

## <a name="c-example"></a>Ejemplo de C#

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> El ejemplo de código anterior se puede ejecutar sin modificar los valores del entorno predeterminados.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la consulta, vea la [referencia sobre las API de consulta](https://docs.microsoft.com/rest/api/time-series-insights/preview-query).

- Lea cómo [conectar una aplicación de JavaScript mediante el SDK de cliente](https://github.com/microsoft/tsiclient) a Time Series Insights.
