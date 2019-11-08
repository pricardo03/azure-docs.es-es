---
title: Interpretación de modelo en aprendizaje automático automatizado
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo explicar los motivos por los que el modelo de aprendizaje automático automatizado hace predicciones con el SDK de Azure Machine Learning. Se puede usar durante el entrenamiento y la inferencia para reconocer cómo el modelo determina la importancia de las características y hace predicciones.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: 2c9df55eb319dd45281eca4684c79d83dc6ef933
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511053"
---
# <a name="model-interpretability-for-automated-ml-models"></a>Interpretación de modelo para aprendizaje automático automatizado

[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

En estas instrucciones, obtendrá información sobre cómo habilitar la funcionalidad de interpretación en el aprendizaje automático automatizado con Azure Machine Learning Service. El aprendizaje automático automatizado le permite reconocer la importancia de las características sin procesar y las diseñadas. Para usar la interpretación del modelo, establezca `model_explainability=True` en el objeto `AutoMLConfig`.  

En este artículo, obtendrá información sobre las siguientes tareas:

* Interpretación durante el entrenamiento para el mejor o cualquier modelo
* Habilitación de visualizaciones para ayudarle en la detección de patrones en datos y explicaciones
* Interpretación durante la inferencia o puntuación

## <a name="prerequisites"></a>Requisitos previos

* Ejecute `pip install azureml-interpret azureml-contrib-interpret` para obtener los paquetes necesarios en las funciones de interpretación.
* Este artículo asume el conocimiento de la compilación de experimentos de aprendizaje automático automatizado. Consulte el [tutorial](tutorial-auto-train-models.md)o las [instrucciones](how-to-configure-auto-train.md) para aprender a utilizar el aprendizaje automático automatizado en el SDK.
 
## <a name="interpretability-during-training-for-the-best-model"></a>Interpretación durante el entrenamiento del mejor modelo 

Recuperar la explicación de la `best_run`, que incluye explicaciones de las características diseñadas y las características sin procesar. 

### <a name="download-engineered-feature-importance-from-artifact-store"></a>Descargue la importancia de las características diseñadas de la tienda de artefactos

Puede usar `ExplanationClient` para descargar las explicaciones de las características diseñadas del almacén de artefactos de best_run. Para obtener la explicación de las características sin procesar establecidas `raw=True`. 

```python
from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient

client = ExplanationClient.from_run(best_run)
engineered_explanations = client.download_model_explanation(raw=False)
print(engineered_explanations.get_feature_importance_dict())
```

## <a name="interpretability-during-training-for-any-model"></a>Interpretación durante el entrenamiento de cualquier modelo 

En esta sección, obtendrá información sobre cómo calcular las explicaciones del modelo y visualizarlas. Además de recuperar la explicación de un modelo existente para un modelo de aprendizaje automático automatizado, también puede explicar el modelo con diferentes datos de prueba. Los siguientes pasos permitirán calcular y visualizar la importancia de las características diseñadas y la importancia de las características sin procesar según los datos de las pruebas.

### <a name="retrieve-any-other-automl-model-from-training"></a>Recuperar cualquier otro modelo de aprendizaje automático automatizado del entrenamiento

```python
automl_run, fitted_model = local_run.get_output(metric='r2_score')
```

### <a name="setup-the-model-explanations"></a>Configurar las explicaciones del modelo

El fitted_model puede generar los siguientes elementos, que se usarán para obtener las explicaciones de características diseñadas y sin procesar con automl_setup_model_explanations:

* Datos caracterizados de muestras de entrenamientos/muestras de pruebas
* Recopilación de listas de nombres de características diseñadas y sin formato
* Busque las clases en la columna etiquetada de los escenarios de clasificación

El automl_explainer_setup_obj contiene todas las estructuras de la lista anterior.

```python
from azureml.train.automl.automl_explain_utilities import AutoMLExplainerSetupClass, automl_setup_model_explanations

automl_explainer_setup_obj = automl_setup_model_explanations(fitted_model, X=X_train, 
                                                             X_test=X_test, y=y_train, 
                                                             task='classification')
```
### <a name="initialize-the-mimic-explainer-for-feature-importance"></a>Inicializar el explicador Mimic para la importancia de la característica

Para explicar los modelos de aprendizaje automático automatizado, utilice la clase `MimicWrapper`. El MimicWrapper se puede inicializar con parámetros para el objeto de configuración del explicador, el área de trabajo y un modelo LightGBM que actúa como un modelo sustituto para explicar el modelo de aprendizaje automático automatizado (fitted_model aquí). El MimicWrapper también toma el objeto automl_run a donde se cargarán las explicaciones sin procesar y diseñadas.

```python
from azureml.interpret.mimic.models.lightgbm_model import LGBMExplainableModel
from azureml.interpret.mimic_wrapper import MimicWrapper

explainer = MimicWrapper(ws, automl_explainer_setup_obj.automl_estimator, LGBMExplainableModel, 
                         init_dataset=automl_explainer_setup_obj.X_transform, run=automl_run,
                         features=automl_explainer_setup_obj.engineered_feature_names, 
                         feature_maps=[automl_explainer_setup_obj.feature_map],
                         classes=automl_explainer_setup_obj.classes)
```

### <a name="use-mimicexplainer-for-computing-and-visualizing-engineered-feature-importance"></a>Use MimicExplainer para calcular y visualizar la importancia de las características diseñadas

El método explica() de MimicWrapper puede ser llamado con las muestras de prueba transformadas para obtener la importancia de las características diseñadas que se generaron. También puede usar ExplanationDashboard para la visualización del panel de valores de importancia de las características diseñadas generadas por el aprendizaje automático automatizado.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard
engineered_explanations = explainer.explain(['local', 'global'],              
                                            eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(engineered_explanations.get_feature_importance_dict())
ExplanationDashboard(engineered_explanations, automl_explainer_setup_obj.automl_estimator, automl_explainer_setup_obj.X_test_transform)
```
### <a name="use-mimic-explainer-for-computing-and-visualizing-raw-feature-importance"></a>Use MimicExplainer para calcular y visualizar la importancia de las características sin procesar

El método explica() de MimicWrapper puede ser llamado con las muestras de prueba transformadas y la configuración`get_raw` para obtener la importancia de las características sin procesar que se generaron. También puede usar ExplanationDashboard para la visualización del panel de valores de importancia de las características sin procesar.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard

raw_explanations = explainer.explain(['local', 'global'], get_raw=True, 
                                     raw_feature_names=automl_explainer_setup_obj.raw_feature_names,
                                     eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(raw_explanations.get_feature_importance_dict())
ExplanationDashboard(raw_explanations, automl_explainer_setup_obj.automl_pipeline, automl_explainer_setup_obj.X_test_raw)
```

### <a name="interpretability-during-inference"></a>Interpretación durante la inferencia

En esta sección, obtendrá información sobre cómo operar un modelo de aprendizaje automático automatizado con el explicador, que se utilizó para calcular las explicaciones de la sección anterior.

### <a name="register-the-model-and-the-scoring-explainer"></a>Registre el modelo y la explicación de la puntuación

Use el `TreeScoringExplainer` para crear el explicador de puntuación, que se usará para calcular los valores de importancia de las características sin procesar y diseñadas a la hora de la inferencia. Tenga en cuenta que se inicializa el explicador de puntuación con el feature_map que se calculó previamente. El explicador de puntuación usará feature_map para devolver la importancia de la característica sin procesar.

En el código siguiente, se guarda el explicador de puntuación y se registra el modelo y el explicador de puntuación con el servicio de Administración de modelos.

```python
from azureml.interpret.scoring.scoring_explainer import TreeScoringExplainer, save

# Initialize the ScoringExplainer
scoring_explainer = TreeScoringExplainer(explainer.explainer, feature_maps=[automl_explainer_setup_obj.feature_map])

# Pickle scoring explainer locally
save(scoring_explainer, exist_ok=True)

# Register trained automl model present in the 'outputs' folder in the artifacts
original_model = automl_run.register_model(model_name='automl_model', 
                                           model_path='outputs/model.pkl')

# Register scoring explainer
automl_run.upload_file('scoring_explainer.pkl', 'scoring_explainer.pkl')
scoring_explainer_model = automl_run.register_model(model_name='scoring_explainer', model_path='scoring_explainer.pkl')
```

### <a name="create-the-conda-dependencies-for-setting-up-the-service"></a>Crear las dependencias de conda para configurar el servicio

Después, cree las dependencias de entorno necesarias en el contenedor para el modelo implementado.

```python
from azureml.core.conda_dependencies import CondaDependencies 

azureml_pip_packages = [
    'azureml-interpret', 'azureml-train-automl', 'azureml-defaults'
]

myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas', 'numpy', 'py-xgboost<=0.80'],
                                 pip_packages=azureml_pip_packages,
                                 pin_sdk_version=True)

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())

with open("myenv.yml","r") as f:
    print(f.read())

```

### <a name="deploy-the-service"></a>Implementación del servicio

Implementar el servicio con el archivo conda y el archivo de puntuación de los pasos anteriores.

```python
from azureml.core.webservice import Webservice
from azureml.core.model import InferenceConfig
from azureml.core.webservice import AciWebservice
from azureml.core.model import Model

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "Bank Marketing",  
                                                     "method" : "local_explanation"}, 
                                               description='Get local explanations for Bank marketing test data')

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score_local_explain.py",
                                   conda_file="myenv.yml")

# Use configs and models generated above
service = Model.deploy(ws, 'model-scoring', [scoring_explainer_model, original_model], inference_config, aciconfig)
service.wait_for_deployment(show_output=True)
```

### <a name="inference-using-test-data"></a>Inferencia con los datos de prueba

Inferencia con algunos datos de prueba para ver el valor predicho del modelo de aprendizaje automático automatizado, vista de la importancia de la característica diseñada para el valor predicho e importancia de la característica sin procesar para el valor predicho.

```python
if service.state == 'Healthy':
    # Serialize the first row of the test data into json
    X_test_json = X_test[:1].to_json(orient='records')
    print(X_test_json)
    # Call the service to get the predictions and the engineered and raw explanations
    output = service.run(X_test_json)
    # Print the predicted value
    print(output['predictions'])
    # Print the engineered feature importances for the predicted value
    print(output['engineered_local_importance_values'])
    # Print the raw feature importances for the predicted value
    print(output['raw_local_importance_values'])
```

### <a name="visualizations-to-aid-you-in-the-discovery-of-patterns-in-data-and-explanations-at-training-time"></a>Visualizaciones que le ayudan a la detección de patrones en los datos y explicaciones durante el entrenamiento

También puede visualizar la tabla de importancia de las características en el área de trabajo de [Azure Machine Learning Studio](https://ml.azure.com). Una vez finalizada la ejecución del aprendizaje automático automatizado, asegúrese de hacer clic en "Ver detalles del modelo", lo que lo conducirá a una ejecución específica. Desde aquí, haga clic en la pestaña "Explicaciones" para ver el panel de visualización de explicaciones. 

[![Arquitectura de interpretabilidad de Machine Learning](./media/machine-learning-interpretability-explainability/automl-explainability.png)](./media/machine-learning-interpretability-explainability/automl-explainability.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo se pueden habilitar las explicaciones del modelo y la importancia de las características en otras áreas del SDK fuera del aprendizaje automático automatizado, consulte el artículo de [conceptos](how-to-machine-learning-interpretability.md) sobre la interpretabilidad.