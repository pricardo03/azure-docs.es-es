---
title: Generador de expresiones de Azure Data Factory Mapping Data Flow
description: El Generador de expresiones de Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 9267360394568f0f9259a3c818b21f4e585fd958
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57543736"
---
# <a name="mapping-data-flow-expression-builder"></a>Generador de expresiones de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

En Azure Data Factory Mapping Data Flow, encontrará cuadros de expresión donde puede escribir expresiones para la transformación de datos. En dichos cuadros puede usar columnas, campos, variables, parámetros o funciones de su flujo de datos. Para compilar la expresión, use el Generador de expresiones, que se inicia haciendo clic en el cuadro de texto de expresiones dentro de la transformación. A veces también verá las opciones de "Columna calculada" al seleccionar las columnas para la transformación. Al hacer clic en ella, también verá cómo se ejecuta el Generador de expresiones.

![Generador de expresiones](media/data-flow/expression.png "Expression Builder")

El valor predeterminado de la herramienta Generador de expresiones es la opción de editor de texto. La característica Autocompletar lee de todo el modelo de objetos de Azure Data Factory Data Flow con la comprobación y el resaltado de la sintaxis.

![Autocompletar del Generador de expresiones](media/data-flow/expb1.png "Expression Builder auto-complete")

## <a name="currently-working-on-field"></a>Campo Currently Working on (Actualmente trabaja en)

![Generador de expresiones](media/data-flow/exp3.png "Currently Working On (Actualmente trabaja en)")

En la parte superior izquierda de la interfaz de usuario del Generador de expresiones, verá un campo denominado "Currently Working On (Actualmente trabaja en)" con el nombre del campo en el que trabaja actualmente. La expresión que genere en la interfaz de usuario se aplicará solo al campo de trabajo actual. Si desea transformar otro campo, guarde el trabajo actual y use esta lista desplegable para seleccionar otro campo, y cree una expresión para los restantes campos.

## <a name="data-preview-in-debug-mode"></a>Vista previa de datos en modo de depuración

![Generador de expresiones](media/data-flow/exp4b.png "Vista precia de datos de expresiones")

Cuando trabaja en sus expresiones, tiene la opción de activar el modo de depuración desde la superficie de diseño de Azure Data Factory Data Flow, lo que habilita la vista previa en curso en vivo de los resultados de los datos de la expresión que está generando. Depuración en vivo en tiempo real está habilitada para las expresiones.

![Modo de depuración](media/data-flow/debugbutton.png "Botón Depurar")


![Generador de expresiones](media/data-flow/exp5.png "Vista precia de datos de expresiones")

## <a name="comments"></a>Comentarios

Agregue comentarios a sus expresiones mediante la sintaxis de comentarios de una línea y de varias líneas:

![Comentarios](media/data-flow/comments.png "Comments")

## <a name="regular-expressions"></a>Expresiones regulares

El lenguaje de expresiones de Azure Data Factory Mapping Data Flow ([aquí se encuentra toda la documentación](https://aka.ms/dataflowexpressions)) habilita funciones que incluyen la sintaxis de expresiones regulares. Al utilizar las funciones de expresión regular, el Generador de expresiones intentará interpretar la barra diagonal inversa (\) como una secuencia de caracteres de escape. Al usar barras diagonales inversas en una expresión regular, encierre toda la expresión regular entre marcas ` ` o utilice una doble barra diagonal inversa.

Ejemplo de uso de marcas

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

o de uso de doble barra diagonal

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="addressing-array-indexes"></a>Direccionamiento de los índices de matriz

Con las funciones de expresiones que devuelvan matrices, utilice los corchetes [] para tratar índices específicos dentro del objeto de matriz de devolución. La matriz está basada en unos.

![Matriz de Generador de expresiones](media/data-flow/expb2.png "Vista precia de datos de expresiones")

## <a name="handling-names-with-special-characters"></a>Controlar nombres con caracteres especiales

Si tiene nombres de columna que incluyen caracteres especiales ni espacios, escriba el nombre entre llaves.
* ```{[dbo].this_is my complex name$$$}```

## <a name="next-steps"></a>Pasos siguientes

[Comience a crear expresiones de transformación de datos](data-flow-expression-functions.md)
