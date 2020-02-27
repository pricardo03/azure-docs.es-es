---
title: Interpretación de modelo en aprendizaje automático automatizado
titleSuffix: Azure Machine Learning
description: Aprenda a obtener explicaciones sobre cómo el modelo de aprendizaje automático automatizado determina la importancia de las características y realiza predicciones al usar el SDK de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: 4ab3bc43cf8ef479cb91d187a4c177db03415b86
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2020
ms.locfileid: "77525590"
---
# <a name="model-interpretability-in-automated-machine-learning"></a>Interpretación de modelo en aprendizaje automático automatizado

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, obtendrá información sobre cómo habilitar las características de interpretación para el aprendizaje automático automatizado con Azure Machine Learning. El aprendizaje automático automatizado le permite reconocer la importancia de las características sin procesar y las diseñadas. Para usar la interpretación del modelo, establezca `model_explainability=True` en el objeto `AutoMLConfig`.  

En este artículo aprenderá a:

- Interpretar durante el entrenamiento para el mejor o cualquier modelo.
- Habilitar visualizaciones para ayudarle a ver patrones en datos y explicaciones.
- Implementar la interpretación durante la inferencia o puntuación.

## <a name="prerequisites"></a>Prerrequisitos

- Características de interoperabilidad. Ejecute `pip install azureml-interpret azureml-contrib-interpret` para obtener los paquetes necesarios.
- Conocimiento de la compilación de experimentos de aprendizaje automático automatizado. Para más información sobre cómo usar el SDK de Azure Machine Learning, complete este [tutorial de modelo de regresión](tutorial-auto-train-models.md) o consulte cómo [configurar experimentos de aprendizaje automático automatizado](how-to-configure-auto-train.md).

## <a name="interpretability-during-training-for-the-best-model"></a>Interpretación durante el entrenamiento del mejor modelo

Recuperar la explicación de la `best_run`, que incluye explicaciones de las características diseñadas y las características sin procesar.

### <a name="download-engineered-feature-importance-from-artifact-store"></a>Descargue la importancia de las características diseñadas de la tienda de artefactos

Puede usar `ExplanationClient` para descargar las explicaciones de las características diseñadas del almacén de artefactos de `best_run`. Para obtener la explicación de las características sin procesar establecidas `raw=True`.

```python
from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient

client = ExplanationClient.from_run(best_run)
engineered_explanations = client.download_model_explanation(raw=False)
print(engineered_explanations.get_feature_importance_dict())
```

## <a name="interpretability-during-training-for-any-model"></a>Interpretación durante el entrenamiento de cualquier modelo 

Al calcular las explicaciones del modelo y visualizarlas, no está limitado a una explicación de modelo existente para un modelo de aprendizaje automático automatizado. También puede obtener una explicación del modelo con datos de prueba diferentes. Los pasos de esta sección muestran cómo calcular y visualizar la importancia de las características diseñadas y la importancia de las características sin procesar según los datos de prueba.

### <a name="retrieve-any-other-automl-model-from-training"></a>Recuperar cualquier otro modelo de aprendizaje automático automatizado del entrenamiento

```python
automl_run, fitted_model = local_run.get_output(metric='r2_score')
```

### <a name="set-up-the-model-explanations"></a>Configuración de las explicaciones del modelo

Utilice `automl_setup_model_explanations` para obtener las explicaciones de características diseñadas y sin procesar. `fitted_model` puede generar los siguientes elementos:

- Datos destacados de ejemplos entrenados o de prueba
- Listas de nombres de características diseñadas y sin formato
- Clases de búsqueda en la columna etiquetada de los escenarios de clasificación

`automl_explainer_setup_obj` contiene todas las estructuras de la lista anterior.

```python
from azureml.train.automl.runtime.automl_explain_utilities import AutoMLExplainerSetupClass, automl_setup_model_explanations

automl_explainer_setup_obj = automl_setup_model_explanations(fitted_model, X=X_train, 
                                                             X_test=X_test, y=y_train, 
                                                             task='classification')
```

### <a name="initialize-the-mimic-explainer-for-feature-importance"></a>Inicializar el explicador Mimic para la importancia de la característica

Para generar una explicación para modelos de aprendizaje automático automatizado, utilice la clase `MimicWrapper`. MimicWrapper se puede inicializar con estos parámetros:

- El objeto de configuración del explicador
- El área de trabajo
- Un modelo LightGBM que actúa como un modelo sustituto para el modelo de aprendizaje automático automatizado `fitted_model`

MimicWrapper también toma el objeto `automl_run` donde se cargarán las explicaciones sin procesar y diseñadas.

```python
from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
from azureml.explain.model.mimic_wrapper import MimicWrapper

# Initialize the Mimic Explainer
explainer = MimicWrapper(ws, automl_explainer_setup_obj.automl_estimator, LGBMExplainableModel,
                         init_dataset=automl_explainer_setup_obj.X_transform, run=automl_run,
                         features=automl_explainer_setup_obj.engineered_feature_names,
                         feature_maps=[automl_explainer_setup_obj.feature_map],
                         classes=automl_explainer_setup_obj.classes)
```

### <a name="use-mimicexplainer-for-computing-and-visualizing-engineered-feature-importance"></a>Use MimicExplainer para calcular y visualizar la importancia de las características diseñadas

Puede llamar al método `explain()` de MimicWrapper con los ejemplos de prueba transformados para obtener la importancia de las características diseñadas que se generaron. También puede usar `ExplanationDashboard` para la visualización del panel de valores de importancia de las características diseñadas generadas por el aprendizaje automático automatizado.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard
engineered_explanations = explainer.explain(['local', 'global'],              
                                            eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(engineered_explanations.get_feature_importance_dict())
ExplanationDashboard(engineered_explanations, automl_explainer_setup_obj.automl_estimator, automl_explainer_setup_obj.X_test_transform)
```

### <a name="use-mimic-explainer-for-computing-and-visualizing-raw-feature-importance"></a>Use MimicExplainer para calcular y visualizar la importancia de las características sin procesar

Puede llamar al método `explain()` de MimicWrapper de nuevo con los ejemplos de prueba transformados y la configuración `get_raw=True` para obtener la importancia de las características sin procesar que se generaron. También puede usar `ExplanationDashboard` para la visualización del panel de valores de importancia de las características sin procesar.

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

Use `TreeScoringExplainer` para crear el explicador de puntuación, que calculará los valores de importancia de las características sin procesar y diseñadas a la hora de la inferencia. El explicador de puntuación se inicializa con el `feature_map` que se calculó previamente. El explicador de puntuación utiliza el `feature_map` para devolver la importancia de las características sin procesar.

Guarde el explicador de puntuación y, a continuación, registre el modelo y el explicador de puntuación con el servicio de Administración de modelos. Ejecute el código siguiente:

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

Después, cree las dependencias de entorno necesarias en el contenedor para el modelo implementado. Tenga en cuenta que azureml-defaults con versión > = 1.0.45 debe indicarse como una dependencia de PIP, ya que contiene la funcionalidad necesaria para hospedar el modelo como un servicio web.

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
from azureml.core.webservice import AciWebservice
from azureml.core.model import Model, InferenceConfig
from azureml.core.environment import Environment

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                               memory_gb=1,
                                               tags={"data": "Bank Marketing",  
                                                     "method" : "local_explanation"},
                                               description='Get local explanations for Bank marketing test data')
myenv = Environment.from_conda_specification(name="myenv", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score_local_explain.py", environment=myenv)

# Use configs and models generated above
service = Model.deploy(ws,
                       'model-scoring',
                       [scoring_explainer_model, original_model],
                       inference_config,
                       aciconfig)
service.wait_for_deployment(show_output=True)
```

### <a name="inference-with-test-data"></a>Inferencia con datos de prueba

Realice la inferencia con algunos datos de prueba para ver el valor de predicción del modelo de aprendizaje automático automatizado. Vea la importancia de las características diseñadas y de las características sin procesar para el valor de predicción.

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

### <a name="visualize-to-discover-patterns-in-data-and-explanations-at-training-time"></a>Visualización para detectar patrones en datos y explicaciones durante el entrenamiento

Puede visualizar la tabla de importancia de las características en el área de trabajo de [Azure Machine Learning Studio](https://ml.azure.com). Una vez completada la ejecución de aprendizaje automático automatizado, seleccione **Ver detalles del modelo** para ver una ejecución específica. Seleccione la pestaña **Explicaciones** para ver el panel de visualización de explicaciones.

[![Arquitectura de interpretabilidad de Machine Learning](./media/how-to-machine-learning-interpretability-automl/automl-explainability.png)](./media/how-to-machine-learning-interpretability-automl/automl-explainability.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo se pueden habilitar las explicaciones del modelo y la importancia de las características en áreas del SDK de Azure Machine Learning distintas al aprendizaje automático automatizado, consulte el [artículo de conceptos sobre la interpretabilidad](how-to-machine-learning-interpretability.md).
