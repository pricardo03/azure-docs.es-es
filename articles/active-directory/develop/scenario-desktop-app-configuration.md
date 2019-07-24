---
title: 'Aplicación de escritorio que llama a las API web: configuración del código - Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación de escritorio que llama a las API web (configuración del código de la aplicación)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 600b6db1eb3d3b422d62e49c5bc816a1a56370f9
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798506"
---
# <a name="desktop-app-that-calls-web-apis---code-configuration"></a>Aplicación de escritorio que llama a las API web: configuración de código

Ahora que ha creado la aplicación, aprenderá a configurar el código con las coordenadas de la aplicación.

## <a name="msal-libraries"></a>Bibliotecas MSAL

La única biblioteca MSAL que admite actualmente aplicaciones de escritorio es MSAL.NET

## <a name="public-client-application"></a>Aplicación cliente pública

Desde un punto de vista de código, las aplicaciones de escritorio son aplicaciones cliente públicas, por eso compilará y manipulará MSAL.NET `IPublicClientApplication`. De nuevo, el proceso diferirá un poco si se utiliza o no la autenticación interactiva.

![IPublicClientApplication](media/scenarios/public-client-application.png)

### <a name="exclusively-by-code"></a>Exclusivamente mediante código

El código siguiente crea una instancia de una aplicación cliente pública e inicia la sesión de los usuarios en la nube pública de Microsoft Azure con una cuenta profesional o educativa o bien con una cuenta Microsoft personal.

```CSharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

Si piensa utilizar la autenticación interactiva o el flujo de código de dispositivo, tal como se muestra anteriormente, querrá utilizar el modificador `.WithRedirectUri`:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="using-configuration-files"></a>Uso de archivos de configuración

El código siguiente crea una instancia de una aplicación cliente pública a partir de un objeto de configuración, que podría rellenarse mediante programación o leerse desde un archivo de configuración:

```CSharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="more-elaborated-configuration"></a>Configuración más elaborada

Puede elaborar la compilación de la aplicación agregando una serie de modificadores. Por ejemplo, si quiere que la aplicación sea una aplicación de varios inquilinos en una nube nacional (en la Administración Pública de Estados Unidos), podría escribir lo siguiente:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment,
                         AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

MSAL.NET también contiene un modificador de ADFS 2019:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Por último, si quiere adquirir tokens para un inquilino de Azure AD B2C, puede especificar el inquilino tal como se muestra en el siguiente fragmento de código:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```

### <a name="learn-more"></a>Más información

Para más información sobre cómo configurar una aplicación de escritorio de MSAL.NET:

- Para obtener la lista de todos los modificadores disponibles en `PublicClientApplicationBuilder`, consulte la documentación de referencia [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods).
- Para obtener la descripción de todas las opciones que se muestran en `PublicClientApplicationOptions`, vea [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions) en la documentación de referencia.

## <a name="complete-example-with-configuration-options"></a>Ejemplo completo con opciones de configuración

Imagine una aplicación de consola .NET Core que tiene el siguiente archivo de configuración `appsettings.json`:

```JSon
{
  "Authentication": {
    "AzureCloudInstance": "AzurePublic",
    "AadAuthorityAudience": "AzureAdMultipleOrgs",
    "ClientId": "ebe2ab4d-12b3-4446-8480-5c3828d04c50"
  },

  "WebAPI": {
    "MicrosoftGraphBaseEndpoint": "https://graph.microsoft.com"
  }
}
```

Tiene poco código para leer este archivo mediante el marco de configuración de .NET proporcionado;

```CSharp
public class SampleConfiguration
{
 /// <summary>
 /// Authentication options
 /// </summary>
 public PublicClientApplicationOptions PublicClientApplicationOptions { get; set; }

 /// <summary>
 /// Base URL for Microsoft Graph (it varies depending on whether the application is ran
 /// in Microsoft Azure public clouds or national / sovereign clouds
 /// </summary>
 public string MicrosoftGraphBaseEndpoint { get; set; }

 /// <summary>
 /// Reads the configuration from a json file
 /// </summary>
 /// <param name="path">Path to the configuration json file</param>
 /// <returns>SampleConfiguration as read from the json file</returns>
 public static SampleConfiguration ReadFromJsonFile(string path)
 {
  // .NET configuration
  IConfigurationRoot Configuration;
  var builder = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile(path);
  Configuration = builder.Build();

  // Read the auth and graph endpoint config
  SampleConfiguration config = new SampleConfiguration()
  {
   PublicClientApplicationOptions = new PublicClientApplicationOptions()
  };
  Configuration.Bind("Authentication", config.PublicClientApplicationOptions);
  config.MicrosoftGraphBaseEndpoint =
  Configuration.GetValue<string>("WebAPI:MicrosoftGraphBaseEndpoint");
  return config;
 }
}
```

Ahora, para crear la aplicación, basta con que escriba el código siguiente:

```CSharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .WithDefaultRedirectUri()
           .Build();
```

y antes de llamar al método `.Build()`, puede invalidar la configuración con llamadas a los métodos `.WithXXX`, tal como se mostró anteriormente.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquisición de un token para una aplicación de escritorio](scenario-desktop-acquire-token.md)
