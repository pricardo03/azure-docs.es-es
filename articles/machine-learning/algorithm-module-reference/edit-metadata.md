---
title: 'Editar los metadatos: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo editar metadatos en el servicio de Azure Machine Learning para cambiar los metadatos que están asociados a las columnas en un conjunto de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: cfee607aca155b6cf68e5bddc40eb9c752df5e34
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028821"
---
# <a name="edit-metadata-module"></a>Modificar el módulo de metadatos

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para cambiar los metadatos que están asociados a las columnas en un conjunto de datos. Se cambiará el valor y tipo de datos del conjunto de datos después de usar el **editar metadatos** módulo. 
 
Cambios de metadatos típicos podrían incluir:
  
+ Tratar columnas booleanas o numéricas como valores de categorías  
  
+ Que indica la columna que contiene el *clase* etiqueta o los valores que desea clasificar o predecir  
  
+ Marcar columnas como características
  
+ Fecha y hora de cambiar los valores a un valor numérico, o viceversa  
  
+ Cambiar el nombre de columnas
  
 Use [editar metadatos siempre que necesite modificar la definición de una columna, normalmente para cumplir los requisitos para un módulo de nivel inferior. Por ejemplo, algunos módulos pueden trabajar solo con tipos de datos específicos, o requieren marcas en las columnas, como `IsFeature` o `IsCategorical`.  
  
 Después de realizar la operación necesaria, puede restablecer los metadatos a su estado original. 
  
## <a name="configure-edit-metadata"></a>Configurar los metadatos de edición
  
1.  En Azure Machine Learning, agregue [editar metadatos](./edit-metadata.md) módulo al experimento y conecte el conjunto de datos que desea actualizar. Puede encontrarlo en **transformación de datos**, en el **manipular** categoría.
  
2.  Haga clic en **iniciar el selector de columnas** y elija la columna o conjunto de columnas para que funcione con. Puede elegir las columnas individualmente, por nombre o índice, o puede elegir un grupo de columnas, por tipo.  
  
3.  Seleccione el **tipo de datos** opción si tiene que asignar un tipo de datos diferentes a las columnas seleccionadas. Cambiar los datos de tipo podría ser necesario para ciertas operaciones: por ejemplo, si el conjunto de datos de origen tiene números que se tratan como texto, debe cambiarlos a un tipo de datos numéricos antes de usar las operaciones matemáticas. 

    + Los tipos de datos admitidos son `String`, `Integer`, `Double`, `Boolean`, `DateTime`. 

    + Si se seleccionan varias columnas, debe aplicar los cambios de metadatos para **todas** columnas seleccionadas. Por ejemplo, supongamos que elija las columnas numéricas de 2-3. Puede cambiarlos todo en un tipo de datos de cadena y cambiar su nombre en una sola operación. Sin embargo, no se puede cambiar una columna a un tipo de datos de cadena y otra columna de float a un entero.
  
    + Si no especifica un nuevo tipo de datos, los metadatos de columna no cambia. 
    
    + Se cambiará el tipo de columna y valores después de realizar la [editar metadatos](./edit-metadata.md) operación. Puede recuperar el tipo de datos original en cualquier momento mediante el uso de [editar metadatos](./edit-metadata.md) para restablecer el tipo de datos de columna.  

    > [!NOTE]
    > Si cambia cualquier tipo de número en el **DateTime** escriba, deje el **formato DateTime** campo en blanco. Actualmente, no es posible especificar el formato de datos de destino.  

      
4.  Seleccione el **categorías** opción para especificar que los valores de las columnas seleccionadas deben tratarse como de categorías. 

    Por ejemplo, podría tener una columna que contiene los números de 0,1 y 2, pero sabe que los números realmente significan "Fumador", "Non-fumador" y "Desconocido". En ese caso, marcando la columna como de categorías puede asegurarse de que los valores no se usan en los cálculos numéricos, sólo para agrupar los datos. 
  
5.  Use la **campos** opción si desea cambiar la forma en que Azure Machine Learning usa los datos de un modelo.

    + **Característica**: Use esta opción para marcar una columna como una característica para su uso con los módulos que sólo funcionan en las columnas de característica. De forma predeterminada, todas las columnas se tratan inicialmente como funciones.  
  
    + **Etiqueta**: Use esta opción para marcar la etiqueta (también conocido como el atributo de predicción o variable de destino). Muchos módulos requieren que al menos uno (y sólo uno) estar presente en el conjunto de datos de columna de etiqueta. 
    
        En muchos casos, Azure Machine Learning puede deducir que una columna contiene una etiqueta de clase, pero estableciendo estos metadatos puede asegurarse de que la columna se identifica correctamente. Esta opción no cambia los valores de datos, solo la forma en que algunos algoritmos de aprendizaje automático controlan los datos.
  

  
    > [!TIP]
    >  ¿Tiene datos que no encajan en estas categorías?  Por ejemplo, el conjunto de datos puede contener valores como identificadores únicos que no son útiles como variables. En ocasiones identificadores pueden causar problemas cuando se usa en un modelo. 
    >   
    >  Afortunadamente "tras el telón" Azure Machine Learning mantiene todos los datos, por lo que no tiene que eliminar estas columnas del conjunto de datos. Cuando necesite realizar operaciones en un conjunto especial de columnas, simplemente quitar temporalmente de todas las demás columnas mediante el [Select Columns in Dataset](./select-columns-in-dataset.md) módulo. Más adelante puede combinar las columnas en el conjunto de datos mediante el uso de la [agregar columnas](./add-columns.md) módulo.  
  
6. Utilice las siguientes opciones para borrar las selecciones anteriores y restaurar los metadatos a los valores predeterminados.  
  
    + **Borrar característica**: Utilice esta opción para quitar la marca de características.  
  
         Dado que todas las columnas se tratan inicialmente como características, los módulos que realizan operaciones matemáticas, necesita usar esta opción para impedir que las columnas numéricas que se tratan como variables.
  
    + **Borrar etiqueta**: Use esta opción para quitar el **etiqueta** metadatos de la columna especificada.  
  
    + **Borrar puntuación**: Use esta opción para quitar el **puntuación** metadatos de la columna especificada.  
  
         Actualmente, la capacidad de marcar explícitamente una columna como una puntuación no está disponible en Azure Machine Learning. Sin embargo, algunas operaciones se traducirá en una columna que se marca como una puntuación internamente. Además, un módulo R personalizado puede generar valores de puntuación.
  
  
7.  Para **nuevos nombres de columna**, escriba el nuevo nombre de la columna o columnas seleccionadas.  
  
    + Los nombres de columna pueden usar únicamente caracteres que son compatibles con UTF-8 de codificación. No se permiten cadenas vacías, los valores NULL o que consta únicamente de espacios de nombres.  
  
    + Para cambiar el nombre de varias columnas, escriba los nombres como una lista separada por comas en el orden de los índices de columna.  
  
    + Todas las columnas seleccionadas deben cambiarse. No se puede omitir u omitir columnas.  
  
  
8.  Ejecute el experimento.  

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 