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
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: f7fef00cfff9295036d7545470f86e27314e6451
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258705"
---
# <a name="what-is-azure-content-moderator"></a>¿Qué es Azure Content Moderator?

Azure Content Moderator API es un servicio Cognitive que comprueba el contenido de texto, imágenes y vídeo de material que es potencialmente ofensivo, arriesgado o no deseable por algún otro motivo. Cuando se encuentra dicho material, el servicio aplica las etiquetas adecuadas (marcas) al contenido. Luego, la aplicación puede controlar el contenido marcado para cumplir las normativas o mantener el entorno adecuado para los usuarios. Consulte la sección [API de Content Moderator](#content-moderator-apis) para más información acerca de lo que indican las diferentes marcas de contenido.

## <a name="where-it-is-used"></a>Dónde se usa

Los siguientes son algunos escenarios en los que un desarrollador de software o un equipo de desarrolladores usaría Content Moderator:

- Instancias de Marketplace en línea que moderan catálogos de productos y otro contenido generado por el usuario.
- Empresas de juegos que moderan los artefactos de juego generados por el usuario y las salas de chat.
- Plataformas de mensajería de las redes sociales que moderan las imágenes, el texto y los vídeos que agregan los usuarios.
- Empresas multimedia que implementan la moderación de centralizada de su contenido.
- Proveedores de soluciones educativas de tipo K-12 que filtran contenido que no es apropiado para alumnos y educadores.

## <a name="what-it-includes"></a>Qué incluye

El servicio Content Moderator consta de varias API de servicio de web disponibles mediante llamadas REST y un SDK de .NET. También incluye la herramienta de revisión humana, que permite a los revisores humanos ayudar el servicio y mejorar o ajustar su función de moderación.

![diagrama de bloques de Content Moderator que muestra las API de moderación, las API de revisión y la herramienta de revisión humana](images/content-moderator-block-diagram.png)

### <a name="content-moderator-apis"></a>API de Content Moderator

El servicio Content Moderator incluye API para los escenarios siguientes.

| . | DESCRIPCIÓN |
| ------ | ----------- |
|[**Moderación de texto**](text-moderation-api.md)| Analiza si el texto contiene contenido ofensivo, sexualmente explícito o sugerente, blasfemias e información personal identificable (PII).|
|[**Listas de términos personalizada**](try-terms-list-api.md)| Analiza el texto con respecto a una lista personalizada de términos, además de los términos integrados. Use listas personalizadas para bloquear o permitir el contenido en función de sus propias directivas de contenido.|  
|[**Moderación de imágenes**](image-moderation-api.md)| Analiza imágenes en busca de contenido para adultos o inapropiado, detecta texto en imágenes con la funcionalidad de reconocimiento óptico de caracteres (OCR) y detecta rostros.|
|[**Listas de imágenes personalizadas**](try-image-list-api.md)| Analiza imágenes con una lista personalizada de imágenes. Use las listas de imágenes personalizadas para filtrar el contenido comúnmente recurrente que no desea volver a clasificar.|
|[**Moderación en vídeos**](video-moderation-api.md)| Examina si los vídeos tienen contenido para adultos o inapropiado y devuelve los marcadores de tiempo de dicho contenido.|
|[**Revisión**](try-review-api-job.md)| Use las operaciones de [Jobs](try-review-api-job.md), [Reviews](try-review-api-review.md) y [Workflow](try-review-api-workflow.md) para crear y automatizar los flujos de trabajo administrados por personas con la herramienta de revisión humana. La API de flujo de trabajo aún no está disponible mediante el SDK de .NET.|

### <a name="human-review-tool"></a>Herramienta de revisión humana

El servicio Content Moderator también incluye [herramienta de revisión humana](Review-Tool-User-Guide/human-in-the-loop.md) basada en web. 

![Página principal de la herramienta de revisión humana de Content Moderator](images/homepage.PNG)

Las API de revisión se pueden usar para configurar las revisiones por parte del equipo del contenido de texto, imagen y vídeo, según los filtros que especifique. A continuación, los moderadores humanos pueden tomar las decisiones finales con respecto a la moderación. La entrada humana no entrena el servicio, pero el trabajo combinado del servicio y los equipos de revisión humana permite a los desarrolladores lograr un equilibrio adecuado entre eficacia y precisión.

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de los datos
Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Content Moderator deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Comience a usar el servicio Content Moderator siguiendo las instrucciones de [Try Content Moderator on the web](quick-start.md) (Probar Content Moderator en la web).
