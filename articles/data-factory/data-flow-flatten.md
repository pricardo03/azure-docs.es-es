---
title: Transformación Aplanar de Mapping Data Flow
description: Transformación Aplanar de Mapping Data Flow de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 415a093fd8a8fbe27e1d240b061548e18f2ca6b6
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2020
ms.locfileid: "78164736"
---
# <a name="azure-data-factory-flatten-transformation"></a>Transformación Aplanar de Azure Data Factory

La transformación Aplanar se puede usar para dinamizar los valores de matriz dentro de una estructura jerárquica en filas nuevas, esencialmente desnormalizando los datos.

![Cuadro de herramientas de la transformación](media/data-flow/flatten5.png "Cuadro de herramientas de la transformación")

![Transformación Aplanar 1](media/data-flow/flatten7.png "Transformación Aplanar 1")

## <a name="unroll-by"></a>Expandir por

En primer lugar, elija la columna de la matriz que quiere expandir y dinamizar.

![Configuración de la transformación Aplanar](media/data-flow/flatten1.png "Configuración de la transformación Aplanar")

## <a name="unroll-root"></a>Expandir la raíz

De manera predeterminada, ADF aplanará la estructura en la matriz de expansión que eligió anteriormente. O bien puede elegir una parte distinta de la jerarquía para expandirla. "Expandir la raíz" es una configuración opcional.

## <a name="input-columns"></a>Columnas de entrada

Por último, elija la proyección de la estructura nueva en función de los campos de entrada, así como la columna normalizada que expandió.

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream>
foldDown(unroll(<unroll cols>),
    mapColumn(
        name,
        each(<array>(type == '<arrayDataType>')),
        each(<array>, match(true())),
        location
    )) ~> <transformationName>
```

### <a name="example"></a>Ejemplo

```
source(output(
        name as string,
        location as string,
        satellites as string[],
        goods as (trade as boolean, customers as string[], orders as (orderId as string, orderTotal as double, shipped as (orderItems as (itemName as string, itemQty as string)[]))[])
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source2
source2 foldDown(unroll(goods.orders.shipped.orderItems),
    mapColumn(
        name,
        each(goods.orders, match(type == 'integer')),
        each(goods.orders.shipped.orderItems, match(true())),
        location
    )) ~> Flatten1
Flatten1 sink(allowSchemaDrift: true,
    validateSchema: false,
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> sink1
```    

## <a name="next-steps"></a>Pasos siguientes

* Use la [transformación Dinamizar](data-flow-pivot.md) para dinamizar las filas en columnas.
* Use la [transformación Anular dinamización](data-flow-unpivot.md) para dinamizar las columnas a filas.
