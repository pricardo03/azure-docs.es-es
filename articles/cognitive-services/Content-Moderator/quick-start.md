---
title: Introducción a Azure Content Moderator | Microsoft Docs
description: Introducción a Azure Content Moderator.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/15/2018
ms.author: sajagtap
ms.openlocfilehash: ae4333047ebd95733c7baaed0323a0c2c477d323
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380802"
---
# <a name="get-started-with-content-moderator"></a>Introducción a Content Moderator

Para empezar a trabajar con las API de Content Moderator y la herramienta de revisión, haga lo siguiente:

- [Comience con la herramienta de revisión](#start-with-the-review-tool) para crear las claves de API y un equipo de revisión. Explore la herramienta de revisión y aprenda a integrar con las API de Content Moderator.
- [Suscríbase a Content Moderator](#start-with-the-apis) en Azure Portal. Aún deberá iniciar sesión en línea para crear un equipo de revisión.
- [Use el conector y las plantillas de Flow](https://flow.microsoft.com/connectors/shared_cognitiveservicescontentmoderator/content-moderator/) para comprobar una amplia gama de integraciones con un diseñador fácil de usar.

Independientemente de la opción que elija, consulte el artículo [Administración de credenciales](review-tool-user-guide/credentials.md) para encontrar las credenciales de la API.

## <a name="start-with-the-review-tool"></a>Comenzar con la herramienta de revisión
[Regístrese](http://contentmoderator.cognitive.microsoft.com/) en el sitio web de la herramienta de revisión de Content Moderator.

![Página principal de Content Moderator](images/homepage.PNG)

### <a name="create-a-review-team"></a>Creación de un equipo de revisión
Póngale un nombre al equipo. Si desea invitar a sus colegas, puede escribir sus direcciones de correo electrónico.

![Invitar a miembros del equipo](images/QuickStart-2-small.png)

### <a name="upload-images-or-enter-text"></a>Cargar imágenes o escribir texto
Haga clic en **Probar > Imagen** o **Probar > Texto**. Cargue hasta cinco imágenes de ejemplo o escriba el texto de ejemplo para moderación.

![Probar moderación de imagen o texto](images/tryimagesortext.png)

### <a name="submit-for-automated-moderation"></a>Envío para moderación automatizada
Envíe el contenido para moderación automatizada. Internamente, la herramienta de revisión llama a las API de moderación para examinar el contenido. Cuando haya finalizado el análisis, verá un mensaje que le informa sobre los resultados a la espera de que los revise.

![Moderar archivos](images/submitted.png)

### <a name="review-and-confirm-results"></a>Revisión y confirmación de resultados
Revise las etiquetas moderadas automáticamente, cámbielas si es preciso, y envíe con el botón **Siguiente**. Dado que la aplicación empresarial llama a las API de Moderator, el contenido etiquetado comienza a apilarse en la cola, listo para que los equipos de revisión humana los revise. Con este enfoque, puede revisar rápidamente grandes volúmenes de contenido.

![Revisión de los resultados](images/reviewresults.png)

Aprenda a usar todas las [características de la herramienta de revisión](Review-Tool-User-Guide/human-in-the-loop.md) o continúe con la siguiente sección para obtener información acerca de las API. Omita el paso de registro porque la clave de API se aprovisiona por usted en la herramienta de revisión, como se muestra en el artículo [Administración de credenciales](review-tool-user-guide/credentials.md).

### <a name="use-the-apis"></a>Usar las API

Ahora que ha explorado la moderación de contenido y la experiencia de la herramienta de revisión, obtenga información sobre cómo integrar Content Moderator con sus aplicaciones empresariales. En la sección siguiente, obtendrá más información y ampliará sus conocimientos con los SDK y ejemplos.

## <a name="start-with-the-apis"></a>Comenzar con las API

[Suscríbase a Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) en Azure Portal. Comience con una de las API siguientes:

### <a name="image-moderation"></a>Moderación de imágenes

Comience con la [consola de API](try-image-api.md) o use el [inicio rápido de .NET](image-moderation-quickstart-dotnet.md) para examinar imágenes y detectar posible contenido explícito y para adultos mediante etiquetas, puntuaciones de confianza y otra información extraída.

### <a name="text-moderation"></a>Moderación de texto

Comience con la [consola de API](try-text-api.md) o use el [inicio rápido de .NET](text-moderation-quickstart-dotnet.md) para examinar el contenido de texto para buscar posibles palabras soeces, clasificación de texto no deseado asistida por máquina (versión preliminar) e información de identificación personal (DCP). 


### <a name="video-moderation"></a>Moderación de vídeo

Comience con el [inicio rápido de .NET](video-moderation-api.md) para examinar vídeos y detectar posible contenido explícito y para adultos. 


### <a name="review-apis"></a>Revisión de las API

Empiece aquí eligiendo las API de trabajos, revisiones y flujos de trabajo.

- La [API de trabajos](try-review-api-job.md) examina el contenido con las API de moderación y genera las revisiones en la herramienta de revisión. 
- La [API de revisiones](try-review-api-review.md) crea directamente revisiones de imágenes, texto o vídeos para moderadores humanos sin examinar antes el contenido. 
- La [API de flujos de trabajo](try-review-api-workflow.md) crea, actualiza y obtiene información detallada sobre los flujos de trabajo personalizados que crea el equipo.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo iniciar la moderación de contenido con la [API de moderación de imágenes](image-moderation-api.md).
