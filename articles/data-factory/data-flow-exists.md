---
title: Transformación Existe de Azure Data Factory Mapping Data Flow
description: Transformación Existe de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 18d8a0e231e8b4dbe33911dd6267966674366904
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56734496"
---
# <a name="azure-data-factory-mapping-data-flow-exists-transformation"></a>Transformación Existe de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

La transformación Existe es una transformación de filtrado de filas que impide o permite que las filas de datos fluyan. La transformación Existe es similar a ```SQL WHERE EXISTS``` y ```SQL WHERE NOT EXISTS```. Después de la transformación Filtrar, las filas resultantes de la secuencia de datos incluirán todas las filas cuyos valores de columna del origen 1 existen en el origen 2 o no existen en el origen 2.

![Configuración de Existe](media/data-flow/exsits.png "exists 1")

Elija el segundo origen para su transformación Existe para que Data Flow pueda comparar los valores de la secuencia 1 con la secuencia 2.

Seleccione la columna de los orígenes 1 y 2 cuyos valores desea comprobar si existen o no.
