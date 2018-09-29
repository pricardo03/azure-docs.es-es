---
title: ¿Qué es Content Moderator?
titlesuffix: Azure Cognitive Services
description: Aprenda a usar Content Moderator para realizar el seguimiento, marcar, evaluar y filtrar contenido inapropiado del contenido que cree el usuario.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: overview
ms.date: 06/15/2017
ms.author: sajagtap
ms.openlocfilehash: e109376f47d921fb18d7bb9a6252e80315419ec0
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226048"
---
# <a name="what-is-content-moderator"></a>¿Qué es Content Moderator?

La moderación de contenido es el proceso de supervisión de posibles contenidos ofensivos, indeseables y peligrosos. El contenido moderado puede estar compuesto de imágenes, texto y vídeos.

## <a name="where-it-is-used"></a>Dónde se usa

En la lista siguiente se muestran algunos casos de ejemplo donde se usa Content Moderator:

- Mercados en línea que moderan catálogos de productos y contenido que haya creado el usuario.
- Empresas de juegos moderan los artefactos del juego que crea el usuario y las salas de chat.
- Plataformas de mensajería de las redes sociales que moderan las imágenes, el texto y los vídeos que agregan los usuarios.
- Empresas multimedia que implementan la moderación de contenido centralizada en a su propio contenido.
- Proveedores de soluciones educativas de tipo K-12 que filtran el contenido malo y ofensivo para los estudiantes y educadores.

## <a name="what-it-includes"></a>Qué incluye

Content Moderator consta de varias API de servicios web y una herramienta integrada de revisión administrada por personas, que ayuda a moderar imágenes, texto y vídeos.

![Diagrama de bloques de Content Moderator](images/content-moderator-block-diagram.png)

## <a name="apis"></a>API existentes

Content Moderator incluye las siguientes API:
  - [**API de moderación de texto**](text-moderation-api.md): use esta API para analizar el texto en busca de posibles insultos, contenido explícito, sugestivo, ofensivo e información de identificación personal (PII).
  - [**API de lista de términos personalizada**](try-terms-list-api.md): use esta API para hacer coincidir las listas de términos personalizadas con los términos integrados. Use estas listas para bloquear o permitir contenido según sus directivas de contenido.  
  - [**API de moderación de imágenes**](image-moderation-api.md): use esta API para analizar imágenes en busca de contenido para adultos o inapropiado, detectar texto en imágenes con la funcionalidad de reconocimiento óptico de caracteres (OCR) y detectar rostros.
  - [**API de lista de imágenes personalizada**](try-image-list-api.md): use esta API para hacer coincidir las listas personalizadas de imágenes con el contenido previamente identificado que no necesite clasificar de nuevo.
  - [**API de moderación de vídeos**](video-moderation-api.md): use esta API para analizar vídeos en busca de contenido adulto e inapropiado.
  - [ **API de revisión**](try-review-api-job.md): use las operaciones [Trabajos](try-review-api-job.md), [Revisiones](try-review-api-review.md) y [Flujo de trabajo](try-review-api-workflow.md) para crear y automatizar los flujos de trabajo administrados por personas, dentro de la herramienta de revisión.

## <a name="human-review-tool"></a>Herramienta de revisión humana

Su suscripción a Content Moderator incluye la [herramienta de revisión humana](Review-Tool-User-Guide/human-in-the-loop.md) integrada. Use la API de revisión que se mencionó anteriormente para crear revisiones de texto, imágenes y vídeos, para que sus moderadores puedan tomar las decisiones finales.

![Herramienta de revisión de vídeos de Content Moderator](images/video-review-default-view.png)

## <a name="next-steps"></a>Pasos siguientes

Use la guía de [inicio rápido](quick-start.md) para empezar a usar Content Moderator.
