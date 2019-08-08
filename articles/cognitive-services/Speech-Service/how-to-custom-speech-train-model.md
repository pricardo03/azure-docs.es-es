---
title: 'Entrenamiento de un modelo de Custom Speech: servicio de Voz'
titleSuffix: Azure Cognitive Services
description: El entrenamiento de voz a texto es necesario para mejorar la precisión del reconocimiento en el modelo de línea de base de Microsoft o en un modelo personalizado que tenga pensado crear. Un modelo se entrena mediante transcripciones con etiqueta humana y el texto relacionado. Estos conjuntos de datos, junto con los datos de audio cargados anteriormente, se usan para refinar y entrenar el modelo de texto a voz para el reconocimiento de palabras, frases, acrónimos, nombres y otros términos específicos de cada producto.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: b5893b4f07444b07bf142971a5df4776e549d307
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562810"
---
# <a name="train-a-model-for-custom-speech"></a>Entrenamiento de un modelo de Custom Speech

El entrenamiento de voz a texto es necesario para mejorar la precisión del reconocimiento en el modelo de línea de base de Microsoft o en un modelo personalizado que tenga pensado crear. Un modelo se entrena mediante transcripciones con etiqueta humana y el texto relacionado. Estos conjuntos de datos, junto con los datos de audio cargados anteriormente, se usan para refinar y entrenar el modelo de texto a voz para el reconocimiento de palabras, frases, acrónimos, nombres y otros términos específicos de cada producto. Cuantos más conjuntos de datos en dominio proporcione (datos que están relacionados con qué dirán los usuarios y qué espera reconocer), más preciso será su modelo y, por lo tanto, mejor será el reconocimiento. Tenga en cuenta que el uso de datos no relacionados en el entrenamiento puede reducir o perjudicar la precisión del modelo.

## <a name="use-training-to-resolve-accuracy-issues"></a>Uso del entrenamiento para solucionar problemas de precisión

Si se está encontrando con problemas de reconocimiento en el modelo, el uso de transcripciones con etiqueta humana y los datos relacionados para realizar entrenamiento adicional puede ayudar a mejorar la precisión. Use esta tabla para determinar qué conjunto de datos se debe usar para solucionar su problema:

| Caso de uso | Tipo de datos |
|----------|-----------|
| Mejorar la precisión del reconocimiento en el vocabulario y la gramática específicos del sector, como la terminología médica o la jerga de TI | Texto relacionado (frases o expresiones) |
| Definir el formato fonético y mostrado de una palabra o término que tenga una pronunciación no estándar, como nombres de producto o acrónimos | Texto relacionado (pronunciación) |
| Mejorar la precisión del reconocimiento en estilos de habla, acentos o ruidos de fondo específicos | Transcripciones de audio con etiqueta humana |
> [!IMPORTANT]
> Si no ha cargado un conjunto de datos, consulte [Preparación y prueba de los datos](how-to-custom-speech-test-data.md). En este documento se proporcionan instrucciones para cargar datos y directrices para crear conjuntos de datos de alta calidad.

## <a name="train-and-evaluate-a-model"></a>Entrenamiento y evaluación de un modelo

El primer paso para entrenar un modelo es cargar los datos de entrenamiento. Use [Preparación y prueba de los datos](how-to-custom-speech-test-data.md) para obtener instrucciones paso a paso para preparar las transcripciones con etiqueta humana y el texto relacionado (expresiones y pronunciaciones). Después de cargar los datos de entrenamiento, siga estas instrucciones para empezar a entrenar el modelo:

1. Vaya a **Speech-to-text > Custom Speech > Training** (Conversión de voz a texto > Custom Speech > Aprendizaje).
2. Haga clic en **Train model** (Entrenar modelo).
3. A continuación, asigne a su entrenamiento un **nombre** y una **descripción**.
4. En el menú desplegable **Scenario and Baseline model** (Escenario y modelo de línea de base), seleccione el escenario que mejor se adapte a su dominio. Si no está seguro de qué escenario elegir, seleccione **General**. El modelo de línea de base es el punto de partida para el entrenamiento. Si no tiene una preferencia, puede usar el último.
5. En la página **Select training data** (Seleccionar datos de entrenamiento), elija uno o varios conjuntos de datos de audio + transcripción con la etiqueta humana que quiera usar para el entrenamiento.
6. Una vez finalizado el entrenamiento, puede elegir realizar pruebas de precisión en el modelo recién entrenado. Este paso es opcional.
7. Seleccione **Create** (Crear) para generar el modelo personalizado.

En la tabla de entrenamiento se muestra una nueva entrada que corresponde a este modelo recién creado. En la tabla también se muestra el estado:  Procesando, Correcto, Error.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>Evaluación de la precisión de un modelo entrenado

Puede inspeccionar los datos y evaluar la precisión del modelo mediante estos documentos:

* [Inspección de los datos](how-to-custom-speech-inspect-data.md)
* [Evaluación de los datos](how-to-custom-speech-evaluate-data.md)


Si eligió probar la precisión, es importante seleccionar un conjunto de datos acústico diferente del usado para la creación del modelo para obtener una idea realista del rendimiento del modelo.

## <a name="next-steps"></a>Pasos siguientes

* [Implementación del modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionales

* [Preparación y prueba de los datos](how-to-custom-speech-test-data.md)
* [Inspección de los datos](how-to-custom-speech-inspect-data.md)
* [Evaluación de los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenamiento del modelo](how-to-custom-speech-train-model.md)
