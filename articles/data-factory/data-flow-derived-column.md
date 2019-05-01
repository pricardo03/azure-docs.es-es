---
title: Asignación de flujo de datos de Azure Data Factory de transformación columna derivada
description: ¿Cómo transformar datos a escala con Azure Data Factory asignación de datos de flujo de transformación columna derivada
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 6568e5ebf356bb0e6b4ac8ff6059cd093f8da821
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917576"
---
# <a name="mapping-data-flow-derived-column-transformation"></a>Flujo de datos de asignación de transformación columna derivada

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Use la transformación Columna derivada para generar nuevas columnas en el flujo de datos o para modificar los campos existentes.

![columna derivada](media/data-flow/dc1.png "Derived Column")

Puede realizar varias acciones de columna derivada en una sola transformación Columna derivada. Haga clic en "Agregar columna" para transformar más de una columna en el paso de transformación único.

En el campo Columna, seleccione una columna existente para sobrescribirla con un nuevo valor derivado, o haga clic en "Crear nueva columna" para generar una nueva columna con el valor derivado.

El cuadro de texto Expresión abrirá el Generador de expresiones donde puede construir la expresión para las columnas derivadas utilizando funciones de expresión.

## <a name="column-patterns"></a>Patrones de columnas

Si los nombres de columna son variables de los orígenes, puede crear transformaciones dentro de la columna derivada mediante patrones de columnas en lugar de utilizar columnas con nombre. Consulte la [desviación en el esquema](concepts-data-flow-schema-drift.md) artículo para obtener más detalles.

![patrón de columnas](media/data-flow/columnpattern.png "patrones de columnas")

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [lenguaje de expresiones de la factoría de datos para las transformaciones](https://aka.ms/dataflowexpressions) y [generador de expresiones](concepts-data-flow-expression-builder.md)
