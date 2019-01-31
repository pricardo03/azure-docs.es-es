---
title: Recuentos de caracteres de Translator Text API
titlesuffix: Azure Cognitive Services
description: Cómo hace Translator Text API para contar caracteres.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: c56622ee5e25fc422d02cec6ecfaa1f847e9e769
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226441"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Cómo hace Translator Text API para contar caracteres

Translator Text API cuenta cada carácter de la entrada. Caracteres en el sentido de Unicode, no en bytes. Los suplentes Unicode cuentan como dos caracteres. Los espacios en blanco y el marcado cuentan como caracteres. La longitud de la respuesta no importa.

Las llamadas a los métodos Detect y BreakSentence no se cuentan en el consumo de caracteres. Sin embargo, se espera que las llamadas a los métodos Detect y BreakSentence tengan una proporción razonable frente al uso de otras funciones que se cuentan. Microsoft se reserva el derecho a empezar a contar Detect y BreakSentence.

Para obtener más información sobre los recuentos de caracteres, consulte las [preguntas más frecuentes sobre Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
