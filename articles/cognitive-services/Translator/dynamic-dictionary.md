---
title: Diccionario dinámico de Translator Text API
titlesuffix: Azure Cognitive Services
description: Descripción del uso de la característica de diccionario dinámico de Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 56558a2da5f29611d90021e9efb292720d1cea35
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128135"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>Descripción del uso de la característica de diccionario dinámico de Translator Text API

Si ya conoce la traducción que quiere aplicar a una palabra o frase, puede proporcionarla como marcado dentro de la solicitud. El diccionario dinámico solo es seguro para los nombres compuestos, como nombres propios y nombres de productos. 

**Sintaxis:** 

<mstrans:dictionary translation="traducción de la frase">phrase</mstrans:dictionary>

**Ejemplo: en-de:**

Entrada de origen: The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry (La palabra "wordomatic" es una entrada del diccionario).

Salida de destino: Das Wort "wordomatic" ist ein Wörterbucheintrag.

Esta característica funciona del mismo modo con y sin el modo HTML. 

La característica debe usarse con moderación. La mejor forma de personalización de traducción, y más adecuada, es mediante el uso de Custom Translator. Custom Translator hace un uso completo del contexto y las probabilidades estadísticas. Si tiene o puede crear datos de aprendizaje que muestren el trabajo o la frase en contexto, obtendrá resultados mucho mejores. Para más información sobre el traductor personalizado, consulte [http://aka.ms/CustomTranslator](http://aka.ms/CustomTranslator).

