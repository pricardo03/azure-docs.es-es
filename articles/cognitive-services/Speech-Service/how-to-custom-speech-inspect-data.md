---
title: Inspeccionar la calidad de los datos de voz personalizados - servicios de voz
titlesuffix: Azure Cognitive Services
description: Conversión personalizada de voz proporciona herramientas que le permiten inspeccionar visualmente la calidad del reconocimiento de un modelo mediante la comparación de datos de audio con el resultado de reconocimiento correspondiente. Desde el portal de voz personalizado, puede reproducir audio cargado y determinar si el resultado de reconocimiento proporcionada es correcto.  Esta herramienta le permite inspeccionar rápidamente la calidad del modelo de voz a texto de línea de base de Microsoft o un modelo entrenado personalizado sin tener que transcribirlo datos de audio.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 682f3df362a7fbb0e95a07aa8a8f3a068367eef2
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025823"
---
# <a name="inspect-custom-speech-data"></a>Inspeccionar los datos de voz personalizado

> [!NOTE]
> Esta página se supone que ha leído [preparar los datos de prueba de voz personalizado](how-to-custom-speech-test-data.md) y ha cargado un conjunto de datos para su inspección.

Conversión personalizada de voz proporciona herramientas que le permiten inspeccionar visualmente la calidad del reconocimiento de un modelo mediante la comparación de datos de audio con el resultado de reconocimiento correspondiente. Desde el portal de voz personalizado, puede reproducir audio cargado y determinar si el resultado de reconocimiento proporcionada es correcto. Esta herramienta le permite inspeccionar rápidamente la calidad del modelo de voz a texto de línea de base de Microsoft o un modelo entrenado personalizado sin tener que transcribirlo datos de audio.

En este documento, obtendrá información sobre cómo inspeccionar visualmente la calidad de un modelo con los datos de entrenamiento cargado previamente.

En esta página, obtendrá información sobre cómo inspeccionar visualmente la calidad del modelo de voz a texto de línea de base de Microsoft o un modelo personalizado que ha formado. Usará los datos que se cargó en el **datos** ficha para realizar pruebas.

## <a name="create-a-test"></a>Crear una prueba

Siga estas instrucciones para crear una prueba:

1. Vaya a **Speech to text > personalizado de voz > pruebas**.
2. Haga clic en **Agregar prueba**.
3. Seleccione **inspeccionar calidad (datos de solo Audio)**. Asigne un nombre, descripción, a la prueba y seleccione el conjunto de datos de audio.
4. Seleccione hasta dos modelos que le gustaría probar.
5. Haga clic en **Create**(Crear).

Una vez se ha creado correctamente una prueba, puede comparar los modelos en paralelo.

## <a name="side-by-side-model-comparisons"></a>Comparaciones de modelo en paralelo

Cuando el estado de la prueba es *Succeeded*, haga clic en el nombre de elemento de prueba para ver los detalles de la prueba. Esta página de detalles muestra todas las palabras pronunciadas en el conjunto de datos, que indica los resultados del reconocimiento de los dos modelos, junto con la transcripción del conjunto de datos enviado.

Con el fin de inspeccionar la comparación en paralelo, puede alternar los distintos tipos de errores, incluida la inserción, eliminación y sustitución. Al escuchar el audio y comparar los resultados del reconocimiento de cada columna (mostrando la transcripción de la etiqueta humanos y los resultados de dos modelos de voz a texto), puede decidir qué modelo satisfaga sus necesidades y donde se necesitan mejoras.

Inspección de pruebas de calidad es útil para validar si la calidad de un punto de conexión de reconocimiento de voz es suficiente para una aplicación.  Para una medida objetiva de precisión, la necesidad de transcripción de audio, siga las instrucciones que encontrará en fase de prueba: Evalúe su precisión.

## <a name="next-steps"></a>Pasos siguientes

* [Evaluar los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenar el modelo.](how-to-custom-speech-train-model.md)
* [Implementar el modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionales

* [Preparar los datos de prueba de voz personalizado](how-to-custom-speech-test-data.md)
