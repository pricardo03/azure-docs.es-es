---
title: Transformación División condicional en el flujo de datos de asignación de Azure Data Factory
description: Transformación División condicional de Azure Data Factory Data Flow
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: d06b5b86737d0940930a3ccea3b6d65be0a802f9
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387902"
---
# <a name="mapping-data-flow-conditional-split-transformation"></a>Transformación División condicional de asignación de flujo de datos



![cuadro de herramientas de división condicional](media/data-flow/conditionalsplit2.png "cuadro de herramientas de división condicional")

La transformación División condicional puede redirigir filas de datos a diferentes secuencias según el contenido de los datos. La implementación de la transformación División condicional es similar a una estructura de decisión CASE en un lenguaje de programación. La transformación evalúa las expresiones y, según los resultados, dirige la fila de datos a la secuencia especificada. Esta transformación también proporciona una salida predeterminada, por lo que si una fila no coincide con ninguna expresión, se dirige a la salida predeterminada.

![división condicional](media/data-flow/conditionalsplit1.png "opciones de división condicional")

## <a name="multiple-paths"></a>Varias rutas de acceso

Para agregar condiciones adicionales, seleccione "Agregar secuencia" en el panel de configuración de la parte inferior y haga clic en el cuadro de texto Generador de expresiones para crear la expresión.

![división condicional múltiple](media/data-flow/conditionalsplit3.png "división condicional múltiple")

## <a name="next-steps"></a>Pasos siguientes

Transformaciones de flujo de datos comunes utilizadas con División condicional: [transformación de combinación](data-flow-join.md), [transformación de búsqueda](data-flow-lookup.md), [transformación de selección](data-flow-select.md)
