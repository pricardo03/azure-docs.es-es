---
title: Transformación Nueva rama de Azure Data Factory Mapping Data Flow
description: Transformación Nueva rama de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 9fac78f21f2f128ccb040e176891c33d39bf2820
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61349159"
---
# <a name="azure-data-factory-mapping-data-flow-new-branch-transformation"></a>Transformación Nueva rama de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![opciones de rama](media/data-flow/menu.png "menu")

La creación de ramas toma la secuencia de datos actual en el flujo de datos y la replica en otra secuencia. Use Nueva rama para realizar varios conjuntos de operaciones y transformaciones en la misma secuencia de datos.

Ejemplo: El flujo de datos tiene una transformación de origen con un conjunto de columnas y conversiones de tipos de datos seleccionado. Después, coloca una columna derivada inmediatamente después de ese origen. En la columna derivada, crea un nuevo campo que combina el nombre y el apellido para crear un nuevo campo "nombre completo".

Puede tratar esa nueva secuencia con un conjunto de transformaciones y un receptor en una fila y usar Nueva rama para crear una copia de la secuencia en la que puede transformar esos mismos datos con un conjunto de transformaciones diferente. Al transformar los datos copiados en una rama independiente, podrá enviarlos a una ubicación diferente.

> [!NOTE]
> "Nueva rama" solo se muestra como una acción en el menú + Transformación cuando hay una transformación posterior después de la ubicación actual en la que está intentando crear la rama. Esto quiere decir que no verá una opción "Nueva rama" hasta que agregue otra transformación después de Seleccionar.

![Rama](media/data-flow/branch2.png "Branch 2")
