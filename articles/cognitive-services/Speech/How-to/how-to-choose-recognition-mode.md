---
title: Elección del modo de reconocimiento de Bing Speech | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Cómo elegir el mejor modo de reconocimiento en Bing Speech.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX
ms.openlocfilehash: a39b357a26823e322d4e902f2d99b67488bbf2df
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950886"
---
# <a name="bing-speech-recognition-modes"></a>Modos de reconocimiento de voz de Bing Speech

Las instancias de Bing Speech to Text API admiten varios modos de reconocimiento de voz. Elija el modo que produzca los mejores resultados de reconocimiento para su aplicación.

| Mode | DESCRIPCIÓN |
|---|---|
| *interactive* | Reconocimiento de "comando y control" para escenarios de aplicaciones de usuario interactivas. Los usuarios dicen frases cortas como comandos para una aplicación. |
| *dictation* | Reconocimiento continuo para escenarios de dictado. Los usuarios dicen frases más largas que se muestran como texto. Los usuarios adoptan un estilo de habla más formal. |
| *conversation* | Reconocimiento continuo para transcribir conversaciones entre humanos. Los usuarios adoptan un estilo de habla menos formal y pueden alternar entre oraciones más largas y frases más cortas.

> [!NOTE]
> Estos modos son aplicables cuando se usan las [API REST](../GetStarted/GetStartedREST.md). Las [bibliotecas cliente](../GetStarted/GetStartedClientLibraries.md) usan parámetros diferentes para especificar el modo de reconocimiento. Para más información, consulte la biblioteca cliente de su elección.

Para más información, consulte la página [Modos de reconocimiento](../concepts.md#recognition-modes).
