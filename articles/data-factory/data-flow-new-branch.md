---
title: Transformación Nueva rama del flujo de datos de asignación
description: Transformación Nueva rama de flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 02/12/2019
ms.openlocfilehash: b4617689fe1ab14856bde9a4e8134b12aa6d815b
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930313"
---
# <a name="azure-data-factory-mapping-data-flow-new-branch-transformation"></a>Transformación Nueva rama de flujo de datos de asignación de Azure Data Factory

![Opciones de rama](media/data-flow/menu.png "menú")

La creación de ramas toma la secuencia de datos actual en el flujo de datos y la replica en otra secuencia. Use Nueva rama para realizar varios conjuntos de operaciones y transformaciones en la misma secuencia de datos.

Ejemplo: El flujo de datos tiene una transformación de origen con un conjunto de columnas y conversiones de tipos de datos seleccionado. Después, coloca una columna derivada inmediatamente después de ese origen. En la columna derivada, crea un nuevo campo que combina el nombre y el apellido para crear un nuevo campo "nombre completo".

Puede tratar esa nueva secuencia con un conjunto de transformaciones y un receptor en una fila y usar Nueva rama para crear una copia de la secuencia en la que puede transformar esos mismos datos con un conjunto de transformaciones diferente. Al transformar los datos copiados en una rama independiente, podrá enviarlos a una ubicación diferente.

> [!NOTE]
> "Nueva rama" solo se muestra como una acción en el menú + Transformación cuando hay una transformación posterior después de la ubicación actual en la que está intentando crear la rama. Esto quiere decir que no verá una opción "Nueva rama" hasta que agregue otra transformación después de Seleccionar.

![Rama](media/data-flow/branch2.png "Rama 2")
