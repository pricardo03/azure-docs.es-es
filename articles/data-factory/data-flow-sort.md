---
title: Transformación Ordenar de Azure Data Factory Mapping Data Flow
description: Transformación Ordenar de Azure Data Factory Mapping Data
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: f9d2699c870de21daea9ce421c79f089c04b99fa
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029129"
---
# <a name="azure-data-factory-data-flow-sort-transformations"></a>Transformaciones Ordenar de Azure Data Factory Data Flow



![Opciones de Ordenar](media/data-flow/sort.png "Sort")

La transformación Ordenar permite ordenar las filas entrantes de la secuencia de datos actual. Las filas de salida de la transformación Ordenar seguirán después las reglas de ordenación que establezca. Puede seleccionar columnas individuales y ordenarlas de forma ascendente o descendente, con la flecha situada junto a cada campo. Si necesita modificar la columna antes de aplicar la ordenación, haga clic en "Columnas calculadas" para abrir el editor de expresiones. Esto le permitirá crear una expresión para la operación de ordenación en lugar de simplemente aplicar una columna para la ordenación.

## <a name="case-insensitive"></a>No distingue mayúsculas de minúsculas
Activa "No distinguir entre mayúsculas y minúsculas" para ordenar los campos de cadena o texto.

"Ordenar solo dentro de las particiones" aprovecha las particiones de datos de Spark. Al ordenar los datos entrantes solo dentro de cada partición, los flujos de datos puede ordenar los datos con particiones en lugar de ordenar toda la secuencia de datos.

Es posible reorganizar cada una de las condiciones de ordenación de la transformación Ordenar. Si tiene que mover una columna en la prioridad de ordenación, arrástrela con el mouse y llévela hacia arriba o hacia abajo en la lista de ordenación.

Efectos de las particiones en la ordenación

ADF Data Flow se ejecuta en clústeres Spark de macrodatos con datos distribuidos en varios nodos y particiones. A la hora de diseñar el flujo de datos, es importante recordar esto si va a depender de la transformación Ordenar para mantener los datos en el mismo orden. Si decide volver a particionar los datos en una transformación posterior, puede perder la ordenación porque los datos se vuelven a mezclar.

## <a name="next-steps"></a>Pasos siguientes

Después de la ordenación, es posible que quiera usar la [transformación Agregar](data-flow-aggregate.md).
