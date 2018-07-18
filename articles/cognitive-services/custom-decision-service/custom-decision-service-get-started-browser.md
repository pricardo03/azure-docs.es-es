---
title: Llamada a API desde un explorador - Azure Cognitive Services | Microsoft Docs
description: Cómo empezar a trabajar con Azure Custom Decision Service para optimizar una página web mediante la realización de llamadas API directamente desde un explorador.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: slivkins,marcozo,alekh
ms.openlocfilehash: 10236c9d8f70d9b90a896464b4f86a847ee904c2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382255"
---
# <a name="call-api-from-a-browser"></a>Llamada a la API desde un explorador

En este artículo, le ayudaremos a realizar llamadas a las API de Azure Custom Decision Service directamente desde un explorador.

En primer lugar, haga clic en [Registrar la aplicación](custom-decision-service-get-started-register.md).

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

Por último, debe proporcionar Action Set API, que devuelve la lista de artículos (acciones) que debe considerar Custom Decision Service. Implemente esta API como fuente RSS, como se muestra aquí:

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

Aquí, cada elemento `<item>` de nivel superior describe un artículo. `<link>` es obligatorio y Custom Decision Service lo usa como id. de acción. Especifique `<date>` (en un formato RSS estándar) si tiene más de 15 artículos. Se usan los 15 artículos más recientes. `<title>` es opcional y se usa para crear características relacionadas con el texto para el artículo.

## <a name="next-steps"></a>Pasos siguientes

* Complete un [tutorial](custom-decision-service-tutorial-news.md) para obtener un ejemplo más detallado.
* Consulte la [API](custom-decision-service-api-reference.md) de referencia para obtener más información sobre la funcionalidad que proporciona.