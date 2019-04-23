---
title: Uso de grandes conjuntos de datos
description: Aprenda a obtener y controlar grandes conjuntos de datos mientras trabaja con Azure Resource Graph.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/01/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: 729e9fe749212942c6dc18fc7d6301934e7dd184
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59788245"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Uso de grandes conjuntos de datos de recursos de Azure

Azure Resource Graph está diseñado para trabajar con el entorno de Azure y obtener información sobre los recursos que contiene. Resource Graph permite obtener estos datos de manera más rápida, incluso cuando se consultan miles de registros. Resource Graph presenta varias opciones de funcionamiento con estos grandes conjuntos de datos.

## <a name="data-set-result-size"></a>Tamaño de resultados del conjunto de datos

De forma predeterminada, en Resource Graph hay un límite en el número de registros que se pueden devolver en una consulta, y son **100** registros. Este control protege tanto al usuario como al servicio de consultas no intencionadas que darían lugar a grandes conjuntos de datos. Este caso suele suceder cuando un cliente experimenta con consultas para buscar y filtrar los recursos de la manera en que se adapte a sus necesidades concretas. Este control es diferente a usar los operadores de lenguaje [top](/azure/kusto/query/topoperator) o [limit](/azure/kusto/query/limitoperator) de Azure Data Explorer para limitar los resultados.

> [!NOTE]
> Cuando se usa **primera**, se recomienda para ordenar los resultados por al menos una columna con `asc` o `desc`. Sin clasificación, los resultados devueltos sean aleatorias y no repetibles.

El límite predeterminado se puede invalidar mediante todos los métodos de interacción con Resource Graph. En los ejemplos siguientes se muestra cómo cambiar el límite de tamaño del conjunto de datos a _200_:

```azurecli-interactive
az graph query -q "project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -First 200
```

En la [API REST](/rest/api/azureresourcegraph/resources/resources), el control es **$top** y forma parte de **QueryRequestOptions**.

El control que sea _más restrictivo_ ganará. Por ejemplo, si la consulta usa los operadores **top** o **limit** y el resultado va a ser un número de registros superior a **First**, el número máximo de registros devueltos será igual a **First**. Igualmente, si **top** o **limit** son más pequeños que **First**, el conjunto de registros devuelto sería el valor más pequeño configurado por **top** o **limit**.

**First** tiene actualmente un valor permitido máximo de _5000_.

## <a name="skipping-records"></a>Omisión de registros

La siguiente opción para trabajar con grandes conjuntos de datos es el control **Skip**. Este control permite que la consulta salte u omita el número definido de registros antes de devolver los resultados. **Skip** es útil con consultas que ordenan los resultados de una manera significativa donde la intención es obtener los recursos que se encuentran hacia la mitad del conjunto de resultados. Si los resultados necesarios están al final del conjunto de datos devuelto, es mejor usar una configuración de ordenación diferente y recuperar los resultados del principio del conjunto de datos.

> [!NOTE]
> Cuando se usa **omitir**, se recomienda para ordenar los resultados por al menos una columna con `asc` o `desc`. Sin clasificación, los resultados devueltos sean aleatorias y no repetibles.

En los ejemplos siguientes se muestra cómo omitir los primeros _10_ registros que devolvería una consulta, en lugar de comenzar el conjunto de resultados devuelto con el registro número 11:

```azurecli-interactive
az graph query -q "project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -Skip 10
```

En la [API REST](/rest/api/azureresourcegraph/resources/resources), el control es **$skip** y forma parte de **QueryRequestOptions**.

## <a name="paging-results"></a>Paginación de resultados

Cuando es necesario dividir un conjunto de resultados en conjuntos más pequeños de registros para el procesamiento o porque un conjunto de resultados, se superará el valor máximo permitido de _1000_ registros devueltos, use la paginación. La [API REST](/rest/api/azureresourcegraph/resources/resources) **QueryResponse** proporciona valores para indicar que un conjunto de resultados se ha dividido: **resultTruncated** y **$skipToken**.
**resultTruncated** es un valor booleano que informa al consumidor si existen registros adicionales no devueltos en la respuesta. Esta condición también se puede identificar cuando la propiedad **count** es menor que la propiedad **totalRecords**. **totalRecords** define cuántos registros coinciden con la consulta.

Cuando **resultTruncated** es **true**, la propiedad **$skipToken** se establece en la respuesta. Este valor se usa con los mismos valores de consulta y suscripción para obtener el siguiente conjunto de registros que coinciden con la consulta.

> [!IMPORTANT]
> La consulta debe **proyectar** el campo **id** para que la paginación funcione. Si no aparece en la consulta, la respuesta de API de REST no incluirá el **$skipToken**.

Para ver un ejemplo, consulte [Next page query](/rest/api/azureresourcegraph/resources/resources#next_page_query) (Consulta de página siguiente) en la documentación de la API REST.

## <a name="next-steps"></a>Pasos siguientes

- Ver el lenguaje en uso en el artículo de[consultas básicas](../samples/starter.md)
- Consultar usos avanzados en el artículo de [consultas avanzadas](../samples/advanced.md)
- Aprender a [explorar recursos](explore-resources.md)