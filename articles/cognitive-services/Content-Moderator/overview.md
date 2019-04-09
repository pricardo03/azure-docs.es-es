---
title: ¿Qué es Azure Content Moderator?
titlesuffix: Azure Cognitive Services
description: Aprenda a usar Content Moderator para realizar el seguimiento, marcar, evaluar y filtrar material inapropiado en el contenido creado por los usuarios.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: 440471acb6e122bf25ba21b0ab3b5a2f7d9b021d
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758139"
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

| Grupo de API | DESCRIPCIÓN |
| ------ | ----------- |
|[**Moderación de texto**](text-moderation-api.md)| Examina si el texto contiene contenido ofensivo, sexualmente explícito o sugerente, blasfemias y datos personales.|
|[**Listas de términos personalizada**](try-terms-list-api.md)| Analiza el texto con respecto a una lista personalizada de términos, además de los términos integrados. Use listas personalizadas para bloquear o permitir el contenido en función de sus propias directivas de contenido.|  
|[**Moderación de imágenes**](image-moderation-api.md)| Analiza imágenes en busca de contenido para adultos o inapropiado, detecta texto en imágenes con la funcionalidad de reconocimiento óptico de caracteres (OCR) y detecta rostros.|
|[**Listas de imágenes personalizadas**](try-image-list-api.md)| Analiza imágenes con una lista personalizada de imágenes. Use las listas de imágenes personalizadas para filtrar el contenido comúnmente recurrente que no desea volver a clasificar.|
|[**Moderación en vídeos**](video-moderation-api.md)| Examina si los vídeos tienen contenido para adultos o inapropiado y devuelve los marcadores de tiempo de dicho contenido.|
|[**Revisión de las API**](try-review-api-job.md)| Use las operaciones de [Jobs](try-review-api-job.md), [Reviews](try-review-api-review.md) y [Workflow](try-review-api-workflow.md) para crear y automatizar los flujos de trabajo administrados por personas con la herramienta de revisión humana. La API de flujo de trabajo aún no está disponible mediante .NET SDK.|

### <a name="review-tool"></a>Herramienta de revisión

El servicio Content Moderator también incluye la [herramienta de revisión](Review-Tool-User-Guide/human-in-the-loop.md) basada en web, que hospeda las revisiones del contenido para su procesamiento por parte de moderadores humanos. La entrada humana no entrena el servicio, pero el trabajo combinado del servicio y los equipos de revisión humana permite a los desarrolladores lograr un equilibrio adecuado entre eficacia y precisión. La herramienta de revisión también proporciona un servidor front-end sencillo para diversos recursos de Content Moderator.

![Página principal de la herramienta de revisión humana de Content Moderator](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de los datos

Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan el servicio Content Moderator deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Comience a usar el servicio Content Moderator siguiendo las instrucciones de [Try Content Moderator on the web](quick-start.md) (Probar Content Moderator en la web).