---
title: 'Preguntas más frecuentes: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Obtenga respuestas para las preguntas más frecuentes acerca del uso de Content Moderator.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: sajagtap
ms.openlocfilehash: df8d957fc2de620d63567a9cc1b14b24b73052bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61268928"
---
# <a name="frequently-asked-questions-faq"></a>Preguntas más frecuentes

### <a name="what-does-my-content-moderator-subscription-include"></a>¿Qué incluye mi suscripción a Content Moderator?

Su suscripción de Content Moderator incluye acceso a la herramienta de revisión y las API. Puede decidir si quiere utilizar una u otra, o ambas, en función de las necesidades empresariales.

### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>¿Cuáles son los límites y restricciones del contenido que se puede moderar mediante la API?

Cuando utilice la API, las imágenes deben tener un mínimo de 128 píxeles y un tamaño máximo de archivo de 4 MB. El texto no puede superar los 1024 caracteres de longitud. No hay ningún límite para la duración del vídeo.

### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>¿Qué ocurre si el contenido pasado a la API de texto o a la API de imagen supera los límites de tamaño?

La API de texto devuelve un código de error que informa que el texto es más largo de lo permitido. La API de imagen también devuelve un código de error que informa que la imagen no cumple los requisitos de tamaño.

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>¿Se conservan las imágenes, el texto o los vídeos que se envían para moderación?

El contenido es de su propiedad y Microsoft no puede retenerlo sin su consentimiento. Microsoft usa medidas de seguridad líderes del sector para ayudar a proteger su contenido.

### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>¿Puedo usar Content Moderator para filtrar imágenes ilegales de explotación infantil?

No. Sin embargo, las organizaciones cualificadas pueden usar [PhotoDNA Cloud Service](https://www.microsoft.com/photodna "Microsoft PhotoDNA Cloud Service") para filtrar este tipo de contenido.

### <a name="how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>¿Cuántos revisión los equipos pueden una combinación de usuario? ¿Puede el usuario cambiar entre equipos?

Un usuario sólo puede unir un equipo a la vez.

### <a name="what-kind-of-team-member-roles-are-supported-by-the-review-tool-how-are-they-different"></a>¿Qué tipo de funciones miembros del equipo son compatibles con la herramienta de revisión? ¿En qué se diferencian?

Las herramientas de revisión actualmente permite la asignación de roles de administrador y revisor. Los administradores pueden invitar a otros usuarios y tener acceso a los valores de configuración, mientras que los revisores solo pueden revisar los resultados de la moderación y etiquetar o quitar etiquetas del contenido.

### <a name="what-is-a-tag-does-the-review-tool-support-custom-tags"></a>¿Qué es una etiqueta? ¿Admite la herramienta de revisión etiquetas personalizadas?

Una etiqueta es un código corto letra de uno o dos denota una marca de moderación, por ejemplo, 'a' para adulto, 'r' para adultos y así sucesivamente. Los administradores pueden definir nuevas etiquetas para su negocio según sea necesario.

### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>¿Cuántos miembros del equipo puede haber en mi equipo de revisión?

Puede tener un máximo de cinco miembros del equipo, incluido el administrador, en un equipo.
