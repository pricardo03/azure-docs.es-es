---
title: 'Seleccionar columnas de conjunto de datos: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar las columnas que seleccione en el módulo de conjunto de datos del servicio Azure Machine Learning para elegir un subconjunto de columnas para usarlas en operaciones descendentes.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: e7407f62bd3401411d56076b298bd8cd134ece62
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028101"
---
# <a name="select-columns-in-dataset-module"></a>Seleccione las columnas en el módulo de conjunto de datos

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para elegir un subconjunto de columnas para usarlas en operaciones descendentes. El módulo no elimina físicamente las columnas del conjunto de datos de origen; en su lugar, crea un subconjunto de columnas, muy similar a una base de datos *vista* o *proyección*.

Este módulo es útil cuando se necesita limitar las columnas disponibles para una operación de nivel inferior, o si desea reducir el tamaño del conjunto de datos mediante la eliminación de las columnas innecesarias.

Las columnas del conjunto de datos son la salida en el mismo orden que en los datos originales, aunque especifique en un orden diferente.

## <a name="how-to-use"></a>Modo de uso

Este módulo no tiene parámetros. Utilice el selector de columnas para elegir las columnas para incluir o excluir.

### <a name="choose-columns-by-name"></a>Elija las columnas por nombre

Existen varias opciones en el módulo para elegir las columnas por nombre: 

+ Filtrado y búsqueda

    Haga clic en el **por nombre** opción.

    Si se ha conectado a un conjunto de datos que ya se hayan rellenado, debe aparecer una lista de columnas disponibles. Si no aparece ninguna columna, es posible que deba ejecutar módulos de nivel superior para ver la lista de columnas.

    Para filtrar la lista, escriba en el cuadro de búsqueda. Por ejemplo, si escribe la letra `w` en el cuadro de búsqueda, la lista se filtra para mostrar los nombres de columna que contengan la letra `w`.

    Seleccione las columnas y haga clic en el botón de flecha derecha para mover las columnas seleccionadas a la lista en el panel derecho.

    + Para seleccionar un intervalo continuo de los nombres de columna, presione **Mayús + clic**.
    + Para agregar columnas individuales a la selección, presione **Ctrl + clic**.

    Haga clic en el botón de marca de verificación para guardar y cerrar.

+ Use nombres en combinación con otras reglas

    Haga clic en el **con reglas** opción.
    
    Elija una regla, por ejemplo, que muestra las columnas de un tipo de datos específico.

    A continuación, haga clic en columnas individuales de ese tipo por nombre, para agregarlos a la lista de selección.

+ Escriba o pegue una lista separada por comas de nombres de columna

    Si el conjunto de datos es grande, podría ser más fácil de usar índices o generar listas de nombres, en lugar de seleccionar columnas individualmente. Suponiendo que ha preparado la lista de antemano:

    1. Haga clic en el **con reglas** opción. 
    2. Seleccione **ninguna columna**, seleccione **Include**y luego haga clic en el cuadro de texto con el signo de exclamación rojo. 
    3. Pegue o escriba una lista separada por comas de nombres de columna validado anteriormente. No se puede guardar el módulo si alguna columna tiene un nombre no válido, por lo que puede para comprobar los nombres de antemano.
    
    También puede usar este método para especificar una lista de las columnas utilizando sus valores de índice. 

### <a name="choose-by-type"></a>Elija por tipo

Si usas el **con reglas** opción, puede aplicar varias condiciones en las selecciones de columna. Por ejemplo, es posible que deba obtener solamente las columnas de característica de un tipo de datos numéricos.

El **comenzar con** opción determina el punto de partida y es importante para entender los resultados. 

+ Si selecciona el **todas las columnas** opción, todas las columnas se agregan a la lista. A continuación, debe usar el **excluir** opción *quitar* columnas que cumplen ciertas condiciones. 

    Por ejemplo, podría comenzar con todas las columnas y, a continuación, quite las columnas por nombre o por tipo.

+ Si selecciona el **columnas n** opción, la lista de columnas inicia vacío. A continuación, especifique las condiciones que *agregar* columnas a la lista. 

    Si aplica varias reglas, cada condición es **aditivos**. Por ejemplo, supongamos que comience con ninguna columna y, a continuación, agregue una regla para obtener todas las columnas numéricas. En el conjunto de datos de precios de automóviles, que es el resultado de 16 columnas. A continuación, haga clic en el **+** inicie sesión agregar una nueva condición y seleccione **incluyen todas las características**. El conjunto de datos resultante incluye todas las columnas numéricas, además de todas las columnas de característica, incluidas algunas columnas de característica de cadena.

### <a name="choose-by-column-index"></a>Elija el índice de columna

El índice de columna hace referencia al orden de la columna del conjunto de datos original.

+ Las columnas se numeran secuencialmente, comenzando en 1.  
+ Para obtener un intervalo de columnas, use un guión. 
+ Especificaciones abiertas como `1-` o `-3` no se permiten.
+ No se permiten valores de índice duplicados (o nombres de columna) y podrían producir un error.

Por ejemplo, suponiendo que el conjunto de datos tiene al menos ocho columnas, puede pegar en cualquiera de los ejemplos siguientes para devolver varias columnas no contiguas: 

+ `8,1-4,6`
+ `1,3-8`
+ `1,3-6,4` 

el último ejemplo no produce un error; Sin embargo, devuelve una única instancia de la columna `4`.



### <a name="change-order-of-columns"></a>Cambiar el orden de columnas

La opción **permite duplicados y conservar el orden de las columnas de selección** comienza con una lista vacía y agrega las columnas que especifique por nombre o por índice. A diferencia de otras opciones, que siempre se devuelven las columnas de "su orden natural", esta opción muestra las columnas en el orden que denomine o enumerarlos. 

Por ejemplo, en un conjunto de datos con las columnas Col1, Col2, Col3 y Col4 podría invertir el orden de las columnas y omita la columna 2, mediante la especificación de las listas siguientes:

+ `Col4, Col3, Col1`
+ `4,3,1`


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 