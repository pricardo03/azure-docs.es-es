---
title: 'Inicio rápido: Creación de una aplicación web que inicia el Lector inmersivo con C#'
titlesuffix: Azure Cognitive Services
description: En este inicio rápido, va a crear una aplicación web desde cero y a agregar la funcionalidad de la API de Lector inmersivo.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 3b408de6b60e7e7704ee228b52c399e5b80e3a9e
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718419"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Inicio rápido: Creación de una aplicación web que inicia el Lector inmersivo (C#)

El [Lector inmersivo](https://www.onenote.com/learningtools) es una herramienta diseñada de forma inclusiva que implementa técnicas demostradas para mejorar la comprensión lectora.

En este inicio rápido, creará una aplicación web desde cero e integrará el Lector inmersivo mediante su SDK. Se puede encontrar un completo ejemplo funcional [aquí](https://github.com/microsoft/immersive-reader-sdk/tree/master/samples/quickstart-csharp).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads)
* Una clave de suscripción para el Lector inmersivo. Para obtener una, siga [estas instrucciones](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).

## <a name="create-a-web-app-project"></a>Creación de un proyecto de aplicación web

Cree un proyecto en Visual Studio mediante la plantilla de aplicación web de ASP.NET Core con Modelo-Vista-Controlador integrado.

![Nuevo proyecto](./media/vswebapp.png)

![Nueva aplicación web de ASP.NET Core](./media/vsmvc.png)

## <a name="acquire-an-access-token"></a>Adquisición de un token de acceso

Necesitará la clave de suscripción y el punto de conexión para este siguiente paso. Puede encontrar la clave de suscripción en la página Claves del recurso Lector inmersivo de Azure Portal. Puede encontrar el punto de conexión en la página Información general.

Haga clic con el botón derecho en el proyecto en el _Explorador de soluciones_ y elija **Administrar secretos de usuario**. Se abrirá un archivo denominado _secrets.json_. Reemplace el contenido de ese archivo por lo siguiente, y proporcione la clave de suscripción y el punto de conexión donde sea adecuado.

```json
{
  "SubscriptionKey": YOUR_SUBSCRIPTION_KEY,
  "Endpoint": YOUR_ENDPOINT
}
```

Abra _Controllers\HomeController.cs_ y reemplace la clase `HomeController` por el código siguiente.

```csharp
public class HomeController : Controller
{
    private readonly string SubscriptionKey;
    private readonly string Endpoint;

    public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
    {
        SubscriptionKey = configuration["SubscriptionKey"];
        Endpoint = configuration["Endpoint"];

        if (string.IsNullOrEmpty(Endpoint) || string.IsNullOrEmpty(SubscriptionKey))
        {
            throw new ArgumentNullException("Endpoint or subscriptionKey is null!");
        }
    }

    public IActionResult Index()
    {
        return View();
    }

    [Route("token")]
    public async Task<string> Token()
    {
        return await GetTokenAsync();
    }

    /// <summary>
    /// Exchange your Azure subscription key for an access token
    /// </summary>
    private async Task<string> GetTokenAsync()
    {
        using (var client = new System.Net.Http.HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", SubscriptionKey);
            using (var response = await client.PostAsync(Endpoint, null))
            {
                return await response.Content.ReadAsStringAsync();
            }
        }
    }
}
```

## <a name="add-sample-content"></a>Adición de contenido de ejemplo

Ahora, vamos a agregar contenido de ejemplo a esta aplicación web. Abra _Views\Home\Index.cshtml_ y reemplace el código generado automáticamente por este ejemplo:

```html
<h1 id='title'>Geography</h1>
<span id='content'>
    <p>The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers. Landforms are sometimes called physical features. It is important for students to know about the physical geography of Earth. The seasons, the atmosphere and all the natural processes of Earth affect where people are able to live. Geography is one of a combination of factors that people use to decide where they want to live.</p>
</span>

<div class='immersive-reader-button' data-button-style='iconAndText' onclick='launchImmersiveReader()'></div>

@section scripts {
<script type='text/javascript' src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js'></script>
<script type='text/javascript' src='https://code.jquery.com/jquery-3.3.1.min.js'></script>
<script type='text/javascript'>
    function getImmersiveReaderTokenAsync() {
        return new Promise((resolve) => {
            $.ajax({
                url: '/token',
                type: 'GET',
                success: token => {
                    resolve(token);
                }
            });
        });
    }

    async function launchImmersiveReader() {
        const content = {
            title: document.getElementById('title').innerText,
            chunks: [ {
                content: document.getElementById('content').innerText,
                lang: 'en'
            } ]
        };

        const token = await getImmersiveReaderTokenAsync();
        ImmersiveReader.launchAsync(token, content, { uiZIndex: 1000000 });
    }
</script>
}
```

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

En la barra de menús, seleccione **Depurar > Iniciar depuración** o presione **F5** para iniciar la depuración.

En el explorador, verá:

![Aplicación de ejemplo](./media/quickstart-result.png)

Al hacer clic en el botón "Lector inmersivo", verá que se inicia dicha herramienta con el contenido de la página.

![Lector inmersivo](./media/quickstart-immersive-reader.png)

## <a name="next-steps"></a>Pasos siguientes

* Ver el [tutorial](./tutorial.md) para consultar qué más puede hacer con el SDK del Lector inmersivo
* Explorar el [SDK del Lector inmersivo](https://github.com/Microsoft/immersive-reader-sdk) y agregar la [Referencia del SDK del Lector inmersivo](./reference.md)
