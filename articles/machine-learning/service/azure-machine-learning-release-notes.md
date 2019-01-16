---
title: Novedades de la versión
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre las actualizaciones más recientes a Azure Machine Learning Service y los SDK de Python para el aprendizaje automático y la preparación de datos.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
author: hning86
ms.author: haining
ms.reviewer: j-martens
ms.date: 12/20/2018
ms.custom: seodec18
ms.openlocfilehash: 157591897adb7495382b540dc1517c4c652a2632
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190741"
---
# <a name="azure-machine-learning-service-release-notes"></a>Notas de la versión del servicio Azure Machine Learning

En este artículo obtendrá información acerca de las versiones del servicio Azure Machine Learning. 

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>SDK de preparación de datos de Azure Machine Learning v1.0.6

+ **Documentos de referencia del SDK**: https://aka.ms/data-prep-sdk

+ **Correcciones de errores**
  + Se corrigió el error en la lectura desde contenedores de Azure Blob públicamente legibles en Spark

## <a name="2018-12-20"></a>20-12-2018: 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>SDK de Azure Machine Learning para Python v1.0.6

+ **Documentos de referencia del SDK**: https://aka.ms/aml-sdk

+ **Correcciones de errores**: Esta versión contiene principalmente correcciones de errores menores.

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>SDK de preparación de datos de Azure Machine Learning v1.0.4

+ **Documentos de referencia del SDK**: https://aka.ms/data-prep-sdk

+ **Nuevas características:**
  + La función `to_bool` ahora permite que los valores no coincidentes se convierten en valores Error. Este es el nuevo comportamiento de error de coincidencia predeterminado para `to_bool` y `set_column_types`, mientras que el comportamiento predeterminado anterior era convertir los valores no coincidentes en False.
  + Al llamar a `to_pandas_dataframe`, hay una nueva opción para interpretar los valores que faltan o NULL en columnas numéricas como NaN.
  + Se agregó la posibilidad de comprobar el tipo de valor devuelto de algunas expresiones para garantizar la coherencia de los tipos y errores pronto.
  + Ahora puede llamar a `parse_json` para analizar los valores de una columna como objetos JSON y expandirlos en varias columnas.

+ **Correcciones de errores**
  + Se ha corregido un error que bloqueaba a `set_column_types` en Python 3.5.2.
  + Se ha corregido un error que ocasionaba un bloqueo al conectarse al almacén de datos mediante una imagen de Azure Machine Learning.

+ **Actualizaciones**
  * [Cuadernos de ejemplo](https://aka.ms/aml-data-prep-notebooks) para obtener tutoriales de introducción, casos prácticos y guías de procedimientos.

## <a name="2018-12-04-general-availability"></a>04-12-2018: Disponibilidad general

Azure Machine Learning Service está disponible con carácter general.

### <a name="azure-machine-learning-compute"></a>Proceso de Azure Machine Learning
Con esta versión, anunciamos una nueva experiencia de proceso administrado a través del [Proceso de Machine Learning](how-to-set-up-training-targets.md#amlcompute). Este destino de proceso reemplaza el proceso de Azure Batch AI para Azure Machine Learning. 

Este destino de proceso:
+ Se usa para las inferencias de lotes y el entrenamiento del modelo
+ Es un proceso de uno a varios nodos
+ Realiza la administración del clúster y la programación de trabajos para el usuario
+ Se escala automáticamente de forma predeterminada
+ Es compatible con recursos de CPU y GPU 
+ Permite el uso de máquinas virtuales de prioridad baja por un costo reducido

El proceso de Azure Machine Learning se puede crear en Python, mediante Azure Portal o la CLI. Debe crearse en la región de su área de trabajo y no se puede asociar a ninguna otra área de trabajo. Este destino de proceso usa un contenedor de Docker para su ejecución y empaqueta sus dependencias para replicar el mismo entorno en todos sus nodos.

> [!Warning]
> Recomendamos crear una nueva área de trabajo para usar el proceso de Azure Machine Learning. Existe la remota posibilidad de que los usuarios que tratan de crear el proceso de Azure Machine Learning a partir de un área de trabajo existente puedan ver un error. El proceso existente en su área de trabajo debería continuar funcionando del modo habitual.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>SDK de Azure Machine Learning para Python v1.0.2
+ **Cambios importantes**
  + Con esta versión, eliminamos el soporte para crear una máquina virtual desde Azure Machine Learning. Aún puede asociar una máquina virtual de la nube existente o un servidor local remoto. 
  + También eliminamos el soporte para BatchAI, que ahora debe recibirlo a través del proceso de Azure Machine Learning.

+ **Nuevo**
  + Para las canalizaciones de Machine Learning:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Updated**
  + Para las canalizaciones de Machine Learning:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) ahora acepta runconfig
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) ahora se copia en y desde un origen de datos de SQL
    + Programación de la funcionalidad en el SDK para crear y actualizar programaciones a fin de ejecutar canalizaciones publicadas

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>SDK de preparación de datos de Azure Machine Learning v0.5.2
+ **Cambios importantes** 
  * Se ha cambiado el nombre de `SummaryFunction.N` a `SummaryFunction.Count`.
  
+ **Correcciones de errores**
  * Use el token de ejecución de AML al leer desde almacenes de datos de ejecuciones remotas y escribir en estos. Anteriormente, si el token de ejecución de AML se actualizaba en Python, el tiempo de ejecución de preparación de datos no se actualizaba con el token de ejecución de AML actualizado.
  * Mensajes de error más claros adicionales
  * to_spark_dataframe() ya no se bloqueará cuando Spark use la serialización `Kryo`
  * Ahora, el inspector del recuento de valores puede mostrar más de 1000 valores únicos
  * La división aleatoria ya no producirá ningún error si el flujo de datos original no tiene nombre  

+ **Más información**
  * [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Documentos y cuadernos
+ Canalizaciones de Machine Learning
  + Cuadernos nuevos y actualizados para empezar a trabajar con canalizaciones, ámbitos de lote y ejemplos de transferencia del estilo: https://aka.ms/aml-pipeline-notebooks
  + Aprenda a [crear su primera canalización](how-to-create-your-first-pipeline.md)
  + Aprenda a [ejecutar predicciones por lotes mediante canalizaciones](how-to-run-batch-predictions.md)
+ Destino de proceso de Azure Machine Learning
  + Los [cuadernos de ejemplo] (https://aka.ms/aml-notebooks) ahora están actualizados para usar este nuevo proceso administrado.
  + [Obtenga información sobre este proceso](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure Portal: nuevas características
+ Cree y administre los tipos de [proceso de Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute) en el portal.
+ Supervise el uso de cuota y [solicite una cuota](how-to-manage-quotas.md) para el proceso de Azure Machine Learning.
+ Vea el estado del clúster del Proceso de Machine Learning en tiempo real.
+ La compatibilidad con la red virtual se agregó para la creación de Azure Kubernetes Service y el proceso de Azure Machine Learning.
+ Vuelva a ejecutar sus canalizaciones publicadas con los parámetros existentes.
+ Nuevos [gráficos de aprendizaje automático automatizados](how-to-track-experiments.md#auto) para modelos de clasificación (gráfico de importancia de características, elevación, ganancias y calibración con explicabilidad de modelo) y modelos de regresión (gráfico de importancia de características y valores residuales con explicabilidad de modelo). 
+ Las canalizaciones se pueden ver en Azure Portal




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>SDK de Azure Machine Learning para Python v0.1.80

+ **Cambios importantes** 
  * El espacio de nombres *azureml.train.widgets* se ha movido a *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* deja de usar las siguientes clases: *azureml.core.compute.BatchAICompute* y *azureml.core.compute.DSVMCompute*. La última clase se quitará en versiones posteriores. Ahora la clase AmlCompute tiene una definición más fácil. Solo necesita los valores de vm_size y max_nodes, y escala automáticamente su clúster de 0 al valor de max_nodes cuando se envía un trabajo. Nuestros [cuadernos de ejemplo] (https://github.com/Azure/MachineLearningNotebooks/tree/master/training) se actualizaron con esta información y deberían ofrecer ejemplos de uso. Esperamos que disfrute de esta simplificación y de una gran cantidad de características atractivas adicionales en una versión posterior.

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>SDK de preparación de datos de Azure Machine Learning v0.5.1 

Obtenga más información sobre el SDK de preparación de datos mediante la lectura de [documentos de referencia](https://aka.ms/data-prep-sdk).
+ **Nuevas características**
   * Se ha creado una nueva CLI de DataPrep para ejecutar paquetes de DataPrep y ver el perfil de datos de un conjunto de datos o del flujo de datos.
   * Se ha rediseñado la API de SetColumnType para mejorar la facilidad de uso.
   * El nombre del archivo smart_read_file se ha cambiado por auto_read_file.
   * Ahora incluye sesgo y curtosis en el perfil de datos.
   * Puede realizar un muestreo con el muestreo estratificado.
   * Puede leer archivos ZIP que contienen archivos CSV.
   * Puede dividir conjuntos de datos por filas con la división aleatoria (por ejemplo, en conjuntos de prueba-aprendizaje).
   * Puede obtener los tipos de datos de todas las columna de un flujo de datos o un perfil de datos mediante una llamada a `.dtypes`.
   * Puede obtener el recuento de filas de un flujo de datos o un perfil de datos mediante una llamada a `.row_count`.

+ **Correcciones de errores**
   * Se ha corregido la conversión de larga a doble. 
   * Se ha corregido la aserción después de cualquier adición de columna. 
   * Se ha corregido un problema con FuzzyGrouping, en que no se detectaban grupos en algunos casos.
   * Se ha corregido la función de ordenación para respetar el criterio de ordenación de varias columnas.
   * Se han corregido las expresiones and/or para que se gestionen como en `pandas`.
   * Se ha corregido la lectura de la ruta de acceso de dbfs.
   * Se ha simplificado la comprensión de los mensajes de error. 
   * Ya no se produce ningún error al leer en el destino de proceso remoto mediante el token AML.
   * Ya no se produce ningún error en Linux DSVM.
   * Ya no se bloquea cuando los valores que no son de cadena están en predicados de cadena.
   * Ahora controla correctamente los errores de aserción cuando el flujo de datos generaba un error.
   * Ahora es compatible con las ubicaciones de almacenamiento montadas en dbutils en Azure Databricks.

## <a name="2018-11-05"></a>05-11-2018

### <a name="azure-portal"></a>Azure Portal 
Azure Portal para el servicio Azure Machine Learning tiene las siguientes actualizaciones:
  * Una nueva pestaña **Canalizaciones** para las canalizaciones publicadas.
  * Se agregó compatibilidad para adjuntar un clúster de HDInsight existente como un destino de proceso.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>SDK de Azure Machine Learning para Python v0.1.74

+ **Cambios importantes** 
  * *Workspace.compute_targets, almacenes de datos, experimentos, imágenes, modelos y *servicios web* son propiedades en lugar de métodos. Por ejemplo, sustituya *Workspace.compute_targets()* por *Workspace.compute_targets*.
  * *Run.get_context* reemplaza a *Run.get_submitted_run*. El último método se quitará en versiones posteriores.
  * La clase *PipelineData* espera ahora un objeto de almacén de datos como un parámetro en lugar de datastore_name. De forma similar, *Canalización* acepta default_datastore en lugar de default_datastore_name.

+ **Nuevas características:**
  * El [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) de las canalizaciones de Azure Machine Learning ahora usa los pasos de MPI.
  * El widget RunDetails para instancias de Jupyter Notebook se actualiza para mostrar una visualización de la canalización.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>SDK de preparación de datos de Azure Machine Learning v0.4.0 
 
+ **Nuevas características:**
  * Recuento de tipos agregados al perfil de datos. 
  * Recuento de valores e histograma ahora disponibles.
  * Más percentiles en el perfil de datos.
  * La mediana está disponible en el resumen.
  * Ahora se admite Python 3.7.
  * Cuando se guarda un flujo de datos que contiene almacenes de datos en un paquete DataPrep, la información del almacén de datos se guardará como parte del paquete DataPrep.
  * Ahora se admite la escritura en el almacén de datos. 
        
+ **Error corregido**
  * Los desbordamientos de enteros sin signo de 64 bits ahora se controlan correctamente en Linux.
  * Etiqueta de texto incorrecta corregida para archivos de texto sin formato en smart_read.
  * El tipo de columna de cadenas se muestra ahora en la vista de métricas.
  * El recuento de tipos ahora se corrige para mostrar los tipos de valores asignados a un tipo de campo única en lugar de valores individuales.
  * Write_to_csv ya no genera ningún error cuando la ruta de acceso se proporciona como una cadena.
  * Cuando se usa Reemplazar, dejar "Buscar" en blanco no generará ningún error. 

## <a name="2018-10-12"></a>12 de octubre de 2018

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>SDK de Azure Machine Learning para Python v0.1.68

+ **Nuevas características:**
  * Compatibilidad con varios inquilinos al crear una nueva área de trabajo.

+ **Errores corregidos**
  * Ya no es necesario anclar la versión de la biblioteca pynacl durante la implementación del servicio web.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>SDK de preparación de datos de Azure Machine Learning v0.3.0

+ **Nuevas características:**
  * Se ha agregado el método transform_partition_with_file(script_path), lo que permite a los usuarios usar la ruta de acceso del archivo de Python que va a ejecutar

## <a name="2018-10-01"></a>01 de octubre de 2018

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>SDK de Azure Machine Learning para Python v0.1.65
La [versión 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) incluye nuevas características, más documentación, correcciones de los errores y más [cuadernos de ejemplo](https://aka.ms/aml-notebooks).

Para obtener información acerca de errores conocidos y soluciones alternativas, consulte [la lista de problemas conocidos](resource-known-issues.md).

+ **Cambios importantes**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services devuelven una lista de diccionarios devuelto que se ha devuelto previamente. Consulte la documentación de la API [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py).

  * Automated Machine Learning ha eliminado un error de la media cuadrática normalizada de la métrica principal.

+ **HyperDrive**
  * Varias correcciones de errores de HyperDrive para obtener mejoras bayesianas del rendimiento para la obtención de llamadas a métricas. 
  * Actualización de Tensorflow 1.9 a 1.10 
  * Optimización de la imagen de Docker para el arranque en frío. 
  * Los trabajos ahora informan acerca del estado correcto, aunque salgan con un código de error que no sea 0. 
  * Validación del atributo RunConfig en SDK. 
  * El objeto de ejecución de HyperDrive admite una cancelación similar a la de una ejecución normal: no es preciso usar parámetros. 
  * Mejoras en los widgets para mantener el estado de los valores de la lista desplegable de ejecuciones distribuidas y ejecuciones de HyperDrive. 
  * Se ha corregido el soporte de TensorBoard y otros archivos de registro para el servidor del parámetro. 
  * Compatibilidad con Intel (R) MPI en el lado del servicio. 
  * Corrección de errores en el ajuste de parámetros para la corrección de ejecuciones distribuidas durante la validación de BatchAI. 
  * El administrador de contextos ahora identifica la instancia principal. 

+ **Experiencia de Azure Portal**
  * log_table() y log_row() se admiten en los detalles de la ejecución. 
  * Creación automáticamente de gráficos para tablas y filas con 1, 2 o 3 columnas numéricas y una columna de categorías opcional.

+ **Automated Machine Learning**
  * Mejora en la control de errores y la documentación 
  * Se han corregido los problemas de rendimiento de la recuperación de la propiedad de ejecución. 
  * Se ha corregido el problema de la ejecución continua. 
  * Se han corregido los problemas de iteración del ensamblado.
  * Se ha corregido el error de falta de respuesta del aprendizaje en MAC OS.
  * Reducción de la curva de PR/ROC media de macros en un escenario de validación personalizada.
  * Se ha eliminado la lógica de índices adicional.
  * Se ha eliminado el filtro de la API get_output.

+ **Procesos**
  * Agrega un método Pipeline.publish() para publicar una canalización directamente, sin necesidad de que antes se realice una ejecución.   
  * Se ha agregado un método PipelineRun.get_pipeline_runs() para capturar las ejecuciones de la canalización que se generaron desde una canalización publicada.

+ **Project Brainwave**
  * Compatibilidad actualizada con los nuevos modelos de IA disponibles en FPGA.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>SDK de preparación de datos de Azure Machine Learning v0.2.0
La [versión 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) incluye las siguientes características y correcciones de errores:

+ **Nuevas características:**
  * Compatibilidad con la codificación "one-hot"
  * Compatibilidad con la transformación de cuantiles
   
+ **Error corregido:**
  * Funciona con cualquier versión de Tornado sin necesidad de bajar de versión de Tornado
  * Recuentos de valores para todos los valores, no solo para los tres primeros

## <a name="2018-09-public-preview-refresh"></a>2018-09 (Actualización de la versión preliminar pública)

Una versión nueva y actualizada de Azure Machine Learning: obtenga más información sobre esta versión: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Pasos siguientes

Consulte la introducción a [Azure Machine Learning Service](../service/overview-what-is-azure-ml.md).
