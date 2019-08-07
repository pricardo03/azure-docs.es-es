---
title: 'Aplicación de demonio que llama a las API web (configuración de la aplicación): Plataforma de identidad de Microsoft'
description: Aprenda a compilar una aplicación de demonio que llama a las API web (configuración de la aplicación)
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
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 705545fd5167087be1a001c45f58907d6ff225e8
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277834"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Aplicación de demonio que llama a las API web: configuración de código

Aprenda a configurar el código para la aplicación de demonio que llama a las API web.

## <a name="msal-libraries-supporting-daemon-apps"></a>Bibliotecas MSAL que admiten aplicaciones de demonio

Las bibliotecas de Microsoft que admiten aplicaciones de demonio son:

  Biblioteca MSAL | DESCRIPCIÓN
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Las plataformas admitidas para compilar una aplicación de demonio son las plataformas .NET Framework y .NET Core (no UWP, Xamarin.iOS ni Xamarin.Android, ya que estas plataformas se usan para compilar aplicaciones cliente públicas)
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Desarrollo en curso: en versión preliminar pública
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Desarrollo en curso: en versión preliminar pública

## <a name="configuration-of-the-authority"></a>Configuración de la autoridad

Dado que las aplicaciones de demonio no usan permisos delegados, sino permisos de aplicación, sus *tipos de cuenta admitidos* no pueden ser *cuentas de ningún directorio de la organización ni cuentas Microsoft personales (por ejemplo, Skype, Xbox, Outlook.com)* . De hecho, no hay ningún administrador de inquilinos para otorgar consentimiento a la aplicación de demonio para las cuentas Microsoft personales. Debe elegir *cuentas de mi organización* o *cuentas de cualquier organización*.

Por lo tanto, la autoridad especificada en la configuración de la aplicación debe tener inquilino (con identificador o nombre de dominio asociado a la organización).

Si es un ISV y quiere ofrecer una herramienta para varios inquilinos, puede usar `organizations`. Aún así, tenga en cuenta que también deberá explicar a los clientes cómo conceder el consentimiento de administrador. Consulte [Solicitud de consentimiento para un inquilino al completo](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant) para más información. También hay actualmente una limitación en MSAL, en la que `organizations` solo se permite cuando las credenciales de cliente son un secreto de aplicación (no un certificado). Consulte [el error n.º 891 de MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/891)

## <a name="application-configuration-and-instantiation"></a>Creación de instancias y configuración de la aplicación

En las bibliotecas MSAL, las credenciales del cliente (secreto o certificado) se pasan como parámetro de la construcción de la aplicación cliente confidencial.

> [!IMPORTANT]
> Aunque la aplicación sea una aplicación de consola que se ejecute como servicio, si es una aplicación de demonio debe ser una aplicación cliente confidencial.

### <a name="msalnet"></a>MSAL.NET

Agregue el paquete NuGet [Microsoft.IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) a la aplicación.

Uso del espacio de nombres de MSAL.NET

```CSharp
using Microsoft.Identity.Client;
```

La aplicación de demonio la presentará un `IConfidentialClientApplication`

```CSharp
IConfidentialClientApplication app;
```

Este es el código para compilar una aplicación con un secreto de aplicación:

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

Este es el código para compilar una aplicación con un certificado:

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

Por último, en lugar de un secreto de cliente o un certificado, las aplicaciones cliente confidenciales también pueden demostrar su identidad mediante aserciones de cliente. Este escenario avanzado se detalla en las [aserciones de cliente](msal-net-client-assertions.md).


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
