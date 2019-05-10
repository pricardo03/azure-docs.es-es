---
title: Disponibilidad de escenario - servicios de voz
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
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65519824"
---
# <a name="scenario-availability"></a>Disponibilidad de escenario

El servicio de Speech SDK presenta muchos escenarios en una amplia variedad de lenguajes y entornos de programación.  No todos los escenarios aún están actualmente disponibles en todos los lenguajes de programación o todos los entornos.  La siguiente es la disponibilidad de cada escenario.

- **Reconocimiento de voz (SR), lista de frases, intención, traducción, locales y en contenedores**
  - Todos los lenguajes de programación y entornos donde hay un vínculo de flecha <img src="media/index/link.jpg" height="15" width="15"></img> en la tabla de la Guía de inicio rápido [aquí](https://aka.ms/csspeech).
- **Text-to-Speech (TTS)**
  - C++/ Windows y Linux
  - C#/Windows
  - API de REST de TTS puede usarse en todas las situaciones.
- **Reactivación de Word (palabra clave observador/KWS)**
  - C++/ Windows y Linux
  - C#/ Windows y Linux
  - Python/Windows y Linux
  - Java/Windows y Linux y Android (dispositivos de Speech SDK)
  - Funcionalidad de Word (palabra clave observador/KWS) de reactivación podría funcionar con cualquier tipo de micrófono, oficial KWS admite, sin embargo, está limitado a las matrices de micrófono se encuentra actualmente en el hardware de Azure Kinect DK o el SDK de dispositivos de voz
- **Asistente de voz en primer lugar Virtual**
  - C++/ Windows Linux y macOS
  - C#/Windows
  - Java/Windows & Linux y macOS y Android (Speech SDK de dispositivos)
- **Transcripción de conversaciones**
  - C++/ Windows y Linux
  - C#(Framework y .NET Core) o Windows, UWP & Linux
  - Java/Windows y Linux y Android (dispositivos de Speech SDK)
- **Transcripción de centro de llamadas**
  - API de REST y pueden usarse en cualquier situación.
- **Entrada de Audio comprimido de códec**
  - C++/Linux
  - C#/Linux
  - Java/Linux y Android
