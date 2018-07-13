---
title: API - Azure Cognitive Services | Microsoft Docs
description: Aquí encontrará una guía de API fácil de usar sobre Azure Custom Decision Service, y una API basada en la nube para la toma de decisiones contextual que mejora con la experiencia.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/11/2018
ms.author: slivkins
ms.reviewer: marcozo, alekh
ms.openlocfilehash: 403b17e33394016a07a7b33ba1bcbfe6afdcc05b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382391"
---
# <a name="api"></a>API

Azure Custom Decision Service proporciona dos API para cada decisión: [Ranking API](#ranking-api) para detallar la clasificación de acciones y [Reward API](#reward-api) para generar la recompensa. Además, proporciona [Action Set API ](#action-set-api-customer-provided) para especificar las acciones en Azure Custom Decision Service. En este artículo se detallan estas tres API. A continuación usaremos un escenario típico para mostrar el momento en que Custom Decision Service optimiza la clasificación de los artículos.

## <a name="ranking-api"></a>Ranking API

Ranking API usa un patrón de comunicación de estilo [JSONP](https://en.wikipedia.org/wiki/JSONP) estándar para optimizar la latencia y eludir la [directiva de origen similar](https://en.wikipedia.org/wiki/Same-origin_policy). Esta última prohíbe que JavaScript obtenga datos desde fuera del origen de la página.

Inserte este fragmento de código en el encabezado HTML de su página principal (donde se muestra una lista personalizada de artículos):

```html
// define the "callback function" to render UI
<script> function callback(data) { … } </script>
// "data" provides the ranking of URLs, see example below.

// call to Ranking API
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>?<parameters>" async></script>
// action set id is the name of the corresponding RSS/Atom feed.

// same call with multiple action sets:
// <script src="https://ds.microsoft.com/api/v2/<appId>/rank/<A1>/<A2>/.../<An>?<parameters>" async></script>
```

> [!IMPORTANT]
> La función de devolución de llamada se debe definir antes de llamar a Ranking API.

> [!TIP]
> Para mejorar la latencia, Ranking API se expone a través de HTTP en lugar de HTTPS, como sucede en `http://ds.microsoft.com/api/v2/<appId>/rank/*`.
> Sin embargo, se debe usar un punto de conexión HTTPS si la página principal se muestra a través de HTTPS.

Cuando los parámetros no se utilizan, la respuesta HTTP de Ranking API es una cadena con formato JSONP:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<A2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]});
```

A continuación, el explorador ejecuta esta cadena como una llamada a la función `callback()`.

El parámetro para la función de devolución de llamada en el ejemplo anterior tiene el siguiente esquema:

- `ranking` proporciona la clasificación de las direcciones URL que se mostrarán.
- Custom Decision Service usa `eventId` de forma interna para hacer coincidir esta clasificación con los clics correspondientes.
- `appId` permite que la función de devolución de llamada distinga entre varias aplicaciones de Custom Decision Service que se ejecutan en la misma página web.
- `actionSets` enumera cada conjunto de acciones que se usa en la llamada de Ranking API, junto con la marca de tiempo UTC de la última actualización que se realizó con éxito. Custom Decision Service actualiza periódicamente las fuentes del conjunto de acciones. Por ejemplo, si algunos de los conjuntos de acciones no están actualizadas, la función de devolución de llamada podría necesitar volver a su clasificación predeterminada.

> [!IMPORTANT]
> Los conjuntos de acciones especificados se procesan, y posiblemente se eliminan, para formar la clasificación predeterminada de los artículos. A continuación, la clasificación predeterminada se vuelve a ordenar y se devuelve en la respuesta HTTP. La clasificación predeterminada se define aquí:
>
> - Dentro de cada conjunto de acciones, los artículos se eliminan hasta que quedan los 15 artículos más recientes (si es que se devuelven más de 15).
> - Cuando se especifican varios conjuntos de acciones, se combinan en el mismo orden que en la llamada API. El orden original de los artículos se conserva dentro de cada conjunto de acciones. Los duplicados se eliminan para dejar paso a las copias anteriores.
> - Los primeros `n` artículos se quitan de la lista combinada de artículos, donde `n=20` por defecto.

### <a name="ranking-api-with-parameters"></a>Ranking API con parámetros

Ranking API permite estos parámetros:

- `details=1` y `details=2` insertan detalles adicionales sobre cada artículo enumerado en `ranking`.
- `limit=<n>` especifica la cantidad máxima de artículos en la clasificación predeterminada. `n` debe estar entre `2` y `30` (si no, se trunca a `2` o `30` respectivamente).
- `dnt=1` deshabilita las cookies de usuario.

Los parámetros se pueden combinar en la sintaxis estándar de la cadena de consulta; por ejemplo, `details=2&dnt=1`.

> [!IMPORTANT]
> La configuración predeterminada en Europa debe ser `dnt=1` hasta que el cliente acepte el banner de las cookies. Esta también debe ser la configuración predeterminada de los sitios web que se dirigen a menores. Para obtener más información, consulte los [términos de uso](https://www.microsoft.com/cognitive-services/en-us/legal/CognitiveServicesTerms20160804).

El elemento `details=1` inserta el valor `guid` de cada artículo, si lo proporciona Action Set API. La respuesta HTTP:

```json
callback({
   "ranking":[{"id":"url1","details":[{"guid":"123"}]},
              {"id":"url2","details":[{"guid":"456"}]},
              {"id":"url3","details":[{"guid":"789"}]}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

El elemento `details=2` agrega más detalles que Custom Decision Service puede extraer del [código de caracterización](https://github.com/Microsoft/mwt-ds/tree/master/Crawl) de metaetiquetas SEO:

- `title` de `<meta property="og:title" content="..." />` o `<meta property="twitter:title" content="..." />` o `<title>...</title>`
- `description` de `<meta property="og:description" ... />` o `<meta property="twitter:description" content="..." />` o `<meta property="description" content="..." />`
- `image` de `<meta property="og:image" content="..." />`
- `ds_id` de `<meta name=”microsoft:ds_id” content="..." />`

La respuesta HTTP:

```json
callback({
   "ranking":[{"id":"url1","details":<details>},
              {"id":"url2","details":<details>},
              {"id":"url3","details":<details>}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

En el elemento `<details>`:

```json
[{"guid":"123"}, {"description":"some text", "ds_id":"234", "image":"ImageUrl1", "title":"some text"}]
```

## <a name="reward-api"></a>Reward API

Custom Decision Service usa clics solo en la ranura superior. Cada clic se interpreta como una recompensa de 1. La falta de un clic se interpreta como una recompensa de 0. Los clics se comparan con las clasificaciones correspondientes mediante identificaciones de eventos, que se crean con la llamada [Ranking API](#ranking-api). Si es necesario, los id. de eventos se pueden pasar a través de las cookies de la sesión.

Para manejar un clic en la ranura superior, coloque este código en su página principal:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Aquí `data` es el argumento para la función `callback()`, tal como se describió anteriormente. Recuerde que debe tener cuidado al usar `data` en el código de manejo de clics. Un ejemplo se muestra en este [tutorial](custom-decision-service-tutorial-news.md#use-the-apis).

Solo para realizar pruebas, puede llamar a Reward API a través de [cURL](https://en.wikipedia.org/wiki/CURL):

```sh
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST -d 1 -H "Content-Type: application/json"
```

El efecto esperado es una respuesta HTTP de 200 (OK). Puede ver la recompensa de 1 para este evento en el registro (si se proporcionó una clave de cuenta de almacenamiento de Azure en el portal).

## <a name="action-set-api-customer-provided"></a>Action Set API (la proporciona el cliente)

En un nivel alto, Action Set API devuelve una lista de artículos (acciones). Cada artículo está especificado por su propia dirección URL y, opcionalmente, el título del artículo y la fecha de publicación. Puede especificar varios conjuntos de acciones en el portal. Se debe usar una Action Set API para cada conjunto de acciones, como una dirección URL distinta.

Cada Action Set API se puede implementar de dos maneras: como fuente RSS o como fuente Atom. Cualquiera de las dos opciones debe cumplir con el estándar y devolver un XML correcto. En cuanto a RSS, he aquí un ejemplo:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <guid>guid (could be a your internal database id)</guid>
      <pubDate>date</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Cada elemento `<item>` de nivel superior describe una acción:

- `<link>` es obligatorio y se usa como un id. de acción.
- `<date>` se ignora si es menor o igual a 15 elementos; de lo contrario, es obligatorio.
  - Si hay más de 15 elementos, se utilizan los 15 más recientes.
  - Debe estar en el formato estándar de RSS o Atom, respectivamente:
    - [RFC 822](https://tools.ietf.org/html/rfc822) para RSS: por ejemplo, `"Fri, 28 Apr 2017 18:02:06 GMT"`
    - [RFC 3339](https://tools.ietf.org/html/rfc3339) para Atom: por ejemplo, `"2016-12-19T16:39:57-08:00"`
- `<title>` es opcional y se usa para generar características que describen el artículo.
- `<guid>` es opcional y se pasa a través del sistema a la función de devolución de llamada (si el parámetro `?details` se especifica en la llamada de Ranking API).

Se ignoran otros elementos de `<item>`.

La versión de la fuente Atom usa la misma sintaxis y convenciones XML.

> [!TIP]
> Si su sistema usa sus propios id. de artículo, se pueden pasar a la función de devolución de llamada mediante `<guid>`.