---
title: 'Dividir los datos: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo dividir datos en el servicio de Azure Machine Learning para dividir un conjunto de datos en dos conjuntos distintos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: a7395280ed9a2e9dcb94a081f0b3bf10a28da719
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029406"
---
# <a name="split-data-module"></a>Módulo dividir datos

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para dividir un conjunto de datos en dos conjuntos distintos.

Este módulo es especialmente útil cuando necesita separar los datos de entrenamiento y conjuntos de pruebas. Puede personalizar la forma en que los datos se dividen también. Algunas opciones admiten selección aleatoria de datos. otros usuarios están adaptados para un determinado tipo de datos o tipo de modelo.

## <a name="how-to-configure"></a>Configuración

> [!TIP]
> Antes de elegir el modo de división, lea todas las opciones para determinar el tipo de división que necesita.
> Si cambia el modo de división, se podrían restablecer todas las demás opciones.

1. Agregar el **dividir datos** módulo al experimento en la interfaz. Puede encontrar este módulo en **transformación de datos**, en el **muestrear y dividir** categoría.

2. **Modo de división**: Elija uno de los modos siguientes, según el tipo de datos que tiene y cómo desea dividirlo. Cada modo de división tiene distintas opciones. Haga clic en los temas siguientes para obtener instrucciones detalladas y ejemplos. 

    - **Dividir filas**: Use esta opción si desea dividir los datos en dos partes. Puede especificar el porcentaje de datos que se colocarán en cada división, pero de forma predeterminada, los datos se dividen entre 50 y 50.

        También puede hacer la selección de filas de cada grupo de forma aleatoria y usar muestreo estratificado. En el muestreo estratificado, debe seleccionar una sola columna de datos para el que desea que se distribuyen por igual entre los conjuntos de datos de dos resultado valores.  

    - **División de expresión regular** elija esta opción cuando desee que se va a dividir el conjunto de datos mediante la comprobación de una sola columna para un valor.

        Por ejemplo, si va a analizar las opiniones, podría comprobar la presencia de un nombre de producto en particular en un campo de texto y, a continuación, dividir el conjunto de datos en las filas con el nombre de producto de destino y aquellos sin.

    - **División de expresión relativa**:  Use esta opción siempre que desee aplicar una condición a una columna de número. El número podría ser un campo de fecha y hora, una antigüedad de contenedor de la columna o importes o incluso un porcentaje. Por ejemplo, es posible que desee dividir el conjunto de datos según el costo de los elementos, el grupo de personas por intervalos de edad o datos separados por una fecha del calendario.

### <a name="split-rows"></a>Dividir filas
1.  Agregar el [dividir datos](./split-data.md) módulo al experimento en la interfaz y conecte el conjunto de datos que desea dividir.
  
2.  Para **modo de división**, elija **Dividir filas**. 

3.  **Fracción de filas del primer conjunto de datos de salida**. Use esta opción para determinar cuántas filas pertenecen a la primera salida (izquierda). Todas las demás filas irá a la segunda salida (derecha).

    El coeficiente representa el porcentaje de filas que se envían para el primer conjunto de datos de salida, por lo que debe escribir un número decimal comprendido entre 0 y 1.
     
     Por ejemplo, si escribe 0,75 como valor, el conjunto de datos se dividiría usando una relación de 75: 25, con el 75% de las filas enviadas al primer conjunto de datos de salida y el 25% enviado al segundo conjunto de datos de salida.
  
4. Seleccione el **división aleatoria** opción si desea realizar la selección de datos en los dos grupos de forma aleatoria. Esta es la opción preferida cuando se crean los conjuntos de datos de entrenamiento y prueba.

5.  **Valor de inicialización aleatorio**: Escriba un valor entero no negativo para inicializar la secuencia seudoaleatoria de instancias que se usará. Este valor de inicialización predeterminado se utiliza en todos los módulos que generan números aleatorios. 

     Especificar un valor de inicialización hace que los resultados sean generalmente reproducibles. Si tiene que repetir los resultados de una operación de división, debe especificar un valor de inicialización para el generador de números aleatorios. En caso contrario, el valor de inicialización aleatorio se establece de forma predeterminada en 0, lo que significa que el valor de inicialización inicial se obtiene del reloj del sistema. Como resultado, la distribución de datos puede ser ligeramente diferente cada vez que realice una división. 

6. **División estratificada**: Establezca esta opción en **True** para garantizar que los conjuntos de datos de dos resultados contienen una muestra representativa de los valores de la *columna de estratos* o *columna de clave de estratificación*. 

    Con el muestreo estratificado, los datos se dividen de forma que cada conjunto de datos de salida obtiene aproximadamente el mismo porcentaje de cada valor de destino. Por ejemplo, es posible que desee asegurarse de que su entrenamiento y conjuntos de pruebas están equilibrados aproximadamente en relación con el resultado, o con respecto a ot alguna otra columna, como el sexo.

7. Ejecute el experimento.


## <a name="regular-expression-split"></a>División de expresión regular

1.  Agregar el [dividir datos](./split-data.md) módulo para el experimento y conéctelo como entrada para el conjunto de datos que desea dividir.  
  
2.  Para **modo de división**, seleccione **división de expresión Regular**.

3. En el **expresión Regular** , escriba una expresión regular válida. 
  
   La expresión regular debe seguir la sintaxis de expresión regular de Python.


4. Ejecute el experimento.

    En función de la expresión regular que proporcione, el conjunto de datos se divide en dos conjuntos de filas: las filas con valores que coinciden con la expresión y todas las filas restantes. 

## <a name="relative-expression-split"></a>División de expresión relativa.

1. Agregar el [dividir datos](./split-data.md) módulo para el experimento y conéctelo como entrada para el conjunto de datos que desea dividir.
  
2. Para **modo de división**, seleccione **división de expresión relativa**.
  
3. En el **expresión relacional** cuadro de texto, escriba una expresión que se realiza una operación de comparación, en una sola columna:


 - Columna numérica:
    - La columna contiene números de cualquier tipo de datos numéricos, incluidos los tipos de datos de fecha y hora.

    - La expresión puede hacer referencia a un máximo de un nombre de columna.

    - Utilice el carácter "y" comercial (&) para la operación y y use el carácter de barra vertical (|) para la operación de OR.

    - Se admiten los siguientes operadores: `<`, `>`, `<=`, `>=`, `==`, `!=`

    - No se puede agrupar las operaciones con `(` y `)`.

 - Columna de cadena: 
    - Se admiten los siguientes operadores: `==`, `!=`



4. Ejecute el experimento.

    La expresión divide el conjunto de datos en dos conjuntos de filas: las filas con valores que cumplen la condición y todas las filas restantes.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 