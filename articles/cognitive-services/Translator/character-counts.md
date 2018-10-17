---
title: Recuentos de caracteres de Translator Text API
titlesuffix: Azure Cognitive Services
description: Cómo hace Translator Text API para contar caracteres.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/20/2017
ms.author: v-jansko
ms.openlocfilehash: c6234a46ae55d73739dcc23110c5e0f6375c3f96
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128747"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Cómo hace Translator Text API para contar caracteres

Translator Text API cuenta cada carácter de la entrada. Caracteres en el sentido de Unicode, no en bytes. Los suplentes Unicode cuentan como dos caracteres. Los espacios en blanco y el marcado cuentan como caracteres. La longitud de la respuesta no importa.

Las llamadas a los métodos Detect y BreakSentence no se cuentan en el consumo de caracteres. Sin embargo, se espera que las llamadas a los métodos Detect y BreakSentence tengan una proporción razonable frente al uso de otras funciones que se cuentan. Microsoft se reserva el derecho a empezar a contar Detect y BreakSentence. 

Para obtener más información sobre los recuentos de caracteres, consulte las [preguntas más frecuentes sobre Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
