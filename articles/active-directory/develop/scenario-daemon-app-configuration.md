---
title: Daemon app que realiza la llamada API web (configuración de aplicación), plataforma Microsoft identity
description: Obtenga información sobre cómo compilar una aplicación demonio que llama a web API (configuración de aplicación)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: d8d377db827a6548c380128624c21f4ae7896aff
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075331"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Aplicación de demonio que llama a web API: configuración de código

Obtenga información sobre cómo configurar el código de la aplicación de demonio que llama a las API web.

## <a name="msal-libraries-supporting-daemon-apps"></a>Aplicaciones auxiliares de demonio de bibliotecas MSAL

Las bibliotecas de Microsoft que admiten aplicaciones de demonio son:

  Biblioteca MSAL | DESCRIPCIÓN
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Las plataformas admitidas para crear una aplicación demonio son las plataformas .NET Framework y .NET Core (no de UWP, Xamarin.iOS y Xamarin.Android como esas plataformas se usan para crear aplicaciones de cliente público)
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Desarrollo en curso: en versión preliminar pública
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Desarrollo en curso: en versión preliminar pública

## <a name="configuration-of-the-authority"></a>Configuración de la autoridad

Dado que las aplicaciones de demonio no usan permisos delegados, pero los permisos de aplicación, sus *admite el tipo de cuenta* no puede ser *Microsoft personal y cuentas en el directorio de cualquier organización de cuentas () Por ejemplo, Skype, Xbox, Outlook.com)*. De hecho, no hay ningún administrador de inquilinos para otorgar consentimiento a la aplicación de demonio de cuentas personales de Microsoft. Debe elegir *las cuentas de mi organización* o *las cuentas de cualquier organización*.

Por lo tanto, la entidad especificada en la configuración de la aplicación debe ser inquilino-ed (especificando un identificador de inquilino o un nombre de dominio asociado con su organización). Si es un ISV y desea ofrecer una herramienta de varios inquilinos, puede usar `organizations`. Pero tenga en cuenta que también deba explicar a los clientes cómo conceder consentimiento del administrador. Consulte [solicitud de consentimiento para un inquilino al completo](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant) para obtener más información

## <a name="application-configuration-and-instantiation"></a>Creación de instancias y configuración de la aplicación

En las bibliotecas MSAL, las credenciales del cliente (secreto o certificado) se pasan como un parámetro de la construcción de la aplicación cliente confidencial.

> [!IMPORTANT]
> Incluso si la aplicación es una aplicación de consola que se ejecuta como un servicio, si es una aplicación de demonio debe ser una aplicación cliente confidencial.

### <a name="msalnet"></a>MSAL.NET

Agregar el [Microsoft.IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) paquete NuGet para la aplicación.

Usar espacio de nombres MSAL.NET

```CSharp
using Microsoft.Identity.Client;
```

La aplicación de demonio se estará presentada por un `IConfidentialClientApplication`

```CSharp
IConfidentialClientApplication app;
```

Este es el código para crear una aplicación con un secreto de aplicación:

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

Este es el código para crear una aplicación con un certificado:

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

### <a name="msalpython"></a>MSAL.Python

```Python
# Create a preferably long-lived app instance which maintains a token cache.

app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache

    )
```

### <a name="msaljava"></a>MSAL.Java

```Java
PrivateKey key = getPrivateKey();
X509Certificate publicCertificate = getPublicCertificate();

// create clientCredential with public and private key
IClientCredential credential = ClientCredentialFactory.create(key, publicCertificate);

ConfidentialClientApplication cca = ConfidentialClientApplication
  .builder(CLIENT_ID, credential)
  .authority(AUTHORITY_MICROSOFT)
  .build();
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: adquisición de tokens para la aplicación](./scenario-daemon-acquire-token.md)