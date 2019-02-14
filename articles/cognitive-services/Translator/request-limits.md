---
title: 'Límites de solicitudes: Translator Text API'
titleSuffix: Azure Cognitive Services
description: En este artículo se enumeran los límites de solicitudes de Translator Text API. Los cargos se generan en función del número de caracteres, y no por la frecuencia de solicitud, con un límite de 5000 caracteres por solicitud. Los límites de caracteres se basan en la suscripción, con F0 limitado a 2 millones de caracteres por hora.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: erhopf
ms.openlocfilehash: f89e7e674efe3a823b7c969840772565650d8d07
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859477"
---
# <a name="request-limits-for-translator-text"></a>Límites de solicitudes de Translator Text

En este artículo se proporcionan los valores de limitación de Translator Text API. Los servicios incluyen traducción, transliteración, detección de la longitud de oraciones, detección de idiomas y traducciones alternativas.

## <a name="character-limits-per-request"></a>Límites de caracteres por solicitud

Cada solicitud está limitada a 5000 caracteres. Se le cobrará por cada carácter, no por el número de solicitudes. Se recomienda enviar solicitudes más cortas, para tener alguna solicitud pendiente en un momento dado.

No hay ningún límite en el número de solicitudes pendientes a Translator Text API.

## <a name="character-limits-per-hour"></a>Límites de caracteres por hora

El límite de caracteres por hora se basa en el nivel de suscripción de Translator Text. Si se alcanzan o se superan estos límites, probablemente recibirá una respuesta para informarle de que ha agotado la cuota:

| Nivel | Límite de caracteres |
|------|-----------------|
| F0 | 2 millones de caracteres por hora |
| S1 | 40 millones de caracteres por hora |
| S2 | 40 millones de caracteres por hora |
| S3 | 120 millones de caracteres por hora |
| S4 | 200 millones de caracteres por hora |

Estos límites están restringidos a sistemas genéricos de Microsoft. Los sistemas de traducción personalizada que usan Microsoft Translator Hub están limitados a 1800 caracteres por segundo.

## <a name="latency"></a>Latencia

Translator Text tiene una latencia máxima de 13 segundos. Llegados a este punto, habrá recibido un resultado o una respuesta de tiempo de espera agotado. Normalmente, las respuestas tardan entre 150 milisegundos y 300 milisegundos en devolverse. Los tiempos de respuesta variarán según el tamaño de la solicitud o el par de idioma.

## <a name="sentence-length-limits"></a>Límites de longitud de oraciones

Cuando se usa la función [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence), la longitud de la oración se limita a 275 caracteres. Existen excepciones para estos idiomas:

| Idioma | Código | Límite de caracteres |
|----------|------|-----------------|
| Chino | zh | 132 |
| Alemán | de | 290 |
| Italiano | it | 280 |
| Japonés | ja | 150 |
| Portugués | pt | 290 |
| Español | es | 280 |
| Italiano | it | 280 |
| Tailandés | th | 258 |

> [!NOTE]
> Este límite no se aplica a las traducciones.

## <a name="next-steps"></a>Pasos siguientes

* [Precios](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Disponibilidad regional](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [Referencia de Translator Text API v3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
