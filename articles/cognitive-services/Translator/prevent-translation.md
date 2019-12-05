---
title: 'Impedir la traducción de contenido: Translator Text API'
titleSuffix: Azure Cognitive Services
description: Impida que se traduzca contenido con Translator Text API. Translator Text API permite etiquetar contenido para que no se traduzca.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: swmachan
ms.openlocfilehash: 15a36451c18d65df6667f24284f3f69f3d1c06b8
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326757"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Impedir la traducción de contenido con Translator Text API

Translator Text API permite etiquetar contenido para que no se traduzca. Por ejemplo, es posible que desee etiquetar código, un nombre de marca, una palabra o una frase que no tengan sentido si se traducen.

## <a name="methods-for-preventing-translation"></a>Métodos para impedir la traducción
1. Utilice caracteres de escape con las etiquetas de Twitter: @somethingtopassthrough o #somethingtopassthrough. Quite los caracteres de escape después de la traducción. Esta es la expresión regular para las etiquetas válidas de Twitter: `\B@[A-Za-z]+[A-Za-z0-9_]+)`. Una etiqueta debe empezar con un signo "@", seguido de un carácter y, a continuación, seguido de uno o varios caracteres, dígitos o subrayado. Se recomienda mantener las etiquetas cortas y la etiqueta de apertura debe ir precedida de un espacio.

2. Etiquete el contenido con `notranslate`. Esto solo funciona cuando el elemento textType de entrada se establece como HTML.

   Ejemplo:

   ```html
   <span class="notranslate">This will not be translated.</span>
   <span>This will be translated. </span>
   ```
   
   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Utilice el [diccionario dinámico](dynamic-dictionary.md) para prescribir una traducción específica.

4. No pase la cadena a Translator Text API para que se traduzca.

5. Traductor personalizado: utilice un [diccionario de Traductor personalizado](custom-translator/what-is-dictionary.md) para prescribir la traducción de una frase cuya probabilidad es del 100 %.


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Impedir la traducción en la llamada API de Translator](reference/v3-0-translate.md)
