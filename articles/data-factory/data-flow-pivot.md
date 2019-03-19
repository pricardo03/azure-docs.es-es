---
title: Transformación Dinamizar de Azure Data Factory Mapping Data Flow
description: Transformación Dinamizar de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 5548a62218aaac2e4da3853e8e5d43a584922bc0
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569899"
---
# <a name="azure-data-factory-mapping-data-flow-pivot-transformation"></a>Transformación Dinamizar de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Utiliza Dinamizar en ADF Data Flow como una agregación en la que los valores distintivos de una o más columnas de agrupación se transforman en columnas individuales. Básicamente, puede dinamizar valores de fila en nuevas columnas (convertir datos en metadatos).

![Opciones de dinamización](media/data-flow/pivot1.png "pivot 1")

## <a name="group-by"></a>Agrupar por

![Opciones de dinamización](media/data-flow/pivot2.png "pivot 2")

En primer lugar, establezca las columnas que desea agrupar para la agregación dinámica. Puede establecer más de una columna con el signo + que aparece junto a la lista de columnas.

## <a name="pivot-key"></a>Clave dinámica

![Opciones de dinamización](media/data-flow/pivot3.png "pivot 3")

La clave dinámica es la columna que ADF dinamizará de fila a columna. De forma predeterminada, cada valor único del conjunto de datos de este campo se dinamizará en una columna. Sin embargo, también puede escribir los valores del conjunto de datos que desea dinamizar en valores de columna.

## <a name="pivoted-columns"></a>Columnas dinamizadas

![Opciones de dinamización](media/data-flow/pivot4.png "pivot 4")

Por último, elegirá la agregación que se va a usar para los valores dinamizados y cómo desea que las columnas se muestren en la nueva proyección de salida de la transformación.

(Opcional) Puede establecer un patrón de nomenclatura con un prefijo, un término intermedio y el sufijo que se agregará a los nuevos nombres de columna creados a partir de los valores de fila.

Por ejemplo, dinamizar "Ventas" por "Región" daría lugar a nuevos valores de columna a partir de cada valor de ventas; es decir: "25", "50", "1000", etc. Sin embargo, si establece un valor de prefijo de "Venta-", cada valor de columna agregaría "Sales-" al principio del valor.

![Opciones de dinamización](media/data-flow/pivot5.png "pivot 5")

Al establecer la organización de la columna en "Normal", se agruparán todas las columnas dinámicas con sus valores agregados. Al establecer la organización de las columnas en "Lateral", se alternará entre columna y valor.

### <a name="aggregation"></a>Agregación

Para establecer la agregación que se va a usar para los valores dinámicos, haga clic en el campo situado en la parte inferior del panel Columnas dinamizadas. Se abrirá el Generador de expresiones de ADF Data Flow, donde puede crear una expresión de agregación y proporcionar un nombre de alias descriptivo para los nuevos valores agregados.

Use el lenguaje de expresiones de ADF Data Flow para describir las transformaciones de columna dinamizada en el Generador de expresiones: https://aka.ms/dataflowexpressions.

### <a name="how-to-rejoin-original-fields"></a>Cómo volver a combinar los campos originales
> [!NOTE]
> La transformación dinámica solo proyectará las columnas utilizadas en la acción de agregación, agrupación y dinamización. Si desea incluir las demás columnas en el paso anterior en el flujo, use una nueva rama en el paso anterior y usar el patrón de autocombinación para conectar el flujo con los metadatos originales.

## <a name="next-steps"></a>Pasos siguientes

Pruebe el [transformación Anulación de dinamización](data-flow-unpivot.md) para convertir los valores de columna en los valores de fila. 
