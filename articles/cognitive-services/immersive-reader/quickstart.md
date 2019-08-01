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
ms.openlocfilehash: 978b56f5c38a154c79544c7deb5d49396b1deff9
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688860"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-c"></a>Inicio rápido: Creación de una aplicación web que inicia el Lector inmersivo (C#)

El [Lector inmersivo](https://www.onenote.com/learningtools) es una herramienta diseñada de forma inclusiva que implementa técnicas demostradas para mejorar la comprensión lectora.

En este inicio rápido, creará una aplicación web desde cero e integrará el Lector inmersivo mediante su SDK. Se puede encontrar un completo ejemplo funcional [aquí](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/quickstart-csharp).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads)
* Un recurso de Lector inmersivo configurado para la autenticación de Azure Active Directory (Azure AD). Siga [estas instrucciones](./azure-active-directory-authentication.md) para realizar la configuración. Al configurar las propiedades del proyecto de ejemplo, necesitará algunos de los valores creados aquí. Guarde la salida de la sesión en un archivo de texto para futuras referencias.

## <a name="create-a-web-app-project"></a>Creación de un proyecto de aplicación web

Cree un proyecto en Visual Studio mediante la plantilla de aplicación web de ASP.NET Core con Modelo-Vista-Controlador integrado.

![Nuevo proyecto](./media/vswebapp.png)

![Nueva aplicación web de ASP.NET Core](./media/vsmvc.png)

## <a name="acquire-an-azure-ad-authentication-token"></a>Adquisición de un token de autenticación de Azure AD

Para esta parte, necesitará algunos valores del paso anterior de requisitos previos de configuración de la autenticación de Azure AD. Vuelva a consultar el archivo de texto que ha guardado de esa sesión.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

Haga clic con el botón derecho en el proyecto en el _Explorador de soluciones_ y elija **Administrar secretos de usuario**. Se abrirá un archivo denominado _secrets.json_. Reemplace el contenido de ese archivo por lo siguiente y proporcione los valores de propiedad personalizados anteriores.

```json
{
  "TenantId": YOUR_TENANT_ID,
  "ClientId": YOUR_CLIENT_ID,
  "ClientSecret": YOUR_CLIENT_SECRET,
  "Subdomain": YOUR_SUBDOMAIN
}
```

Abra _Controllers\HomeController.cs_ y reemplace el archivo por el código siguiente.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.IdentityModel.Clients.ActiveDirectory;

namespace QuickstartSampleWebApp.Controllers
{
    public class HomeController : Controller
    {
        private readonly string TenantId;     // Azure subscription TenantId
        private readonly string ClientId;     // Azure AD ApplicationId
        private readonly string ClientSecret; // Azure AD Application Service Principal password
        private readonly string Subdomain;    // Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')

        public HomeController(Microsoft.Extensions.Configuration.IConfiguration configuration)
        {
            TenantId = configuration["TenantId"];
            ClientId = configuration["ClientId"];
            ClientSecret = configuration["ClientSecret"];
            Subdomain = configuration["Subdomain"];

            if (string.IsNullOrWhiteSpace(TenantId))
            {
                throw new ArgumentNullException("TenantId is null! Did you add that info to secrets.json?");
            }

            if (string.IsNullOrWhiteSpace(ClientId))
            {
                throw new ArgumentNullException("ClientId is null! Did you add that info to secrets.json?");
            }

            if (string.IsNullOrWhiteSpace(ClientSecret))
            {
                throw new ArgumentNullException("ClientSecret is null! Did you add that info to secrets.json?");
            }

            if (string.IsNullOrWhiteSpace(Subdomain))
            {
                throw new ArgumentNullException("Subdomain is null! Did you add that info to secrets.json?");
            }
        }

        public IActionResult Index()
        {
            return View();
        }

        [Route("subdomain")]
        public string GetSubdomain()
        {
            return Subdomain;
        }

        [Route("token")]
        public async Task<string> GetToken()
        {
            return await GetTokenAsync();
        }

        /// <summary>
        /// Get an Azure AD authentication token
        /// </summary>
        private async Task<string> GetTokenAsync()
        {
            string authority = $"https://login.windows.net/{TenantId}";
            const string resource = "https://cognitiveservices.azure.com/";

            AuthenticationContext authContext = new AuthenticationContext(authority);
            ClientCredential clientCredential = new ClientCredential(ClientId, ClientSecret);

            AuthenticationResult authResult = await authContext.AcquireTokenAsync(resource, clientCredential);

            return authResult.AccessToken;
        }
    }
}
```

## <a name="add-the-microsoftidentitymodelclientsactivedirectory-nuget-package"></a>Adición del paquete NuGet Microsoft.IdentityModel.Clients.ActiveDirectory

El código anterior usa objetos del paquete NuGet **Microsoft.IdentityModel.Clients.ActiveDirectory**, por lo que tendrá que agregar una referencia a ese paquete en el proyecto.

Abra la Consola del Administrador de paquetes NuGet en **Herramientas -> Administrador de paquetes NuGet -> Consola del Administrador de paquetes** y escriba lo siguiente:

```powershell
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 5.1.0
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
    <script type='text/javascript' src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.2.js'></script>
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

    function getSubdomainAsync() {
        return new Promise((resolve) => {
            $.ajax({
                url: '/subdomain',
                type: 'GET',
                success: subdomain => {
                    resolve(subdomain);
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
        var subdomain = await getSubdomainAsync();

        ImmersiveReader.launchAsync(token, subdomain, content, { uiZIndex: 1000000 });
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
* Explorar el [SDK del Lector inmersivo](https://github.com/microsoft/immersive-reader-sdk) y agregar la [Referencia del SDK del Lector inmersivo](./reference.md)
