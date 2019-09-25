---
title: Interoperabilidad del modelo
titleSuffix: Azure Machine Learning
description: Aprenda a explicar los motivos por los que un modelo realiza predicciones mediante el SDK de Azure Machine Learning. Se puede usar durante el entrenamiento y la inferencia para saber cómo realiza las predicciones el modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 06/21/2019
ms.openlocfilehash: 6b825e61542dabc92baf482ede6c93edc486e059
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002354"
---
# <a name="model-interpretability-with-azure-machine-learning"></a>Interpretabilidad del modelo con Azure Machine Learning

En este artículo, aprenderá a explicar los motivos por los que el modelo realizó las predicciones con los distintos paquetes de interpretabilidad del de SDK de Python de Azure Machine Learning.

Mediante las clases y métodos del SDK, puede obtener:
+ Los valores de la importancia de las tanto para las características sin procesar como en las que se ha realizado ingeniería
+ Interpretabilidad en los conjuntos de datos del mundo real a escala, durante el entrenamiento y la inferencia.
+ Visualizaciones interactivas que le ayudan a detectar de patrones en los datos y explicaciones durante el entrenamiento

En la fase de entrenamiento del ciclo de desarrollo, tanto los evaluadores como los diseñadores de modelos pueden usar el resultado de la interoperabilidad de un modelo para comprobar las hipótesis y crear relaciones de confianza con las partes interesadas.  También usan la conclusiones sobre el modelo para la depuración, para validar que el comportamiento del modelo coincide con sus objetivos y para comprobar si hay sesgo.

En el aprendizaje automático, las **características** son los campos de datos que se usan para predecir un punto de datos de destino. Por ejemplo, para predecir el riesgo de crédito, se pueden usar los campos de datos de la edad, el tamaño de la cuenta y la antigüedad de la cuenta. En este caso, la edad, el tamaño de la cuenta y la antigüedad de la cuenta son **características**. La importancia de las características le indica la forma en que cada campo de datos ha afectado las predicciones del modelo. Por ejemplo, es posible que la edad se use mucho en la predicción, mientras que la antigüedad y el tamaño de la cuenta no afectan a la precisión de la predicción significativamente. Este proceso permite a los científicos de datos explicar las predicciones resultantes, con el fin de que partes interesadas puedan ver qué puntos de datos son los más importantes del modelo.

Mediante estas herramientas se pueden explicar los modelos de Machine Learning **de forma global en todos los datos** o **de forma local en un punto de datos específico** mediante el uso de tecnologías vanguardistas de un modo escalable y fácil de usar.

Las clases de interpretabilidad se ponen a disposición a través de varios paquetes de SDK. Aprenda a [instalar paquetes de SDK para Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [`azureml.explain.model`](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py), el paquete principal, que contiene funcionalidades compatibles con Microsoft.

* `azureml.contrib.explain.model`, versión preliminar, y las funcionalidades experimentales que puede probar.

* El paquete `azureml.train.automl.automlexplainer` para interpretar los modelos de Machine Learning automatizados.

> [!IMPORTANT]
> El contenido del espacio de nombres `contrib` no es totalmente compatible. A medida que maduran las funcionalidades experimentales, se mueven gradualmente al espacio de nombres principal.

## <a name="how-to-interpret-your-model"></a>Interpretación del modelo

Puede aplicar las clases y métodos de interpretabilidad para conocer el comportamiento global o las predicciones específicas del modelo. Al primero se le denomina explicación global y al último explicación local.

Los métodos también se pueden clasificar también en función de si el método es independiente del modelo o específico de un modelo. Algunos métodos van dirigidos a un tipo de modelos concreto. Por ejemplo, explicación del árbol de SHAP solo se aplica a modelos basados en árbol. Algunos métodos de tratan el modelo como una caja negra, como una explicación de imitación o explicación del kernel de SHAP. El paquete `explain` aprovecha estos diferentes enfoques basados en conjuntos de datos, tipos de modelo y casos de uso.

El resultado es un conjunto de información acerca de la forma en que un modelo dado realiza su predicción, como:
* Importancia de características relativas globales o locales

* Relación entre las características y las predicciones globales o locales

### <a name="explainers"></a>Explicaciones

Hay dos conjuntos de explicaciones: Explicaciones directas y metaexplicaciones en el SDK.

Las __explicaciones directas__ proceden de bibliotecas integradas. El SDK encapsula todas las explicaciones, con el fin de que expongan una API común y un formato de salida. Si se siente más cómodo usando directamente estas explicaciones, puede invocarlas directamente, en lugar de usar la API común y el formato de salida. A continuación encontrará una lista de las explicaciones disponibles en el SDK:

* **SHAP Tree Explainer**: La explicación del árbol de SHAP, que se centra en el algoritmo de cálculo rápido de valores SHAP de tiempo polinómicos específicos de los árboles y los conjuntos de árboles.
* **SHAP Deep Explainer**: Según la explicación de SHAP, Deep Explainer "es un algoritmo de aproximación de alta velocidad para los valores de SHAP en los modelos de aprendizaje profundo que se basa en una conexión con DeepLIFT descrita en el documento acerca de SHAP NIPS. Se admiten tanto los modelos de TensorFlow como modelos de Keras que usan el back-end TensorFlow (también hay compatibilidad preliminar con PyTorch)".
* **SHAP Kernel Explainer**: La explicación del kernel de SHAP utiliza una regresión lineal local ponderada especialmente para calcular los valores de SHAP para cualquier modelo.
* **Mimic Explainer**: La explicación de imitación se basa en la idea de modelos suplentes globales. Un modelo suplente global es un modelo interpretable de forma intrínseca que está entrenado para aproximarse a las predicciones de un modelo de caja negra de la forma más precisa posible. Los científicos de datos pueden interpretar el modelo suplente para extraer conclusiones acerca del modelo de caja negra. Cualquiera de los siguientes modelos se puede usar como modelo suplente: LightGBM (LinearExplainableModel), regresión lineal (LinearExplainableModel), modelo explicable de descenso de gradiente estocástico (SGDExplainableModel) y el árbol de decisiones (DecisionTreeExplainableModel).


* **Permutation Feature Importance Explainer**: La importancia de características de permutación es una técnica que se usa para explicar los modelos de clasificación y regresión que está inspirada en el [documento acerca de bosques aleatorios de Breiman](https://www.stat.berkeley.edu/%7Ebreiman/randomforest2001.pdf) (consulte la sección 10). A alto nivel, funciona de la siguiente manera, se barajan los datos de forma aleatoria, característica a característica, en todo el conjunto de datos y se calcula cuánto disminuye la métrica de rendimiento que interesa. Cuanto mayor sea el cambio, más importante es la característica.

* **LIME Explainer** (`contrib`): En función de LIME, la explicación de LIME usa el innovador algoritmo LIME (explicaciones independientes de los modelos interpretables locales) para crear modelos de suplente locales. A diferencia de los modelos suplentes globales, LIME se centra en entrenar modelos suplentes locales para explicar predicciones individuales.
* **HAN Text Explainer** (`contrib`): Explicación de texto HAN usa una red de atención jerárquica para obtener explicaciones de modelos a partir de datos de texto para un modelo de texto de caja negra dado. El modelo suplente de HAN se entrena en las salidas predichas de un modelo de profesor dado. Después del entrenamiento global en el corpus del texto, hemos agregado un paso de ajuste preciso para un documento específico, con el fin de mejorar la precisión de las explicaciones. HAN usa una red neuronal recurrente bidireccional con dos capas de atención, para prestar atención a las frases y a las palabras. Una vez que la red neuronal profunda se entrena con el modelo del profesor y se ajusta de forma precisa en un documento concreto, podemos extraer las importancias de las palabras de las capas de atención. Nos hemos dado cuenta de que HAN es más precisa que LIME o SHAP para datos de texto, pero también es más cara en términos de tiempo de entrenamiento. Sin embargo, hemos realizado mejoras en el tiempo de entrenamiento, ya que hemos dado al usuario la opción de inicializar la red con incrustaciones de palabras de GloVe, aunque sigue siendo lenta. El tiempo de entrenamiento se puede reducir considerablemente mediante la ejecución HAN en una máquina virtual con GPU de Azure remota. La implementación de HAN se describe en [Hierarchical Attention Networks for Document Classification (Yang et al., 2016)](https://www.researchgate.net/publication/305334401_Hierarchical_Attention_Networks_for_Document_Classification) (HAN para clasificación de documentos).


Las __metaexplicaciones__ seleccionan automáticamente, una explicación directa adecuada y generan la mejor información de la explicación según el modelo especificado y los conjuntos de datos. Las meta explicaciones aprovechan todas las bibliotecas (SHAP, LIME, Mimic, etc.) que hemos desarrollado o integrado. A continuación encontrará las metaexplicaciones disponibles en el SDK:

* **Tabular Explainer**: se utiliza con conjuntos de datos tabulares.
* **Text Explainer**: se usa con conjuntos de datos de texto.
* **Image Explainer**: se usa con conjuntos de datos de imágenes.

Además de la metaselección de explicaciones directas, las metaexplicaciones desarrollan características adicionales sobre las bibliotecas subyacentes y mejoran la velocidad y escalabilidad con respecto a las explicaciones directas.

Actualmente `TabularExplainer` emplea la siguiente lógica para invocar a las explicaciones SHAP directas:

1. Si es un modelo basado en árbol, aplique SHAP `TreeExplainer`, de lo contrario
2. Si es un modelo de red neuronal profunda, aplique SHAP `DeepExplainer`
3. Trátelo como un modelo de caja negra y aplique SHAP `KernelExplainer`

La inteligencia integrada en `TabularExplainer` aumentará su sofisticación a medida que se integran más bibliotecas en el SDK y se conocen las ventajas y desventajas de cada explicación.

`TabularExplainer` también ha realizado importantes mejoras en cuanto a características y rendimiento con respecto a las explicaciones directas:

* **Resumen del conjunto de datos de inicialización**. En los casos en que la velocidad de la explicación es lo más importante, resumimos el conjunto de datos de inicialización y generamos un pequeño conjunto de ejemplos representativos, lo que acelera tanto la explicación global como la local.
* **Muestreo del conjunto de datos de evaluación**. Si el usuario pasa un gran conjunto de ejemplos de evaluación, pero no necesita que se evalúen todos, el parámetro de muestreo puede establecerse en true para acelerar la explicación global.

El siguiente diagrama muestra la estructura actual de las explicaciones directas y de las metaexplicaciones.

[![Arquitectura de interpretabilidad de Machine Learning](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)


### <a name="models-supported"></a>Modelos que se admiten

Todos los modelos que se entrenan con conjuntos de datos en el formato de `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData` o `scipy.sparse.csr_matrix` de Python son compatibles con el paquete `explain` de interpretabilidad del SDK.

Las funciones de explicación aceptan como entrada tanto modelos como canalizaciones. Si se proporciona un modelo, este debe implementar la función de predicción `predict` o `predict_proba` que se ajuste a la convención de Scikit. Si se proporciona una canalización (nombre del script de la canalización), la función de la explicación da por supuesto que el script de canalización de ejecución devuelve una predicción. Se admiten modelos entrenados mediante los marcos de aprendizaje profundo Keras, TensorFlow y PyTorch.

### <a name="local-and-remote-compute-target"></a>Destino de proceso local y remoto

El paquete `explain` está diseñado para trabajar con destinos de proceso locales y remotos. Si se ejecutan localmente, las funciones el SDK no entrarán en contacto con ningún servicio de Azure. Puede ejecutar una explicación de forma remota en Proceso de Azure Machine Learning y registrar la información de la explicación en servicios de historial de ejecución de Azure Machine Learning. Una vez que esta información se registra, tanto los informes como las visualizaciones de la explicación están disponibles en el portal del área de trabajo de Azure Machine Learning para que los pueda analizar el usuario.

## <a name="interpretability-in-training"></a>Interoperabilidad en el entrenamiento

### <a name="train-and-explain-locally"></a>Entrenamiento y explicación locales

1. Entrene el modelo en un cuaderno de Jupyter Notebook local.

    ```python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    
    # split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data,            
                                                        breast_cancer_data.target,  
                                                        test_size=0.2,
                                                        random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Llame a la explicación: Para inicializar un objeto de explicación, es preciso que pasar el modelo y algunos datos de entrenamiento al constructor de la explicación. Opcionalmente, también puede pasar los nombres de las características y los nombres de las clases de salida (si está realizando la clasificación) que se usarán para que las explicaciones y visualizaciones sean más informativas. Así es como se crea una instancia de un objeto de explicación mediante [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py), [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py) y [PFIExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.permutation.permutation_importance.pfiexplainer?view=azure-ml-py) localmente. `TabularExplainer` llama a una de los tres explicaciones SHAP de debajo (`TreeExplainer`, `DeepExplainer` o `KernelExplainer`) y se selecciona automáticamente la más adecuada para su caso de uso. Sin embargo, es posible llamar directamente a cada una de las tres explicaciones subyacentes.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=breast_cancer_data.feature_names, 
                                 classes=classes)
    ```

    o

    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    
    # you can use one of the following four interpretable models as a global surrogate to the black box model
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
    from azureml.explain.model.mimic.models.linear_model import LinearExplainableModel
    from azureml.explain.model.mimic.models.linear_model import SGDExplainableModel
    from azureml.explain.model.mimic.models.tree_model import DecisionTreeExplainableModel

    # "features" and "classes" fields are optional
    # augment_data is optional and if true, oversamples the initialization examples to improve surrogate model accuracy to fit original model.  Useful for high-dimensional data where the number of rows is less than the number of columns. 
    # max_num_of_augmentations is optional and defines max number of times we can increase the input data size.
    # LGBMExplainableModel can be replaced with LinearExplainableModel, SGDExplainableModel, or DecisionTreeExplainableModel
    explainer = MimicExplainer(model, 
                               x_train, 
                               LGBMExplainableModel, 
                               augment_data=True, 
                               max_num_of_augmentations=10, 
                               features=breast_cancer_data.feature_names, 
                               classes=classes)
    ```
   o

    ```python
    from azureml.explain.model.permutation.permutation_importance import PFIExplainer 
    
    # "features" and "classes" fields are optional
    explainer = PFIExplainer(model, 
                             features=breast_cancer_data.feature_names, 
                             classes=classes)
    ```

3. Obtenga los valores de la importancia de las características globales.

    ```python
    # you can use the training data or the test data here
    global_explanation = explainer.explain_global(x_train)
    
    # if you used the PFIExplainer in the previous step, use the next line of code instead
    # global_explanation = explainer.explain_global(x_train, true_labels=y_test)

    # sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))

    # alternatively, you can print out a dictionary that holds the top K feature names and values
    global_explanation.get_feature_importance_dict()
    ```

4. Obtenga valores de la importancia de la características locales: use las siguientes llamadas a funciones para explicar una instancia individual o un grupo de instancias. Tenga en cuenta que PFIExplainer no admite explicaciones locales.

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

### <a name="train-and-explain-remotely"></a>Entrenamiento y explicación remotas

Aunque puede entrenar en los distintos destinos de proceso compatibles con Azure Machine Learning, el ejemplo de esta sección muestra cómo hacerlo con un destino de Proceso de Azure Machine Learning.

1. Cree un script de entrenamiento en un cuaderno local de Jupyter Notebook (por ejemplo, run_explainer.py).

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    from azureml.core.run import Run

    run = Run.get_context()
    client = ExplanationClient.from_run(run)

    # write code to get and split your data into train and test sets here
    # write code to train your model here 

    # explain predictions on your local machine
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                x_train, 
                                features=feature_names, 
                                classes=classes)

    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    
    # uploading global model explanation data for storage or visualization in webUX
    # the explanation can then be downloaded on any compute
    # multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Siga las instrucciones de [Configuración de destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md#amlcompute) para aprender a configurar Proceso de Azure Machine Learning como destino de proceso y a enviar una ejecución de entrenamiento.

3. Descargue la explicación en el cuaderno de Jupyter Notebook local.

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    
    client = ExplanationClient.from_run(run)
    
    # get model explanation data
    explanation = client.download_model_explanation()
    # or only get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="visualizations"></a>Visualizaciones

Use el panel de visualización para conocer e interpretar el modelo:

### <a name="global-visualizations"></a>Visualizaciones globales

Los siguientes gráficos proporcionan una vista global del modelo entrenado, junto con sus predicciones y explicaciones.

|Gráfico|DESCRIPCIÓN|
|----|-----------|
|Exploración de datos| Información general del conjunto de datos, junto con los valores de predicción.|
|Importancia global|Muestra las principales características importantes de K (K configurable) globalmente. Este gráfico es útil para conocer el comportamiento global del modelo subyacente.|
|Exploración de la explicación|Muestra hasta qué punto una característica es responsable de realizar un cambio en los valores de predicción de un modelo (o la probabilidad de los valores de predicción). |
|Resumen| Usa los valores de importancia de características locales con signo en todos los puntos de datos para mostrar la distribución del impacto que tiene cada característica en el valor de predicción.|

[![Panel de visualización global](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Visualizaciones locales

Puede hacer clic en cualquier punto de datos individual en cualquier momento de los gráficos anteriores para cargar el gráfico de importancia de características locales para el punto de datos determinado.

|Gráfico|DESCRIPCIÓN|
|----|-----------|
|Importancia local|Muestra las principales características importantes de K (K configurable) globalmente. Este gráfico es útil para conocer el comportamiento local del modelo subyacente en un punto de datos concreto.|
|Exploración de perturbaciones|Permite cambiar los valores de las características del punto de datos seleccionado y observar cómo van a afectar dichos cambios el valor de predicción.|
|Expectativa condicional individual (ICE)| Permite cambiar el valor de una característica de un valor mínimo a un valor máximo para ver cómo cambia la predicción del punto de datos cuando cambia una característica.|

[![Importancia de características locales del panel de visualización](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)


[![Perturbation de características del panel de visualización](./media/machine-learning-interpretability-explainability/perturbation.gif)](./media/machine-learning-interpretability-explainability/perturbation.gif#lightbox)


[![Gráficos ICE del panel de visualización](./media/machine-learning-interpretability-explainability/ice-plot.png)](./media/machine-learning-interpretability-explainability/ice-plot.png#lightbox)

Tenga en cuenta que deberá tener extensiones de widget del panel visualización habilitadas antes de iniciar el kernel de Jupyter.

* Jupyter Notebook

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.explain.model.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.explain.model.visualize
    ```



* Laboratorios de Jupyter

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```
Para cargar el panel de visualización, use el siguiente código:

```python
from azureml.contrib.explain.model.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

## <a name="raw-feature-transformations"></a>Transformaciones de características sin procesar

Si lo desea, puede pasar la canalización de transformación de características a la explicación para recibir explicaciones en cuanto a las características sin procesar antes de la transformación (en lugar de las características procesadas). Si omite este paso, la explicación proporciona explicaciones en cuanto a las características procesadas.

El formato de las transformaciones que se admiten es el mismo que se describe en [sklearn pandas](https://github.com/scikit-learn-contrib/sklearn-pandas). En general, se admiten todas las transformaciones, siempre que operen en una sola columna y, por tanto, sean claramente uno a varios. 

Las características sin procesar se pueden explicar usando un `sklearn.compose.ColumnTransformer` o una lista de tuplas de transformadores apropiadas. La celda siguiente usa `sklearn.compose.ColumnTransformer`. 

```python
from sklearn.compose import ColumnTransformer

numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)])

# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=preprocessor)
```

Si desea ejecutar el ejemplo con la lista de tuplas de transformadores apropiadas, use el siguiente código: 
```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(
    strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(
    handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# append model to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                      ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=transformations)
```

## <a name="interpretability-at-inferencing-time"></a>Interpretabilidad en el momento de la inferencia

La explicación se puede implementar junto con el modelo original y se puede utilizar para proporcionar la información de explicación local. También podemos ofrecer explicaciones de puntuación ligeros para mejorar el rendimiento de la interpretabilidad en el momento de la inferencia. El proceso de implementación de una explicación de puntuación ligera es similar al de implementación de un modelo e incluye los siguientes pasos:




1. Crear un objeto de explicación (por ejemplo, mediante TabularExplainer):

   ```python
   from azureml.contrib.explain.model.tabular_explainer import TabularExplainer

   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Cree una explicación de puntuación mediante el objeto de explicación:

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import KernelScoringExplainer, save

   # create a lightweight explainer at scoring time
   scoring_explainer = KernelScoringExplainer(explainer)

   # pickle scoring explainer
   # pickle scoring explainer locally
   OUTPUT_DIR = 'my_directory'
   save(scoring_explainer, directory=OUTPUT_DIR, exist_ok=True)
   ```

1. Configure y registre una imagen que utilice al modelo de explicación de puntuación.

   ```python
   # register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   # scoring_explainer.pkl is the filename on disk, while my_scoring_explainer.pkl will be the filename in cloud storage
   run.upload_file('my_scoring_explainer.pkl', os.path.join(OUTPUT_DIR, 'scoring_explainer.pkl'))
   
   scoring_explainer_model = run.register_model(model_name='my_scoring_explainer', 
                                                model_path='my_scoring_explainer.pkl')
   print(scoring_explainer_model.name, scoring_explainer_model.id, scoring_explainer_model.version, sep = '\t')
   ```

1. [Opcional] Recuperar la explicación de la puntuación de la nube y probar las explicaciones

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import load

   # retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'my_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # load scoring explainer from disk
   scoring_explainer = load(scoring_explainer_model_path)

   # test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. Implemente la imagen en un destino de proceso:

   1. Cree un archivo de puntuación (antes de este paso, siga los de [Implementación de modelos con Azure Machine Learning ](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) para registrar el modelo de predicción original)

        ```python
        %%writefile score.py
        import json
        import numpy as np
        import pandas as pd
        import os
        import pickle
        from sklearn.externals import joblib
        from sklearn.linear_model import LogisticRegression
        from azureml.core.model import Model

        def init():

            global original_model
            global scoring_model

            # retrieve the path to the model file using the model name
            # assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('my_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

        def run(raw_data):
            # get predictions and explanations for each data point
            data = pd.read_json(raw_data)
            # make prediction
            predictions = original_model.predict(data)
            # retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # you can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
        ```

   1. Defina la configuración de implementación (esta configuración depende de los requisitos del modelo. En el ejemplo siguiente se define una configuración que usa un núcleo de CPU y 1 GB de memoria)

        ```python
        from azureml.core.webservice import AciWebservice

        aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                       memory_gb=1,
                                                       tags={"data": "NAME_OF_THE_DATASET",
                                                             "method" : "local_explanation"},
                                                       description='Get local explanations for NAME_OF_THE_PROBLEM')
        ```

   1. Cree un archivo con las dependencias del entorno

        ```python
        from azureml.core.conda_dependencies import CondaDependencies

        # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)

        azureml_pip_packages = ['azureml-defaults', 'azureml-contrib-explain-model', 'azureml-core', 'azureml-telemetry', 'azureml-explain-model']
 

        # specify CondaDependencies obj
        myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas'],
                                         pip_packages=['sklearn-pandas'] + azureml_pip_packages,
                                         pin_sdk_version=False)


        with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())

        with open("myenv.yml","r") as f:
            print(f.read())
        ```

   1. Cree un dockerfile personalizado con g ++ instalado

        ```python
        %%writefile dockerfile
        RUN apt-get update && apt-get install -y g++
        ```

   1. Implemente la imagen creada (tiempo estimado: 5 minutos)

        ```python
        from azureml.core.webservice import Webservice
        from azureml.core.image import ContainerImage

        # use the custom scoring, docker, and conda files we created above
        image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                        docker_file="dockerfile",
                                                        runtime="python",
                                                        conda_file="myenv.yml")

        # use configs and models generated above
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

    # create data to test service with
    examples = x_list[:4]
    input_data = examples.to_json()

    headers = {'Content-Type':'application/json'}

    # send request to service
    resp = requests.post(service.scoring_uri, input_data, headers=headers)

    print("POST to url", service.scoring_uri)
    # can covert back to Python objects from json string if desired
    print("prediction:", resp.text)
    ```

1. Limpieza: Para eliminar un servicio web implementado, use `service.delete()`.




## <a name="next-steps"></a>Pasos siguientes

Para ver una colección de cuadernos de Jupyter Notebook que muestran las instrucciones anteriores, consulte [Cuadernos de ejemplo de interpretabilidad de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
