---
title: 'Disponibilidad de escenarios: Speech Service'
titleSuffix: Azure Cognitive Services
description: Referencia de las regiones del servicio Voz.
services: cognitive-services
author: chrisbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: cbasoglu
ms.openlocfilehash: 6ec31df7cef8391728eae7845f64f55bb1c6466a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491342"
---
# <a name="scenario-availability"></a>Disponibilidad de escenarios

El SDK del servicio Voz presenta muchos escenarios en una amplia variedad de lenguajes y entornos de programación.  No todos los escenarios están actualmente disponibles en todos los lenguajes de programación o todos los entornos.  A continuación se indica la disponibilidad de cada escenario.

- **Reconocimiento de voz (SR), lista de frases, intención, traducción y contenedores locales**
  - Todos los lenguajes de programación y entornos donde haya un vínculo de flecha <img src="media/index/link.jpg" height="15" width="15"></img> en [esta](https://aka.ms/csspeech) tabla de inicio rápido.
- **Texto a voz (TTS)**
  - C++/Windows y Linux
  - C#/Windows, UWP y Unity
  - Java (JRE y Android)
  - Python
  - Swift
  - Objective-C
  - La API REST de TTS puede usarse en todas las demás situaciones.
- **Detección de palabras clave (KWS)**
  - C++/Windows y Linux
  - C#/Windows & Linux
  - Python/Windows y Linux
  - Java/Windows, Linux y Android (SDK de dispositivos de voz)
  - La funcionalidad de detección de palabras clave (KWS) podría funcionar con cualquier tipo de micrófono; no obstante, la compatibilidad oficial de KWS está limitada actualmente a las matrices de micrófonos que se encuentran en el hardware de Azure Kinect DK o el SDK de dispositivos de voz.
- **Asistentes de voz**
  - C++/Windows, Linux y macOS
  - C#/Windows
  - Java/Windows, Linux, macOS y Android (SDK de dispositivos de voz)
- **Transcripción de conversaciones**
  - C++/Windows y Linux
  - C# (Framework y .NET Core)/Windows, UWP y Linux
  - Java/Windows, Linux y Android (SDK de dispositivos de voz)
- **Transcripción para centros de llamadas**
  - La API REST se puede usar en cualquier situación.
- **Entrada de audio comprimido con códec**
  - C++/Linux
  - C#/Linux
  - Java/Linux, Android e iOS
