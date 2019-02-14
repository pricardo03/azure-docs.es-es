---
title: 'Llamada a la API desde una aplicación: Custom Decision Service'
titlesuffix: Azure Cognitive Services
description: Cómo llamar a las API de Custom Decision Service desde una aplicación de smartphone.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: slivkins
ms.openlocfilehash: 0e5c99aae61fb927ea7f101bab74d661a747f88b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55870017"
---
# <a name="call-api-from-an-app"></a>Llamada a la API desde una aplicación

Realice llamadas a las API de Azure Custom Decision Service desde una aplicación de smartphone. En este artículo se explica cómo comenzar con algunas opciones básicas.

En primer lugar, asegúrese de [Registrar su aplicación](custom-decision-service-get-started-register.md).

Hay dos llamadas a la API que se realizan desde la aplicación de smartphone a Custom Decision Service: una llamada a Ranking API para obtener una lista clasificada del contenido y una llamada a Reward API para notificar una recompensa. A continuación, se indican las llamadas de ejemplo en [cURL](https://en.wikipedia.org/wiki/CURL).

Para obtener más información, consulte la [API](custom-decision-service-api-reference.md) de referencia.

Empiece con la llamada a Ranking API. Cree el archivo `<request.json>`, que tiene el id. del conjunto de acciones. El id. es el nombre de la fuente Atom o RSS correspondiente que introdujo en el portal:

```json
{"decisions":
     [{ "actionSets":[{"id":{"id":"<actionSetId>"}}] }]}
```

Muchos conjuntos de acciones pueden especificarse del siguiente modo:

```json
{"decisions":
    [{ "actionSets":[{"id":{"id":"<actionSetId1>"}},
                     {"id":{"id":"<actionSetId2>"}}] }]}
```

Después, este archivo JSON se envía como parte de la solicitud de clasificación:

```shell
curl -d @<request.json> -X POST https://ds.microsoft.com/api/v2/<appId>/rank --header "Content-Type: application/json"
```

En este caso, `<appId>` es el nombre de la aplicación registrada en el portal. Debería recibir un conjunto ordenado de elementos de contenido, que puede representar en la aplicación. Un ejemplo devuelto tiene el siguiente aspecto:

```json
[{ "ranking":[{"id":"actionId3"}, {"id":"actionId1"}, {"id":"actionId2"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "rewardAction":"actionId3",
   "actionSets":[{"id":"<actionSetId1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<actionSetId2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]}]
```

La primera parte de la devolución incluye una lista de acciones ordenadas, especificadas por sus id. En el caso de un artículo, el id. de la acción es una dirección URL. La solicitud general también tiene un valor de `<eventId>` único creado por el sistema.

Más adelante, puede especificar si observó un clic en el primer elemento de contenido de este evento, que es `<actionId3>`. A continuación, puede informar acerca de una recompensa en este elemento `<eventId>` a Custom Decision Service a través de Reward API, con otra solicitud como:

```shell
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST
```

Por último, debe proporcionar Action Set API, que devuelve la lista de artículos (acciones) que debe considerar Custom Decision Service. Implemente esta API como una fuente RSS, tal como se muestra a continuación:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <pubDate>Thu, 27 Apr 2017 16:30:52 GMT</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

En este ejemplo, cada elemento `<item>` de nivel superior describe un artículo. El elemento `<link>` es obligatorio y Custom Decision Service lo usa como un Id. de acción. Especifique `<date>` (en un formato RSS estándar) si tiene más de 15 artículos. Se usan los 15 artículos más recientes. El elemento `<title>` es opcional y se usa para crear características relacionadas con texto para el artículo.

## <a name="next-steps"></a>Pasos siguientes

* Examine un [tutorial](custom-decision-service-tutorial-news.md) para obtener ejemplos más detallados.
* Consulte la [API](custom-decision-service-api-reference.md) de referencia para obtener más información acerca de la funcionalidad proporcionada.
