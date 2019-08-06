---
title: Patrones de columna de Azure Data Factory Mapping Data Flow
description: Cree patrones de transformación de datos generalizados con patrones de columna de Azure Data Factory en Mapping Data Flows.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 076c3318a68a50e6bd1b4f9f2a4a4b9a034533c6
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68346576"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Patrones de columna de Azure Data Factory Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Varias transformaciones de Azure Data Factory Data Flow admiten la idea de "Patrones de columnas" para que pueda crear columnas de plantilla basadas en patrones, en lugar de nombres de columna codificados de forma rígida. Esta característica se puede usar en Generador de expresiones para definir patrones que coinciden con las columnas para la transformación, en lugar de requerir nombres de campo específicos y exactos. Los patrones son útiles si los campos de origen de entrada cambian a menudo, en concreto en el caso de que cambien columnas en archivos de texto o bases de datos NoSQL. A veces esta condición se denomina "Desviación en el esquema".

![patrones de columna](media/data-flow/columnpattern2.png "Column Patterns")

Los patrones de columna son útiles para controlar los tanto los escenarios de desviación en el esquema como los escenarios generales. Es bueno en los casos en los que no puede conocer por completo el nombre de todas y cada una de las columnas. Los patrones pueden coincidir por nombre y tipo de datos de columna y crear una expresión para la transformación que ejecutará dicha operación en cualquier campo del flujo de datos que coincida con sus patrones `name` & `type`.

Cuando agregue una expresión a una transformación que acepta patrones, elija "Agregar columna Pattern" (Agregar patrón de columna). Patrones de columnas permite patrones coincidentes de columna con desplazamiento de esquema.

Al crear patrones de las columnas de la plantilla, use `$$` en la expresión para representar una referencia a cada campo coincidente desde el flujo de datos de entrada.

Si elige usar una de las funciones de expresión regular del Generador de expresiones, posteriormente podrá usar $1, $2, $3... para hacer referencia a los patrones secundarios que coinciden con su expresión regular.

Un ejemplo de escenario de patrón de columnas usa SUM con una serie de campos de entrada. Los cálculos de SUM de agregados se encuentran en la transformación Agregado. Posteriormente puede usar SUM en todas las coincidencias de tipos de campos que coincidan con "integer" y, después, usar $$ para hacer referencia a cada coincidencia en la expresión.

## <a name="match-columns"></a>Coincidencia de columnas
![Tipos de patrones de columna](media/data-flow/pattern2.png "Tipos de patrones")

Para crear patrones basados en columnas, puede buscar coincidencias en función del nombre, el tipo, la secuencia o la posición de la columna y usar cualquier combinación de estos con funciones de expresión y expresiones regulares.

![posición de la columna](media/data-flow/position.png "Posición de la columna")

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre el [lenguaje de expresiones](https://aka.ms/dataflowexpressions) de Mapping Data Flow de ADF para las transformaciones de datos.
