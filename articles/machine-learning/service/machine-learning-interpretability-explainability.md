---
title: Interoperabilidad del modelo
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo explicar por qué el modelo realiza predicciones mediante el SDK de Azure Machine Learning. Se puede usar durante el entrenamiento y la inferencia para comprender cómo el modelo realiza predicciones.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 04/29/2019
ms.openlocfilehash: 4261e869fe17283886d7d8ea8101e03110d6dad4
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851994"
---
# <a name="model-interpretability-with-azure-machine-learning-service"></a>Interoperabilidad de modelo con el servicio de Azure Machine Learning

En este artículo, aprenderá a explicar por qué el modelo realiza las predicciones hizo con el paquete de interoperabilidad de SDK de Python de Azure Machine Learning.

Mediante las clases y métodos de este paquete, puede obtener:
+ Interoperabilidad en los conjuntos de datos del mundo real a escala, durante el aprendizaje e inferencia. 
+ Visualizaciones interactivas que le ayudarán a la detección de patrones en los datos y obtener una explicación en tiempo de entrenamiento
+ Los valores de la importancia de características: características sin procesar y de ingeniería

Durante la fase de aprendizaje del ciclo de desarrollo, evaluadores y los diseñadores de modelos pueden usar para explicar el resultado de un modelo a las partes interesadas para crear relaciones de confianza.  También usan la información sobre el modelo para la depuración, validar el comportamiento del modelo coincida con sus objetivos y para comprobar si el sesgo.

Inferencia o modelo de puntuación, es la fase donde se usa el modelo implementado para la predicción, con más frecuencia en los datos de producción. Durante esta fase, los científicos de datos pueden explicar las predicciones a las personas que utilizan el modelo resultantes. Por ejemplo, ¿por qué el modelo denegar un préstamo hipotecario o predecir una cartera de inversiones que lleva a un riesgo más alto?

Con estas ofertas, puede explicar los modelos de aprendizaje automático **globalmente en todos los datos**, o **localmente en un punto de datos específico** mediante las tecnologías de última generación de forma escalable y fácil de usar.

Las clases de interoperabilidad se ponen a disposición a través de dos paquetes de Python. Obtenga información sobre cómo [instalar paquetes de SDK para Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [`azureml.explain.model`](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py), el paquete principal, que contiene funcionalidades compatibles con Microsoft. 

* `azureml.contrib.explain.model`, versión preliminar y funcionalidades experimentales que puede probar.

> [!IMPORTANT]
> Las cosas en contrib no son totalmente compatibles. Como las funcionalidades experimentales maduran, gradualmente se moverán al paquete principal.

## <a name="how-to-interpret-your-model"></a>Cómo interpretar el modelo

Puede aplicar las clases de interoperabilidad y métodos para comprender el comportamiento global del modelo o las predicciones específicas. El primero se denomina explicación global y este último se denomina explicación local.

Los métodos se pueden clasificar también en función de si el método es independiente del modelo o un modelo específico. Algunos métodos de destino determinados tipos de modelos. Por ejemplo, explicación del árbol de formas solo se aplica a modelos basados en árbol. Algunos métodos de tratan el modelo como una caja negra, como una explicación imitar o explicación del kernel de formas. El `explain` paquete aprovecha estos distintos enfoques basados en conjuntos de datos, tipos de modelo y casos de uso. 

El resultado es un conjunto de información sobre cómo un modelo determinado realiza su predicción, como:
* Importancia de características relativas de global o local

* Relación de característica y predicción global o local

### <a name="explainers"></a>Explainers

Hay dos conjuntos de explainers: Explainers directos y Explainers Meta en el SDK.

__Dirigir explainers__ proceden de las bibliotecas integradas. El SDK incluye todas las explainers para que exponen una API común y el formato de salida. Si se siente más cómodo directamente mediante estas explainers, se pueden invocar directamente en lugar de usar la API común y el formato de salida. Los siguientes son una lista de los explainers directos disponibles en el SDK:

* **Explicación de árbol**: Explicación de árbol de formas, que se centra en polinómica tiempo rápido formas valor estimación específico del algoritmo árboles y conjuntos de árboles de.
* **Explicación profundo**: Según la explicación de formas, explicación profundo "es un algoritmo de aproximación de alta velocidad para los valores de formas en los modelos de aprendizaje profundo que se basa en una conexión con DeepLIFT descrito en el documento de NIPS formas. Se admiten los modelos de TensorFlow y modelos de Keras con TensorFlow back-end (también hay compatibilidad preliminar con PyTorch) ".
* **Explicación del kernel**: Explicación del Kernel de formas utiliza una regresión lineal local especialmente ponderada para calcular los valores de formas para cualquier modelo.
* **Explicación de imitar**: Imitar explicación se basa en la idea de los modelos de suplente global. Un modelo de suplente global es un modelo de forma intrínseca interpretable está entrenado para aproximar las predicciones de un modelo de caja negra de forma más precisa posible. Científico de datos puede interpretar el modelo de suplentes para extraer conclusiones sobre el modelo de caja negra.
* **Explicación de LIMA** (`contrib`): En función de las CAL, LIMA explicación utiliza el algoritmo de última generación Local interpretable independiente del modelo explicaciones (CAL) para crear modelos de suplente local. A diferencia de los modelos de suplente global LIMA se centra en entrenar modelos de suplente local para explicar las predicciones individuales.
* **Explicación de texto HAN** (`contrib`): Explicación de texto HAN usa una red de atención jerárquica para obtener explicaciones de modelo de datos de texto para un modelo de texto determinado cuadro negro. Se entrena el modelo de suplentes de patrón en salidas de predicción de un modelo determinado de profesor. Después del entrenamiento global en el corpus de texto, hemos agregado un paso de ajuste preciso para un documento específico con el fin de mejorar la precisión de las explicaciones. HAN usa un RNN bidireccional con dos capas de atención, palabras y oraciones requiere atención. Una vez que la DNN se entrenó el modelo de profesor para ajustarlos en un documento concreto, podemos extraer el importances word de los niveles de atención. Hemos encontrado HAN de ser más precisos que LIMA o formas con datos de texto, pero más costosos en términos de tiempo también de entrenamiento. Sin embargo, hemos realizado mejoras en el tiempo de entrenamiento por proporcionar al usuario la opción para inicializar la red con guante incrustaciones de palabras, aunque sigue siendo lenta. El tiempo de entrenamiento se puede mejorar considerablemente mediante la ejecución HAN en una máquina virtual GPU de Azure remota. La implementación del patrón se describe en "Redes de atención jerárquica para clasificación de documentos (Yang et al, 2016)" ([https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf)).

__Explainers meta__ automáticamente, seleccione una explicación directo adecuado y generar la mejor información de explicación según el modelo especificado y los conjuntos de datos. El explainers meta aprovechan todas las bibliotecas (formas, LIMA, imagen, etc.) que hemos desarrollado o integrada. Estos son los explainers meta disponibles en el SDK:

* **Explicación tabular**: Se utiliza con conjuntos de datos tabulares.
* **Explicación del texto**: Puede usar con los conjuntos de datos de texto.

Además a Meta selección de la explainers directas, explainers meta desarrollar características adicionales a las bibliotecas subyacentes y mejoran la velocidad y escalabilidad a través de la explainers directas.

Actualmente `TabularExplainer` emplea la lógica siguiente para invocar la Explainers directa:

1. Si es un modelo basado en el árbol, se aplican `TreeExplainer`de lo contrario,
2. Si es un modelo DNN, aplicar `DeepExplainer`de lo contrario,
3. Tratarlo como un modelo de caja negra y aplicar `KernelExplainer`

La inteligencia integrada en `TabularExplainer` son más sofisticados como más bibliotecas están integradas en el SDK y Aprendemos sobre las ventajas y desventajas de cada explicación.

`TabularExplainer` También se ha realizado importantes mejoras de características y rendimiento a través de la Explainers directa:

* **Resumen del conjunto de datos de inicialización**. En los casos en que la velocidad de explicación es más importante, nos resumir el conjunto de datos de inicialización y generar un pequeño conjunto de ejemplos representativos, lo que acelera la explicación global y local.
* **El conjunto de datos de evaluación de muestreo**. Si el usuario pasa de un gran conjunto de ejemplos de evaluación, pero realmente no necesita todos ellos va a evaluar, el parámetro de muestreo puede establecerse en true para acelerar la explicación global.

El siguiente diagrama muestra la relación entre los dos conjuntos de direct y explainers meta.

[![Arquitectura de interoperabilidad de Machine Learning](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)

### <a name="models-supported"></a>Modelos admitidos

Los modelos que se entrenan con conjuntos de datos en Python `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData`, o `scipy.sparse.csr_matrix` formato son compatibles con la interoperabilidad de `explain` paquete del SDK.

Las funciones de explicación aceptan modelos y las canalizaciones como entrada. Si se proporciona un modelo, el modelo debe implementar la función de predicción `predict` o `predict_proba` que se ajusta a la convención de Scikit. Si se proporciona una canalización (nombre de la secuencia de comandos de la canalización), la función de la explicación se da por supuesto que el script de canalización de ejecución devuelve una predicción.

### <a name="local-and-remote-compute-target"></a>Destino de proceso local y remota

El `explain` paquete está diseñado para trabajar con ambos destinos de proceso local y remota. Si se ejecutan localmente, las funciones el SDK no se comunicará con los servicios de Azure. Puede ejecutar una explicación de forma remota en Azure Machine Learning Compute y registrar la información de la explicación en servicios de historial de ejecución de Azure Machine Learning. Una vez que esta información se registra, informes y visualizaciones de la explicación están disponibles en el portal del área de trabajo de Azure Machine Learning para el análisis de usuario.

## <a name="interpretability-in-training"></a>Interoperabilidad de entrenamiento

### <a name="train-and-explain-locally"></a>Entrenar y explique localmente

1. Entrenar el modelo en un cuaderno de Jupyter local. 

    ``` python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    # Split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data, breast_cancer_data.target, test_size=0.2, random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Llame a la explicación: Para inicializar un objeto de la explicación, deberá pasar el modelo y algunos datos de entrenamiento al constructor de la explicación. Opcionalmente, también puede pasar en los nombres de las características y los nombres de clase de salida (si está realizando la clasificación) que se usará para realizar sus explicaciones y visualizaciones más informativo. Aquí le mostramos cómo crear una instancia de un objeto de explicación mediante [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py) y [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py) localmente. `TabularExplainer` se llama a uno de los tres explainers debajo (`TreeExplainer`, `DeepExplainer`, o `KernelExplainer`) y se selecciona automáticamente uno más adecuada para su caso de uso. Sin embargo, puede llamar directamente a cada uno de sus tres explainers subyacentes.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    ```
    o
    
    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel

    # "features" and "classes" fields are optional
    explainer = MimicExplainer(model, x_train, LGBMExplainableModel, features=breast_cancer_data.feature_names, classes=classes)
    ```

3. Obtener valores de la importancia de la función global.

    ```python
    # You can use the training data or the test data here
    global_explanation = explainer.explain_global(x_train)
    # Sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))
    ```

4. Los valores de importancia de características locales: use la siguiente función que llama a explicar una instancia individual o un grupo de instancias.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0])
    
    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```
    o
    ```python
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:4])
    
    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

### <a name="train-and-explain-remotely"></a>Entrenar y explicar de forma remota

Aunque puede entrenar en los distintos destinos de proceso compatibles con el servicio Azure Machine Learning, el ejemplo de esta sección muestra cómo hacerlo con un destino de Azure Machine Learning Compute.

1. Crear un script de entrenamiento en un cuaderno de Jupyter local (por ejemplo, run_explainer.py).

    ``` python  
    run = Run.get_context()
    client = ExplanationClient.from_run(run)
    
    # Train your model here

    # explain predictions on your local machine   
    # "features" and "classes" fields are optional 
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    # explain local data points (individual instances)
    local_explanation = explainer.explain_local(x_test[0])
    # upload global and local explanation objects to Run History
    client.upload_model_explanation(run, local_explanation, top_k=2, comment='local explanation: top 2 features')
    # Uploading global model explanation data for storage or visualization in webUX
    # The explanation can then be downloaded on any compute
    # Multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # Or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Siga las instrucciones de [configurar destinos de proceso de entrenamiento del modelo](how-to-set-up-training-targets.md#amlcompute) para obtener información sobre cómo configurar un proceso de Azure Machine Learning como destino de proceso y envía la ejecución de entrenamiento.

3. Descargue la explicación en el cuaderno de Jupyter local. 


    ``` python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    # Get model explanation data
    client = ExplanationClient.from_run(run)
    explanation = client.download_model_explanation()
    local_importance_values = explanation.local_importance_values
    expected_values = explanation.expected_values
    # Or you can use the saved run.id to retrive the feature importance values
    client = ExplanationClient.from_run_id(ws, experiment_name, run.id)
    explanation = client.download_model_explanation()
    local_importance_values = explanation.local_importance_values
    expected_values = explanation.expected_values
    # Get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="visualizations"></a>Visualizaciones

Use el panel de visualización para comprender e interpretar el modelo:

### <a name="global-visualizations"></a>Visualizaciones globales

Los siguientes gráficos proporcionan una vista global del modelo entrenado, junto con sus predicciones y explicaciones.

|Trazado|DESCRIPCIÓN|
|----|-----------|
|Exploración de datos| Información general sobre el conjunto de datos junto con los valores de predicción.|
|Importancia global|Muestra las principales características importantes de K (K configurable) globalmente. Este gráfico es útil para comprender el comportamiento global del modelo subyacente.|
|Exploración de explicación|Muestra cómo una característica es responsable de realizar un cambio en los valores de predicción del modelo (o la probabilidad de que los valores de predicción). |
|Resumen| Usa los valores de importancia de características locales con signo en todos los puntos de datos para mostrar la distribución del impacto que tiene cada característica en el valor de predicción.|

[![Panel de visualización Global](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Visualizaciones locales
Puede hacer clic en cualquier punto de datos individuales en cualquier momento de los trazados anteriores para cargar el trazado de importancia de características locales para el punto de datos determinado.

|Trazado|DESCRIPCIÓN|
|----|-----------|
|Importancia local|Muestra las principales características importantes de K (K configurable) globalmente. Este gráfico es útil para comprender el comportamiento local del modelo subyacente en un punto de datos específico.|

[![Local del panel de visualización](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)

Para cargar el panel de visualización, use el código siguiente:

```python
from azureml.contrib.explain.model.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
``` 

## <a name="raw-feature-transformations"></a>Transformaciones de características sin procesar

Si lo desea, puede pasar la canalización de transformación de características para la explicación para recibir explicaciones en cuanto a las características sin procesar antes de la transformación (en lugar de ingeniería de características). Si omite este paso, la explicación proporciona explicaciones en cuanto a las características diseñadas. 

El formato de transformaciones que se admiten es la mismo que la descrita en [sklearn pandas](https://github.com/scikit-learn-contrib/sklearn-pandas). En general, se admiten las transformaciones, siempre que operan en una sola columna y, por tanto, son claramente uno a varios.

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# Assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# Append model to preprocessing pipeline.
# Now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                    ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# Pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1], initialization_examples=x_train, features=dataset_feature_names, classes=dataset_classes, transformations=transformations)
```

## <a name="interpretability-in-inference"></a>Interoperabilidad de inferencia

La explicación se puede implementar junto con el modelo original y puede utilizarse para proporcionar la información local explicación en tiempo de puntuación. El proceso de implementación de una explicación de la puntuación es similar a la implementación de un modelo e incluye los siguientes pasos:

1. Cree un objeto de explicación:
   ```python
   from azureml.contrib.explain.model.tabular_explainer import TabularExplainer

   explainer = TabularExplainer(model, x_test)
   ``` 

1. Cree una explicación de puntuación mediante el objeto de explicación:
   ```python
   scoring_explainer = explainer.create_scoring_explainer(x_test)

   # Pickle scoring explainer
   scoring_explainer_path = scoring_explainer.save('scoring_explainer_deploy')
   ``` 

1. Configurar y registrar una imagen que utiliza el modelo de puntuación de explicación.
   ```python
   # Register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   run.upload_file('breast_cancer_scoring_explainer.pkl', scoring_explainer_path)
   model = run.register_model(model_name='breast_cancer_scoring_explainer', model_path='breast_cancer_scoring_explainer.pkl')
   print(model.name, model.id, model.version, sep = '\t')
   ``` 

1. [Opcional] Recuperar la explicación de la puntuación de la nube y las explicaciones de prueba
   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import ScoringExplainer

   # Retreive the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'breast_cancer_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # Load scoring explainer from disk
   scoring_explainer = ScoringExplainer.load(scoring_explainer_model_path)

   # Test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. Implementar la imagen en un destino de proceso:

   1. Crear un archivo de puntuación (antes de este paso, siga los pasos de [implementar modelos con el servicio de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) para registrar el modelo de predicción original)
        ```python
        %%writefile score.py
        import json
        import numpy as np
        import os
        import pickle
        from sklearn.externals import joblib
        from sklearn.linear_model import LogisticRegression
        from azureml.core.model import Model

        def init():

            global original_model
            global scoring_model

            # Retrieve the path to the model file using the model name
            # Assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('breast_cancer_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

        def run(raw_data):
            # Get predictions and explanations for each data point
            data = np.array(json.loads(raw_data)['data'])
            # Make prediction
            predictions = original_model.predict(data)
            # Retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # You can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
        ``` 
    1. Definir la configuración de implementación (esta configuración depende de los requisitos del modelo. El ejemplo siguiente define una configuración que usa un núcleo de CPU y 1 GB de memoria)
        ```python
        from azureml.core.webservice import AciWebservice

        aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                                       memory_gb=1, 
                                                       tags={"data": "breastcancer",  
                                                             "method" : "local_explanation"}, 
                                                       description='Get local explanations for breast cancer data')
        ``` 

    1. Cree un archivo con las dependencias del entorno

        ```python
        from azureml.core.conda_dependencies import CondaDependencies 

        # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)


        myenv = CondaDependencies.create(pip_packages=["azureml-defaults", "azureml-explain-model", "azureml-contrib-explain-model"], 
                                        conda_packages=["scikit-learn"])

        with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())
            
        with open("myenv.yml","r") as f:
            print(f.read())
        ``` 
    1. Crear un dockerfile personalizado con g ++ instalado

        ```python
        %%writefile dockerfile
        RUN apt-get update && apt-get install -y g++  
        ``` 
    1. Implementar la imagen creada (tiempo estimado: 5 minutos)
        ```python
        from azureml.core.webservice import Webservice
        from azureml.core.image import ContainerImage

        # Use the custom scoring, docker, and conda files we created above
        image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                        docker_file="dockerfile", 
                                                        runtime="python", 
                                                        conda_file="myenv.yml")

        # Use configs and models generated above
        service = Webservice.deploy_from_model(workspace=ws,
                                            name='model-scoring-service',
                                            deployment_config=aciconfig,
                                            models=[scoring_explainer_model, original_model],
                                            image_config=image_config)

        service.wait_for_deployment(show_output=True)
        ``` 

1. Prueba de la implementación
    ```python
    import requests

    # Create data to test service with
    x_list = x_test.tolist()
    examples = x_list[:4]
    input_data = "{\"data\": " + str(examples) + "}"

    headers = {'Content-Type':'application/json'}

    # send request to service
    resp = requests.post(service.scoring_uri, input_data, headers=headers)

    print("POST to url", service.scoring_uri)
    # can covert back to Python objects from json string if desired
    print("prediction:", resp.text)
    ``` 

1. Limpiar: Para eliminar un servicio web implementado, use `service.delete()`.

## <a name="next-steps"></a>Pasos siguientes

Para ver una colección de instancias de Jupyter Notebook que muestran las instrucciones anteriores, consulte el [cuadernos de ejemplo de interoperabilidad de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
