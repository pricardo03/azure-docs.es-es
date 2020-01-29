---
title: 'Nubes soberanas: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a usar las nubes soberanas.
services: cognitive-services
author: cbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 1/14/2020
ms.author: cbasoglu
ms.openlocfilehash: 3b96ded52aa2148df9e96d6f1e878d50b821abab
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170458"
---
# <a name="speech-services-with-sovereign-clouds"></a>Servicios de voz con nubes soberanas

## <a name="azure-government-united-states"></a>Azure Government (Estados Unidos)

Solo los gobiernos federales, estatales, locales y tribales y sus asociados tienen acceso a esta instancia dedicada con operaciones controladas por ciudadanos autorizados de EE. UU.
- Regiones: Gobierno de EE. UU. - Virginia
- SR en SpeechSDK:*config.FromHost("wss://virginia.stt.speech.azure.us", "\<su-clave\>");*
- TTS en SpeechSDK: *config.FromHost("https[]()://virginia.tts.speech.azure.us", "\<su-clave\>");*
- Tokens de autenticación: https[]()://virginia.api.cognitive.microsoft.us/sts/v1.0/issueToken
- Azure Portal: https://portal.azure.us  
- Portal de Custom Speech: https://virginia.cris.azure.us/Home/CustomSpeech
- SKU disponibles: S0
- Características admitidas:
  - Voz a texto
  - Custom Speech (adaptación acústica/idioma)
  - Text-to-Speech
  - Translator Speech
- Características no admitidas
  - Voz personalizada
  - Voces neuronales para la conversión de texto a voz
- Configuraciones regionales admitidas: Se admiten las configuraciones regionales de los siguientes idiomas.
  - Árabe (ar-*)
  - Chino (zh-*)
  - Inglés (en-*)
  - Francés (fr-*)
  - Alemán (de-*)
  - Hindi
  - Coreano
  - Ruso
  - Español (es-*)

## <a name="microsoft-azure-china"></a>Microsoft Azure China

Se ubica en China, un centro de datos de Azure con acceso directo a China Mobile, China Telecom, China Unicom y a otras redes troncales de los principales operadores, para proporcionar a los usuarios chinos una experiencia de acceso estable y de alta velocidad a la red local.
- Regiones: Este de China 2 (Shanghái)
- SR en SpeechSDK: *config.FromHost("wss://chinaeast2.stt.speech.azure.cn", "\<su-clave\>");*
- TTS en SpeechSDK:  *config.FromHost("https[]()://chinaeast2.tts.speech.azure.cn", "\<su-clave\>");*
- Tokens de autenticación: https[]()://chinaeast2.api.cognitive.microsoft.cn/sts/v1.0/issueToken
- Azure Portal: https://portal.azure.cn
- Portal de Custom Speech: https://chinaeast2.cris.azure.cn/Home/CustomSpeech
- SKU disponibles: S0
- Características admitidas:
  - Voz a texto
  - Custom Speech (adaptación acústica/idioma)
  - Text-to-Speech
  - Translator Speech
- Características no admitidas
  - Voz personalizada
  - Voces neuronales para la conversión de texto a voz
- Configuraciones regionales admitidas: Se admiten las configuraciones regionales de los siguientes idiomas.
  - Árabe (ar-*)
  - Chino (zh-*)
  - Inglés (en-*)
  - Francés (fr-*)
  - Alemán (de-*)
  - Hindi
  - Coreano
  - Ruso
  - Español (es-*)

