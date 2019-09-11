---
title: Aprendizaje profundo frente a aprendizaje automático
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre aprendizaje profundo frente al aprendizaje automático y sepa cómo se relacionan ambos conceptos con la inteligencia artificial. El aprendizaje profundo se puede aplicar en distintos escenarios, como la detección de fraudes, el reconocimiento facial y de voz, el análisis de opiniones y la previsión de series temporales.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: lazzeri
author: FrancescaLazzeri
ms.date: 08/07/2019
ms.openlocfilehash: 3b0546c42ddd5252b35d1a77d7b152733beec233
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70240956"
---
# <a name="deep-learning-vs-machine-learning"></a>Aprendizaje profundo frente a aprendizaje automático

En este artículo podrá comparar el aprendizaje profundo y el aprendizaje automático. En él se compararán los dos conceptos y aprenderá cómo encajan en la categoría más amplia de inteligencia artificial. En este artículo también se describe la forma en que el aprendizaje profundo se puede aplicar a distintos escenarios del mundo real, como la detección de fraudes, el reconocimiento facial y de voz, el análisis de opiniones y la previsión de series temporales.

## <a name="deep-learning-machine-learning-and-ai"></a>Aprendizaje profundo, Machine Learning e inteligencia artificial

![Diagrama de relaciones: Inteligencia artificial frente a aprendizaje automático y a aprendizaje profundo](./media/concept-deep-learning-vs-machine-learning/ai-vs-machine-learning-vs-deep-learning.png)

Tenga en cuenta las siguientes definiciones para comprender el aprendizaje profundo y el aprendizaje automático. Inteligencia artificial:

- El **aprendizaje profundo** es un subconjunto del aprendizaje automático basado en redes neuronales artificiales. El _proceso de aprendizaje_ se llama _profundo_ porque la estructura de redes neuronales artificiales se compone de varias capas de entrada, salida y ocultas. Cada capa contiene unidades que transforman los datos de entrada en información que la capa siguiente puede usar para realizar una tarea de predicción determinada. Gracias a esta estructura, una máquina puede aprender a través de su propio procesamiento de datos.

- El **aprendizaje automático** es un subconjunto de la inteligencia artificial que incluye técnicas (como el aprendizaje profundo) que permiten a las máquinas mejorar en las tareas con la experiencia. El _proceso de aprendizaje_ se basa en los pasos siguientes:

   1. Agregue datos a un algoritmo. (En este paso puede proporcionar información adicional al modelo como, por ejemplo, realizando la extracción de características).
   1. Utilice estos datos para entrenar un modelo.
   1. Pruebe e implemente el modelo.
   1. Use el modelo implementado para realizar una tarea de predicción automatizada. (En otras palabras, llame y use el modelo implementado para recibir las predicciones que devuelva el modelo).

- La  **inteligencia artificial (IA)** es una técnica que permite a los equipos imitar la inteligencia humana. Incluye el aprendizaje automático. 
 
Es importante conocer la relación entre la inteligencia artificial, el aprendizaje automático y el aprendizaje profundo. El aprendizaje automático es una forma de lograr inteligencia artificial. Al usar el aprendizaje automático y las técnicas de aprendizaje profundo, puede compilar sistemas y aplicaciones de equipos que realicen tareas que normalmente se asocian a la inteligencia humana. Estas tareas incluyen la percepción visual, el reconocimiento de voz, la toma de decisiones y la traducción de idiomas.

## <a name="techniques-of-deep-learning-vs-machine-learning"></a>Técnicas del aprendizaje profundo y del aprendizaje automático 

Ahora que ya tiene información general acerca del aprendizaje automático y el aprendizaje profundo, vamos a comparar ambas técnicas. En el aprendizaje automático, se debe indicar al algoritmo cómo realizar una predicción precisa; para ello debe conseguir más información (por ejemplo, realizando la extracción de características). En el aprendizaje profundo, en cambio, el algoritmo puede obtener información sobre cómo hacer una predicción precisa a través de su propio procesamiento de datos, gracias a la estructura de red neuronal artificial.

En la tabla siguiente se comparan las dos técnicas con más detalle:

| |Todo el aprendizaje automático |Solo aprendizaje profundo|
|---|---|---|
|  **Número de puntos de datos** | Puede usar pequeñas cantidades de datos que proporcionan los usuarios. | Requiere una gran cantidad de datos de entrenamiento para sacar conclusiones concisas. |
|  **Dependencias del hardware** | Puede trabajar en equipos lentos. No necesita una gran cantidad de potencia de cálculo. | Depende de máquinas rápidas. Realiza intrínsecamente un gran número de operaciones de multiplicación de matrices. Una GPU puede optimizar eficazmente estas operaciones. |
|  **Proceso de características** | Requiere que los usuarios creen e identifiquen con precisión las características. | Aprende las características de alto nivel de los datos y crea nuevas características automáticamente. |
|  **Enfoque del aprendizaje** | Divide las tareas en pequeñas partes y, después, combina los resultados recibidos para sacar una conclusión. | Resuelve el problema de forma global. |
|  **Tiempo de ejecución** | Comparativamente, tarda menos tiempo en entrenarse; puede tardar unos segundos o unas pocas horas. | Tarda demasiado tiempo en entrenarse, porque los algoritmos de aprendizaje profundo tienen muchos parámetros. |
|  **Salida** | La salida suele ser un valor numérico, como una puntuación o una clasificación. | La salida puede ser una puntuación, un texto, un elemento o un sonido. |

## <a name="deep-learning-use-cases"></a>Casos de uso del aprendizaje profundo

A causa de la estructura de la red neuronal artificial, el aprendizaje profundo es excelente para identificar patrones en datos no estructurados, como imágenes, sonido, vídeo y texto. Por esta razón, el aprendizaje profundo está transformando rápidamente muchos sectores, como la atención sanitaria, la energía, las finanzas y el transporte. Gracias a ello, estos sectores se están replanteando los procesos empresariales tradicionales. 

En los párrafos siguientes se describen algunas de las aplicaciones más comunes del aprendizaje profundo.

### <a name="named-entity-recognition"></a>Reconocimiento de entidades con nombre

Un uso de las redes de aprendizaje profundo es el reconocimiento de entidades con nombre, que es una manera de extraer determinados tipos de información de datos no estructurados y sin etiquetar. Esta información varía entre personas, lugares, empresas o cosas. Asimismo, esa información se puede almacenar en un esquema estructurado para compilar una lista de direcciones, o puede servir como banco de pruebas de un motor de validación de identidades.

### <a name="object-detection"></a>Detección de objetos

El aprendizaje profundo se ha aplicado en muchos casos de uso de detección de objetos. La detección de objetos tiene dos partes: la clasificación de imágenes y la localización de imágenes. La _clasificación_ de imágenes identifica los objetos de la imagen, como automóviles o personas. La _localización_ de imágenes, por su parte, proporciona la ubicación específica de estos objetos. 

La detección de objetos ya se está usando en sectores como los videojuegos, los comercios minoristas, el turismo y los vehículos autónomos.

### <a name="image-caption-generation"></a>Generación de subtítulos para imágenes

De forma similar a la tarea de reconocimiento de imágenes, la generación de subtítulos para imágenes es la tarea en que, para una imagen determinada, el sistema debe generar un subtítulo que describa el contenido de la imagen. Cuando puede detectar y etiquetar objetos en fotografías, el siguiente paso es convertir esas etiquetas en oraciones descriptivas y coherentes. Por lo general, los sistemas de subtítulos para imágenes implican el uso de redes neuronales convolucionales muy grandes para la detección de objetos en las fotografías; asimismo, también necesitan una red neuronal recurrente (RNN) para convertir las etiquetas en oraciones coherentes.

### <a name="machine-translation"></a>Traducción automática

La traducción automática toma palabras, frases u oraciones de un idioma y las traduce automáticamente a otro. La traducción automática lleva mucho tiempo utilizándose, pero el aprendizaje profundo está obteniendo magníficos resultados en dos áreas específicas: la traducción automática de texto (y de voz a texto) y la traducción automática de imágenes. 

Con la transformación apropiada de los datos, una red profunda es capaz de comprender texto, audio y señales visuales. La traducción automática se puede usar para identificar fragmentos de sonido en archivos de audio mayores y transcribir la palabra hablada o la imagen como texto.

### <a name="text-analytics"></a>Text Analytics

Una tarea importante del aprendizaje profundo es la detección electrónica. Las empresas usan análisis de texto basándose en el aprendizaje profundo para detectar las actividades comerciales y el cumplimiento normativo de las regulaciones gubernamentales. Los fondos de cobertura usan el análisis de texto para explorar en profundidad los repositorios de documentos más grandes con el fin de obtener detalles precisos acerca del rendimiento de las inversiones en el futuro y poder analizar la opinión del mercado. El caso de uso del análisis de texto basado en el aprendizaje profundo gira en torno a su capacidad de analizar grandes cantidades de datos de texto y realizar análisis o suspender agregaciones.

## <a name="artificial-neural-networks"></a>Redes neuronales artificiales

Las redes neuronal artificiales se forman con capas de nodos conectados. Los modelos de aprendizaje profundo usan redes neuronales que tienen un gran número de capas. 

En las secciones que tiene a continuación, se describen las tipologías de redes neuronales artificiales más populares.

### <a name="feedforward-neural-network"></a>Red neuronal del tipo feedforward

La red neuronal de tipo feedforward es el tipo más básico de red neuronal artificial. En una red de tipo feedforward, la información se desplaza solo en una dirección desde la capa de entrada a la de salida. Las redes neuronales de tipo feedforward transforman una entrada pasándola por una serie de capas ocultas. Cada capa consta de un conjunto de neuronas, donde cada capa está totalmente conectada a todas los neuronas de la capa anterior. Por último, hay una última capa totalmente conectada (la capa de salida) que representa las predicciones generadas.

### <a name="recurrent-neural-network"></a>Redes neuronales recurrentes

Las redes neuronales recurrentes son una red neuronal artificial que se usa ampliamente. Estas redes guardan la salida de una capa y la reenvían a la capa de entrada para poder predecir el resultado de esa capa. Las redes neuronales recurrentes tienen grandes capacidades de aprendizaje. Se usan ampliamente en tareas complejas, como el aprendizaje de escritura a mano y el reconocimiento de idiomas.

### <a name="convolutional-neural-networks"></a>Redes neuronal convolucionales

Una red neuronal convolucional es un tipo especialmente eficaz de red neuronal artificial que presenta una arquitectura única. Las capas se organizan en tres dimensiones: ancho, alto y profundo. Además, las neuronas de una capa no se conectan con todas las neuronas de la capa siguiente, sino que solo se conectan a una pequeña región de la misma. Asimismo, la salida final se reducirá a un solo vector de puntuaciones de probabilidad, organizadas a lo largo de la dimensión de profundidad. 

Estas redes neuronales se han utilizado en áreas como el reconocimiento y la clasificación de imágenes.

## <a name="next-steps"></a>Pasos siguientes

En los siguientes artículos se muestra cómo usar la tecnología de aprendizaje profundo en [Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/?WT.mc_id=docs-article-lazzeri):

- [Clasificación de dígitos manuscritos mediante un modelo TensorFlow](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-tensorflow?WT.mc_id=docs-article-lazzeri)
- [Clasificación de dígitos manuscritos mediante un estimador de TensorFlow y Keras](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-keras?WT.mc_id=docs-article-lazzeri)
- [Clasificación de imágenes mediante un modelo Pytorch](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-pytorch?WT.mc_id=docs-article-lazzeri)
- [Clasificación de dígitos manuscritos mediante un modelo Chainer](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-train-chainer?WT.mc_id=docs-article-lazzeri)
