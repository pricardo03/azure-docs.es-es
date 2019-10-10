---
title: Patrones de columna de Azure Data Factory Mapping Data Flow
description: Cree patrones de transformación de datos generalizados con patrones de columna de Azure Data Factory en Mapping Data Flows.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 535656f315f65ffb7aa241618fe9e73b8246b71f
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72027863"
---
# <a name="mapping-data-flows-column-patterns"></a>Asignación de patrones de columnas de flujos de datos



Varias transformaciones de Azure Data Factory Data Flow admiten la idea de "Patrones de columnas" para que pueda crear columnas de plantilla basadas en patrones, en lugar de nombres de columna codificados de forma rígida. Esta característica se puede usar en Generador de expresiones para definir patrones que coinciden con las columnas para la transformación, en lugar de requerir nombres de campo específicos y exactos. Los patrones son útiles si los campos de origen de entrada cambian a menudo, en concreto en el caso de que cambien columnas en archivos de texto o bases de datos NoSQL. A veces esta condición se denomina "Desviación en el esquema".

Este control de "esquema flexible" se encuentra actualmente en la columna derivada y en las transformaciones Agregar, así como en las transformaciones Seleccionar y Receptor como "asignación basada en reglas".

![patrones de columna](media/data-flow/columnpattern2.png "Column Patterns")

## <a name="column-patterns"></a>Patrones de columnas
Los patrones de columna son útiles para controlar los tanto los escenarios de desviación en el esquema como los escenarios generales. Es bueno en los casos en los que no puede conocer por completo el nombre de todas y cada una de las columnas. Los patrones pueden coincidir por nombre y tipo de datos de columna y crear una expresión para la transformación que ejecutará dicha operación en cualquier campo del flujo de datos que coincida con sus patrones `name` & `type`.

Cuando agregue una expresión a una transformación que acepta patrones, elija "Agregar columna Pattern" (Agregar patrón de columna). Patrones de columnas permite patrones coincidentes de columna con desplazamiento de esquema.

Al crear patrones de las columnas de la plantilla, use `$$` en la expresión para representar una referencia a cada campo coincidente desde el flujo de datos de entrada.

Si elige usar una de las funciones de expresión regular del Generador de expresiones, posteriormente podrá usar $1, $2, $3... para hacer referencia a los patrones secundarios que coinciden con su expresión regular.

Un ejemplo de escenario de patrón de columnas usa SUM con una serie de campos de entrada. Los cálculos de SUM de agregados se encuentran en la transformación Agregado. Posteriormente puede usar SUM en todas las coincidencias de tipos de campos que coincidan con "integer" y, después, usar $$ para hacer referencia a cada coincidencia en la expresión.

## <a name="match-columns"></a>Coincidencia de columnas
![Tipos de patrones de columna](media/data-flow/pattern2.png "Tipos de patrones")

Para crear patrones basados en columnas, puede buscar coincidencias en función del nombre, el tipo, la secuencia o la posición de la columna y usar cualquier combinación de estos con funciones de expresión y expresiones regulares.

![posición de la columna](media/data-flow/position.png "Posición de la columna")

## <a name="rule-based-mapping"></a>Asignación basada en reglas
Cuando asigna columnas a las transformaciones de origen y Seleccionar, tendrá la opción de elegir "Asignación fija" o "Asignación basada en reglas". Si conoce el esquema de los datos y espera columnas específicas del conjunto de datos de origen que siempre coinciden con nombres estáticos específicos, puede usar la asignación fija. Sin embargo, si trabaja con esquemas flexibles, use la asignación basada en reglas. Podrá crear una coincidencia de patrones con las reglas descritas anteriormente.

![Asignación basada en reglas](media/data-flow/rule2.png "Rule based mapping")

Cree las reglas mediante el generador de expresiones. Las expresiones devolverán un valor booleano que coincide con las columnas (true) o que excluya las columnas (false).

## <a name="pattern-matching-special-columns"></a>Columnas especiales de coincidencia de patrones

* `$$` se traducirá al nombre de cada coincidencia en tiempo de diseño en modo de depuración y tras la ejecución en tiempo de ejecución
* `name` representa el nombre de cada columna de entrada
* `type` representa el tipo de datos de cada columna de entrada
* `stream` representa el nombre asociado a cada secuencia o transformación del flujo
* `position` es la posición ordinal de las columnas en el flujo de datos

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre el [lenguaje de expresiones](https://aka.ms/dataflowexpressions) de Mapping Data Flow de ADF para las transformaciones de datos.
* Uso de patrones de columnas en la [transformación de receptor](data-flow-sink.md) y en la [transformación Seleccionar](data-flow-select.md) con asignación basada en reglas
