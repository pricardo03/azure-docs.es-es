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
ms.openlocfilehash: ebe3e3606a0413730e1fbfd704a6403f77275f89
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380814"
---
# <a name="how-the-microsoft-translator-text-api-counts-characters"></a>Cómo hace Microsoft Translator Text API para contar caracteres

Microsoft Translator cuenta cada carácter de la entrada. Caracteres en el sentido de Unicode, no en bytes. Los suplentes Unicode cuentan como dos caracteres. Los espacios en blanco y el marcado cuentan como caracteres. La longitud de la respuesta no importa.

Las llamadas a los métodos Detect y BreakSentence no se cuentan en el consumo de caracteres. Sin embargo, se espera que las llamadas a los métodos Detect y BreakSentence tengan una proporción razonable frente al uso de otras funciones que se cuentan. Microsoft se reserva el derecho a empezar a contar Detect y BreakSentence. 

Translate ofrece la detección automática si se omite el parámetro de idioma From. 

Para obtener más información sobre los recuentos de caracteres, consulte las [preguntas más frecuentes sobre Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
