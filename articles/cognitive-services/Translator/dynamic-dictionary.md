---
title: Diccionario dinámico de Translator Text API
titlesuffix: Azure Cognitive Services
description: Descripción del uso de la característica de diccionario dinámico de Translator Text API.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: v-pawal
ms.openlocfilehash: d3a77ee9273e9f587b6157bb6ed2294865b1fca5
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514258"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Cómo usar un diccionario dinámico

Si ya conoce la traducción que quiere aplicar a una palabra o frase, puede proporcionarla como marcado dentro de la solicitud. El diccionario dinámico solo es seguro para los nombres compuestos, como nombres propios y nombres de productos.

**Sintaxis:**

<mstrans:dictionary translation="traducción de la frase">phrase</mstrans:dictionary>

**Ejemplo: en-de:**

Entrada de origen: La palabra <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> es una entrada de diccionario.

Salida de destino: Das Wort "wordomatic" ist ein Wörterbucheintrag.

Esta característica funciona del mismo modo con y sin el modo HTML.

La característica debe usarse con moderación. La mejor forma de personalización de traducción, y más adecuada, es mediante el uso de Custom Translator. Custom Translator hace un uso completo del contexto y las probabilidades estadísticas. Si tiene o puede crear datos de aprendizaje que muestren el trabajo o la frase en contexto, obtendrá resultados mucho mejores. Para más información sobre el traductor personalizado, consulte [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).
