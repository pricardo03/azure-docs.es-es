---
title: Interpretación del modelo de Azure Machine Learning Service
titleSuffix: Azure Machine Learning
description: Aprenda a explicar los motivos por los que un modelo realiza predicciones mediante el SDK de Azure Machine Learning. Se puede usar durante el entrenamiento y la inferencia para saber cómo realiza las predicciones el modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: 5f1008e8fcbbf7b82a694fd151a9dea9ca7f001e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511057"
---
# <a name="model-interpretability-in-azure-machine-learning-service"></a>Interpretación del modelo de Azure Machine Learning Service
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

## <a name="overview-of-model-interpretability"></a>Explicación de la interpretación del modelo

La interpretación es fundamental para los científicos de datos y los responsables de la toma de decisiones empresariales para garantizar el cumplimiento de las directivas de la empresa, los estándares del sector y las regulaciones gubernamentales:
+ Los científicos de datos necesitan la capacidad de explicar sus modelos a ejecutivos y partes interesadas para que puedan reconocer el valor y la precisión de sus hallazgos 
+ Los responsables de la toma de decisiones empresariales necesitan armonizar la capacidad de proporcionar transparencia a los usuarios finales para obtener y mantener su confianza

La habilitación de la capacidad de explicar un modelo de aprendizaje automático es importante durante dos fases principales del desarrollo del modelo:
+ Durante la fase de entrenamiento del ciclo de desarrollo del modelo de aprendizaje automático. Los diseñadores y evaluadores de modelos pueden usar los resultados de la interpretación de un modelo para verificar las hipótesis y crear confianza en las partes interesadas. También usan la conclusiones sobre el modelo para la depuración, para validar que el comportamiento del modelo coincide con sus objetivos y para comprobar si hay características insignificantes o de sesgo.
+ Durante la fase de inferencia, como la transparencia en torno a los modelos implementados, los ejecutivos pueden reconocer "Cuando se implementan" cómo funciona el modelo y cómo sus decisiones tratan e impactan a las personas en la vida real. 

## <a name="interpretability-with-azure-machine-learning"></a>Interpretación con Azure Machine Learning

En este artículo, obtendrá información sobre cómo se implementan los conceptos de interpretación del modelo en el SDK.

Mediante las clases y métodos del SDK, puede obtener:
+ Los valores de la importancia de las tanto para las características sin procesar como en las que se ha realizado ingeniería
+ Interpretabilidad en los conjuntos de datos del mundo real a escala, durante el entrenamiento y la inferencia.
+ Visualizaciones interactivas que le ayudan a detectar de patrones en los datos y explicaciones durante el entrenamiento


En el aprendizaje automático, las **características** son los campos de datos que se usan para predecir un punto de datos de destino. Por ejemplo, para predecir el riesgo de crédito, se pueden usar los campos de datos de la edad, el tamaño de la cuenta y la antigüedad de la cuenta. En este caso, la edad, el tamaño de la cuenta y la antigüedad de la cuenta son **características**. La importancia de las características le indica la forma en que cada campo de datos ha afectado las predicciones del modelo. Por ejemplo, es posible que la edad se use mucho en la predicción, mientras que la antigüedad y el tamaño de la cuenta no afectan a la precisión de la predicción significativamente. Este proceso permite a los científicos de datos explicar las predicciones resultantes, con el fin de que partes interesadas puedan ver qué puntos de datos son los más importantes del modelo.

Mediante estas herramientas se pueden explicar los modelos de Machine Learning **de forma global en todos los datos** o **de forma local en un punto de datos específico** mediante el uso de tecnologías vanguardistas de un modo escalable y fácil de usar.

Las clases de interpretabilidad se ponen a disposición a través de varios paquetes de SDK. Aprenda a [instalar paquetes de SDK para Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* `azureml.interpret`, el paquete principal, que contiene funcionalidades compatibles con Microsoft.

* `azureml.contrib.interpret`, versión preliminar, y las funcionalidades experimentales que puede probar.

* El paquete `azureml.train.automl.automlexplainer` para interpretar los modelos de Machine Learning automatizados.

> [!IMPORTANT]
> El contenido del espacio de nombres `contrib` no es totalmente compatible. A medida que maduran las funcionalidades experimentales, se mueven gradualmente al espacio de nombres principal.

## <a name="how-to-interpret-your-model"></a>Interpretación del modelo

Puede aplicar las clases y métodos de interpretabilidad para conocer el comportamiento global o las predicciones específicas del modelo. Al primero se le denomina explicación global y al último explicación local.

Los métodos también se pueden clasificar también en función de si el método es independiente del modelo o específico de un modelo. Algunos métodos van dirigidos a un tipo de modelos concreto. Por ejemplo, explicación del árbol de SHAP solo se aplica a modelos basados en árbol. Algunos métodos de tratan el modelo como una caja negra, como una explicación de imitación o explicación del kernel de SHAP. El paquete `interpret` aprovecha estos diferentes enfoques basados en conjuntos de datos, tipos de modelo y casos de uso.

El resultado es un conjunto de información acerca de la forma en que un modelo dado realiza su predicción, como:
* Importancia de características relativas globales o locales
* Relación entre las características y las predicciones globales o locales

### <a name="explainers"></a>Explicaciones

Este paquete usa las técnicas de interpretación desarrolladas en [Interprete-Comunidad](https://github.com/interpretml/interpret-community/), un paquete python de código abierto para entrenar modelos que se puedan interpretar y para ayudar a explicar los sistemas IA de caja negra. [Interpretar-Comunidad ](https://github.com/interpretml/interpret-community/)actúa como el host de los Explicadores admitidos por este SDK y actualmente admite las siguientes técnicas de interpretación:

* **SHAP Tree Explainer**: [El explicador de árboles de SHAP](https://github.com/slundberg/shap), que se centra en el algoritmo de cálculo rápido de valores SHAP en tiempo polinómico específico para árboles y conjuntos de árboles.
* **SHAP Deep Explainer**: Según la explicación de [SHAP](https://github.com/slundberg/shap), Deep Explainer "es un algoritmo de aproximación de alta velocidad para valores SHAP en modelos de aprendizaje profundo que se basa en una conexión con DeepLIFT descrita en el documento [SHAP NIPS](https://papers.nips.cc/paper/7062-a-unified-approach-to-interpreting-model-predictions). Se admiten tanto los modelos de TensorFlow como modelos de Keras que usan el back-end TensorFlow (también hay compatibilidad preliminar con PyTorch)".
* **Explicador lineal SHAP**: [El Explicador lineal de SHAP](https://github.com/slundberg/shap) calcula los valores SHAP para un modelo lineal, teniendo en cuenta opcionalmente las correlaciones entre funciones.

* **SHAP Kernel Explainer**: [El explicador del Kernel de SHAP](https://github.com/slundberg/shap) usa una regresión lineal local especialmente ponderada para calcular los valores SHAP de cualquier modelo.
* **Mimic Explainer**: El explicador Mimic se basa en la idea de entrenar [modelos globales de sustitución](https://christophm.github.io/interpretable-ml-book/global.html) para imitar los modelos de caja negra. Un modelo suplente global es un modelo interpretable de forma intrínseca que está entrenado para aproximarse a las predicciones de un modelo de caja negra de la forma más precisa posible. Los científicos de datos pueden interpretar el modelo suplente para extraer conclusiones acerca del modelo de caja negra. Cualquiera de los siguientes modelos se puede usar como modelo suplente: LightGBM (LGBMExplicableModel), Regresión lineal (LinearExplicableModel), Modelo explicable de descenso de gradiente estocástico (SGDExplicableModel) y Árbol de decisión (DecisionTreeExplicableModel).


* **Permutation Feature Importance Explainer**: La importancia de características de permutación es una técnica que se usa para explicar los modelos de clasificación y regresión que está inspirada en el [documento acerca de bosques aleatorios de Breiman](https://www.stat.berkeley.edu/%7Ebreiman/randomforest2001.pdf) (consulte la sección 10). En un alto nivel, la forma en que funciona es cuando se barajan aleatoriamente los datos de una característica para todo el conjunto de datos y se calcula en qué medida cambia la métrica de rendimiento que interesa. Cuanto mayor sea el cambio, más importante es la característica.

* **LIME Explainer** (`contrib`): Según [LIME](https://github.com/marcotcr/lime), LIME Explainer usa el algoritmo de explicaciones agnósticas de modelos locales que se pueden interpretar (LIME) de última generación para crear modelos de sustitución locales. A diferencia de los modelos suplentes globales, LIME se centra en entrenar modelos suplentes locales para explicar predicciones individuales.
* **HAN Text Explainer** (`contrib`): Explicación de texto HAN usa una red de atención jerárquica para obtener explicaciones de modelos a partir de datos de texto para un modelo de texto de caja negra dado. Entrena el modelo sustituto HAN en las salidas previstas de un modelo de caja negra dado. Después del entrenamiento global en el corpus del texto, agrega un paso de ajuste preciso para un documento específico con el fin de mejorar la precisión de las explicaciones. HAN usa una red neuronal recurrente bidireccional con dos capas de atención, para prestar atención a las frases y a las palabras. Una vez que la red neuronal profunda se entrena en el modelo de caja negra y se ajusta de forma precisa en un documento específico, el usuario puede extraer las importancias de las palabras de las capas de atención. Se ha demostrado que el HAN es más preciso que el LIME o el SHAP para los datos de texto, pero también más costoso en términos de tiempo de entrenamiento. Se han realizado mejoras para proporcionar al usuario la opción de inicializar la red con inserciones de palabras de la guantera para reducir el tiempo de entrenamiento. El tiempo de entrenamiento se puede reducir considerablemente mediante la ejecución HAN en una máquina virtual con GPU de Azure remota. La implementación de HAN se describe en [Hierarchical Attention Networks for Document Classification (Yang et al., 2016)](https://www.researchgate.net/publication/305334401_Hierarchical_Attention_Networks_for_Document_Classification) (HAN para clasificación de documentos).


* **Explicador tabular**: `TabularExplainer` emplea la siguiente lógica para invocar los explicadores directos de [SHAP](https://github.com/slundberg/shap):

    1. Si es un modelo basado en árbol, aplique SHAP `TreeExplainer`, de lo contrario
    2. Si es un modelo de red neuronal profunda, aplique SHAP `DeepExplainer`, de lo contrario
    3. Si es un modelo lineal, aplique SHAP `LinearExplainer`, de lo contrario
    3. Trátelo como un modelo de caja negra y aplique SHAP `KernelExplainer`


`TabularExplainer` también realizó mejoras significativas en las características y en el rendimiento con respecto a los Explicadores directos de la SHAP:

* **Resumen del conjunto de datos de inicialización**. En los casos en que la velocidad de la explicación es lo más importante, resumimos el conjunto de datos de inicialización y generamos un pequeño conjunto de ejemplos representativos, lo que acelera tanto la explicación global como la local.
* **Muestreo del conjunto de datos de evaluación**. Si el usuario pasa un gran conjunto de ejemplos de evaluación, pero no necesita que se evalúen todos, el parámetro de muestreo puede establecerse en true para acelerar la explicación global.

El siguiente diagrama muestra la estructura actual de las explicaciones directas y de las metaexplicaciones.

[![Arquitectura de interpretabilidad de Machine Learning](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)


### <a name="models-supported"></a>Modelos que se admiten

Todos los modelos que se entrenan con conjuntos de datos en el formato de `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData` o `scipy.sparse.csr_matrix` de Python son compatibles con el paquete `explain` de interpretabilidad del SDK.

Las funciones de explicación aceptan como entrada tanto modelos como canalizaciones. Si se proporciona un modelo, este debe implementar la función de predicción `predict` o `predict_proba` que se ajuste a la convención de Scikit. Si se proporciona una canalización (nombre del script de la canalización), la función de la explicación da por supuesto que el script de canalización de ejecución devuelve una predicción. Se admiten modelos entrenados mediante los marcos de aprendizaje profundo Keras, TensorFlow y PyTorch.

### <a name="local-and-remote-compute-target"></a>Destino de proceso local y remoto

El paquete `explain` está diseñado para trabajar con destinos de proceso locales y remotos. Si se ejecutan localmente, las funciones el SDK no entrarán en contacto con ningún servicio de Azure. Puede ejecutar una explicación de forma remota en Proceso de Azure Machine Learning y registrar la información de la explicación en servicios de historial de ejecución de Azure Machine Learning. Una vez que esta información se registra, tanto los informes como las visualizaciones de la explicación están disponibles en el portal del área de trabajo de Azure Machine Learning para que los pueda analizar el usuario.


## <a name="next-steps"></a>Pasos siguientes

Vea las [instrucciones](how-to-machine-learning-interpretability-aml.md) para habilitar la interpretación de entrenamiento de modelos tanto localmente como en los recursos de computación remota de Azure Machine Learning. Consulte los [cuadernos de muestras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model/tabular-data) para ver otros escenarios.
