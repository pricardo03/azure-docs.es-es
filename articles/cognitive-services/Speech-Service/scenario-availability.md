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
ms.date: 05/07/2019
ms.author: cbasoglu
ms.openlocfilehash: 94fd415909e86a43916ee2f510732a6a6d9c5ed3
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552950"
---
# <a name="scenario-availability"></a>Disponibilidad de escenarios

El SDK del servicio Voz presenta muchos escenarios en una amplia variedad de lenguajes y entornos de programación.  No todos los escenarios están actualmente disponibles en todos los lenguajes de programación o todos los entornos.  A continuación se indica la disponibilidad de cada escenario.

- **Reconocimiento de voz (SR), lista de frases, intención, traducción y contenedores locales**
  - Todos los lenguajes de programación y entornos donde haya un vínculo de flecha <img src="media/index/link.jpg" height="15" width="15"></img> en [esta](https://aka.ms/csspeech) tabla de inicio rápido.
- **Texto a voz (TTS)**
  - C++/Windows y Linux
  - C#/Windows, UWP y Unity
  - La API REST de TTS puede usarse en todas las demás situaciones.
- **Palabra de reactivación (observador de palabras clave/KWS)**
  - C++/Windows y Linux
  - C#/Windows & Linux
  - Python/Windows y Linux
  - Java/Windows, Linux y Android (SDK de dispositivos de voz)
  - La funcionalidad de palabra de reactivación (observador de palabras clave/KWS) podría funcionar con cualquier tipo de micrófono; no obstante, la compatibilidad oficial de KWS está limitada actualmente a las matrices de micrófonos que se encuentran en el hardware de Azure Kinect DK o el SDK de dispositivos de voz.
- **Asistente virtual por voz**
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
  - Java/Linux y Android
