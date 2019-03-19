---
title: ¿Qué son el entrenamiento y los modelos? - Custom Translator
titleSuffix: Azure Cognitive Services
description: 'Un modelo es el sistema que proporciona una traducción para pares de idiomas específicos. El resultado de un entrenamiento correcto es un modelo. Cuando se entrena un modelo, se requieren tres conjuntos de datos mutuamente exclusivos: el conjunto de datos de aprendizaje, el de ajuste y el de pruebas.'
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-rada
ms.openlocfilehash: adc78b176afb0a43a7e1e3a7bb882282a914761d
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57783081"
---
# <a name="what-are-trainings-and-models"></a>¿Qué son los entrenamientos y los modelos?

Un modelo es el sistema que proporciona una traducción para pares de idiomas específicos.
El resultado de un entrenamiento correcto es un modelo. Cuando se entrena un modelo, se requieren tres conjuntos de datos mutuamente exclusivos: el conjunto de datos de aprendizaje, el de ajuste y el de pruebas. También se pueden proporcionar datos de diccionario.

Si solo se proporcionan datos de aprendizaje al poner en cola un entrenamiento, Custom Translator reunirá automáticamente los conjuntos de datos de pruebas y de ajuste. Excluirá 5000 oraciones de los datos de aprendizaje y usará 2500 para reunir un conjunto de pruebas y otro de ajustes.

## <a name="training-dataset-for-custom-translator"></a>Conjunto de datos de aprendizaje para Custom Translator

Custom Translator usa los documentos incluidos en el conjunto de aprendizaje como base para generar el modelo. Durante la ejecución del entrenamiento, se alinean (o emparejan) las oraciones que están presentes en estos documentos. Puede tomarse libertades durante la creación del conjunto de documentos de aprendizaje. Puede incluir documentos que crea que son de importancia tangencial en un modelo. Vuelva a incluirlos en otro entrenamiento para ver el efecto que esto tiene en la puntuación [BLEU (suplente de evaluación bilingüe)](what-is-bleu-score.md). Siempre que el conjunto de ajuste y el de pruebas sean constantes, no dude en experimentar con la creación del conjunto de aprendizaje. Este enfoque es una forma eficaz de modificar la calidad de su sistema de traducción.

Puede ejecutar varios entrenamientos dentro de un proyecto y comparar las [puntuaciones BLEU ](what-is-bleu-score.md) en todas las ejecuciones del entrenamiento. Cuando ejecute varios entrenamientos para compararlos, asegúrese de usar los mismos datos de prueba o de ajuste cada vez. También asegúrese de inspeccionar los resultados manualmente en la pestaña de [pruebas](how-to-view-system-test-results.md).

## <a name="tuning-dataset-for-custom-translator"></a>Conjunto de datos de ajuste para Custom Translator

Custom Translator utiliza los documentos paralelos incluidos en este conjunto para optimizar el sistema de traducción y así obtener resultados óptimos.

El conjunto de ajuste se usa durante el entrenamiento para ajustar todos los parámetros y las ponderaciones del sistema de traducción según los valores óptimos. Elija su conjunto de ajuste cuidadosamente: debe ser representativo del contenido de los documentos que va a traducir en el futuro. El conjunto de ajuste tiene una importante influencia en la calidad de las traducciones generadas. El ajuste permite al sistema de traducción proporcionar traducciones más cercanas a los ejemplos proporcionados en el conjunto de datos de ajuste. No se necesitan más que 2500 oraciones para el conjunto de ajuste. Para una calidad de traducción óptima, se recomienda que seleccione manualmente al conjunto de ajuste mediante la selección del grupo de oraciones más representativas.

Durante la creación del conjunto de ajuste, seleccione las oraciones que tengan una longitud representativa de las oraciones que espera traducir en el futuro. También debe elegir las oraciones que contengan palabras y frases que pretende traducir, con la distribución aproximada que espera encontrar en futuras traducciones. En la práctica, las oraciones con una longitud de entre 8 y 18 palabras generarán los mejores resultados, dado que estas oraciones contienen suficiente contexto para mostrar inflexiones y proporcionan una longitud que es significativa sin ser demasiado compleja.

Una buena descripción del tipo de oraciones que debe usar en el conjunto de ajuste es la prosa: oraciones reales y fluidas. No celdas de una tabla, ni poemas, ni listas de cosas, ni solo puntuación o números: idioma común.

Si selecciona manualmente el conjunto de datos de ajuste, en él no debe repetirse ninguna de las oraciones incluidas en los datos de aprendizaje o de pruebas. El conjunto de ajuste tiene un impacto importante en la calidad de las traducciones: elija cuidadosamente las oraciones.

Si no está seguro de qué opciones elegir para el conjunto de ajuste, simplemente seleccione el conjunto de aprendizaje y permita que Custom Translator seleccione el conjunto de ajuste por usted. Cuando Custom Translator elige automáticamente al conjunto de ajuste, usa un subconjunto aleatorio de oraciones tomadas de los documentos bilingües de aprendizaje y excluye a las oraciones que están en el material de aprendizaje.

## <a name="testing-dataset-for-custom-translator"></a>Conjunto de datos de pruebas para Custom Translator

Los documentos paralelos que se incluyen en el conjunto de pruebas se utilizan para calcular la puntuación BLEU (suplente de evaluación bilingüe). Esta puntuación indica la calidad de su sistema de traducción. Realmente, esta puntuación le indica qué tanto se acercan las traducciones del sistema de traducción creado con este entrenamiento a las oraciones de referencia incluidas en el conjunto de datos de pruebas.

La puntuación BLEU es una medida de la delta entre la traducción automática y la traducción de referencia. Su valor varía entre 0 y 100. Una puntuación de 0 indica que en la traducción no aparece ni una sola palabra de la referencia. Una puntuación de 100 indica que la traducción automática coincide exactamente con la referencia: las mismas palabras están en exactamente la misma posición. La puntuación que recibe es la puntuación BLEU promedio de todas las oraciones en el conjunto de pruebas.

El conjunto de pruebas debe incluir documentos paralelos en los que las oraciones del idioma de destino sean las traducciones más deseables de las oraciones del idioma de origen correspondientes. Es posible que quiera usar los mismos criterios que utilizó para crear el conjunto de ajuste. Sin embargo, el conjunto de pruebas no influye en la calidad del sistema de traducción. Se utiliza exclusivamente para generar la puntuación BLEU, y nada más.

No se necesitan más de 2500 oraciones para el conjunto de pruebas. Cuando el sistema elige automáticamente al conjunto de pruebas, usa un subconjunto aleatorio de oraciones tomadas de los documentos bilingües de aprendizaje y excluye a las oraciones que están en el material de aprendizaje.

Puede ver las traducciones personalizadas del conjunto de pruebas y compararlas con las traducciones proporcionadas en el conjunto de pruebas. Para ello, vaya a la pestaña de pruebas dentro de un modelo.
