---
title: ¿Qué es Project URL Preview?
titlesuffix: Azure Cognitive Services
description: Introducción a Project URL Preview.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: overview
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: 2e974acb1a7583fbd1a961eda4daa3732fbeff7c
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55218180"
---
# <a name="what-is-project-url-preview"></a>¿Qué es Project URL Preview?
El punto de conexión de URL Preview adopta un parámetro de consulta de dirección URL y devuelve una respuesta JSON con el nombre del recurso de destino, una breve descripción y un vínculo a una imagen para mostrarla en una vista previa. La respuesta incluye también la marca [isFamilyFriendly](url-preview-reference.md#query-parameters), que indica si la dirección URL contiene contenido para adultos, pirateado o ilegal. 

Para obtener los resultados de URL Preview, envíe una solicitud GET e incluya el encabezado *Ocp-Apim-Subscription-Key* con un token válido:  
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```
La respuesta: 
```
HTTP Headers:
BingAPIs-TraceId: 3CC74C94769440C0851D9DF0869FCE7F
BingAPIs-SessionId: 52219085A6364692958C9C83983A0DBA
X-MSEdge-ClientID: 13D44DC2DE946B4B0F25460CDF036AD6
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 3CC74C94769440C0851D9DF0869FCE7F Ref B: CO1EDGE0315 Ref C: 2018-04-11T18:47:40Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https:\/\/swiftkey.com\/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https:\/\/swiftkey.com\/images\/og\/default.jpg"
  }
}

```
## <a name="scenarios"></a>Escenarios 

URL Preview API admite descripciones breves de recursos web. Los desarrolladores la usan para crear experiencias enriquecidas de vista previa.  Los usuarios pueden compartir o marcar páginas web, noticias, blogs, foros, etc. Esta API también puede utilizarse para moderación de contenido.    

Las aplicaciones usan URL Preview para enviar solicitudes web al punto de conexión con una consulta asignada a la dirección URL para la vista previa.  La respuesta JSON contiene la información de vista previa: el nombre, la descripción del recurso, la marca *familyFriendly* y los vínculos que proporcionen acceso a una imagen representativa y al recurso completo en línea. 

## <a name="terms-of-use"></a>Términos de uso
Utilice únicamente los datos de Project URL Preview para mostrar fragmentos de código de versión preliminar e imágenes en miniatura con hipervínculos a sus sitios de origen, en la URL iniciada por el usuario final compartida en redes sociales, bot de chat u ofertas similares. No copie, almacene ni almacene en caché los datos que reciba de Project URL Preview. Respete las solicitudes para deshabilitar las versiones preliminares que pueda recibir del sitio web o los propietarios del contenido.

Tenga en cuenta que ni usted, ni un tercero en su nombre, pueden usar, retener, guardar, almacenar en caché, compartir o distribuir datos de URL Preview API con el fin de probar, desarrollar, entrenar, distribuir o poner a disposición cualquier servicio o característica que no sea de Microsoft. 

## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](csharp.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de JavaScript](javascript.md)
- [Inicio rápido de Node](node-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)
