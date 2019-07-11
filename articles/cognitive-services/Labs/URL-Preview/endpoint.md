---
title: Punto de conexión de Project URL Preview
titlesuffix: Azure Cognitive Services
description: Resumen del punto de conexión de URL Preview.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh
ms.openlocfilehash: 43254db734a48f3e7aaa5a26a7fbf3981c9e9d87
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592866"
---
# <a name="project-url-preview-endpoint"></a>Punto de conexión de Project URL Preview

URL Preview API incluye un punto de conexión.

## <a name="endpoint"></a>Punto de conexión
Para solicitar URL Preview, envíe una solicitud al punto de conexión siguiente. Use los encabezados y parámetros de dirección URL para otras especificaciones.

GET:
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```

### <a name="query-parameters"></a>Parámetros de consulta
|NOMBRE|Valor|type|Obligatorio|  
|----------|-----------|----------|--------------|  
|q|Dirección URL para la vista previa|Cadena |Sí|
|safeSearch|El contenido para adultos ilegal o contenido pirateado se bloquea con el código de error 400 y no se devuelve la marca *isFamilyFriendly*. <p>Para el contenido para adultos legal, se muestra el comportamiento a continuación. Código de estado devuelve 200 y la marca *isFamilyFriendly* se establece en false.<ul><li>safeSearch=strict: no se devolverán los valores de título, descripción, dirección URL e imagen.</li><li>safeSearch=moderate: se obtienen los valores de título, dirección URL y descripción, pero no la imagen descriptiva.</li><li>safeSearch=off: se obtienen todos los objetos/elementos de respuesta: título, dirección URL, descripción e imagen.</li></ul> |Cadena|No se requiere. </br> Se establece de manera predeterminada en safeSearch=strict.| 

## <a name="response-object"></a>Objeto de respuesta

La respuesta incluye encabezados HTTP y el objeto WebPage con atributos, como se muestra en el ejemplo siguiente: `name`, `url`, `description`, `isFamilyFriendly` y `primaryImageOfPage`.

```
BingAPIs-TraceId: 15AFE52A97AA422F960433A94803F6CE
BingAPIs-SessionId: 40587764F42142D3A8BA99F66B2B3BB6
X-MSEdge-ClientID: 0389E3EDED106B5E1424E82FEC436A56
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 15AFE52A97AA422F960433A94803F6CE Ref B: PAOEDGE0418 Ref C: 2018-03-30T16:36:27Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile ...",
  "url": "https://swiftkey.com/",
   "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

```

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](csharp.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de JavaScript](javascript.md)
- [Inicio rápido de Node](node-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)
