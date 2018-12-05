---
title: ¿Qué es un diccionario? - Custom Translator
titleSuffix: Azure Cognitive Services
description: Un diccionario es un documento alineado que especifica una lista de frases u oraciones (con sus respectivas traducciones) que Microsoft Translator siempre debe traducir de la misma manera. A veces, los diccionarios también se denominan glosarios o bases terminológicas.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 18260de82125c2e582e9c817ef733dd3757790b3
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "51975099"
---
# <a name="what-is-a-dictionary"></a>¿Qué es un diccionario?

Un diccionario es un par alineado de documentos que especifica una lista de frases u oraciones y sus traducciones correspondientes. Use un diccionario en el entrenamiento cuando quiera que Microsoft Translator siempre traduzca todas las instancias de la frase u oración de origen de la forma que especifique el diccionario. A veces, los diccionarios se denominan glosarios o bases terminológicas. Puede pensar en el diccionario como un "copiar y remplazar" a la fuerza para todos los términos incluidos.

Los diccionarios solo funcionan para los proyectos de pares de idiomas que tengan un sistema de traducción automática neuronal (NMT) de Microsoft totalmente compatible. Consulte http://www.aka.ms/translatorlanguages para obtener una lista completa de idiomas.

## <a name="phrase-dictionary"></a>Diccionario de frases 
Al incluir un diccionario de frases durante el entrenamiento de un modelo, todas las palabras y frases incluidas se traducen de la forma especificada. El resto de la oración se traduce como de costumbre. Puede usar un diccionario de frases para especificar las frases que no se deben traducir si proporciona la misma frase sin traducir en los archivos de origen y de destino del diccionario.

## <a name="sentence-dictionary"></a>Diccionario de oraciones
El diccionario de oraciones le permite especificar una traducción exacta de destino para una oración de origen. Para que exista una coincidencia con el diccionario de oraciones, la oración completa que se envíe debe coincidir con la entrada de origen del diccionario.  Si solo coincide una parte de la oración, la entrada será una coincidencia.  Cuando se detecta una coincidencia, se devuelve la entrada de destino del diccionario de oraciones.

## <a name="dictionary-only-trainings"></a>Entrenamientos de solo diccionario
Puede entrenar un modelo usando solo los datos de un diccionario. Para ello, seleccione solo el documento de diccionario (o los documentos de diccionario) que quiere incluir y pulse en Crear modelo. Ya que se trata de un entrenamiento de solo diccionario, no hay un número mínimo de oraciones de aprendizaje necesarias. Normalmente, el modelo completará el entrenamiento mucho más rápido que un entrenamiento estándar.  Los modelos resultantes utilizarán los modelos base de Microsoft para la traducción, con la adición de los diccionarios que ha agregado.  No obtendrá un informe de pruebas.

>[!Note]
>Custom Translator no alinea las oraciones en los archivos de diccionario, por lo que es importante que dichos documentos incluyan el mismo número de frases u oraciones (tanto de origen como de destino) y que estén perfectamente alineados.

## <a name="recommendations"></a>Recomendaciones

- Los diccionarios no reemplazan a los modelos entrenados con datos de aprendizaje.  Básicamente, los diccionarios buscan y reemplazan palabras o frases.  Dejar que el sistema aprenda con el material de aprendizaje mediante oraciones completas suele ser una mejor opción que el uso de un diccionario. 
- El diccionario de frases debe usarse con moderación. Cuando se reemplaza una frase dentro de una oración, el contexto de esa oración se pierde o se ve limitado para continuar con la traducción del resto de la oración. Como resultado, aunque la frase o palabra en la oración se traducirá según el diccionario de frases, la calidad general de la traducción de la oración a menudo se verá afectada.
- El diccionario de frases funciona bien para los nombres compuestos, como nombres de producto ("Microsoft SQL Server"), nombres propios ("Ciudad de Hamburgo") o las características del producto ("tabla dinámica"). No funciona igual de bien para los verbos o adjetivos, ya que normalmente se declinan en el idioma de origen o en el de destino. Evite agregar al diccionario de frases entradas que sean cualquier excepto nombres compuestos. 
- Cuando se usa un diccionario, las mayúsculas, minúsculas y signos de puntuación de las traducciones serán iguales a los que se hayan proporcionado en el archivo de destino. El uso de mayúsculas y signos de puntuación se omite cuando se intentan identificar coincidencias entre la oración ingresada y las oraciones de origen en el archivo de diccionario. Por ejemplo, supongamos que hemos entrenado un sistema de inglés a español con un diccionario que contenía "City of Hamburg" en el archivo de origen y "Ciudad de hamburg" en el archivo de destino. Si se solicita la traducción de una oración que incluya la frase "city of Hamburg", entonces "city of Hamburg" coincide con el archivo de diccionario para la entrada "City of Hamburg" y se le asigna "Ciudad de hamburg" a la traducción final.
- Si una palabra aparece varias veces en un archivo de diccionario, el sistema usará siempre la última entrada proporcionada. El diccionario no debe contener varias traducciones para la misma palabra.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información en relación a las [directrices sobre los formatos de documento](document-formats-naming-convention.md).