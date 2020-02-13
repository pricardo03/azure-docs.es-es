---
title: 'Split Data: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Dividir datos en Azure Machine Learning para dividir un conjunto de datos en dos conjuntos distintos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 3e831e58b47d53e2924956cab13568c69bc1432e
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153747"
---
# <a name="split-data-module"></a>Módulo Split Data

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Use este módulo para dividir un conjunto de datos en dos conjuntos distintos.

Este módulo es especialmente útil cuando necesita separar los datos en conjuntos de entrenamiento y de prueba. Además, puede personalizar la forma en que los datos se dividen. Algunas opciones admiten la selección aleatoria de datos; otras están adaptadas a un determinado tipo de datos o tipo de modelo.

## <a name="how-to-configure"></a>Cómo se configura

> [!TIP]
> Antes de elegir el modo de división, lea todas las opciones para determinar el tipo de división que necesita.
> Si cambia el modo de división, se podrían restablecer todas las demás opciones.

1. Agregue el módulo **Split Data** a la canalización en el diseñador. Puede encontrar este módulo en **Transformación de datos**, en la categoría **Sample and Split** (Muestrear y dividir).

2. **Splitting mode** (Modo de división): Elija uno de los modos siguientes, según el tipo de datos que tenga y cómo quiera dividirlos. Cada modo de división tiene distintas opciones. Haga clic en los temas siguientes para obtener instrucciones detalladas y ejemplos. 

    - **Split Rows** (Dividir filas): Use esta opción si quiere dividir los datos en dos partes. Puede especificar el porcentaje de datos que se colocará en cada división, pero de forma predeterminada, los datos se dividen 50 y 50.

        También puede hacer la selección de filas de cada grupo de forma aleatoria y usar muestreo estratificado. En el muestreo estratificado, debe seleccionar una sola columna de datos para la que quiera que los valores se distribuyan por igual entre los dos conjuntos de datos de resultados.  

    - **Regular Expression Split** (División de expresión regular): Elija esta opción cuando quiera dividir el conjunto de datos probando una sola columna para un valor.

        Por ejemplo, si va a analizar las opiniones, podría comprobar la presencia de un nombre de producto en particular en un campo de texto y, luego, dividir el conjunto de datos en filas que contienen el nombre de producto de destino y otras que no lo contienen.

    - **Relative Expression Split** (División de expresión relativa):  Use esta opción siempre que quiera aplicar una condición a una columna de números. El número podría ser un campo de fecha y hora, una columna que contiene edades o importes monetarios, incluso un porcentaje. Por ejemplo, es posible que quiera dividir el conjunto de datos según el costo de los artículos, agrupar personas por intervalos de edad, o separar datos por una fecha del calendario.

### <a name="split-rows"></a>Dividir filas

1.  Agregue el módulo [Dividir datos](./split-data.md) a la canalización en el diseñador y conecte el conjunto de datos que quiera dividir.
  
2.  Para **Splitting mode**, elija **Split rows**. 

3.  **Fraction of rows in the first output dataset** (Fracción de filas del primer conjunto de datos de salida). Use esta opción para determinar cuántas filas pertenecen a la primera salida (izquierda). Todas las demás filas irán a la segunda salida (derecha).

    El coeficiente representa el porcentaje de filas que se envían para el primer conjunto de datos de salida, por lo que debe escribir un número decimal comprendido entre 0 y 1.
     
     Por ejemplo, si escribe 0,75 como valor, el conjunto de datos se dividiría usando una relación de 75:25, donde el 75 % de las filas se enviará al primer conjunto de datos de salida, y el 25 % se enviará al segundo conjunto de datos de salida.
  
4. Seleccione la opción **Randomized split** (División aleatoria) si quiere realizar la selección de datos en los dos grupos de forma aleatoria. Esta es la opción preferida cuando se crean conjuntos de datos de entrenamiento y de prueba.

5.  **Valor de inicialización aleatorio**: Escriba un valor entero no negativo para inicializar la secuencia seudoaleatoria de instancias que se usará. Este valor de inicialización predeterminado se usa en todos los módulos que generan números aleatorios. 

     Especificar un valor de inicialización hace que los resultados sean reproducibles en general. Si tiene que repetir los resultados de una operación de división, debe especificar un valor de inicialización para el generador de números aleatorios. De lo contrario, el valor de inicialización aleatorio se establece de forma predeterminada en 0, lo que significa que el valor de inicialización inicial se obtiene a partir del reloj del sistema. Como consecuencia, la distribución de datos puede ser ligeramente diferente cada vez que haga una división. 

6. **Stratified split** (División estratificada): Establezca esta opción en **True** para garantizar que los dos conjuntos de datos de resultados contengan una muestra representativa de los valores de la *columna de estratos* o la *columna de clave de estratificación*. 

    Con el muestreo estratificado, los datos se dividen de forma que cada conjunto de datos de salida obtiene aproximadamente el mismo porcentaje de cada valor de destino. Por ejemplo, es posible que quiera asegurarse de que sus conjuntos de entrenamiento y de pruebas estén más o menos equilibrados en relación con el resultado, o con respecto a alguna otra columna, como el género.

7. Ejecución de la canalización


## <a name="regular-expression-split"></a>División de expresiones regulares

1.  Agregue el módulo [Split Data](./split-data.md) a la canalización y conéctelo como entrada en el conjunto de datos que quiere dividir.  
  
2.  Para **Splitting mode**, seleccione **Regular expression split**.

3. En el cuadro **Expresión regular**, escriba una expresión regular válida. 
  
   La expresión regular debe seguir la sintaxis de expresión regular de Python.


4. Ejecución de la canalización

    En función de la expresión regular que proporcione, el conjunto de datos se divide en dos conjuntos de filas: las filas con valores que coinciden con la expresión y todas las filas restantes. 

## <a name="relative-expression-split"></a>División de expresión relativa

1. Agregue el módulo [Split Data](./split-data.md) a la canalización y conéctelo como entrada en el conjunto de datos que quiere dividir.
  
2. Para **Splitting mode**, seleccione **Relative expression split**.
  
3. En el cuadro de texto **Expresión relacional**, escriba una expresión que realice una operación de comparación, en una sola columna:


 - Columna numérica:
    - La columna contiene números de cualquier tipo de datos numéricos, incluidos los tipos de datos de fecha y hora.

    - La expresión puede hacer referencia a un máximo de un nombre de columna.

    - Utilice el carácter "y" comercial (&) para la operación AND y use el carácter de barra vertical (|) para la operación OR.

    - Se admiten los siguientes operadores: `<`, `>`, `<=`, `>=`, `==`, `!=`.

    - No puede agrupar las operaciones con `(` y `)`.

 - Columna de cadenas: 
    - Se admiten los siguientes operadores: `==`, `!=`.



4. Ejecución de la canalización

    La expresión divide el conjunto de datos en dos conjuntos de filas: las filas con valores que cumplen la condición y todas las filas restantes.

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 