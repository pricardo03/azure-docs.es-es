---
title: Interoperabilidad del modelo
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo explicar por qué el modelo realiza predicciones mediante el SDK de interoperabilidad de Azure Machine Learning. Se puede usar durante el entrenamiento e inferencia para comprender cómo el modelo realiza predicciones.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 04/09/2019
ms.openlocfilehash: 668551603dfa2a9c42f4538fd9a66ee646e1feb7
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682464"
---
# <a name="model-interpretability-with-azure-machine-learning-service"></a>Interoperabilidad de modelo con el servicio de Azure Machine Learning

En este artículo, aprenderá a explicar por qué el modelo realiza las predicciones se realiza mediante el SDK de interoperabilidad de Azure Machine Learning. Es importante poder explicar el modelo por las razones siguientes:

* Los clientes y las partes interesadas desean saber **si puede confiar en las predicciones del modelo hace**.
* Como científico de datos, le interesa entender **cómo consultar el modelo para buscar información**. También necesita herramientas para tomar decisiones informadas sobre **cómo mejorar el modelo**.
* Como empresa, debe comprender **el comportamiento del modelo con distintas distribuciones de entrada** y **cómo se comportará el modelo mientras se analiza la entrada específica**.

Interoperabilidad de Machine learning es importante en dos fases del ciclo de desarrollo de aprendizaje automático: 

* Durante la **entrenamiento**: Diseñadores de modelos y evaluadores requieren herramientas de interoperabilidad para explicar el resultado de un modelo a las partes interesadas para crear relaciones de confianza. También necesitan información sobre el modelo para que se puede depurar el modelo y tomar decisiones sobre si el comportamiento coincide con sus objetivos. Por último, que necesitan para garantizar que el modelo se inclina no.

* Durante la **inferencia**: Predicciones deben ser explicación a las personas que utilizan el modelo. Por ejemplo, ¿por qué el modelo denegar un préstamo hipotecario o predecir una cartera de inversiones que lleva a un riesgo más alto?

El SDK de Azure Machine Learning interoperabilidad incorpora las tecnologías desarrolladas por Microsoft y probadas las bibliotecas de terceros (por ejemplo, formas y CAL). El SDK crea una API común a través de las bibliotecas integradas e integra servicios de Azure Machine Learning. Con este SDK, puede explicar los modelos de aprendizaje automático **globalmente en todos los datos**, o **localmente en un punto de datos específico** mediante las tecnologías de última generación de forma escalable y fácil de usar.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Interoperabilidad de Azure Machine Learning se pueden aplicar para entender el modelo global comportamiento o predicciones específicas. El primero se denomina explicación global y este último se denomina explicación local.

Métodos de interoperabilidad de Machine Learning de Azure se pueden clasificar también en función de si el método es independiente del modelo o un modelo específico. Algunos métodos de destino determinados tipos de modelos. Por ejemplo, explicación del árbol de formas solo se aplica a modelos basados en árbol. Algunos métodos de tratan el modelo como una caja negra, como una explicación imitar o explicación del kernel de formas. SDK de Azure Machine Learning interoperabilidad aprovecha estos distintos enfoques basados en conjuntos de datos, tipos de modelo y casos de uso.

Interoperabilidad de Azure Machine Learning devuelve un conjunto de información sobre cómo un modelo realiza su predicción. La información incluye elementos tales como:

* Importancia de características relativas de global o local
* Relación de característica y predicción global o local

## <a name="architecture"></a>Arquitectura

Interoperabilidad de SDK de Azure Machine Learning está estructurada en dos paquetes de Python:

* [azureml.EXPLAIN.Model](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py) -el paquete principal, que contiene las funcionalidades que son compatibles con Microsoft.
* `azureml.contrib.explain.model` -Versión preliminar y funcionalidades experimentales que puede probar.

    > [!IMPORTANT]
    > Las cosas en contrib no son totalmente compatibles. Como las funcionalidades experimentales maduran, gradualmente se moverán al paquete principal.

### <a name="explainers"></a>Explainers

El SDK de interoperabilidad de Azure Machine Learning incluye dos conjuntos de explainers: Explainers directos y Explainers Meta.

__Dirigir explainers__ proceden de las bibliotecas integradas. El SDK incluye todas las explainers para que exponen una API común y el formato de salida. Los siguientes son una lista de los explainers directos disponibles en el SDK:

> [!TIP]
> Si se siente más cómodo directamente mediante estas explainers, se pueden invocar directamente en lugar de usar la API común y el formato de salida.

* **Explicación de árbol**: Explicación de árbol de formas, que se centra en polinómica tiempo rápido formas valor estimación específico del algoritmo árboles y conjuntos de árboles de.
* **Explicación profundo**: Según la explicación de formas, explicación profundo "es un algoritmo de aproximación de alta velocidad para los valores de formas en los modelos de aprendizaje profundo que se basa en una conexión con DeepLIFT descrito en el documento de NIPS formas. Se admiten los modelos de TensorFlow y modelos de Keras con TensorFlow back-end (también hay compatibilidad preliminar con PyTorch) ".
* **Explicación del kernel**: Explicación del Kernel de formas utiliza una regresión lineal local especialmente ponderada para calcular los valores de formas para cualquier modelo.
* **Explicación de imitar**: Imitar explicación se basa en la idea de los modelos de suplente global. Un modelo de suplente global es un modelo de forma intrínseca interpretable está entrenado para aproximar las predicciones de un modelo de caja negra de forma más precisa posible. Científico de datos puede interpretar el modelo de suplentes para extraer conclusiones sobre el modelo de caja negra.
* **Explicación de LIMA**: En función de las CAL, LIMA explicación utiliza el algoritmo de última generación Local interpretable independiente del modelo explicaciones (CAL) para crear modelos de suplente local. A diferencia de los modelos de suplente global LIMA se centra en entrenar modelos de suplente local para explicar las predicciones individuales.
* **Explicación de texto HAN**: Explicación de texto HAN usa una red de atención jerárquica para obtener explicaciones de modelo de datos de texto para un modelo de texto determinado cuadro negro. Se entrena el modelo de suplentes de patrón en salidas de predicción de un modelo determinado de profesor. Después del entrenamiento global en el corpus de texto, hemos agregado un paso de ajuste preciso para un documento específico con el fin de mejorar la precisión de las explicaciones. HAN usa un RNN bidireccional con dos capas de atención, palabras y oraciones requiere atención. Una vez que la DNN se entrenó el modelo de profesor para ajustarlos en un documento concreto, podemos extraer el importances word de los niveles de atención. Hemos encontrado HAN de ser más precisos que LIMA o formas con datos de texto, pero más costosos en términos de tiempo también de entrenamiento. Sin embargo, hemos realizado mejoras en el tiempo de entrenamiento por proporcionar al usuario la opción para inicializar la red con guante incrustaciones de palabras, aunque sigue siendo lenta. El tiempo de entrenamiento se puede mejorar considerablemente mediante la ejecución HAN en una máquina virtual GPU de Azure remota. La implementación del patrón se describe en "Redes de atención jerárquica para clasificación de documentos (Yang et al, 2016)" ([https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf)).

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

Los modelos que se entrenan con conjuntos de datos en Python `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData`, o `scipy.sparse.csr_matrix` formato son compatibles con el SDK de interoperabilidad de Machine Learning.

Las funciones de explicación aceptan modelos y las canalizaciones como entrada. Si se proporciona un modelo, el modelo debe implementar la función de predicción `predict` o `predict_proba` que se ajusta a la convención de Scikit. Si se proporciona una canalización (nombre de la secuencia de comandos de la canalización), la función de la explicación se da por supuesto que el script de canalización de ejecución devuelve una predicción.

### <a name="local-and-remote-compute-target"></a>Destino de proceso local y remota

El SDK de interoperabilidad de Machine Learning está diseñado para trabajar con ambos destinos de proceso local y remota. Si se ejecutan localmente, las funciones el SDK no se comunicará con los servicios de Azure. Puede ejecutar una explicación de forma remota en Azure Machine Learning Compute y registrar la información de la explicación en servicios de historial de ejecución de Azure Machine Learning. Una vez que esta información se registra, informes y visualizaciones de la explicación están disponibles en el Portal del área de trabajo de Azure Machine Learning para el análisis de usuario.

## <a name="train-and-explain-locally"></a>Entrenar y explique localmente

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

2. Llame a la explicación: Para iniciar un objeto de la explicación, deberá pasar el modelo, los datos de entrenamiento, las características de interés (opcional) y los nombres de clase de salida (si clasificación) para la explicación. Aquí le mostramos cómo crear una instancia de un objeto de explicación mediante [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py), [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py), y `LimeExplainer` localmente. `TabularExplainer` se llama a uno de los tres explainers debajo (`TreeExplainer`, `DeepExplainer`, o `KernelExplainer`) y se selecciona automáticamente uno más adecuada para su caso de uso. Sin embargo, puede llamar directamente a cada uno de sus tres explainers subyacentes.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    ```
    o
    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
    explainer = MimicExplainer(model, x_train, LGBMExplainableModel, features=breast_cancer_data.feature_names, classes=classes)
    ```

3. Obtener valores de la importancia de la función global.

    ```python
    # You can use the training data or the test data here. 
    global_explanation = explainer.explain_global(x_train)
    # Sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))
    ```

4. Los valores de importancia de características locales: use la siguiente función que llama a explicar una instancia individual o un grupo de instancias.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0,:])
    
    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```
    o
    ```python
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:4,:])
    
    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

## <a name="train-and-explain-remotely"></a>Entrenar y explicar de forma remota

Aunque puede entrenar en los distintos destinos de proceso compatibles con el servicio Azure Machine Learning, en el ejemplo de esta sección se muestra cómo hacer esto utilizando AMLCompute.

1. Crear un script de entrenamiento en un cuaderno de Jupyter local (por ejemplo, run_explainer.py).

    ``` python  
    run = Run.get_context()
    client = ExplanationClient.from_run(run)
    
    # Train your model here

    # explain predictions on your local machine    
    explainer = TabularExplainer(model, x_train, features=breast_cancer_data.feature_names, classes=classes)
    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    # explain local data points (individual instances)
    local_explanation = explainer.explain_local(x_test[0,:])
    # upload global and local explanation objects to Run History
    upload_model_explanation(run, local_explanation, top_k=2, comment='local explanation: top 2 features')
    # Uploading global model explanation data for storage or visualization in webUX
    # The explanation can then be downloaded on any compute
    # Multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # Or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Siga las instrucciones de [configurar destinos de proceso de entrenamiento del modelo](how-to-set-up-training-targets.md#amlcompute) para obtener información sobre cómo configurar un proceso de Azure Machine Learning como destino de proceso y envía la ejecución de entrenamiento.

3. Descargue la explicación en el cuaderno de Jupyter local. 
    > [!IMPORTANT]
    > Las cosas en contrib no son totalmente compatibles. Como las funcionalidades experimentales maduran, gradualmente se moverán al paquete principal.

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

## <a name="next-steps"></a>Pasos siguientes

Para ver una colección de instancias de Jupyter Notebook que muestran las instrucciones anteriores, consulte el [cuadernos de ejemplo de interoperabilidad de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
