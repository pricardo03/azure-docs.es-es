---
title: Entrenar un modelo de voz personalizados - servicios de voz
titlesuffix: Azure Cognitive Services
description: Entrenamiento de voz a texto es necesario para mejorar la precisión del reconocimiento de modelo de línea de base de Microsoft o un modelo personalizado que se va a crear. Un modelo se entrena con etiqueta humanos transcripciones y el texto relacionado. Estos conjuntos de datos junto con los datos de audio cargados previamente, se usan para refinar y entrenar el modelo de texto a voz para reconocer palabras, frases, siglas, nombres y otros términos específicos de cada producto.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 194ae477bb3cba4ac7e3350da6b793c6fea6ecdb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025862"
---
# <a name="train-a-model-for-custom-speech"></a>Entrenar un modelo de voz personalizado

Entrenamiento de voz a texto es necesario para mejorar la precisión del reconocimiento de modelo de línea de base de Microsoft o un modelo personalizado que se va a crear. Un modelo se entrena con etiqueta humanos transcripciones y el texto relacionado. Estos conjuntos de datos junto con los datos de audio cargados previamente, se usan para refinar y entrenar el modelo de texto a voz para reconocer palabras, frases, siglas, nombres y otros términos específicos de cada producto. Los más en el dominio conjuntos de datos que proporcionan (datos que está relacionado con lo que los usuarios se indicará que es y lo que esperar para que reconozca), más precisas serán será el modelo, lo que resulta en un reconocimiento mejorado. Tenga en cuenta que procesando datos no relacionados con la formación, puede reducir o perjudicar la precisión del modelo.

## <a name="use-training-to-resolve-accuracy-issues"></a>Use la formación para solucionar problemas de precisión

Si está produciendo problemas de reconocimiento con el modelo, mediante la etiqueta humanos transcripciones y datos relacionados para el entrenamiento adicional pueden ayudar a mejorar la precisión. Use esta tabla para determinar qué conjunto de datos deben usar para abordar los problemas:

| Caso de uso | Tipo de datos | Cantidad de datos |
|----------|-----------|---------------|
| Se ha nombres propios | Se relacionan con el texto (frases o grabaciones de voz) | 10 MB a 500 MB |
| Las palabras se ha debido a un acento | Texto relacionado (pronunciación) | Proporcionar las palabras misrecognized |
| Eliminar palabras comunes o no reconoció | Transcripciones de audio + humanos con etiqueta | horas de transcripción de 10 a 1000 |

> [!IMPORTANT]
> Si no ha cargado un conjunto de datos, consulte la sección [preparar y probar los datos](how-to-custom-speech-test-data.md). Este documento proporciona instrucciones para cargar datos e instrucciones para crear conjuntos de datos de alta calidad.

## <a name="train-and-evaluate-a-model"></a>Entrenar y evaluar un modelo

Es el primer paso para entrenar un modelo cargar datos de entrenamiento. Use [preparar y probar los datos](how-to-custom-speech-test-data.md) para obtener instrucciones paso a paso Preparar la etiqueta humanos transcripciones y texto relacionado (declaraciones y las pronunciaciones). Después de cargar datos de entrenamiento, siga estas instrucciones para iniciar el entrenamiento del modelo:

1. Vaya a **Speech to text > personalizado de voz > aprendizaje**.
2. Haga clic en **entrenar modelo**.
3. A continuación, asigne a su formación un **nombre** y **descripción**.
4. Desde el **modelos de escenario y de línea base** menú desplegable, seleccione el escenario que mejor se adapte a su dominio. Si no está seguro de qué escenario se elija, seleccione **General**. El modelo de línea base es el punto de partida para el entrenamiento. Si no tienes una preferencia, puede usar la versión más reciente.
5. Desde el **seleccionar datos de entrenamiento** página, elija uno o varios conjuntos de datos transcripción de audio + etiqueta humanos que le gustaría usar para el entrenamiento.
6. Una vez completado el entrenamiento, puede elegir realizar pruebas en el modelo recién entrenado de precisión. Este paso es opcional.
7. Seleccione **crear** para generar el modelo personalizado.

La tabla de entrenamiento, se muestra una nueva entrada que corresponde a esta recién creada el modelo. La tabla también muestra el estado:  Procesamiento, se ha realizado correctamente, con errores.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>Evaluar la precisión de un modelo entrenado

Puede inspeccionar los datos y evaluar la precisión del modelo mediante estos documentos:

* [Inspeccione los datos](how-to-custom-speech-inspect-data.md)
* [Evaluar los datos](how-to-custom-speech-evaluate-data.md)


Si decide probar la exactitud, es importante seleccionar un conjunto de datos acústico es diferente a la usada con el modelo para hacerse una idea del rendimiento del modelo realista.

## <a name="next-steps"></a>Pasos siguientes

* [Implementar el modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionales

* [Preparar y probar los datos](how-to-custom-speech-test-data.md)
* [Inspeccione los datos](how-to-custom-speech-inspect-data.md)
* [Evaluar los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenar el modelo.](how-to-custom-speech-train-model.md)
