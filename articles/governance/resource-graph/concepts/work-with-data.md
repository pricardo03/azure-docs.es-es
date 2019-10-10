---
title: Uso de grandes conjuntos de datos
description: Aprenda a obtener y controlar grandes conjuntos de datos mientras trabaja con Azure Resource Graph.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/01/2019
ms.topic: conceptual
ms.service: resource-graph
ms.openlocfilehash: 4da890a5ef7acb44d0e8628dc4ec3904f6a065e4
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2019
ms.locfileid: "71980320"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Uso de grandes conjuntos de datos de recursos de Azure

Azure Resource Graph está diseñado para trabajar con el entorno de Azure y obtener información sobre los recursos que contiene. Resource Graph permite obtener estos datos de manera más rápida, incluso cuando se consultan miles de registros. Resource Graph presenta varias opciones de funcionamiento con estos grandes conjuntos de datos.

Para instrucciones sobre cómo trabajar con consultas con mucha frecuencia, consulte [Guía para solicitudes limitadas](./guidance-for-throttled-requests.md).

## <a name="data-set-result-size"></a>Tamaño de resultados del conjunto de datos

De forma predeterminada, en Resource Graph hay un límite en el número de registros que se pueden devolver en una consulta, y son **100** registros. Este control protege tanto al usuario como al servicio de consultas no intencionadas que darían lugar a grandes conjuntos de datos. Este caso suele suceder cuando un cliente experimenta con consultas para buscar y filtrar los recursos de la manera en que se adapte a sus necesidades concretas. Este control es diferente a usar los operadores de lenguaje [top](/azure/kusto/query/topoperator) o [limit](/azure/kusto/query/limitoperator) de Azure Data Explorer para limitar los resultados.

> [!NOTE]
> Al usar la opción **Primero**, se recomienda ordenar los resultados con al menos una columna con `asc` o `desc`. Si los resultados están sin clasificar, los resultados devueltos son aleatorios y no se pueden repetir.

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
> Al usar la opción **Omitir**, se recomienda ordenar los resultados con al menos una columna con `asc` o `desc`. Si los resultados están sin clasificar, los resultados devueltos son aleatorios y no se pueden repetir.

En los ejemplos siguientes se muestra cómo omitir los primeros _10_ registros que devolvería una consulta, en lugar de comenzar el conjunto de resultados devuelto con el registro número 11:

```azurecli-interactive
az graph query -q "project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -Skip 10
```

En la [API REST](/rest/api/azureresourcegraph/resources/resources), el control es **$skip** y forma parte de **QueryRequestOptions**.

## <a name="paging-results"></a>Paginación de resultados

Cuando sea necesario dividir un conjunto de resultados en conjuntos de registros más pequeños para su procesamiento, o porque un conjunto de resultados superaría el valor máximo permitido de _1000_ registros devueltos, use la paginación. La [API REST](/rest/api/azureresourcegraph/resources/resources) **QueryResponse** proporciona valores para indicar que un conjunto de resultados se ha dividido: **resultTruncated** y **$skipToken**.
**resultTruncated** es un valor booleano que informa al consumidor si existen registros adicionales no devueltos en la respuesta. Esta condición también se puede identificar cuando la propiedad **count** es menor que la propiedad **totalRecords**. **totalRecords** define cuántos registros coinciden con la consulta.

Cuando **resultTruncated** es **true**, la propiedad **$skipToken** se establece en la respuesta. Este valor se usa con los mismos valores de consulta y suscripción para obtener el siguiente conjunto de registros que coinciden con la consulta.

Los ejemplos siguientes muestran cómo **omitir** los primeros 3000 registros y cómo devolver los **primeros** 1000 registros después de los omitidos con la CLI de Azure y Azure PowerShell:

```azurecli-interactive
az graph query -q "project id, name | order by id asc" --first 1000 --skip 3000
```

```azurepowershell-interactive
Search-AzGraph -Query "project id, name | order by id asc" -First 1000 -Skip 3000
```

> [!IMPORTANT]
> La consulta debe **proyectar** el campo **id** para que la paginación funcione. Si no aparece en la consulta, la respuesta no incluirá **$skipToken**.

Para ver un ejemplo, consulte [Next page query](/rest/api/azureresourcegraph/resources/resources#next-page-query) (Consulta de página siguiente) en la documentación de la API REST.

## <a name="next-steps"></a>Pasos siguientes

- Consulte el lenguaje en uso en[Consultas básicas](../samples/starter.md).
- Consulte los usos avanzados en [Consultas avanzadas](../samples/advanced.md).
- Más información sobre la [exploración de recursos](explore-resources.md).