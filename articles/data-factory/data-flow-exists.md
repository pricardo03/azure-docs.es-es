---
title: Transformación Existe de Azure Data Factory Mapping Data Flow
description: Transformación Existe de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 6ce27ba699ae766ed4d2428f67d91379464bb9f1
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59006144"
---
# <a name="azure-data-factory-mapping-data-flow-exists-transformation"></a>Transformación Existe de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

La transformación Existe es una transformación de filtrado de filas que impide o permite que las filas de datos fluyan. La transformación Existe es similar a ```SQL WHERE EXISTS``` y ```SQL WHERE NOT EXISTS```. Después de la transformación Filtrar, las filas resultantes de la secuencia de datos incluirán todas las filas cuyos valores de columna del origen 1 existen en el origen 2 o no existen en el origen 2.

![Configuración de Existe](media/data-flow/exsits.png "exists 1")

Elija el segundo origen para su transformación Existe para que Data Flow pueda comparar los valores de la secuencia 1 con la secuencia 2.

Seleccione la columna de los orígenes 1 y 2 cuyos valores desea comprobar si existen o no.

## <a name="multiple-exists-conditions"></a>Varias condiciones de existe

Junto a cada fila en las condiciones de la columna para existe, encontrará un + inicio de sesión disponible cuando se desplaza sobre llegar a fila. Esto le permitirá agregar varias filas para las condiciones de Exists.

## <a name="next-steps"></a>Pasos siguientes

