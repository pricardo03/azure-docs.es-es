---
title: 'Preguntas más frecuentes: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Obtenga respuestas para las preguntas más frecuentes acerca del uso de Content Moderator.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: faq
ms.date: 11/21/2016
ms.author: sajagtap
ms.openlocfilehash: ce61a89069caf90695a2cb6c54f61de8fa8cb112
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226943"
---
# <a name="frequently-asked-questions-faq"></a>Preguntas más frecuentes

#### <a name="what-does-my-content-moderator-subscription-include"></a>¿Qué incluye mi suscripción a Content Moderator?
Su suscripción a Content Moderator incluye el acceso a la herramienta de revisión y a las API. Puede decidir si quiere utilizar una u otra, o ambas, en función de las necesidades empresariales.

#### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>¿Cuáles son los límites y restricciones del contenido que se puede moderar mediante la API?
Cuando utilice la API, las imágenes deben tener un mínimo de 128 píxeles y un tamaño máximo de archivo de 4 MB. El texto no puede superar los 1024 caracteres de longitud. No hay ningún límite para la duración del vídeo.

#### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>¿Qué ocurre si el contenido pasado a la API de texto o a la API de imagen supera los límites de tamaño?
La API de texto devuelve un código de error que informa que el texto es más largo de lo permitido. La API de imagen también devuelve un código de error que informa que la imagen no cumple los requisitos de tamaño.

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>¿Se conservan las imágenes, el texto o los vídeos que se envían para moderación?
El contenido es de su propiedad y Microsoft no puede retenerlo sin su consentimiento. Microsoft usa medidas de seguridad líderes del sector para ayudar a proteger su contenido.

#### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>¿Puedo usar Content Moderator para filtrar imágenes ilegales de explotación infantil?
No. Sin embargo, las organizaciones cualificadas pueden usar [PhotoDNA Cloud Service](https://www.microsoft.com/photodna "Microsoft PhotoDNA Cloud Service") para filtrar este tipo de contenido.

#### <a name="up-to-how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>¿A cuántos equipos de revisión puede unirse un usuario como máximo? ¿Puede el usuario cambiar entre equipos?
Un usuario puede unirse a un equipo cada vez.

#### <a name="what-kind-of-team-member-roles-are-supported-by-the-review-tool-how-are-they-different"></a>¿Qué tipos de roles de los miembros del equipo son compatibles con la herramienta de revisión? ¿En qué se diferencian?
Studio actualmente permite la asignación de los roles de administrador y revisor. Los administradores pueden invitar a otros usuarios y tener acceso a los valores de configuración, mientras que los revisores solo pueden revisar los resultados de la moderación y etiquetar o quitar etiquetas del contenido.

#### <a name="what-is-a-tag-does-the-review-tool-support-custom-tags"></a>¿Qué es una etiqueta? ¿Admite la herramienta de revisión etiquetas personalizadas?
Una etiqueta es un código corto de una o dos letras que indica una marca de moderación, por ejemplo, "a" para adultos, "o" para obsceno, etc. Los administradores pueden definir nuevas etiquetas para su negocio según sea necesario.

#### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>¿Cuántos miembros del equipo puede haber en mi equipo de revisión?
Puede haber un máximo de cinco miembros del equipo, incluido el administrador del equipo.
