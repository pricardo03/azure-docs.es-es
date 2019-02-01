---
title: 'Filtrado de datos: Traductor personalizado'
titleSuffix: Azure Cognitive Services
description: Al enviar documentos que se usarán para entrenar un sistema personalizado, dichos documentos se someten a una serie de pasos de procesamiento y filtrado para prepararse para el aprendizaje.
author: jann-skotdal
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 12/18/2018
ms.author: v-jansko
ms.topic: article
ms.openlocfilehash: dadcf228d2a633426fc759ca6914b8b894ea8c3d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226322"
---
# <a name="data-filtering"></a>Filtrado de datos 

Al enviar documentos que se usarán para entrenar un sistema personalizado, dichos documentos se someten a una serie de pasos de procesamiento y filtrado para prepararse para el aprendizaje. Estos pasos se explican aquí. Conocer el filtrado puede ayudarle a entender el recuento de oraciones que aparece en el traductor personalizado, así como los pasos que puede seguir para preparar los documentos para el entrenamiento de Traductor personalizado. 

## <a name="sentence-alignment"></a>Alineación de frases 
Si el documento no está en formato XLIFF, TMX o ALIGN, Traductor personalizado alinea las oraciones de los documentos de origen y de destino entre sí, oración por oración. Traductor no realiza la alineación del documento, sigue la nomenclatura de los documentos para buscar el documento coincidente del otro idioma. Dentro del documento, Traductor personalizado intenta encontrar la frase correspondiente en el otro idioma. Usa el marcado de documento, como etiquetas HTML incrustadas, para ayudar con la alineación.  

Si ve una gran discrepancia entre el número de frases en el lado de origen y de destino de los documentos, es posible que el documento no fuese paralelo en primer lugar, o que no se pueda alinear bien por otros motivos. Si el documento se empareja con una gran diferencia (>10 %) de oraciones en cada lado, deberá asegurarse de que están realmente en paralelo. Traductor personalizado muestra una advertencia junto al documento si el recuento de frases es sospechosamente diferente.  


## <a name="deduplication"></a>Desduplicación 
Traductor personalizado quita las frases que están presentes en los documentos de prueba y optimización de los datos de aprendizaje. La eliminación se realiza de forma dinámica dentro de la ejecución de aprendizaje, no en el paso de procesamiento de datos. Traductor personalizado le notifica el recuento de oraciones en la información general del proyecto antes de esa eliminación.  

## <a name="length-filter"></a>Filtro de longitud 
* Quite las frases con solo una palabra a ambos lados. 
* Quite las frases con más de 100 palabras a ambos lados.  Las frases en chino, japonés o coreano están exentas. 
* Quite las frases con menos de 3 caracteres. Las frases en chino, japonés o coreano están exentas. 
* Quite las frases con más de 2000 caracteres en chino, japonés o coreano. 
* Quite las frases con menos de un 1 % de caracteres alfabéticos. 
* Quite las entradas de diccionario que contengan más de 50 palabras. 

 
## <a name="white-space"></a>Espacio en blanco 
* Reemplace cualquier secuencia de caracteres de espacios en blanco, incluyendo las tabulaciones y secuencias de Retorno de carro/Avance de línea con un carácter de espacio único. 
* Quitar espacio inicial o final de la oración 


## <a name="sentence-end-punctuation"></a>Puntuación de final de frase 
Reemplace varios caracteres de puntuación de final de frase con una sola instancia.  

 
## <a name="japanese-character-normalization"></a>Normalización de caracteres japoneses 
Normalice los caracteres japoneses duplicados: Convierta caracteres de ancho medio a ancho completo. 

 
## <a name="unescaped-xml-tags"></a>Etiquetas XML sin escape 
El filtrado transforma las etiquetas sin escape en etiquetas con escape: 
* `&lt;` se convierte en `&amp;lt;` 
* `&gt;` se convierte en `&amp;gt;` 
* `&amp;` se convierte en `&amp;amp;` 

 
## <a name="invalid-characters"></a>Caracteres no válidos 
Traductor personalizado quita frases que contengan el carácter Unicode U+FFFD. El carácter U+FFFD indica una conversión de codificación incorrecta. 

## <a name="next-steps"></a>Pasos siguientes

- [Entrenamiento de un modelo](how-to-train-model.md) en Traductor personalizado.
