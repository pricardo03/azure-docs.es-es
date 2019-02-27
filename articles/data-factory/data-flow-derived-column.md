---
title: Transformación Columna derivada de Azure Data Factory Data Flow
description: Transformación Columna derivada de Azure Data Factory Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: aee005aed52563e214e24148af2563fd7869de76
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271432"
---
# <a name="azure-data-factory-data-flow-derived-column-transformation"></a>Transformación Columna derivada de Azure Data Factory Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Use la transformación Columna derivada para generar nuevas columnas en el flujo de datos o para modificar los campos existentes.

![columna derivada](media/data-flow/dc1.png "Derived Column")

Puede realizar varias acciones de columna derivada en una sola transformación Columna derivada. Haga clic en "Agregar columna" para transformar más de una columna en el paso de transformación único.

En el campo Columna, seleccione una columna existente para sobrescribirla con un nuevo valor derivado, o haga clic en "Crear nueva columna" para generar una nueva columna con el valor derivado.

El cuadro de texto Expresión abrirá el Generador de expresiones donde puede construir la expresión para las columnas derivadas utilizando funciones de expresión.
