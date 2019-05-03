---
title: Evaluar la precisión de voz personalizados - servicios de voz
titlesuffix: Azure Cognitive Services
description: En este documento aprenderá cómo trasvasar medir la calidad del modelo de texto a voz de Microsoft o sus modelos personalizados. Datos de la transcripción de audio + etiqueta humanos son necesarios para probar la exactitud y 30 minutos a 5 horas de audio representativa debe proporcionarse.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 7c1b3602b09c1a129923180c4b1d4a5f8293de2c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025914"
---
# <a name="evaluate-custom-speech-accuracy"></a>Evaluar la precisión de voz personalizado

En este documento, obtendrá información sobre cómo medir la calidad del modelo de texto a voz de Microsoft o sus modelos personalizados de sentido cuantitativo. Datos de la transcripción de audio + etiqueta humanos son necesarios para probar la exactitud y 30 minutos a 5 horas de audio representativa debe proporcionarse.

## <a name="what-is-word-error-rate-wer"></a>¿Qué es la tasa de errores de Word (WER)?

Es el estándar del sector para medir la precisión del modelo *tasa de errores de Word* (WER). WER cuenta el número de palabras incorrectas identificadas durante el reconocimiento, a continuación, se divide por el número total de las palabras proporcionadas en la transcripción de la etiqueta humanos. Por último, ese número se multiplica por 100% para calcular el WER.

![Fórmula WER](./media/custom-speech/custom-speech-wer-formula.png)

Ha identificado incorrectamente las palabras otoño en tres categorías:

* Inserción (I): Palabras que se agregan incorrectamente en la transcripción de hipótesis
* Eliminación (D): Palabras que están sin detectar en la transcripción de hipótesis
* Sustitución (S): Palabras que se sustituyen entre hipótesis y de referencia

Este es un ejemplo:

![Ejemplo de palabras identificadas incorrectamente](./media/custom-speech/custom-speech-dis-words.png)

## <a name="resolve-errors-and-improve-wer"></a>Resolver errores y mejorar WER

Puede usar el WER desde los resultados de reconocimiento de la máquina para evaluar la calidad del modelo que se usa con su aplicación, la herramienta o el producto. Un WER de 5-10% se considera que es buena calidad y está listo para usarse. Un WER del 20% es aceptable, pero que desee considerar entrenamiento adicional. Un WER del 30% o más señala una calidad deficiente y requiere la personalización y entrenamiento.

Cómo se distribuyen los errores es importante. Cuando se producen muchos errores de eliminación, suele ser debido a la intensidad de señal de audio débil. Para resolver este problema, debe recopilar datos de audio más cercano al origen. Errores de inserción significan que el audio se grabó en un entorno ruidoso y puede estar presente, hablan a la vez que causan problemas de reconocimiento. A menudo se producen errores de sustitución cuando una muestra suficiente de términos específicos de dominio se ha proporcionado como transcripciones etiquetados humanos o relacionados con texto.

Al analizar archivos individuales, puede determinar qué tipo de errores existen y qué errores son únicos en un archivo específico. Comprensión de los problemas en el nivel de archivo le ayudará a las mejoras de destino.

## <a name="create-a-test"></a>Crear una prueba

Si desea probar la calidad del modelo de línea de base de texto a voz de Microsoft o un modelo personalizado que ha formado, se pueden comparar dos modelos en paralelo para evaluar la precisión. La comparación incluye los resultados de reconocimiento y WER. Normalmente, un modelo personalizado se compara con el modelo de línea de base de Microsoft.

Para evaluar los modelos en paralelo:

1. Vaya a **Speech to text > personalizado de voz > pruebas**.
2. Haga clic en **Agregar prueba**.
3. Seleccione **evalúe su precisión**. Asigne un nombre, descripción, a la prueba y seleccione el conjunto de datos de la transcripción de audio + etiqueta humanos.
4. Seleccione hasta dos modelos que le gustaría probar.
5. Haga clic en **Create**(Crear).

Después de la prueba se ha creado correctamente, puede comparar los resultados en paralelo.

## <a name="side-by-side-comparison"></a>Comparación en paralelo

Una vez completada la prueba, indicado por el cambio de estado a *Succeeded*, encontrará una serie WER para ambos modelos incluidos en la prueba. Haga clic en el nombre de la prueba para ver la página de detalles de pruebas. Esta página de detalles muestra todas las palabras pronunciadas en el conjunto de datos, que indica los resultados del reconocimiento de los dos modelos, junto con la transcripción del conjunto de datos enviado. Con el fin de inspeccionar la comparación en paralelo, puede alternar los distintos tipos de errores, incluida la inserción, eliminación y sustitución. Al escuchar el audio y comparar los resultados del reconocimiento en cada columna, que muestra la transcripción de la etiqueta humanos y los resultados de dos modelos de voz a texto, puede decidir qué modelo satisfaga sus necesidades y dónde están entrenamiento adicionales y mejoras Obligatorio.

## <a name="next-steps"></a>Pasos siguientes

* [Entrenar el modelo.](how-to-custom-speech-train-model.md)
* [Implementar el modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionales

* [Preparar y probar los datos](how-to-custom-speech-test-data.md)
* [Inspeccione los datos](how-to-custom-speech-inspect-data.md)
