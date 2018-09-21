---
title: Clasificación de las opiniones de Twitter con el paquete Azure Machine Learning (AML) para el análisis de texto (AMLPTA) y con el Proceso de ciencia de datos en equipo (TDSP) | Microsoft Docs
description: Describe el uso de TDSP (Proceso de ciencia de datos en equipo) y de AMLPTA para la clasificación de opiniones
services: machine-learning, team-data-science-process
documentationcenter: ''
author: deguhath
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: deguhath
ms.openlocfilehash: 9e5018bc4c7b90897f7f8c91169410284217b172
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45577020"
---
# <a name="twitter-sentiment-classification-with-azure-machine-learning-aml-package-for-text-analytics-amlpta-and-team-data-science-process-tdsp"></a>Clasificación de las opiniones de Twitter con el paquete Azure Machine Learning (AML) para el análisis de texto (AMLPTA) y con el Proceso de ciencia de datos en equipo (TDSP)

## <a name="introduction"></a>Introducción
La normalización de la estructura y la documentación de los proyectos de ciencia de datos, basada en un [ciclo de vida de los datos](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md) establecido, es fundamental para facilitar una colaboración eficaz entre los equipos de ciencia de datos.

Previamente se presentó un [repositorio de GitHub para las plantillas y la estructura del proyecto de TDSP](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Ahora ya se ha habilitado la creación de proyectos de Azure Machine Learning instanciados con las [plantillas de estructura y documentación de TDSP para Azure Machine Learning](https://github.com/amlsamples/tdsp). [Aquí](https://docs.microsoft.com/azure/machine-learning/preview/how-to-use-tdsp-in-azure-ml) se proporcionan instrucciones sobre cómo usar las plantillas y la estructura de TDSP en Azure Machine Learning. 

En este ejemplo, vamos a mostrar el uso del paquete de Azure Machine Learning para el análisis de texto y del TDSP para el desarrollo y la implementación de modelos predictivos para la clasificación de opiniones de Twitter.


## <a name="use-case"></a>Caso de uso
### <a name="twitter-sentiment-polarity-sample"></a>Ejemplo de polaridad de opiniones de Twitter
En este artículo se utiliza un ejemplo para mostrar cómo crear una instancia y ejecutar un proyecto de Machine Learning. El ejemplo usa las plantillas y la estructura de TDSP en Azure Machine Learning Workbench. El ejemplo completo se proporciona en este tutorial. La tarea de modelado predice la polaridad de opinión (positiva o negativa) utilizando el texto de los tweets. Este artículo resume las tareas de modelado de datos que se describen en el tutorial. En el tutorial se describen las tareas siguientes:

1. Exploración de datos, aprendizaje e implementación de un modelo de aprendizaje automático que permita resolver el problema de la predicción que se describe en la introducción del caso de uso. Los datos de opinión de Twitter se utilizan para estas tareas.
2. Ejecución del proyecto mediante la plantilla TDSP de Azure Machine Learning para este proyecto. Para la ejecución de proyectos y los informes se usa el ciclo de vida de TDSP.
3. Operacionalización de la solución directamente desde Azure Machine Learning en Azure Container Service.

El proyecto resalta el uso del paquete de análisis de texto para Azure Machine Learning.


## <a name="link-to-github-repository"></a>Vínculo al repositorio de GitHub
El vínculo al repositorio de GitHub está [aquí](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction). 

### <a name="purpose"></a>Propósito
El propósito principal de este ejemplo es mostrar cómo crear una instancia y ejecutar un proyecto de aprendizaje automático mediante la estructura y las plantillas del Proceso de ciencia de datos en equipo (TDSP) en Azure Machine Learning. Para ello, usamos [los datos de opinión de Twitter](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip). La tarea de modelado es predecir la polaridad (positiva o negativa) de la opinión mediante el texto de los tweets.

### <a name="scope"></a>Ámbito
- Exploración de datos, entrenamiento e implementación de un modelo de aprendizaje automático que permita resolver el problema de predicción que se describe en la introducción del caso de uso.
- Ejecución del proyecto en Azure Machine Learning mediante la plantilla de proceso de ciencia de datos de equipo (TDSP) para este proyecto. Para la ejecución de proyectos y los informes se va a usar el ciclo de vida de TDSP.
- Operacionalización de la solución directamente desde Azure Machine Learning en Azure Container Services.

El proyecto destaca varias características de Azure Machine Learning, tales como la creación de instancias y el uso de estructura de TDSP, la ejecución de código en Azure Machine Learning Work Bench, y la fácil operacionalización en Azure Container Services mediante Docker y Kubernetes.

## <a name="team-data-science-process-tds"></a>Proceso de ciencia de datos en equipo (TDSP)
Para ejecutar este ejemplo usamos la estructura y las plantillas de documentación del proyecto de TDSP. Sigue el [ciclo de vida del TDSP](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/lifecycle). El proyecto se crea según las instrucciones proporcionadas [aquí](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).


<img src="./media/predict-twitter-sentiment-amltextpackage/tdsp-lifecycle2.png" alt="tdsp-lifecycle" width="800" height="600">


## <a name="use-case-overview"></a>Información general del caso de uso
La tarea es predecir la polaridad binaria de cada opinión de Twitter mediante las características de las incrustaciones de palabras extraídas del texto de twitter. Para obtener una descripción detallada, consulte este [repositorio](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction).

### <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Adquisición y comprensión de los datos](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
El primer paso de este ejemplo es descargar el conjunto de datos sentiment140 y dividirlo en los conjuntos de datos de prueba y de aprendizaje. El conjunto de datos Sentiment140 contiene el contenido real del tweet (sin los emoticonos) junto con la polaridad de cada tweet (negativo = 0, positivo = 4) y con los tweets neutros eliminados. Los datos de aprendizaje resultantes tienen 1,3 millones de filas, mientras que los datos de prueba tienen 320 000 filas.

### <a name="modelinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode02modeling"></a>[Modelado](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/02_modeling)
Esta parte del ejemplo se subdivide en otras tres partes: 
- La **ingeniería de características** corresponde a la generación de características mediante distintos algoritmos de inserción de palabras, a saber, Word2Vec. 
- La **creación del modelo** tiene que ver con el aprendizaje de modelos diferentes, como la _regresión logística_ y la _potenciación del gradiente_ para predecir la opinión del texto de entrada. 
- La **evaluación del modelo** aplica el modelo entrenado a los datos de prueba.

#### <a name="feature-engineering"></a>Ingeniería de características
Usamos <b>Word2Vec</b> para generar inserciones de palabras. En primer lugar usamos el algoritmo Word2Vec en el modo de Skip gram, como se describe en el documento de [Mikolov, Tomas, et al. Distributed representations of words and phrases and their compositionality. Advances in neural information processing systems (Representaciones distribuidas de palabras y frases y su composicionalidad. Avances en los sistemas de procesamiento de información neuronal). 2013.](https://arxiv.org/abs/1310.4546) para generar inserciones de palabras.

Skip-gram es una red neuronal superficial que toma la palabra de destino codificada como un vector activo como entrada y la usa para predecir las palabras cercanas. Si V es el tamaño del vocabulario, el tamaño de la capa de salida sería __C*V__, donde C es el tamaño de la ventana de contexto. La arquitectura basada en Skip-gram se muestra en la ilustración siguiente.
 
<table class="image" align="center">
<caption align="bottom">Modelo de Skip-gram</caption>
<tr><td><img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/06/05000515/Capture2-276x300.png" alt="Skip-gram model"/></td></tr>
</table>

Los detalles del algoritmo de Word2Vec y del modelo de Skip-gram quedan fuera del ámbito de este ejemplo y se solicita a los lectores interesados que recorran los siguientes vínculos para obtener más detalles. El código 02_A_Word2Vec.py hace referencia a [ejemplos de tensorflow](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)

* [Vector Representation of Words](https://www.tensorflow.org/tutorials/word2vec) (Representación vectorial de palabras)
* [How exactly does word2vec work?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf) (¿Cómo funciona exactamente word2vec?)
* [Notes on Noise Contrastive Estimation and Negative Sampling](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf) (Notas sobre la estimación comparativa de ruido y el muestreo negativo)

Una vez acabado el proceso de aprendizaje, se generan dos archivos de inserción en formato TSV para la fase de modelado.

#### <a name="model-training"></a>Entrenamiento del modelo
Una vez que se han generado los vectores de palabra mediante los algoritmos SSWE o Word2vec, el siguiente paso es entrenar los modelos de clasificación predecir la polaridad real de las opiniones. Se aplican los dos tipos de características, Word2Vec y SSWE, a los dos modelos: regresión logística y redes neuronales convolucionales CNN). 

#### <a name="model-evaluation"></a>Evaluación del modelo
Se proporciona código para cargar y evaluar varios modelos entrenados en el conjunto de datos de prueba.


### <a name="deploymenthttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode03deployment"></a>[Implementación](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/03_deployment)
Esta parte se proporcionan punteros a instrucciones sobre cómo operacionalizar un modelo de predicción de opiniones previamente entrenado en un servicio web de un clúster de Azure Kubernetes Service (AKS). El entorno de operacionalización aprovisiona Docker y Kubernetes en el clúster para administrar la implementación del servicio web. [Aquí](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy) encontrará más información sobre el proceso de operacionalización.

## <a name="conclusion"></a>Conclusión
Hemos explicado los detalles de cómo entrenar un modelo de inserción de palabras mediante Word2Vec y, después, usar las inserciones extraídas como características para entrenar dos modelos diferentes para predecir la puntuación de las opiniones de los datos de texto de Twitter. Uno de estos modelos se implementa en Azure Kubernetes Service (AKS). 

## <a name="next-steps"></a>Pasos siguientes
Para empezar, lea la documentación adicional acerca de [Azure Machine Learning Package for Text Analytics (AMLPTA)](https://docs.microsoft.com/python/api/overview/azure-machine-learning/textanalytics?view=azure-ml-py-latest) y el [Proceso de ciencia de datos en equipo (TDSP)](https://aka.ms/tdsp).

## <a name="references"></a>Referencias
* [Proceso de ciencia de datos en equipo (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) 
* [Uso del proceso de ciencia de datos en equipo (TDSP) en Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [Plantilla de proyecto TDSP para Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Azure ML Work Bench](https://docs.microsoft.com/azure/machine-learning/preview/)
* [Mikolov, Tomas y otros Distributed representations of words and phrases and their compositionality. Advances in neural information processing systems (Representaciones distribuidas de palabras y frases y su composicionalidad. Avances en los sistemas de procesamiento de información neuronal). 2013.](https://arxiv.org/abs/1310.4546)
