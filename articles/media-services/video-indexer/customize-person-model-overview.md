---
title: 'Personalización de un modelo de persona en Video Indexer: Azure'
titlesuffix: Azure Media Services
description: En este artículo se proporciona información general sobre qué es un modelo de persona en Video Indexer y cómo personalizarlo.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/05/2018
ms.author: anzaman
ms.openlocfilehash: 073cff22f17f496c2ff85cfbf716751dfea1e03e
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53283241"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Personalización de un modelo de persona en Video Indexer


Video Indexer admite la detección de caras y el reconocimiento de celebridades en contenido de vídeo. La característica de reconocimiento de celebridades abarca aproximadamente 1 000 000 de caras basadas en orígenes de datos comúnmente solicitados, como IMDB, Wikipedia y personas con más influencia de LinkedIn. Las caras que no reconoce la característica de reconocimiento de celebridades se detectan, pero se dejan sin nombre. Después de cargar un vídeo en Video Indexer y recibir los resultados, puede volver y poner nombre a las caras que no se reconocieron. Cuando se etiqueta una cara con un nombre, la cara y el nombre se agregan al modelo de persona de la cuenta. Así, Video Indexer reconocerá esta cara en los vídeos futuros y pasados.

Puede usar el sitio web o la API de Video Indexer para editar las caras que se detectaron en un vídeo de su cuenta, como se describe en los siguientes temas:

- [Personalización del modelo de persona mediante las API](customize-person-model-with-api.md)
- [Customize Person model using the website](customize-person-model-with-website.md) (Personalización del modelo de persona mediante el sitio web)
