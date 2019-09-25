---
title: Transformación División condicional de Azure Data Factory Mapping Data Flow
description: Transformación División condicional de Azure Data Factory Data Flow
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: dd51cc2d5f95598154b76b5da8e3fc9e4801100d
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104945"
---
# <a name="mapping-data-flow-conditional-split-transformation"></a>Transformación División condicional de asignación de flujo de datos

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Cuadro de herramientas de División condicional](media/data-flow/conditionalsplit2.png "conditional split toolbox")

La transformación División condicional puede redirigir filas de datos a diferentes secuencias según el contenido de los datos. La implementación de la transformación División condicional es similar a una estructura de decisión CASE en un lenguaje de programación. La transformación evalúa las expresiones y, según los resultados, dirige la fila de datos a la secuencia especificada. Esta transformación también proporciona una salida predeterminada, por lo que si una fila no coincide con ninguna expresión, se dirige a la salida predeterminada.

![División condicional](media/data-flow/conditionalsplit1.png "conditional split options")

## <a name="multiple-paths"></a>Varias rutas de acceso

Para agregar condiciones adicionales, seleccione "Agregar secuencia" en el panel de configuración de la parte inferior y haga clic en el cuadro de texto Generador de expresiones para crear la expresión.

![División condicional múltiple](media/data-flow/conditionalsplit3.png "conditional split multi")

## <a name="next-steps"></a>Pasos siguientes

Transformaciones de flujo de datos comunes utilizadas con División condicional: [transformación de combinación](data-flow-join.md), [transformación de búsqueda](data-flow-lookup.md), [transformación de selección](data-flow-select.md)
