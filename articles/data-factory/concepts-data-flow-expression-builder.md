---
title: Generador de expresiones del flujo de datos de asignación
description: Genere expresiones con el generador de expresiones de los flujos de datos de asignación en Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 12/9/2019
ms.openlocfilehash: 01aa2574ac6edd1ce5e1b209eac3e43bbed82fce
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74969402"
---
# <a name="building-expressions-in-mapping-data-flow"></a>Generación de expresiones del flujo de datos de asignación

En el flujo de datos de asignación, muchas propiedades de transformación se especifican como expresiones. Estas expresiones se componen de valores de columna, parámetros, funciones, operadores y literales que se evalúan como un tipo de datos de Spark en tiempo de ejecución.

## <a name="opening-the-expression-builder"></a>Apertura del generador de expresiones

La interfaz de edición de expresiones de la experiencia de usuario de Data Factory se conoce como el **generador de expresiones**. Al indicar su lógica de expresión, la factoría de datos usa la finalización de código de [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense?view=vs-2019) para el resaltado, la comprobación de sintaxis y la finalización automática.

![Generador de expresiones](media/data-flow/xpb1.png "Generador de expresiones")

En transformaciones como la columna derivada y el filtro, donde las expresiones son obligatorias, abra el generador de expresiones haciendo clic en el cuadro de expresión azul.

![Generador de expresiones](media/data-flow/expressionbox.png "Generador de expresiones")

Al hacer referencia a columnas en una coincidencia o grupo por condición, una expresión puede extraer valores de las columnas. Para crear una expresión, seleccione la opción "columna calculada".

![Generador de expresiones](media/data-flow/computedcolumn.png "Generador de expresiones")

En los casos en los que una expresión o un valor literal son entradas válidas, "agregar contenido dinámico" le permitirá crear una expresión que se evalúe como un literal.

![Generador de expresiones](media/data-flow/add-dynamic-content.png "Generador de expresiones")

## <a name="expression-language-reference"></a>Referencia de lenguaje de expresiones

La asignación de flujos de datos tiene funciones y operadores integrados que se pueden usar en las expresiones. En la página de referencia del [lenguaje de expresiones del flujo de datos de asignación](data-flow-expression-functions.md) encontrará una lista de funciones disponibles.

## <a name="column-names-with-special-characters"></a>Nombres de columna con caracteres especiales

Si tiene nombres de columna que incluyen caracteres especiales o espacios, escriba el nombre entre llaves para hacer referencia a estos en una expresión.

```{[dbo].this_is my complex name$$$}```

## <a name="previewing-expression-results"></a>Vista previa de los resultados de la expresión

Si el [modo de depuración](concepts-data-flow-debug-mode.md) está activado, puede usar el clúster de Spark activo para ver una versión preliminar en curso de cómo se evalúa su expresión. Al crear su lógica, puede depurar su expresión en tiempo real. 

![Generador de expresiones](media/data-flow/exp4b.png "Vista previa de datos de expresión")

Haga clic en el botón Actualizar para actualizar los resultados de la expresión en un ejemplo en directo del origen.

![Generador de expresiones](media/data-flow/exp5.png "Vista previa de datos de expresión")

## <a name="string-interpolation"></a>Interpolación de cadena

Use comillas dobles para incluir texto de cadena literal junto con expresiones. Puede incluir parámetros, columnas y funciones de expresión. La interpolación de cadena es útil para evitar el uso extensivo de la concatenación de cadenas al incluir parámetros en cadenas de consulta. Para usar la sintaxis de expresión, escríbala entre llaves.

Algunos ejemplos de interpolación de cadena:

* ```"My favorite movie is {iif(instr(title,', The')>0,"The {split(title,', The')[1]}",title)}"```

* ```"select * from {$tablename} where orderyear > {$year}"```

* ```"Total cost with sales tax is {round(totalcost * 1.08,2)}"```

## <a name="commenting-expressions"></a>Comentarios de las expresiones

Agregue comentarios a sus expresiones mediante la sintaxis de comentarios de una línea y de varias líneas:

![Comentarios](media/data-flow/comments.png "Comentarios")

A continuación se muestran ejemplos de comentarios válidos:

* ```/* This is my comment */```

* ```/* This is a```
*   ```multi-line comment */```
   
* ```// This is a single line comment```

Si coloca un comentario al comienzo de la expresión, aparecerá en el cuadro de texto de transformación para documentar las expresiones de transformación:

![Comentarios](media/data-flow/comments2.png "Comentarios")

## <a name="regular-expressions"></a>Expresiones regulares

Muchas funciones de lenguaje de expresiones usan la sintaxis de expresión regular. Al utilizar las funciones de expresión regular, el Generador de expresiones intentará interpretar la barra diagonal inversa (\\) como una secuencia de caracteres de escape. Al usar barras diagonales inversas en una expresión regular, encierre toda la expresión regular entre marcas (\`) o utilice una doble barra diagonal inversa.

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

![Matriz del Generador de expresiones](media/data-flow/expb2.png "Vista previa de datos de expresión")

## <a name="keyboard-shortcuts"></a>Métodos abreviados de teclado

* ```Ctrl-K Ctrl-C```: Comentar en toda la línea
* ```Ctrl-K Ctrl-U```: Quitar marca de comentario
* ```F1```: Proporcionar comandos de ayuda del editor
* ```Alt-Down Arrow```: Bajar la línea actual
* ```Alt-Up Arrow```: Subir la línea actual
* ```Cntrl-Space```: Mostrar ayuda contextual

## <a name="convert-to-dates-or-timestamps"></a>Conversión en fechas o marcas de tiempo

Para incluir literales de cadena en la salida de la marca de tiempo, debe ajustar la conversión en ```toString()```.

```toString(toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss'), 'MM/dd /yyyy\'T\'HH:mm:ss')```

Para convertir los milisegundos de la época a una fecha o marca de tiempo, use `toTimestamp(<number of milliseconds>)`. Si el tiempo estará disponible en segundos, multiplíquelo por 1000.

```toTimestamp(1574127407*1000l)```

El signo "l" final al final de la expresión anterior indica que hay una conversión a un tipo long como sintaxis insertada.

## <a name="next-steps"></a>Pasos siguientes

[Iniciar la compilación de expresiones de transformación de datos](data-flow-expression-functions.md)
