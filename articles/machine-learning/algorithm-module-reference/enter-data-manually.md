---
title: 'Escribir manualmente los datos: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo especificar datos manualmente en el servicio de Azure Machine Learning para crear un conjunto de datos pequeño escribiendo valores. El conjunto de datos puede tener varias columnas.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ee15b6fb7160ece907d55e790b0ae38ee458ab96
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028611"
---
# <a name="enter-data-manually-module"></a>Especifique un módulo de datos manualmente

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un conjunto de datos pequeño escribiendo valores. El conjunto de datos puede tener varias columnas.
  
Este módulo puede ser útil en escenarios como estos:  
  
- Generar un pequeño conjunto de valores para las pruebas  
  
- Creación de una breve lista de etiquetas
  
- Escriba una lista de nombres de columna se va a insertar en un conjunto de datos

## <a name="enter-data-manually"></a>Escribir manualmente los datos 
  
1.  Agregar el [especificar datos manualmente](./enter-data-manually.md) módulo al experimento. Puede encontrar este módulo en el **datos de entrada y salida** categoría en Azure Machine Learning. 
  
2.  Para **DataFormat**, seleccione una de las siguientes opciones. Estas opciones determinan cómo se deben analizar los datos que proporcione. Los requisitos para cada formato varían en gran medida, así que asegúrese de leer los temas relacionados.  
  
    -   **ARFF**. El formato de archivo de relación de atributos, usado por Weka.   
  
    -   **CSV**. Formato de valores separados por comas. Para obtener más información, consulte [convertir a CSV](./convert-to-csv.md).  
  
    -   **SVMLight**. Formato usado por Vowpal Wabbit y otros marcos de aprendizaje automático.  
  
    -   **TSV**. Formato de valores separados por tabulaciones.

     Si elige un formato y no proporcionan datos que cumplen las especificaciones de formato, se produce un error en tiempo de ejecución.
  
3.  Haga clic dentro de la **datos** empiece a escribir datos de cuadro de texto. Los formatos siguientes requieren una atención especial:  
  
    - **CSV**:  Para crear varias columnas, pegue texto separados por comas o varias columnas con comas entre los campos de tipo.
  
        Si selecciona el **HasHeader** opción, puede usar la primera fila de valores como encabezado de columna.  
  
        Si desactiva esta opción, los nombres de las columnas, Col1, Col2 y así sucesivamente, se usan. Puede agregar o cambiar las columnas nombres más adelante mediante [editar metadatos](./edit-metadata.md).  
  
    - **TSV**: Para crear varias columnas, pegue texto separado por tabulaciones o varias columnas mediante las fichas entre los campos de tipo.  
  
        Si selecciona el **HasHeader** opción, puede usar la primera fila de valores como encabezado de columna.  
  
        Si desactiva esta opción, los nombres de las columnas, Col1, Col2 y así sucesivamente, se usan. Puede agregar o cambiar las columnas nombres más adelante mediante [editar metadatos](./edit-metadata.md).  
  
    -   **ARFF**:  Pegar en un archivo de formato ARFF existente. Si está escribiendo valores directamente, no olvide agregar los campos de atributo requerido y el encabezado opcional al principio de los datos. 
    
        Por ejemplo, las filas de encabezado y el atributo siguientes podrían agregarse a una lista simple. El encabezado de columna sería `SampleText`.
    
        ```text
        % Title: SampleText.ARFF  
        % Source: Enter Data module  
        @ATTRIBUTE SampleText STRING  
        @DATA  
        \<type first data row here>  
        ```

    -   **SVMLight**: Escriba o pegue los valores con el formato SVMLight.  
  
        Por ejemplo, el ejemplo siguiente representa las primeras líneas de par del conjunto de datos donación de sangre, en formato SVMight:  
  
        ```text  
        # features are [Recency], [Frequency], [Monetary], [Time]  
        1 1:2 2:50 3:12500 4:98   
        1 1:0 2:13 3:3250 4:28   
        ```  
  
        Al ejecutar el [especificar datos manualmente](./enter-data-manually.md) módulo, estas líneas se convierten en un conjunto de datos de columnas e indexar valores como sigue:  
  
        |Col1|Col2|Col3|Col4|Etiquetas|  
        |-|-|-|-|-|  
        |0.00016|0.004|0.999961|0.00784|1|  
        |0|0.004|0.999955|0.008615|1|  
  
4.  Presione ENTRAR después de cada fila, para iniciar una nueva línea.  
  
     **Asegúrese de presionar ENTRAR después de la fila final.** 
     
     Si presiona ENTRAR varias veces para agregar varios vacía al final de las filas, la fila vacía final se quita recortadas, pero otras filas vacías se tratan como valores que faltan.  
  
     Si crea filas con valores que faltan, siempre puede filtrar ellos más adelante.  
  
5.  Haga clic en el módulo y seleccione **ejecutar seleccionado** para analizar los datos y cargarlos en el área de trabajo como un conjunto de datos.  
  
     Para ver el conjunto de datos, haga clic en el puerto de salida y seleccione **visualizar**.  
## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 