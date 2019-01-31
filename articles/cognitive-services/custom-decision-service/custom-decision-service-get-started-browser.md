---
title: 'Llamada a la API desde un explorador: Custom Decision Service'
titlesuffix: Azure Cognitive Services
description: Cómo optimizar una página web mediante la realización de llamadas API directamente desde un explorador a Custom Decision Service.
services: cognitive-services
author: slivkins
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ms.openlocfilehash: db993693acc7e64a789564b92f4d0eacfa0e69f7
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225268"
---
# <a name="call-api-from-a-browser"></a>Llamada a la API desde un explorador

En este artículo, le ayudaremos a realizar llamadas a las API de Azure Custom Decision Service directamente desde un explorador.

En primer lugar, asegúrese de [Registrar su aplicación](custom-decision-service-get-started-register.md).

Comencemos. La aplicación se modela como si tuviera una página principal con vínculos a varias páginas de artículo. La página principal utiliza Custom Decision Service para especificar el orden de las páginas del artículo. Inserte el código siguiente en el encabezado HTML de la página principal:

```html
// Define the "callback function" to render UI
<script> function callback(data) { … } </script>

// call Ranking API, after callback() is defined
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>" async></script>
```

El argumento `data` contiene la clasificación de las direcciones URL que se representarán. Para más información, consulte la [API](custom-decision-service-api-reference.md) de referencia.

Para administrar un clic de usuario en el artículo principal, llame al código siguiente en la página principal:

```javascript
// call Reward API to report a click
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

En este caso, `data` es el argumento para la función `callback()`. Encontrará un ejemplo de implementación en este [tutorial](custom-decision-service-tutorial-news.md#use-the-apis).

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

En este ejemplo, cada elemento `<item>` de nivel superior describe un artículo. `<link>` es obligatorio y Custom Decision Service lo usa como id. de acción. Especifique `<date>` (en un formato RSS estándar) si tiene más de 15 artículos. Se usan los 15 artículos más recientes. El elemento `<title>` es opcional y se usa para crear características relacionadas con texto para el artículo.

## <a name="next-steps"></a>Pasos siguientes

* Examine un [tutorial](custom-decision-service-tutorial-news.md) para obtener ejemplos más detallados.
* Consulte la [API](custom-decision-service-api-reference.md) de referencia para obtener más información acerca de la funcionalidad proporcionada.
