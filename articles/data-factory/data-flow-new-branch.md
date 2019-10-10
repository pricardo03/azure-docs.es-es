---
title: Transformación Nueva rama de Azure Data Factory Mapping Data Flow
description: Transformación Nueva rama de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 3f443396627a4bbaba2a3eeb0a3ac05cc1597c85
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029295"
---
# <a name="azure-data-factory-mapping-data-flow-new-branch-transformation"></a>Transformación Nueva rama de Azure Data Factory Mapping Data Flow



![opciones de rama](media/data-flow/menu.png "menu")

La creación de ramas toma la secuencia de datos actual en el flujo de datos y la replica en otra secuencia. Use Nueva rama para realizar varios conjuntos de operaciones y transformaciones en la misma secuencia de datos.

Ejemplo: El flujo de datos tiene una transformación de origen con un conjunto de columnas y conversiones de tipos de datos seleccionado. Después, coloca una columna derivada inmediatamente después de ese origen. En la columna derivada, crea un nuevo campo que combina el nombre y el apellido para crear un nuevo campo "nombre completo".

Puede tratar esa nueva secuencia con un conjunto de transformaciones y un receptor en una fila y usar Nueva rama para crear una copia de la secuencia en la que puede transformar esos mismos datos con un conjunto de transformaciones diferente. Al transformar los datos copiados en una rama independiente, podrá enviarlos a una ubicación diferente.

> [!NOTE]
> "Nueva rama" solo se muestra como una acción en el menú + Transformación cuando hay una transformación posterior después de la ubicación actual en la que está intentando crear la rama. Esto quiere decir que no verá una opción "Nueva rama" hasta que agregue otra transformación después de Seleccionar.

![Rama](media/data-flow/branch2.png "Branch 2")
