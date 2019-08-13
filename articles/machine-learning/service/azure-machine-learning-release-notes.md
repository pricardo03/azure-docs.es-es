---
title: Novedades de la versión
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre las actualizaciones más recientes a Azure Machine Learning Service y los SDK de Python para el aprendizaje automático y la preparación de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jmartens
author: j-martens
ms.date: 07/25/2019
ms.custom: seodec18
ms.openlocfilehash: ade107f51fabb133e8e4046bf645f4dff284102b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565103"
---
# <a name="azure-machine-learning-service-release-notes"></a>Notas de la versión del servicio Azure Machine Learning

En este artículo obtendrá información acerca de las versiones del servicio Azure Machine Learning.  Para obtener el contenido completo de referencia del SDK, visite la página de referencia del [**SDK principal para Python**](https://aka.ms/aml-sdk) de Azure Machine Learning.

Para obtener información acerca de errores conocidos y soluciones alternativas, consulte [la lista de problemas conocidos](resource-known-issues.md).

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>SDK de Azure Machine Learning para Python v1.0.53

+ **Nuevas características:**
  + Automated Machine Learning ahora admite modelos de ONNX de entrenamiento en el destino de proceso remoto
  + Ahora Azure Machine Learning proporciona la capacidad de reanudar el entrenamiento de una ejecución anterior, punto de comprobación o archivos de modelo.
    + Obtenga información sobre [cómo usar los estimadores para reanudar el entrenamiento desde una ejecución anterior](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb)

+ **Mejoras y correcciones de errores**
  + **automl-client-core-nativeclient**
    + Se corrige el error sobre la pérdida de tipos de columnas después de la transformación (error vinculado). 
    + Se permite que y_query sea un tipo de objeto que contiene None(s) al principio (#459519).
  + **azure-cli-ml**
    + Los comandos de la CLI de implementación de modelos y de actualización de servicio ahora aceptan parámetros, archivos de configuración o una combinación de ambos. Los parámetros tienen prioridad sobre los atributos en los archivos.
    + La descripción del modelo ahora se puede actualizar después del registro.
  + **azureml-automl-core**
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
    + Se agrega compatibilidad con los estimadores y canalizaciones de Nimbus ML que se van a usar dentro de los estimadores de AutoML.
    + Se corrige el error en el procedimiento de selección de conjuntos que hacía aumentar innecesariamente el conjunto resultante aunque las puntuaciones permanecieran constantes.
    + Se habilita la reutilización de algunas características en las divisiones de CV para las tareas de previsión. De este modo, se acelera la ejecución del programa de instalación aproximadamente en un factor de n_cross_validations para las características caras, como las ventanas con desplazamiento y retraso.
    + Se soluciona un problema si el tiempo está fuera del intervalo de tiempo admitido de pandas. Ahora se genera una excepción DataException si el tiempo es menor que pd.Timestamp.min o mayor que pd.Timestamp.max.
    + La previsión permite ahora frecuencias diferentes en los conjuntos de entrenamiento y de prueba si se pueden alinear. Por ejemplo, "quarterly starting in January" y "quarterly starting in October" se pueden alinear.
    + La propiedad "parameters" se ha agregado a TimeSeriesTransformer.
    + Se quitan las clases de excepción antiguas.
    + En las tareas de previsión, el parámetro `target_lags` ahora acepta un valor entero único o una lista de enteros. Si se proporcionó el entero, solo se creará un retardo. Si se proporciona una lista, se tomarán los valores únicos de los retardos. target_lags=[1, 2, 2, 4] creará retardos de uno, dos y cuatro períodos.
    + Se corrige el error sobre la pérdida de tipos de columnas después de la transformación (error vinculado).
    + En `model.forecast(X, y_query)` se permite que y_query sea un tipo de objeto que contiene None(s) al principio (#459519).
    + Se agregan los valores esperados a la salida de automl.
  + **azureml-contrib-datadrift**
    +  Se incluyen mejoras en el cuaderno de ejemplo, incluido el cambio a azureml-opendatasets en lugar de azureml-contrib-opendatasets y mejoras de rendimiento al enriquecer datos.
  + **azureml-contrib-explain-model**
    + Se corrige el argumento de transformaciones para el explicador LIME con respecto a la importancia de la característica sin formato en el paquete azureml-contrib-explain-model.
    + Se han agregado segmentaciones a las explicaciones de imágenes en el explicador de imágenes del paquete AzureML-contrib-explain-model.
    + Se agrega compatibilidad dispersa con SciPy para LimeExplainer.
    + Se agrega batch_size para imitar el explicador cuando include_local=False para las explicaciones globales de streaming en los lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel.
  + **azureml-contrib-featureengineering**
    + Se corrige para llamar a set_featurizer_timeseries_params(): el cambio de tipo de valor dict y null check. Se agrega un cuaderno para el caracterizador timeseries.
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
  + **azureml-core**
    + Se agrega la capacidad de asociar almacenes de datos DBFS en la CLI de AzureML. 
    + Se corrige el error con la carga del almacén de datos en que se creaba una carpeta vacía si `target_path` se iniciaba con `/`.
    + Se corrige el problema de deepcopy en ServicePrincipalAuthentication.
    + Se han agregado los comandos "az ml environment show" y "az ml environment list" a la CLI.
    + Los entornos ahora admiten la especificación de un base_dockerfile como alternativa a una base_image ya creada.
    + La configuración de RunConfiguration sin usar en auto_prepare_environment se ha marcado como en desuso.
    + La descripción del modelo ahora se puede actualizar después del registro.
    + Corrección de los errores: Ahora, la eliminación del modelo y la imagen proporciona más información sobre la recuperación de objetos ascendentes que dependen de ellos si la eliminación produce un error debido a una dependencia ascendente.
    + Se corrige un error que imprimía la duración en blanco para las implementaciones que se producen al crear un área de trabajo para algunos entornos.
    + Se han mejorado las excepciones de error de la creación de áreas de trabajo. Así, los usuarios no verán el mensaje "Unable to create workspace. Unable to find... " (No se puede crear el área de trabajo. No se puede encontrar.) y, en su lugar, verán el error de creación real.
    + Se agrega compatibilidad con la autenticación de token en los servicios web de AKS. 
    + Agregue el método `get_token()` a los objetos `Webservice`.
    + Se ha agregado compatibilidad con la CLI para administrar conjuntos de datos de aprendizaje automático.
    + `Datastore.register_azure_blob_container` ahora, opcionalmente, acepta un valor `blob_cache_timeout` (en segundos) que configura los parámetros de montaje de blobfuse para habilitar la expiración de la memoria caché para este almacén de datos. El valor predeterminado es sin tiempo de espera, es decir, cuando se lea un blob, permanecerá en la memoria caché local hasta que el trabajo finalice. La mayoría de los trabajos preferirán esta configuración, pero algunos trabajos necesitan leer más datos de un conjunto de datos grande que quepa en sus nodos. Para estos trabajos, el ajuste de este parámetro les ayudará a funcionar correctamente. Tenga cuidado al optimizar este parámetro: si establece un valor demasiado bajo, puede producirse un rendimiento deficiente, ya que los datos que se usan en una época pueden expirar antes de volver a usarse. Esto significa que todas las lecturas se realizarán desde el almacenamiento de blobs (es decir, la red) en lugar de la memoria caché local, lo que afecta desfavorablemente a los tiempos de entrenamiento.
    + La descripción del modelo ahora se puede actualizar correctamente después del registro.
    + La eliminación de modelos e imágenes ahora proporciona más información sobre los objetos de nivel superior que dependen de ellos, lo que provoca un error en la eliminación.
    + Se mejora la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
  + **azureml-explain-model**
    + Se corrige el argumento de transformaciones para el explicador LIME con respecto a la importancia de la característica sin formato en el paquete azureml-contrib-explain-model.
    + Se agrega compatibilidad dispersa con SciPy para LimeExplainer.
    + Se agrega el contenedor de explicación lineal, así como otro nivel de explicación tabular para explicar los modelos lineales.
    + Para imitar la explicación en la biblioteca de modelos de explicación, se corrige el error cuando include_local=False para la entrada de datos dispersos.
    + Se agregan los valores esperados a la salida de automl.
    + Se corrige la importancia de la característica de permutación cuando se proporcionaba el argumento de transformaciones para obtener la importancia de la característica sin procesar.
    + Se agrega batch_size para imitar el explicador cuando include_local=False para las explicaciones globales de streaming en los lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel.
    + Para la biblioteca de explicación de modelos, se corrigieron las explicaciones de la caja negra en los que la entrada de trama de datos de Pandas es necesaria para la predicción.
    + Se corrige el error por el que `explanation.expected_values` a veces devolvería un valor float en lugar de una lista con dicho valor incluido.
  + **azureml-mlflow**
    + Se mejora el rendimiento de mlflow.set_experiment(experiment_name).
    + Se corrige el error en el uso de InteractiveLoginAuthentication para tracking_uri de mlflow.
    + Se mejora la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
    + Se mejora la documentación del paquete azureml-mlflow.
    + Se corrige el error que hacía que mlflow.log_artifacts("my_dir") guardara artefactos en "my_dir/artifact-paths" en lugar de "artifact-paths".
  + **azureml-opendatasets**
    + Se ancla pyarrow de opendatasets para las versiones anteriores (<0.14.0) debido al problema de memoria reciente.
    +  Se mueve azureml-contrib-opendatasets a azureml-opendatasets. - Se permite que las clases del conjunto de datos abierto se registren en el área de trabajo de AML y aprovechen sin problemas las funcionalidades del conjunto de datos de AML. - Se mejora significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-pipeline-steps**
    + El almacén de datos DBFS se admite ahora en las entradas y salidas de DatabricksStep.
    + Se actualiza la documentación para Azure Batch Step con respecto a las entradas y salidas.
    + En AzureBatchStep, se cambió el valor predeterminado de *delete_batch_job_after_finish* a *true*.
  + **azureml-telemetry**
    +  Se mueve azureml-contrib-opendatasets a azureml-opendatasets. - Se permite que las clases del conjunto de datos abierto se registren en el área de trabajo de AML y aprovechen sin problemas las funcionalidades del conjunto de datos de AML. - Se mejora significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-train-automl**
    + Se actualiza la documentación sobre get_output para reflejar el tipo de valor devuelto real y proporcionar notas adicionales sobre cómo recuperar las propiedades de clave.
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
    + Se agregan los valores esperados a la salida de automl.
  + **azureml-train-core**
    + Ahora se aceptan cadenas como destino de proceso para la optimización automática de hiperparámetros.
    + La configuración de RunConfiguration sin usar en auto_prepare_environment se ha marcado como en desuso.

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>SDK de preparación de datos de Azure Machine Learning v1.1.9

+ **Nuevas características:**
  + Se agrega compatibilidad para leer un archivo directamente desde una dirección URL HTTP o HTTPS.

+ **Mejoras y correcciones de errores**
  + Se mejora el mensaje de error al intentar leer un conjunto de datos Parquet de un origen remoto (que no se admite actualmente).
  + Se corrige un error al escribir en el formato de archivo Parquet en ADLS Gen 2 y al actualizar el nombre del contenedor ADLS Gen 2 en la ruta de acceso.

## <a name="2019-07-09"></a>09/07/2019

### <a name="visual-interface"></a>Interfaz visual
+ **Características en versión preliminar**
  + Se ha agregado el módulo "Ekecutar script R" en la interfaz visual.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>SDK de Azure Machine Learning para Python v1.0.48

+ **Nuevas características:**
  + **azureml-opendatasets**
    + **azureml-contrib-opendatasets** ahora está disponible como **azureml-opendatasets**. El paquete antiguo puede seguir funcionando, pero le recomendamos que use **azureml-opendatasets** de ahora en adelante para obtener mejoras y funcionalidades más avanzadas.
    + Este nuevo paquete le permite registrar conjuntos de datos abiertos como un conjunto de datos en el área de trabajo de AML y aprovechar las funcionalidades que ofrece dicho conjunto de datos.
    + También incluye funcionalidades existentes, por ejemplo, el consumo de conjuntos de datos abiertos como dataframes de SPARK o Pandas y combinaciones de ubicaciones para algún conjunto de datos como el tiempo.

+ **Características en versión preliminar**
    + Ahora, HyperDriveConfig puede aceptar el objeto de canalización como un parámetro a fin de admitir la optimización de hiperparámetros mediante una canalización.

+ **Mejoras y correcciones de errores**
  + **azureml-train-automl**
    + Se corrigió el error sobre la pérdida de tipos de columnas después de la transformación.
    + Se corrigió el error para permitir que y_query sea un tipo de objeto que contiene None(s) al principio. 
    + Se corrigió el problema en el que el procedimiento de selección de conjuntos hacía aumentar innecesariamente el conjunto resultante aunque las puntuaciones permanecieran constantes.
    + Se corrigió el problema con la configuración whitelist_models y blacklist_models en AutoMLStep.
    + Se corrigió el problema que impedía el uso del preprocesamiento cuando AutoML se habría usado en el contexto de las canalizaciones de Azure ML.
  + **azureml-opendatasets**
    + Se movió azureml-contrib-opendatasets a azureml-opendatasets.
    + Se permitió que las clases del conjunto de datos abierto se registren en el área de trabajo de AML y aprovechen sin problemas las funcionalidades del conjunto de datos de AML.
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-explain-model**
    + Se actualizó la documentación en línea sobre los objetos de interpretabilidad.
    + Se agregó batch_size para imitar la explicación cuando include_local = False para las explicaciones globales de streaming de los lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel.
    + Se corrigió el problema por el que `explanation.expected_values` a veces devolvería un valor float en lugar de una lista con dicho valor incluido.
    + Se agregaron los valores esperados en la salida automl para la explicación de imitación en la biblioteca de modelos de explicación.
    + Se corrigió la importancia de la característica de permutación cuando se proporcionaba el argumento de transformaciones para obtener la importancia de la característica sin procesar.
    + Se agregó batch_size para imitar la explicación cuando include_local = False para las explicaciones globales de streaming de los lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel para la biblioteca de explicación del modelo.
  + **azureml-core**
    + Se agregó la capacidad de asociar almacenes de datos DBFS en la CLI de AzureML.
    + Se corrigió el problema con la carga del almacén de datos en que se creaba una carpeta vacía si `target_path` se iniciaba con `/`.
    + Se habilitó la comparación de dos conjuntos de datos:
    + Ahora, la eliminación del modelo y la imagen proporciona más información sobre la recuperación de objetos ascendentes que dependen de ellos si la eliminación produce un error debido a una dependencia ascendente.
    + Se dejó de usar la configuración de RunConfiguration sin usar en auto_prepare_environment.
  + **azureml-mlflow**
    + Se mejoró la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
    + Se mejoró la documentación del paquete azureml-mlflow.
    + Se corrigió el problema que hacía que mlflow.log_artifacts("my_dir") guardara artefactos en "my_dir/artifact-paths" en lugar de "artifact-paths".
  + **azureml-pipeline-core**
    + El parámetro hash_paths de todos los pasos de la canalización está en desuso y se quitará en el futuro. De forma predeterminada, se aplica hash al contenido de source_directory (excepto en los archivos que se enumeran en .amlignore o .gitignore).
    + Se seguirán mejorando los valores Module y ModuleStep para admitir módulos específicos de tipo de proceso, en la preparación para la integración de RunConfiguration y otros cambios para desbloquear su uso en canalizaciones.
  + **azureml-pipeline-steps**
    + AzureBatchStep: Se mejoró la documentación con respecto a las entradas y salidas.
    + AzureBatchStep: Se cambió el valor predeterminado de delete_batch_job_after_finish a true.
  + **azureml-train-core**
    + Ahora se aceptan cadenas como destino de proceso para la optimización automática de hiperparámetros.
    + Se dejó de usar la configuración de RunConfiguration sin usar en auto_prepare_environment.
    + Se dejaron de usar los parámetros `conda_dependencies_file_path` y `pip_requirements_file_path` para usar `conda_dependencies_file` y `pip_requirements_file`, respectivamente.
  + **azureml-opendatasets**
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>SDK de preparación de datos de Azure Machine Learning v1.1.8

+ **Nuevas características:**
 + Ahora se puede procesar una iteración en los objetos de flujo de datos, lo que genera una secuencia de registros. Consulte la documentación de `Dataflow.to_record_iterator`.

+ **Mejoras y correcciones de errores**
 + Se aumentó la solidez del SDK DataPrep.
 + Se mejoró el control de los DataFrames de Pandas con índices de columna que no son cadenas.
 + Se mejoró el rendimiento de `to_pandas_dataframe` en los conjuntos de datos.
 + Se corrigió un error en que no se podía completar la ejecución de Spark de los conjuntos de datos cuando se realizaba en un entorno de varios nodos.

## <a name="2019-07-01"></a>2019-07-01

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>SDK de preparación de datos de Azure Machine Learning v1.1.7

Hemos revertido un cambio que mejoraba el rendimiento, ya que provocaba problemas a algunos clientes que usaban Azure Databricks. Si ha tenido algún problema en Azure Databricks, puede actualizar a la versión 1.1.7, para lo que puede seguir cualquiera de estos métodos:
1. Ejecute este script para realizar la actualización: `%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. Vuelva a crear el clúster, que instalará la versión más reciente del SDK de preparación de datos.

## <a name="2019-06-25"></a>2019-06-25

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>SDK de Azure Machine Learning para Python v1.0.45

+ **Nuevas características:**
  + Se ha agregado un modelo suplente del árbol de decisión a la explicación de imitación en el paquete azureml-explain-model
  + Capacidad de especificar una versión de CUDA que se va instalar en imágenes de inferencia. Compatibilidad con CUDA 9.0, 9.1 y 10.0.
  + La información acerca de las imágenes base del entrenamiento de Azure Machine Learning ya está disponible en el [repositorio de GitHub de contenedores de Azure Machine Learning](https://github.com/Azure/AzureML-Containers) y [DockerHub](https://hub.docker.com/_/microsoft-azureml)
  + Se ha agregado compatibilidad de la CLI con la programación de canalizaciones. Para más información, ejecute "az ml pipeline -h"
  + Se ha agregado el parámetro del espacio de nombres de Kubernetes personalizado a la configuración de implementación del servicio Web AKS y a la CLI.
  + El parámetro hash_paths está en desuso en todos los pasos de las canalizaciones
  + Model.register ahora admite el registro de varios archivos individuales como un único modelo con el uso del parámetro `child_paths`.
  
+ **Características en versión preliminar**
    + Ahora, las explicaciones de la puntuación pueden guardar información de conda y pip para que la serialización y deserialización sean más confiables.
    + Corrección de errores en el selector automático de características.
    + Mlflow.azureml.build_image actualizado a la nueva API, los errores revisados se han expuesto en la nueva implementación.

+ **Mejoras y correcciones de errores**
  + Se ha quitado la dependencia de Paramiko de azureml-core. Se han agregado advertencias de desuso para los métodos para adjuntar destinos de proceso heredados.
  + Mejora del rendimiento de run.create_children
  + En la explicación de imitación con clasificador binario, se ha corregido el orden de las probabilidades cuando se usa la probabilidad del profesor para escalar los valores Shap
  + Se ha mejorado el control de errores y los mensajes para el aprendizaje automático automatizado. 
  + Se ha corregido el problema de tiempo de expiración de iteraciones para el aprendizaje automático automatizado.
  + Ha mejorado el rendimiento de la transformación de las series temporales para el aprendizaje automático automatizado.

## <a name="2019-06-24"></a>2019-06-24

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>SDK de preparación de datos de Azure Machine Learning v1.1.6

+ **Nuevas características:**
  + Se han agregado funciones de resumen tanto a los valores superiores (`SummaryFunction.TOPVALUES`) como a los inferiores (`SummaryFunction.BOTTOMVALUES`).

+ **Mejoras y correcciones de errores**
  + Se ha mejorado considerablemente el rendimiento de `read_pandas_dataframe`.
  + Se ha corregido un error que provocaría que `get_profile()` en un flujo de datos que apunte a archivos binarios no funcione.
  + Se ha expuesto `set_diagnostics_collection()` para permitir la habilitación y deshabilitación por programación de la colección de telemetría.
  + Ha cambiado el comportamiento de `get_profile()`. Los valores NaN ahora se omiten ahora en Min, Mean, Std y Sum, lo que se alinea con el comportamiento de Pandas.


## <a name="2019-06-10"></a>2019-06-10

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>SDK de Azure Machine Learning para Python v1.0.43

+ **Nuevas características:**
  + Azure Machine Learning ahora proporciona soporte técnico de primera clase para el popular marco de análisis de datos y aprendizaje automático Scikit-learn. Mediante un [ `SKLearn` estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py), los usuarios pueden entrenar e implementar fácilmente modelos de Scikit-learn.
    + Aprenda a [ejecutar el ajuste de hiperparámetros con Scikit-learn mediante HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb).
  + Se ha agregado compatibilidad para crear ModuleStep en las canalizaciones junto con las clases Module y ModuleVersion para administrar unidades de proceso reutilizables.
  + Los servicios web ACI ahora admiten scoring_uri persistente a través de actualizaciones. scoring_uri cambiará de IP a nombre de dominio completo. La etiqueta de nombre DNS del nombre de dominio completo se puede configurar estableciendo dns_name_label en deploy_configuration. 
  + Nuevas características del aprendizaje automático automatizado:
    + Administrador de características de STL para la previsión
    + La agrupación en clústeres de KMeans está habilitada para el barrido de características
  + Se han agilizado las aprobaciones de las cuotas de AmlCompute. Se ha automatizado el proceso para aprobar las solicitudes de cuota dentro de unos márgenes. Para más información acerca del funcionamiento de las cuotas, aprenda a[administrar las cuotas](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

+ **Características en versión preliminar**
    + Integración con el seguimiento de [MLflow](https://mlflow.org) 1.0.0 a través del paquete azureml mlflow ([cuadernos de ejemplo](https://aka.ms/azureml-mlflow-examples)).
    + Envío de Jupyter Notebook como una ejecución. [Documentación de referencia de API](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Versión preliminar pública del [detector de desfase de datos](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift?view=azure-ml-py) a través del paquete azureml-contrib-datadrift ([cuadernos de ejemplo](https://aka.ms/azureml-datadrift-example)). El desfase de datos es una de las principales razones por las que la precisión del modelo empeora con el tiempo. Se produce cuando los datos que se sirven a un modelo en producción no coinciden con los que se usaron para entrenar dicho modelo. El detector de desfase de datos de Azure Machine Learning ayuda a los clientes a supervisar el desfase de datos y envía una alerta cada vez que se detecta un desfase. 

+ **Cambios importantes**

+ **Mejoras y correcciones de errores**
  + La carga y almacenamiento de RunConfiguration admite la especificación de una ruta de acceso completa al archivo con total compatibilidad con el comportamiento anterior.
  + Se ha agregado almacenamiento en caché en ServicePrincipalAuthentication, que estaba desactivado de forma predeterminada.
  + Habilitar el registro de varios trazados en el mismo nombre de métrica.
  + La clase del modelo ahora se puede importar correctamente de azureml.core (`from azureml.core import Model`).
  + En los pasos de la canalización, el parámetro `hash_path` está en desuso. El nuevo comportamiento es hacer hash a todo source_directory, excepto a los archivos que aparecen en .amlignore o .gitignore a.
  + En los paquetes de la canalización, varios métodos `get_all` y `get_all_*` han quedado en desuso en favor de `list` y `list_*`, respectivamente.
  + azureml.core.get_run ya no requiere que se importen clases antes de volver al tipo de ejecución original.
  + Se ha corregido el problema por el que algunas llamadas a WebService Update no desencadenaban una actualización.
  + El tiempo de espera de la puntuación en los servicios web de AKS debe oscilar entre 5 ms y 300 000 ms. El valor máximo permitido de scoring_timeout_ms para las solicitudes de anotación pasa de 1 a 5 minutos.
  + Los objetos de LocalWebservice ahora tienen las propiedades `scoring_uri` y `swagger_uri`.
  + La creación del directorio de salidas y la carga de directorio de salidas se ha sacado del proceso del usuario. Se ha habilitado el SDK del historial de ejecución para que se ejecute en todos los procesos de usuario. Esta acción debería resolver algunos problemas de sincronización experimentados por las ejecuciones del entrenamiento distribuido.
  + El nombre del registro de azureml escrito desde el nombre del proceso del usuario ahora incluirá el nombre del proceso (solo para el entrenamiento distribuido) y el PID.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>SDK de preparación de datos de Azure Machine Learning v1.1.5

+ **Mejoras y correcciones de errores**
  + Para los valores de fecha y hora interpretados que tengan un formato de año de 2 dígitos, el rango de años válidos se ha actualizado para que coincida con la versión de mayo de Windows. Este rango se ha cambiado de 1930-2029 a 1950-2049.
  + Si se leen en un archivo y se establecen en `handleQuotedLineBreaks=True`, `\r` se tratará como una línea nueva.
  + Se ha corregido un error que provocó algunos errores en `read_pandas_dataframe`.
  + Rendimiento mejorado de `get_profile`.
  + Han mejoraron los mensajes de error.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>SDK de preparación de datos de Azure Machine Learning v1.1.4

+ **Nuevas características:**
  + Ahora puede usar las siguientes funciones del lenguaje de expresiones para extraer y analizar los valores de fecha y hora en nuevas columnas.
    + `RegEx.extract_record()` extrae los elementos de fecha y hora en una nueva columna.
    + `create_datetime()` crea objetos DateTime a partir de elementos de fecha y hora independientes.
  + Al llamar a `get_profile()`, ahora puede ver que las columnas de cuantil se etiquetan como (est.), con el fin de indicar que los valores son aproximaciones.
  + Ahora puede usar los comodines ** al leer desde Azure Blob Storage.
    + p. ej. `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Correcciones de errores**
  + Se ha corregido un error relacionado con la lectura de un archivo Parquet desde un origen remoto (Blob de Azure).

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>SDK de Azure Machine Learning para Python v1.0.39
+ **Cambios**
  + La ejecución de la opción auto_prepare_environment de configuración está en desuso y la preparación automática es el valor predeterminado.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>SDK de preparación de datos de Azure Machine Learning v1.1.3

+ **Nuevas características:**
  + Se agregó compatibilidad para leer desde una base de datos de PostgresSQL, ya sea mediante una llamada a read_postgresql o mediante un almacén de datos.
    + Puede ver ejemplos en las guías de procedimientos:
      + [Cuaderno de ingesta de datos](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Cuaderno de almacén de datos](https://aka.ms/aml-data-prep-datastore-nb)

+ **Mejoras y correcciones de errores**
  + Se han corregido problemas de conversión de tipos de columna:
  + Ahora una columna numérica o booleana se convierte correctamente en una columna booleana.
  + Ahora no se producirá un error al intentar establecer que una columna de fecha sea del tipo de fecha.
  + De ha mejorado los tipos de JoinType y la documentación de referencia que los acompaña. Al unir dos flujos de datos, ahora puede especificar uno de estos tipos de combinación:
    + NONE, MATCH, INNER, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, RIGHTANTI, RIGHTOUTER, FULLANTI, FULL.
  + Mejora en la inferencia de los tipos de datos para reconocer más formatos de datos.

## <a name="2019-05-06"></a>06-05-2019

### <a name="azure-portal"></a>Portal de Azure

En Azure Portal, ahora puede:
+ Crear y ejecutar experimentos de Azure Machine Learning automatizados 
+ Crear una máquina virtual de Jupyter Notebook para probar tanto cuadernos de Jupyter Notebook de ejemplo como el suyo propio.
+ Una nueva sección, Creación (versión preliminar), en el área de trabajo del servicio Azure Machine Learning Service, que incluye las máquinas virtuales de Aprendizaje automático automatizado, Interfaz visual y Cuaderno hospedado
    + Creación automática de un modelo mediante aprendizaje automático automatizado 
    + Uso de una interfaz visual de arrastrar y colocar para ejecutar experimentos
    + Creación de una máquina virtual de Notebook para explorar datos, crear modelos e implementar servicios.
+ Gráfico en vivo y actualización de métricas en informes de ejecución y páginas de detalles de ejecución
+ Visor de archivos actualizado para registros, salidas y instantáneas de las páginas de detalles de ejecución.
+ Mejoras y novedades en la creación de informes en la pestaña Experimentos. 
+ Se ha agregado la capacidad para descargar el archivo config.json de la página Información general del área de trabajo de Azure Machine Learning Service.
+ Compatibilidad con la creación de espacios de trabajo de Azure Machine Learning desde el área de trabajo de Azure Databricks 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>SDK de Azure Machine Learning para Python v1.0.33
+ **Nuevas características:**
  + El método _Workspace.create_ ahora acepta las configuraciones predeterminadas para los clústeres de CPU y GPU.
  + Si se produce un error en la creación del área de trabajo, se borran los recursos dependientes.
  + La SKU predeterminada de Azure Container Registry se ha cambiado a una modalidad básica.
  + Azure Container Registry se crea de forma diferida, cuando se necesita para la creación de una imagen o ejecución.
  + Compatibilidad con los entornos para ejecuciones de entrenamiento.

### <a name="notebook-virtual-machine"></a>Máquina virtual de Notebook 

Use una máquina virtual de Notebook como un entorno de hospedaje seguro y preparado para la empresa para cuadernos de Jupyter Notebook en el que se pueden programar experimentos de aprendizaje automático, implementar modelos como puntos de conexión web y realizar todas las demás operaciones compatibles con el SDK de Azure Machine Learning mediante Python. Proporciona varias funcionalidades:
+ [Inicie rápidamente una máquina virtual de Notebook preconfigurada](quickstart-run-cloud-notebook.md) que tenga la versión más reciente del SDK de Azure Machine Learning y los paquetes relacionados.
+ El acceso se protege mediante tecnologías probadas, como HTTPS y autorización y autenticación de Azure Active Directory.
+ Almacenamiento en la nube confiable tanto de cuadernos como de código en la cuenta de almacenamiento del blob del área de trabajo de Azure Machine Learning. Puede eliminar la máquina virtual de Notebook de forma segura sin perder su trabajo.
+ Cuadernos de ejemplo preinstalados para explorar y experimentar con las características de Azure Machine Learning Service.
+ Completas funcionalidades de personalización completa de máquinas virtuales de Azure, cualquier tipo de máquina virtual, todos los paquetes y todos los controladores. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>SDK de Azure Machine Learning para Python v1.0.33.

+ Los modelos acelerados mediante hardware de Azure Machine Learning de la [matriz de puertas programables](concept-accelerate-with-fpgas.md) están disponible con carácter general.
  + Ya se puede [usar el paquete de modelos de azureml-accel](how-to-deploy-fpga-web-service.md) para:
    + Entrenar las ponderaciones de una red neuronal profunda compatible (ResNet 50, ResNet 152, DenseNet 121, VGG-16 y SSD VGG)
    + Usar el aprendizaje de transferencia con la DNN compatible
    + Registrar el modelo con el servicio Administración de modelos e incluir el modelo en un contenedor
    + Implementar el modelo en una máquina virtual de Azure con una matriz de puertas programables en un clúster de Azure Kubernetes Service (AKS)
  + Implementar el contenedor en un dispositivo servidor de [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview)
  + Puntuar los datos con el punto de conexión gRPC con este [ejemplo](https://github.com/Azure-Samples/aml-hardware-accelerated-models)

### <a name="automated-machine-learning"></a>Automated Machine Learning

+ Barrido de características para habilitar la incorporación dinámica de administradores de características que mejoren el rendimiento. Nuevos administradores de características: incrustaciones de trabajos, peso de la evidencia, codificaciones de destino, codificación de destino de texto, distancia del clúster
+ CV inteligente para controlar el entrenamiento o las divisiones válidas en Machine Learning automatizado
+ Algunos cambios en la optimización de la memoria y mejora del rendimiento del runtime
+ Mejora del rendimiento en la explicación de modelos
+ Conversión del modelo de ONNX para la ejecución local
+ Se ha agregado compatibilidad con submuestreos
+ Detención inteligente cuando haya criterios de salida definidos
+ Conjuntos apilados

+ Previsión de series temporales
  + Nueva función de previsión de predicciones   
  + Ahora puede usar la validación cruzada de origen variable en los datos de series temporales
  + Se ha agregado una nueva funcionalidad para configurar los retardos de las series temporales 
  + Se ha agregado una nueva funcionalidad que admita las características agregadas del periodo acumulado
  + Nueva detección de festivos y administrador de características cuando el código de país se define en la configuración del experimento

+ Azure Databricks
  + Se han habilitado la previsión de series temporales y la funcionalidad de explicabilidad e interpretabilidad de modelos
  + Ya se pueden cancelar y reanudar (continuar) experimentos de Machine Learning automatizado
  + Se ha agregado compatibilidad con el procesamiento con múltiples núcleos

### <a name="mlops"></a>MLOps
+ **Implementación y depuración locales para la puntuación de contenedores**<br/> Ya se puede implementar un modelo de Machine Learning localmente y realizar iteraciones rápidas en un archivo de puntuación y en las dependencias para asegurarse de que se comportan según lo previsto.

+ **Se han introducido InferenceConfig y Model.deploy()**<br/> La implementación de modelos ahora admite la especificación de una carpeta de origen con un script de entrada, igual que en RunConfig.  Además, se ha simplificado la implementación de modelos a un solo comando.

+ **Seguimiento de referencias de Git**<br/> Los clientes llevan tiempo solicitando funcionalidades de integración de Git básicas, ya que son una ayuda para mantener un registro de auditoría de un extremo a otro. Hemos implementado el seguimiento por las entidades principales de Azure Machine Learning de los metadatos relacionados con Git (repo, commit, clean state). Esta información la recopilarán automáticamente el SDK y la CLI.

+ **Servicio de validación y generación de perfiles de modelos**<br/> Los clientes se quejan con frecuencia de la dificultad que conlleva ajustar correctamente el tamaño del proceso asociado a su servicio de inferencia. Con nuestro servicio de generación de perfiles de modelos, el cliente puede proporcionar entradas de ejemplo y generaremos perfiles en 16 configuraciones de memoria/CPU diferentes para determinar el tamaño óptimo para la implementación.

+ **Traiga su propia imagen base para la inferencia**<br/> Otra queja habitual era la dificultad para moverse de las dependencias de experimentación al uso compartido de RE de inferencia. Con la nueva funcionalidad de uso compartido de imágenes base, ahora es posible reutilizar las imágenes base de la experimentación, las dependencias y todo en general, para la inferencia. Esto debería acelerar las implementaciones y reducir la brecha entre el bucle interno y el externo.

+ **Mejora en la generación de esquemas de Swagger**<br/> El anterior método de generación de Swagger era imposible de automatizar y propenso a errores. Hay una nueva forma en línea de generar esquemas de Swagger desde cualquier función de Python mediante los decoradores. Ahora este es un código abierto y el protocolo de generación de esquema no está emparejado a la plataforma de Azure Machine Learning.

+ **La CLI de Azure ML está disponible con carácter general**<br/> Ahora se pueden implementar modelos con un solo comando de la CLI. Recibimos comentarios de clientes comunes que informan de que nadie implementa un modelo de Machine Learning desde un cuaderno de Jupyter Notebook. La [ **documentación de referencia de la CLI** ](https://aka.ms/azmlcli) se ha actualizado.


## <a name="2019-04-22"></a>2019-04-22

SDK de Azure Machine Learning para Python v1.0.30.

[ `PipelineEndpoint` ](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) se ha introducido para agregar una nueva versión de una canalización publicada manteniendo el mismo punto de conexión.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>SDK de preparación de datos de Azure Machine Learning v1.1.2

Nota: El SDK de Python de preparación de datos no instalará los paquetes `numpy` y `pandas`. Consulte [las instrucciones de instalación actualizadas](https://aka.ms/aml-data-prep-installation).

+ **Nuevas características:**
  + Ahora puede usar la transformación de dinamización.
    + Guía paso a paso: [Cuaderno de dinamización](https://aka.ms/aml-data-prep-pivot-nb)
  + Ahora puede usar expresiones regulares en funciones nativas.
    + Ejemplos:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Ahora puede usar las funciones `to_upper`  y `to_lower`  en el lenguaje de expresiones.
  + Ahora puede ver el número de valores únicos de cada columna de un perfil de datos.
  + En algunos de los pasos de lector usados habitualmente, ahora se puede usar el argumento `infer_column_types`. Si se establece en `True`, la preparación de datos intentará detectar los tipos de columna y convertirlos automáticamente.
    + `inference_arguments` ahora está en desuso.
  + Ahora puede llamar a `Dataflow.shape`.

+ **Mejoras y correcciones de errores**
  + `keep_columns` ahora acepta un argumento opcional adicional `validate_column_exists`, que comprueba si el resultado de `keep_columns` contendrá todas las columnas.
  + Todos los pasos de lector (que se leen de un archivo) ahora aceptan un argumento opcional adicional `verify_exists`.
  + Ha mejorado el rendimiento de lectura del dataframe de pandas y de la obtención de perfiles de datos.
  + Se ha corregido un error que se producía al segmentar un solo paso de flujo de datos con un índice único.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Portal de Azure
  + Ahora puede volver a enviar un ciclo de script existente de un clúster de proceso remoto existente. 
  + Ahora puede ejecutar una canalización publicada con parámetros nuevos en la pestaña Canalizaciones. 
  + Los detalles de la ejecución ahora admiten un nuevo visor de archivos de instantánea. Puede ver una instantánea del directorio cuando envió una ejecución concreta. También puede descargar el cuaderno que se envió para iniciar la ejecución.
  + Ahora puede cancelar las ejecuciones primarias desde Azure Portal.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>SDK de Azure Machine Learning para Python v1.0.23

+ **Nuevas características:**
  + El SDK de Azure Machine Learning ahora admite Python 3.7.
  + Ahora, los estimadores de DNN de Azure Machine Learning proporcionan compatibilidad con múltiples versiones integrada. Por ejemplo, el estimador `TensorFlow`   ahora acepta un parámetro `framework_version` y los usuarios pueden especificar las versiones "1.10" o "1.12". Para obtener una lista de las versiones compatibles con la versión actual del SDK, llame a `get_supported_versions()` en la clase de marco deseada (por ejemplo, `TensorFlow.get_supported_versions()`).
  Para obtener una lista de las versiones compatibles con la versión más reciente del SDK, consulte la [documentación del estimador de DNN](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py).

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>SDK de preparación de datos de Azure Machine Learning v1.1.1

+ **Nuevas características:**
  + Puede leer varios orígenes de Datastore/DataPath/DataReference mediante transformaciones de read_*.
  + Puede realizar las siguientes operaciones en columnas para crear una columna:division, floor, modulo, power, length.
  + La preparación de datos ahora forma parte del conjunto de diagnósticos de Azure Machine Learning y registrará la información de diagnóstico de forma predeterminada.
    + Para desactivar esta característica, establezca esta variable de entorno en true: DISABLE_DPREP_LOGGER

+ **Mejoras y correcciones de errores**
  + Mejora de la documentación del código mejorada para funciones y clases usadas habitualmente.
  + Se ha corregido un error en auto_read_file que impedía leer archivos de Excel.
  + Se ha agregado la opción de sobrescribir la carpeta en read_pandas_dataframe.
  + Se ha mejorado el rendimiento de la instalación de la dependencia de dotnetcore2 y se ha agregado compatibilidad con Fedora 27/28 y Ubuntu 1804.
  + Se ha mejorado el rendimiento de la lectura de blobs de Azure.
  + La detección del tipo de columna ahora admite columnas del tipo Long.
  + Se ha corregido un error por el que algunos valores se muestran como marcas de tiempo, en lugar de objetos DateTime de Python.
  + Se ha corregido un error por el que algunos tipos de recuentos se muestran algunos recuentos de tipo como dobles, en lugar de como enteros.

  
## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>SDK de Azure Machine Learning para Python v1.0.21

+ **Nuevas características:**
  + El método *azureml.core.Run.create_children* permite la creación de baja latencia de varias ejecuciones secundarias con una sola llamada.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>SDK de preparación de datos de Azure Machine Learning v1.1.0

+ **Cambios importantes**
  + El concepto del paquete de preparación de datos está en desuso y ya no se admite. En lugar de almacenar varios flujos de datos en un solo paquete, puede hacerlo individualmente.
    + Guía paso a paso: [Apertura y almacenamiento del cuaderno Flujos de datos](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Nuevas características:**
  + Preparación de datos ahora puede reconocer columnas que se ajustes a un tipo semántico concreto y se dividen en consecuencia. Los STypes compatibles actualmente incluyen: dirección de correo electrónico, coordenadas geográficas (latitud y longitud), direcciones IPv4 e IPv6, número de teléfono de EE. UU. y código postal de EE.UU.
    + Guía paso a paso: [Cuaderno Tipos semánticos](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Preparación de datos ahora admite las siguientes operaciones para generar una columna resultante a partir de dos columnas numéricas: subtract, multiply, divide y modulo.
  + Puede llamar a `verify_has_data()` en un flujo de datos para comprobar si este generaría registros si se ejecutara.

+ **Mejoras y correcciones de errores**
  + Ahora puede especificar el número de rangos que se usarán en un histograma para los perfiles de las columnas numéricas.
  + La transformación `read_pandas_dataframe` ahora requiere que DataFrame tenga nombres de columna de los tipos de byte o de cadena.
  + Se ha corregido un error en la transformación `fill_nulls`, donde los valores no se rellenaron correctamente si la columna faltara.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>SDK de Azure Machine Learning para Python v1.0.18

 + **Cambios**
   + El paquete azureml tensorboard reemplaza azureml-contrib-tensorboard.
   + Con esta versión, puede configurar una cuenta de usuario en el clúster de proceso administrado (amlcompute) al crearlo. Esto puede realizarse usando estas propiedades en la configuración de aprovisionamiento. Encontrará más información en la [documentación de referencia del SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none-).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>SDK de preparación de datos de Azure Machine Learning v1.0.17

+ **Nuevas características:**
  + Ahora admite la adición de dos columnas numéricas para generar una columna resultante mediante el lenguaje de expresiones.

+ **Mejoras y correcciones de errores**
  + Ha mejorado la documentación y la comprobación de parámetros de random_split.
  
## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>SDK de preparación de datos de Azure Machine Learning v1.0.16

+ **Corrección de error**
  + Se ha corregido un problema de autenticación de una entidad de servicio que se produjo por un cambio de API.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>SDK de Azure Machine Learning para Python v1.0.17

+ **Nuevas características:**

  + Ahora, Azure Machine Learning proporciona compatibilidad de primera clase con Chainer del marco DNN popular. Mediante [ `Chainer` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) los usuarios de la clase pueden entrenar e implementar fácilmente modelos de Chainer.
    + Aprenda a [ejecutar el entrenamiento distribuido con ChainerMN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb)
    + Aprenda a [ejecutar el ajuste de hiperparámetros con Chainer mediante HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + La capacidad agregada de las canalizaciones de Azure Machine Learning desencadena una ejecución de una canalización basada en modificaciones del almacén de datos. El [cuaderno de programación](https://aka.ms/pl-schedule) de la canalización se actualice para presentar esta característica.

+ **Mejoras y correcciones de errores**
  + Hemos agregado compatibilidad con canalizaciones de Azure Machine Learning para establecer la propiedad source_directory_data_store en un almacén de datos deseado (como un almacenamiento de blobs) en [RunConfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) que se suministran a [ PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py). De forma predeterminada los pasos utilizan el almacén de Azure File como el almacén de datos de respaldo, lo que provocar problemas de limitación cuando un gran número de pasos se ejecutan simultáneamente.

### <a name="azure-portal"></a>Portal de Azure

+ **Nuevas características:**
  + Nueva experiencia de arrastrar y soltar el editor de tablas en informes. Los usuarios pueden arrastrar una columna desde el cuadro al área de la tabla en la que se mostrará una vista previa de la tabla. Las columnas se pueden reorganizar.
  + Nuevo visor de archivos de registros
  + Vínculos a ejecuciones de experimentos, proceso, modelos, imágenes e implementaciones desde la pestaña de actividades

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>SDK de preparación de datos de Azure Machine Learning v1.0.15

+ **Nuevas características:**
  + La preparación de datos ahora admite la escritura de secuencias de archivos desde un flujo de datos. También proporciona la capacidad de manipular los nombres de las secuencias de archivo para crear nombres de archivo.
    + Guía paso a paso: [Cuaderno Trabajo con secuencias de archivo](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Mejoras y correcciones de errores**
  + Mejora en el rendimiento mejorado de t-Digest en conjuntos de datos grandes.
  + La preparación de datos ahora admite la lectura de datos desde DataPath.
  + Ahora se activa una codificación frecuente en columnas booleanas y numéricas.
  + Correcciones de otros errores varios.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>SDK de Azure Machine Learning para Python v1.0.15

+ **Nuevas características:**
  + Las canalizaciones de Azure Machine Learning AzureBatchStep ([cuaderno](https://aka.ms/pl-azbatch)), HyperDriveStep (cuaderno) y la funcionalidad de programación basada en tiempo ([cuaderno](https://aka.ms/pl-schedule)).
  +  La funcionalidad DataTranferStep se actualizó para funcionar con Azure Database para PostgreSQL ([bloc de notas](https://aka.ms/pl-data-trans)).

+ **Cambios**
  + Entra en desuso `PublishedPipeline.get_published_pipeline` en favor de `PublishedPipeline.get`.
  + Entra en desuso `Schedule.get_schedule` en favor de `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>SDK de preparación de datos de Azure Machine Learning v1.0.12

+ **Nuevas características:**
  + La preparación de los datos admite la lectura desde una base de datos de Azure SQL con Almacén de datos.
 
+ **Cambios**
  + Ha mejorado el rendimiento de la memoria de determinadas operaciones en datos de gran tamaño.
  + La opción `read_pandas_dataframe()` requiere que se especifique `temp_folder`.
  + La propiedad `name` en `ColumnProfile` está en desuso, use `column_name` en su lugar.

## <a name="2019-01-28"></a>28-01-2019

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>SDK de Azure Machine Learning para Python v1.0.10

+ **Cambios**: 
  + SDK de Azure Machine Learning ya no tiene los paquetes azure-cli como dependencia. En concreto, las dependencias azure-cli-core y azure-cli-profile azureml-core se quitaron de azureml-core. Estos son los cambios que afectan al usuario:
    + Si va a realizar "az login" y, después, va a usar azureml-sdk, el SDK iniciará la sesión del código del dispositivo o del explorador una vez más. No usará ningún estado de las credenciales creado por "az login".
    + Para la autenticación de la CLI de Azure, como el uso de "az login", use la clase _azureml.core.authentication.AzureCliAuthentication_. Para la autenticación de la CLI de Azure, siga _pip install azure-cli_ en el entorno de Python donde ha instalado azureml-sdk.
    + Si está realizando "az login" mediante una entidad de servicio para la automatización, se recomienda usar la clase _azureml.core.authentication.ServicePrincipalAuthentication_, ya que azureml-sdk no usará el estado de las credenciales creadas mediante la CLI de Azure. 

+ **Correcciones de errores**: Esta versión contiene principalmente correcciones de errores menores.

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>SDK de preparación de datos de Azure Machine Learning v1.0.8

+ **Correcciones de errores**
  + Ha mejorado el rendimiento de la obtención de perfiles de datos.
  + Se corrigieron los errores menores relacionados con los informes de error.
  
### <a name="azure-portal-new-features"></a>Azure Portal: nuevas características
+ Nuevo experiencia de arrastrar y colocar gráficos en informes. Los usuarios podrán arrastrar una columna o atributo desde el pozo al área del gráfico donde el sistema seleccionará automáticamente un tipo de gráfico adecuado para el usuario según el tipo de datos. Los usuarios pueden cambiar el tipo de gráfico por otros tipos aplicables o agregar atributos adicionales.

    Tipos de gráficos compatibles:
    - Gráfico de líneas
    - Histograma
    - Gráfico de barras apiladas
    - Diagrama de caja
    - Gráfico de dispersión
    - Gráfico de burbujas
+ Ahora el portal genera dinámicamente los informes para los experimentos. Cuando un usuario envía una ejecución para un experimento, se generará automáticamente un informe con las métricas registradas y los grafos a fin de permitir la comparación entre distintas ejecuciones. 

## <a name="2019-01-14"></a>14-01-2019

### <a name="azure-machine-learning-sdk-for-python-v108"></a>SDK de Azure Machine Learning para Python v1.0.8

+ **Correcciones de errores**: Esta versión contiene principalmente correcciones de errores menores.

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>SDK de preparación de datos de Azure Machine Learning v1.0.7

+ **Nuevas características:**
  + Mejoras de almacén de datos (documentadas en la [guía sobre almacenes de datos](https://aka.ms/aml-data-prep-datastore-nb))
    + Se agregó la capacidad de leer y escribir en el recurso compartido de archivos de Azure y almacenes de datos de ADLS en escalado vertical.
    + Al utilizar almacenes de datos, la preparación de datos ahora admite el uso de autenticación de la entidad de servicio en lugar de la autenticación interactiva.
    + Se agregó compatibilidad para las direcciones URL wasb y wasbs.

## <a name="2019-01-09"></a>09-01-2019

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>SDK de preparación de datos de Azure Machine Learning v1.0.6

+ **Correcciones de errores**
  + Se corrigió el error en la lectura desde contenedores de Azure Blob públicamente legibles en Spark

## <a name="2018-12-20"></a>20-12-2018 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>SDK de Azure Machine Learning para Python v1.0.6
+ **Correcciones de errores**: Esta versión contiene principalmente correcciones de errores menores.

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>SDK de preparación de datos de Azure Machine Learning v1.0.4

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
+ Se usa para el entrenamiento de modelos y la inferencia y puntuación de lotes
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
  + Los [cuadernos de ejemplo](https://aka.ms/aml-notebooks) ahora están actualizados para usar este nuevo proceso administrado.
  + [Obtenga información sobre este proceso](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure Portal: nuevas características
+ Cree y administre los tipos de [proceso de Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute) en el portal.
+ Supervise el uso de cuota y [solicite una cuota](how-to-manage-quotas.md) para el proceso de Azure Machine Learning.
+ Vea el estado del clúster del Proceso de Machine Learning en tiempo real.
+ La compatibilidad con la red virtual se agregó para la creación de Azure Kubernetes Service y el proceso de Azure Machine Learning.
+ Vuelva a ejecutar sus canalizaciones publicadas con los parámetros existentes.
+ Nuevos [gráficos de aprendizaje automático automatizados](how-to-understand-automated-ml.md) para modelos de clasificación (gráfico de importancia de características, elevación, ganancias y calibración con explicabilidad de modelo) y modelos de regresión (gráfico de importancia de características y valores residuales con explicabilidad de modelo). 
+ Las canalizaciones se pueden ver en Azure Portal




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>SDK de Azure Machine Learning para Python v0.1.80

+ **Cambios importantes** 
  * El espacio de nombres *azureml.train.widgets* se ha movido a *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* deja de usar las siguientes clases: *azureml.core.compute.BatchAICompute* y *azureml.core.compute.DSVMCompute*. La última clase se quitará en versiones posteriores. Ahora la clase AmlCompute tiene una definición más fácil. Solo necesita los valores de vm_size y max_nodes, y escala automáticamente su clúster de 0 al valor de max_nodes cuando se envía un trabajo. Nuestros [cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) se actualizaron con esta información y deberían ofrecer ejemplos de uso. Esperamos que disfrute de esta simplificación y de una gran cantidad de características atractivas adicionales en una versión posterior.

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

### <a name="azure-portal"></a>Portal de Azure 
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
