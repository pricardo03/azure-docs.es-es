---
title: Diccionario dinámico de Microsoft Translator Text API | Microsoft Docs
description: Describe cómo usar la característica de diccionario dinámico de Microsoft Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a18348c9786669ac41c4e149577d97cd631d5531
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382874"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-microsoft-translator-text-api"></a>Descripción del uso de la característica de diccionario dinámico de Microsoft Translator Text API

Si ya conoce la traducción que quiere aplicar a una palabra o frase, puede proporcionarla como marcado dentro de la solicitud. El diccionario dinámico solo es seguro para los nombres compuestos, como nombres propios y nombres de productos. 

**Sintaxis:** 

<mstrans:dictionary translation="traducción de la frase">phrase</mstrans:dictionary>

**Ejemplo: en-de:**

Entrada de origen: The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry (La palabra "wordomatic" es una entrada del diccionario).

Salida de destino: Das Wort "wordomatic" ist ein Wörterbucheintrag.

Esta característica funciona del mismo modo con y sin el modo HTML. 

La característica debe usarse con moderación. La mejor forma de personalización de traducción, y más adecuada, es mediante el uso de Microsoft Translator Hub. Hub hace un uso completo del contexto y las probabilidades estadísticas. Si tiene o puede permitirse crear datos de entrenamiento que muestren el trabajo o una frase en contexto, obtendrá resultados mucho mejores. Puede obtener más información sobre Hub en [http://hub.microsofttranslator.com](http://hub.microsofttranslator.com).

