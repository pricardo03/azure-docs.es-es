---
title: Patrones de columna en el flujo de datos de asignación de Azure Data Factory
description: Cree patrones generalizados de transformación de datos mediante patrones de columna en flujos de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/21/2019
ms.openlocfilehash: 0c9a3c2ef05f4a11933ca7fc81c7c0f87a612293
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72789909"
---
# <a name="using-column-patterns-in-mapping-data-flow"></a>Uso de patrones de columnas en el flujo de datos de asignación

Varias transformaciones de flujo de datos de asignación permiten hacer referencia a columnas de plantilla en función de patrones en lugar de nombres de columna codificados de forma rígida. Esta coincidencia se conoce como *patrones de columna*. Puede definir patrones para que coincidan con las columnas según el nombre, el tipo de datos, la secuencia o la posición, en lugar de requerir nombres de campo exactos. Hay dos escenarios en los que resultan útiles los patrones de columna:

* Si los campos de origen de entrada cambian a menudo, como en el caso de columnas que cambian en los archivos de texto o bases de datos NoSQL. Este escenario se conoce como [desfase de esquema](concepts-data-flow-schema-drift.md).
* Si desea realizar una operación común en un grupo grande de columnas. Por ejemplo, si desea convertir cada columna que contiene "total" en el nombre de columna en un valor doble.

Los patrones de columna están disponibles actualmente en las transformaciones de columna derivada, de agregado, de selección y de receptor.

## <a name="column-patterns-in-derived-column-and-aggregate"></a>Patrones de columna en columna derivada y de agregado

Para agregar un patrón de columna en una columna derivada o en la pestaña Agregados de una transformación de agregado, haga clic en el icono de signo más situado a la derecha de una columna existente. Seleccione **Add column pattern** (Agregar patrón de columna). 

![patrones de columna](media/data-flow/columnpattern.png "Patrones de columnas")

Use el [generador de expresiones](concepts-data-flow-expression-builder.md) para escribir la condición de coincidencia. Cree una expresión booleana que coincida con las columnas en función de los elementos `name`, `type`, `stream` y `position` de la columna. El patrón afectará a cualquier columna, desfasada o definida, donde la condición devuelva true.

Los dos cuadros de expresión situados debajo de la condición de coincidencia especifican los nuevos nombres y valores de las columnas afectadas. Use `$$` para hacer referencia al valor existente del campo coincidente. El cuadro de expresión de la izquierda define el nombre y el cuadro de expresión de la derecha define el valor.

![patrones de columna](media/data-flow/columnpattern2.png "Patrones de columnas")

El patrón de columna anterior coincide con cada columna de tipo de datos doble y crea una columna de agregado por coincidencia. El nombre de la nueva columna es el nombre de la columna coincidente concatenado con "_total". El valor de la nueva columna es la suma redondeada y agregada del valor doble existente.

Para comprobar que la condición de coincidencia es correcta, puede validar el esquema de salida de las columnas definidas en la pestaña **Inspeccionar** u obtener una instantánea de los datos en la pestaña **Vista previa de los datos**. 

![patrones de columna](media/data-flow/columnpattern3.png "Patrones de columnas")

## <a name="rule-based-mapping-in-select-and-sink"></a>Asignación basada en reglas en selección y receptor

Cuando asigna columnas en transformaciones de origen y selección, puede agregar asignación fija o asignación basada en reglas. Si conoce el esquema de los datos y espera que columnas específicas del conjunto de datos de origen coincidan siempre con nombres estáticos específicos, use la asignación fija. Si trabaja con esquemas flexibles, use la asignación basada en reglas para generar una coincidencia de patrones basada en los elementos `name`, `type`, `stream` y `position` de las columnas. Puede usar cualquier combinación de asignaciones basadas en reglas y fijas. 

Para agregar una asignación basada en reglas, haga clic en **Agregar asignación** y seleccione **Rule based mapping** (Asignación basada en reglas).

![asignación basada en reglas](media/data-flow/rule2.png "Asignación basada en reglas")

En el cuadro de expresión de la izquierda, escriba la condición de coincidencia booleana. En el cuadro de expresión de la derecha, especifique a qué se asignará la columna coincidente. Use `$$` para hacer referencia al nombre existente del campo coincidente.

Si hace clic en el icono del botón de contenido adicional hacia abajo, puede especificar una condición de asignación de regex.

Haga clic en el icono de gafas situado a junto a una asignación basada en reglas para ver qué columnas definidas coinciden y a qué están asignadas.

![asignación basada en reglas](media/data-flow/rule1.png "Asignación basada en reglas")

En el ejemplo anterior, se crean dos asignaciones basadas en reglas. La primera toma todas las columnas que no tienen el nombre "movie" y las asigna a los valores existentes. La segunda regla usa regex para hacer coincidir todas las columnas que comienzan por "movie" y las asigna a la columna "movieId".

Si la regla produce varias asignaciones idénticas, habilite **Skip duplicate inputs** (Omitir entradas duplicadas) o **Skip duplicate outputs** (Omitir salidas duplicadas) para evitar duplicados.

## <a name="pattern-matching-expression-values"></a>Valores de expresión de coincidencia de patrones.

* `$$` traduce al nombre o valor de cada coincidencia en tiempo de ejecución
* `name` representa el nombre de cada columna de entrada
* `type` representa el tipo de datos de cada columna de entrada
* `stream` representa el nombre asociado a cada secuencia o transformación del flujo
* `position` es la posición ordinal de las columnas en el flujo de datos

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre el [lenguaje de expresiones](data-flow-expression-functions.md) del flujo de datos de asignación para las transformaciones de datos.
* Uso de patrones de columnas en la [transformación de receptor](data-flow-sink.md) y en la [transformación de selección](data-flow-select.md) con asignación basada en reglas.
