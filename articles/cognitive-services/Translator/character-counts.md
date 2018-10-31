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
ms.openlocfilehash: 8e04158d34765b8a077fc56f2108ea6d7b4b03a6
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649201"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Cómo hace Translator Text API para contar caracteres

Translator Text API cuenta cada carácter de la entrada. Caracteres en el sentido de Unicode, no en bytes. Los suplentes Unicode cuentan como dos caracteres. Los espacios en blanco y el marcado cuentan como caracteres. La longitud de la respuesta no importa.

Las llamadas a los métodos Detect y BreakSentence no se cuentan en el consumo de caracteres. Sin embargo, se espera que las llamadas a los métodos Detect y BreakSentence tengan una proporción razonable frente al uso de otras funciones que se cuentan. Microsoft se reserva el derecho a empezar a contar Detect y BreakSentence.

Para obtener más información sobre los recuentos de caracteres, consulte las [preguntas más frecuentes sobre Microsoft Translator](https://www.microsoft.com/en-us/translator/faq.aspx).
