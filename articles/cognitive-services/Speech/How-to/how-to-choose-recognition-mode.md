---
title: Elección del modo de reconocimiento de Bing Speech | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Cómo elegir el mejor modo de reconocimiento en Bing Speech.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 99e69691d9904285ecff356523813b426c98fca2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515019"
---
# <a name="bing-speech-recognition-modes"></a>Modos de reconocimiento de voz de Bing Speech

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Las instancias de Bing Speech to Text API admiten varios modos de reconocimiento de voz. Elija el modo que produzca los mejores resultados de reconocimiento para su aplicación.

| Mode | DESCRIPCIÓN |
|---|---|
| *interactive* | Reconocimiento de "comando y control" para escenarios de aplicaciones de usuario interactivas. Los usuarios dicen frases cortas como comandos para una aplicación. |
| *dictation* | Reconocimiento continuo para escenarios de dictado. Los usuarios dicen frases más largas que se muestran como texto. Los usuarios adoptan un estilo de habla más formal. |
| *conversation* | Reconocimiento continuo para transcribir conversaciones entre humanos. Los usuarios adoptan un estilo de habla menos formal y pueden alternar entre oraciones más largas y frases más cortas.

> [!NOTE]
> Estos modos son aplicables cuando se usan las [API REST](../GetStarted/GetStartedREST.md). Las [bibliotecas cliente](../GetStarted/GetStartedClientLibraries.md) usan parámetros diferentes para especificar el modo de reconocimiento. Para más información, consulte la biblioteca cliente de su elección.

Para más información, consulte la página [Modos de reconocimiento](../concepts.md#recognition-modes).
