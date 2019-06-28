---
title: 'Disponibilidad de escenarios: servicios de Voz'
titlesuffix: Azure Cognitive Services
description: Referencia de las regiones del servicio Voz.
services: cognitive-services
author: chrisbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cbasoglu
ms.openlocfilehash: d844b171ff99dc97e5d1107bcb745f9e8d5b3e9d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65519824"
---
# <a name="scenario-availability"></a>Disponibilidad de escenarios

El SDK del servicio Voz presenta muchos escenarios en una amplia variedad de lenguajes y entornos de programación.  No todos los escenarios están actualmente disponibles en todos los lenguajes de programación o todos los entornos.  A continuación se indica la disponibilidad de cada escenario.

- **Reconocimiento de voz (SR), lista de frases, intención, traducción y contenedores locales**
  - Todos los lenguajes de programación y entornos donde haya un vínculo de flecha <img src="media/index/link.jpg" height="15" width="15"></img> en [esta](https://aka.ms/csspeech) tabla de inicio rápido.
- **Texto a voz (TTS)**
  - C++/Windows y Linux
  - C#/Windows
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
