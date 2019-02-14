---
title: 'Tutorial: Personalización de artículos (Custom Decision Service)'
titlesuffix: Azure Cognitive Services
description: Tutorial para la personalización de artículos para la toma de decisiones según el contexto.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: tutorial
ms.date: 05/08/2018
ms.author: slivkins
ms.openlocfilehash: d8ddafe20ff93e7ae4d51e2180bbd40447729234
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55983039"
---
# <a name="tutorial-article-personalization-for-contextual-decision-making"></a>Tutorial: Personalización de artículos para la toma de decisiones en función del contexto

Este tutorial se centra en la personalización de la selección de artículos de la página principal de un sitio web. Custom Decision Service afecta a *varias* listas de artículos en la página principal, por ejemplo. Quizás la página es una página web de noticias que abarca solo política y deportes. Mostraría tres listas clasificadas de artículos: política, deportes y recientes.

## <a name="applications-and-action-sets"></a>Aplicaciones y conjuntos de acciones

Esta es la forma de adaptar el escenario al marco. Imaginemos tres aplicaciones, una para cada lista que se está optimizando: app-politics, app-sports, and app-recent. Para especificar los artículos candidatos para cada aplicación, hay dos conjuntos de acciones: uno para política y otro para deportes. El conjunto de aplicaciones para app-recent se crea automáticamente como unión de los otros dos.

> [!TIP]
> Los conjuntos de acciones se pueden compartir entre aplicaciones en Custom Decision Service.

## <a name="prepare-action-set-feeds"></a>Preparar fuentes del conjunto de acciones

Custom Decision Service consume conjuntos de acciones a través de fuentes Atom o RSS que proporciona el cliente. Se proporcionan dos fuentes: una para política y otra para deportes. Suponga que se proporcionan desde `http://www.domain.com/feeds/<feed-name>`.

Cada fuente proporciona una lista de artículos. En RSS, cada uno de ellos se especifica mediante un elemento `<item>`, como se indica a continuación:

```xml
<rss version="2.0"><channel>
   <item>
      <title><![CDATA[article title]]></title>
      <link>"article url"</link>
      <pubDate>publication date</pubDate>
    </item>
</channel></rss>
```

El orden de los artículos es importante. Especifica la clasificación predeterminada, que es la mejor manera de adivinar cómo se deben ordenar los artículos. A continuación, la clasificación predeterminada se usa para la comparación de rendimiento en el panel.

Para obtener más información sobre el formato de fuente, consulte la [referencia de la API](custom-decision-service-api-reference.md#action-set-api-customer-provided).

## <a name="register-a-new-app"></a>Registrar una aplicación nueva

1. Inicie sesión con la [cuenta de Microsoft](https://portal.ds.microsoft.com/). En la cinta, haga clic en **Mi portal**.

2. Para registrar una aplicación nueva, haga clic en el botón **Nueva aplicación**.

    ![Portal de Custom Decision Service](./media/custom-decision-service-tutorial/portal.png)

3. Escriba un nombre único para la aplicación en el cuadro de texto **Id. de la aplicación**. Si otro cliente ya usa este nombre, el sistema le pide que seleccione otro id. de aplicación. Seleccione la casilla **Avanzadas** y escriba la [cadena de conexión](../../storage/common/storage-configure-connection-string.md) para su cuenta de almacenamiento de Azure. Normalmente, se utiliza la misma cuenta de almacenamiento para todas las aplicaciones.

    ![Cuadro de diálogo Nueva aplicación](./media/custom-decision-service-tutorial/new-app-dialog.png)

    Después de registrar las tres aplicaciones en el escenario anterior, se muestran:

    ![Lista de aplicaciones](./media/custom-decision-service-tutorial/apps.png)

    Puede volver a esta lista haciendo clic en el botón **Aplicaciones**.

4. En el cuadro de diálogo **Nueva aplicación**, especifique una fuente de acción. Otra forma de especificar fuentes de acción es hacer clic en el botón **Fuentes** y, a continuación, hacer clic en el botón **Nueva fuente**. Escriba un **Nombre** para la nueva fuente, especifique la **URL** desde la que se proporciona y especifique el **tiempo de actualización**. El tiempo de actualización especifica la frecuencia con que Custom Decision Service debe actualizar la fuente.

    ![Cuadro de diálogo Nueva fuente](./media/custom-decision-service-tutorial/new-feed-dialog.png)

    Cualquier aplicación puede usar fuentes de acción, independientemente de dónde se hayan especificado. Después de especificar ambas fuentes de acción en un escenario, se muestran:

    ![Lista de fuentes](./media/custom-decision-service-tutorial/feeds.png)

    Para volver a esta lista, haga clic en el botón **Fuentes**.

## <a name="use-the-apis"></a>Usar las API

Custom Decision Service clasifica los artículos mediante Ranking API. Para utilizar esta API, inserte el código siguiente en el encabezado HTML de la página principal:

```html
<!-- Define the "callback function" to render UI -->
<script> function callback(data) { … } </script>

<!--  call Ranking API after callback() is defined, separately for each app -->
<script src="https://ds.microsoft.com/api/v2/app-politics/rank/feed-politics" async></script>
<script src="https://ds.microsoft.com/api/v2/app-sports/rank/feed-sports" async></script>
<script src="https://ds.microsoft.com/api/v2/app-recent/rank/feed-politics/feed-sports" async></script>
<!-- NB: action feeds for 'app-recent' are listed one after another. -->
```

La respuesta HTTP a través de Ranking API es una cadena con formato JSONP. Para app-politics, por ejemplo, la cadena tiene este aspecto:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"app-politics",
   "actionSets":[{"id":"feed-politics","lastRefresh":"date"}] });
```

A continuación, el explorador ejecuta esta cadena como una llamada a la función `callback()`. El argumento `data` de la función `callback()` contiene el identificador de la aplicación y la clasificación de las direcciones URL que se representarán. En concreto, `callback()` debe usar `data.appId` para distinguir entre las tres aplicaciones. Custom Decision Service usa `eventId` internamente para que coincida con la clasificación proporcionado con el clic correspondiente, si existe.

> [!TIP]
> `callback()` puede comprobar si cada fuente de acciones está actualizada mediante el campo `lastRefresh`. Si una fuente determinada no está bastante actualizada, `callback()` podría ignorar la clasificación proporcionada, llamar directamente a esta fuente y usar la clasificación predeterminada que proporciona la fuente.

Para obtener más información sobre las especificaciones y las opciones adicionales que proporciona Ranking API, consulte la [referencia de la API](custom-decision-service-api-reference.md).

Las opciones de artículo principal del usuario se devuelven mediante una llamada a Reward API. Cuando se recibe una opción de artículo principal, se debe invocar el siguiente código en la página principal:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/<eventId>',
    contentType: "application/json" })
```

Para usar `appId` y `eventId` en el código de control de clics, es necesario tener cuidado. Por ejemplo, puede implementar la función `callback()` como se indica a continuación:

```javascript
function callback(data) {
    $.map(data.ranking, function (element) {
        //custom rendering function given content info
        render({appId:data.appId,
                article:element,
                onClick: element.id != data.rewardAction ? null :
                   function() {
                       $.ajax({
                       type: "POST",
                       url: '//ds.microsoft.com/api/v2/' + data.appId + '/reward/' + data.eventId,
                       contentType: "application/json" })}
                });
}}
```

En este ejemplo, implemente la función `render()` para representar un artículo determinado para una aplicación determinada. Esta función introduce el id. de la aplicación y el artículo (en el formato de Ranking API). El parámetro `onClick` es la función que se debe llamar desde `render()` para controlar un clic. Comprueba si el clic en está en el espacio superior. A continuación, llama a Reward API con el id. de evento y el id. de aplicación adecuados.

## <a name="next-steps"></a>Pasos siguientes

* Consulte la [referencia de la API](custom-decision-service-api-reference.md) para obtener más información sobre la funcionalidad que proporciona.
