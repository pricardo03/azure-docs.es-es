---
title: ¿Qué es Azure Content Moderator?
titlesuffix: Azure Cognitive Services
description: Aprenda a usar Content Moderator para realizar el seguimiento, marcar, evaluar y filtrar material inapropiado en el contenido creado por los usuarios.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: overview
ms.date: 10/05/2018
ms.author: sajagtap
ms.openlocfilehash: 5756e8fb451b073c68271359848ab27373ad85ed
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309559"
---
# <a name="what-is-content-moderator"></a>¿Qué es Content Moderator?

La moderación de contenido es el proceso de supervisión de texto, imágenes o contenido de vídeo en busca de material que pueda no ser deseado o que sea ofensivo o de riesgo. El contenido marcado puede ocultarse o abordarse de otra manera para cumplir las normativas o mantener un entorno adecuado para los usuarios.

## <a name="where-it-is-used"></a>Dónde se usa

En la siguiente lista se muestran algunos casos de ejemplo donde se puede usar Content Moderator:

- Mercados en línea que moderan catálogos de productos y contenido que hayan creado los usuarios.
- Empresas de juegos que moderan los artefactos del juego que crean los usuarios y las salas de chat.
- Plataformas de mensajería de las redes sociales que moderan las imágenes, el texto y los vídeos que agregan los usuarios.
- Empresas multimedia que implementan la moderación de contenido centralizada en a su propio contenido.
- Proveedores de soluciones educativas de tipo K-12 que filtran el contenido inapropiado y ofensivo para los estudiantes y educadores.

## <a name="what-it-includes"></a>Qué incluye

Content Moderator consta de varias API de servicios web y una herramienta integrada de revisión administrada por personas, que ayuda a moderar imágenes, texto y vídeos.

![Diagrama de bloques de Content Moderator](images/content-moderator-block-diagram.png)

### <a name="apis"></a>API existentes

Content Moderator incluye las siguientes API:
  - [**API de moderación de texto**](text-moderation-api.md): use esta API para analizar el texto en busca de posibles insultos, contenido explícito, sugestivo, ofensivo e información de identificación personal (PII).
  - [**API de lista de términos personalizada**](try-terms-list-api.md): use esta API para hacer coincidir las listas de términos personalizadas con los términos integrados. Use estas listas para bloquear o permitir contenido según sus directivas de contenido.  
  - [**API de moderación de imágenes**](image-moderation-api.md): use esta API para analizar imágenes en busca de contenido para adultos o inapropiado, detectar texto en imágenes con la funcionalidad de reconocimiento óptico de caracteres (OCR) y detectar rostros.
  - [**API de lista de imágenes personalizada**](try-image-list-api.md): use esta API para hacer coincidir las listas personalizadas de imágenes con el contenido previamente identificado que no necesite clasificar de nuevo.
  - [**API de moderación de vídeos**](video-moderation-api.md): use esta API para analizar vídeos en busca de contenido adulto e inapropiado.
  - [ **API de revisión**](try-review-api-job.md): use las operaciones [Trabajos](try-review-api-job.md), [Revisiones](try-review-api-review.md) y [Flujo de trabajo](try-review-api-workflow.md) para crear y automatizar los flujos de trabajo administrados por personas, dentro de la herramienta de revisión.

### <a name="human-review-tool"></a>Herramienta de revisión humana

Su suscripción a Content Moderator incluye la [herramienta de revisión humana](Review-Tool-User-Guide/human-in-the-loop.md) integrada. Use la API de revisión que se mencionó anteriormente para crear revisiones de texto, imágenes y vídeos, para que sus moderadores puedan tomar las decisiones finales.

![Herramienta de revisión de vídeos de Content Moderator](images/video-review-default-view.png)

## <a name="next-steps"></a>Pasos siguientes

Use la guía de [inicio rápido](quick-start.md) para empezar a usar Content Moderator.
