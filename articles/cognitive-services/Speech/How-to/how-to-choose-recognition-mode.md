---
title: Elección del modo de reconocimiento | Microsoft Docs
description: Cómo elegir el mejor modo de reconocimiento.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 4f02b683dde16b537ae5554e6435c068f0fcb808
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380539"
---
# <a name="speech-recognition-modes"></a>Modos de reconocimiento de voz

Las API *Speech to Text* admiten varios modos de reconocimiento de voz. Elija el modo que produzca los mejores resultados de reconocimiento para su aplicación.

| Mode | DESCRIPCIÓN |
|---|---|
| *interactive* | Reconocimiento de "comando y control" para escenarios de aplicaciones de usuario interactivas. Los usuarios dicen frases cortas como comandos para una aplicación. |
| *dictation* | Reconocimiento continuo para escenarios de dictado. Los usuarios dicen frases más largas que se muestran como texto. Los usuarios adoptan un estilo de habla más formal. |
| *conversation* | Reconocimiento continuo para transcribir conversaciones entre humanos. Los usuarios adoptan un estilo de habla menos formal y pueden alternar entre oraciones más largas y frases más cortas.

> [!NOTE]
> Estos modos son aplicables cuando se usan las [API REST](../GetStarted/GetStartedREST.md). Las [bibliotecas cliente](../GetStarted/GetStartedClientLibraries.md) usan parámetros diferentes para especificar el modo de reconocimiento. Para más información, consulte la biblioteca cliente de su elección.

Para más información, consulte la página [Modos de reconocimiento](../concepts.md#recognition-modes).
