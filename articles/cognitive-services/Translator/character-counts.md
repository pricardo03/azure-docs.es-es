---
title: Recuento de caracteres de Microsoft Translator Text API | Microsoft Docs
description: Cómo hace Microsoft Translator Text API para contar caracteres.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: 1b4987509c17e4064d7c54608395e272efa8de3b
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41929760"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Cómo hace Microsoft Translator Text API para contar caracteres

Microsoft Translator cuenta cada carácter de la entrada. Caracteres en el sentido de Unicode, no en bytes. Los suplentes Unicode cuentan como dos caracteres. Los espacios en blanco y el marcado cuentan como caracteres. La longitud de la respuesta no importa.

Las llamadas a los métodos Detect y BreakSentence no se cuentan en el consumo de caracteres. Sin embargo, se espera que las llamadas a los métodos Detect y BreakSentence tengan una proporción razonable frente al uso de otras funciones que se cuentan. Microsoft se reserva el derecho a empezar a contar Detect y BreakSentence. 

Para obtener más información sobre los recuentos de caracteres, consulte las [preguntas más frecuentes sobre Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
