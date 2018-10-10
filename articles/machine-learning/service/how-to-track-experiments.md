---
title: 'Seguimiento de experimentos y métricas de entrenamiento: Azure Machine Learning | Microsoft Docs'
description: Con el servicio Azure Machine Learning, puede realizar un seguimiento de sus experimentos y supervisar las métricas para mejorar el proceso de creación de modelos. Aprenda a agregar el registro al script de entrenamiento, enviar el experimento, comprobar el progreso de un trabajo en ejecución y ver los resultados de una ejecución.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: ced10a54d569531b06ee47b646130f43cedd2963
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984609"
---
# <a name="track-experiments-and-training-metrics-in-azure-machine-learning"></a>Seguimiento de experimentos y métricas de entrenamiento en Azure Machine Learning

Con el servicio Azure Machine Learning, puede realizar un seguimiento de sus experimentos y supervisar las métricas para mejorar el proceso de creación de modelos. En este artículo, aprenderá sobre las distintas maneras de agregar el registro al script de entrenamiento, enviar el experimento con **start_logging** y **ScriptRunConfig**, comprobar el progreso de un trabajo en ejecución y ver los resultados de una ejecución. 

## <a name="list-of-training-metrics"></a>Lista de métricas de entrenamiento 

Las siguientes métricas se pueden agregar a una ejecución durante el entrenamiento de un experimento. Para ver una lista más detallada de aquello de lo que puede realizar el seguimiento en una ejecución, consulte la [documentación de referencia del SDK](https://docs.microsoft.com/python/api/overview/azure/azure-ml-sdk-overview?view=azure-ml-py).

|Escriba| Función de Python | Notas|
|----|:----:|:----:|
|Valores escalares | `run.log(name, value, description='')`| Registra un valor de métrica en la ejecución con el nombre especificado. Al registrar una métrica en una ejecución, esa métrica se almacena en el registro de ejecución en el experimento.  Puede registrar la misma métrica varias veces dentro de una ejecución y el resultado se considerará un vector de esa métrica.|
|Listas| `run.log_list(name, value, description='')`|Registra un valor de métrica de lista en la ejecución con el nombre especificado.|
|Fila| `run.log_row(name, description=None, **kwargs)`|El uso de *log_row* crea una métrica de tabla con columnas, como se describe en argumentos kwargs. Cada parámetro con nombre genera una columna con el valor especificado.  Se puede llamar una vez a *log_row* para registrar una tupla arbitraria, o varias veces en un bucle para generar una tabla completa.|
|Tabla| `run.log_table(name, value, description='')`| Registra una métrica de tabla en la ejecución con el nombre especificado. |
|Imágenes| `run.log_image(name, path=None, plot=None)`|Registra una métrica de imagen en el registro de ejecución. Use log_image para registrar un archivo de imagen o un trazado matplotlib en la ejecución.  Estas imágenes serán visibles y comparables en el registro de ejecución.|
|Etiquetar una ejecución| `run.tag(key, value=None)`|Etiqueta la ejecución con una clave de cadena y un valor de cadena opcional.|
|Cargar archivo o directorio|`run.upload_file(name, path_or_stream)`|Carga un archivo en el registro de ejecución. Las ejecuciones capturan automáticamente el archivo en el directorio de salida especificado, cuyo valor predeterminado es "./outputs" para la mayoría de tipos de ejecución.  Use upload_file solo cuando sea necesario cargar archivos adicionales o no se especifique un directorio de salida. Se recomienda agregar `outputs` al nombre, para que se cargue en el directorio de salida. Puede enumerar todos los archivos que están asociados con este registro de ejecución por el parámetro `run.get_file_names()` invocado.|

> [!NOTE]
> Las métricas de valores escalares, listas, filas y tablas pueden tener el tipo: float, integer o string.

## <a name="log-metrics-for-experiments"></a>Métricas de registro para experimentos

Si quiere realizar un seguimiento del experimento o supervisarlo, debe agregar código para iniciar el registro al enviar la ejecución. Las siguientes son formas de desencadenar el envío de ejecución:
* __Run.start_logging__: agrega funciones de registro al script de entrenamiento e inicia una sesión de registro interactiva en el experimento especificado. **start_logging** crea una ejecución interactiva para su uso en escenarios como los cuadernos. Todas las métricas que se registran durante la sesión se agregan al registro de ejecución en el experimento.
* __ScriptRunConfig__: agrega funciones de registro al script de entrenamiento y carga la carpeta del script completo con la ejecución.  **ScriptRunConfig** es una clase para configurar las ejecuciones del script. Con esta opción, puede agregar código de supervisión para recibir una notificación al finalizar o para obtener un widget visual para supervisar.

## <a name="set-up-the-workspace-and-experiment"></a>Configuración del área de trabajo y el experimento
Antes de agregar el registro y enviar un experimento, debe configurar el área de trabajo y el experimento.

1. Cargue el área de trabajo. Para más información sobre la definición de la configuración del área de trabajo, siga la [guía de inicio rápido](https://docs.microsoft.com/azure/machine-learning/service/quickstart-get-started).

  ```python
  from azureml.core import Workspace, Run
  import azureml.core
  
  ws = Workspace(workspace_name = <<workspace_name>>,
               subscription_id = <<subscription_id>>,
               resource_group = <<resource_group>>)
   ```

2. Cree el experimento.

  ```python
  from azureml.core import Experiment

  # make up an arbitrary name
  experiment_name = 'train-in-notebook'
  exp = Experiment(workspace_object = ws, name = experiment_name)
  ```
  
## <a name="option-1-use-startlogging"></a>Opción 1: Usar start_logging

**start_logging** crea una ejecución interactiva para su uso en escenarios como los cuadernos. Todas las métricas que se registran durante la sesión se agregan al registro de ejecución en el experimento.

En el ejemplo siguiente, se entrena un modelo sklearn Ridge sencillo de forma local en un cuaderno de Jupyter local. Para más información sobre el envío de experimentos a diferentes entornos, consulte [Configuración de destinos de proceso para el entrenamiento de modelos con el servicio Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-set-up-training-targets).

1. Cree un script de entrenamiento en un cuaderno local de Jupyter. 

  ``` python
  # load diabetes dataset, a well-known small dataset that comes with scikit-learn
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import Ridge
  from sklearn.metrics import mean_squared_error
  from sklearn.model_selection import train_test_split
  from sklearn.externals import joblib

  X, y = load_diabetes(return_X_y = True)
  columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
  data = {
      "train":{"X": X_train, "y": y_train},        
      "test":{"X": X_test, "y": y_test}
  }
  reg = Ridge(alpha = 0.03)
  reg.fit(data['train']['X'], data['train']['y'])
  preds = reg.predict(data['test']['X'])
  print('Mean Squared Error is', mean_squared_error(preds, data['test']['y']))
  joblib.dump(value = reg, filename = 'model.pkl');
  ```

2. Agregue seguimiento del experimento mediante el SDK del servicio Azure Machine Learning y cargue un modelo guardado en el registro de ejecución del experimento. El código siguiente agrega etiquetas y registros y carga un archivo de modelo en la ejecución del experimento.

  ```python
  run = Run.start_logging(experiment = exp)
  run.tag("Description","My first run!")
  run.log('alpha', 0.03)
  reg = Ridge(alpha = 0.03)
  reg.fit(data['train']['X'], data['train']['y'])
  preds = reg.predict(data['test']['X'])
  run.log('mse', mean_squared_error(preds, data['test']['y']))
  joblib.dump(value = reg, filename = 'model.pkl')
  # Upload file directly to the outputs folder
  run.upload_file(name = 'outputs/model.pkl', path_or_stream = './model.pkl')

  run.complete()
  ```

El script finaliza con ```run.complete()```, que marca la ejecución como completada.  Este script se usa normalmente en escenarios de cuaderno interactivos.

## <a name="option-2-use-scriptrunconfig"></a>Opción 2: Usar ScriptRunConfig

**ScriptRunConfig** es una clase para configurar las ejecuciones del script. Con esta opción, puede agregar código de supervisión para recibir una notificación al finalizar o para obtener un widget visual para supervisar.

En este ejemplo se amplía el modelo sklearn Ridge básico anterior. Se realiza un barrido simple de parámetros sobre los valores alfa del modelo para capturar las métricas y los modelos entrenados en las ejecuciones del experimento. El ejemplo se ejecuta localmente en un entorno administrado por el usuario. 

1. Cree un script de entrenamiento. Se usa ```%%writefile%%``` para escribir la salida del código de entrenamiento en la carpeta del script como ```train.py```.

  ```python
  %%writefile $project_folder/train.py

  import os
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import Ridge
  from sklearn.metrics import mean_squared_error
  from sklearn.model_selection import train_test_split
  from azureml.core.run import Run
  from sklearn.externals import joblib

  import numpy as np

  #os.makedirs('./outputs', exist_ok = True)

  X, y = load_diabetes(return_X_y = True)

  run = Run.get_submitted_run()

  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
  data = {"train": {"X": X_train, "y": y_train},
          "test": {"X": X_test, "y": y_test}}

  # list of numbers from 0.0 to 1.0 with a 0.05 interval
  alphas = np.arange(0.0, 1.0, 0.05)

  for alpha in alphas:
      # Use Ridge algorithm to create a regression model
      reg = Ridge(alpha = alpha)
      reg.fit(data["train"]["X"], data["train"]["y"])

      preds = reg.predict(data["test"]["X"])
      mse = mean_squared_error(preds, data["test"]["y"])
      # log the alpha and mse values
      run.log('alpha', alpha)
      run.log('mse', mse)

      model_file_name = 'ridge_{0:.2f}.pkl'.format(alpha)
      # save model in the outputs folder so it automatically get uploaded
      with open(model_file_name, "wb") as file:
          joblib.dump(value = reg, filename = model_file_name)

      # upload the model file explicitly into artifacts 
      run.upload_file(name = model_file_name, path_or_stream = model_file_name)

      # register the model
      #run.register_model(file_name = model_file_name)

      print('alpha is {0:.2f}, and mse is {1:0.2f}'.format(alpha, mse))
  
  ```

2. El script ```train.py``` hace referencia a ```mylib.py```. Este archivo le permite obtener la lista de valores alfa que se usarán en el modelo de Ridge.

  ```python
  %%writefile $script_folder/mylib.py
  import numpy as np

  def get_alphas():
      # list of numbers from 0.0 to 1.0 with a 0.05 interval
      return np.arange(0.0, 1.0, 0.05)
  ```

3. Configure un entorno local administrado por el usuario.

  ```python
  from azureml.core.runconfig import RunConfiguration

  # Editing a run configuration property on-fly.
  run_config_user_managed = RunConfiguration()

  run_config_user_managed.environment.python.user_managed_dependencies = True

  # You can choose a specific Python environment by pointing to a Python path 
  #run_config.environment.python.interpreter_path = '/home/user/miniconda3/envs/sdk2/bin/python'
  ```

4. Envíe el script ```train.py``` para ejecutarse en el entorno administrado por el usuario. Esta carpeta del script completo se envía para el entrenamiento, incluido el archivo ```mylib.py```.

  ```python
  from azureml.core import ScriptRunConfig

  src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_user_managed)
  run = exp.submit(src)
  ```
  
## <a name="view-run-details"></a>Visualización de los detalles de ejecución

### <a name="monitor-run-with-jupyter-notebook-widgets"></a>Supervisar la ejecución con widgets de cuadernos de Jupyter
Cuando se usa el método **ScriptRunConfig** para enviar ejecuciones, se puede ver el progreso de la ejecución con un widget de cuaderno de Jupyter. Al igual que el envío de ejecuciones, el widget es asincrónico y proporciona actualizaciones directas cada 10 o 15 segundos hasta que se completa el trabajo.

1. Vea el widget de Jupyter mientras espera a que finalice la ejecución.

  ```python
  from azureml.train.widgets import RunDetails
  RunDetails(run).show()
  ```

  ![Captura de pantalla del widget de cuaderno de Jupyter](./media/how-to-track-experiments/widgets.PNG)

### <a name="get-log-results-upon-completion"></a>Obtener los resultados de registros tras la finalización

El entrenamiento y la supervisión de modelos tienen lugar en segundo plano, de modo que pueden ejecutar otras tareas mientras espera. También puede esperar a que el modelo haya completado el entrenamiento antes de ejecutar más código. Cuando se usa **ScriptRunConfig**, se puede usar ```run.wait_for_completion(show_output = True)``` para mostrar cuándo finaliza el entrenamiento del modelo. La marca ```show_output``` le ofrece una salida detallada. 
  
### <a name="query-run-metrics"></a>Métricas de ejecución de consulta

Puede ver las métricas de un modelo entrenado con ```run.get_metrics()```. Ahora puede obtener todas las métricas que se registraron en el ejemplo anterior para determinar el mejor modelo.

## <a name="view-the-experiment-in-the-azure-portal"></a>Visualización del experimento en Azure Portal

Cuando un experimento ha terminado de ejecutarse, puede ir al registro de ejecución del experimento grabado. Puede hacerlo de dos maneras:

* Obtener directamente la dirección URL a la ejecución ```print(run.get_portal_url())```
* Ver los detalles de la ejecución mediante el envío del nombre de la ejecución (en este caso, ```run```). Aquí se le indica el nombre del experimento, el identificador, el tipo, el estado, la página de detalles, un vínculo a Azure Portal y un vínculo a documentación.

El vínculo para la ejecución le lleva directamente a la página de detalles de ejecución en Azure Portal. Aquí puede ver las propiedades, las métricas sometidas a seguimiento, las imágenes y los gráficos que se registran en el experimento. En este caso, se registra MSE y los valores alfa.

  ![Captura de pantalla de detalles de ejecución en Azure Portal](./media/how-to-track-experiments/run-details-page-web.PNG)

También puede ver las salidas o los registros de la ejecución, o descargar la instantánea del experimento que ha enviado para que pueda compartir la carpeta del experimento con otros usuarios.

## <a name="example-notebooks"></a>Cuadernos de ejemplo
Los cuadernos siguientes muestran los conceptos de este artículo:
* `01.getting-started/01.train-within-notebook/01.train-within-notebook.ipynb`
* `01.getting-started/02.train-on-local/02.train-on-local.ipynb`

Obtenga estos cuadernos: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

Pruebe estos pasos para saber cómo usar el SDK de Azure Machine Learning para Python:

* Vea un ejemplo de cómo registrar el mejor modelo e implementarlo en el tutorial, [Entrenar un modelo de clasificación de imágenes con Azure Machine Learning](tutorial-train-models-with-aml.md).

* Aprenda a [entrenar modelos de PyTorch con Azure Machine Learning](how-to-train-pytorch.md).
