---
title: Transformación Nueva rama de Azure Data Factory Mapping Data Flow
description: Transformación Nueva rama de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: af2225d749283c7124f89d5a7cd735b2f6bfd121
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792623"
---
# <a name="mapping-data-flow-union-transformation"></a>Transformación Unión de flujo de datos de asignación

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Unión combina varias secuencias de datos en una, y la unión SQL de esas secuencias será la salida de la transformación Unión. Todo el esquema de cada secuencia de entrada se combina dentro de su flujo de datos, sin necesidad de tener una clave de combinación.

Puede combinar n-el número de flujos en la tabla de configuración, seleccione el icono "+" junto a cada fila configurada, incluidos datos de origen así como flujos procedentes de las transformaciones existentes en el flujo de datos.

![Transformación Unión](media/data-flow/union.png "Union")

En este caso, puede combinar los metadatos dispares de varios orígenes (en este ejemplo, tres archivos de origen diferente) y combinarlos en una única secuencia:

![Información general de la transformación Unión](media/data-flow/union111.png "unión 1")

Para lograr esto, agregue filas adicionales en la configuración de la unión mediante la inclusión de todo el código fuente que desea agregar. No hay ninguna necesidad de una clave común de búsqueda o unión:

![Configuración de transformación Unión](media/data-flow/unionsettings.png "configuración de unión")

Si establece una transformación seleccione después de la unión, podrá cambiar el nombre de campos o los campos que no se han llamado desde orígenes headerless superpuestas. Haga clic en "Inspeccionar" para ver los metadatos se combinan con 132 columnas total en este ejemplo de tres orígenes diferentes:

![Transformación Unión final](media/data-flow/union333.png "unión 3")

## <a name="name-and-position"></a>Nombre y la posición

Al elegir "unión por nombre", cada valor de columna se quitará en la columna correspondiente de cada origen, con un nuevo esquema de metadatos concatenados.

Si elige "unión por posición", cada valor de columna se quitará en la posición original de cada origen correspondiente, lo que resulta en un nuevo flujo combinado de datos donde los datos de cada origen se agregan a la misma secuencia:

![Salida de unión](media/data-flow/unionoutput.png "salida unión")

## <a name="next-steps"></a>Pasos siguientes

Explorar las transformaciones similar incluido [unir](data-flow-join.md) y [Exists](data-flow-exists.md).
