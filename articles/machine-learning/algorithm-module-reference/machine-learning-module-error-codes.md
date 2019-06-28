---
title: Solución de errores de módulos
titleSuffix: Azure Machine Learning service
description: Solucione las excepciones de módulos en Azure Machine Learning Studio mediante códigos de error
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 09a2b616e2bba93be86241c64d37daec7d6dea3b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029766"
---
# <a name="exceptions-and-error-codes-for-algorithm--module-reference"></a>Excepciones y códigos de error para la referencia de módulos y algoritmos

Obtenga información sobre los mensajes de error y códigos de excepción que pueden surgir al usar módulos en Azure Machine Learning Studio. 

Para resolver el problema, busque el error en este artículo para conocer las causas comunes. Hay dos maneras de obtener el texto completo de un mensaje de error en Studio:  
 
- Haga clic en el vínculo **View Output Log** (Ver el registro de salida) en el panel derecho y desplácese hasta el final. El mensaje de error detallado se muestra en las dos últimas líneas de la ventana.  
  
- Seleccione el módulo que tiene el error y haga clic en la X roja. Se muestra únicamente el texto de error pertinente.  
  
Si el texto del mensaje de error no es útil, envíenos información sobre el contexto y las adiciones o cambios deseables. Puede enviar comentarios sobre el tema de error, o visitar el [foro de Azure Machine Learning Studio](https://aka.ms/aml-forum-studio) y publicar una pregunta.  


## <a name="error-0001"></a>Error 0001  
 Se produce una excepción si no se encontraron una o más columnas especificadas del conjunto de datos.  
  
 Recibirá este error si se realiza una selección de columnas para un módulo, pero las columnas seleccionadas no existen en el conjunto de datos de entrada. Este error puede producirse si escribió manualmente un nombre de columna o si el selector de columnas sugirió una columna que no existía en el conjunto de datos al realizar el experimento.  
  
**Resolución:** Vuelva a consultar el módulo que genera esta excepción y valide que los nombres de columna son correctos y que todas las columnas referenciadas existen.  
  
|Mensajes de excepción|  
|------------------------|  
|No se encontraron una o varias de las columnas especificadas.|  
|No se encontró la columna con el nombre o índice "{0}"|  
|La columna con el nombre o índice "{0}" no existe en "{1}"|  
 

## <a name="error-0002"></a>Error 0002  
 Se produce una excepción si no se pudieron analizar o convertir uno o más parámetros del tipo especificado al solicitado por el tipo de método de destino.  
  
 Este error de Azure Machine Learning se produce cuando se especifica un parámetro como entrada y el tipo de valor es distinto al esperado. En este caso, no se puede realizar una conversión implícita.  
  
**Resolución:** Compruebe los requisitos del módulo y determine qué tipo de valor se necesita (cadena, entero, doble, etcétera).  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo analizar el parámetro.|  
|No se pudo analizar el parámetro "{0}"|  
|No se pudo analizar (convertir) el parámetro "{0}" a "{1}"|  
|No se pudo convertir el parámetro "{0}" de "{1}" a "{2}"|  
|No se pudo convertir el valor "{1}" del parámetro "{0}" de "{2}" a "{3}"|  
|No se pudo convertir el valor "{0}" en la columna"{1}" de "{2}" a "{3}" mediante el formato "{4}" proporcionado.|  
  

## <a name="error-0003"></a>Error 0003  
 Se produce una excepción si una o varias de las entradas son NULL o están vacías.  
  
 Recibirá este error en Azure Machine Learning si las entradas o parámetros a un módulo son null o están vacíos.  Este error puede ocurrir, por ejemplo, cuando no escribió ningún valor para un parámetro. También puede ocurrir si elige un conjunto de datos que tiene valores faltantes, o un conjunto de datos vacío.  
  
**Resolución:**
 
+ Abra el módulo que generó la excepción y compruebe que todas las entradas se hayan especificado. Asegúrese de especificar todas las entradas necesarias. 
+ Asegúrese de que los datos que se cargan desde Azure Storage están accesibles y que no ha cambiado el nombre o clave de la cuenta.  
+ Compruebe los datos de entrada para los valores que faltan o los valores NULL.
+ Si utilizó una consulta en un origen de datos, compruebe que los datos se devuelven en el formato esperado. 
+ Busque errores tipográficos u otros cambios en la especificación de los datos.
  
|Mensajes de excepción|  
|------------------------|  
|Una o varias de las entradas son NULL o están vacías.|  
|La entrada "{0}" es NULL o está vacía.|  
  

## <a name="error-0004"></a>Error 0004  
 Se produce una excepción si el parámetro es menor o igual que el valor especificado.  
  
 Recibirá este error en Azure Machine Learning si el parámetro en el mensaje es inferior al valor de límite necesario para que el módulo procese los datos.  
  
**Resolución:** Vuelva a consultar el módulo que genera la excepción y modifique el parámetro para que sea mayor que el valor especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|El parámetro debe ser mayor que el valor de límite.|  
|El valor del parámetro "{0}" debe ser mayor que {1}.|  
|El parámetro "{0}" tiene un valor "{1}", que debería ser mayor que {2}.|  
  


## <a name="error-0005"></a>Error 0005  
 Se produce una excepción si el parámetro es menor que el valor especificado.  
  
 Recibirá este error en Azure Machine Learning si el parámetro en el mensaje es igual o inferior al valor de límite necesario para que el módulo procese los datos.  
  
**Resolución:** Vuelva a consultar el módulo que genera la excepción y modifique el parámetro para que sea mayor o igual que el valor especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|El parámetro debe ser mayor o igual que el valor de límite.|  
|El valor del parámetro "{0}" debe ser mayor que {1}.|  
|El parámetro "{0}" tiene un valor "{1}", que debería ser mayor o igual que {2}.|  
  

## <a name="error-0006"></a>Error 0006  
 Se produce una excepción si el parámetro es mayor o igual que el valor especificado.  
  
 Recibirá este error en Azure Machine Learning si el parámetro en el mensaje es mayor o igual que el valor de límite necesario para que el módulo procese los datos.  
  
**Resolución:** Vuelva a consultar el módulo que genera la excepción y modifique el parámetro para que sea menor que el valor especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|Error de coincidencia de parámetros. Uno de los parámetros debe ser menor que el otro.|  
|El valor del parámetro "{0}" debe ser menor que el valor del parámetro "{1}".|  
|El parámetro "{0}" tiene un valor "{1}", que debería ser menor que {2}.|  
  

## <a name="error-0007"></a>Error 0007  
 Se produce una excepción si el parámetro es mayor que el valor especificado.  
  
 Recibirá este error en Azure Machine Learning si, en las propiedades del módulo, se especificó un valor que es mayor que lo permitido. Por ejemplo, podría especificar un dato que está fuera del intervalo de fechas admitidas, o podría indicar que se utilicen cinco columnas cuando solo están disponibles tres. 
 
 También puede ver este error si especifica dos conjuntos de datos que deben coincidir de alguna manera. Por ejemplo, si se está cambiando el nombre de las columnas y especifica las columnas por índice, el número de nombres que proporcione debe coincidir con el número de índices de columna. Otro ejemplo podría ser una operación matemática que utiliza dos columnas, donde las columnas deben tener el mismo número de filas. 
  
**Resolución:**
 
 + Abra el módulo en cuestión y revise cualquier configuración de propiedades numéricas.
 + Asegúrese de que los valores de parámetro se encuentran dentro del intervalo de valores admitido para esa propiedad.
 + Si el módulo acepta varias entradas, asegúrese de que las entradas son del mismo tamaño.
<!-- + If the module has multiple properties that can be set, ensure that related properties have appropriate values. For example, when using [Group Data into Bins](group-data-into-bins.md), if you use the option to specify custom bin edges, the number of bins must match the number of values you provide as bin boundaries.-->
 + Compruebe si el conjunto de datos o el origen de datos cambiaron. A veces, un valor que funcionaba con una versión anterior de los datos producirá un error después de que cambió el número de columnas, los tipos de datos de la columna o el tamaño de los datos.  
  
|Mensajes de excepción|  
|------------------------|  
|Error de coincidencia de parámetros. Uno de los parámetros debe ser menor o igual que el otro.|  
|El valor del parámetro "{0}" debe ser menor o igual que el valor del parámetro "{1}".|  
|El parámetro "{0}" tiene un valor "{1}", que debería ser menor o igual que {2}.|  
  

## <a name="error-0008"></a>Error 0008  
 Se produce una excepción si el parámetro no se encuentra en el intervalo.  
  
 Recibirá este error en Azure Machine Learning si el parámetro en el mensaje está fuera de los límites necesarios para que el módulo procese los datos.  
  
 Por ejemplo, se muestra este error si intenta usar [Add Rows](add-rows.md) para combinar dos conjuntos de datos que tienen un número de columnas diferente.  
  
**Resolución:** Vuelva a consultar el módulo que genera la excepción y modifique el parámetro para que se encuentre dentro del intervalo especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|El valor del parámetro no se encuentra en el intervalo especificado.|  
|El valor del parámetro "{0}" no está en el intervalo.|  
|El valor del parámetro "{0}" debe encontrarse en el intervalo de [{1}, {2}].|  
  

## <a name="error-0009"></a>Error 0009  
 Se produce una excepción cuando se especifica incorrectamente el nombre del contenedor o el nombre de la cuenta de Azure Storage.  
  
Este error de Azure Machine Learning Studio se produce cuando especifica parámetros para una cuenta de almacenamiento de Azure, pero no se puede resolver el nombre o la contraseña. Los errores en las contraseñas o nombres de cuenta pueden ocurrir por diversos motivos:
 
 + La cuenta es de un tipo incorrecto. No se admite el uso de algunos nuevos tipos de cuenta para Machine Learning Studio. Consulte [Import Data](import-data.md) (importar datos) para obtener más información.
 + Escribió el nombre de cuenta incorrecto.
 + La cuenta ya no existe.
 + La contraseña de la cuenta de almacenamiento es incorrecta o ha cambiado.
 + No ha especificado el nombre del contenedor, o el contenedor no existe.
 + No especificó la ruta de acceso de archivo completa (ruta de acceso al blob)
   
**Resolución:**

Estos problemas suelen producirse al intentar escribir manualmente la ruta de acceso del contenedor, la contraseña o el nombre de cuenta. Se recomienda que utilice el nuevo asistente para el módulo [Import Data](import-data.md) (importar datos), que le ayuda a buscar y comprobar los nombres.

Compruebe también si se ha eliminado la cuenta, el contenedor o el blob. Utilice otra utilidad de almacenamiento de Azure para comprobar que el nombre de cuenta y la contraseña se escribieron correctamente y que existe el contenedor. 

Azure Machine Learning no admite algunos nuevos tipos de cuenta. Por ejemplo, los nuevos tipos de almacenamiento "en frío" o "de acceso frecuente" no se pueden usar para el aprendizaje automático. Las cuentas de almacenamiento clásicas y las creadas como "de uso general" funcionan correctamente.

Si se especificó la ruta de acceso completa a un blob, compruebe que la ruta de acceso se especificó con el formato **contenedor/nombredeblob**, y que tanto el contenedor como el blob existen en la cuenta.  
  
 La ruta de acceso no debe contener una barra inicial. Por ejemplo **/contenedor/blob** no es correcto y debe especificarse como **contenedor/blob**.  

  
|Mensajes de excepción|  
|------------------------|  
|El nombre de la cuenta de almacenamiento de Azure o el nombre del contenedor es incorrecto.|  
|El nombre de la cuenta de almacenamiento de Azure "{0}" o el nombre de contenedor "{1}" no es correcto; se esperaba un nombre de contenedor con el formato contenedor/blob.|  
  

## <a name="error-0010"></a>Error 0010  
 Se produce una excepción si los conjuntos de datos de entrada tienen nombres de columna que deben coincidir, pero no coinciden.  
  
 Recibirá este error en Azure Machine Learning si el índice de columna en el mensaje tiene nombres de columna diferentes en los dos conjuntos de datos de entrada.  
  
**Resolución:** Utilice [Edit Metadata](edit-metadata.md) (editar metadatos) o modifique el conjunto de datos original para que tenga el mismo nombre de columna que el índice de columna especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|Las columnas con índice correspondiente en los conjuntos de datos de entrada tienen nombres diferentes.|  
|Los nombres de columna no son los mismos para la columna {0} (de base cero) en los conjuntos de datos de entrada ({1} y {2}, respectivamente).|  
  

## <a name="error-0011"></a>Error 0011  
 Se produce una excepción si el argumento que se pasó al conjunto de columnas no se aplica a alguna de las columnas del conjunto de datos.  
  
 Recibirá este error en Azure Machine Learning si la selección de columna especificada no coincide con ninguna de las columnas del conjunto de datos determinado.  
  
 También es posible que vea este error si no ha seleccionado una columna y se requiere una como mínimo para que el módulo funcione.  
  
**Resolución:** Modifique la selección de columnas en el módulo para que se aplique a las columnas del conjunto de datos.  
  
 Si el módulo requiere que seleccione una columna específica, como una columna de etiqueta, compruebe que está seleccionada la columna correcta.  
  
 Si se seleccionan las columnas incorrectas, quítelas y vuelva a ejecutar el experimento.  
  
|Mensajes de excepción|  
|------------------------|  
|El conjunto de columnas especificado no se aplica a ninguna de las columnas del conjunto de datos.|  
|El conjunto de columnas "{0}" especificado no se aplica a ninguna de las columnas del conjunto de datos.|  
  

## <a name="error-0012"></a>Error 0012  
 Se produce una excepción si no se pudo crear la instancia de clase con el conjunto de argumentos que se pasó.  
  
**Resolución:** Este error no lo ocasiona el usuario y quedará en desuso en una versión futura.  
  
|Mensajes de excepción|  
|------------------------|  
|El modelo no está entrenado, entrene primero el modelo.|  
|El modelo ({0}) no está entrenado, utilice un modelo entrenado.|  
  

## <a name="error-0013"></a>Error 0013  
 Se produce una excepción si el aprendiz que se pasa al módulo es de un tipo no válido.  
  
 Este error se produce siempre que un modelo entrenado no es compatible con el módulo de puntuación conectado. <!--For example, connecting the output of [Train Matchbox Recommender](train-matchbox-recommender.md) to [Score Model](score-model.md) (instead of [Score Matchbox Recommender](score-matchbox-recommender.md)) will generate this error when the experiment is run.  -->
  
**Resolución:**

Determine el tipo de aprendiz que genera el módulo de entrenamiento y determine cuál módulo de puntuación es adecuado para el aprendiz. 

Si el modelo se entrenó con cualquiera de los módulos de entrenamiento especializados, conecte el modelo entrenado únicamente con el módulo de puntuación especializado correspondiente. 


|Tipo de modelo|Módulo de entrenamiento| Módulo de puntuación|
|----|----|----|
|cualquier clasificador|[Train Model](train-model.md) (entrenar modelo) |[Score Model](score-model.md) (puntuar modelo)|
|cualquier modelo de regresión|[Train Model](train-model.md) (entrenar modelo) |[Score Model](score-model.md) (puntuar modelo)|
<!--| modelos de agrupación en clústeres| [Train Clustering Model](train-clustering-model.md) (entrenar un modelo de agrupación en clústeres) o [Sweep Clustering](sweep-clustering.md) (limpiar una agrupación en clústeres)| [Assign Data to Clusters](assign-data-to-clusters.md) (asignar datos a los clústeres)|
| detección de anomalías - SVM de una clase | [Train Anomaly Detection Model](train-anomaly-detection-model.md) (entrenar un modelo de detección de anomalías) |[Score Model](score-model.md) (puntuar modelo)|
| detección de anomalías - PCA |[Train Model](train-model.md) (entrenar modelo) |[Score Model](score-model.md) (puntuar modelo) </br> Se requieren algunos pasos adicionales para evaluar el modelo. |
| detección de anomalías - serie temporal|  [Detección de anomalías en una series temporal](time-series-anomaly-detection.md) |El modelo se entrena a partir de los datos y genera las puntuaciones. El módulo no crear un aprendiz entrenado y no se requiere ninguna puntuación adicional. |
| modelo de recomendación| [Train Matchbox Recommender](train-matchbox-recommender.md) (entrenar un recomendador de Matchbox) | [Score Matchbox Recommender](score-matchbox-recommender.md) (puntuar un recomendador de Matchbox) |
| clasificación de imágenes | [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md) (clasificación de imágenes en cascada previamente entrenada) | [Score Model](score-model.md) (puntuar modelo) |
|Modelos de VowPal Wabbit| [Train Vowpal Wabbit Version 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) (entrenar un modelo de Vowpal Wabbit versión 7-4) | [Score Vowpal Wabbit Version 7-4 Model](score-vowpal-wabbit-version-7-4-model.md) (puntuar un modelo de Vowpal Wabbit versión 7-4) |   
|Modelos de VowPal Wabbit| [Train Vowpal Wabbit Version 7-10 Model](train-vowpal-wabbit-version-7-10-model.md) (entrenar un modelo de Vowpal Wabbit versión 7-10) | [Score Vowpal Wabbit Version 7-10 Model](score-vowpal-wabbit-version-7-10-model.md) (puntuar un modelo de Vowpal Wabbit versión 7-10) |
|Modelos de VowPal Wabbit| [Train Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) (entrenar un modelo de Vowpal Wabbit versión 8) | [Score Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) (puntuar un modelo de Vowpal Wabbit versión 8) |-->
  
|Mensajes de excepción|  
|------------------------|  
|Se pasó un aprendiz de tipo no válido.|  
|El aprendiz "{0}" tiene un tipo no válido.|  


## <a name="error-0014"></a>Error 0014  
 Se produce una excepción si el recuento de valores únicos de columna es mayor que lo permitido.  
  
 Este error se produce cuando una columna contiene demasiados valores únicos.  Por ejemplo, podría ver este error si especifica que una columna se trate como datos de categoría, pero hay demasiados valores únicos en la columna como para permitir que se complete el procesamiento. También puede ver este error si hay una discrepancia entre el número de valores únicos de dos entradas.   
  
**Resolución:**

Abra el módulo que generó el error e identifique las columnas que se usan como entradas. En el caso de algunos módulos, puede hacer clic en la entrada del conjunto de datos y seleccionar **Visualizar** para obtener las estadísticas de columnas individuales, incluido el número de valores únicos y su distribución.

En el caso de las columnas que va a utilizar para agrupación o clasificación, tome medidas para reducir el número de valores únicos de las columnas. Puede reducirlo de maneras diferentes, según el tipo de datos de la columna. 
<!--
+ For text data, you might be able to use [Preprocess Text](preprocess-text.md) to collapse similar entries. 
+ For numeric data, you can create a smaller number of bins using [Group Data into Bins](group-data-into-bins.md), remove or truncate values using [Clip Values](clip-values.md), or use machine learning methods such as [Principal Component Analysis](principal-component-analysis.md) or [Learning with Counts](data-transformation-learning-with-counts.md) to reduce the dimensionality of the data.  
-->
> [!TIP]
> ¿No logra encontrar una solución que coincida con su escenario? Puede enviar comentarios sobre este tema incluyendo el nombre del módulo que genera el error y el tipo de datos y cardinalidad de la columna. Utilizaremos la información para proporcionarle pasos más enfocados en solucionar problemas de escenarios comunes.   
  
|Mensajes de excepción|  
|------------------------|  
|El número de valores únicos de columna es mayor que lo permitido.|  
|El número de valores únicos en la columna: "{0}" supera el recuento de tuplas de {1}.|  
  

## <a name="error-0015"></a>Error 0015  
 Se produce una excepción si hay un error de conexión de base de datos.  
  
 Recibirá este error si escribe un nombre, una contraseña, un servidor de bases de datos o un nombre de base de datos de cuenta SQL incorrectos, o bien si no se puede establecer una conexión con la base de datos debido a problemas con la base de datos o el servidor.  
  
**Resolución:** Compruebe que el nombre de cuenta, la contraseña, el servidor de base de datos y la base de datos se escribieron correctamente y que la cuenta especificada tiene el nivel correcto de permisos. Compruebe que la base de datos es accesible en este momento.  
  
|Mensajes de excepción|  
|------------------------|  
|Error al realizar la conexión de base de datos.|  
|Error al realizar la conexión de base de datos: {0}.|  
  


## <a name="error-0016"></a>Error 0016  
 Se produce una excepción si los conjuntos de datos de entrada que se pasan al módulo deben tener tipos de columna compatibles, pero no es así.  
  
 Recibirá este error en Azure Machine Learning si los tipos de las columnas que se pasaron en dos o más conjuntos de datos no son compatibles entre sí.  
  
**Resolución:** Utilice [Edit Metadata](edit-metadata.md) (editar metadatos) o modifique el conjunto de datos de entrada original.<!--, or use [Convert to Dataset](convert-to-dataset.md)--> para asegurarse de que los tipos de las columnas son compatibles.  
  
|Mensajes de excepción|  
|------------------------|  
|Las columnas con índice correspondiente en los conjuntos de datos de entrada tienen tipos incompatibles.|  
|Las columnas {0} y {1} son incompatibles.|  
|Los tipos de elemento de columna no son compatibles para la columna {0} (de base cero) en los conjuntos de datos de entrada ({1} y {2}, respectivamente).|  
  

## <a name="error-0017"></a>Error 0017  
 Se produce una excepción si una columna seleccionada usa un tipo de datos que no es compatible con el módulo actual.  
  
 Por ejemplo, podría recibir este error en Azure Machine Learning si la selección de columna incluye una columna con un tipo de datos que el módulo no puede procesar, como una columna de cadena para una operación matemática o una columna de puntuación donde se requiere una columna de característica de categoría.  
  
**Resolución:**
 1. Identifique a la columna problemática.
 2. Revise los requisitos del módulo.
 3. Modifique la columna para que cumpla con los requisitos. Es posible que deba utilizar varios de los siguientes módulos para realizar cambios, dependiendo de la columna y la conversión que intenta realizar:
    + Utilice [Edit Metadata](edit-metadata.md) (editar metadatos) para cambiar el tipo de datos de las columnas, o para cambiar el uso de las columnas de característica a numérica, de categoría a no categórica y así sucesivamente.
<!--    + Use [Convert to Dataset](convert-to-dataset.md) to ensure that all included columns use data types that are supported by Azure Machine Learning.  If you cannot convert the columns, consider removing them from the input dataset.
    + Use the [Apply SQL Transformation](apply-sql-transformation.md) or [Execute R Script](execute-r-script.md) modules to cast or convert any columns that cannot be modified using [Edit Metadata](edit-metadata.md). These modules provide more flexibility for working with datetime data types.
    + For numeric data types, you can use the [Apply Math Operation](apply-math-operation.md) module to round or truncate values, or use the [Clip Values](clip-values.md) module to remove out of range values.  -->
 4. Como último recurso, es posible que necesite modificar el conjunto de datos de entrada original.

> [!TIP]
> ¿No logra encontrar una solución que coincida con su escenario? Puede enviar comentarios sobre este tema incluyendo el nombre del módulo que genera el error y el tipo de datos y cardinalidad de la columna. Utilizaremos la información para proporcionarle pasos más enfocados en solucionar problemas de escenarios comunes. 
  
|Mensajes de excepción|  
|------------------------|  
|No se puede procesar la columna del tipo actual. El tipo no es compatible con el módulo.|  
|No se puede procesar la columna del tipo {0}. El tipo no es compatible con el módulo.|  
|No se puede procesar la columna "{1}" del tipo {0}. El tipo no es compatible con el módulo.|  
|No se puede procesar la columna "{1}" del tipo {0}. El tipo no es compatible con el módulo. Nombre de parámetro: {2}|  
  

## <a name="error-0018"></a>Error 0018  
 Se produce una excepción si el conjunto de datos de entrada no es válido.  
  
**Resolución:** Este error en Azure Machine Learning puede aparecer en muchos contextos, por lo que no existe una solución única. En general, el error indica que los datos proporcionados como entrada para un módulo tienen un número incorrecto de columnas, o que el tipo de datos no coincide con los requisitos del módulo. Por ejemplo:  
  
-   El módulo requiere una columna de etiqueta, pero no hay ninguna columna marcada como etiqueta, o aún no selecciona una columna de etiqueta.  
  
-   El módulo requiere que los datos sean de categoría, pero son numéricos.  
  
<!---   The module requires a specific data type. For example, ratings provided to [Train Matchbox Recommender](train-matchbox-recommender.md) can be either numeric or categorical, but cannot be floating point numbers.  -->
  
-   Los datos tienen un formato incorrecto.  
  
-   Los datos importados contienen caracteres no válidos, valores erróneos, o valores fuera del intervalo.  
-   La columna está vacía o faltan demasiados valores.  
  
 Para determinar los requisitos y cómo se usarán los datos, revise el tema de ayuda del módulo que consumirá el conjunto de datos como entrada.  
  
 <!--We also recommend that you use [Summarize Data](summarize-data.md) or [Compute Elementary Statistics](compute-elementary-statistics.md) to profile your data, and use these modules to fix metadata and clean values: [Edit Metadata](edit-metadata.md) and [Clean Missing Data](clean-missing-data.md), [Clip Values](clip-values.md)-->.  
  
|Mensajes de excepción|  
|------------------------|  
|El conjunto de datos no es válido.|  
|{0} contiene datos no válidos.|  
|{0} y {1} deben tener columnas coherentes.|  
  

## <a name="error-0019"></a>Error 0019  
 Se produce una excepción si se espera que la columna contenga valores ordenados, pero es así.  
  
 Recibirá este error en Azure Machine Learning si los valores de la columna especificada están desordenados.  
  
**Resolución:** Ordene los valores de columna al modificar manualmente el conjunto de datos de entrada y volver a ejecutar el módulo.  
  
|Mensajes de excepción|  
|------------------------|  
|Los valores en la columna no están ordenados.|  
|Los valores en la columna "{0}" no están ordenados.|  
|Los valores en la columna "{0}" del conjunto de datos "{1}" no están ordenados.|  
  

## <a name="error-0020"></a>Error 0020  
 Se produce una excepción si en algunos de los conjuntos de datos que se pasan al módulo el número de columnas es demasiado pequeño.  
  
 Recibirá este error en Azure Machine Learning, si no se han seleccionado suficientes columnas para un módulo.  
  
**Resolución:** Vuelva a consultar el módulo y asegúrese de que el selector de columnas tiene el número correcto de columnas seleccionado.  
  
|Mensajes de excepción|  
|------------------------|  
|El número de columnas en el conjunto de datos de entrada es menor que el mínimo permitido.|  
|El número de columnas en el conjunto de datos de entrada es menor que el mínimo permitido de {0} columna(s).|  
|El número de columnas en el conjunto de datos de entrada "{0}" es menor que el mínimo permitido de {1} columna(s).|

## <a name="error-0021"></a>Error 0021  
 Se produce una excepción si el número de filas en algunos de los conjuntos de datos que se pasan al módulo es demasiado pequeño.  
  
 Este error aparece en Azure Machine Learning cuando en el conjunto de datos no hay suficientes filas para realizar la operación especificada. Por ejemplo, es posible que vea este error si el conjunto de datos de entrada está vacío, o si está intentando realizar una operación que requiere que un número mínimo de filas sea válido. Estas operaciones pueden incluir, entre otras, agrupaciones o clasificaciones basadas en métodos estadísticos, ciertos tipos de cuantificación y aprendizaje con recuentos.  
  
**Resolución:**
 
 + Abra el módulo que devolvió el error y compruebe las propiedades del conjunto de datos de entrada y el módulo. 
 + Compruebe que el conjunto de datos de entrada no está vacío y que no hay suficientes filas de datos para cumplir los requisitos descritos en la ayuda del módulo.  
 + Si los datos se cargan desde un origen externo, asegúrese de que el origen de datos está disponible y que no hay ningún error o cambio en la definición de datos que haría que el proceso de importación obtenga menos filas.
 + Si está realizando una operación en los datos ascendentes del módulo que pueden afectar al tipo de datos o el número de valores, como las operaciones de limpieza, división o unión, compruebe los resultados de esas operaciones para determinar el número de filas devueltas.  



## <a name="error-0022"></a>Error 0022  
 Se produce una excepción si el número de columnas seleccionadas en el conjunto de datos de entrada no es igual al número esperado.  
  
 Este error en Azure Machine Learning puede producirse cuando el módulo descendente o la operación requieren un número específico de entradas o columnas, y cuando proporcionó demasiados o muy pocas columnas o entradas. Por ejemplo:  
  
-   Especificó una columna de una sola etiqueta o una columna de clave y seleccionó accidentalmente varias columnas.  
  
-   Está cambiando el nombre de columnas, pero proporcionó un número de nombres mayor o menor que el número de columnas.  
  
-   El número de columnas en el origen o destino cambió o no coincide con el número de columnas utilizado por el módulo.  
  
-   Proporcionó una lista de valores separada por comas para las entradas, pero el número de valores no coincide o no se admiten varias entradas.  
  
**Resolución:** Vuelva a consultar el módulo y compruebe la selección de columna para asegurarse de que está seleccionado el número correcto de columnas. Compruebe las salidas de los módulos ascendentes y los requisitos de las operaciones descendentes.  
  
 Si utilizó una de las opciones de selección de columnas que puede seleccionar varias columnas (índices de columnas, todas las de características, todas las numéricas, etc.), valide el número exacto de las columnas devueltas por la selección.  
  
 <!--If you are trying to specify a comma-separated list of datasets as inputs to [Unpack Zipped Datasets](unpack-zipped-datasets.md), unpack only one dataset at a time. Multiple inputs are not supported.  -->
  
 Compruebe que el número o tipo de las columnas ascendentes no haya cambiado.  
  
 Si utiliza un conjunto de datos de recomendación para entrenar un modelo, recuerde que el recomendador espera un número limitado de columnas, correspondiente a los pares de usuario y elemento o las clasificaciones de usuario y elemento. Quite las columnas adicionales antes de entrenar el modelo o dividir los conjuntos de datos de recomendación. Para obtener más información, consulte [Split Data](split-data.md) (dividir datos).  
  
|Mensajes de excepción|  
|------------------------|  
|El número de columnas seleccionadas en el conjunto de datos de entrada no es igual al número esperado.|  
|El número de columnas seleccionadas en el conjunto de datos de entrada no es igual a {0}.|  
|El patrón de selección de columnas "{0}" proporcionó un número de columnas seleccionadas en el conjunto de datos de entrada no es igual a {1}.|  
|Se espera que el patrón de selección de columnas "{0}" proporcione la(s) {1} columna(s) seleccionada(s) en el conjunto de datos de entrada, pero se proporcionaron {2} columna(s).|  



## <a name="error-0023"></a>Error 0023  
 Se produce una excepción si la columna de destino del conjunto de datos de entrada no es válida para el módulo de aprendiz actual.  
  
 Este error de Azure Machine Learning se produce si la columna de destino (como se seleccionó en los parámetros del módulo) no es del tipo de datos válido, contenía todos los valores que faltan o no era de categoría como se esperaba.  
  
**Resolución:** Vuelva a consultar la entrada del módulo para inspeccionar el contenido de la columna de etiqueta o destino. Asegúrese de que no contenga todos los valores que faltan. Si el módulo está esperando que la columna de destino sea de categoría, asegúrese de que hay más de un valor distinto en la columna de destino.  
  
|Mensajes de excepción|  
|------------------------|  
|El conjunto de datos de entrada tiene una columna de destino no compatible.|  
|El conjunto de datos de entrada tiene una columna de destino no compatible "{0}".|  
|El conjunto de datos de entrada tiene una columna de destino no compatible "{0}" para el aprendiz de tipo {1}.|  
 

## <a name="error-0024"></a>Error 0024  
Se produce una excepción si el conjunto de datos no contiene una columna de etiqueta.  

 Este error de Azure Machine Learning se produce cuando el módulo requiere una columna de etiqueta y el conjunto de datos no tiene una columna de etiqueta. Por ejemplo, la evaluación de un conjunto de datos con puntuación normalmente requiere la presencia de una columna de etiqueta para calcular las métricas de precisión.  
 
También puede ocurrir que una columna de etiqueta esté presente en el conjunto de datos, pero Azure Machine Learning no la detecte correctamente.
  
**Resolución:**

+ Abra el módulo que generó el error y determine si hay una columna de etiqueta presente. El tipo de datos o el nombre de la columna no importan, siempre y cuando la columna contenga un único resultado (o variable dependiente) que se está intentando predecir. Si no está seguro de qué columna tiene la etiqueta, busque un nombre genérico, como *Class* o *Target*. 
+  Si el conjunto de datos no incluye una columna de etiqueta, es posible que la columna de etiqueta se haya quitado de forma ascendente explícitamente o por accidente. También podría ser que el conjunto de datos no sea el resultado de un módulo de puntuación ascendente.
+ Para marcar explícitamente la columna como columna de etiqueta, agregue el módulo [Edit Metadata](edit-metadata.md) (editar metadatos) y conecte el conjunto de datos. Seleccione solo la columna de etiqueta y seleccione **Etiqueta** desde la lista desplegable **Campos**. 
+ Si se equivoca al elegir una columna como etiqueta, puede seleccionar **Borrar etiqueta** desde **Campos** para corregir los metadatos en la columna. 
  
|Mensajes de excepción|  
|------------------------|  
|No hay ninguna columna de etiqueta en el conjunto de datos.|  
|No hay ninguna columna de etiqueta en "{0}".|  
  

## <a name="error-0025"></a>Error 0025  
 Se produce una excepción si el conjunto de datos no contiene una columna de puntuación.  
  
 Este error de Azure Machine Learning se produce si la entrada para el modelo de evaluación no contiene columnas de puntuación válidas. Por ejemplo, el usuario intentó evaluar un conjunto de datos antes de que se puntuara con un modelo entrenado correcto, o la columna de puntuación se quitó explícitamente de forma ascendente. Esta excepción también se produce si las columnas de puntuación en los dos conjuntos son incompatibles. Por ejemplo, es posible que esté intentando comparar la precisión de un regresor lineal con el de un clasificador binario.  
  
**Resolución:** Vuelva a consultar la entrada para el modelo de evaluación y compruebe si contiene una o varias columnas de puntuación. Si no es así, el conjunto de datos no se puntuó o las columnas de puntuación se quitaron en un módulo de nivel superior.  
  
|Mensajes de excepción|  
|------------------------|  
|No hay ninguna columna de puntuación en el conjunto de datos.|  
|No hay ninguna columna de puntuación en "{0}".|  
|No hay ninguna columna de puntuación en "{0}" producida por un "{1}". Puntúe el conjunto de datos usando el tipo de aprendiz correcto.|  
  

## <a name="error-0026"></a>Error 0026  
 Se produce una excepción si no se permiten columnas con el mismo nombre.  
  
 Este error de Azure Machine Learning se produce si varias columnas tienen el mismo nombre. Puede recibir este error si el conjunto de datos no tiene una fila de encabezado y los nombres de columna se asignan automáticamente: Col0, Col1, etc.  
  
**Resolución:** Si las columnas tienen el mismo nombre, inserte un módulo [Edit Metadata](edit-metadata.md) (editar metadatos) entre el conjunto de datos de entrada y el módulo. Utilice el selector de columnas en [Edit Metadata](edit-metadata.md) (editar metadatos) para seleccionar las columnas a las que cambiará el nombre y escriba los nombres nuevos en el cuadro de texto **New column names** (nuevos nombres de columna).  
  
|Mensajes de excepción|  
|------------------------|  
|Se especificaron nombres de columna iguales en los argumentos. El módulo no permite los nombres de columna iguales.|  
|No se permiten nombres de columna iguales en los argumentos "{0}" y "{1}". Especifique nombres diferentes.|  
  

## <a name="error-0027"></a>Error 0027  
 Se produce una excepción cuando dos objetos tienen que ser del mismo tamaño, pero no es así.  
  
 Este es un error común en Azure Machine Learning y puede deberse a muchos motivos.  
  
**Resolución:** No hay ninguna resolución específica. Sin embargo, puede comprobar la existencia de condiciones como las siguientes:  
  
-   Si está cambiando el nombre de las columnas, asegúrese de que cada lista (las columnas de entrada y la lista de nombres nuevos) tiene el mismo número de elementos.  
  
-   Si va a unir o concatenar dos conjuntos de datos, asegúrese de que tienen el mismo esquema.  
  
-   Si va a unir dos conjuntos de datos que tienen varias columnas, asegúrese de que las columnas de clave tienen los mismos tipos de datos y seleccione la opción **Allow duplicates and preserve column order in selection** (permitir duplicados y conservar el orden de las columnas en la selección).  
  
|Mensajes de excepción|  
|------------------------|  
|El tamaño de los objetos que se pasaron es incoherente.|  
|El tamaño de "{0}" no es coherente con el tamaño de "{1}".|  
  

## <a name="error-0028"></a>Error 0028  
 La excepción se produce cuando el conjunto de columnas contiene nombres de columna duplicados y no está permitido.  
  
 Este error de Azure Machine Learning se produce cuando los nombres de columna están duplicados; es decir, no son únicos.  
  
**Resolución:** Si hay columnas con el mismo nombre, agregue una instancia de [Edit Metadata](edit-metadata.md) (editar metadatos) entre el conjunto de datos de entrada y el módulo que genera el error. Utilice el selector de columnas en [Edit Metadata](edit-metadata.md) (editar metadatos) para seleccionar las columnas a las que cambiará el nombre y escriba los nuevos nombres de columna en el cuadro de texto **New column names** (nuevos nombres de columna). Si está cambiando el nombre de varias columnas, asegúrese de que los valores que escriba en **New column names** (nuevos nombres de columna) son únicos.  
  
|Mensajes de excepción|  
|------------------------|  
|El conjunto de columnas contiene nombres de columna duplicados.|  
|El nombre "{0}" está duplicado.|  
|El nombre "{0}" está duplicado en "{1}".|  
  

## <a name="error-0029"></a>Error 0029  
 Se produce una excepción cuando se pasa un URI no válido.  
  
 Este error en Azure Machine Learning se produce cuando se pasa un URI no válido.  Recibirá este error si se cumple alguna de las condiciones siguientes:  
  
-   El URI público o de SAS proporcionado a Azure Blob Storage para lectura o escritura contiene un error.  
  
-   Se agotó el período de tiempo para SAS.  
  
-   La dirección URL web a través del origen HTTP representa un archivo o un URI de bucle invertido.  
  
-   La dirección URL web a través de HTTP contiene una dirección URL con formato incorrecto.  
  
-   El origen remoto no puede resolver la dirección URL.  
  
**Resolución:** Vuelva a consultar el módulo y compruebe el formato del URI. Si el origen de datos es una dirección URL web a través de HTTP, compruebe que el origen deseado no sea un archivo o un URI (localhost) de bucle invertido.  
  
|Mensajes de excepción|  
|------------------------|  
|Se pasa un URI no válido.|  
  

## <a name="error-0030"></a>Error 0030  
 Se produce una excepción cuando no es posible descargar un archivo.  
  
 Esta excepción en Azure Machine Learning se produce cuando no es posible descargar un archivo. Recibirá esta excepción cuando un intento de lectura desde un origen HTTP devuelva un error después de tres (3) reintentos.  
  
**Resolución:** Compruebe que el URI para el origen HTTP esté correcto y que actualmente se pueda acceder al sitio a través de Internet.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede descargar un archivo.|  
|Error al descargar el archivo: {0}.|  
  

## <a name="error-0031"></a>Error 0031  
 Se produce una excepción si el conjunto de columnas incluye un número de columnas menor que el necesario.  
  
 Este error en Azure Machine Learning se produce si el número de columnas seleccionado es menor que el necesario.  Recibirá este error si no se selecciona el número mínimo requerido de columnas.  
  
**Resolución:** Agregue columnas adicionales a la selección de columnas mediante el **selector de columnas**.  
  
|Mensajes de excepción|  
|------------------------|  
|El número de columnas del conjunto de columnas es menor que el requerido.|  
|Es necesario especificar {0} columna(s). El número real de columnas especificadas es {1}.|  

## <a name="error-0032"></a>Error 0032  
 Se produce una excepción si el argumento no es un número.  
  
 Si el argumento es un valor doble o NaN, recibirá este error en Azure Machine Learning.  
  
**Resolución:** Modifique el argumento especificado para que utilice un valor válido.  
  
|Mensajes de excepción|  
|------------------------|  
|El argumento no es un número.|  
|"{0}" no es un número.|  
  

## <a name="error-0033"></a>Error 0033  
 Se produce una excepción si el argumento es infinito.  
  
 Este error de Azure Machine Learning se produce si el argumento es infinito. Recibirá este error si el argumento es `double.NegativeInfinity` o `double.PositiveInfinity`.  
  
**Resolución:** Modifique el argumento especificado para que utilice un valor válido.  
  
|Mensajes de excepción|  
|------------------------|  
|El argumento deben ser finito.|  
|"{0}" no es finito.|  
  

## <a name="error-0034"></a>Error 0034  
 Se produce una excepción si existe más de una clasificación para un par usuario-elemento determinado.  
  
 Este error de Azure Machine Learning se produce como recomendación si un par usuario-elemento tiene más de un valor de clasificación.  
  
**Resolución:** Asegúrese de que el par usuario-elemento posee solo un valor de clasificación.  
  
|Mensajes de excepción|  
|------------------------|  
|Existe más de una clasificación para los valores del conjunto de datos.|  
|Hay más de una clasificación para el usuario {0} y elemento {1} en la tabla de datos de predicción de clasificación.|  
  

## <a name="error-0035"></a>Error 0035  
 Se produce una excepción si no se proporcionaron características para un usuario o elemento determinado.  
  
 Este error de Azure Machine Learning se produce cuando está intentando usar un modelo de recomendación para la puntuación, pero no se encuentra un vector de característica.  
  
**Resolución:**

El recomendador de Matchbox tiene ciertos requisitos que deben cuando se usan las características de elemento o de usuario.  Este error indica que falta un vector de característica para un usuario o un elemento que proporcionó como entrada.  Debe asegurarse de que un vector de características está disponible en los datos para cada usuario o elemento.  
  
 Por ejemplo, si entrenó un modelo de recomendación con características como la edad del usuario, su ubicación o ingresos, pero ahora desea crear puntuaciones para nuevos usuarios que no estuvieron presentes durante el entrenamiento, debe proporcionar un conjunto equivalente de características (es decir, los valores de edad, ubicación e ingresos) para los nuevos usuarios con el fin de realizar predicciones adecuadas para ellos. 
 
 Si no tiene ninguna característica para estos usuarios, considere la posibilidad de ingeniería de características para generar características adecuadas.  Por ejemplo, si no tiene los valores de edad o ingresos de un usuario individual, puede generar valores aproximados para un grupo de usuarios. 
 
<!--When you are scoring from a recommendation mode, you can use item or user features only if you previously used item or user features during training. For more information, see [Score Matchbox Recommender](score-matchbox-recommender.md).
 
For general information about how the Matchbox recommendation algorithm works, and how to prepare a dataset of item features or user features, see [Train Matchbox Recommender](train-matchbox-recommender.md).  -->
  
 > [!TIP]
 > ¿La resolución no se aplica a su caso? Puede enviar comentarios sobre este artículo y proporcionar información sobre el escenario, incluido el módulo y el número de filas en la columna. Usaremos esta información para proporcionarle más pasos para solucionar problemas en el futuro.
   
|Mensajes de excepción|  
|------------------------|  
|No se proporcionaron características para un usuario o elemento obligatorios.|  
|Las características para {0} son obligatorias pero no se han proporcionado.|  
  

## <a name="error-0036"></a>Error 0036  
 Se produce una excepción si se proporcionaron varios vectores de características para un usuario o elemento determinado.  
  
 Este error de Azure Machine Learning se produce si un vector de característica se ha definido más de una vez.  
  
**Resolución:** Asegúrese de que el vector de características no está definido más de una vez.  
  
|Mensajes de excepción|  
|------------------------|  
|Hay una definición de característica duplicada para un usuario o elemento.|  
|Hay una definición de característica duplicada para {0}.|  
  

## <a name="error-0037"></a>Error 0037  
 Se produce una excepción si se especifican varias columnas de etiqueta y solo se permite una.  
  
 Este error de Azure Machine Learning se produce si se selecciona más de una columna para que sea la nueva columna de etiqueta. La mayoría de los algoritmos de aprendizaje supervisado requieren que una sola columna se marque como destino o etiqueta.  
  
**Resolución:** Asegúrese de seleccionar una sola columna como la nueva columna de etiqueta.  
  
|Mensajes de excepción|  
|------------------------|  
|Se especificaron varias columnas de etiqueta.|  
  

## <a name="error-0038"></a>Error 0038  
 Se produce una excepción si se espera que el número de elementos sea un valor exacto, pero no lo es.  
  
 Este error de Azure Machine Learning se produce si se espera que el número de elementos sea un valor exacto, pero no lo es.  Recibirá este error si el número de elementos no es igual que el valor esperado válido.  
  
**Resolución:** Modifique la entrada para que tenga el número correcto de elementos.  
  
|Mensajes de excepción|  
|------------------------|  
|El número de elementos no es válido.|  
|El número de elementos de "{0}" no es válido.|  
|El número de elementos de "{0}" no es igual al número válido de elementos de {1}.|  
  

## <a name="error-0039"></a>Error 0039  
 Se produce una excepción si una operación devuelve error.  
  
 Este error de Azure Machine Learning se produce cuando no se puede completar una operación interna.  
  
**Resolución:** Este error se produce por muchos motivos y no hay ninguna solución específica.  
 La tabla siguiente contiene los mensajes genéricos para este error, que van seguidos de una descripción específica del motivo. 
 
 Si no hay detalles disponibles, [envíenos sus comentarios](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) y proporcione información acerca de los módulos que generaron el error y las condiciones relacionadas.
  
|Mensajes de excepción|  
|------------------------|  
|Error en la operación|  
|Error al completar la operación: {0}.|  
  

## <a name="error-0040"></a>Error 0040  
 Se produce una excepción al llamar a un módulo en desuso.  
  
 Este error de Azure Machine Learning se produce cuando se llama a un módulo en desuso.  
  
**Resolución:** Reemplace el módulo en desuso por uno compatible. Consulte el registro de salida del módulo para obtener información sobre qué módulo va a usar en su lugar.  
  
|Mensajes de excepción|  
|------------------------|  
|Acceso a un módulo en desuso.|  
|El módulo "{0}" está en desuso. Utilice el módulo "{1}" en su lugar.|  
 

## <a name="error-0041"></a>Error 0041  
 Se produce una excepción al llamar a un módulo en desuso.  
  
 Este error de Azure Machine Learning se produce cuando se llama a un módulo en desuso.  
  
**Resolución:** Reemplace el módulo en desuso por un conjunto de módulos compatibles. Esta información debe estar visible en el registro de salida del módulo.  
  
|Mensajes de excepción|  
|------------------------|  
|Acceso a un módulo en desuso.|  
|El módulo "{0}" está en desuso. Utilice los módulos "{1}" para obtener la funcionalidad solicitada.|  
 

## <a name="error-0042"></a>Error 0042  
 Se produce una excepción cuando no es posible convertir la columna a otro tipo.  
  
 Este error de Azure Machine Learning se produce cuando no es posible convertir la columna al tipo especificado.  Recibirá este error si un módulo requiere un tipo de datos determinado (como fecha y hora, texto, un número de punto flotante o entero), pero no es posible convertir una columna existente al tipo solicitado.  
 
Por ejemplo, podría seleccionar una columna e intentar convertirla a un tipo de datos numéricos para usarla en una operación matemática y recibiría este error si la columna contiene datos no válidos. 

Otro motivo por el que puede encontrarse con este error es si intenta utilizar como columna de categoría a una columna con números de punto flotante o con muchos valores únicos. 
  
**Resolución:**

+ Abra la página de ayuda del módulo que generó el error y compruebe los requisitos del tipo de datos.
+ Revise los tipos de datos de las columnas en el conjunto de datos de entrada.
+ Examine los datos que se originen en lo que se conoce como orígenes de datos sin esquema.
+ Compruebe el conjunto de datos en busca de valores faltantes o de caracteres especiales que podrían bloquear la conversión al tipo de datos deseado. 
    + Los tipos de datos numéricos deben ser coherentes: por ejemplo, busque números de punto flotante en una columna de enteros.
    + Busque valores de cadena de texto o NA en una columna numérica. 
    + Los valores booleanos se pueden convertir en una representación adecuada según el tipo de datos necesarios.
    + Examine las columnas de texto en busca de caracteres que no sean Unicode, caracteres de tabulación o caracteres de control.
    + Los datos de fecha y hora deben ser coherentes para evitar errores de modelado, pero el proceso de limpieza puede ser complicado debido al gran número de formatos. Considere la posibilidad de utilizar <!--the [Execute R Script](execute-r-script.md) or -->los módulos [Execute Python Script](execute-python-script.md) (ejecutar script de Python) para realizar la limpieza.  
+ Si es necesario, modifique los valores del conjunto de datos de entrada para que la columna pueda convertirse correctamente. Entre las modificaciones pueden incluirse la cuantificación, las operaciones de truncamiento o redondeo, la eliminación de valores atípicos o la imputación de valores faltantes. Consulte los siguientes artículos para conocer algunos escenarios comunes de transformación de datos en aprendizaje automático:
    + [Clean Missing Data](clean-missing-data.md) (limpiar datos faltantes)
    + [Normalize Data](normalize-data.md) (normalizar datos)
<!--+ [Clip Values](clip-values.md) 
    + [Group Data Into Bins](group-data-into-bins.md)
  -->
 
> [!TIP]
> ¿La resolución no está clara o no se aplica a su caso? Puede enviar comentarios sobre este artículo y proporcionar información sobre el escenario, incluido el módulo y el tipo de datos de la columna. Usaremos esta información para proporcionarle más pasos para solucionar problemas en el futuro.  
  
|Mensajes de excepción|  
|------------------------|  
|Conversión no permitida.|  
|No se pudo convertir la columna del tipo {0} a la columna de tipo {1}.|  
|No se pudo convertir la columna "{2}" del tipo {0} a la columna de tipo {1}.|  
|No se pudo convertir la columna "{2}" del tipo {0} a la columna "{3}" de tipo {1}.|  
  

## <a name="error-0043"></a>Error 0043  
 Se produce una excepción cuando el tipo de elemento no implementa Equals de manera explícita.  
  
 Este error de Azure Machine Learning no se usa y quedará en desuso.  
  
**Resolución:** Ninguno.  
  
|Mensajes de excepción|  
|------------------------|  
|No se encontró ningún método Equals explícito accesible.|  
|No se pueden comparar los valores de la columna \\"{0}\\" de tipo {1}. No se encontró ningún método Equals explícito accesible.|  


## <a name="error-0044"></a>Error 0044  
 Se produce una excepción cuando no es posible derivar el tipo de elemento de columna a partir de los valores existentes.  
  
 Este error de Azure Machine Learning se produce cuando no es posible deducir el tipo de una columna o columnas en un conjunto de datos. Esto suele suceder al concatenar dos o más conjuntos de datos con diferentes tipos de elemento. Si Azure Machine Learning no puede determinar un tipo común que pueda representar a todos los valores de una o varias columnas sin perder información, se generará este error.  
  
**Resolución:** Asegúrese de que todos los valores de una columna determinada en los dos conjuntos de datos que va a combinar sean del mismo tipo (numérico, booleano, categoría, cadena, fecha, etc.) o que puedan convertirse en el mismo tipo.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede derivar el tipo de elemento de la columna.|  
|No se puede derivar el tipo de elemento de la columna "{0}", todos los elementos son referencias nulas.|  
|No se puede derivar el tipo de elemento de la columna "{0}" en el conjunto de datos "{1}", todos los elementos son referencias nulas.|  
  

## <a name="error-0045"></a>Error 0045  
 Se produce una excepción cuando no es posible crear una columna debido a tipos de elementos mixtos en el origen.  
  
 Este error de Azure Machine Learning se produce cuando los tipos de elemento de dos conjuntos de datos que van a combinarse son diferentes.  
  
**Resolución:** Asegúrese de que todos los valores de una columna determinada en los dos conjuntos de datos que va a combinar sean del mismo tipo (numérico, booleano, categoría, cadena, fecha, etc.).  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede crear una columna con tipos de elemento mixtos.|  
|No se puede crear la columna con ID "{0}" de los tipos de elemento mixtos:\n\tEl tipo de datos[{1}, {0}] es {2}\n\tEl tipo de datos[{3}, {0}] es {4}.|  
  

## <a name="error-0046"></a>Error 0046  
 Se produce una excepción cuando no es posible crear el directorio en la ruta de acceso especificada.  
  
 Este error de Azure Machine Learning se produce cuando no es posible crear un directorio en la ruta de acceso especificada. Recibirá este error si algún elemento de la ruta de acceso al directorio de salida de una consulta de Hive es incorrecto o inaccesible.  
  
**Resolución:** Vuelva a consultar el módulo y compruebe que la ruta de acceso del directorio tenga el formato correcto y que sea accesible con las credenciales actuales.  
  
|Mensajes de excepción|  
|------------------------|  
|Especifique un directorio de salida válido.|  
|El directorio: {0} no se puede crear. Especifique una ruta de acceso válida.|  
  

## <a name="error-0047"></a>Error 0047  
 Se produce una excepción si en algunos de los conjuntos de datos que se pasan al módulo el número de columnas de características es demasiado pequeño.  
  
 Este error de Azure Machine Learning se produce si el conjunto de datos de entrada para el entrenamiento no contiene el número mínimo de columnas necesarias para el algoritmo. Normalmente el conjunto de datos está vacío o solo contiene columnas de entrenamiento.  
  
**Resolución:** Vuelva a consultar el conjunto de datos de entrada con el fin de asegurarse de que tenga una o varias columnas adicionales a la columna de etiqueta.  
  
|Mensajes de excepción|  
|------------------------|  
|El número de columnas de características en el conjunto de datos de entrada es menor que el mínimo permitido.|  
|El número de columnas de características en el conjunto de datos de entrada es menor que el mínimo permitido de {0} columna(s).|  
|El número de columnas de características en el conjunto de datos de entrada "{0}" es menor que el mínimo permitido de {1} columna(s).|  
  

## <a name="error-0048"></a>Error 0048  
 Se produce una excepción cuando no es posible abrir un archivo.  
  
 Este error de Azure Machine Learning se produce cuando no es posible abrir un archivo para lectura o escritura. Este error puede aparecer por los siguientes motivos:  
  
-   No existe el contenedor o el archivo (blob).  
  
-   El nivel de acceso del archivo o el contenedor no le permite acceder al archivo.  
  
-   El archivo es demasiado grande para su lectura o tiene el formato incorrecto.  
  
**Resolución:** Vuelva a consultar el módulo y el archivo que está intentando leer.  
  
 Compruebe que los nombres del contenedor y del archivo son correctos.  
  
 Utilice el portal de Azure clásico o una herramienta de almacenamiento de Azure para comprobar que tiene permiso para acceder al archivo.  
  
  <!--If you are trying to read an image file, make sure that it meets the requirements for image files in terms of size, number of pixels, and so forth. For more information, see [Import Images](import-images.md).  -->
  
|Mensajes de excepción|  
|------------------------|  
|No se puede abrir un archivo.|  
|Error al abrir el archivo: {0}.|  


## <a name="error-0049"></a>Error 0049  
 Se produce una excepción cuando no es posible analizar un archivo.  
  
 Este error de Azure Machine Learning se produce cuando no es posible analizar un archivo. Recibirá este error si el formato de archivo seleccionado en el módulo [Import Data](import-data.md) (importar datos) no coincide con el formato real del archivo, o si el archivo contiene un carácter no reconocible.  
  
**Resolución:** Vuelva a consultar el módulo y corrija la selección de formato de archivo si no coincide con el formato del archivo. Si es posible, inspeccione el archivo para confirmar que no contiene ningún carácter no válido.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede analizar un archivo.|  
|Error al analizar el archivo: {0}.|  
  

## <a name="error-0050"></a>Error 0050  
 Se produce una excepción cuando los archivos de entrada y salida son los mismos.  
  
**Resolución:** Este error de Azure Machine Learning no se usa y quedará en desuso.  
  
|Mensajes de excepción|  
|------------------------|  
|Los archivos especificados para la entrada y salida no pueden ser el mismo.|


## <a name="error-0051"></a>Error 0051  
 Se produce una excepción cuando varios archivos de salida son el mismo.  
  
**Resolución:** Este error de Azure Machine Learning no se usa y quedará en desuso.  
  
|Mensajes de excepción|  
|------------------------|  
|Los archivos especificados para las salidas no pueden ser el mismo.|


## <a name="error-0052"></a>Error 0052  
 Se produce una excepción si se especifica incorrectamente la clave de cuenta de almacenamiento de Azure.  
  
 Este error de Azure Machine Learning se produce si la clave utilizada acceder a la cuenta de almacenamiento de Azure es incorrecta. Por ejemplo, es posible que vea este error si la clave de almacenamiento de Azure se ha truncado al copiarla y pegarla, o si se usó una clave incorrecta.  
  
 Para obtener más información acerca de cómo obtener la clave de una cuenta de almacenamiento de Azure, consulte [Visualización, copia y regeneración de claves de acceso de almacenamiento](https://azure.microsoft.com/documentation/articles/storage-create-storage-account-classic-portal/).  
  
**Resolución:** Vuelva a consultar el módulo y compruebe que la clave de almacenamiento de Azure es correcta para la cuenta; copie la clave de nuevo desde el portal clásico de Azure si es necesario.  
  
|Mensajes de excepción|  
|------------------------|  
|La clave de cuenta de almacenamiento de Azure es incorrecta.|  
  

## <a name="error-0053"></a>Error 0053  
 Se produce una excepción cuando no hay características o elementos de usuario para las recomendaciones de Matchbox.  
  
 Este error de Azure Machine Learning se produce cuando no se puede encontrar un vector de característica.  
  
**Resolución:** Asegúrese de que un vector de característica se encuentra en el conjunto de datos de entrada.  
  
|Mensajes de excepción|  
|------------------------|  
|Se requieren características y/o elementos de usuario, pero no se proporcionaron.|  

## <a name="error-0054"></a>Error 0054  
 Se produce una excepción si no hay suficientes valores distintivos en la columna para completar la operación.  
  
**Resolución:** Este error de Azure Machine Learning no se usa y quedará en desuso.  
  
|Mensajes de excepción|  
|------------------------|  
|Los datos tienen valores distintivos insuficientes en la columna especificada para completar la operación.|  
|Los datos tienen valores distintivos insuficientes en la columna especificada para completar la operación. El mínimo requerido es {0} elementos.|  
|Los datos tienen valores distintivos insuficientes en la columna "{1}" para completar la operación. El mínimo requerido es {0} elementos.|  
  

## <a name="error-0055"></a>Error 0055  
 Se produce una excepción al llamar a un módulo en desuso.  Este error de Azure Machine Learning se produce si intenta llamar a un módulo que está en desuso.
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Acceso a un módulo en desuso.|  
|El módulo "{0}" está en desuso.|  

## <a name="error-0056"></a>Error 0056  
 Se produce una excepción si las columnas seleccionadas para una operación infringen los requisitos.  
  
 Este error de Azure Machine Learning se produce cuando se seleccionan las columnas para una operación que requiere que la columna sea de un tipo de datos determinado. 
 
 Este error también puede ocurrir si la columna tiene el tipo de datos correcto, pero el módulo que está utilizando requiere que la columna también esté marcada como una columna de característica, etiqueta o categorías.  
  
  <!--For example, the [Convert to Indicator Values](convert-to-indicator-values.md) module requires that columns be categorical, and will raise this error if you select a feature column or label column.  -->
  
**Resolución:**
  
1.  Revise el tipo de datos de las columnas seleccionadas actualmente. 

2. Determine si las columnas seleccionadas son de categorías, etiqueta o características.  
  
3.  Revise el tema de ayuda para el módulo en el que realizó la selección de columnas para determinar si hay requisitos específicos sobre el tipo de datos o el uso de columnas.  
  
3.  Utilice [Edit Metadata](edit-metadata.md) (editar metadatos) para cambiar el tipo de columna durante la duración de esta operación. No olvide cambiar el tipo de columna a su valor original con otra instancia de [Edit Metadata](edit-metadata.md) (editar metadatos) si la necesita para operaciones descendentes.  
  
|Mensajes de excepción|  
|------------------------|  
|Una o varias de las columnas seleccionadas no estaban en una categoría permitida.|  
|La columna con nombre "{0}" no tiene una categoría permitida.|  
  

## <a name="error-0057"></a>Error 0057  
 Se produce una excepción al intentar crear un archivo o blob que ya existe.  
  
 Esta excepción se produce cuando se usa el módulo [Export Data](export-data.md) (exportar datos) u otro módulo para guardar los resultados de un experimento de Azure Machine Learning en Azure Blob Storage, pero se intenta crear un archivo o blob que ya existe.   
  
**Resolución:**
 
 Recibirá este error solo si estableció previamente la propiedad **modo de lectura de Azure Blob Storage** en **Error**. De manera deliberada, este módulo genera un error si intenta escribir un conjunto de datos en un blob que ya existe.
 
 - Abra las propiedades del módulo y cambie la propiedad **Modo de escritura de Azure Blob Storage** a **Overwrite**.
 - También puede escribir el nombre de un blob o archivo de destino diferente. Asegúrese de especificar un blob que aún no exista.  
  
|Mensajes de excepción|  
|------------------------|  
|El archivo o blob ya existe.|  
|El archivo o blob "{0}" ya existe.|  
  

## <a name="error-0058"></a>Error 0058  
 Este error de Azure Machine Learning se produce si el conjunto de datos no contiene la columna de etiqueta esperada.  
  
 Esta excepción también puede producirse cuando la columna de etiqueta proporcionada no coincide con los datos o tipo de datos que espera el aprendiz, o bien si tiene valores incorrectos. Por ejemplo, esta excepción se produce cuando se usa una columna de etiqueta con valores reales al entrenar un clasificador binario.  
  
**Resolución:** La resolución depende el aprendiz o entrenador que está usando y de los tipos de datos de las columnas en el conjunto de datos. En primer lugar, compruebe los requisitos del algoritmo de aprendizaje automático o del módulo de entrenamiento.  
  
 Vuelva a consultar el conjunto de datos de entrada. Compruebe que la columna que espera se trate como etiqueta tenga el tipo de datos correcto para el modelo que va a crear.  
  
 Compruebe las entradas en busca de valores faltantes y elimínelos o reemplácelos si es necesario.  
  
 Si es necesario, agregue el módulo [Edit Metadata](edit-metadata.md) (editar metadatos) y asegúrese de que la columna de etiqueta está marcada como una etiqueta.  
  
|Mensajes de excepción|  
|------------------------|  
|La columna de etiqueta no es la esperada.|  
|La columna de etiqueta no es la esperada en "{0}".|  
|La columna de etiqueta "{0}" no es la esperada en "{1}".|  
  

## <a name="error-0059"></a>Error 0059  
 Se produce una excepción si no se puede analizar el índice de columna especificado en un selector de columna.  
  
 Este error de Azure Machine Learning se produce si no se puede analizar un índice de columna especificado cuando se usa el selector de columnas.  Recibirá este error cuando el índice de columna esté en un formato no válido que no se puede analizar.  
  
**Resolución:** Modifique el índice de columna para usar un valor de índice válido.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pueden analizar uno o varios de los índices de columna o intervalos de índice especificados.|  
|No se pudo analizar el índice o intervalo de columnas "{0}".|  
  

## <a name="error-0060"></a>Error 0060  
 Se produce una excepción cuando se especifica un intervalo de columnas fuera del intervalo en un selector de columnas.  
  
 Este error de Azure Machine Learning se produce cuando se especifica un intervalo de columna fuera del intervalo en el selector de columnas. Recibirá este error si el intervalo de columnas en el selector de columnas no corresponde a las columnas del conjunto de datos.  
  
**Resolución:** Modifique el intervalo de columnas en el selector de columnas a fin de que corresponda con las columnas del conjunto de datos.  
  
|Mensajes de excepción|  
|------------------------|  
|Se especificó un intervalo de índices de columna no válido o fuera del intervalo.|  
|El intervalo de columnas "{0}" no es válido o está fuera del intervalo.|  
  

## <a name="error-0061"></a>Error 0061  
 Se produce una excepción al intentar agregar una fila a un objeto DataTable que tiene un número de columnas distinto al de la tabla.  
  
 Este error de Azure Machine Learning se produce cuando se intenta agregar una fila a un conjunto de datos con un número diferente de columnas.  Recibirá este error si la fila que se va a agregar al conjunto de datos tiene un número de columnas distinto al del conjunto de datos de entrada.  La fila no se puede anexar al conjunto de datos si el número de columnas es diferente.  
  
**Resolución:** Modifique el conjunto de datos de entrada para que tenga el mismo número de columnas que la fila agregada, o bien modifique la fila agregada para que tenga el mismo número de columnas que el conjunto de datos.  
  
|Mensajes de excepción|  
|------------------------|  
|Todas las tablas deben tener el mismo número de columnas.|  
  

## <a name="error-0062"></a>Error 0062  
 Se produce una excepción cuando se intenta comparar dos modelos con tipos diferentes de aprendiz.  
  
 Este error de Azure Machine Learning se produce cuando no se pueden comparar las métricas de evaluación de dos conjuntos de datos con puntuación diferentes. En este caso, no es posible comparar la eficacia de los modelos usados para generar los dos conjuntos de datos con puntuación.  
  
**Resolución:** Compruebe que los resultados puntuados los produce el mismo tipo de modelo de aprendizaje automático (clasificación binaria, regresión, clasificación de varias clases, recomendación, agrupación en clústeres, detección de anomalías, entre otros). Todos los modelos que se comparan deben tener el mismo tipo de aprendiz.  
  
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
|Error al evaluar el script de R.|  
|Se produjo el siguiente error durante la evaluación del script de R: ---------- Inicio del mensaje de error de R ---------- {0} ----------- Fin del mensaje de error de R -----------|  
|Durante la evaluación del script de R "{1}" se produjo el siguiente error: ---------- Inicio del mensaje de error de R ---------- {0} ----------- Fin del mensaje de error de R -----------|  
  


## <a name="error-0064"></a>Error 0064  
 Se produce una excepción si se especifica incorrectamente el nombre o clave de almacenamiento de una cuenta de almacenamiento de Azure.  
  
 Este error de Azure Machine Learning se produce si el nombre o la clave de almacenamiento de la cuenta de almacenamiento de Azure se especifica incorrectamente. Recibirá este error si escribe un nombre o contraseña de cuenta incorrecta para la cuenta de almacenamiento. Esto puede ocurrir si especifica manualmente el nombre o contraseña de la cuenta. También puede producirse si la cuenta se eliminó.  
  
**Resolución:** Compruebe que el nombre de cuenta y la contraseña se escribieron correctamente y que la cuenta existe.  
  
|Mensajes de excepción|  
|------------------------|  
|El nombre o la clave de almacenamiento de la cuenta de almacenamiento de Azure son incorrectos.|  
|El nombre "{0}" o la clave de almacenamiento de la cuenta de almacenamiento de Azure son incorrectos.|  
  

## <a name="error-0065"></a>Error 0065  
 Se produce una excepción si se especifica incorrectamente el nombre del blob de Azure.  
  
 Este error de Azure Machine Learning se produce si el nombre de blob se especifica incorrectamente.  Recibirá el error si:  
  
-   El blob no se encuentra en el contenedor especificado.  
  
 <!---   The fully qualified name of the blob specified for output in one of the [Learning with Counts](data-transformation-learning-with-counts.md) modules is greater than 512 characters.  -->
  
-   Solo se especificó el contenedor como el origen en una solicitud de [Import Data](import-data.md) (importar datos) cuando el formato era Excel o CSV con codificación; con estos formatos, no se permite la concatenación del contenido de todos los blobs dentro de un contenedor.  
  
-   Un URI de SAS no contiene el nombre de un blob válido.  
  
**Resolución:** Vuelva a consultar el módulo que genera la excepción. Compruebe que el blob especificado existe en el contenedor de la cuenta de almacenamiento y que los permisos le permiten ver el blob. Compruebe que la entrada tiene el formato **nombredecontenedor/nombredearchivo** si tiene archivos Excel o CSV con formatos de codificación. Compruebe que un URI de SAS contiene el nombre de un blob válido.  
  
|Mensajes de excepción|  
|------------------------|  
|La instancia de Azure Storage Blob es incorrecta.|  
|El nombre de instancia de Azure Storage Blob "{0}" es incorrecto.|  
  

## <a name="error-0066"></a>Error 0066  
 Se produce una excepción si no se pudo cargar un recurso en un blob de Azure.  
  
 Este error de Azure Machine Learning se produce si no se pudo cargar un recurso en un blob de Azure.  <!--You will receive this message if [Train Vowpal Wabbit 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) encounters an error attempting to save either the model or the hash created when training the model.--> Ambos se guardan en la misma cuenta de almacenamiento de Azure que contiene el archivo de entrada.  
  
**Resolución:** Vuelva a consultar el módulo. Compruebe que el nombre, la clave de almacenamiento y el contenedor de la cuenta de Azure son correctos y que la cuenta tiene permiso para escribir en el contenedor.  
  
|Mensajes de excepción|  
|------------------------|  
|El recurso no se pudo cargar al almacenamiento de Azure.|  
|El archivo "{0}" no se pudo cargar al almacenamiento de Azure como {1}.|  
  

## <a name="error-0067"></a>Error 0067  
 Se produce una excepción si un conjunto de datos tiene un número de columnas distinto al esperado.  
  
 Este error de Azure Machine Learning se produce si un conjunto de datos tiene un número de columnas distinto al esperado.  Recibirá este error cuando el número de columnas del conjunto de datos sea diferente del número de columnas que el módulo espera durante la ejecución.  
  
**Resolución:** Modifique el conjunto de datos de entrada o los parámetros.  
  
|Mensajes de excepción|  
|------------------------|  
|Hay un número inesperado de columnas en DataTable.|  
|Se esperaban "{0}" columnas, pero en su lugar se encontraron "{1}" columnas.|  
  

## <a name="error-0068"></a>Error 0068  
 Se produce una excepción si el script de Hive especificado no es correcto.  
  
 Este error de Azure Machine Learning se produce si hay errores de sintaxis en un script de Hive QL, o si el intérprete de Hive encuentra un error al ejecutar la consulta o script.  
  
**Resolución:**

El mensaje de error de Hive normalmente se incluye en el registro de errores para que pueda tomar medidas según el error específico. 

+ Abra el módulo e inspeccione la consulta en busca de errores.  
+ Compruebe que la consulta funciona correctamente fuera de Azure Machine Learning al iniciar sesión en la consola de Hive del clúster de Hadoop y ejecutar la consulta.  
+ Intente colocar comentarios en el script de Hive en una línea independiente en lugar de mezclar las instrucciones ejecutables y los comentarios en una sola línea.  

### <a name="resources"></a>Recursos

Consulte los siguientes artículos para obtener ayuda con las consultas de Hive para el aprendizaje automático:

+ [Creación de tablas de Hive y carga de datos desde Azure Blob Storage](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-move-hive-tables)
+ [Exploración de los datos de las tablas de Hive con consultas de Hive](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-explore-data-hive-tables)
+ [Creación de características para los datos en un clúster de Hadoop mediante consultas de Hive](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-create-features-hive)
+ [Hive for SQL Users Cheat Sheet (PDF)](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) (hoja de referencia rápida de Hive para usuarios de SQL [PDF])

  
|Mensajes de excepción|  
|------------------------|  
|El script de Hive es incorrecto.|  
|El script de Hive {0} es incorrecto.|  
  

## <a name="error-0069"></a>Error 0069  
 Se produce una excepción si el script de SQL especificado no es correcto.  
  
 Este error de Azure Machine Learning se produce si el script SQL especificado tiene problemas de sintaxis, o si las columnas o tabla especificada en el script no son válidas. 
 
 Recibirá este error si el motor de SQL detecta cualquier error al ejecutar la consulta o script. El mensaje de error de SQL normalmente se incluye en el registro de errores para que pueda tomar medidas según el error específico.  
  
**Resolución:** Vuelva a consultar el módulo e inspeccione la consulta SQL en busca de errores.  
  
 Compruebe que la consulta funciona correctamente fuera de Azure Machine Learning al iniciar sesión directamente en el servidor de la base de datos y ejecutar la consulta.  
  
 Si la excepción del módulo le informa de un mensaje generado por SQL, tome medidas según el error notificado. Por ejemplo, los mensajes de error a veces incluyen instrucciones específicas sobre el posible error:
+ *La columna no existe o falta la base de datos*, que indica que tal vez escribió de forma incorrecta el nombre de una columna. Si está seguro de que nombre de columna es correcto, intente usar corchetes o comillas para delimitar el identificador de columna.
+ *Error lógico de SQL cerca de una \<palabra clave de SQL\>* , que indica que tal vez hay un error de sintaxis antes de la palabra clave especificada.

  
|Mensajes de excepción|  
|------------------------|  
|El script de SQL es incorrecto.|  
|La consulta SQL "{0}" no es correcta.|  
|La consulta SQL "{0}" no es correcta: {1}.|  
  

## <a name="error-0070"></a>Error 0070  
 Se produce una excepción al intentar acceder a la tabla de Azure inexistente.  
  
 Este error de Azure Machine Learning se produce cuando se intenta acceder a una tabla de Azure inexistente. Recibirá este error si, en el almacenamiento de Azure, especifica una tabla que existe cuando se lee o se escribe en Azure Table Storage. Esto puede ocurrir si se escribe incorrectamente el nombre de la tabla deseada, o si tiene una discrepancia entre el nombre de destino y el tipo de almacenamiento. Por ejemplo, quizá quería leer una tabla,pero escribió el nombre de un blob en su lugar.  
  
**Resolución:** Vuelva a consultar el módulo para comprobar que el nombre de la tabla es correcto.  
  
|Mensajes de excepción|  
|------------------------|  
|La tabla de Azure no existe.|  
|La tabla de Azure "{0}" no existe.|  
  
## <a name="error-0071"></a>Error 0071  
 Se produce una excepción si se proporcionan credenciales incorrectas.  
  
 Este error de Azure Machine Learning se produce si las credenciales proporcionadas son incorrectas.  
  
 También podría recibir este error si el módulo no se puede conectar a un clúster de HDInsight.  
  
**Resolución:** Revise las entradas en el módulo y compruebe el nombre de cuenta y la contraseña.  
  
 Busque los siguientes problemas que pueden producir un error:  
  
-   El esquema del conjunto de datos no coincide con el esquema del DataTable de destino.  
  
-   Los nombres de columna están mal escritos o faltan.  
  
-   Está escribiendo en una tabla que tiene nombres de columna con caracteres no válidos. Normalmente puede delimitar estos nombres de columna entre corchetes, pero si no funciona, edite los nombres de columna para que incluyan solo letras y guiones bajos (_).  
  
-   Las cadenas que está intentando escribir contienen comillas simples.  
  
 Si está intentando conectarse a un clúster de HDInsight, compruebe que se puede acceder al clúster de destino con las credenciales proporcionadas.  
  
|Mensajes de excepción|  
|------------------------|  
|Se pasaron credenciales incorrectas.|  
|Se pasó un nombre de usuario "{0}" o contraseña incorrectos.|  
  

## <a name="error-0072"></a>Error 0072  
 Se produce una excepción cuando se agota el tiempo de espera de conexión.  
  
 Este error de Azure Machine Learning se produce cuando se agota el tiempo de espera de una conexión. Recibirá este error si actualmente hay problemas de conectividad con el origen de datos o el destino (como una conectividad a Internet lenta), si el conjunto de datos es grande o si la consulta SQL que se leerá en los datos realiza procesos complicados.  
  
**Resolución:** Determine si actualmente hay problemas con conexiones lentas a Azure Storage o Internet.  
  
|Mensajes de excepción|  
|------------------------|  
|Se agotó el tiempo de espera de la conexión.|  
  

## <a name="error-0073"></a>Error 0073  
 Se produce una excepción si hay un error al convertir una columna a otro tipo.  
  
 Este error de Azure Machine Learning se produce cuando no es posible convertir la columna a otro tipo.  Recibirá este error si un módulo requiere un tipo concreto y no es posible convertir la columna al nuevo tipo.  
  
**Resolución:** Modifique el conjunto de datos de entrada para que la columna se pueda convertir en función de la excepción interna.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo convertir la columna.|  
|No se pudo convertir la columna a {0}.|  
  

## <a name="error-0074"></a>Error 0074  
 Se produce una excepción cuando [Edit Metadata](edit-metadata.md) (editar metadatos) intenta convertir una columna dispersa en categórica.  
  
 Este error en Azure Machine Learning se produce cuando [Edit Metadata](edit-metadata.md) (editar metadatos) intenta convertir una columna dispersa en categórica.  Recibirá este error al intentar convertir las columnas dispersas en categorías con la opción **Make categorical** (convertir en categoría).  Azure Machine Learning no admite matrices dispersas de categorías, por lo que se producirá un error en el módulo.  
  
 <!--**Resolution:**
 Make the column dense by using [Convert to Dataset](convert-to-dataset.md) first or do not convert the column to categorical.  -->
  
|Mensajes de excepción|  
|------------------------|  
|Las columnas dispersas no se pueden convertir en categorías.|  
  

## <a name="error-0075"></a>Error 0075  
Se produce una excepción cuando se usa una función de cuantificación no válida al cuantificar un conjunto de datos.  
  
Este error de Azure Machine Learning se produce cuando intenta cuantificar datos con un método no admitido, o cuando las combinaciones de parámetros no son válidas.  
  
**Resolución:**

Se introdujo un control de errores para este evento en una versión anterior de Azure Machine Learning que permitía más personalización de los métodos de cuantificación. Actualmente todos los métodos de cuantificación se basan en la selección de una lista desplegable por lo que, técnicamente, ya no debería ser posible obtener este error.

 <!--If you get this error when using the [Group Data into Bins](group-data-into-bins.md) module, consider reporting the issue in the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MachineLearning), providing the data types, parameter settings, and the exact error message.  -->
  
|Mensajes de excepción|  
|------------------------|  
|Se usó una función de cuantificación no válida.|  
  

## <a name="error-0077"></a>Error 0077  
 Se produce una excepción cuando se pasa un modo de escritura de archivo de blob desconocido.  
  
 Este error de Azure Machine Learning se produce si se pasa un argumento no válido en las especificaciones de un destino u origen de archivo de blob.  
  
**Resolución:** En casi todos los módulos que importan o exportan datos desde y hacia Azure Blob Storage los valores de parámetro que controlan el modo de escritura se asignan mediante una lista desplegable. Por lo tanto, no es posible pasar un valor no válido y no debe aparecer este error. Este error quedará en desuso en una versión posterior.  
  
|Mensajes de excepción|  
|------------------------|  
|Modo de escritura de blob no admitido.|  
|Modo de escritura de blob no admitido: {0}.|  
  

## <a name="error-0078"></a>Error 0078  
 Se produce una excepción cuando la opción HTTP para [Import Data](import-data.md) (importar datos) recibe un código de estado 3xx que indica redirección.  
  
 Este error de Azure Machine Learning se produce cuando la opción HTTP para [Import Data](import-data.md) (importar datos) recibe un código de estado 3xx (301, 302, 304, etc.) que indica redirección. Recibirá este error si intenta conectarse a un origen HTTP que redirige el explorador a otra página. Por cuestiones de seguridad, los sitios web de redirección no están permitidos como orígenes de datos en Azure Machine Learning.  
  
**Resolución:** Si el sitio web es un sitio web de confianza, escriba la dirección URL redirigida directamente.  
  
|Mensajes de excepción|  
|------------------------|  
|No se permite la redirección HTTP.|  
  

## <a name="error-0079"></a>Error 0079  
 Se produce una excepción si se especifica incorrectamente el nombre del contenedor de almacenamiento de Azure.  
  
 Este error de Azure Machine Learning se produce si el nombre del contenedor de almacenamiento de Azure se especifica incorrectamente. Recibirá este error si no especificó el nombre del contenedor y del blob (archivo) mediante la opción **Ruta de acceso que comienza con el contenedor de blobs** al escribir en Azure Blob Storage.  
  
**Resolución:** Vuelva a consultar el módulo [Export Data](export-data.md) (exportar datos) y compruebe que la ruta de acceso especificada en el blob contiene el nombre del contenedor y de archivo en el formato **contenedor/nombredearchivo**.  
  
|Mensajes de excepción|  
|------------------------|  
|El nombre del contenedor de almacenamiento de Azure es incorrecto.|  
|El nombre del contenedor de almacenamiento de Azure "{0}" no es correcto; se esperaba un nombre de contenedor con el formato contenedor/blob.|  
  

## <a name="error-0080"></a>Error 0080  
 Se produce una excepción cuando el módulo no permite las columnas con todos los valores que faltan.  
  
 Este error de Azure Machine Learning se produce cuando una o varias de las columnas utilizadas por el módulo contienen todos los valores que faltan. Por ejemplo, si un módulo está calculando estadísticas agregadas para cada columna, no puede funcionar en una columna que no contiene datos. En tales casos, la ejecución del módulo se detiene con esta excepción.  
  
**Resolución:** Vuelva a consultar el conjunto de datos de entrada y quite todas las columnas que contienen todos los valores que faltan.  
  
|Mensajes de excepción|  
|------------------------|  
|No se permiten las columnas que tienen todos los valores que faltan.|  
|La columna {0} tiene todos los valores que faltan.|  
  

## <a name="error-0081"></a>Error 0081  
 Se produce una excepción en el módulo PCA si el número de dimensiones para la reducción es igual al número de columnas de características en el conjunto de datos de entrada, que contiene al menos una columna de características dispersas.  
  
 Este error de Azure Machine Learning se produce si se cumplen las siguientes condiciones: (a) el conjunto de datos de entrada tiene al menos una columna dispersa y (b) el número final de dimensiones solicitado es el mismo que el número de dimensiones de entrada.  
  
**Resolución:** Considere la posibilidad de reducir el número de dimensiones en la salida para que sea menor que el número de dimensiones en la entrada. Esto es común en las aplicaciones de PCA.   <!--For more information, see [Principal Component Analysis](principal-component-analysis.md).  -->
  
|Mensajes de excepción|  
|------------------------|  
|Para los conjuntos de datos que contienen columnas de características dispersas, el número de dimensiones para la reducción debe ser menor que el número de columnas de características.|  
 

## <a name="error-0082"></a>Error 0082  
 Se produce una excepción cuando un modelo no se puede deserializar correctamente.  
  
 Este error de Azure Machine Learning se produce cuando una versión más nueva del runtime de Azure Machine Learning no puede cargar una transformación o modelo de aprendizaje automático guardado como resultado de un cambio importante.  
  
**Resolución:** El experimento de entrenamiento que generó el modelo o la transformación debe volver a ejecutarse, y el modelo o la transformación se deben volver a guardar.  
  
|Mensajes de excepción|  
|------------------------|  
|El modelo no se pudo deserializar porque probablemente se serializó con un formato de serialización anterior. Vuelva a entrenar y guardar el modelo.|  
  

## <a name="error-0083"></a>Error 0083  
 Se produce una excepción si el conjunto de datos que se usó para el entrenamiento no se puede usar para un tipo concreto de aprendiz.  
  
 Este error de Azure Machine Learning se produce cuando el conjunto de datos no es compatible con el aprendiz que se está entrenando. Por ejemplo, el conjunto de datos podría contener al menos un valor faltante en cada fila y, como resultado, todo el conjunto de datos se omitirá durante el entrenamiento. En otros casos, algunos algoritmos de aprendizaje automático (como la detección de anomalías) no esperan que las etiquetas estén presentes y generar esta excepción si las etiquetas están presentes en el conjunto de datos.  
  
**Resolución:** Consulte la documentación del aprendiz que está usando para comprobar los requisitos del conjunto de datos de entrada. Examine las columnas para comprobar que todas las columnas necesarias están presentes.  
  
|Mensajes de excepción|  
|------------------------|  
|El conjunto de datos usado para el entrenamiento no es válido.|  
|{0} contiene datos no válidos para el entrenamiento.|  
|{0} contiene datos no válidos para el entrenamiento. Tipo de aprendiz: {1}.|  
  

## <a name="error-0084"></a>Error 0084  
 Se produce una excepción cuando se evalúan las puntuaciones generadas a partir de un script de R. No se admite actualmente.  
  
 Este error de Azure Machine Learning se produce si intenta utilizar uno de los módulos para evaluar un modelo con el resultado de un script de R que contiene puntuaciones.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Actualmente no se admiten las puntuaciones de evaluación generadas por R.|  
  

## <a name="error-0085"></a>Error 0085  
 Se produce una excepción cuando se produce un error en la evaluación del script.  
  
 Este error de Azure Machine Learning se produce cuando se ejecuta un script personalizado que contiene errores de sintaxis.  
  
**Resolución:** Revise el código en un editor externo y compruebe si hay errores.  
  
|Mensajes de excepción|  
|------------------------|  
|Error al evaluar el script.|  
|Se produjo el siguiente error durante la evaluación del script. Consulte el registro de salida para obtener más información:  ---------- Inicio del mensaje de error del intérprete de {0} ---------- {1} ---------- Fin del mensaje de error del intérprete de {0} --------|  
  

## <a name="error-0086"></a>Error 0086  
 Se produce una excepción cuando una transformación de recuento no es válida.  
  
 Este error de Azure Machine Learning se produce cuando selecciona una transformación basada en una tabla de recuento, pero la transformación seleccionada no es compatible con los datos actuales o con la nueva tabla de recuento.  
  
**Resolución:** El módulo permite guardar los recuentos y las reglas que componen la transformación en dos formatos diferentes. Si va a combinar las tablas de recuento, compruebe que las dos tablas que quiere combinar utilizan el mismo formato.  
  
En general, una transformación basada en recuentos solo puede aplicarse a los conjuntos de datos que tienen el mismo esquema que el conjunto de datos en el que se creó originalmente la transformación.  
  
 <!-- For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). For requirements specific to creating and merging count-based features, see these topics:  
  
-   [Merge Count Transform](merge-count-transform.md)  
  
-   [Import Count Table](import-count-table.md)  
  
-   [Modify Count Table Parameters](modify-count-table-parameters.md)  
  -->
|Mensajes de excepción|  
|------------------------|  
|Se especificó una transformación de recuento no válida.|  
|La transformación de recuento en el puerto de entrada "{0}" no es válida.|  
|La transformación de recuento en el puerto de entrada "{0}" no se puede combinar con la transformación de recuento en el puerto de entrada "{1}". Compruebe los metadatos utilizados para el recuento de coincidencias.|  
  

## <a name="error-0087"></a>Error 0087  
 Se produce una excepción cuando se especifica un tipo de tabla de recuento no válido para el aprendizaje con módulos de recuentos.  
  
 Este error de Azure Machine Learning se produce cuando intenta realizar una importación a una tabla de recuento, pero la tabla no es compatible con los datos actuales o con la nueva tabla de recuento.  
  
**Resolución:** Existen formatos diferentes para guardar los recuentos y las reglas que componen la transformación. Si va a combinar las tablas de recuento, compruebe que ambas utilizan el mismo formato.  
  
 En general, una transformación basada en recuentos solo puede aplicarse a los conjuntos de datos que tienen el mismo esquema que el conjunto de datos en el que se creó originalmente la transformación.  
  
  <!--For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). -->
  

## <a name="error-0088"></a>Error 0088  
 Se produce una excepción cuando se especifica un tipo de recuento no válido para el aprendizaje con módulos de recuentos.  
  
 Este error de Azure Machine Learning se produce cuando intenta utilizar un método de recuento distinto al admitido para la caracterización basada en recuentos.  
  
**Resolución:** En general, los métodos de recuento se eligen desde una lista desplegable, por lo que no debería ver este error.  
  
  <!--For general information, see [Learning with Counts](data-transformation-learning-with-counts.md). For requirements specific to creating and merging count-based features, see these topics:  
  
-   [Merge Count Transform](merge-count-transform.md)  
  
-   [Import Count Table](import-count-table.md)  
  
-   [Modify Count Table Parameters](modify-count-table-parameters.md)  
  -->
|Mensajes de excepción|  
|------------------------|  
|Se especificó un tipo de recuento no válido.|  
|El tipo de recuento especificado "{0}" no es válido.|  
  

## <a name="error-0089"></a>Error 0089  
 Se produce una excepción cuando el número de clases especificado es menor que el número real de clases en el conjunto de datos utilizado para el recuento.  
  
 Este error de Azure Machine Learning se produce cuando crea una tabla de recuento y la columna de etiqueta contiene un número de clases distinto al especificado en los parámetros del módulo.  
  
**Resolución:** Examine el conjunto de datos y averigüe exactamente cuántos valores distintos (clases posibles) hay en la columna de etiqueta. Cuando se crea la tabla de recuento, debe especificar este número de clases como mínimo.  
  
 La tabla de recuento no puede determinar automáticamente el número de clases disponibles.  
  
 Cuando se crea la tabla de recuento, no se puede especificar 0 o ni ningún otro número menor que el número real de clases en la columna de etiqueta.  
  
|Mensajes de excepción|  
|------------------------|  
|El número de clases es incorrecto. Asegúrese de que el número de clases que especifique en el panel de parámetros es mayor o igual que el número de clases de la columna de etiqueta.|  
|El número de clases especificado es "{0}"; no es mayor que un valor de etiqueta "{1}" en el conjunto de datos que se usó para el recuento. Asegúrese de que el número de clases que especifique en el panel de parámetros es mayor o igual que el número de clases de la columna de etiqueta.|  
  

## <a name="error-0090"></a>Error 0090  
 Se produce una excepción cuando se produce un error en la creación de la tabla de Hive.  
  
 Este error de Azure Machine Learning se produce cuando utiliza [Export Data](export-data.md) (exportar datos) u otra opción para guardar datos en un clúster de HDInsight y no se puede crear la tabla de Hive especificada.  
  
**Resolución:** Compruebe el nombre de la cuenta de almacenamiento de Azure asociada al clúster y asegúrese de usar la misma cuenta en las propiedades del módulo.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo crear la tabla de Hive. Para un clúster de HDInsight, asegúrese de que el nombre de cuenta de almacenamiento de Azure asociado con el clúster es igual al valor que se pasa a través del parámetro de módulo.|  
|No se pudo crear la tabla de Hive "{0}". Para un clúster de HDInsight, asegúrese de que el nombre de cuenta de almacenamiento de Azure asociado con el clúster es igual al valor que se pasa a través del parámetro de módulo.|  
|No se pudo crear la tabla de Hive "{0}". Para un clúster de HDInsight, asegúrese de que el nombre de cuenta de almacenamiento de Azure asociado con el clúster es "{1}".|  
 

## <a name="error-0100"></a>Error 0100  
 Se produce una excepción cuando se especifica un lenguaje no admitido para un módulo personalizado.  
  
 Este error de Azure Machine Learning se produce cuando se está compilando un módulo personalizado y la propiedad de nombre del elemento **Language** en un archivo de definición de XML de módulo personalizado tiene un valor no válido. Actualmente, el único valor válido para esta propiedad es `R`. Por ejemplo:  
  
 `<Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />`  
  
**Resolución:** Compruebe que la propiedad de nombre del elemento **Language** en el archivo de definición de XML del módulo personalizado está establecida en `R`. Guarde el archivo, actualice el paquete ZIP del módulo personalizado e intente agregar de nuevo el módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|Se especificó un lenguaje de módulo personalizado no admitido.|  
  

## <a name="error-0101"></a>Error 0101  
 Todos los id. de puerto y de parámetro deben ser únicos.  
  
 Este error de Azure Machine Learning se produce cuando uno o varios puertos o parámetros se asignan el mismo valor de identificador en un archivo de definición de XML de módulo personalizado.  
  
**Resolución:** Compruebe que los valores de id. en todos los puertos y los parámetros son únicos. Guarde el archivo XML, actualice el paquete ZIP del módulo personalizado e intente agregar de nuevo el módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|Todos los id. de puerto y de parámetro deben ser únicos.|  
|El módulo "{0}" tiene id. de puerto o argumento duplicados. Todos los id. de puerto o argumento deben ser únicos para un módulo.|  
  

## <a name="error-0102"></a>Error 0102  
 Se produce cuando no se puede extraer un archivo ZIP.  
  
 Este error de Azure Machine Learning se produce cuando se importa un paquete comprimido con la extensión .zip, pero el paquete no es un archivo ZIP o no utiliza un formato ZIP admitido.  
  
**Resolución:** Asegúrese de que el archivo seleccionado es un archivo .ZIP válido y que se comprimió con alguno de los algoritmos de compresión admitidos.  
  
 Si recibe este error al importar los conjuntos de datos en formato comprimido, compruebe que todos los archivos contenidos utilicen alguno de los formatos de archivo compatibles y estén en formato Unicode.  <!--For more information, see [Unpack Zipped Datasets](unpack-zipped-datasets.md).  -->
  
 Intente volver a agregar los archivos deseados a una nueva carpeta ZIP comprimida e intente agregar de nuevo el módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|El archivo ZIP proporcionado no está en el formato correcto.|  


## <a name="error-0103"></a>Error 0103  
 Se produce cuando un archivo ZIP no contiene ningún archivo .xml.  
  
 Este error de Azure Machine Learning se produce cuando el paquete ZIP del módulo personalizado no contiene ningún archivo (.xml) de definición de módulo. Estos archivos deben residir en la raíz del paquete ZIP (por ejemplo, no dentro de una subcarpeta).  
  
**Resolución:** Compruebe que haya uno o varios archivos de definición del módulo XML en la carpeta raíz del paquete ZIP al extraerlos en una carpeta temporal de la unidad de disco. Los archivos XML deben encontrarse directamente en la carpeta en la que extrajo el paquete ZIP. Asegúrese de que al crear el paquete ZIP no selecciona una carpeta con los archivos XML que va a comprimir. Si lo hace, se creará una subcarpeta dentro del paquete ZIP con el mismo nombre que la carpeta que seleccionó para comprimir.  
  
|Mensajes de excepción|  
|------------------------|  
|El archivo ZIP no contiene los archivos de definición del módulo (archivos .xml)|  


## <a name="error-0104"></a>Error 0104  
 Se produce cuando un archivo de definición de módulo hace referencia a un script que no se encuentra.  
  
 Este error de Azure Machine Learning se produce cuando un archivo de definición de XML del módulo personalizado usa el elemento **Language** para hacer referencia a un archivo de script que no existe en el paquete ZIP. La ruta de acceso del archivo de script se define en la propiedad **sourceFile** del elemento **Language**. La ruta de acceso al archivo de origen es relativa a la raíz del paquete ZIP (la misma ubicación que los archivos de definición XML del módulo). Si el archivo de script está en una subcarpeta, debe especificarse la ruta de acceso relativa al archivo de script. Por ejemplo, si todos los scripts se almacenaran en una carpeta **myScripts** dentro del paquete ZIP, el elemento **Language** tendría que agregar esta ruta de acceso a la propiedad **sourceFile** como se muestra a continuación. Por ejemplo:  
  
 `<Language name="R" sourceFile="myScripts/CustomAddRows.R" entryPoint="CustomAddRows" />`  
  
**Resolución:** Asegúrese de que el valor de la propiedad **sourceFile** en el elemento **Language** de la definición de XML del módulo personalizado es correcta y de que el archivo de origen existe en la ruta de acceso relativa correcta dentro del paquete ZIP.  
  
|Mensajes de excepción|  
|------------------------|  
|El archivo de script de R al que se hace referencia no existe.|  
|No se pudo encontrar el archivo de script de R "{0}". Asegúrese de que la ruta de acceso relativa al archivo es correcta desde la ubicación de las definiciones.|  


## <a name="error-0105"></a>Error 0105  
 Este error aparece cuando un archivo de definición de módulo contiene un tipo de parámetro no admitido.  
  
 Este error de Azure Machine Learning se produce cuando crea una definición de XML de módulo personalizado y el tipo de un parámetro o un argumento en la definición no coincide con un tipo admitido.  
  
**Resolución:** Asegúrese de que la propiedad de tipo de todos los elementos **Arg** en el archivo de definición de XML del módulo personalizado sean de un tipo admitido.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo de parámetro no admitido.|  
|Se especificó un tipo de parámetro "{0}" no admitido.|  


## <a name="error-0106"></a>Error 0106  
 Se produce cuando un archivo de definición de módulo define un tipo de entrada no admitido.  
  
 Este error de Azure Machine Learning se produce cuando el tipo de un puerto de entrada en una definición XML de módulo personalizado no coincide con un tipo admitido.  
  
**Resolución:** Asegúrese de que la propiedad de tipo del elemento Input en el archivo de definición de XML del módulo personalizado sea de un tipo admitido.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo de entrada no admitido.|  
|Se especificó un tipo de entrada "{0}" no admitido.|  


## <a name="error-0107"></a>Error 0107  
 Se produce cuando un archivo de definición de módulo define un tipo de salida no admitido.  
  
 Este error de Azure Machine Learning se produce cuando el tipo de un puerto de salida en una definición XML de módulo personalizado no coincide con un tipo admitido.  
  
**Resolución:** Asegúrese de que la propiedad de tipo de un elemento Output en el archivo de definición de XML del módulo personalizado sea de un tipo admitido.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo de salida no admitido.|  
|Se especificó un tipo de salida "{0}" no admitido.|  


## <a name="error-0108"></a>Error 0108  
 Se produce cuando un archivo de definición de módulo define más puertos de entrada o salida de los que se admiten.  
  
 Este error de Azure Machine Learning se produce cuando se definen demasiados puertos de entrada o salida en una definición XML de módulo personalizado.  
  
**Resolución:** Asegúrese de que el número máximo de puertos de entrada y salida definidos en la definición XML de módulo personalizado no supera el número máximo admitido de puertos.  
  
|Mensajes de excepción|  
|------------------------|  
|Se superó el número admitido de puertos de entrada o salida.|  
|Se superó el número de puertos de "{0}" admitidos. El número máximo permitido de puertos de "{0}" es "{1}".| 

## <a name="error-0109"></a>Error 0109  
 Se produce cuando un archivo de definición de módulo define incorrectamente un selector de columnas.  
  
 Este error de Azure Machine Learning se produce cuando la sintaxis de un argumento de selector de columnas contiene un error en una definición de XML de módulo personalizado.  
  
**Resolución:** Este error se produce cuando la sintaxis de un argumento de selector de columnas contiene un error en una definición de XML de módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|Sintaxis no admitida para el selector de columnas.|  
  

## <a name="error-0110"></a>Error 0110  
 Se produce cuando un archivo de definición de módulo define un selector de columnas que hace referencia a un id. de puerto de entrada inexistente.  
  
 Este error de Azure Machine Learning se produce cuando la propiedad *portId* dentro del elemento Properties de un Arg del tipo ColumnPicker no coincide con el valor de id. de un puerto de entrada.  
  
**Resolución:** Asegúrese de que la propiedad portId coincide con el valor de id. de un puerto de entrada definido en la definición XML de módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|El selector de columnas hace referencia a un id. de puerto de entrada que no existe.|  
|El selector de columnas hace referencia a un id. de puerto de entrada "{0}" que no existe.|  
  

## <a name="error-0111"></a>Error 0111  
 Se produce cuando un archivo de definición de módulo define una propiedad no válida.  
  
 Este error de Azure Machine Learning se produce cuando se asigna una propiedad no válida a un elemento en la definición XML de módulo personalizado.  
  
**Resolución:** Asegúrese de que la propiedad es compatible con el elemento de módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|La definición de propiedad no es válida.|  
|La definición de propiedad "{0}" no es válida.|  
  

## <a name="error-0112"></a>Error 0112  
 Se produce cuando no se puede analizar un archivo de definición de módulo.  
  
 Este error de Azure Machine Learning se produce cuando se produce un error en el formato XML que impide que la definición de XML de módulo personalizado se analice como un archivo XML válido.  
  
**Resolución:** Asegúrese de que todos los elementos se abren y se cierran correctamente. Asegúrese de que no hay ningún error en el formato XML.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede analizar el archivo de definición del módulo.|  
|No se puede analizar el archivo de definición del módulo "{0}".|  
  

## <a name="error-0113"></a>Error 0113  
 Se produce cuando un archivo de definición de módulo contiene errores.  
  
 Este error de Azure Machine Learning se produce cuando el archivo de definición de XML de módulo personalizado puede analizarse pero contiene errores, tales como la definición de elementos que los módulos personalizados no admiten.  
  
**Resolución:** Asegúrese de que el archivo de definición de módulo personalizado define los elementos y propiedades que son compatibles con módulos personalizados.  
  
|Mensajes de excepción|  
|------------------------|  
|El archivo de definición de módulo contiene errores.|  
|El archivo de definición de módulo "{0}" contiene errores.|  
|El archivo de definición de módulo "{0}" contiene errores. {1}|  
  

## <a name="error-0114"></a>Error 0114  
 Se produce cuando se produce un error al crear un módulo.  
  
 Este error de Azure Machine Learning se produce cuando se produce un error al crear un módulo personalizado. Esto se produce cuando experimenta uno o varios errores relacionados con el módulo personalizado al agregar el módulo personalizado. Los errores adicionales se notifican dentro de este mensaje de error.  
  
**Resolución:** Resuelva los errores notificados en este mensaje de excepción.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo crear el módulo personalizado.|  
|Las creaciones de módulos personalizados devolvieron los errores: {0}|  
  

## <a name="error-0115"></a>Error 0115  
 Se produce cuando un script predeterminado de módulo personalizado tiene una extensión no admitida.  
  
 Este error de Azure Machine Learning se produce cuando proporciona un script para un módulo personalizado que utiliza una extensión de nombre de archivo desconocida.  
  
**Resolución:** Compruebe el formato de archivo y la extensión del nombre de archivo de todos los archivos de script incluidos en el módulo personalizado.  
  
|Mensajes de excepción|  
|------------------------|  
|Extensión no admitida para el script predeterminado.|  
|Extensión de archivo {0} no admitida para el script predeterminado.|  
  

## <a name="error-0121"></a>Error 0121  
 Se produce cuando los procesos de escritura SQL devuelven un error porque no se puede escribir en la tabla.  
  
 Este error de Azure Machine Learning se produce cuando se usa el módulo [Export Data](export-data.md) (exportar datos) para guardar los resultados en una tabla de una base de datos SQL y no se puede escribir en la tabla. Normalmente, verá este error si el módulo [Export Data](export-data.md) (exportar datos) establece correctamente una conexión con la instancia de SQL Server pero no logra escribir el contenido del conjunto de datos de Azure Machine Learning en la tabla.  
  
**Resolución:**
 - Abra el panel de propiedades del módulo [Export Data](export-data.md) (exportar datos) y compruebe que los nombres de base de datos y tabla se escribieron correctamente. 
 - Revise el esquema del conjunto de datos que va a exportar y asegúrese de que los datos son compatibles con la tabla de destino.
 - Compruebe que el inicio de sesión SQL asociado con el nombre de usuario y contraseña tiene los permisos para escribir en la tabla. 
 - Si la excepción contiene información de error adicional desde SQL Server, utilice esa información para realizar las correcciones.  
  
|Mensajes de excepción|  
|------------------------|  
|Se conectó al servidor, no se puede escribir en la tabla.|  
|No se puede escribir en la tabla de SQL: {0}|  


## <a name="error-0122"></a>Error 0122  
 Se produce una excepción si se especifican varias columnas de peso y solo se permite una.  
  
 Este error de Azure Machine Learning se produce cuando se han seleccionado demasiadas columnas como columnas de peso.  
  
**Resolución:** Revise el conjunto de datos de entrada y sus metadatos. Asegúrese de que solo una columna contiene pesos.  
  
|Mensajes de excepción|  
|------------------------|  
|Se especificaron varias columnas de peso.|  


## <a name="error-0123"></a>Error 0123  
 Se produce una excepción si se especifica la columna de vectores en la columna de etiqueta.  
  
 Este error de Azure Machine Learning se produce si utiliza un vector como columna de etiqueta.  
  
**Resolución:** De ser necesario, cambie el formato de datos de la columna o elija una columna diferente.  
  
|Mensajes de excepción|  
|------------------------|  
|La columna de vectores se especificó como una columna de etiqueta.|  


## <a name="error-0124"></a>Error 0124  
 Se produce una excepción si se especifican columnas no numéricas como columnas de peso.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Se especificó una columna no numérica como columna de peso.|  
  


## <a name="error-0125"></a>Error 0125  
 Se produce cuando el esquema de varios conjuntos de datos no coincide.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|El esquema del conjunto de datos no coincide.|  


## <a name="error-0126"></a>Error 0126  
 Se produce una excepción si el usuario especifica un dominio SQL que no se admite en Azure Machine Learning.  
  
 Este error se produce cuando el usuario especifica un dominio SQL que no se admite en Azure Machine Learning. Recibirá este error si intenta conectarse a un servidor de base de datos en un dominio que no está permitido. Actualmente, los dominios SQL permitidos son: ". database.windows.net",".cloudapp.net" o ".database.secure.windows.net". Es decir, el servidor debe ser una instancia de Azure SQL server o un servidor en una máquina virtual de Azure.  
  
**Resolución:** Vuelva a consultar el módulo. Compruebe que el servidor de base de datos SQL pertenece a uno de los dominios aceptados:  
  
-   .database.windows.net  
  
-   .cloudapp.net  
  
-   .database.secure.windows.net  
  
|Mensajes de excepción|  
|------------------------|  
|Dominio SQL no admitido.|  
|El dominio SQL {0} no se admite actualmente en Azure Machine Learning.|  
  

## <a name="error-0127"></a>Error 0127  
 El tamaño en píxeles de la imagen supera el límite permitido.  
  
 Este error se produce si se están leyendo imágenes de un conjunto de datos de imagen para clasificación y las imágenes son más grandes de lo que el modelo puede controlar.  
  
 <!--**Resolution:**
 For more information about the image size and other requirements, see these topics:  
  
-   [Import Images](import-images.md)  
  
-   [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md)  -->
  
|Mensajes de excepción|  
|------------------------|  
|El tamaño en píxeles de la imagen supera el límite permitido.|  
|El tamaño en píxeles de la imagen en el archivo "{0}" supera el límite permitido: "{1}".|  


## <a name="error-0128"></a>Error 0128  
 El número de probabilidades condicionales para las columnas de categorías supera el límite.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|El número de probabilidades condicionales para las columnas de categorías supera el límite.|  
|El número de probabilidades condicionales para las columnas de categorías supera el límite. Las columnas "{0}" y "{1}" son el par problemático.|  


## <a name="error-0129"></a>Error 0129  
 El número de columnas en el conjunto de datos supera el límite permitido.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|El número de columnas en el conjunto de datos supera el límite permitido.|  
|El número de columnas en el conjunto de datos de "{0}" supera lo permitido.|  
|El número de columnas en el conjunto de datos de "{0}" supera el límite permitido de "{1}".|  
|El número de columnas en el conjunto de datos de "{0}" supera el límite de "{1}" permitido de "{2}".|  
## <a name="error-0130"></a>Error 0130  
 Se produce una excepción cuando todas las filas del conjunto de datos de entrenamiento contienen valores faltantes.  
  
 Esto sucede si alguna columna del conjunto de datos de entrenamiento está vacía.  
  
**Resolución:** Utilice el módulo [Clean Missing Data](clean-missing-data.md) (limpiar datos que faltan) para quitar las columnas con todos los valores que faltan.  
  
|Mensajes de excepción|  
|------------------------|  
|Todas las filas del conjunto de datos de entrenamiento contienen valores que faltan.  Considere utilizar el módulo Clean Missing Data (limpiar datos que faltan) para quitar los valores que faltan.|  
 

## <a name="error-0131"></a>Error 0131  
 Se produce una excepción si se produce un error al descomprimir y registrar correctamente uno o varios conjuntos de datos en un archivo ZIP.  
  
 Este error se produce cuando se produce un error al descomprimir y leer correctamente uno o varios conjuntos de datos en un archivo ZIP. Recibirá este error si la descompresión produce un error porque el archivo ZIP o uno de los archivos que contiene están dañados, o bien si hay un error del sistema al intentar descomprimir y expandir un archivo.  
  
**Resolución:** Utilice los detalles proporcionados en el mensaje de error para determinar el procedimiento a seguir.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudieron cargar los conjuntos de datos comprimidos.|  
|Error del conjunto de datos comprimido {0} con el mensaje siguiente: {1}|  
|Error del conjunto de datos comprimido {0} con una excepción de {1} con el mensaje: {2}|  
  

## <a name="error-0132"></a>Error 0132  
 No se especificó ningún nombre de archivo para descomprimirse; se encontraron varios archivos en el archivo ZIP.  
  
 Este error se produce cuando no se especificó ningún nombre de archivo para descomprimirse; se encontraron varios archivos en el archivo ZIP. Recibirá este error si el archivo .zip contiene más de un archivo comprimido, pero no especificó un archivo para extracción en el cuadro de texto **Dataset to Unpack** (conjunto de datos para desempaquetar) en el panel **Property** del módulo. Actualmente, se puede extraer un único archivo cada vez que se ejecuta el módulo.  
  
**Resolución:** El mensaje de error proporciona una lista de los archivos que se encuentran en el archivo .zip. Copie el nombre del archivo deseado y péguelo en el cuadro de texto **Dataset to Unpack** (conjunto de datos para desempaquetar).  
  
|Mensajes de excepción|  
|------------------------|  
|El archivo ZIP contiene varios archivos; debe especificar el archivo que se va a expandir.|  
|El archivo contiene más de un archivo. Especifique el archivo que se va a expandir. Se encontraron los siguientes archivos: {0}|  
  

## <a name="error-0133"></a>Error 0133  
 No se encontró el archivo especificado en el archivo ZIP.  
  
 Este error se produce cuando el nombre de archivo especificado en el campo **Dataset to Unpack** (conjunto de datos para desempaquetar) del panel **Property** no coincide con el nombre de ninguno de los archivos que se encuentran en el archivo .zip. Las causas más comunes de este error son los errores tipográficos o buscar el archivo que quiere expandir en el archivo incorrecto.  
  
**Resolución:** Vuelva a consultar el módulo. Si el nombre del archivo que quería descomprimir se muestra en la lista de archivos encontrados, copie el nombre de archivo y péguelo en el cuadro de propiedad **Dataset to Unpack** (conjunto de datos para descomprimir). Si no ve el nombre de archivo deseado en la lista, compruebe que tiene el archivo .zip correcto y el nombre correcto del archivo deseado.  
  
|Mensajes de excepción|  
|------------------------|  
|No se encontró el archivo especificado en el archivo ZIP.|  
|No se encontró el archivo especificado. Se encontraron los siguientes archivos: {0}|  
  

## <a name="error-0134"></a>Error 0134
Se produce una excepción cuando falta la columna de etiqueta o no tiene el número suficiente de filas con etiqueta.  
  
Este error se produce cuando el módulo requiere una columna de etiqueta, pero no se incluyó una en la selección de columnas o a la columna de etiquetas le faltan demasiados valores.

Este error también puede producirse cuando una operación anterior cambia el conjunto de datos de forma que no hay filas suficientes disponibles para una operación descendente. Por ejemplo, suponga que utiliza una expresión en el módulo **Partition and sample** (partición y muestra) para dividir un conjunto de datos por valores. Si no se encuentra ninguna coincidencia para la expresión, uno de los conjuntos de datos resultantes de la partición estaría vacío.

Resolución: 

 Si incluye una columna de etiqueta en la selección de columna, pero no se reconoce, utilice el módulo [Edit Metadata](edit-metadata.md) (editar metadatos) para marcarla como una columna de etiqueta.
  
  <!--Use the [Summarize Data](summarize-data.md) module to generate a report that shows how many values are missing in each column. -->A continuación, puede usar el módulo [Clean Missing Data](clean-missing-data.md) (eliminar datos faltantes) para quitar las filas con valores que faltan en la columna de etiqueta. 

 Compruebe los conjuntos de datos de entrada para asegurarse de que contienen datos válidos y filas suficientes para cumplir los requisitos de la operación. Muchos algoritmos generarán un mensaje de error si requieren un número mínimo de filas de datos, pero los datos contienen solo unas pocas filas, o solo un encabezado.
  
|Mensajes de excepción|
|------------------------|
|Se produce una excepción cuando falta la columna de etiqueta o no tiene el número suficiente de filas con etiqueta.|  
|Se produce una excepción cuando falta la columna de etiqueta o tiene menos de {0} filas con etiqueta.|  
  

## <a name="error-0135"></a>Error 0135  
 Solo se admiten los clústeres basados en centroides.  
  
**Resolución:** Podría encontrar este mensaje de error si intenta evaluar un modelo de agrupación en clústeres basado en un algoritmo de agrupación en clústeres personalizado que no utilice centroides para inicializar el clúster.  
  
  <!--You can use [Evaluate Model](evaluate-model.md) to evaluate clustering models that are based on the  [K-Means Clustering](k-means-clustering.md) module. For custom algorithms, use the [Execute R Script](execute-r-script.md) module to create a custom evaluation script.  -->
  
|Mensajes de excepción|  
|------------------------|  
|Solo se admiten los clústeres basados en centroides.|  
  

## <a name="error-0136"></a>Error 0136  
 No se devolvió ningún nombre de archivo; como resultado, no se puede procesar el archivo.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|No se devolvió ningún nombre de archivo; como resultado, no se puede procesar el archivo.|  
  

## <a name="error-0137"></a>Error 0137  
 El SDK de Azure Storage encontró un error al convertir entre las propiedades de tabla y columnas de conjunto de datos durante la lectura o escritura.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Error de conversión entre la propiedad de Azure Table Storage y la columna de conjunto de datos.|  
|Error de conversión entre la propiedad de Azure Table Storage y la columna de conjunto de datos. Información adicional: {0}|  

## <a name="error-0138"></a>Error 0138  
 La memoria se ha agotado, no se puede completar la ejecución del módulo. La reducción del conjunto de datos puede ayudar a solucionar el problema.  
  
 Este error se produce cuando el módulo que se está ejecutando requiere más memoria de la que está disponible en el contenedor de Azure. Esto puede ocurrir si está trabajando con un conjunto de datos de gran tamaño y la operación actual no cabe en la memoria.  
  
**Resolución:** Si intenta leer un conjunto de datos grande y no se puede completar la operación, puede ser útil reducir el conjunto de datos.  
  
  <!--If you use the visualizations on datasets to check the cardinality of columns, only some rows are sampled. To get a full report, use [Summarize Data](summarize-data.md). You can also use the [Apply SQL Transformation](apply-sql-transformation.md) to check for the number of unique values in each column.  
  
 Sometimes transient loads can lead to this error. Machine support also changes over time. 
  
 Try using [Principal Component Analysis](principal-component-analysis.md) or one of the provided feature selection methods to reduce your dataset to a smaller set of more feature-rich columns: [Feature Selection](feature-selection-modules.md)  -->
  
|Mensajes de excepción|  
|------------------------|  
|La memoria se ha agotado, no se puede completar la ejecución del módulo.|  
  

## <a name="error-0139"></a>Error 0139  
 Se produce una excepción cuando no es posible convertir una columna a otro tipo.  
  
 Este error de Azure Machine Learning se produce cuando intenta convertir una columna en un tipo de datos diferente, pero dicho tipo no lo admiten la operación actual o el módulo.  
  
 El error también puede aparecer cuando un módulo intenta convertir implícitamente los datos para cumplir con los requisitos del módulo actual, pero la conversión no es posible.  
  
**Resolución:**

1. Revise los datos de entrada y determine el tipo de datos exacto de la columna que quiere usar y el tipo de datos de la columna que está generando el error. En ocasiones puede parecer que el tipo de datos es correcto, pero descubre que una operación ascendente modificó el tipo de datos o el uso de una columna. Utilice el módulo [Edit Metadata](edit-metadata.md) (editar etiquetas) para restablecer los metadatos de columna a su estado original. 
2. Examine la página de ayuda del módulo para comprobar los requisitos para la operación especificada. Determine qué tipos de datos son compatibles con el módulo actual, y qué intervalo de valores se admite. 
 <!--3. If values need to be truncated, rounded, or outliers removed, use the [Apply Math Operation](apply-math-operation.md) or [Clip Values](clip-values.md) modules to make corrections.-->
4. Considere si es posible convertir la columna a un tipo de datos diferente. Todos los módulos siguientes proporcionan bastante flexibilidad y eficacia para modificar datos: 
 <!--
   + [Apply SQL Transformation](apply-sql-transformation.md)
   + [Execute R Script](execute-r-script.md)
-->   
   + [Execute Python Script](execute-python-script.md) (ejecutar script de Python).  

> [!NOTE]
> ¿Sigue sin funcionar? Considere la posibilidad de enviar comentarios adicionales sobre el problema para ayudarnos a desarrollar mejores guías para solucionar problemas. Simplemente envíe comentarios sobre esta página y proporcione el nombre del módulo que generó el error, así como la conversión de tipos de datos que produjo un error.
  
|Mensajes de excepción|  
|------------------------|  
|Conversión no permitida.|  
|No se pudo convertir: {0}.|  
|No se pudo convertir: {0}, en la fila {1}.|  
|No se pudo convertir la columna del tipo {0} a la columna del tipo {1} en la fila {2}.|  
|No se pudo convertir la columna "{2}" del tipo {0} a la columna del tipo {1} en la fila {3}.|  
|No se pudo convertir la columna "{2}" del tipo {0} a la columna "{3}" del tipo {1} en la fila {4}.| 

## <a name="error-0140"></a>Error 0140  
 Se produce una excepción si el argumento del conjunto de columnas que se pasó no contiene otras columnas, excepto una columna de etiqueta.  
  
 Este error se produce si se conectó un conjunto de datos con un módulo que requiere varias columnas (incluida una de características), pero se ha proporcionado solo la columna de etiqueta.  
  
**Resolución:** Elija al menos una columna de características para incluirla en el conjunto de datos.  
  
|Mensajes de excepción|  
|------------------------|  
|El conjunto de columnas especificado no contiene otras columnas, excepto una columna de etiqueta.|  
  

## <a name="error-0141"></a>Error 0141  
 Se produce una excepción si el número de columnas numéricas seleccionadas y valores únicos en las columnas de categoría y de cadena es demasiado pequeño.  
  
 Este error de Azure Machine Learning se produce cuando no hay suficientes valores únicos en la columna seleccionada para realizar la operación.  
  
**Resolución:** Algunas operaciones realizan operaciones estadísticas en columnas de categorías y de características y, si no hay suficientes valores, la operación podría producir un error o devolver un resultado no válido. Examine el conjunto de datos para ver cuántos valores hay en las columnas de característica y etiqueta y determine si la operación que está intentando realizar es estadísticamente válida.  
  
 Si el conjunto de datos de origen es válido, también puede comprobar si alguna manipulación de datos ascendentes u operación de metadatos cambió los datos y quitó algunos valores.  
  
 Si las operaciones ascendentes incluyen la división, el muestreo o nuevos muestreos, compruebe que las salidas contienen el número esperado de filas y valores.  
  
|Mensajes de excepción|  
|------------------------|  
|El número de columnas numéricas seleccionadas y valores únicos en las columnas de categoría y de cadena es demasiado pequeño.|  
|El número total de columnas numéricas seleccionadas y valores únicos en las columnas de categoría y de cadena (actualmente, {0}) debería ser {1} como mínimo.|  
  

## <a name="error-0142"></a>Error 0142  
 Se produce una excepción cuando el sistema no puede cargar el certificado para autenticación.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|No se puede cargar el certificado.|  
|No se puede cargar el certificado {0}. Su huella digital es {1}.|  
  

## <a name="error-0143"></a>Error 0143  
 No se puede analizar la dirección URL proporcionada por el usuario que se supone procede de GitHub.  
  
 Este error de Azure Machine Learning se produce cuando se especifica una dirección URL no válida y el módulo requiere una URL válida de GitHub.  
  
**Resolución:** Compruebe que la dirección URL haga referencia a un repositorio de GitHub válido. Actualmente, no se admiten otros tipos de sitio.  
  
|Mensajes de excepción|  
|------------------------|  
|La dirección URL no es de github.com.|  
|La dirección URL no es de github.com: {0}|  

## <a name="error-0144"></a>Error 0144  
 A la URL de GitHub proporcionada por el usuario le falta un elemento esperado.  
  
 Este error de Azure Machine Learning se produce cuando se especifica un origen de archivo de GitHub con un formato de dirección URL no válido.  
  
**Resolución:** Compruebe que la dirección URL del repositorio de GitHub es válida y termina con \blob\ o \tree\\.  
  
|Mensajes de excepción|  
|------------------------|  
|No se puede analizar la dirección URL de GitHub.|  
|No se puede analizar la dirección URL de GitHub (se esperaba "\blob\\" o "\tree\\" después del nombre del repositorio): {0}|  

## <a name="error-0145"></a>Error 0145  
 Por algún motivo, no se puede crear el directorio de replicación.  
  
 Este error de Azure Machine Learning se produce cuando el módulo no puede crear el directorio especificado.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|No se puede crear el directorio de replicación.|  
  

## <a name="error-0146"></a>Error 0146  
 Cuando los archivos de usuario se descomprimen en el directorio local, la ruta de acceso combinada puede ser demasiado larga.  
  
 Este error de Azure Machine Learning se produce cuando está extrayendo archivos, pero algunos de los nombres de archivo son demasiado largos después de la descompresión.  
  
**Resolución:** Edite los nombres de archivo de forma que la ruta de acceso y el nombre de archivo no superen los 248 caracteres.  
  
|Mensajes de excepción|  
|------------------------|  
|La ruta de replicación tiene más de 248 caracteres, acorte el nombre del script o la ruta de acceso.|  

## <a name="error-0147"></a>Error 0147  
 Por algún motivo, no se pudieron descargar elementos de GitHub.  
  
 Este error de Azure Machine Learning se produce cuando no se pueden leer o descargar los archivos especificados desde GitHub.  
  
**Resolución:** El problema podría ser temporal; intente acceder a los archivos en otro momento. O bien compruebe que tiene los permisos necesarios y que el origen es válido.  
  
|Mensajes de excepción|  
|------------------------|  
|Error de acceso a GitHub.|  
|Error de acceso a GitHub. {0}|  
  

## <a name="error-0148"></a>Error 0148  
 Problemas de acceso no autorizado al extraer datos o crear el directorio.  
  
 Este error de Azure Machine Learning se produce cuando está intentando crear un directorio o leer datos de almacenamiento, pero no tiene los permisos necesarios.  
  
**Resolución:**
  
|Mensajes de excepción|  
|------------------------|  
|Excepción de acceso no autorizado al extraer datos.|  
  

## <a name="error-0149"></a>Error 0149  
 El archivo de usuario no existe dentro de la agrupación de GitHub.  
  
 Este error de Azure Machine Learning se produce cuando no se puede encontrar el archivo especificado.  
  
Resolución: 
  
|Mensajes de excepción|  
|------------------------|  
|No se encuentra el archivo de GitHub.|  
|No se encuentra el archivo de GitHub: {0}|  
  

## <a name="error-0150"></a>Error 0150  
 Los scripts que proceden de los paquetes de usuario no se pudieron comprimir, probablemente debido a una colisión con archivos de GitHub.  
  
 Este error de Azure Machine Learning se produce cuando no se puede extraer un script, normalmente cuando hay un archivo existente del mismo nombre.  
  
Resolución:
  
|Mensajes de excepción|  
|------------------------|  
|La agrupación no se puede descomprimir; probablemente hay una colisión de nombres con archivos de GitHub.|  
  

## <a name="error-0151"></a>Error 0151  
 Se produjo un error al escribir en el almacenamiento en nube. Compruebe la dirección URL.  
  
 Este error de Azure Machine Learning se produce cuando el módulo intenta escribir datos en el almacenamiento en la nube, pero la dirección URL no está disponible o no es válida.  
  
Resolución: Examine la dirección URL y compruebe que es grabable.  
  
|Mensajes de excepción|  
|------------------------|  
|Error al escribir en el almacenamiento en la nube (posiblemente debido a una dirección URL incorrecta).|  
|Se produjo un error al escribir en el almacenamiento en la nube: {0}. Compruebe la dirección URL.|  
  
## <a name="error-0152"></a>Error 0152  
 El tipo de la nube de Azure se especificó incorrectamente en el contexto del módulo.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo de nube de Azure incorrecto.|  
|Tipo de nube de Azure incorrecto: {0}|  
  
## <a name="error-0153"></a>Error 0153  
 El punto de conexión de almacenamiento especificado no es válido.  
  
|Mensajes de excepción|  
|------------------------|  
|Tipo de nube de Azure incorrecto.|  
|Punto de conexión de almacenamiento incorrecto: {0}|  

## <a name="error-0154"></a>Error 0154  
 No se pudo resolver el nombre del servidor especificado.  
  
|Mensajes de excepción|  
|------------------------|  
|No se pudo resolver el nombre del servidor especificado.|  
|No se pudo resolver el servidor {0}.documents.azure.com especificado.|

## <a name="error-0155"></a>Error 0155  
 El cliente DocDb produjo una excepción.  
  
|Mensajes de excepción|  
|------------------------|  
|El cliente DocDb produjo una excepción.|  
|Cliente DocDb: {0}|

## <a name="error-0156"></a>Error 0156  
 Respuesta incorrecta para el servidor de HCatalog.  
  
|Mensajes de excepción|  
|------------------------|  
|Respuesta incorrecta para el servidor de HCatalog. Compruebe que se están ejecutando todos los servicios.|  
|Respuesta incorrecta para el servidor de HCatalog. Compruebe que se están ejecutando todos los servicios. Detalles del error: {0}|

## <a name="error-0157"></a>Error 0157  
 Se produjo un error al leer desde Azure Cosmos DB debido a esquemas de documentos incoherentes o distintos. El lector necesita que todos los documentos tengan el mismo esquema.  
  
|Mensajes de excepción|  
|------------------------|  
|Se detectaron documentos con esquemas distintos. Asegúrese de que todos los documentos tienen el mismo esquema.|

## <a name="error-1000"></a>Error 1000  
Excepción interna de biblioteca.  
  
Este error se proporciona para capturar los errores de motor interno que no se controlan de otra forma. Por lo tanto, la causa de este error puede variar según el módulo que generó el error.  
  
Para obtener más ayuda, se recomienda que publique el mensaje detallado que acompaña al error en el foro de Azure Machine Learning, junto con una descripción del escenario. Incluya los datos que se usaron como entradas. Estos comentarios nos ayudarán a dar prioridad a los errores e identificar los problemas más importantes para operaciones futuras.  
  
|Mensajes de excepción|  
|------------------------|  
|Excepción de biblioteca.|  
|Excepción de biblioteca: {0}|  
|Excepción de biblioteca {1}: {0}|  
