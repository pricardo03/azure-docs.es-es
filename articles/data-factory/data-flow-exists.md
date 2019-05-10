---
title: Transformación de Azure Data Factory datos flujo existe asignación
description: Cómo buscar las filas existentes con los datos de asignación de data factory flujos con la transformación de Exists
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: b98b7afb21f2f50d44ba93ed793b6efb20f75164
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235963"
---
# <a name="mapping-data-flow-exists-transformation"></a>Asignación de flujo de datos existe transformación

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

La transformación Existe es una transformación de filtrado de filas que impide o permite que las filas de datos fluyan. La transformación Existe es similar a ```SQL WHERE EXISTS``` y ```SQL WHERE NOT EXISTS```. Después de la transformación existe, las filas resultantes de la transmisión de datos bien incluirá todas las filas donde los valores de columna del origen 1 existen en el origen 2 o no existe en el origen 2.

![Configuración de Existe](media/data-flow/exists.png "exists 1")

Elija el segundo origen para su transformación Existe para que Data Flow pueda comparar los valores de la secuencia 1 con la secuencia 2.

Seleccione la columna de los orígenes 1 y 2 cuyos valores desea comprobar si existen o no.

## <a name="multiple-exists-conditions"></a>Varias condiciones de existe

Junto a cada fila en las condiciones de la columna para Exists, encontrará un + inicio de sesión disponible cuando se desplaza sobre llegar a fila. Esto le permitirá agregar varias filas para las condiciones de Exists. Cada condición adicional es un "Y".

## <a name="custom-expression"></a>Expresión personalizada

![Existe una configuración personalizada](media/data-flow/exists1.png "existe personalizado")

Puede hacer clic en "Expresión personalizada" para crear una expresión de forma libre como su existe o la condición de no existe. Al activar esta casilla, podrá escribir su propia expresión como una condición.

## <a name="next-steps"></a>Pasos siguientes

Las transformaciones similares son [búsqueda](data-flow-lookup.md) y [unir](data-flow-join.md).
