---
title: Transformación Clave suplente de Azure Data Factory Mapping Data Flow
description: Cómo usar asignación de flujo de suplente clave transformación de Azure Data Factory de datos para generar valores de clave secuenciales
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: eaa1c577f7e208400d3430222b006e0dbbd7956a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61350618"
---
# <a name="mapping-data-flow-surrogate-key-transformation"></a>Transformación de clave de suplente de flujo de datos de asignación

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Para agregar un valor de clave incremental arbitrario no empresarial al conjunto de filas de su flujo de datos, utilice la transformación Clave suplente. Resulta útil para diseñar tablas de dimensiones en un modelo de datos analíticos con un esquema de estrella, donde cada miembro de las tablas de dimensión debe tener una clave única que no sea empresarial, como parte de la metodología Kimball DW.

![Transformación Clave suplente](media/data-flow/surrogate.png "Surrogate Key Transformation")

"Columna de clave" es el nombre que dará a la nueva columna de clave suplente.

"Valor inicial" es el punto de partida del valor incremental.

## <a name="increment-keys-from-existing-sources"></a>Claves de incremento de orígenes existentes

Si desea iniciar la secuencia de un valor que existe en un origen, puede utilizar una transformación columna derivada inmediatamente después de la transformación de la clave suplente y sumar los dos valores:

![SK agregar Max](media/data-flow/sk006.png "suplente clave transformación Agregar máx.")

Para inicializar el valor de clave con el número máximo de anterior, hay dos técnicas que puede usar:

### <a name="database-sources"></a>Orígenes de base de datos

Use la opción de "Consulta" para seleccionar MAX() desde el origen de la transformación de origen:

![Suplentes consulta clave](media/data-flow/sk002.png "suplentes de la consulta de transformación de clave")

### <a name="file-sources"></a>Orígenes de archivo

Si el valor máximo anterior está en un archivo, puede usar la transformación de origen junto con una transformación agregado y use la función MAX() de expresión para obtener el máximo valor anterior:

![Archivo de clave de suplentes](media/data-flow/sk008.png "sustituta de archivo de clave")

En ambos casos, debe unir los datos entrantes del nuevo junto con el origen que contiene el valor máximo anterior:

![Combinación de claves de suplentes](media/data-flow/sk004.png "suplentes de la combinación de claves")

## <a name="next-steps"></a>Pasos siguientes

Estos ejemplos se usa el [unir](data-flow-join.md) y [columna derivada](data-flow-derived-column.md) transformaciones.
