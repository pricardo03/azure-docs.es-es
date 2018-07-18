---
title: ¿Qué es Custom Decision Service? - Azure Cognitive Services | Microsoft Docs
description: Este artículo ofrece información general sobre Azure Custom Decision Service, una API basada en la nube para la toma de decisiones contextual que se agudiza con la experiencia.
services: cognitive-services
author: alekh
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh;marossi
ms.openlocfilehash: 774467446513dcd7ade7255d998b11f41824cafe
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2018
ms.locfileid: "35383427"
---
# <a name="what-is-custom-decision-service"></a>¿Qué es Custom Decision Service?

En una aplicación web o móvil típica, con una página principal con vínculos a varios artículos u otros tipos de contenido. Cuando la página principal se carga, podría solicitar a Custom Decision Service que clasifique por orden de prioridad los artículos incluidos en esa página. Por lo tanto, cuando un usuario hace clic en un artículo para elegirlo, se podría enviar una segunda solicitud a Custom Decision Service, que registrará la salida de esa decisión del usuario.

Custom Decision Service es fácil de usar, ya que requiere solo una fuente RSS para el contenido y agregar unas pocas líneas de JavaScript a la aplicación.

Custom Decision Service convierte el contenido en características para el aprendizaje automático. El sistema utiliza estas características para entender el contenido en cuanto a su texto, imágenes, vídeos y opiniones generales. Usa otros servicios de [Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services), como [Entity Linking](../entitylinking/home.md), [Text Analytics](../text-analytics/overview.md), [Emotion](../emotion/home.md) y [Computer Vision](../computer-vision/home.md).

Algunos escenarios comunes para Custom Decision Service son:

* Personalizar artículos en un sitio web de noticias
* Personalizar el contenido de vídeo en un portal multimedia
* Optimizar la ubicación de los anuncios o las páginas web a las que el anuncio dirige
* Clasificación por orden de prioridad de los elementos de un sitio web de compra.

Custom Decision Service está actualmente en *versión preliminar pública gratuita*. Puede personalizar una lista de artículos en un sitio web o una aplicación. La extracción de características funciona mejor con contenido en inglés. Se ofrece [funcionalidad limitada](../text-analytics/overview.md) para otros idiomas, como español, francés, alemán, portugués y japonés. Esta documentación se revisará a medida que haya nueva funcionalidad disponible.

Custom Decision Service puede usarse en aplicaciones que no están en el dominio de personalización de contenido. Estas aplicaciones podrían ser una buena opción para una versión preliminar personalizada. [Póngase en contacto con nosotros para obtener más información](https://azure.microsoft.com/overview/sales-number/).

## <a name="api-usage-modes"></a>Modos de uso de las API

Custom Decision Service puede aplicarse tanto a páginas web como a aplicaciones móviles. Las API pueden llamarse desde un explorador o desde una aplicación. El uso de la API es similar en ambos, pero algunos detalles son diferentes.

## <a name="glossary-of-terms"></a>Glosario de términos

En esta documentación se repiten varios términos con frecuencia:

* **Conjunto de acciones**: el conjunto de elementos de contenido que Custom Decision Service va a clasificar por orden de prioridad. Este conjunto se puede especificar como un punto de conexión *RSS* o *Atom*.
* **Clasificar por orden de prioridad**: cada solicitud a Custom Decision Service especifica uno o varios conjuntos de acción. El sistema responde y escoge todas las opciones de contenido de estos conjuntos, que después devuelve por orden de prioridad.
* **Función de devolución de llamada**: esta función, que se especifica, representa el contenido de la interfaz de usuario. El contenido se clasifica por el orden de prioridad devuelto por Custom Decision Service.
* **Recompensa**: una medida de cómo el usuario respondió al contenido representado. Custom Decision Service mide la respuesta del usuario mediante clics. Se notifican los clics al sistema mediante el código personalizado que se inserta en la aplicación.

## <a name="next-steps"></a>Pasos siguientes

* [Registro de la aplicación](custom-decision-service-get-started-register.md) en Custom Decision Service
* Empiece a optimizar [una página web](custom-decision-service-get-started-browser.md) o [una aplicación de smartphone](custom-decision-service-get-started-app.md).
* Consulte la [referencia de la API](custom-decision-service-api-reference.md) para obtener más información sobre la funcionalidad que proporciona.