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
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: 97b0b6256b7aaf7b42565fe9453fb87a0c414569
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57861577"
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

Translator Text API tiene una latencia máxima de 15 segundos con los modelos estándar. Conversión mediante modelos personalizados tiene una latencia máxima de 25 segundos. Llegados a este punto, habrá recibido un resultado o una respuesta de tiempo de espera agotado. Normalmente, las respuestas tardan entre 150 milisegundos y 300 milisegundos en devolverse. Tiempos de respuesta variará en función del tamaño del par de solicitud y de idioma. Si no recibe una traducción o un [respuesta de error](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) en ese período de tiempo, debe comprobar la conexión de red y vuelva a intentar.

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
