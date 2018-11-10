---
title: Información del lenguaje de consulta de Azure Resource Graph
description: Describe cómo funciona el lenguaje de consulta de Azure Resource Graph.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/22/2018
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 09bcedc5250755f06ba23b84a0ae90b4d43a23db
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086172"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Información del lenguaje de consulta de Azure Resource Graph

El lenguaje de consulta de Azure Resource Graph admite numerosos operadores y funciones. Cada uno de ellos trabaja y opera en función de [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md).

La mejor manera de obtener información sobre el lenguaje de consultas que Resource Graph utiliza es empezar con la documentación del [lenguaje de consultas](/azure/kusto/query/index) de Azure Data Explorer. En esta documentación, se explica cómo está estructurado el lenguaje y cómo trabajan juntos los distintos operadores y funciones compatibles.

## <a name="supported-tabular-operators"></a>Operadores tabulares compatibles

Esta es la lista de los operadores tabulares compatibles en Resource Graph:

- [count](/azure/kusto/query/countoperator)
- [distinct](/azure/kusto/query/distinctoperator)
- [extend](/azure/kusto/query/extendoperator)
- [limit](/azure/kusto/query/limitoperator)
- [order by](/azure/kusto/query/orderoperator)
- [project](/azure/kusto/query/projectoperator)
- [project-away](/azure/kusto/query/projectawayoperator)
- [sample](/azure/kusto/query/sampleoperator)
- [sample-distinct](/azure/kusto/query/sampledistinctoperator)
- [sort by](/azure/kusto/query/sortoperator)
- [summarize](/azure/kusto/query/summarizeoperator)
- [take](/azure/kusto/query/takeoperator)
- [top](/azure/kusto/query/topoperator)
- [top-nested](/azure/kusto/query/topnestedoperator)
- [top-hitters](/azure/kusto/query/tophittersoperator)
- [where](/azure/kusto/query/whereoperator)

## <a name="supported-functions"></a>Funciones compatibles

Esta es la lista de las funciones compatibles en Resource Graph:

- [ago()](/azure/kusto/query/agofunction)
- [buildschema()](/azure/kusto/query/buildschema-aggfunction)
- [strcat()](/azure/kusto/query/strcatfunction)
- [isnotempty()](/azure/kusto/query/isnotemptyfunction)
- [tostring()](/azure/kusto/query/tostringfunction)

## <a name="next-steps"></a>Pasos siguientes

- Ver el lenguaje en uso en el artículo de[consultas básicas](../samples/starter.md)
- Consultar usos avanzados en el artículo de [consultas avanzadas](../samples/advanced.md)
- Aprender a [explorar recursos](explore-resources.md)
