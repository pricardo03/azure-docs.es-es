---
title: Generador de expresiones de Azure Data Factory Mapping Data Flow
description: El Generador de expresiones de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: 67a6de6d85a58f48af4761e0b5d5b0a1a4d74b1a
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703411"
---
# <a name="mapping-data-flow-expression-builder"></a>Generador de expresiones de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

En Azure Data Factory Mapping Data Flow, encontrará cuadros de expresión donde puede escribir expresiones para la transformación de datos. En dichos cuadros puede usar columnas, campos, variables, parámetros o funciones de su flujo de datos. Para compilar la expresión, use el Generador de expresiones, que se inicia haciendo clic en el cuadro de texto de expresiones dentro de la transformación. A veces también verá las opciones de "Columna calculada" al seleccionar las columnas para la transformación. Al hacer clic en ella, también verá cómo se ejecuta el Generador de expresiones.

![Generador de expresiones](media/data-flow/xpb1.png "Expression Builder")

El valor predeterminado de la herramienta Generador de expresiones es la opción de editor de texto. La característica Autocompletar lee de todo el modelo de objetos de Azure Data Factory Data Flow con la comprobación y el resaltado de la sintaxis.

![Autocompletar del Generador de expresiones](media/data-flow/expb1.png "Expression Builder auto-complete")

## <a name="build-schemas-in-output-schema-pane"></a>Compilación de esquemas en el panel Output Schema (Esquema de salida)

![Incorporación de columnas complejas](media/data-flow/complexcolumn.png "Incorporación de columnas")

En el panel izquierdo Output Schema (Esquema de salida) verá las columnas que está modificando y agregando al esquema. Aquí puede compilar de manera interactiva estructuras de datos simples y complejas. Agregue campos adicionales mediante "Add column" (Agregar columna) y compile jerarquías mediante "Add subcolumn" (Agregar subcolumna).

![Agregar subcolumna](media/data-flow/addsubcolumn.png "Add Subcolumn")

## <a name="data-preview-in-debug-mode"></a>Vista previa de los datos en el modo de depuración

![Generador de expresiones](media/data-flow/exp4b.png "Vista precia de datos de expresiones")

Cuando trabaja en sus expresiones de flujo de datos, active el modo de depuración desde la superficie de diseño de Azure Data Factory Data Flow para habilitar la vista previa en curso y en directo de los resultados de los datos de la expresión que está compilando. Depuración en vivo en tiempo real está habilitada para las expresiones.

![Modo de depuración](media/data-flow/debugbutton.png "Botón Depurar")

Haga clic en el botón Actualizar para actualizar los resultados de la expresión en un ejemplo en directo del origen en tiempo real.

![Generador de expresiones](media/data-flow/exp5.png "Vista precia de datos de expresiones")

## <a name="comments"></a>Comentarios

Agregue comentarios a sus expresiones mediante la sintaxis de comentarios de una línea y de varias líneas:

![Comentarios](media/data-flow/comments.png "Comments")

## <a name="regular-expressions"></a>Expresiones regulares

El lenguaje de expresiones de Azure Data Factory Mapping Data Flow ([aquí se encuentra toda la documentación](https://aka.ms/dataflowexpressions)) habilita funciones que incluyen la sintaxis de expresiones regulares. Al utilizar las funciones de expresión regular, el Generador de expresiones intentará interpretar la barra diagonal inversa (\\) como una secuencia de caracteres de escape. Al usar barras diagonales inversas en una expresión regular, encierre toda la expresión regular entre marcas (\`) o utilice una doble barra diagonal inversa.

Ejemplo de uso de marcas

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

o de uso de doble barra diagonal

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="addressing-array-indexes"></a>Direccionamiento de los índices de matrices

Con las funciones de expresiones que devuelvan matrices, utilice los corchetes [] para tratar índices específicos dentro del objeto de matriz de devolución. La matriz está basada en unos.

![Matriz de Generador de expresiones](media/data-flow/expb2.png "Vista precia de datos de expresiones")

## <a name="handling-names-with-special-characters"></a>Control de nombres con caracteres especiales

Si tiene nombres de columna que incluyen caracteres especiales o espacios, escriba el nombre entre llaves.
* ```{[dbo].this_is my complex name$$$}```

## <a name="next-steps"></a>Pasos siguientes

[Iniciar la compilación de expresiones de transformación de datos](data-flow-expression-functions.md)
