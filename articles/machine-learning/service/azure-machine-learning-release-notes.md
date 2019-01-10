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
ms.openlocfilehash: 8a67a20beff306cd23b08e1d651ab5dc4c779fd2
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742769"
---
# <a name="azure-machine-learning-service-release-notes"></a>Notas de la versión del servicio Azure Machine Learning

En este artículo obtendrá información acerca de las versiones del servicio Azure Machine Learning. 

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

## <a name="older-notes-sept-2017---jun-2018"></a>Notas anteriores: septiembre de 2017 a junio de 2018
### <a name="2018-05-sprint-5"></a>2018-05 (Sprint 5)

Con esta versión de Azure Machine Learning, puede:
+ Caracterizar imágenes con una versión cuantificada de ResNet 50, entrenar un clasificador en función de dichas características e [implementar dicho modelo en un FPGA en Azure](../service/how-to-deploy-fpga-web-service.md) de inferencia de latencia ultrabaja.

+ Compilar e implementar rápidamente aprendizaje automático de alta precisión y modelos de aprendizaje profundo con [paquetes personalizados de Azure Machine Learning](../desktop-workbench/reference-python-package-overview.md).

### <a name="2018-03-sprint-4"></a>2018-03 (Sprint 4)
**Número de versión**: 0.1.1801.24353  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([busque su versión](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Muchas de las actualizaciones siguientes son consecuencia directa de esos comentarios. ¡No deje de enviarlos!

**Nuevas características y cambios destacables**

- Compatibilidad para ejecutar los scripts en máquinas virtuales de Ubuntu remotas de forma nativa en su propio entorno, además la ejecución basada en docker remoto.
- La nueva experiencia de entorno en la aplicación Workbench le permite crear destinos de proceso y ejecutar configuraciones además de la experiencia basada en la CLI.
![Pestaña Entornos](media/azure-machine-learning-release-notes/environment-page.png)
- Informes de historial de ejecución personalizables ![Imagen de los nuevos informes de historial de ejecución](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**Actualizaciones detalladas**

A continuación figura una lista de actualizaciones detalladas de cada área de componente de Azure Machine Learning en esta versión de sprint.

#### <a name="workbench-ui"></a>Interfaz de usuario de Workbench
- Informes de historial de ejecución personalizables
  - Configuración de gráficos mejorada para los informes de historial de ejecución
    - Se pueden cambiar los puntos de entrada utilizados.
    - Se pueden agregar y modificar filtros de nivel superior. ![Agregar filtros](media/azure-machine-learning-release-notes/add-filters.jpg).
    - Se pueden agregar o modificar gráficos y estadísticas (y reorganizarlos mediante arrastrar y colocar).
    ![Creación de nuevos gráficos](media/azure-machine-learning-release-notes/configure-charts.png)

  - CRUD para informes de historial de ejecución
  - Se han movido todos los archivos de configuración de las vistas de listas de historial de ejecución existentes a los informes del lado servidor, que actúan como canalizaciones en las ejecuciones desde los puntos de entrada seleccionados.

- Pestaña Entornos
  - Agregue fácilmente nuevos archivos de destino de proceso y de archivos de configuración de ejecución en el proyecto ![Nuevo destino de proceso](media/azure-machine-learning-release-notes/add-new-environments.png).
  - Administre y actualice los archivos de configuración con la experiencia del usuario simple, basada en formularios
  - Nuevo botón para preparar los entornos para ejecución

- Mejoras de rendimiento en la lista de archivos de la barra lateral

#### <a name="data-preparation"></a>Preparación de los datos 
- Azure Machine Learning Workbench ahora le permite buscar una columna mediante el nombre de una columna conocida.


#### <a name="experimentation"></a>Experimentación
- Azure Machine Learning Workbench admite ahora la ejecución de los scripts de forma nativa en su propio entorno de python o pyspark. Para esta funcionalidad, el usuario crea y administra su propio entorno en la máquina virtual remota y utiliza Azure Machine Learning Workbench para ejecutar sus scripts en dicho destino. Consulte [Configuración del servicio Experimentación de Azure Machine Learning](../desktop-workbench/experimentation-service-configuration.md) 

#### <a name="model-management"></a>Administración de modelos
- Compatibilidad con la personalización de los contenedores implementados: permite personalizar la imagen del contenedor al permitir la instalación de bibliotecas externas mediante apt get, por ejemplo. Ya no se limita a las bibliotecas en las que se puede instalar PIP. Consulte la [documentación](../desktop-workbench/model-management-custom-container.md) para más información.
  - Utilice la marca `--docker-file myDockerStepsFilename` y el nombre de archivo con los comandos de creación de manifiesto, imagen o servicio.
  - La imagen base es Ubuntu y no se puede modificar.
  - Comando de ejemplo: 
  
    ```shell
    $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
    ```

### <a name="2018-01-sprint-3"></a>2018-01 (Sprint 3) 
**Número de versión**: 0.1.1712.18263  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([busque su versión](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

A continuación, encontrará las actualizaciones y mejoras de este sprint. Muchas de las actualizaciones descritas aquí son consecuencia directa de los comentarios de los usuarios. 

### <a name="2017-12-sprint-2"></a>2017-12 (Sprint 2)
**Número de versión**: 0.1.1711.15263  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([busque su versión](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Esta versión es la tercera actualización de Azure Machine Learning. Esta actualización incluye mejoras en la aplicación de workbench, la interfaz de línea de comandos (CLI) y los servicios back-end. Le agradecemos enormemente sus comentarios, tanto positivos como negativos. Muchas de las actualizaciones siguientes son consecuencia directa de esos comentarios. 

### <a name="2017-11-sprint-1"></a>2017-11 (Sprint 1) 
**Número de versión**: 0.1.1710.31013  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([busque su versión](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

En esta versión, hemos realizado mejoras de seguridad, estabilidad y mantenimiento en la aplicación Workbench, en la CLI y en el nivel de servicios back-end. 

### <a name="2017-10-sprint-0"></a>2017-10 (Sprint 0) 
**Número de versión**: 0.1.1710.31013  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;([busque su versión](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Esta versión es la primera actualización de Azure Machine Learning Workbench después de la versión preliminar pública inicial en la conferencia de Microsoft Ignite 2017. Las actualizaciones principales en esta versión son correcciones para mejorar la confiabilidad y la estabilidad. 

## <a name="next-steps"></a>Pasos siguientes

Consulte la introducción de [Azure Machine Learning](../service/overview-what-is-azure-ml.md).
