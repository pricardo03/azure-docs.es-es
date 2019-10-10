---
title: Transformación Existe de la asignación de Data Flow en Azure Data Factory
description: Cómo buscar filas existentes mediante la asignación de Data Flow en Data Factory con la transformación Exists
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 8b488a079b2da1bcf0dd064025ed251a1dc25213
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029391"
---
# <a name="mapping-data-flow-exists-transformation"></a>Transformación Existe de la asignación de Data Flow



La transformación Existe es una transformación de filtrado de filas que impide o permite que las filas de datos fluyan. La transformación Existe es similar a ```SQL WHERE EXISTS``` y ```SQL WHERE NOT EXISTS```. Después de la transformación Existe, las filas resultantes de la secuencia de datos incluirán todas las filas cuyos valores de columna del origen 1 existen en el origen 2 o no existen en el origen 2.

![Configuración de Existe](media/data-flow/exists.png "exists 1")

Elija el segundo origen para su transformación Existe para que Data Flow pueda comparar los valores de la secuencia 1 con la secuencia 2.

Seleccione la columna de los orígenes 1 y 2 cuyos valores desea comprobar si existen o no.

## <a name="multiple-exists-conditions"></a>Varias condiciones Existe

Junto a cada fila de las condiciones de columna de Existe, encontrará un signo + disponible al mantener el puntero sobre cada fila. Este signo le permitirá agregar varias filas para las condiciones de Existe. Cada condición adicional es una operación "Y".

## <a name="custom-expression"></a>Expresión personalizada

![Configuración personalizada de Existe](media/data-flow/exists1.png "Existe personalizado")

Puede hacer clic en "Expresión personalizada" para crear una expresión de forma libre como condición Existe o No existe. Al activar esta casilla, podrá escribir su propia expresión como una condición.

## <a name="next-steps"></a>Pasos siguientes

Otras transformaciones similares son [Búsqueda](data-flow-lookup.md) y [Combinación](data-flow-join.md).
