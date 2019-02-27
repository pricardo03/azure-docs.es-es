---
title: Transformación Ventana de Azure Data Factory Mapping Data Flow
description: Transformación Ventana de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 329125d39c0da403ff078182af2eee74bcd9c84d
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271445"
---
# <a name="azure-data-factory-mapping-data-flow-window-transformation"></a>Transformación Ventana de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

En la transformación Ventana se definen las agregaciones basadas en ventanas de las columnas de las secuencias de datos. En el Generador de expresiones, puede definir diferentes tipos de agregaciones basadas en ventanas de datos o tiempo (cláusula OVER de SQL), como LEAD, LAG, NTILE, CUMEDIST, RANK, etc.). Se generará un nuevo campo en la salida que incluya estas agregaciones. También puede incluir campos de agrupación opcionales.

![Opciones de Ventana](media/data-flow/windows1.png "windows 1")

## <a name="over"></a>Over
Establezca las particiones de los datos de columna para la transformación Ventana. El equivalente en SQL es ```Partition By``` en la cláusula Over de SQL. Si desea crear un cálculo o una expresión para usarlos en la creación de las particiones, desplace el cursor por encima del nombre de la columna y seleccione "Columna calculada".

![Opciones de Ventana](media/data-flow/windows4.png "windows 4")

## <a name="sort"></a>Sort
Otra parte de la cláusula Over es establecer ```Order By```. Esto establecerá el orden de los datos. También puede crear una expresión que genere un valor de cálculo en este campo de columna para ordenar.

![Opciones de Ventana](media/data-flow/windows5.png "windows 5")

## <a name="range-by"></a>Range By
A continuación, enlace o desenlace el marco de la ventana. Para establecer un marco de ventana sin enlazar, establezca el control deslizante en Unbounded (Sin enlazar) en ambos extremos. Si elige un valor entre Unbounded (Sin enlazar) y Current Row (Fila actual), debe establecer los valores de desplazamiento inicial y final. Ambos valores serán números enteros positivos. Puede usar números relativos o valores de sus datos.

El control deslizante de la ventana puede establecer dos valores: valores antes de la fila actual y valores después de la fila actual. Los desplazamientos inicial y final coinciden con los dos selectores del control deslizante.

![Opciones de Ventana](media/data-flow/windows6.png "windows 6")

## <a name="window-columns"></a>Columnas de ventana
Por último, use el Generador de expresiones para definir las agregaciones que desea utilizar con las ventanas de datos, por ejemplo, RANK, COUNT, MIN, MAX, DENSE RANK, LEAD, LAG, etc.

![Opciones de Ventana](media/data-flow/windows7.png "windows 7")

Para ver la lista completa de las funciones de agregación y análisis disponibles para usar en el lenguaje de expresiones del Generador de expresiones de ADF Data Flow, consulte: https://aka.ms/dataflowexpressions.

