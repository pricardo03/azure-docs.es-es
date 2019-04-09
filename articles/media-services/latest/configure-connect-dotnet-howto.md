---
title: Conectarse a Azure Media Services v3 API - .NET
description: Obtenga información sobre cómo conectarse a la API de Media Services v3 con. NET.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2019
ms.author: juliako
ms.openlocfilehash: cb6826be8f6a9a8caf54a23b0dbc5f3dda7830d6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59273981"
---
# <a name="connect-to-media-services-v3-api---net"></a>Conectarse a la API de Media Services v3 - .NET

Este artículo muestra cómo conectar con el SDK de .NET de Azure Media Services v3 mediante el método de inicio de sesión principal de servicio.

## <a name="prerequisites"></a>Requisitos previos

- [Cree una cuenta de Media Services](create-account-cli-how-to.md). Asegúrese de recordar el nombre del grupo de recursos y el nombre de cuenta de Media Services
- Instale una herramienta que le gustaría usar para el desarrollo. NET. Los pasos descritos en este artículo muestran cómo usar [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Puede usar Visual Studio Code, consulte [trabajar con C# ](https://code.visualstudio.com/docs/languages/csharp). O bien, puede usar un editor de código diferente.

## <a name="create-a-console-application"></a>Creación de una aplicación de consola

1. Inicie Visual Studio. 
1. Desde el **archivo** menú, haga clic en **New** > **proyecto**. 
1. Crear un **.NET Core** aplicación de consola.

Tiene como destino de la aplicación de ejemplo en este tema, `netcoreapp2.0`. El código usa 'async main', que está disponible a partir de C# 7.1. Vea este [blog](https://blogs.msdn.microsoft.com/benwilli/2017/12/08/async-main-is-available-but-hidden/) para obtener más detalles.

## <a name="add-required-nuget-packages"></a>Agregar paquetes de NuGet necesarios

1. En Visual Studio, seleccione **herramientas** > **Administrador de paquetes de NuGet** > **consola de administrador de NuGet**.
2. En el **Package Manager Console** ventana, utilice `Install-Package` comando para agregar los siguientes paquetes de NuGet. Por ejemplo, `Install-Package Microsoft.Azure.Management.Media`.

|Paquete|DESCRIPCIÓN|
|---|---|
|`Microsoft.Azure.Management.Media`|SDK de Azure Media Services. <br/>Para asegurarse de que usa el paquete más reciente de Azure Media Services, consulte [Microsoft.Azure.Management.Media](https://www.nuget.org/packages/Microsoft.Azure.Management.Media).|
|`Microsoft.Rest.ClientRuntime.Azure.Authentication`|Biblioteca de autenticación de ADAL para Azure SDK para NET|
|`Microsoft.Extensions.Configuration.EnvironmentVariables`|Leer los valores de configuración de las variables de entorno y los archivos JSON locales|
|`Microsoft.Extensions.Configuration.Json`|Leer los valores de configuración de las variables de entorno y los archivos JSON locales
|`WindowsAzure.Storage`|SDK de Storage|

## <a name="create-and-configure-the-app-settings-file"></a>Crear y configurar el archivo de configuración de aplicación

### <a name="create-appsettingsjson"></a>Crear appsettings.json

1. Ir Ir **General** > **archivo de texto**.
1. Asígnele el nombre "appsettings.json".
1. Establezca la propiedad "Copy to Output Directory" del archivo .json en "Copiar si es posterior" (para que la aplicación pueda tener acceso a ella cuando publica).

### <a name="set-values-in-appsettingsjson"></a>Valores del conjunto de appsettings.json

Ejecute el `az ams account sp create` comando tal como se describe en [acceder a las API](access-api-cli-how-to.md). El comando devuelve json que se debe copiar en su "appsettings.json".
 
## <a name="add-configuration-file"></a>Adición de archivo de configuración

Para mayor comodidad, agregue un archivo de configuración que se encarga de leer los valores de "appsettings.json".

1. Agregue una nueva clase de .cs a su proyecto. Asígnele el nombre `ConfigWrapper`. 
1. Pegue el código siguiente en este archivo (en este ejemplo se supone que tiene el espacio de nombres es `ConsoleApp1`).

```csharp
using System;

using Microsoft.Extensions.Configuration;

namespace ConsoleApp1
{
    public class ConfigWrapper
    {
        private readonly IConfiguration _config;

        public ConfigWrapper(IConfiguration config)
        {
            _config = config;
        }

        public string SubscriptionId
        {
            get { return _config["SubscriptionId"]; }
        }

        public string ResourceGroup
        {
            get { return _config["ResourceGroup"]; }
        }

        public string AccountName
        {
            get { return _config["AccountName"]; }
        }

        public string AadTenantId
        {
            get { return _config["AadTenantId"]; }
        }

        public string AadClientId
        {
            get { return _config["AadClientId"]; }
        }

        public string AadSecret
        {
            get { return _config["AadSecret"]; }
        }

        public Uri ArmAadAudience
        {
            get { return new Uri(_config["ArmAadAudience"]); }
        }

        public Uri AadEndpoint
        {
            get { return new Uri(_config["AadEndpoint"]); }
        }

        public Uri ArmEndpoint
        {
            get { return new Uri(_config["ArmEndpoint"]); }
        }

        public string Region
        {
            get { return _config["Region"]; }
        }
    }
}
```

## <a name="connect-to-the-net-client"></a>Conectar con el cliente de .NET

Para empezar a usar las API de Media Services con. NET, debe crear un objeto **AzureMediaServicesClient**. Para crear el objeto, debe proporcionar las credenciales necesarias para que el cliente se conecte a Azure mediante Azure AD. En el código siguiente, la función GetCredentialsAsync crea el objeto de ServiceClientCredentials según las credenciales proporcionadas en el archivo de configuración local.

1. Abra `Program.cs`.
1. Pegue el código siguiente:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;

using Microsoft.Azure.Management.Media;
using Microsoft.Azure.Management.Media.Models;
using Microsoft.Extensions.Configuration;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;

namespace ConsoleApp1
{
    class Program
    {
        public static async Task Main(string[] args)
        {
            
            ConfigWrapper config = new ConfigWrapper(new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                .AddEnvironmentVariables()
                .Build());

            try
            {
                IAzureMediaServicesClient client = await CreateMediaServicesClientAsync(config);
                Console.WriteLine("connected");
            }
            catch (Exception exception)
            {
                if (exception.Source.Contains("ActiveDirectory"))
                {
                    Console.Error.WriteLine("TIP: Make sure that you have filled out the appsettings.json file before running this sample.");
                }

                Console.Error.WriteLine($"{exception.Message}");

                ApiErrorException apiException = exception.GetBaseException() as ApiErrorException;
                if (apiException != null)
                {
                    Console.Error.WriteLine(
                        $"ERROR: API call failed with error code '{apiException.Body.Error.Code}' and message '{apiException.Body.Error.Message}'.");
                }
            }

            Console.WriteLine("Press Enter to continue.");
            Console.ReadLine();
        }
 
        private static async Task<ServiceClientCredentials> GetCredentialsAsync(ConfigWrapper config)
        {
            // Use ApplicationTokenProvider.LoginSilentWithCertificateAsync or UserTokenProvider.LoginSilentAsync to get a token using service principal with certificate
            //// ClientAssertionCertificate
            //// ApplicationTokenProvider.LoginSilentWithCertificateAsync

            // Use ApplicationTokenProvider.LoginSilentAsync to get a token using a service principal with symetric key
            ClientCredential clientCredential = new ClientCredential(config.AadClientId, config.AadSecret);
            return await ApplicationTokenProvider.LoginSilentAsync(config.AadTenantId, clientCredential, ActiveDirectoryServiceSettings.Azure);
        }

        private static async Task<IAzureMediaServicesClient> CreateMediaServicesClientAsync(ConfigWrapper config)
        {
            var credentials = await GetCredentialsAsync(config);

            return new AzureMediaServicesClient(config.ArmEndpoint, credentials)
            {
                SubscriptionId = config.SubscriptionId,
            };
        }

    }
}
```

## <a name="see-also"></a>Vea también

- [Tutorial: Cargar, codificar y transmitir vídeos - .NET](stream-files-tutorial-with-api.md) 
- [Tutorial: Stream en vivo con Media Services v3 - .NET](stream-live-tutorial-with-api.md)
- [Tutorial: Análisis de vídeos con Media Services v3 - .NET](analyze-videos-tutorial-with-api.md)
- [Crear una entrada de trabajo desde un archivo local - .NET](job-input-from-local-file-how-to.md)
- [Crear una entrada de trabajo desde una dirección URL HTTPS - .NET](job-input-from-http-how-to.md)
- [Codificar con una transformación personalizada - .NET](customize-encoder-presets-how-to.md)
- [Usar cifrado dinámico AES-128 y el servicio de entrega de claves - .NET](protect-with-aes128.md)
- [Usar DRM dinámica cifrado y la licencia de servicio de entrega - .NET](protect-with-drm.md)
- [Obtener una clave de firma de la directiva existente - .NET](get-content-key-policy-dotnet-howto.md)
- [Crear filtros con Media Services: .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Ejemplos de vídeo bajo demanda de Azure Functions v2 con Media Services v3 de avanzada](https://aka.ms/ams3functions)

## <a name="next-steps"></a>Pasos siguientes

[Referencia de .NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
