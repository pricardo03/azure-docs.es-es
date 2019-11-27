---
title: Diccionario dinámico de Translator Text API
titleSuffix: Azure Cognitive Services
description: En este artículo se explica cómo usar la característica de diccionario dinámico de la Translator Text API de Azure Cognitive Services.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 5f007660b98a22ff9ffad072991a39cf59518b6a
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73837351"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Cómo usar un diccionario dinámico

Si ya conoce la traducción que quiere aplicar a una palabra o frase, puede proporcionarla como marcado dentro de la solicitud. El diccionario dinámico solo es seguro para los nombres compuestos, como nombres propios y nombres de productos.

**Sintaxis:**

<mstrans:dictionary translation="traducción de la frase">phrase</mstrans:dictionary>

**Requisitos:**

* Los idiomas `From` y `To` deben ser diferentes. 
* Debe incluir el parámetro `From` en la solicitud de traducción de API en lugar de usar la característica de detección automática. 

**Ejemplo: en-de:**

Entrada de origen: La palabra <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> es una entrada de diccionario.

Salida de destino: Das Wort "wordomatic" ist ein Wörterbucheintrag.

Esta característica funciona del mismo modo con y sin el modo HTML.

La característica debe usarse con moderación. La mejor forma de personalización de traducción, y más adecuada, es mediante el uso de Custom Translator. Custom Translator hace un uso completo del contexto y las probabilidades estadísticas. Si tiene o puede crear datos de aprendizaje que muestren el trabajo o la frase en contexto, obtendrá resultados mucho mejores. Para más información sobre el traductor personalizado, consulte [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).
