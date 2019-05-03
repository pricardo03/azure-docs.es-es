---
title: 'Quitar filas duplicadas: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de quitar filas duplicadas en el servicio Azure Machine Learning para quitar posibles duplicados de un conjunto de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: dce90d911085c1f7330a2e0952bb9576c1d765fa
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029301"
---
# <a name="remove-duplicate-rows-module"></a>Quitar filas duplicadas módulo

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para quitar posibles duplicados de un conjunto de datos.

Por ejemplo, supongamos los datos es similar al siguiente y representa varios registros de pacientes. 

| PatientID | Initials| Sexo|Edad|Admitidas|
|----|----|----|----|----|
|1|F.M.| M| 53| Ene|
|2| F.A.M.| M| 53| Ene|
|3| F.A.M.| M| 24| Ene|
|3| F.M.| M| 24| Feb|
|4| F.M.| M| 23| Feb|
| | F.M.| M| 23| |
|5| F.A.M.| M| 53| |
|6| F.A.M.| M| NaN| |
|7| F.A.M.| M| NaN| |

Claramente, este ejemplo tiene varias columnas con datos potencialmente duplicados. Que sean duplicados realmente depende de sus conocimientos de los datos. 

+ Por ejemplo, es posible que sabe que muchos pacientes tienen el mismo nombre. No eliminaría los duplicados utilizando las columnas de nombre, sólo el **ID** columna. De este modo, solo las filas con valores de Id. duplicados se filtran, independientemente de si los pacientes tienen el mismo nombre o no.

+ Como alternativa, decide permitir duplicados en el campo ID y utilice otra combinación de archivos para buscar registros únicos, como nombre, nombre de último, edad y sexo.  

Para establecer los criterios de si una fila está duplicada o no, especifica una sola columna o un conjunto de columnas que se usará como **claves**. Dos filas se consideran duplicados solo cuando los valores de **todas** columnas de clave son iguales. Si cualquier fila tiene un valor que falta para **claves**, no se considerarán las filas duplicadas. Por ejemplo, si género y edad se establecen como claves por encima de la tabla, la fila 6 y 7 no son las filas duplicadas dadas tienen falta el valor de edad.

Cuando se ejecuta el módulo, crea un conjunto de datos de candidato y devuelve un conjunto de filas que no tienen duplicados en el conjunto de columnas especificadas.

> [!IMPORTANT]
> No se modifica el conjunto de datos de origen; Este módulo crea un nuevo conjunto de datos que se filtra para que se excluyan los duplicados, según los criterios que especifique.

## <a name="how-to-use-remove-duplicate-rows"></a>Cómo usar quitar filas duplicadas

1. Agregue el módulo al experimento. Puede encontrar el **quitar filas duplicadas** módulo en **transformación de datos**, **manipulación**.  

2. Conecte el conjunto de datos que desea comprobar las filas duplicadas.

3. En el **propiedades** panel, en **expresión de filtro de selección de columna de clave**, haga clic en **iniciar el selector de columna**, Elegir columnas para usarlas en identificar duplicados.

    En este contexto, **clave** no significa que un identificador único. Todas las columnas que seleccione mediante el Selector de columnas se designan como **columnas de clave**. Todas las columnas no seleccionadas se consideran columnas sin clave. La combinación de columnas que seleccione como claves determina la unicidad de los registros. (Pensar en ella como una instrucción SQL que usa varias combinaciones de igualdades.)

    Ejemplos:

    + "Quiero asegurarme de que los identificadores son únicos": Elija solo la columna de identificador.
    + "Quiero asegurarme de que la combinación de nombre, apellido e identificador es única:" Seleccione las tres columnas.

4. Use la **Conservar primera fila duplicada** casilla de verificación para indicar qué fila va a devolver cuando se encuentran duplicados:

    + Si se selecciona, se devuelve la primera fila y otros se descartan. 
    + Si desactiva esta opción, se mantiene la última fila duplicada en los resultados y otros se descartan. 

5. Ejecute el experimento.

6. Para revisar los resultados, haga clic en el módulo, seleccione **conjunto de datos de resultados**y haga clic en **visualizar**. 

> [!TIP]
> Si los resultados son difíciles de entender, o si desea excluir algunas columnas de la consideración, puede quitar las columnas mediante el [Select Columns in Dataset](./select-columns-in-dataset.md) módulo.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 