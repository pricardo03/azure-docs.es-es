---
title: Solución de problemas de errores de módulo
titleSuffix: Azure Machine Learning service
description: Solución de problemas de excepciones de módulo en Azure Machine Learning Studio mediante códigos de error
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 09a2b616e2bba93be86241c64d37daec7d6dea3b
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029766"
---
# <a name="exceptions-and-error-codes-for-algorithm--module-reference"></a>Excepciones y códigos de error para la referencia de módulo y algoritmos

Obtenga información sobre los mensajes de error y códigos de excepción que pueden surgir con los módulos en Azure Machine Learning Studio. 

Para resolver el problema, busque el error en este artículo para obtener información acerca de las causas comunes. Hay dos maneras de obtener el texto completo de un mensaje de error en Studio:  
 
- Haga clic en el vínculo, **Ver registro de salida**, en el panel derecho y desplácese hasta el final. El mensaje de error detallado se muestra en las dos últimas líneas de la ventana.  
  
- Seleccione el módulo que tiene el error y haga clic en la X roja. Se muestra únicamente el texto de error pertinente.  
  
Si el texto del mensaje de error no es útil, envíenos información sobre el contexto y cualquier deseadas adiciones o cambios. Puede enviar comentarios sobre el tema de error, o visite el [foro de Azure Machine Learning STUDIO](https://aka.ms/aml-forum-studio) y publique una pregunta.  


## <a name="error-0001"></a>Error 0001  
 Se produce una excepción si no se encontró uno o más columnas especificadas del conjunto de datos.  
  
 Recibirá este error si se realiza una selección de columna para un módulo, pero las columnas seleccionadas no existen en el conjunto de datos de entrada. Este error puede producirse si han escrito manualmente en un nombre de columna o si el selector de columnas ha proporcionado una columna sugerida que no existía en el conjunto de datos al ejecutar el experimento.  
  
**Resolución:** Volver a visitar el módulo genere esta excepción y validar que el nombre de columna o los nombres son correctos y que todas las columnas que se hace referencia existen.  
  
|Mensajes de excepción|  
|------------------------|  
|No se encontraron una o varias columnas especificadas|  
|Columna con nombre o índice "{0}" no encontrado|  
|Columna con nombre o índice "{0}"no existe en"{1}"|  
 

## <a name="error-0002"></a>Error 0002  
 Se produce una excepción si uno o más parámetros no se pudieron analizar o convertir del tipo especificado en pedirá por el tipo de método de destino.  
  
 Este error se produce en Azure Machine Learning al especificar un parámetro como entrada y el tipo de valor es diferente del tipo que se espera y no se puede realizar la conversión implícita.  
  
**Resolución:** Compruebe los requisitos de módulo y determinar qué tipo de valor es necesario (string, integer, double, etcetera.)  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo analizar el parámetro|  
|No se pudo analizar "{0}" parámetro|  
|No se pudo analizar (convertir) "{0}"parámetro"{1}"|  
|No se pudo convertir "{0}"parámetro de"{1}"a"{2}"|  
|No se pudo convertir "{0}"valor del parámetro"{1}"de"{2}"a"{3}"|  
|No se pudo convertir el valor "{0}"en la columna"{1}"de"{2}"a"{3}"con el uso del formato de"{4}" proporcionado|  
  

## <a name="error-0003"></a>Error 0003  
 Se produce una excepción si una o varias de las entradas son nulas o vacías.  
  
 Si las entradas o parámetros a un módulo son null o están vacíos, recibirá este error en Azure Machine Learning.  Este error puede ocurrir, por ejemplo, cuando no escribió en cualquier valor para un parámetro. También puede ocurrir si elige un conjunto de datos que tiene valores que faltan, o un conjunto de datos vacío.  
  
**Resolución:**
 
+ Abra el módulo que generó la excepción y compruebe que se han especificado todas las entradas. Asegúrese de que todos los necesarios se especifican las entradas. 
+ Asegúrese de que los datos que se cargan desde Azure storage están accesibles y que no ha cambiado el nombre de cuenta o la clave.  
+ Compruebe los datos de entrada para los valores que faltan o los valores NULL.
+ Si usa una consulta en un origen de datos, compruebe que se devolvieron datos en el formato que espera. 
+ Busque errores tipográficos u otros cambios en la especificación de los datos.
  
|Mensajes de excepción|  
|------------------------|  
|Una o varias de las entradas son nulas o vacías|  
|Entrada "{0}" es nulo o está vacío|  
  

## <a name="error-0004"></a>Error 0004  
 Se produce una excepción si el parámetro es menor o igual al valor específico.  
  
 Si el parámetro en el mensaje está por debajo de un valor de límite necesario para que el módulo procesar los datos, recibirá este error en Azure Machine Learning.  
  
**Resolución:** Volver a visitar el módulo que produce la excepción y modifique el parámetro para que sea mayor que el valor especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|Parámetro debe ser mayor que el valor de límite.|  
|Parámetro "{0}" debe ser mayor que el valor {1}.|  
|Parámetro "{0}"tiene el valor"{1}" que debe ser mayor que {2}|  
  


## <a name="error-0005"></a>Error 0005  
 Se produce una excepción si el parámetro es menor que un valor específico.  
  
 Recibirá este error en Azure Machine Learning si el parámetro en el mensaje es inferior o igual a un valor de límite necesario para que el módulo procesar los datos.  
  
**Resolución:** Volver a visitar el módulo que produce la excepción y modifique el parámetro para que sea mayor o igual que el valor especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|Parámetro debe ser mayor o igual al valor de límite.|  
|Parámetro "{0}" valor debe ser mayor o igual que {1}.|  
|Parámetro "{0}"tiene el valor"{1}" que debe ser mayor o igual que {2}.|  
  

## <a name="error-0006"></a>Error 0006  
 Se produce una excepción si el parámetro es mayor o igual que el valor especificado.  
  
 Si el parámetro en el mensaje es mayor o igual que un valor de límite necesario para que el módulo procesar los datos, recibirá este error en Azure Machine Learning.  
  
**Resolución:** Volver a visitar el módulo que produce la excepción y modifique el parámetro para que sea menor que el valor especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|Error de coincidencia de parámetros. Uno de los parámetros debe ser menor que otro.|  
|Parámetro "{0}"el valor debe ser menor que el parámetro"{1}" valor.|  
|Parámetro "{0}"tiene el valor"{1}" que debe ser menor que {2}.|  
  

## <a name="error-0007"></a>Error 0007  
 Se produce una excepción si el parámetro es mayor que un valor específico.  
  
 Si en las propiedades del módulo, se especificó un valor que es mayor que la permitida, recibirá este error en Azure Machine Learning. Por ejemplo, es posible que especifique los datos que está fuera del intervalo de fechas admitidos, o podría indicar que se utiliza cinco columnas cuando están disponibles solo tres columnas. 
 
 También puede ver este error si se especifica dos conjuntos de datos que deben coincidir de alguna manera. Por ejemplo, si se están cambiando el nombre de las columnas y especificar las columnas por índice, el número de nombres que proporcione debe coincidir con el número de índices de columna. Otro ejemplo podría ser una operación matemática que utiliza dos columnas, donde las columnas deben tener el mismo número de filas. 
  
**Resolución:**
 
 + Abra el módulo en cuestión y revise cualquier configuración de la propiedad numérica.
 + Asegúrese de que los valores de parámetro se encuentran dentro del intervalo admitido de valores para esa propiedad.
 + Si el módulo toma varias entradas, asegúrese de que las entradas son del mismo tamaño.
<!-- + If the module has multiple properties that can be set, ensure that related properties have appropriate values. For example, when using [Group Data into Bins](group-data-into-bins.md), if you use the option to specify custom bin edges, the number of bins must match the number of values you provide as bin boundaries.-->
 + Compruebe si el conjunto de datos u origen de datos ha cambiado. A veces, un valor que ha trabajado con una versión anterior de los datos se producirá un error después de que ha cambiado el número de columnas, los tipos de datos de columna o el tamaño de los datos.  
  
|Mensajes de excepción|  
|------------------------|  
|Error de coincidencia de parámetros. Uno de los parámetros debe ser menor o igual a otro.|  
|Parámetro "{0}"el valor debe ser menor o igual al parámetro"{1}" valor.|  
|Parámetro "{0}"tiene el valor"{1}" que debe ser menor o igual que {2}.|  
  

## <a name="error-0008"></a>Error 0008  
 Se produce una excepción si el parámetro no está en el intervalo.  
  
 Si el parámetro en el mensaje está fuera de los límites necesarios para que el módulo procesar los datos, recibirá este error en Azure Machine Learning.  
  
 Por ejemplo, se muestra este error si intenta usar [agregar filas](add-rows.md) para combinar dos conjuntos de datos que tienen un número diferente de columnas.  
  
**Resolución:** Volver a visitar el módulo que produce la excepción y modifique el parámetro para que esté dentro del intervalo especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|Valor del parámetro no está en el intervalo especificado.|  
|Parámetro "{0}" valor no está en el intervalo.|  
|Parámetro "{0}" valor debe estar en el intervalo de [{1}, {2}].|  
  

## <a name="error-0009"></a>Error 0009  
 Se produce una excepción cuando se especifica incorrectamente el nombre del contenedor o el nombre de la cuenta de almacenamiento de Azure.  
  
Este error se produce en Azure Machine Learning Studio cuando especifica parámetros para una cuenta de almacenamiento de Azure, pero no se puede resolver el nombre o la contraseña. Errores en las contraseñas o los nombres de cuenta pueden ocurrir por diversos motivos:
 
 + La cuenta es un tipo incorrecto. No se admiten algunos nuevos tipos de cuenta para su uso con Machine Learning Studio. Consulte [importar datos](import-data.md) para obtener más información.
 + Escribió el nombre de cuenta incorrecto
 + La cuenta ya no existe
 + La contraseña de la cuenta de almacenamiento es incorrecta o ha cambiado
 + No ha especificado el nombre del contenedor o el contenedor no existe
 + Totalmente no ha especificado la ruta de acceso de archivo (ruta de acceso al blob)
   
**Resolución:**

Estos problemas suelen producen al intentar escribir manualmente la ruta de acceso de contenedor, la contraseña o nombre de cuenta. Se recomienda que use el Asistente para nuevos la [importar datos](import-data.md) módulo, que le permite buscar y comprobar los nombres.

Compruebe también si se ha eliminado la cuenta, contenedor o blob. Use otra utilidad de almacenamiento de Azure para comprobar que el nombre de cuenta y la contraseña se han escrito correctamente y que existe el contenedor. 

No se admiten algunos nuevos tipos de cuenta mediante Azure Machine Learning. Por ejemplo, el nuevo "acceso frecuente" o tipos de almacenamiento "en frío" no se puede usar para el aprendizaje automático. Cuentas de almacenamiento clásicas y cuentas de almacenamiento creadas como "Uso General" funciona correctamente.

Si se ha especificado la ruta de acceso completa a un blob, compruebe que la ruta de acceso se especifica como **contenedor/blobname**, y que existen el contenedor y el blob en la cuenta.  
  
 La ruta de acceso no debe contener una barra inicial. Por ejemplo **/contenedor/blob** no es correcta y debe especificarse como **contenedor/blob**.  

  
|Mensajes de excepción|  
|------------------------|  
|El nombre de la cuenta de almacenamiento de Azure o el nombre del contenedor es incorrecto.|  
|El nombre de cuenta de almacenamiento de Azure "{0}"o el nombre del contenedor"{1}" no es correcto; se esperaba un nombre de contenedor del formato contenedor/blob.|  
  

## <a name="error-0010"></a>Error 0010  
 Se produce una excepción si los conjuntos de datos de entrada tienen nombres de columna que deben coincidir pero no lo hacen.  
  
 Si el índice de columna en el mensaje tiene nombres de columna diferentes en los dos conjuntos de datos de entrada, recibirá este error en Azure Machine Learning.  
  
**Resolución:** Use [editar metadatos](edit-metadata.md) o modificar el conjunto de datos original para que tenga el mismo nombre de columna para el índice de columna especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|Las columnas con el índice correspondiente en conjuntos de datos de entrada tienen nombres diferentes.|  
|Los nombres de columna no son los mismos para columna {0} (basado en cero) de conjuntos de datos de entrada ({1} y {2} respectivamente).|  
  

## <a name="error-0011"></a>Error 0011  
 Se produce una excepción si se pasa el argumento del conjunto de columnas no se aplica a cualquiera de las columnas del conjunto de datos.  
  
 Si la selección de columna especificado no coincide con cualquiera de las columnas del conjunto de datos determinado, recibirá este error en Azure Machine Learning.  
  
 También puede obtener este error si no ha seleccionado una columna y al menos una columna es necesaria para el módulo funcione.  
  
**Resolución:** Modifique la selección de columna en el módulo para que se aplicará a las columnas del conjunto de datos.  
  
 Si el módulo requiere que seleccione una columna específica, como una columna de etiqueta, compruebe que está seleccionada la columna derecha.  
  
 Si se seleccionan las columnas inadecuadas, quítelos y vuelva a ejecutar el experimento.  
  
|Mensajes de excepción|  
|------------------------|  
|Conjunto de columnas especificado no es aplicable a cualquiera de las columnas del conjunto de datos.|  
|Especifica el conjunto de columnas "{0}" no es aplicable a cualquiera de las columnas del conjunto de datos.|  
  

## <a name="error-0012"></a>Error 0012  
 Se produce una excepción si no se pudo crear la instancia de clase con el conjunto de argumentos pasado.  
  
**Resolución:** Este error no es accionable por el usuario y dejará de utilizarse en una versión futura.  
  
|Mensajes de excepción|  
|------------------------|  
|Sin entrenar modelo, entrene el modelo primero.|  
|Modelo no entrenado ({0}), utilice el modelo entrenado.|  
  

## <a name="error-0013"></a>Error 0013  
 Se produce una excepción si el aprendiz que se pasan al módulo es un tipo no válido.  
  
 Este error se produce siempre que no es compatible con el módulo de puntuación conectado un modelo entrenado. <!--For example, connecting the output of [Train Matchbox Recommender](train-matchbox-recommender.md) to [Score Model](score-model.md) (instead of [Score Matchbox Recommender](score-matchbox-recommender.md)) will generate this error when the experiment is run.  -->
  
**Resolución:**

Determinar el tipo de aprendiz producidos por el módulo de aprendizaje y determinar el módulo de puntuación que sea adecuado para el aprendiz. 

Si el modelo se entrenó con cualquiera de los módulos de entrenamiento especializadas, conectar el modelo entrenado solo para el módulo de puntuación especializado correspondiente. 


|Tipo de modelo|Módulo de aprendizaje| Módulo de puntuación|
|----|----|----|
|ningún clasificador|[Entrenar modelo](train-model.md) |[Puntuar modelo](score-model.md)|
|cualquier modelo de regresión|[Entrenar modelo](train-model.md) |[Puntuar modelo](score-model.md)|
<!--| Modelos de agrupación en clústeres| [Entrenar modelo de clúster](train-clustering-model.md) o [barrido de agrupación en clústeres](sweep-clustering.md)| [Asignar datos a clústeres](assign-data-to-clusters.md)|
| detección de anomalías - SVM de una clase | [Entrenar el modelo de detección de anomalías](train-anomaly-detection-model.md) |[Puntuar modelo](score-model.md)|
| detección de anomalías - PCA |[Entrenar modelo](train-model.md) |[Puntuar modelo](score-model.md) </br> Son necesarios algunos pasos adicionales para evaluar el modelo. |
| detección de anomalías: serie temporal|  [Detección de anomalías en una series temporal](time-series-anomaly-detection.md) |Modelo se entrena a partir de datos y genera las puntuaciones. El módulo no crear un aprendiz entrenado y ninguna puntuación adicional es necesaria. |
| modelo de recomendación| [Entrenar el Recomendador Matchbox](train-matchbox-recommender.md) | [Recomendador matchbox para puntuar](score-matchbox-recommender.md) |
| clasificación de imágenes | [Clasificación de imágenes en cascada previamente entrenada](pretrained-cascade-image-classification.md) | [Puntuar modelo](score-model.md) |
|Modelos de Vowpal Wabbit| [Entrenar modelo de 7-4 de versión de Vowpal Wabbit](train-vowpal-wabbit-version-7-4-model.md) | [Puntuación de Vowpal Wabbit versión 7-4 modelo](score-vowpal-wabbit-version-7-4-model.md) |   
|Modelos de Vowpal Wabbit| [Entrenar modelo de entre 7 y 10 de versión de Vowpal Wabbit](train-vowpal-wabbit-version-7-10-model.md) | [Puntuación de Vowpal Wabbit versión 7 y 10 modelo](score-vowpal-wabbit-version-7-10-model.md) |
|Modelos de Vowpal Wabbit| [Entrenar el modelo de Vowpal Wabbit versión 8](score-vowpal-wabbit-version-8-model.md) | [Modelo de puntuación Vowpal Wabbit versión 8](score-vowpal-wabbit-version-8-model.md) |-->
  
|Mensajes de excepción|  
|------------------------|  
|Se pasa el aprendiz de tipo no válido.|  
|Aprendiz "{0}" tiene un tipo no válido.|  


## <a name="error-0014"></a>Error 0014  
 Se produce una excepción si el recuento de valores únicos de columna es mayor que la permitida.  
  
 Este error se produce cuando una columna contiene demasiados valores únicos.  Por ejemplo, podría ver este error si se especifica que una columna se trate como datos categóricos, pero hay demasiados valores únicos en la columna para permitir que se complete el procesamiento. También puede ver este error si hay una discrepancia entre el número de valores únicos de dos entradas.   
  
**Resolución:**

Abra el módulo que ha generado el error e identifique las columnas que se usan como entradas. Para algunos módulos, haga clic en el conjunto de datos de entrada y seleccione **visualizar** para obtener las estadísticas en columnas individuales, incluido el número de valores únicos y su distribución.

Las columnas que se va a utilizar para la agrupación o la clasificación, tomar medidas para reducir el número de valores únicos de las columnas. Puede reducir de maneras diferentes, según el tipo de datos de la columna. 
<!--
+ For text data, you might be able to use [Preprocess Text](preprocess-text.md) to collapse similar entries. 
+ For numeric data, you can create a smaller number of bins using [Group Data into Bins](group-data-into-bins.md), remove or truncate values using [Clip Values](clip-values.md), or use machine learning methods such as [Principal Component Analysis](principal-component-analysis.md) or [Learning with Counts](data-transformation-learning-with-counts.md) to reduce the dimensionality of the data.  
-->
> [!TIP]
> ¿No se puede encontrar una solución que coincida con su escenario? Puede proporcionar comentarios sobre este tema que incluye el nombre del módulo que genera el error y el tipo de datos y la cardinalidad de la columna. Usaremos la información para proporcionar más pasos para escenarios comunes para solucionar problemas de destino.   
  
|Mensajes de excepción|  
|------------------------|  
|Número de valores únicos de columna es mayor que el permitido.|  
|Número de valores únicos de columna: "{0}" supera el recuento de tuplas de {1}.|  
  

## <a name="error-0015"></a>Error 0015  
 Se produce una excepción si el error de conexión de base de datos.  
  
 Recibirá este error si escribe un nombre de cuenta SQL incorrecto, contraseña, el servidor de base de datos o nombre de la base de datos, o si no se puede establecer una conexión con la base de datos debido a problemas con la base de datos o el servidor.  
  
**Resolución:** Compruebe que el nombre de cuenta, contraseña, el servidor de base de datos y base de datos se han escrito correctamente y que la cuenta especificada tiene el nivel correcto de permisos. Compruebe que la base de datos está actualmente accesible.  
  
|Mensajes de excepción|  
|------------------------|  
|Error al realizar la conexión de base de datos.|  
|Error al realizar la conexión de base de datos: {0}.|  
  


## <a name="error-0016"></a>Error 0016  
 Se produce una excepción si los conjuntos de datos de entrada pasado al módulo deben tener tipos de columna compatibles pero no lo hacen.  
  
 Si los tipos de las columnas que se pasa en dos o más conjuntos de datos no son compatibles entre sí, recibirá este error en Azure Machine Learning.  
  
**Resolución:** Use [editar metadatos](edit-metadata.md) o modificar el conjunto de datos de entrada original<!--, or use [Convert to Dataset](convert-to-dataset.md)--> para asegurarse de que los tipos de las columnas son compatibles.  
  
|Mensajes de excepción|  
|------------------------|  
|Las columnas con el índice correspondiente en conjuntos de datos de entrada tienen tipos incompatibles.|  
|Columnas {0} y {1} son incompatibles.|  
|Tipos de elemento de columna no son compatibles para la columna {0} (basado en cero) de conjuntos de datos de entrada ({1} y {2} respectivamente).|  
  

## <a name="error-0017"></a>Error 0017  
 Se produce una excepción si una columna seleccionada usa un tipo de datos que no es compatible con el módulo actual.  
  
 Por ejemplo, podría recibir este error en Azure Machine Learning si la selección de columna incluye una columna con un tipo de datos que no se puede procesar el módulo, como una columna de cadena para una operación matemática o una columna de puntuación donde es una columna de característica de categoría Obligatorio.  
  
**Resolución:**
 1. Identificar la columna que es el problema.
 2. Revise los requisitos del módulo.
 3. Modificar la columna para que encajen con los requisitos. Es posible que deba usar varios de los siguientes módulos para realizar cambios, dependiendo de la columna y la conversión que intenta realizar:
    + Use [editar metadatos](edit-metadata.md) para cambiar el tipo de datos de columnas, o para cambiar el uso de las columnas de característica numéricas, categóricas para que no son categóricos, y así sucesivamente.
<!--    + Use [Convert to Dataset](convert-to-dataset.md) to ensure that all included columns use data types that are supported by Azure Machine Learning.  If you cannot convert the columns, consider removing them from the input dataset.
    + Use the [Apply SQL Transformation](apply-sql-transformation.md) or [Execute R Script](execute-r-script.md) modules to cast or convert any columns that cannot be modified using [Edit Metadata](edit-metadata.md). These modules provide more flexibility for working with datetime data types.
    + For numeric data types, you can use the [Apply Math Operation](apply-math-operation.md) module to round or truncate values, or use the [Clip Values](clip-values.md) module to remove out of range values.  -->
 4. Como último recurso, es posible que necesite modificar el conjunto de datos de entrada original.

> [!TIP]
> ¿No se puede encontrar una solución que coincida con su escenario? Puede proporcionar comentarios sobre este tema que incluye el nombre del módulo que genera el error y el tipo de datos y la cardinalidad de la columna. Usaremos la información para proporcionar más pasos para escenarios comunes para solucionar problemas de destino. 
  
|Mensajes de excepción|  
|------------------------|  
|No se puede procesar la columna del tipo actual. El tipo no es compatible con el módulo.|  
|No se puede procesar la columna de tipo {0}. El tipo no es compatible con el módulo.|  
|No se puede procesar la columna "{1}" de tipo {0}. El tipo no es compatible con el módulo.|  
|No se puede procesar la columna "{1}" de tipo {0}. El tipo no es compatible con el módulo. Nombre de parámetro: {2}|  
  

## <a name="error-0018"></a>Error 0018  
 Se produce una excepción si el conjunto de datos de entrada no es válido.  
  
**Resolución:** Este error en Azure Machine Learning puede aparecer en muchos contextos, por lo que hay no una única solución. En general, el error indica que los datos proporcionados como entrada para un módulo tienen un número incorrecto de columnas, o que el tipo de datos no coincide con los requisitos del módulo. Por ejemplo:   
  
-   El módulo requiere una columna de etiqueta, pero se ha marcado ninguna columna como una etiqueta o no ha seleccionado una columna de etiqueta aún.  
  
-   El módulo requiere datos ser categorías, pero los datos son numéricos.  
  
<!---   The module requires a specific data type. For example, ratings provided to [Train Matchbox Recommender](train-matchbox-recommender.md) can be either numeric or categorical, but cannot be floating point numbers.  -->
  
-   Los datos están en un formato incorrecto.  
  
-   Los datos importados contienen caracteres no válidos, valores erróneos, o valores fuera del intervalo.  
-   La columna está vacía o contiene demasiados valores que faltan.  
  
 Para determinar los requisitos y cómo es posible que los datos, revise el tema de Ayuda del módulo que se van a consumir el conjunto de datos como entrada.  
  
 <!--We also recommend that you use [Summarize Data](summarize-data.md) or [Compute Elementary Statistics](compute-elementary-statistics.md) to profile your data, and use these modules to fix metadata and clean values: [Edit Metadata](edit-metadata.md) and [Clean Missing Data](clean-missing-data.md), [Clip Values](clip-values.md)-->.  
  
|Mensajes de excepción|  
|------------------------|  
|Conjunto de datos no es válido.|  
|{0} contiene datos no válidos.|  
|{0} y {1} debe ser coherente columna conveniente.|  
  

## <a name="error-0019"></a>Error 0019  
 Si la columna se espera que contenga valores ordenados, pero no se produce una excepción.  
  
 Si los valores de columna especificada están fuera de servicio, recibirá este error en Azure Machine Learning.  
  
**Resolución:** Ordenar los valores de columna modificando manualmente el conjunto de datos de entrada y vuelva a ejecutar el módulo.  
  
|Mensajes de excepción|  
|------------------------|  
|No se ordenan los valores de columna.|  
|Los valores de columna "{0}" no están ordenados.|  
|Los valores de columna "{0}"del conjunto de datos"{1}" no están ordenados.|  
  

## <a name="error-0020"></a>Error 0020  
 Se produce una excepción si el número de columnas de algunos de los conjuntos de datos que se pasan al módulo es demasiado pequeño.  
  
 Recibirá este error en Azure Machine Learning, si no hay suficientes columnas se han seleccionado para un módulo.  
  
**Resolución:** Volver a visitar el módulo y asegúrese de que el selector de columnas tiene el número correcto de las columnas seleccionadas.  
  
|Mensajes de excepción|  
|------------------------|  
|Número de columnas de conjunto de datos de entrada es menor que el mínimo permitido.|  
|Número de columnas de conjunto de datos de entrada es menor que el mínimo permitido de {0} columnas.|  
|Número de columnas de conjunto de datos de entrada "{0}" es menor que el mínimo permitido de {1} columnas.|

## <a name="error-0021"></a>Error 0021  
 Se produce una excepción si el número de filas de algunos de los conjuntos de datos que se pasan al módulo es demasiado pequeño.  
  
 Este error en aparecer en Azure Machine Learning cuando hay no hay suficientes filas del conjunto de datos para realizar la operación especificada. Por ejemplo, es posible que vea este error si el conjunto de datos de entrada está vacía, o si está intentando realizar una operación que requiere el número mínimo de filas que se va a ser válido. Estas operaciones pueden incluir (pero no se limitan a) agrupación o la clasificación basada en métodos estadísticos, ciertos tipos de discretización y aprendizaje con recuentos.  
  
**Resolución:**
 
 + Abra el módulo que devolvió el error y comprobar las propiedades de conjunto de datos y el módulo de entrada. 
 + Compruebe que el conjunto de datos de entrada no está vacía y que no hay suficientes filas de datos para cumplir los requisitos descritos en la Ayuda del módulo.  
 + Si los datos se cargan desde un origen externo, asegúrese de que el origen de datos está disponible y que no hay ningún error o cambiar en la definición de datos que haría que el proceso de importación obtener menos filas.
 + Si está realizando una operación en los datos de nivel superior del módulo que puede afectar al tipo de datos o el número de valores, como la limpieza, la división, o participar en operaciones, compruebe los resultados de esas operaciones para determinar el número de filas devueltas.  



## <a name="error-0022"></a>Error 0022  
 Se produce una excepción si el número de columnas seleccionadas en el conjunto de datos de entrada no es igual al número esperado.  
  
 Este error en Azure Machine Learning puede producirse cuando el módulo de nivel inferior o la operación requiere un número específico de entradas o columnas, y ha proporcionado demasiados o muy pocas columnas o entradas. Por ejemplo:   
  
-   Especificar una columna de una sola etiqueta o una columna de clave y seleccionó accidentalmente varias columnas.  
  
-   Está cambiando el nombre de columnas, pero proporcionan nombres más o menos que el número de columnas.  
  
-   El número de columnas en el origen o destino ha cambiado o no coincide con el número de columnas utilizados por el módulo.  
  
-   Ha proporcionado una lista separada por comas de valores para las entradas, pero no coincide con el número de valores o no se admiten varias entradas.  
  
**Resolución:** Volver a visitar el módulo y compruebe la selección de columna para asegurarse de que está seleccionado el número correcto de columnas. Compruebe las salidas de los módulos de nivel superior y los requisitos de operaciones de nivel inferiores.  
  
 Si usa una de las opciones de selección de columna que pueden seleccionar varias columnas (los índices de columnas, todas las características, numéricas, etc.), validar el número exacto de las columnas devueltas por la selección.  
  
 <!--If you are trying to specify a comma-separated list of datasets as inputs to [Unpack Zipped Datasets](unpack-zipped-datasets.md), unpack only one dataset at a time. Multiple inputs are not supported.  -->
  
 Compruebe que no ha cambiado el número o tipo de columnas de nivel superior.  
  
 Si usa un conjunto de datos de recomendación para entrenar un modelo, recuerde que el Recomendador espera un número limitado de columnas, correspondiente a las clasificaciones de elemento de usuario o de pares usuario-elemento. Quitar columnas adicionales antes de entrenar el modelo o la división de conjuntos de datos de recomendación. Para obtener más información, consulte [dividir datos](split-data.md).  
  
|Mensajes de excepción|  
|------------------------|  
|Número de columnas seleccionadas en el conjunto de datos de entrada no es igual al número esperado.|  
|Número de columnas seleccionadas en el conjunto de datos de entrada no es igual a {0}.|  
|Patrón de selección de columna "{0}" proporciona el número de columnas seleccionadas en el conjunto de datos de entrada no es igual a {1}.|  
|Patrón de selección de columna "{0}" se espera que proporcione {1} columnas seleccionadas en el conjunto de datos de entrada, pero {2} columnas es/se proporcionan.|  



## <a name="error-0023"></a>Error 0023  
 Se produce una excepción si la columna de destino de conjunto de datos de entrada no es válida para el módulo de entrenamiento actual.  
  
 Si la columna de destino (como se seleccionaron en los parámetros del módulo) no es del tipo de datos válido, incluidos todos los valores que faltan o no era categoría según lo esperado, se produce este error en Azure Machine Learning.  
  
**Resolución:** Volver a visitar la entrada para inspeccionar el contenido de la columna de etiqueta o destino de módulo. Asegúrese de que no tiene todos los valores que faltan. Si el módulo está esperando la columna de destino para que sea de categorías, asegúrese de que hay más de uno valores distintos en la columna de destino.  
  
|Mensajes de excepción|  
|------------------------|  
|Conjunto de datos de entrada tiene no compatible de columna de destino.|  
|Conjunto de datos de entrada con una columna de destino no admitida "{0}".|  
|Conjunto de datos de entrada con una columna de destino no admitida "{0}" para el aprendiz de tipo {1}.|  
 

## <a name="error-0024"></a>Error 0024  
Se produce una excepción si el conjunto de datos no contiene una columna de etiqueta.  

 Este error en Azure Machine Learning se produce cuando el módulo requiere una columna de etiqueta y el conjunto de datos no tiene una columna de etiqueta. Por ejemplo, la evaluación de un conjunto de datos con puntuación normalmente requiere que una columna de etiqueta está presente para calcular métricas de precisión.  
 
También puede ocurrir que una columna de etiqueta está presente en el conjunto de datos, pero no se detectó correctamente Azure Machine Learning.
  
**Resolución:**

+ Abra el módulo que ha generado el error y determinar si una columna de etiqueta está presente. El tipo de datos o el nombre de la columna no importa, siempre y cuando la columna contiene un único resultado (o variable dependiente) que está intentando predecir. Si no está seguro de qué columna tiene la etiqueta, busque un nombre genérico, como *clase* o *destino*. 
+  Si el conjunto de datos no incluye una columna de etiqueta, es posible que la columna de etiqueta explícitamente o por accidente quitó ascendente. También podría ser que el conjunto de datos no es el resultado de un módulo de puntuación ascendente.
+ Para marcar explícitamente la columna como columna de etiqueta, agregue el [editar metadatos](edit-metadata.md) módulo y conecte el conjunto de datos. Seleccione solo la columna de etiqueta y seleccione **etiqueta** desde el **campos** lista desplegable. 
+ Si se elige la columna errónea como la etiqueta, puede seleccionar **Borrar etiqueta** desde el **campos** para corregir los metadatos en la columna. 
  
|Mensajes de excepción|  
|------------------------|  
|No hay ninguna columna de etiqueta en el conjunto de datos.|  
|No hay ninguna columna de etiqueta en "{0}".|  
  

## <a name="error-0025"></a>Error 0025  
 Se produce una excepción si el conjunto de datos no contiene una columna de puntuación.  
  
 Este error en Azure Machine Learning se produce si la entrada para el modelo de evaluación no contiene puntuación válida columnas. Por ejemplo, el usuario intenta evaluar un conjunto de datos antes de se puntúa con un modelo entrenado correcto o la columna de puntuación explícitamente se quitó de nivel superior. Esta excepción también se produce si las columnas de puntuación en los dos conjuntos son incompatibles. Por ejemplo, es posible que esté intentando comparar la precisión de un regresor lineal con el de un clasificador binario.  
  
**Resolución:** Volver a visitar la entrada para el modelo de evaluar y examinar si contiene una o varias columnas de puntuación. Si no es así, el conjunto de datos no se puntúan o se han quitado las columnas de puntuación en un módulo de nivel superior.  
  
|Mensajes de excepción|  
|------------------------|  
|No hay ninguna columna de puntuación en el conjunto de datos.|  
|No hay ninguna columna de puntuación en "{0}".|  
|No hay ninguna columna de puntuación en "{0}" que se produjeron en un "{1}". Puntuar el conjunto de datos usando el tipo correcto de aprendiz.|  
  

## <a name="error-0026"></a>Error 0026  
 Se produce una excepción si no se permiten columnas con el mismo nombre.  
  
 Si varias columnas tienen el mismo nombre, se produce este error en Azure Machine Learning. Una forma puede recibir este error es si el conjunto de datos no tiene una fila de encabezado y se les asigna automáticamente nombres de columna: Col0, Col1, etc.  
  
**Resolución:** Si las columnas tienen el mismo nombre, inserte un [editar metadatos](edit-metadata.md) módulo entre el conjunto de datos de entrada y el módulo. Utilice el selector de columnas en [editar metadatos](edit-metadata.md) para seleccionar columnas para cambiar el nombre, escriba los nuevos nombres en el **nuevos nombres de columna** cuadro de texto.  
  
|Mensajes de excepción|  
|------------------------|  
|Nombres de columna iguales se especifican argumentos de entrada. No se permiten nombres de columna iguales por módulo.|  
|Nombres de columna en los argumentos iguales "{0}"y"{1}" no se permiten. Especifique nombres diferentes.|  
  

## <a name="error-0027"></a>Error 0027  
 Se produce una excepción cuando dos objetos tienen que ser del mismo tamaño pero no están.  
  
 Esto es un error común en Azure Machine Learning y puede deberse a muchas condiciones.  
  
**Resolución:** No hay ninguna resolución específica. Sin embargo, puede comprobar las condiciones como la siguiente:  
  
-   Si está cambiando el nombre de las columnas, asegúrese de que cada lista (las columnas de entrada y la lista de nombres nuevo) tiene el mismo número de elementos.  
  
-   Si va a unir, o concatenar dos conjuntos de datos, asegúrese de que tienen el mismo esquema.  
  
-   Si va a unir dos conjuntos de datos que tienen varias columnas, asegúrese de que las columnas de clave tienen los mismos datos escriba y seleccione la opción **permite duplicados y conservar el orden de las columnas de selección**.  
  
|Mensajes de excepción|  
|------------------------|  
|El tamaño de los objetos pasados es incoherente.|  
|El tamaño de "{0}"no es coherente con el tamaño de"{1}".|  
  

## <a name="error-0028"></a>Error 0028  
 Excepción se produce en el caso cuando el conjunto de columnas contiene nombres de columna duplicados y no está permitido.  
  
 Este error en Azure Machine Learning se produce cuando se duplican los nombres de columna; es decir, no es único.  
  
**Resolución:** Si las columnas tienen mismo nombre, agregue una instancia de [editar metadatos](edit-metadata.md) entre el conjunto de datos de entrada y el módulo genera el error. Utilice el Selector de columnas en [editar metadatos](edit-metadata.md) para seleccionar columnas para cambiar el nombre y escriba los nuevos nombres de columnas en el **nuevos nombres de columna** cuadro de texto. Si está cambiando el nombre de varias columnas, asegúrese de que los valores que escriba en el **nuevos nombres de columna** son únicos.  
  
|Mensajes de excepción|  
|------------------------|  
|Conjunto de columnas contiene nombres de columna duplicados.|  
|El nombre "{0}" está duplicado.|  
|El nombre "{0}"está duplicado en"{1}".|  
  

## <a name="error-0029"></a>Error 0029  
 Se produce una excepción cuando se pasa un URI no válido.  
  
 Este error en Azure Machine Learning se produce cuando se pasa un URI no válido.  Recibirá este error si se cumple alguna de las condiciones siguientes:, o.  
  
-   La URI de SAS o pública proporcionada para Azure Blob Storage para lectura o escritura contiene un error.  
  
-   Ha expirado el período de tiempo para las asociaciones de seguridad.  
  
-   La dirección URL Web a través de origen HTTP representa un archivo o un URI de bucle invertido.  
  
-   La dirección URL Web a través de HTTP contiene una dirección URL con formato incorrecto.  
  
-   No se puede resolver la dirección URL por el origen remoto.  
  
**Resolución:** Volver a visitar el módulo y compruebe el formato del URI. Si el origen de datos es una dirección URL Web a través de HTTP, compruebe que el origen deseado no es un archivo o un URI (localhost) de bucle invertido.  
  
|Mensajes de excepción|  
|------------------------|  
|Se pasa un Uri no válido.|  
  

## <a name="error-0030"></a>Error 0030  
 Excepción se produce en el caso cuando no es posible descargar un archivo.  
  
 Esta excepción en Azure Machine Learning se produce cuando no es posible descargar un archivo. Recibirá esta excepción cuando falla una intentada lectura desde un origen HTTP después de tres (3) vuelva a intentar intentos.  
  
**Resolución:** Compruebe que el URI para el origen HTTP es correcto y que el sitio está actualmente accesible a través de Internet.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede descargar un archivo.|  
|Error al descargar el archivo: {0}.|  
  

## <a name="error-0031"></a>Error 0031  
 Se produce una excepción si el número de columnas de conjunto de columnas es menor que el necesario.  
  
 Este error en Azure Machine Learning se produce si el número de columnas seleccionado es menor que el necesario.  Recibirá este error si el mínimo requerido no se selecciona el número de columnas.  
  
**Resolución:** Agregar columnas adicionales a la selección de columna mediante el **Selector de columnas**.  
  
|Mensajes de excepción|  
|------------------------|  
|Número de columnas de conjunto de columnas es menor que el necesario.|  
|{0} se deben especificar las columnas. El número real de las columnas especificadas es {1}.|  

## <a name="error-0032"></a>Error 0032  
 Se produce una excepción si el argumento no es un número.  
  
 Si el argumento es un valor doble o NaN, recibirá este error en Azure Machine Learning.  
  
**Resolución:** Modifique el argumento especificado para usar un valor válido.  
  
|Mensajes de excepción|  
|------------------------|  
|Argumento no es un número.|  
|"{0}" no es un número.|  
  

## <a name="error-0033"></a>Error 0033  
 Se produce una excepción si el argumento es infinito.  
  
 Si el argumento es infinito, se produce este error en Azure Machine Learning. Recibirá este error si el argumento es `double.NegativeInfinity` o `double.PositiveInfinity`.  
  
**Resolución:** Modifique el argumento especificado para que sea un valor válido.  
  
|Mensajes de excepción|  
|------------------------|  
|Es el argumento deben ser finito.|  
|"{0}" no es finito.|  
  

## <a name="error-0034"></a>Error 0034  
 Se produce una excepción si existe más de una clasificación para un par usuario-elemento determinado.  
  
 Se produce este error en Azure Machine Learning en recomendación si un par usuario-elemento tiene el valor de más de una clasificación.  
  
**Resolución:** Asegúrese de que el par usuario-elemento posee un valor de clasificación solo.  
  
|Mensajes de excepción|  
|------------------------|  
|Existe más de una clasificación para los valores del conjunto de datos.|  
|Más de una clasificación de usuario {0} y elemento {1} en la tabla de datos de predicción de clasificación.|  
  

## <a name="error-0035"></a>Error 0035  
 Se produce una excepción si no se proporcionaron características para un usuario determinado o un elemento.  
  
 Se produce este error en Azure Machine Learning está intentando usar un modelo de recomendación para la puntuación, pero no se encuentra un vector de característica.  
  
**Resolución:**

El Recomendador Matchbox tiene ciertos requisitos que deben cumplirse cuando se usa características de elementos o características de usuario.  Este error indica que falta un vector de característica para un usuario o un elemento que proporciona como entrada.  Debe asegurarse de que un vector de características está disponible en los datos para cada usuario o elemento.  
  
 Por ejemplo, si entrena una recomendación modelar mediante características como la edad del usuario, ubicación o ingresos, pero ahora desea crear puntuaciones para nuevos usuarios que no se habían visto durante el entrenamiento, debe proporcionar un conjunto equivalente de características (es decir, la edad, la ubicación, y los valores de ingresos) para los nuevos usuarios con el fin de realizar predicciones adecuadas para ellos. 
 
 Si no tiene ninguna característica para estos usuarios, considere la posibilidad de ingeniería de características para generar características adecuadas.  Por ejemplo, si no tiene los valores de edad o los ingresos de usuario individual, puede generar valores aproximados para un grupo de usuarios. 
 
<!--When you are scoring from a recommendation mode, you can use item or user features only if you previously used item or user features during training. For more information, see [Score Matchbox Recommender](score-matchbox-recommender.md).
 
For general information about how the Matchbox recommendation algorithm works, and how to prepare a dataset of item features or user features, see [Train Matchbox Recommender](train-matchbox-recommender.md).  -->
  
 > [!TIP]
 > ¿Resolución no es aplicable a su caso? Pueden enviar comentarios sobre este artículo y proporcionan información sobre el escenario, incluido el módulo y el número de filas en la columna. Usaremos esta información para proporcionar más pasos para solucionar problemas en el futuro.
   
|Mensajes de excepción|  
|------------------------|  
|No se proporcionaron características para un usuario o elemento.|  
|Las características para {0} necesaria pero no ha proporcionado.|  
  

## <a name="error-0036"></a>Error 0036  
 Se produce una excepción si se proporcionaron varios vectores de característica para un usuario determinado o un elemento.  
  
 Si un vector de característica se ha definido más de una vez, se produce este error en Azure Machine Learning.  
  
**Resolución:** Asegúrese de que el vector de característica no está definido más de una vez.  
  
|Mensajes de excepción|  
|------------------------|  
|Característica definición duplicada para un usuario o elemento.|  
|Definición de característica para duplicada {0}.|  
  

## <a name="error-0037"></a>Error 0037  
 Se produce una excepción si se especifican varias columnas de etiqueta y se permite solo una.  
  
 Si se selecciona más de una columna que la nueva columna de etiqueta, se produce este error en Azure Machine Learning. Más algoritmos de aprendizaje supervisado requieren una sola columna se marquen como el destino o la etiqueta.  
  
**Resolución:** Asegúrese de seleccionar una sola columna como la nueva columna de etiqueta.  
  
|Mensajes de excepción|  
|------------------------|  
|Se especifican varias columnas de etiqueta.|  
  

## <a name="error-0038"></a>Error 0038  
 Excepción se produce si el número de elementos esperado debe ser un valor exacto, pero no es.  
  
 Este error en Azure Machine Learning se produce si el número de elementos esperado debe ser un valor exacto, pero no es.  Recibirá este error si el número de elementos no es igual que el valor esperado válido.  
  
**Resolución:** Modifique la entrada para que tenga el número correcto de elementos.  
  
|Mensajes de excepción|  
|------------------------|  
|Número de elementos no es válido.|  
|Número de elementos de "{0}" no es válido.|  
|Número de elementos de "{0}" no es igual al número válido de {1} elementos.|  
  

## <a name="error-0039"></a>Error 0039  
 Se produce una excepción si una operación ha dado error.  
  
 Este error en Azure Machine Learning se produce cuando no se puede completar una operación interna.  
  
**Resolución:** Este error se produce por muchas condiciones y no hay ninguna solución específica.  
 En la tabla siguiente contiene los mensajes genéricos para este error, que van seguidos de una descripción específica de la condición. 
 
 Si no hay detalles disponibles, [enviar comentarios](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) y proporcionan información acerca de los módulos que generó el error y las condiciones relacionadas.
  
|Mensajes de excepción|  
|------------------------|  
|Error en la operación|  
|Error al completar la operación: {0}.|  
  

## <a name="error-0040"></a>Error 0040  
 Se produce una excepción al llamar a un módulo en desuso.  
  
 Este error en Azure Machine Learning se produce cuando se llama a un módulo en desuso.  
  
**Resolución:** Reemplazar el módulo en desuso por uno compatible. Consulte el registro de salida del módulo para obtener información sobre qué módulo va a usar en su lugar.  
  
|Mensajes de excepción|  
|------------------------|  
|Acceso a módulo desusado.|  
|Módulo "{0}" está en desuso. Utilizar el módulo "{1}" en su lugar.|  
 

## <a name="error-0041"></a>Error 0041  
 Se produce una excepción al llamar a un módulo en desuso.  
  
 Este error en Azure Machine Learning se produce cuando se llama a un módulo en desuso.  
  
**Resolución:** Reemplazar el módulo en desuso con un conjunto de los admitidos. Esta información debe estar visible en el registro de salida del módulo.  
  
|Mensajes de excepción|  
|------------------------|  
|Acceso a módulo desusado.|  
|Módulo "{0}" está en desuso. Utilice los módulos "{1}" para la funcionalidad solicitada.|  
 

## <a name="error-0042"></a>Error 0042  
 Se produce una excepción cuando no es posible convertir la columna a otro tipo.  
  
 Este error en Azure Machine Learning se produce cuando no es posible convertir la columna al tipo especificado.  Recibirá este error si un módulo requiere un tipo de datos determinado, como la fecha y hora, texto, un número de punto flotante o entero, pero no es posible convertir una columna existente al tipo requerido.  
 
Por ejemplo, puede seleccionar una columna e intenta convertirlo a un tipo de datos numéricos para su uso en una operación matemática y recibe este error si la columna contiene datos no válidos. 

Otra razón podría obtener este error si intenta utilizar una columna que contiene números de punto flotante o muchos valores únicos como una columna de categorías. 
  
**Resolución:**

+ Abra la página de Ayuda del módulo que ha generado el error y comprobar los requisitos de tipo de datos.
+ Revise los tipos de datos de las columnas del conjunto de datos de entrada.
+ Inspeccione los datos que se originan en los orígenes de datos sin esquemas llamado.
+ Compruebe el conjunto de datos para los valores que faltan o los caracteres especiales que podrían bloquear la conversión al tipo de datos deseado. 
    + Los tipos de datos numéricos deben ser coherentes: por ejemplo, compruebe los números de punto flotante en una columna de enteros.
    + Buscar cadenas de texto o los valores NA en una columna de número. 
    + Los valores booleanos se pueden convertir en una representación adecuada según el tipo de datos necesarios.
    + Examine las columnas de texto para caracteres no unicode, caracteres de tabulación o caracteres de control
    + Datos de fecha y hora deben ser coherentes para evitar errores de modelado, pero limpieza puede ser compleja debido a los formatos de número. Considere el uso de <!--the [Execute R Script](execute-r-script.md) or -->[Ejecutar Script de Python](execute-python-script.md) módulos para realizar la limpieza.  
+ Si es necesario, modifique los valores del conjunto de datos de entrada para que la columna se puede convertir correctamente. Puede incluir la modificación de discretización, el truncamiento o redondeo de las operaciones, la eliminación de los valores atípicos o imputación de valores que faltan. Consulte los siguientes artículos para algunos escenarios comunes de transformación de datos en machine learning:
    + [Limpiar datos que faltan](clean-missing-data.md)
    + [Normalizar datos](normalize-data.md)
<!--+ [Clip Values](clip-values.md) 
    + [Group Data Into Bins](group-data-into-bins.md)
  -->
 
> [!TIP]
> ¿Resolución clara o no es aplicable a su caso? Pueden enviar comentarios sobre este artículo y proporcionan información sobre el escenario, incluido el módulo y el tipo de datos de la columna. Usaremos esta información para proporcionar más pasos para solucionar problemas en el futuro.  
  
|Mensajes de excepción|  
|------------------------|  
|No permite la conversión.|  
|No se pudo convertir la columna de tipo {0} en la columna de tipo {1}.|  
|No se pudo convertir la columna "{2}" de tipo {0} en la columna de tipo {1}.|  
|No se pudo convertir la columna "{2}" de tipo {0} a la columna "{3}" de tipo {1}.|  
  

## <a name="error-0043"></a>Error 0043  
 Se produce una excepción cuando el tipo de elemento no implementa explícitamente es igual a.  
  
 Este error en Azure Machine Learning se utiliza y dejará de utilizarse.  
  
**Resolución:** Ninguno.  
  
|Mensajes de excepción|  
|------------------------|  
|Método explícito no accesible Equals encontrado.|  
|No se pueden comparar los valores de columna \\"{0}\\" de tipo {1}. Método explícito no accesible Equals encontrado.|  


## <a name="error-0044"></a>Error 0044  
 Se produce una excepción cuando no es posible derivar el tipo de elemento de columna de los valores existentes.  
  
 Este error en Azure Machine Learning se produce cuando no es posible deducir el tipo de una o varias columnas en un conjunto de datos. Esto suele suceder al concatenar dos o más conjuntos de datos con diferentes tipos de elemento. Si no puede determinar un tipo común que pueda representar todos los valores de una o varias columnas sin pérdida de información de Azure Machine Learning, generará este error.  
  
**Resolución:** Asegúrese de que todos los valores de una columna determinada en ambos conjuntos de datos que se va a combinar son del mismo tipo (numérico, booleano, categoría, cadena, fecha, etc.) o puede convertirse en el mismo tipo.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede derivar el tipo de elemento de la columna.|  
|No se puede derivar el tipo de elemento de la columna "{0}"--todos los elementos son referencias nulas.|  
|No se puede derivar el tipo de elemento de la columna "{0}"del conjunto de datos"{1}"--todos los elementos son referencias nulas.|  
  

## <a name="error-0045"></a>Error 0045  
 Se produce una excepción cuando no es posible crear una columna por tipos de elementos combinados en el origen.  
  
 Este error en Azure Machine Learning se produce cuando los tipos de elemento del que se va a combinar dos conjuntos de datos son diferentes.  
  
**Resolución:** Asegúrese de que todos los valores de una columna determinada en ambos conjuntos de datos que se va a combinar son del mismo tipo (numérico, booleano, categoría, cadena, fecha, etcetera.).  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede crear la columna con tipos de elementos combinados.|  
|No se puede crear la columna con el identificador "{0}" de los tipos de elementos mixtos: \n\tType de datos [{1}, {0}] es {2}\n\tType de datos [{3}, {0}] es {4}.|  
  

## <a name="error-0046"></a>Error 0046  
 Se produce una excepción cuando no es posible crear el directorio en la ruta de acceso especificada.  
  
 Este error en Azure Machine Learning se produce cuando no es posible crear un directorio en la ruta de acceso especificada. Recibirá este error si cualquier parte de la ruta de acceso al directorio de salida para una consulta de Hive es incorrecta o inaccesible.  
  
**Resolución:** Volver a visitar el módulo y compruebe que la ruta de acceso del directorio es el formato correcto y que sea accesible con las credenciales actuales.  
  
|Mensajes de excepción|  
|------------------------|  
|Especifique un directorio de salida válido.|  
|Directorio: {0} no se puede crear. Especifique la ruta de acceso válida.|  
  

## <a name="error-0047"></a>Error 0047  
 Se produce una excepción si el número de columnas de característica de algunos de los conjuntos de datos que se pasan al módulo es demasiado pequeño.  
  
 Si el conjunto de datos de entrada para el entrenamiento no contiene el número mínimo de las columnas necesarias en el algoritmo, se produce este error en Azure Machine Learning. Normalmente, ya sea el conjunto de datos está vacío o solo contiene columnas de entrenamiento.  
  
**Resolución:** Vuelva a consultar el conjunto de datos de entrada con el fin está seguro de que hay una o más columnas adicionales aparte de la columna de etiqueta.  
  
|Mensajes de excepción|  
|------------------------|  
|Número de columnas de característica en el conjunto de datos de entrada es menor que el mínimo permitido.|  
|Número de columnas de característica en el conjunto de datos de entrada es menor que el mínimo permitido de {0} columnas.|  
|Número de columnas de característica en el conjunto de datos de entrada "{0}" es menor que el mínimo permitido de {1} columnas.|  
  

## <a name="error-0048"></a>Error 0048  
 Excepción se produce en el caso cuando no es posible abrir un archivo.  
  
 Este error en Azure Machine Learning se produce cuando no es posible abrir un archivo para lectura o escritura. Este error puede aparecer por estos motivos:  
  
-   No existe el contenedor o el archivo (blob)  
  
-   El nivel de acceso del archivo o el contenedor no podrá tener acceso al archivo  
  
-   El archivo es demasiado grande para lectura o un formato incorrecto  
  
**Resolución:** Volver a visitar el módulo y el archivo que está intentando leer.  
  
 Compruebe que los nombres del contenedor y del archivo son correctos.  
  
 Utilice el portal de Azure clásico o una herramienta de almacenamiento de Azure para comprobar que tiene permiso para tener acceso al archivo.  
  
  <!--If you are trying to read an image file, make sure that it meets the requirements for image files in terms of size, number of pixels, and so forth. For more information, see [Import Images](import-images.md).  -->
  
|Mensajes de excepción|  
|------------------------|  
|No se puede abrir un archivo.|  
|Error al abrir el archivo: {0}.|  


## <a name="error-0049"></a>Error 0049  
 Excepción se produce en el caso cuando no es posible analizar un archivo.  
  
 Este error en Azure Machine Learning se produce cuando no es posible analizar un archivo. Recibirá este error si el formato de archivo seleccionado en el [importar datos](import-data.md) módulo no coincide con el formato real del archivo, o si el archivo contiene un carácter no reconocible.  
  
**Resolución:** Volver a visitar el módulo y corrija la selección de formato de archivo si no coincide con el formato del archivo. Si es posible, inspeccione el archivo para confirmar que no contiene ningún carácter no válido.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede analizar un archivo.|  
|Error al analizar el archivo: {0}.|  
  

## <a name="error-0050"></a>Error 0050  
 Se produce una excepción en el caso cuando la entrada y los archivos de salida son los mismos.  
  
**Resolución:** Este error en Azure Machine Learning se utiliza y dejará de utilizarse.  
  
|Mensajes de excepción|  
|------------------------|  
|Los archivos especificados para la entrada y salida no pueden ser el mismo.|


## <a name="error-0051"></a>Error 0051  
 Excepción se produce en el caso cuando varios archivos de salida son los mismos.  
  
**Resolución:** Este error en Azure Machine Learning se utiliza y dejará de utilizarse.  
  
|Mensajes de excepción|  
|------------------------|  
|Los archivos especificados para las salidas no pueden coincidir.|


## <a name="error-0052"></a>Error 0052  
 Se produce una excepción si se especifica incorrectamente el clave de cuenta de almacenamiento de Azure.  
  
 Si la clave utilizada para tener acceso a la cuenta de almacenamiento de Azure es incorrecta, se produce este error en Azure Machine Learning. Por ejemplo, es posible que vea este error si la clave de almacenamiento de Azure se ha truncado al copiar y pegar, o si se usó una clave incorrecta.  
  
 Para obtener más información acerca de cómo obtener la clave para una cuenta de almacenamiento de Azure, consulte [visualización, copia y regeneración de claves de acceso de almacenamiento](https://azure.microsoft.com/documentation/articles/storage-create-storage-account-classic-portal/).  
  
**Resolución:** Volver a visitar el módulo y compruebe que la clave de almacenamiento de Azure es correcta para la cuenta; Copie la clave de nuevo desde el portal de Azure clásico si es necesario.  
  
|Mensajes de excepción|  
|------------------------|  
|La clave de cuenta de almacenamiento de Azure es incorrecta.|  
  

## <a name="error-0053"></a>Error 0053  
 Excepción se produce en el caso cuando no haya elementos para recomendaciones de matchbox ni funciones de usuario.  
  
 Este error en Azure Machine Learning se produce cuando no se encuentra un vector de característica.  
  
**Resolución:** Asegúrese de que un vector de característica se encuentra en el conjunto de datos de entrada.  
  
|Mensajes de excepción|  
|------------------------|  
|Características de usuario o / y los elementos son necesarios, pero no proporciona.|  

## <a name="error-0054"></a>Error 0054  
 Se produce una excepción si no hay suficientes valores distintivos en la columna para completar la operación.  
  
**Resolución:** Este error en Azure Machine Learning se utiliza y dejará de utilizarse.  
  
|Mensajes de excepción|  
|------------------------|  
|Datos tienen insuficientes valores distintivos en la columna especificada para completar la operación.|  
|Datos tienen insuficientes valores distintivos en la columna especificada para completar la operación. El mínimo requerido es {0} elementos.|  
|Datos tienen insuficientes valores distintivos en la columna "{1}" para completar la operación. El mínimo requerido es {0} elementos.|  
  

## <a name="error-0055"></a>Error 0055  
 Se produce una excepción al llamar a un módulo en desuso.  Si intenta llamar a un módulo que ha quedado en desuso, aparece este error en Azure Machine Learning.
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Acceso a módulo desusado.|  
|Módulo "{0}" está en desuso.|  

## <a name="error-0056"></a>Error 0056  
 Se produce una excepción si las columnas seleccionadas para una operación infringe los requisitos.  
  
 Este error en Azure Machine Learning se produce cuando se seleccionan las columnas para una operación que requiere la columna de un tipo de datos determinado. 
 
 Este error también puede ocurrir si la columna es el tipo de datos correcto, pero el módulo que está utilizando requiere que la columna también marcarse como una característica, una etiqueta o una columna de categorías.  
  
  <!--For example, the [Convert to Indicator Values](convert-to-indicator-values.md) module requires that columns be categorical, and will raise this error if you select a feature column or label column.  -->
  
**Resolución:**
  
1.  Revise el tipo de datos de las columnas actualmente seleccionadas. 

2. Determinar si las columnas seleccionadas son de categorías, etiqueta o columnas de característica.  
  
3.  Revise el tema de ayuda para el módulo en el que se realiza la selección de columna, para determinar si hay requisitos específicos para el uso de tipo o una columna de datos.  
  
3.  Use [editar metadatos](edit-metadata.md) para cambiar el tipo de columna para la duración de esta operación. No olvide cambiar el tipo de columna a su valor original, con otra instancia de [editar metadatos](edit-metadata.md)si se necesita para las operaciones de nivel inferiores.  
  
|Mensajes de excepción|  
|------------------------|  
|Una o más columnas seleccionadas no estaban en una categoría permitida.|  
|Columna con nombre "{0}" no es una categoría permitida.|  
  

## <a name="error-0057"></a>Error 0057  
 Se produce una excepción al intentar crear un archivo o blob que ya existe.  
  
 Esta excepción se produce cuando se usa el [exportar datos](export-data.md) módulo u otro módulo para guardar los resultados de un experimento en Azure Machine Learning en almacenamiento de blobs de Azure, pero se intenta crear un archivo o blob que ya existe.   
  
**Resolución:**
 
 Recibirá este error solo si ha establecido previamente la propiedad **almacenamiento de blobs de Azure en modo de escritura** a **Error**. Por diseño, este módulo genera un error si se intenta escribir un conjunto de datos en un blob que ya existe.
 
 - Abra las propiedades del módulo y cambie la propiedad **almacenamiento de blobs de Azure en modo de escritura** a **sobrescritura**.
 - Como alternativa, puede escriba el nombre de un blob de destino diferente o un archivo y asegúrese de especificar un blob que aún no existe.  
  
|Mensajes de excepción|  
|------------------------|  
|Ya existe el archivo o Blob.|  
|El archivo o Blob "{0}" ya existe.|  
  

## <a name="error-0058"></a>Error 0058  
 Este error en Azure Machine Learning se produce si el conjunto de datos no contiene la columna de etiqueta esperada.  
  
 Esta excepción también puede producirse cuando la columna de etiqueta proporcionado no coincide con los datos o el tipo de datos esperado por el factor, o tiene valores incorrectos. Por ejemplo, esta excepción se produce cuando se usa una columna de etiqueta con valores reales al entrenar un clasificador binario.  
  
**Resolución:** La resolución depende el aprendiz o instructor que está usando y los tipos de datos de las columnas del conjunto de datos. En primer lugar, compruebe los requisitos del algoritmo de aprendizaje automático o el módulo de entrenamiento.  
  
 Vuelva a consultar el conjunto de datos de entrada. Compruebe que la columna que piensa se tratan como la etiqueta tiene un tipo para el modelo que se va a crear los datos correctos.  
  
 Compruebe las entradas para los valores que faltan y eliminar o reemplazarlos si es necesario.  
  
 Si es necesario, agregue el [editar metadatos](edit-metadata.md) módulo y asegúrese de que la columna de etiqueta está marcada como una etiqueta.  
  
|Mensajes de excepción|  
|------------------------|  
|La columna de etiqueta no es el esperado|  
|La columna de etiqueta no es como esperaba en "{0}".|  
|La columna de etiqueta "{0}"no se esperaba en"{1}".|  
  

## <a name="error-0059"></a>Error 0059  
 Se produce una excepción si no se puede analizar un índice de columna especificado en un selector de columna.  
  
 Si no se puede analizar un índice de columna especificado cuando se usa el Selector de columnas, se produce este error en Azure Machine Learning.  Recibirá este error cuando el índice de columna está en un formato no válido que no se puede analizar.  
  
**Resolución:** Modificar el índice de columna para usar un valor de índice válido.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede analizar uno o más índices de columna especificada o intervalos de índice.|  
|Índice de columna o de intervalo "{0}" no se pudo analizar.|  
  

## <a name="error-0060"></a>Error 0060  
 Se produce una excepción cuando se especifica un fuera del intervalo de la columna de intervalo en un selector de columna.  
  
 Este error en Azure Machine Learning se produce cuando se especifica un intervalo de columna fuera del intervalo en el Selector de columnas. Recibirá este error si el rango de la columna en el selector de columna no corresponde a las columnas del conjunto de datos.  
  
**Resolución:** Modificar el intervalo de columna en el selector de columna para que coincida con las columnas del conjunto de datos.  
  
|Mensajes de excepción|  
|------------------------|  
|No válido o está fuera del intervalo de índices de columna de intervalo especificado.|  
|Rango de la columna "{0}" no es válido o fuera del intervalo.|  
  

## <a name="error-0061"></a>Error 0061  
 Se produce una excepción al intentar agregar una fila a un objeto DataTable que tiene un número diferente de columnas que la tabla.  
  
 Este error en Azure Machine Learning se produce cuando se intenta agregar una fila a un conjunto de datos que tiene un número diferente de columnas que el conjunto de datos.  Recibirá este error si la fila que se va a agregar al conjunto de datos tiene un número diferente de columnas del conjunto de datos de entrada.  La fila no se puede anexar al conjunto de datos si el número de columnas es diferente.  
  
**Resolución:** Modificar el conjunto de datos de entrada para que tenga el mismo número de columnas que la fila agregada o modifique la fila agregada a tener el mismo número de columnas que el conjunto de datos.  
  
|Mensajes de excepción|  
|------------------------|  
|Todas las tablas deben tener el mismo número de columnas.|  
  

## <a name="error-0062"></a>Error 0062  
 Se produce una excepción cuando se intenta comparar dos modelos con tipos diferentes de aprendiz.  
  
 Este error en Azure Machine Learning se produce cuando no se pueden comparar las métricas de evaluación de dos conjuntos diferentes de datos con puntuación. En este caso, no es posible comparar la eficacia de los modelos usados para generar los dos conjuntos de datos con puntuación.  
  
**Resolución:** Compruebe que los resultados puntuados producidos por el mismo tipo de modelo de aprendizaje automático (clasificación binaria, regresión, clasificación de varias clases, la recomendación de agrupación en clústeres, detección de anomalías, etcetera.) Todos los modelos que se comparan deben tener el mismo tipo de aprendiz.  
  
|Mensajes de excepción|  
|------------------------|  
|Todos los modelos deben tener el mismo tipo de aprendiz.|  
  

 <!--## Error 0063  
 This exception is raised when R script evaluation fails with an error.  
  
 This error occurs when you have provided an R script in one of the [R language modules](r-language-modules.md) in Azure Machine Learning, and the R code contains internal syntax errors. The exception can also occur if you provide the wrong inputs to the R script. 
 
 The error can also occur if the script is too large to execute in the workspace. The maximum script size for the **Execute R Script** module is 1,000 lines or 32 KB of work space, whichever is lesser.
  
**Resolution:**

1. In Azure Machine Learning Studio, right-click the module that has the error, and select **View Log**.
2. Examine the standard error log of the module, which contains the stack trace.
    + Lines beginning with [ModuleOutput] indicate output from R.
    + Messages from R marked as **warnings** typically do not cause the experiment to fail.
3. Resolve script issues.  
    + Check for R syntax errors. Check for variables that are defined but never populated.
    + Review the input data and the script to determine if either the data or variables in the script use characters not supported by Azure Machine Learning.
    + Check whether all package dependencies are installed.
    + Check whether your code loads required libraries that are not loaded by default.
    + Check whether the required packages are the correct version.
    + Make sure that any dataset that you want to output is converted to a data frame.  
4.  Resubmit the experiment.

 <!--
> [!NOTE]
> These topics contains examples of R code that you can use, as well as links to experiments in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com) that use R script.
> + [Execute R Script](execute-r-script.md)
> + [Create R Model](create-r-model.md)
-->  
|Mensajes de excepción|  
|------------------------|  
|Error durante la evaluación de script de R.|  
|Se produjo el error siguiente durante la evaluación del script de R:---inicio del mensaje de error de R--- {0} ---fin del mensaje de error de R---|  
|Durante la evaluación de script de R "{1}" se produjo el siguiente error:---inicio del mensaje de error de R--- {0} ---fin del mensaje de error de R---|  
  


## <a name="error-0064"></a>Error 0064  
 Se produce una excepción si se especifica incorrectamente el nombre de la cuenta de almacenamiento de Azure o la clave de almacenamiento.  
  
 Si se especifica incorrectamente el nombre de la cuenta de almacenamiento de Azure o la clave de almacenamiento, se produce este error en Azure Machine Learning. Recibirá este error si escribe un nombre de cuenta incorrecta o la contraseña para la cuenta de almacenamiento. Esto puede ocurrir si se especifica manualmente el nombre de cuenta o contraseña. También puede producirse si se ha eliminado la cuenta.  
  
**Resolución:** Compruebe que el nombre de cuenta y la contraseña se han escrito correctamente y que existe la cuenta.  
  
|Mensajes de excepción|  
|------------------------|  
|El nombre de la cuenta de almacenamiento de Azure o la clave de almacenamiento no es correcta.|  
|El nombre de cuenta de almacenamiento de Azure "{0}" o clave de almacenamiento para el nombre de cuenta es incorrecta.|  
  

## <a name="error-0065"></a>Error 0065  
 Se produce una excepción si se especifica incorrectamente el nombre de blob de Azure.  
  
 Si se especifica incorrectamente el nombre de blob de Azure, se produce este error en Azure Machine Learning.  Recibirá el error si:  
  
-   El blob no se encuentra en el contenedor especificado.  
  
 <!---   The fully qualified name of the blob specified for output in one of the [Learning with Counts](data-transformation-learning-with-counts.md) modules is greater than 512 characters.  -->
  
-   Solo el contenedor se especificó como el origen en un [importar datos](import-data.md) cuando el formato de Excel o CSV con codificación de solicitud; no se permite la concatenación del contenido de todos los blobs dentro de un contenedor con estos formatos.  
  
-   Un URI de SAS no contiene el nombre de un blob válido.  
  
**Resolución:** Volver a visitar el módulo que produce la excepción. Compruebe que el blob especificado existe en el contenedor de la cuenta de almacenamiento y que los permisos para que pueda ver el blob. Compruebe que la entrada tiene el formato **containername/filename** si tiene Excel o CSV con formatos de codificación. Compruebe que un URI de SAS contiene el nombre de un blob válido.  
  
|Mensajes de excepción|  
|------------------------|  
|El blob de almacenamiento de Azure es incorrecto.|  
|El nombre del blob de almacenamiento de Azure "{0}" no es correcto|  
  

## <a name="error-0066"></a>Error 0066  
 Se produce una excepción si no se pudo cargar un recurso en un Blob de Azure.  
  
 Si no se pudo cargar un recurso en un Blob de Azure, se produce este error en Azure Machine Learning.  <!--You will receive this message if [Train Vowpal Wabbit 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) encounters an error attempting to save either the model or the hash created when training the model.--> Ambos se guardan en la misma cuenta de almacenamiento de Azure que la cuenta que contiene el archivo de entrada.  
  
**Resolución:** Volver a visitar el módulo. Compruebe que el nombre de cuenta de Azure, la clave de almacenamiento y contenedor son correctos y que la cuenta tiene permiso para escribir en el contenedor.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo cargar el recurso al almacenamiento de Azure.|  
|El archivo "{0}" no se pudo cargar en Azure storage como {1}.|  
  

## <a name="error-0067"></a>Error 0067  
 Se produce una excepción si un conjunto de datos tiene un número diferente de columnas de lo esperado.  
  
 Si un conjunto de datos tiene un número diferente de columnas de lo esperado, se produce este error en Azure Machine Learning.  Recibirá este error cuando el número de columnas del conjunto de datos es diferente del número de columnas que el módulo espera durante la ejecución.  
  
**Resolución:** Modifique el conjunto de datos de entrada o los parámetros.  
  
|Mensajes de excepción|  
|------------------------|  
|Número inesperado de las columnas de la datatable.|  
|Se esperaba "{0}"columnas pero se encontraron"{1}" columnas en su lugar.|  
  

## <a name="error-0068"></a>Error 0068  
 Se produce una excepción si el script de Hive especificado no es correcto.  
  
 Si hay errores de sintaxis en un script de Hive QL, o si el intérprete de Hive encuentra un error al ejecutar la consulta o script, se produce este error en Azure Machine Learning.  
  
**Resolución:**

El mensaje de error de Hive es normalmente han informado en el registro de errores para que pueda realizar una acción basada en el error específico. 

+ Abra el módulo e inspeccione la consulta para los errores.  
+ Compruebe que la consulta funciona correctamente fuera de Azure Machine Learning iniciando sesión en la consola de Hive del clúster de Hadoop y ejecutar la consulta.  
+ Intente colocar comentarios en el script de Hive en una línea independiente en lugar de mezclar las instrucciones ejecutables y los comentarios de una sola línea.  

### <a name="resources"></a>Recursos

Consulte los artículos siguientes para obtener ayuda con las consultas de Hive para el aprendizaje automático:

+ [Crear tablas de Hive y cargan datos desde Azure Blob Storage](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-move-hive-tables)
+ [Explorar datos en tablas con consultas de Hive](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-explore-data-hive-tables)
+ [Creación de características para los datos en un clúster de Hadoop mediante consultas de Hive](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-create-features-hive)
+ [Subárbol de la hoja de referencia de los usuarios SQL (PDF)](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf)

  
|Mensajes de excepción|  
|------------------------|  
|Script de Hive es incorrecto.|  
|Script de Hive {0} no es correcta.|  
  

## <a name="error-0069"></a>Error 0069  
 Se produce una excepción si el script SQL especificado no es correcto.  
  
 Si el script SQL especificado tiene problemas de sintaxis, o si las columnas o tabla especificada en la secuencia de comandos no es válido, se produce este error en Azure Machine Learning. 
 
 Recibirá este error si el motor de SQL detecta cualquier error al ejecutar la consulta o script. El mensaje de error SQL es normalmente han informado en el registro de errores para que pueda realizar una acción basada en el error específico.  
  
**Resolución:** Volver a visitar el módulo e inspeccione la consulta SQL para los errores.  
  
 Compruebe que la consulta funciona correctamente fuera de Azure ML mediante el registro directamente en el servidor de base de datos y ejecutar la consulta.  
  
 Si hay un mensaje SQL generado notificado por la excepción de módulo, tomar medidas en el error notificado. Por ejemplo, los mensajes de error a veces incluyen instrucciones específicas sobre el error probable:
+ *La base de datos que faltan o no existe columna*, que indica que es posible que haya escrito un nombre de columna incorrecto. Si está seguro de el nombre de columna es correcto, intente usar corchetes o comillas para delimitar el identificador de columna.
+ *Error SQL lógica cerca \<palabra clave SQL\>*, que indica que es posible que haya un error de sintaxis antes de la palabra clave especificada

  
|Mensajes de excepción|  
|------------------------|  
|Secuencia de comandos SQL no es correcto.|  
|Consulta SQL "{0}" no es correcto.|  
|Consulta SQL "{0}" no es correcta: {1}|  
  

## <a name="error-0070"></a>Error 0070  
 Se produce una excepción al intentar acceder a la tabla de Azure inexistente.  
  
 Este error en Azure Machine Learning se produce cuando se intenta tener acceso a una tabla de Azure inexistente. Recibirá este error si se especifica una tabla en el almacenamiento de Azure, que no existe cuando se leen o escriben en Azure Table Storage. Esto puede ocurrir si se escribe incorrectamente el nombre de la tabla deseada, o si tiene una discrepancia entre el nombre de destino y el tipo de almacenamiento. Por ejemplo, diseñado para leer de una tabla pero escribió el nombre de un blob en su lugar.  
  
**Resolución:** Volver a visitar el módulo para comprobar que el nombre de la tabla es correcto.  
  
|Mensajes de excepción|  
|------------------------|  
|Tabla de Azure no existe.|  
|Tabla de Azure "{0}" no existe.|  
  
## <a name="error-0071"></a>Error 0071  
 Se produce una excepción si proporcionan credenciales incorrectas.  
  
 Si las credenciales proporcionadas son incorrectas, se produce este error en Azure Machine Learning.  
  
 También podría recibir este error si el módulo no se puede conectar a un clúster de HDInsight.  
  
**Resolución:** Revise las entradas en el módulo y compruebe el nombre de cuenta y la contraseña.  
  
 Compruebe los siguientes problemas que pueden producir un error:  
  
-   El esquema del conjunto de datos no coincide con el esquema de la tabla de datos de destino.  
  
-   Los nombres de columna están mal escrita o que faltan  
  
-   Va a escribir en una tabla que tiene nombres de columna con los caracteres no válidos. Normalmente puede estos nombres de columna entre corchetes, pero si esto no funciona, editar los nombres de columna para usar solo letras y caracteres de subrayado (_)  
  
-   Las cadenas que está intentando escribir contengan comillas simples  
  
 Si está intentando conectarse a un clúster de HDInsight, compruebe que el clúster de destino es accesible con las credenciales proporcionadas.  
  
|Mensajes de excepción|  
|------------------------|  
|Credenciales incorrectas pasadas.|  
|Nombre de usuario incorrecto "{0}" o la contraseña se pasa|  
  

## <a name="error-0072"></a>Error 0072  
 Se produce una excepción en el caso de tiempo de espera de conexión.  
  
 Este error en Azure Machine Learning se produce cuando se agota el tiempo de espera de una conexión. Recibirá este error si hay problemas de conectividad con el origen de datos o el destino, por ejemplo, la conectividad a internet lenta, o si el conjunto de datos es grande o complicadas procesa la consulta SQL para leer los datos.  
  
**Resolución:** Determinar si hay problemas con conexiones lentas a almacenamiento de Azure o a internet.  
  
|Mensajes de excepción|  
|------------------------|  
|Se agotó el tiempo de espera de conexión.|  
  

## <a name="error-0073"></a>Error 0073  
 Se produce una excepción si se produce un error al convertir una columna a otro tipo.  
  
 Este error en Azure Machine Learning se produce cuando no es posible convertir la columna a otro tipo.  Recibirá este error si un módulo requiere un tipo concreto y no es posible convertir la columna en el nuevo tipo.  
  
**Resolución:** Modifique el conjunto de datos de entrada para que la columna se puede convertir en función de la excepción interna.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo convertir la columna.|  
|No se pudo convertir la columna a {0}.|  
  

## <a name="error-0074"></a>Error 0074  
 Se produce una excepción cuando el [editar metadatos](edit-metadata.md) intenta convertir una columna dispersa en categorías.  
  
 Este error en Azure Machine Learning se produce cuando el [editar metadatos](edit-metadata.md) intenta convertir una columna dispersa en categorías.  Recibirá este error al intentar convertir las columnas dispersas en categorías con el **hacer de categorías** opción.  Azure machine Learning no admite matrices dispersas de categorías, por lo que se producirá un error en el módulo.  
  
 <!--**Resolution:**
 Make the column dense by using [Convert to Dataset](convert-to-dataset.md) first or do not convert the column to categorical.  -->
  
|Mensajes de excepción|  
|------------------------|  
|Las columnas dispersas no se puede convertir en categorías.|  
  

## <a name="error-0075"></a>Error 0075  
Se produce una excepción cuando se usa una función de discretización no válida cuando se cuantifica un conjunto de datos.  
  
Se produce este error en Azure Machine Learning cuando está intentando discretizar datos mediante un método no admitido, o cuando las combinaciones de parámetros no son válidas.  
  
**Resolución:**

Control de errores para este evento se introdujeron en una versión anterior de Azure Machine Learning que permite más personalización de métodos de discretización. Actualmente todos los métodos de discretización se basan en una selección en una lista desplegable, técnicamente ya no debería ser posible obtener este error.

 <!--If you get this error when using the [Group Data into Bins](group-data-into-bins.md) module, consider reporting the issue in the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MachineLearning), providing the data types, parameter settings, and the exact error message.  -->
  
|Mensajes de excepción|  
|------------------------|  
|Función de discretización usada no válida.|  
  

## <a name="error-0077"></a>Error 0077  
 Se produce una excepción cuando el archivo blob desconocido escribe de modo que se pasa.  
  
 Si se pasa un argumento no válido en las especificaciones de un destino de archivo de blob o un origen, se produce este error en Azure Machine Learning.  
  
**Resolución:** En casi todos los módulos que importación o exportación datos a y desde Azure blob storage, se asignan los valores de parámetro para controlar el modo de escritura mediante el uso de una lista desplegable; por lo tanto, no es posible pasar un valor no válido y no debe aparecer este error. Este error dejará de utilizarse en una versión posterior.  
  
|Mensajes de excepción|  
|------------------------|  
|Blob no admitido, escribe el modo.|  
|Modo de escrituras de blob no admitido: {0}.|  
  

## <a name="error-0078"></a>Error 0078  
 Se produce una excepción cuando la opción HTTP para [importar datos](import-data.md) recibe un código de estado 3xx que indica redirección.  
  
 Este error en Azure Machine Learning se produce cuando la opción HTTP para [importar datos](import-data.md) recibe un 3xx (301, 302, 304, etc.) código de estado que indica redirección. Recibirá este error si intenta conectarse a un origen HTTP que redirige el explorador a otra página. Para la seguridad de motivos, redirigir los sitios Web no se permiten como orígenes de datos para Azure Machine Learning.  
  
**Resolución:** Si el sitio Web es un sitio Web de confianza, escriba la dirección URL redirigida directamente.  
  
|Mensajes de excepción|  
|------------------------|  
|Redirección HTTP no permitida|  
  

## <a name="error-0079"></a>Error 0079  
 Se produce una excepción si se especifica incorrectamente el nombre del contenedor de almacenamiento de Azure.  
  
 Si no se especifica incorrectamente el nombre del contenedor de almacenamiento de Azure, se produce este error en Azure Machine Learning. Recibirá este error si no ha especificado el contenedor y el nombre de blob (archivo) mediante **la ruta de acceso que comienza con el contenedor de blobs** opción al escribir en Azure Blob Storage.  
  
**Resolución:** Volver a visitar la [exportar datos](export-data.md) módulo y compruebe que la ruta de acceso especificada en el blob contiene el contenedor y el nombre de archivo en el formato **contenedor/filename**.  
  
|Mensajes de excepción|  
|------------------------|  
|El nombre del contenedor de almacenamiento de Azure es incorrecto.|  
|El nombre del contenedor de almacenamiento de Azure "{0}" no es correcto; se esperaba un nombre de contenedor del formato contenedor/blob.|  
  

## <a name="error-0080"></a>Error 0080  
 Se produce una excepción cuando no se permite la columna con todos los valores que faltan por el módulo.  
  
 Este error en Azure Machine Learning se produce cuando una o varias de las columnas utilizadas por el módulo contienen todos los valores que faltan. Por ejemplo, si un módulo es calcular las estadísticas agregadas para cada columna, no puede funcionar en una columna que contenga ningún dato. En tales casos, se detiene la ejecución del módulo con esta excepción.  
  
**Resolución:** Vuelva a consultar el conjunto de datos de entrada y quite las columnas que contienen todos los valores que faltan.  
  
|Mensajes de excepción|  
|------------------------|  
|No se permiten columnas con todos los valores que faltan.|  
|Columna {0} tiene todos los valores que faltan.|  
  

## <a name="error-0081"></a>Error 0081  
 Excepción se produce en el módulo PCA si el número de dimensiones para reducir es igual al número de columnas de característica en el conjunto de datos de entrada, que contiene al menos una columna de característica dispersas.  
  
 Este error en Azure Machine Learning se produce si se cumplen las condiciones siguientes: (a) el conjunto de datos de entrada tiene al menos una columna dispersa y (b) el último número de dimensiones solicitado es el mismo que el número de dimensiones de entrada.  
  
**Resolución:** Considere la posibilidad de reducir el número de dimensiones en la salida sea menor que el número de dimensiones en la entrada. Esto es habitual en aplicaciones de PCA.   <!--For more information, see [Principal Component Analysis](principal-component-analysis.md).  -->
  
|Mensajes de excepción|  
|------------------------|  
|Conjunto de datos que contiene columnas de característica dispersas número de dimensiones para reducir debe ser menor que el número de columnas de característica.|  
 

## <a name="error-0082"></a>Error 0082  
 Se produce una excepción cuando un modelo no se puede deserializar correctamente.  
  
 Este error en Azure Machine Learning se produce cuando un modelo de aprendizaje de guardado automático o transformación no se puede cargar en una versión más reciente del runtime de Azure Machine Learning como resultado un cambio importante.  
  
**Resolución:** El experimento de entrenamiento que generó el modelo o la transformación debe volver a ejecutar y el modelo o transformación se debe volver a guardar.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo deserializar el modelo porque es probable que se serializa con un formato de serialización anterior. Reciclar y volver a guardar el modelo.|  
  

## <a name="error-0083"></a>Error 0083  
 Se produce una excepción si se usa el conjunto de datos de entrenamiento no se puede usar para un tipo concreto de aprendiz.  
  
 Este error en Azure Machine Learning se produce cuando el conjunto de datos es incompatible con el aprendiz está entrenado. Por ejemplo, el conjunto de datos podría contener al menos un valor que falta en cada fila, y como resultado, todo el conjunto de datos se omitirá durante el entrenamiento. En otros casos, algunos algoritmos de aprendizaje automático, como la detección de anomalías no esperan que las etiquetas estén presentes y pueden producir esta excepción si las etiquetas están presentes en el conjunto de datos.  
  
**Resolución:** Consulte la documentación del aprendiz que se usa para comprobar los requisitos para el conjunto de datos de entrada. Examine las columnas para ver todas las columnas necesarias están presentes.  
  
|Mensajes de excepción|  
|------------------------|  
|Conjunto de datos usado para el entrenamiento no es válido.|  
|{0} contiene datos no válidos para el entrenamiento.|  
|{0} contiene datos no válidos para el entrenamiento. Tipo de aprendiz: {1}.|  
  

## <a name="error-0084"></a>Error 0084  
 Se produce una excepción cuando se evalúan las puntuaciones generadas a partir de un Script de R. Esto no se admite actualmente.  
  
 Este error en Azure Machine Learning se produce si intenta utilizar uno de los módulos para evaluar un modelo con el resultado de un script de R que contiene las puntuaciones.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Evaluar las puntuaciones generadas por R no se admite actualmente.|  
  

## <a name="error-0085"></a>Error 0085  
 Se produce una excepción cuando se produce un error en la evaluación de script con un error.  
  
 Este error en Azure Machine Learning se produce cuando se ejecuta un script personalizado que contiene errores de sintaxis.  
  
**Resolución:** Revise el código en un editor externo y compruebe si hay errores.  
  
|Mensajes de excepción|  
|------------------------|  
|Error durante la evaluación del script.|  
|Se produjo el error siguiente durante la evaluación de la secuencia de comandos, ver el registro de salida para obtener más información:---inicio del mensaje de error de {0} intérprete--- {1} ---final del mensaje de error de {0} intérprete--- ------|  
  

## <a name="error-0086"></a>Error 0086  
 Se produce una excepción cuando una transformación de recuento no es válida.  
  
 Este error en Azure Machine Learning se produce cuando se selecciona una transformación que se basa en una tabla de recuento, pero la transformación seleccionada no es compatible con los datos actuales, o con la nueva tabla de recuento.  
  
**Resolución:** El módulo admite guardar los recuentos y las reglas que componen la transformación en dos formatos diferentes. Si va a combinar las tablas de recuento, compruebe que las dos tablas que se va a combinar utilizan el mismo formato.  
  
En general, una transformación basada en recuento solo puede aplicarse a los conjuntos de datos que tienen el mismo esquema que el conjunto de datos en el que se creó originalmente la transformación.  
  
 <!-- For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). For requirements specific to creating and merging count-based features, see these topics:  
  
-   [Merge Count Transform](merge-count-transform.md)  
  
-   [Import Count Table](import-count-table.md)  
  
-   [Modify Count Table Parameters](modify-count-table-parameters.md)  
  -->
|Mensajes de excepción|  
|------------------------|  
|Transformación de recuento no válida especificada.|  
|La transformación de recuento en el puerto de entrada '{0}' no es válido.|  
|La transformación de recuento en el puerto de entrada '{0}'no se pueden combinar con la transformación de recuento en el puerto de entrada'{1}'. Compruebe los metadatos utilizados para el recuento de coincidencias.|  
  

## <a name="error-0087"></a>Error 0087  
 Se produce una excepción cuando se especifica un tipo de tabla de recuento no válido para el aprendizaje con módulos de recuentos.  
  
 Este error en Azure Machine Learning se produce cuando se intenta importar una tabla de recuento existente, pero la tabla no es compatible con los datos actuales, o con la nueva tabla de recuento.  
  
**Resolución:** Hay diferentes formatos para guardar los recuentos y las reglas que componen la transformación. Si va a combinar las tablas de recuento, compruebe que ambos usan el mismo formato.  
  
 Por lo general, una transformación basada en recuento solo puede aplicarse a conjuntos de datos que tienen el mismo esquema que el conjunto de datos en el que se creó originalmente la transformación.  
  
  <!--For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). -->
  

## <a name="error-0088"></a>Error 0088  
 Se produce una excepción cuando un recuento no válido se especifica el tipo para el aprendizaje con módulos de recuentos.  
  
 Este error en Azure Machine Learning se produce al intentar utilizar un método de recuento diferente que es compatible con características basadas en recuento.  
  
**Resolución:** En general, los métodos de recuento se eligen en una lista desplegable, por lo que no debería ver este error.  
  
  <!--For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). For requirements specific to creating and merging count-based features, see these topics:  
  
-   [Merge Count Transform](merge-count-transform.md)  
  
-   [Import Count Table](import-count-table.md)  
  
-   [Modify Count Table Parameters](modify-count-table-parameters.md)  
  -->
|Mensajes de excepción|  
|------------------------|  
|Se especifica un tipo de recuento de válido.|  
|El tipo de recuento especificado '{0}' no es un tipo de recuento válido.|  
  

## <a name="error-0089"></a>Error 0089  
 Se produce una excepción cuando el número de clases especificado es menor que el número real de clases en un conjunto de datos utilizado para el recuento.  
  
 Este error en Azure Machine Learning se produce cuando va a crear una tabla de recuento y la columna de etiqueta contiene un número diferente de las clases que se ha especificado en los parámetros del módulo.  
  
**Resolución:** Compruebe el conjunto de datos y averiguar exactamente cuántos valores distintos (las clases posibles) hay en la columna de etiqueta. Cuando se crea la tabla de recuento, debe especificar al menos este número de clases.  
  
 La tabla de recuento no puede determinar automáticamente el número de clases disponibles.  
  
 Cuando se crea la tabla de recuento, no se puede especificar 0 o todos los números es menor que el número real de clases en la columna de etiqueta.  
  
|Mensajes de excepción|  
|------------------------|  
|El número de clases es incorrecto. Asegúrese de que el número de clases que especifique en el panel de parámetros es mayor o igual que el número de clases de la columna de etiqueta.|  
|El número de clases especificado es '{0}', que no es mayor que un valor de etiqueta'{1}' en el conjunto de datos que se usa para contar. Asegúrese de que el número de clases que especifique en el panel de parámetros es mayor o igual que el número de clases de la columna de etiqueta.|  
  

## <a name="error-0090"></a>Error 0090  
 Se produce una excepción cuando se produce un error en la creación de la tabla de Hive.  
  
 Este error en Azure Machine Learning se produce cuando se usa [exportar datos](export-data.md) o no se puede crear otra opción para guardar datos en un clúster de HDInsight y la tabla de Hive especificada.  
  
**Resolución:** Compruebe el nombre de cuenta de almacenamiento de Azure asociado al clúster y que está usando la misma cuenta en las propiedades del módulo.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo crear la tabla de Hive. Para un clúster de HDInsight, asegúrese de que el nombre de cuenta de almacenamiento de Azure asociado con el clúster es igual que lo que se pasa a través del parámetro de módulo.|  
|La tabla de Hive "{0}" no se pudo crear. Para un clúster de HDInsight, asegúrese de que el nombre de cuenta de almacenamiento de Azure asociado con el clúster es igual que lo que se pasa a través del parámetro de módulo.|  
|La tabla de Hive "{0}" no se pudo crear. Para un clúster de HDInsight, asegúrese de que el nombre de cuenta de almacenamiento de Azure asociado con el clúster es "{1}".|  
 

## <a name="error-0100"></a>Error 0100  
 Se produce una excepción cuando se especifica un idioma no admitido para un módulo personalizado.  
  
 Se produce este error en Azure Machine Learning al compilar un módulo personalizado y la propiedad de nombre de la **lenguaje** elemento en un archivo de definición xml de módulo personalizado tiene un valor no válido. Actualmente, el único valor válido para esta propiedad es `R`. Por ejemplo:   
  
 `<Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />`  
  
**Resolución:** Compruebe que la propiedad name de la **lenguaje** elemento en el archivo de definición xml de módulo personalizado está establecido en `R`. Guarde el archivo, actualizar el paquete zip de módulo personalizado e intenta agregar el módulo personalizado de nuevo.  
  
|Mensajes de excepción|  
|------------------------|  
|Idioma de módulo personalizado no admitido especificado|  
  

## <a name="error-0101"></a>Error 0101  
 Todos los identificadores de puerto y los parámetros deben ser únicos.  
  
 Este error en Azure Machine Learning se produce cuando uno o varios puertos o los parámetros se asignan el mismo valor de identificador en un archivo de definición XML de módulo personalizado.  
  
**Resolución:** Compruebe que los valores de identificación en todos los puertos y los parámetros son únicos. Guarde el archivo xml, actualice el paquete zip de módulo personalizado e intenta agregar el módulo personalizado de nuevo.  
  
|Mensajes de excepción|  
|------------------------|  
|Todos los puertos y parámetros identificadores para un módulo deben ser únicos|  
|Módulo '{0}' tiene el puerto o argumento identificadores duplicado. Todos los identificadores de puerto o el argumento deben ser únicos para un módulo.|  
  

## <a name="error-0102"></a>Error 0102  
 Se produce cuando no se puede extraer un archivo ZIP.  
  
 Este error en Azure Machine Learning se produce cuando va a importar un paquete comprimido con la extensión .zip, pero el paquete no es un archivo zip o el archivo no utiliza un formato zip admitidos.  
  
**Resolución:** Asegúrese de que el archivo seleccionado es un archivo .zip válido y que se comprimió mediante uno de los algoritmos de compresión admitidos.  
  
 Si recibe este error al importar los conjuntos de datos en formato comprimido, compruebe que todos los archivos contenidos use uno de los formatos de archivo compatibles y están en formato Unicode.  <!--For more information, see [Unpack Zipped Datasets](unpack-zipped-datasets.md).  -->
  
 Intente volver a agregar los archivos deseados a una nueva carpeta zip comprimida e intente agregar de nuevo el módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|ZIP archivo no está en el formato correcto|  


## <a name="error-0103"></a>Error 0103  
 Se produce cuando un archivo ZIP no contiene los archivos .xml  
  
 Este error en Azure Machine Learning se produce cuando el paquete zip de módulo personalizado no contienen los archivos (.xml) de la definición de módulo. Estos archivos deben residir en la raíz del paquete zip (por ejemplo, no dentro de una subcarpeta).  
  
**Resolución:** Compruebe que uno o varios archivos de definición de módulo xml están en la carpeta raíz del paquete zip mediante la extracción en una carpeta temporal en la unidad de disco. Los archivos xml deben ser directamente en la carpeta en la que extrajo el paquete zip. Asegúrese de que al crear el paquete zip que no selecciona una carpeta que contiene archivos xml para el código postal como esto creará una subcarpeta dentro del paquete zip con el mismo nombre que la carpeta que seleccionó para el código postal.  
  
|Mensajes de excepción|  
|------------------------|  
|Archivo zip no contiene los archivos de definición de módulo (archivos .xml)|  


## <a name="error-0104"></a>Error 0104  
 Se produce cuando un archivo de definición de módulo hace referencia a una secuencia de comandos que no se encuentra  
  
 Este error en Azure Machine Learning se produce cuando un archivo de definición xml de módulo personalizado hace referencia a un archivo de script en el **lenguaje** elemento que no existe en el paquete zip. La ruta de acceso del archivo de script se define en el **sourceFile** propiedad de la **lenguaje** elemento. La ruta de acceso al archivo de origen es relativa a la raíz del paquete zip (misma ubicación que los archivos de definición xml de módulo). Si el archivo de script está en una subcarpeta, debe especificarse la ruta de acceso relativa al archivo de script. Por ejemplo, si todos los scripts se almacenaran en un **myscripts como** carpeta dentro del paquete zip, el **lenguaje** elemento tendría que agregar esta ruta de acceso a la **sourceFile** propiedad como a continuación. Por ejemplo:   
  
 `<Language name="R" sourceFile="myScripts/CustomAddRows.R" entryPoint="CustomAddRows" />`  
  
**Resolución:** Asegúrese de que el valor de la **sourceFile** propiedad en el **lenguaje** elementos de la definición xml de módulo personalizado son correcto y que existe el archivo de origen en la ruta de acceso relativa correcta en el paquete zip.  
  
|Mensajes de excepción|  
|------------------------|  
|Archivo de script de R que se hace referencia no existe.|  
|Archivo de script de R que se hace referencia '{0}' no se encuentra. Asegúrese de que la ruta de acceso relativa al archivo es correcta desde la ubicación de las definiciones.|  


## <a name="error-0105"></a>Error 0105  
 Este error aparece cuando un archivo de definición de módulo contiene un tipo de parámetro no admitido  
  
 Este error en Azure Machine Learning se produce al crear una definición de módulo personalizado de xml y el tipo de un parámetro o un argumento en la definición no coincide con un tipo admitido.  
  
**Resolución:** Asegúrese de que la propiedad de tipo de cualquier **Arg** elemento en el archivo de definición xml de módulo personalizado es un tipo admitido.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo de parámetro no admitido.|  
|Tipo de parámetro no admitido '{0}' especificado.|  


## <a name="error-0106"></a>Error 0106  
 Se produce cuando un archivo de definición de módulo define un tipo de entrada no admitido  
  
 Este error en Azure Machine Learning se produce cuando el tipo de un puerto de entrada en un módulo personalizado de definición XML no coincide con un tipo admitido.  
  
**Resolución:** Asegúrese de que la propiedad type de un elemento Input en el archivo de definición XML de módulo personalizado es un tipo admitido.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo de entrada no admitido.|  
|Tipo de entrada no admitido '{0}' especificado.|  


## <a name="error-0107"></a>Error 0107  
 Se produce cuando un archivo de definición de módulo define un tipo de salida no admitido  
  
 Este error en Azure Machine Learning se produce cuando el tipo de un puerto de salida en una definición de módulo personalizado xml no coincide con un tipo admitido.  
  
**Resolución:** Asegúrese de que la propiedad type de un elemento de salida en el archivo de definición xml de módulo personalizado es un tipo admitido.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo de salida no admitido.|  
|Tipo de salida no admitido '{0}' especificado.|  


## <a name="error-0108"></a>Error 0108  
 Se produce cuando un archivo de definición de módulo define más puertos de entrada o salidos que se admiten  
  
 Este error en Azure Machine Learning se produce cuando hay demasiados puertos de entrada o salidos se definen en una definición de módulo personalizado de xml.  
  
**Resolución:** Se asegura de que el número máximo de puertos de entrada y salidos definidas en la definición xml de módulo personalizado no supera el número máximo de puertos admitidos.  
  
|Mensajes de excepción|  
|------------------------|  
|Había superado el número admitido de puertos de entrada o salidos.|  
|Superado el número de admite '{0}' puertos. Número máximo permitido de '{0}'puertos es'{1}'.| 

## <a name="error-0109"></a>Error 0109  
 Se produce cuando un archivo de definición de módulo define un selector de columna incorrecto  
  
 Este error en Azure Machine Learning se produce cuando la sintaxis de un argumento de selector de columna contiene un error en una definición de xml de módulo personalizado.  
  
**Resolución:** Este error se produce cuando la sintaxis de un argumento de selector de columna contiene un error en una definición de xml de módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|Sintaxis no admitida para el selector de columna.|  
  

## <a name="error-0110"></a>Error 0110  
 Se produce cuando un archivo de definición de módulo define un selector de columna que hace referencia a un identificador de puerto de entrada inexistente  
  
 Este error en Azure Machine Learning se produce cuando el *portId* propiedad dentro del elemento de propiedades de un argumento de tipo ColumnPicker no coincide con el valor de identificador de un puerto de entrada.  
  
**Resolución:** Asegúrese de que la propiedad portId coincide con el valor de identificador de un puerto de entrada definido en la definición xml de módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|Selector de columna hace referencia a un identificador de puerto de entrada que no existe.|  
|Selector de columna hace referencia a un identificador de puerto de entrada inexistente '{0}'.|  
  

## <a name="error-0111"></a>Error 0111  
 Se produce cuando un archivo de definición de módulo define una propiedad no válida  
  
 Este error en Azure Machine Learning se produce cuando se asigna una propiedad no válida a un elemento en el módulo personalizado de definición XML.  
  
**Resolución:** Asegúrese de que la propiedad es compatible con el elemento de módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|Definición de propiedad no es válida.|  
|Definición de la propiedad '{0}' no es válido.|  
  

## <a name="error-0112"></a>Error 0112  
 Se produce cuando no se puede analizar un archivo de definición de módulo  
  
 Este error en Azure Machine Learning se produce cuando se produce un error en el formato xml que impide que el módulo personalizado de definición XML del que se va a analizar como un archivo XML válido.  
  
**Resolución:** Asegúrese de que cada elemento se abre y se cierra correctamente. Asegúrese de que no hay ningún error en el formato XML.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede analizar el archivo de definición de módulo.|  
|No se puede analizar el archivo de definición de módulo '{0}'.|  
  

## <a name="error-0113"></a>Error 0113  
 Se produce cuando un archivo de definición de módulo contiene errores.  
  
 Este error en Azure Machine Learning se produce cuando el archivo de definición XML de módulo personalizado puede analizarse pero contiene errores, tales como definición de elementos no admitidos por los módulos personalizados.  
  
**Resolución:** Asegúrese de que el archivo de definición de módulo personalizado define los elementos y propiedades que son compatibles con módulos personalizados.  
  
|Mensajes de excepción|  
|------------------------|  
|Archivo de definición de módulo contiene errores.|  
|Archivo de definición de módulo '{0}' contiene errores.|  
|Archivo de definición de módulo '{0}' contiene errores. {1}|  
  

## <a name="error-0114"></a>Error 0114  
 Se produce cuando se produce un error de creación de un módulo.  
  
 Este error en Azure Machine Learning se produce cuando se produce un error de compilación de un módulo personalizado. Esto se produce cuando uno o más errores personalizados relacionados con el módulo se producen al agregar el módulo personalizado. Se notifican los errores adicionales dentro de este mensaje de error.  
  
**Resolución:** Resuelva los errores notifican en este mensaje de excepción.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo generar el módulo personalizado.|  
|Módulo personalizado compilaciones con error con errores: {0}|  
  

## <a name="error-0115"></a>Error 0115  
 Se produce cuando una secuencia de comandos de módulo personalizado de forma predeterminada tiene una extensión no admitida.  
  
 Este error en Azure Machine Learning se produce cuando se proporciona una secuencia de comandos para un módulo personalizado que utiliza una extensión de nombre de archivo desconocido.  
  
**Resolución:** Compruebe que la extensión de archivo de formato y el nombre de los archivos de script incluidos en el módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|Extensión no admitida para la secuencia de comandos predeterminada.|  
|Extensión de archivo no admitido {0} de script predeterminado.|  
  

## <a name="error-0121"></a>Error 0121  
 Se produce cuando SQL escribe se produce un error porque es posible escribir en la tabla  
  
 Este error en Azure Machine Learning se produce cuando se usa el [exportar datos](export-data.md) módulo para guardar los resultados en una tabla en una base de datos SQL y la tabla no se puede escribir. Normalmente, verá este error si el [exportar datos](export-data.md) módulo correctamente establece una conexión con la instancia de SQL Server, pero no puede, a continuación, escribir el contenido del conjunto de datos de aprendizaje automático de Azure en la tabla.  
  
**Resolución:**
 - Abra el panel de propiedades de la [exportar datos](export-data.md) módulo y compruebe que los nombres de base de datos y tabla se escribió correctamente. 
 - Revise el esquema del conjunto de datos va a exportar y asegúrese de que los datos están compatibles con la tabla de destino.
 - Compruebe que el código SQL iniciar sesión asociado con el nombre de usuario y contraseña tiene permisos para escribir en la tabla. 
 - Si la excepción contiene información de error adicional desde SQL Server, usar esa información para realizar las correcciones.  
  
|Mensajes de excepción|  
|------------------------|  
|Conectado al servidor, no se puede escribir en la tabla.|  
|No se puede escribir en la tabla de Sql: {0}|  


## <a name="error-0122"></a>Error 0122  
 Se produce una excepción si se especifican varias columnas de peso y se permite solo una.  
  
 Este error en Azure Machine Learning se produce cuando se han seleccionado demasiadas columnas como columnas de peso.  
  
**Resolución:** Revise el conjunto de datos de entrada y sus metadatos. Asegúrese de que solo una columna contiene los pesos.  
  
|Mensajes de excepción|  
|------------------------|  
|Se especifican varias columnas de peso.|  


## <a name="error-0123"></a>Error 0123  
 Se produce una excepción si se especifica la columna de vectores a la columna de etiqueta.  
  
 Si utiliza un vector como la columna de etiqueta, se produce este error en Azure Machine Learning.  
  
**Resolución:** Cambiar el formato de datos de la columna si es necesario o elija una columna diferente.  
  
|Mensajes de excepción|  
|------------------------|  
|Columna de vectores se especificó como columna de etiqueta.|  


## <a name="error-0124"></a>Error 0124  
 Se produce una excepción si se especifican columnas no numéricas a ser la columna de peso.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Columna no numérica se especifica como la columna de peso.|  
  


## <a name="error-0125"></a>Error 0125  
 Se produce cuando no coincide con el esquema para varios conjuntos de datos.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Esquema de conjunto de datos no coincide.|  


## <a name="error-0126"></a>Error 0126  
 Se produce una excepción si el usuario especifica un dominio SQL que no se admite en Azure Machine Learning.  
  
 Este error se produce cuando el usuario especifica un dominio SQL que no se admite en Azure Machine Learning. Recibirá este error si intenta conectarse a un servidor de base de datos en un dominio que no está permitido. Actualmente, los dominios SQL permitidos son: ". database.windows.net",". cloudapp.net", o ". database.secure.windows.net". Es decir, el servidor debe ser un servidor SQL Azure o un servidor en una máquina virtual en Azure.  
  
**Resolución:** Volver a visitar el módulo. Compruebe que el servidor de base de datos SQL pertenece a uno de los dominios aceptados:  
  
-   .database.windows.net  
  
-   .cloudapp.net  
  
-   .database.secure.windows.net  
  
|Mensajes de excepción|  
|------------------------|  
|Dominio SQL no admitido.|  
|El dominio SQL {0} no se admite actualmente en Azure Machine Learning|  
  

## <a name="error-0127"></a>Error 0127  
 Tamaño de píxel de la imagen supera el límite permitido  
  
 Este error se produce si se están leyendo las imágenes de un conjunto de imágenes para la clasificación y las imágenes son más grandes que puede controlar el modelo.  
  
 <!--**Resolution:**
 For more information about the image size and other requirements, see these topics:  
  
-   [Import Images](import-images.md)  
  
-   [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md)  -->
  
|Mensajes de excepción|  
|------------------------|  
|Tamaño de píxel de la imagen supera el límite permitido.|  
|Tamaño de píxel en el archivo de imagen '{0}' supera el límite permitido: '{1}'|  


## <a name="error-0128"></a>Error 0128  
 Número de las probabilidades condicionales para las columnas de categorías supera el límite.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Número de las probabilidades condicionales para las columnas de categorías supera el límite.|  
|Número de las probabilidades condicionales para las columnas de categorías supera el límite. Las columnas de{0}'y'{1}' son el par problemático.|  


## <a name="error-0129"></a>Error 0129  
 Número de columnas del conjunto de datos supera el límite permitido.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Número de columnas del conjunto de datos supera el límite permitido.|  
|Número de columnas del conjunto de datos en '{0}"supera el permitido."|  
|Número de columnas del conjunto de datos en '{0}'supera el límite permitido de'{1}'.'|  
|Número de columnas del conjunto de datos en '{0}'supera el permitido'{1}'límite de'{2}'.'|  
## <a name="error-0130"></a>Error 0130  
 Se produce una excepción cuando todas las filas del conjunto de datos de entrenamiento contienen valores que faltan.  
  
 Esto sucede si alguna columna del conjunto de datos de entrenamiento está vacío.  
  
**Resolución:** Use la [Clean Missing Data](clean-missing-data.md) módulo para quitar las columnas con todos los valores que faltan.  
  
|Mensajes de excepción|  
|------------------------|  
|Todas las filas en el conjunto de datos de entrenamiento contienen valores que faltan.  Considere la posibilidad de usar el módulo de limpiar datos que faltan para quitar los valores que faltan.|  
 

## <a name="error-0131"></a>Error 0131  
 Se produce una excepción si se produce un error en uno o varios conjuntos de datos en un archivo zip se descomprimen y registrado correctamente  
  
 Este error se produce cuando se produce un error en uno o varios conjuntos de datos en un archivo zip se descomprimen y leer correctamente. Recibirá este error si el desempaquetado produce un error porque el propio archivo zip o uno de los archivos que está dañado o hay un error del sistema al intentar desempaquetar y expandir un archivo.  
  
**Resolución:** Utilice los detalles proporcionados en el mensaje de error para determinar cómo proceder.  
  
|Mensajes de excepción|  
|------------------------|  
|Cargar conjuntos de datos comprimido no se pudo|  
|Conjunto de datos en zip {0} error con el mensaje siguiente: {1}|  
|Conjunto de datos en zip {0} error con un {1} excepción con el mensaje: {2}|  
  

## <a name="error-0132"></a>Error 0132  
 No se especificó ningún nombre de archivo para desempaquetar; se encontraron varios archivos en el archivo zip.  
  
 Este error se produce cuando no se especificó ningún nombre de archivo para desempaquetar; se encontraron varios archivos en el archivo zip. Recibirá este error si el archivo .zip contiene más de un archivo comprimido, pero no especificó un archivo para la extracción en el **conjunto de datos para desempaquetar** cuadro de texto el **propiedad** panel del módulo. Actualmente, se puede extraer un único archivo cada vez que se ejecuta el módulo.  
  
**Resolución:** El mensaje de error proporciona una lista de los archivos que se encuentran en el archivo zip. Copie el nombre del archivo deseado y péguelo en el **conjunto de datos para desempaquetar** cuadro de texto.  
  
|Mensajes de excepción|  
|------------------------|  
|Archivo zip contiene varios archivos; debe especificar el archivo para expandir.|  
|El archivo contiene más de un archivo. Especifique el archivo para expandir. Se encontraron los siguientes archivos: {0}|  
  

## <a name="error-0133"></a>Error 0133  
 No se encontró el archivo especificado en el archivo zip  
  
 Este error se produce cuando el nombre de archivo especificado en el **conjunto de datos para desempaquetar** campo de la **propiedad** panel no coincide con el nombre de cualquier archivo que se encuentra en el archivo zip. Las causas más comunes de este error son un error al escribir o buscar el archivo de almacenamiento incorrecto para el archivo expandir.  
  
**Resolución:** Volver a visitar el módulo. Si el nombre del archivo que pretende descomprimir aparece en la lista de archivos que se encuentran, copie el nombre de archivo y péguelo en el **conjunto de datos para desempaquetar** cuadro de la propiedad. Si no ve el nombre de archivo que desee en la lista, compruebe que tiene el archivo .zip correcta y el nombre correcto para el archivo que desee.  
  
|Mensajes de excepción|  
|------------------------|  
|El archivo especificado no encuentra el archivo zip de int.|  
|No se encuentra el archivo especificado. Se encontró los siguientes archivos: {0}|  
  

## <a name="error-0134"></a>Error 0134
Se produce una excepción cuando la columna de etiqueta falta o tiene un número suficiente de filas con la etiqueta.  
  
Este error se produce cuando el módulo requiere una columna de etiqueta, pero no incluía una en la selección de columna o la columna de etiqueta falta demasiados valores.

Este error también puede producirse cuando una operación anterior cambia el conjunto de datos que existen suficientes filas para una operación de nivel inferior. Por ejemplo, suponga que utiliza una expresión en el **partición y ejemplo** módulo dividir un conjunto de datos por valores. Si se encuentra ninguna coincidencia para la expresión, uno de los conjuntos de datos resultante de la partición estaría vacía.

Resolución: 

 Si incluye una columna de etiqueta en la selección de columna, pero que no se reconoce, utilice el [editar metadatos](edit-metadata.md) módulo para marcarla como una columna de etiqueta.
  
  <!--Use the [Summarize Data](summarize-data.md) module to generate a report that shows how many values are missing in each column. -->A continuación, puede usar el [Clean Missing Data](clean-missing-data.md) módulo para quitar las filas con valores que faltan en la columna de etiqueta. 

 Compruebe los conjuntos de datos de entrada para asegurarse de que contienen datos válidos y filas suficientes para cumplir los requisitos de la operación. Muchos algoritmos generará un mensaje de error si requiere algunos mínimo número de filas de datos, pero los datos contienen solo unas pocas filas, o solo un encabezado.
  
|Mensajes de excepción|
|------------------------|
|Se produce una excepción cuando la columna de etiqueta falta o tiene un número suficiente de filas con la etiqueta.|  
|Se produce una excepción cuando la columna de etiqueta falta o tiene menos de {0} filas etiquetadas|  
  

## <a name="error-0135"></a>Error 0135  
 Basado en el centroide de clúster solo se admite.  
  
**Resolución:** Podría encontrar este mensaje de error si intenta evaluar un modelo de agrupación en clústeres que se basa en un algoritmo de agrupación en clústeres personalizado que no use centroides para inicializar el clúster.  
  
  <!--You can use [Evaluate Model](evaluate-model.md) to evaluate clustering models that are based on the  [K-Means Clustering](k-means-clustering.md) module. For custom algorithms, use the [Execute R Script](execute-r-script.md) module to create a custom evaluation script.  -->
  
|Mensajes de excepción|  
|------------------------|  
|Basado en el centroide de clúster solo se admite.|  
  

## <a name="error-0136"></a>Error 0136  
 No se devolvió ningún nombre de archivo; no se puede procesar el archivo como resultado.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|No se devolvió ningún nombre de archivo; no se puede procesar el archivo como resultado.|  
  

## <a name="error-0137"></a>Error 0137  
 SDK de Azure Storage ha encontrado un error al convertir entre las propiedades de tabla y columnas de conjunto de datos durante la lectura o escritura.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Error de conversión entre la columna de propiedad y el conjunto de datos de almacenamiento de tablas de Azure.|  
|Error de conversión entre la columna de propiedad y el conjunto de datos de almacenamiento de tablas de Azure. Información adicional: {0}|  

## <a name="error-0138"></a>Error 0138  
 Memoria se ha agotado, no se puede ejecutar completa del módulo. Reducción de tamaño del conjunto de datos puede ayudar a aliviar el problema.  
  
 Este error se produce cuando el módulo que se está ejecutando requiere más memoria que está disponible en el contenedor de Azure. Esto puede ocurrir si está trabajando con un conjunto de datos de gran tamaño y la operación actual no cabe en la memoria.  
  
**Resolución:** Si intenta leer un conjunto de datos grande y no se puede completar la operación, puede ser útil disminuir el conjunto de datos.  
  
  <!--If you use the visualizations on datasets to check the cardinality of columns, only some rows are sampled. To get a full report, use [Summarize Data](summarize-data.md). You can also use the [Apply SQL Transformation](apply-sql-transformation.md) to check for the number of unique values in each column.  
  
 Sometimes transient loads can lead to this error. Machine support also changes over time. 
  
 Try using [Principal Component Analysis](principal-component-analysis.md) or one of the provided feature selection methods to reduce your dataset to a smaller set of more feature-rich columns: [Feature Selection](feature-selection-modules.md)  -->
  
|Mensajes de excepción|  
|------------------------|  
|Memoria se ha agotado, no se puede ejecutar completa del módulo.|  
  

## <a name="error-0139"></a>Error 0139  
 Se produce una excepción cuando no es posible convertir una columna a otro tipo.  
  
 Este error en Azure Machine Learning se produce cuando intenta convertir una columna en un tipo de datos diferente, pero que no se admite el tipo de la operación actual o el módulo.  
  
 También puede aparecer el error cuando un módulo intenta convertir implícitamente los datos para cumplir los requisitos del módulo actual, pero no es posible la conversión.  
  
**Resolución:**

1. Revise los datos de entrada y determinar el tipo de datos exactos de la columna que desea usar y el tipo de datos de la columna que se está produciendo el error. En ocasiones es posible que cree el tipo de datos es correcto, pero encuentra que una operación de nivel superior se ha modificado el tipo de datos o el uso de una columna. Use la [editar metadatos](edit-metadata.md) módulo para restablecer los metadatos de columna a su estado original. 
2. Examine la página de Ayuda del módulo para comprobar los requisitos para la operación especificada. Determinar qué tipos de datos son compatibles con el módulo actual, y es compatible con el intervalo de valores. 
 <!--3. If values need to be truncated, rounded, or outliers removed, use the [Apply Math Operation](apply-math-operation.md) or [Clip Values](clip-values.md) modules to make corrections.-->
4. Tenga en cuenta si es posible convertir o convertir la columna a un tipo de datos diferentes. Los siguientes módulos de todos los proporcionan bastante flexibilidad y eficacia para modificar datos: 
 <!--
   + [Apply SQL Transformation](apply-sql-transformation.md)
   + [Execute R Script](execute-r-script.md)
-->   
   + [Ejecutar Script de Python](execute-python-script.md).  

> [!NOTE]
> ¿Sigue sin funcionar? Considere la posibilidad de proporcionar comentarios adicionales sobre el problema que nos ayudarán a desarrollar una guía la mejor solución. Solo enviar comentarios sobre esta página y proporcione el nombre del módulo que generó el error y la conversión de tipos de datos que no se pudo.
  
|Mensajes de excepción|  
|------------------------|  
|No permite la conversión.|  
|No se pudo convertir: {0}.|  
|No se pudo convertir: {0}, en la fila {1}.|  
|No se pudo convertir la columna de tipo {0} en la columna de tipo {1} en fila {2}.|  
|No se pudo convertir la columna "{2}" de tipo {0} en la columna de tipo {1} en fila {3}.|  
|No se pudo convertir la columna "{2}" de tipo {0} a la columna "{3}" de tipo {1} en fila {4}.| 

## <a name="error-0140"></a>Error 0140  
 Excepción se produce si se pasa el argumento del conjunto de columna no contiene otras columnas, excepto la columna de etiqueta.  
  
 Este error se produce si un conjunto de datos conectado a un módulo que requiere varias columnas, incluidas las características, pero ha proporcionado solo la columna de etiqueta.  
  
**Resolución:** Elija al menos una columna de característica debe incluir en el conjunto de datos.  
  
|Mensajes de excepción|  
|------------------------|  
|Conjunto de columnas especificado no contiene otras columnas, excepto la columna de etiqueta.|  
  

## <a name="error-0141"></a>Error 0141  
 Se produce una excepción si el número de las columnas numéricas seleccionadas y valores únicos en la categoría y las columnas de cadena es demasiado pequeño.  
  
 Este error en Azure Machine Learning se produce cuando no hay suficientes valores exclusivos de la columna seleccionada para realizar la operación.  
  
**Resolución:** Algunas operaciones de realizan operaciones estadísticas en columnas de categorías y de función, y si no hay suficientes valores, la operación podría producir un error o devolver un resultado no válido. Compruebe el conjunto de datos para ver cuántos valores que hay en las columnas de característica y etiqueta y determine si la operación que está intentando realizar es estadísticamente válida.  
  
 Si el conjunto de datos de origen es válido, también puede comprobar si alguna operación de manipulación o metadatos de datos de nivel superior ha cambiado los datos y eliminado algunos valores.  
  
 Si las operaciones de nivel superior incluyen la división, el muestreo o volver a muestrear, compruebe que las salidas contienen el número esperado de filas y valores.  
  
|Mensajes de excepción|  
|------------------------|  
|El número de las columnas numéricas seleccionadas y valores únicos en la categoría y las columnas de cadena es demasiado pequeño.|  
|El número total de las columnas numéricas seleccionadas y valores únicos en la categoría y las columnas de cadena (actualmente {0}) debe ser al menos {1}|  
  

## <a name="error-0142"></a>Error 0142  
 Se produce una excepción cuando el sistema no puede cargar el certificado para autenticar.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|No se puede cargar el certificado.|  
|El certificado {0} no se puede cargar. Es su huella digital {1}.|  
  

## <a name="error-0143"></a>Error 0143  
 No se puede analizar la dirección URL proporcionada por el usuario que se supone que es de GitHub.  
  
 Este error en Azure Machine Learning se produce cuando se especifica una dirección URL no válida y el módulo requiere una URL válida de GitHub.  
  
**Resolución:** Compruebe que la dirección URL hace referencia a un repositorio de GitHub válido. No se admiten otros tipos de sitio.  
  
|Mensajes de excepción|  
|------------------------|  
|Dirección URL no es de github.com.|  
|Dirección URL no es de github.com: {0}|  

## <a name="error-0144"></a>Error 0144  
 Falta el elemento esperado proporcionado por el usuario de dirección url de GitHub.  
  
 Este error en Azure Machine Learning se produce cuando se especifica un origen de archivo de GitHub con un formato de dirección URL no válido.  
  
**Resolución:** Compruebe que la dirección URL del repositorio de GitHub es válida y termina con \blob\ o \tree\\.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede analizar la dirección URL de GitHub.|  
|No se puede analizar la dirección URL de GitHub (se esperaba ' \blob\\' o ' \tree\\' después del nombre del repositorio): {0}|  

## <a name="error-0145"></a>Error 0145  
 No se puede crear el directorio de la replicación por algún motivo.  
  
 Este error en Azure Machine Learning se produce cuando se produce un error en el módulo crear el directorio especificado.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|No se puede crear el directorio de replicación.|  
  

## <a name="error-0146"></a>Error 0146  
 Cuando los archivos de usuario están sin comprimir en el directorio local, la ruta de acceso combinada puede ser demasiado largo.  
  
 Se produce este error en Azure Machine Learning cuando está extrayendo los archivos, pero algunos de los archivos son demasiado largos cuando descomprimido.  
  
**Resolución:** Edite los nombres de archivo que combinan la ruta de acceso y nombre de archivo es no tener más de 248 caracteres.  
  
|Mensajes de excepción|  
|------------------------|  
|Ruta de replicación tiene más de 248 caracteres, acorte el nombre de la secuencia de comandos o la ruta de acceso.|  

## <a name="error-0147"></a>Error 0147  
 No se pudo descargar la cosas desde GitHub por alguna razón  
  
 Este error en Azure Machine Learning se produce cuando no se puede leer o descargar los archivos especificados desde GitHub.  
  
**Resolución:** El problema podría ser temporal; es posible que intente obtener acceso a los archivos en otro momento. O bien, compruebe que tiene los permisos necesarios y que el origen es válido.  
  
|Mensajes de excepción|  
|------------------------|  
|Error de acceso a GitHub.|  
|Error de acceso a GitHub. {0}|  
  

## <a name="error-0148"></a>Error 0148  
 Problemas de acceso no autorizado al extraer datos o crear el directorio.  
  
 Se produce este error en Azure Machine Learning cuando está intentando crear un directorio o leer datos de almacenamiento, pero no tiene los permisos necesarios.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Excepción de acceso no autorizado al extraer datos.|  
  

## <a name="error-0149"></a>Error 0149  
 El archivo de usuario no existe dentro de la agrupación de GitHub.  
  
 Este error en Azure Machine Learning se produce cuando no se encuentra el archivo especificado.  
  
Resolución: 
  
|Mensajes de excepción|  
|------------------------|  
|No se encuentra el archivo de GitHub.|  
|No se encuentra el archivo de GitHub.: {0}|  
  

## <a name="error-0150"></a>Error 0150  
 Las secuencias de comandos que proceden de los paquetes de usuario no se podrían descomprimidos, probablemente debido a una colisión con archivos de GitHub.  
  
 Este error en Azure Machine Learning se produce cuando una secuencia de comandos no se puede extraer, normalmente cuando hay un archivo existente del mismo nombre.  
  
Resolución:
  
|Mensajes de excepción|  
|------------------------|  
|No se puede descomprimir el paquete; colisión de nombres posibles con los archivos de GitHub.|  
  

## <a name="error-0151"></a>Error 0151  
 Se produjo un error al escribir en el almacenamiento en nube. Compruebe la dirección URL.  
  
 Este error en Azure Machine Learning se produce cuando el módulo intenta escribir datos en el almacenamiento en la nube, pero la dirección URL no está disponible o no válido.  
  
Resolución: Compruebe la dirección URL y que es grabable.  
  
|Mensajes de excepción|  
|------------------------|  
|Error al escribir en el almacenamiento (posiblemente una dirección url incorrecta) en la nube.|  
|Error al escribir en el almacenamiento en la nube: {0}. Compruebe la dirección url.|  
  
## <a name="error-0152"></a>Error 0152  
 El tipo de la nube de Azure se especificó incorrectamente en el contexto del módulo.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo incorrecto de la nube de Azure|  
|Tipo incorrecto de la nube de Azure: {0}|  
  
## <a name="error-0153"></a>Error 0153  
 El punto final de almacenamiento especificado no es válido.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo incorrecto de la nube de Azure|  
|Punto de conexión de almacenamiento incorrecta: {0}|  

## <a name="error-0154"></a>Error 0154  
 No se pudo resolver el nombre del servidor especificado  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo resolver el nombre del servidor especificado|  
|El servidor especificado {0}. no se pudo resolver documents.azure.com|

## <a name="error-0155"></a>Error 0155  
 El cliente DocDb produjo una excepción  
  
|Mensajes de excepción|  
|------------------------|  
|El cliente DocDb produjo una excepción|  
|Cliente DocDb: {0}|

## <a name="error-0156"></a>Error 0156  
 Respuesta incorrecta para el servidor de HCatalog.  
  
|Mensajes de excepción|  
|------------------------|  
|Respuesta incorrecta para el servidor de HCatalog. Compruebe que todos los servicios se están ejecutando.|  
|Respuesta incorrecta para el servidor de HCatalog. Compruebe que todos los servicios se están ejecutando. Detalles del error: {0}|

## <a name="error-0157"></a>Error 0157  
 Se produjo un error al leer desde Azure Cosmos DB debido a los esquemas de documentos incoherentes o distintas. Lector requiere que todos los documentos que tienen el mismo esquema.  
  
|Mensajes de excepción|  
|------------------------|  
|Documentos detectados con distintos esquemas. Asegúrese de que todos los documentos tienen el mismo esquema|

## <a name="error-1000"></a>Error 1000  
Excepción interna de biblioteca.  
  
Este error se proporciona capturar en caso contrario, no controlada de errores del motor interno. Por lo tanto, la causa de este error puede variar según el módulo que ha generado el error.  
  
Para obtener más ayuda, se recomienda que publique el mensaje detallado que acompaña al error en el foro de Azure Machine Learning, junto con una descripción del escenario, incluidos los datos que se usan como entradas. Estos comentarios nos ayudarán a dar prioridad a los errores e identificar los problemas más importantes para seguir trabajando.  
  
|Mensajes de excepción|  
|------------------------|  
|Excepción de la biblioteca.|  
|Excepción en biblioteca: {0}|  
|{0} excepción en biblioteca: {1}|  
