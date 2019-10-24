---
title: Transformación Clave suplente de flujo de datos de asignación de Azure Data Factory
description: Uso de la transformación Clave suplente de flujo de datos de asignación de Azure Data Factory para generar valores de claves secuenciales
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 144d6298a13d35d94a68b35c443a3a47cefcfc2a
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387162"
---
# <a name="mapping-data-flow-surrogate-key-transformation"></a>Transformación Clave suplente de flujo de datos de asignación



Para agregar un valor de clave incremental arbitrario no empresarial al conjunto de filas de su flujo de datos, utilice la transformación Clave suplente. Resulta útil para diseñar tablas de dimensiones en un modelo de datos analíticos con un esquema de estrella, donde cada miembro de las tablas de dimensión debe tener una clave única que no sea empresarial, como parte de la metodología Kimball DW.

![Transformación Clave suplente](media/data-flow/surrogate.png "Transformación Clave suplente")

"Columna de clave" es el nombre que dará a la nueva columna de clave suplente.

"Valor inicial" es el punto de partida del valor incremental.

## <a name="increment-keys-from-existing-sources"></a>Incrementar claves a partir de orígenes existentes

Si quiere iniciar la secuencia desde un valor que existe en un origen, puede utilizar una transformación Columna derivada inmediatamente después de la transformación Clave suplente y agregar los dos valores juntos:

![Agregar máximo de Clave suplente](media/data-flow/sk006.png "Agregar máximo de transformación Clave suplente")

Para inicializar el valor de clave con el máximo anterior, puede usar dos técnicas:

### <a name="database-sources"></a>Orígenes de base de datos

Use la opción "Consulta" para seleccionar MAX() desde el origen mediante la transformación Origen:

![Consulta de clave suplente](media/data-flow/sk002.png "Consulta de transformación Clave suplente")

### <a name="file-sources"></a>Orígenes de archivo

Si el valor máximo anterior está en un archivo, puede usar la transformación Origen junto con una transformación Agregar y usar la función de expresión MAX() para obtener el valor máximo anterior:

![Archivo de clave suplente](media/data-flow/sk008.png "Archivo de clave suplente")

En ambos casos, debe unir los nuevos datos entrantes junto con el origen que contiene el valor máximo anterior:

![Unión de clave suplente](media/data-flow/sk004.png "Unión de clave suplente")

## <a name="next-steps"></a>Pasos siguientes

En estos ejemplos se usan las transformaciones [Combinación](data-flow-join.md) y [Columna derivada](data-flow-derived-column.md).
