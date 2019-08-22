---
title: Introducción a la relación entre aprendizaje profundo y aprendizaje automático
titleSuffix: Azure Machine Learning service
description: Obtenga información acerca de la relación entre aprendizaje profundo y aprendizaje automático, y cómo encajan en la categoría de inteligencia artificial. En este artículo se describe la forma en que el aprendizaje profundo soluciona distintos escenarios, como la detección de fraudes, el reconocimiento facial y de voz, el análisis de opiniones y la previsión de series temporales.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: lazzeri
author: FrancescaLazzeri
ms.date: 08/07/2019
ms.openlocfilehash: 186ba03142e01c4b9340fc5b492621cc17575104
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934292"
---
# <a name="introduction-to-deep-learning-vs-machine-learning"></a>Introducción a la relación entre aprendizaje profundo y aprendizaje automático

Este artículo le ayuda a conocer la relación entre el aprendizaje profundo y el aprendizaje automático. En él se compararán los dos conceptos y aprenderá cómo encajan en la categoría más amplia de inteligencia artificial. Por último, el artículo se describe la forma en que el aprendizaje profundo se puede aplicar a distintos escenarios del mundo real, como la detección de fraudes, el reconocimiento facial y de voz, el análisis de opiniones y la previsión de series temporales.

## <a name="how-do-deep-learning-machine-learning-and-ai-relate-to-one-another"></a>Relación entre aprendizaje profundo, aprendizaje automático e inteligencia artificial

![Diagrama de relaciones: Inteligencia artificial frente a aprendizaje automático y a aprendizaje profundo](./media/concept-deep-learning-vs-machine-learning/ai-vs-machine-learning-vs-deep-learning.png)

Para conocer la diferencia entre el aprendizaje profundo, el aprendizaje automático y la inteligencia artificial, obtenga información acerca de cada uno de estos campos de estudio:

- El **aprendizaje profundo** es un subconjunto del aprendizaje automático basado en redes neuronal artificiales que permiten a un equipo entrenarse a sí mismo. En este caso, el _proceso de aprendizaje_ se llama _profundo_ porque la estructura de redes neuronales artificiales se compone de varias capas de entrada, salida y ocultas. Cada capa contiene unidades que transforman los datos de entrada en información que la capa siguiente puede usar para realizar una tarea de predicción determinada. Gracias a esta estructura, un equipo puede aprender a través de su propio procesamiento de datos.

- El **aprendizaje automático** es un subconjunto de la inteligencia artificial que incluye técnicas (como el aprendizaje profundo) que permiten a los equipos mejorar en las tareas con la experiencia. En este caso, el _proceso de aprendizaje_ se basa en los pasos siguientes:

    1. Alimente un algoritmo con datos proporcionándole más información (por ejemplo, realizando la extracción de características).
    2. Utilice estos datos para entrenar un modelo.
    3. Pruebe e implemente el modelo.
    4. Consuma el modelo implementado para realizar una tarea de predicción automatizada concreta.

- La  **inteligencia artificial (IA)** es una técnica que permite a los equipos imitar la inteligencia humana. Incluye el aprendizaje automático. Es importante conocer la relación entre _aprendizaje automático, aprendizaje profundo e inteligencia artificial_: El aprendizaje automático es una forma de lograr inteligencia artificial, lo que significa que, mediante el uso de técnicas de aprendizaje automático y aprendizaje profundo, se pueden crear sistemas informáticos y aplicaciones que puedan realizar tareas asociadas normalmente a la inteligencia humana, como la percepción visual, el reconocimiento de voz, la toma de decisiones y la traducción de un idioma a otro.

## <a name="comparison-of-deep-learning-vs-machine-learning"></a>Comparación entre aprendizaje profundo y aprendizaje automático

Ahora que ya tiene una información general acerca del aprendizaje automático y el aprendizaje profundo, vamos a comparar ambas técnicas. En el caso del aprendizaje automático, es preciso indicar al algoritmo cómo realizar una predicción precisa proporcionándole más información; mientras que en el caso del aprendizaje profundo, el algoritmo es capaz de aprender a través del procesamiento de sus propios datos gracias a la estructura de una red neuronal artificial.

En la tabla siguiente se comparan las dos técnicas con más detalle:

| |Todo el aprendizaje automático |Solo aprendizaje profundo|
|---|---|---|
|  **Número de puntos de datos** | Puede usar pequeñas cantidades de datos que proporcionan los usuarios | Requiere una gran cantidad de datos de entrenamiento para sacar conclusiones concisas |
|  **Dependencias del hardware** | Puede trabajar en equipos lentos. No necesita gran cantidad de potencia de cálculo | Depende de máquinas rápidas. Realiza intrínsecamente un gran número de operaciones de multiplicación de matrices. Estas operaciones se pueden optimizar eficazmente mediante una GPU |
|  **Proceso de características** | Requiere que los usuarios creen e identifiquen con precisión las características | Aprende las características de alto nivel de los datos y crea nuevas características automáticamente |
|  **Enfoque del aprendizaje** | Divide las tareas en pequeñas partes y, después, combina los resultados recibidos para sacar una conclusión | Resuelve el problema de forma global |
|  **Tiempo de ejecución** | Comparativamente tarda menos tiempo en entrenarse, tarda unos segundos y unas pocas horas | Tarda demasiado tiempo en entrenarse, porque los algoritmos de aprendizaje profundo tienen muchos parámetros |
|  **Salida** | La salida suele ser un valor numérico, como una puntuación o una clasificación | La salida puede ser una puntuación, un texto, un elemento o un sonido |

## <a name="deep-learning-use-cases-what-problems-does-it-solve"></a>Casos de uso del aprendizaje profundo: ¿Qué problemas soluciona?

A causa de la estructura de la red neuronal artificial, el aprendizaje profundo es excelente para identificar patrones en datos no estructurados, como imágenes, sonido, vídeo y texto. Por esta razón, está transformando rápidamente muchos sectores, como el de la atención sanitaria, el energético, la tecnología financiera y el transporte, entre otros, donde está haciendo que se replanteen los procesos empresariales tradicionales. En los párrafos siguientes se describen algunas de las aplicaciones más comunes del aprendizaje profundo.

### <a name="named-entity-recognition"></a>Reconocimiento de entidades con nombre

Un uso de las redes de aprendizaje profundo es el reconocimiento de entidades con nombre, que es una manera de extraer determinados tipos de información, como personas, lugares, compañías o cosas de datos no estructurados y sin etiquetar. Esa información luego se puede almacenar en un esquema estructurado para generar una lista de direcciones o puede servir como banco de pruebas para un motor de validación de identidades.

### <a name="object-detection"></a>Detección de objetos

El aprendizaje profundo se ha aplicado en muchos casos de uso de detección de objetos. La detección de objetos es en realidad un proceso que tiene dos partes: la clasificación de imágenes y la localización de imágenes. La clasificación de imágenes determina qué son los objetos de la imagen, como un automóvil o una persona, mientras que la localización de imágenes proporciona la ubicación específica de estos objetos. La detección de objetos ya se está usando en sectores como el juego, el comercio minorista, el turismo y el de vehículos autónomos.

### <a name="image-caption-generation"></a>Generación de subtítulos para imágenes

De forma similar a la tarea de reconocimiento de imágenes, la generación de subtítulos para imágenes es la tarea en que, para una imagen determinada, el sistema debe generar un subtítulo que describa el contenido de la imagen. Una vez que pueda detectar objetos en fotografías y generar etiquetas para esos objetos, puede ver que el siguiente paso es convertir esas etiquetas en una descripción de frase coherente. Por lo general, los sistemas implican el uso de redes neuronales convolucionales muy grandes para la detección de objetos en las fotografías y, después, una red neuronal recurrente (RNN) para convertir las etiquetas en una oración coherente.

### <a name="machine-translation"></a>Traducción automática

La traducción automática toma palabras, frases u oraciones de un idioma y las traduce automáticamente a otro. La traducción automática lleva mucho tiempo utilizándose, pero el aprendizaje profundo está obteniendo magníficos resultados en dos áreas específicas: la traducción automática de texto (y de voz a texto) y traducción automática de imágenes. Con la transformación apropiada de los datos, una red profunda es capaz de comprender texto, audio y señales visuales. La traducción automática se puede usar para identificar fragmentos de sonido en archivos de audio mayores y transcribir la palabra hablada o la imagen como texto.

### <a name="text-analytics"></a>Text Analytics

Una tarea importante que puede realizar el aprendizaje profundo es la detección electrónica. Las empresas usan el análisis de texto basado en el aprendizaje profundo para la detección de negociaciones en el mercado de valores que hacen un uso indebido de información privilegiada y el cumplimiento normativo gubernamental. Los fondos de cobertura utilizan el análisis de texto para explorar en profundidad los repositorios de documentos masivos con el fin de obtener información acerca del rendimiento de las inversiones en futuros y el sentimiento del mercado. El caso de uso del análisis de texto basado en aprendizaje profundo gira en torno a su capacidad de analizar grandes cantidades de datos de texto para realizar análisis o generar agregaciones.

## <a name="what-are-artificial-neural-networks"></a>¿Qué son las redes neuronales artificiales?

Las redes neuronal artificiales se forman con capas de nodos conectados. Los modelos de aprendizaje profundo usan redes neuronales con un gran número de capas. A continuación, se describen las tipologías de redes neuronales artificiales más populares.

### <a name="feedforward-neural-network"></a>Red neuronal del tipo feedforward

La red neuronal del tipo feedforward es el tipo más básico. En ella, la información se desplaza solo en una dirección, de la capa de entrada a la capa de salida. Las redes neuronales de tipo feedforward transforman una entrada pasándola por una serie de capas ocultas. Cada capa consta de un conjunto de neuronas, donde cada capa está totalmente conectada a todas los neuronas de la capa anterior. Por último, hay una última capa totalmente conectada (la capa de salida) que representa las predicciones generadas.

### <a name="recurrent-neural-network"></a>Redes neuronales recurrentes

Las redes neuronales recurrentes son un tipo más utilizado de red neuronal artificial, que funciona según el principio de guardar la salida de una capa y volver a introducirla en la capa de entrada para ayudar a predecir el resultado de la capa. Estas redes neuronales poseen más capacidades de aprendizaje y se emplean ampliamente para tareas más complejas, como el reconocimiento de escritura a mano o de idiomas.

### <a name="convolutional-neural-networks"></a>Redes neuronal convolucionales

Una red neuronal convolucional es un tipo especialmente eficaz de red neuronal artificial que presenta una arquitectura única. En primer lugar, las capas se organizan en tres dimensiones: ancho, alto y profundo. Además, las neuronas de una capa no se conectan con todas las neuronas de la capa siguiente, sino solo a una región pequeña de la misma. Por último, la salida final se reducirá a un solo vector de puntuaciones de probabilidad, organizadas a lo largo de la dimensión de profundidad. Estas redes neuronales se han utilizado en áreas como el reconocimiento de imágenes y la clasificación.

## <a name="next-steps"></a>Pasos siguientes

En los siguientes artículos se muestra cómo usar la tecnología de aprendizaje profundo en [Azure Machine Learning Service](/azure/machine-learning/service/):

- [Clasificación de dígitos manuscritos con un modelo TensorFlow](how-to-train-tensorflow.md)
- [Clasificación de dígitos manuscritos con un estimador de TensorFlow y Keras](how-to-train-keras.md)
- [Clasificación de imágenes con un modelo Pytorch](how-to-train-pytorch.md)
- [Clasificación de dígitos manuscritos con un modelo Chainer](how-to-train-pytorch.md)
