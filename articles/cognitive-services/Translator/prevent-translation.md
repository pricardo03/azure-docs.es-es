---
title: 'Impedir la traducción de contenido: Translator Text API'
titlesuffix: Azure Cognitive Services
description: Impida que se traduzca contenido con Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: 0b227803379ed0a64d17c7a7d19b187250b9f845
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226922"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Impedir la traducción de contenido con Translator Text API

Translator Text API permite etiquetar contenido para que no se traduzca. Por ejemplo, es posible que desee etiquetar código, un nombre de marca, una palabra o una frase que no tengan sentido si se traducen. 

## <a name="methods-for-preventing-translation"></a>Métodos para impedir la traducción
1. Utilice caracteres de escape con las etiquetas de Twitter: @somethingtopassthrough o #somethingtopassthrough. Quite los caracteres de escape después de la traducción.

2. Etiquete el contenido con `notranslate`.

   Ejemplo:

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
