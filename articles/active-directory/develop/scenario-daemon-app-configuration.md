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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32fbd4af78e02dad2a8a74ee21f9cb8c6ef0a976
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175497"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Aplicación de demonio que llama a las API web: configuración de código

Aprenda a configurar el código para la aplicación de demonio que llama a las API web.

## <a name="msal-libraries-supporting-daemon-apps"></a>Bibliotecas MSAL que admiten aplicaciones de demonio

Las bibliotecas de Microsoft que admiten aplicaciones de demonio son:

  Biblioteca MSAL | DESCRIPCIÓN
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Las plataformas admitidas para compilar una aplicación de demonio son las plataformas .NET Framework y .NET Core (no UWP, Xamarin.iOS ni Xamarin.Android, ya que estas plataformas se usan para compilar aplicaciones cliente públicas)
  ![Python](media/sample-v2-code/logo_python.png) <br/> Python de MSAL | Desarrollo en curso: en versión preliminar pública
  ![Java](media/sample-v2-code/logo_java.png) <br/> Java de MSAL | Desarrollo en curso: en versión preliminar pública

## <a name="configuration-of-the-authority"></a>Configuración de la autoridad

Dado que las aplicaciones de demonio no usan permisos delegados, sino permisos de aplicación, sus *tipos de cuenta admitidos* no pueden ser *cuentas de ningún directorio de la organización ni cuentas Microsoft personales (por ejemplo, Skype, Xbox, Outlook.com)* . De hecho, no hay ningún administrador de inquilinos para otorgar consentimiento a la aplicación de demonio para las cuentas Microsoft personales. Debe elegir *cuentas de mi organización* o *cuentas de cualquier organización*.

Por lo tanto, la autoridad especificada en la configuración de la aplicación debe tener inquilino (con identificador o nombre de dominio asociado a la organización).

Si es un ISV y quiere ofrecer una herramienta para varios inquilinos, puede usar `organizations`. Aún así, tenga en cuenta que también deberá explicar a los clientes cómo conceder el consentimiento de administrador. Consulte [Solicitud de consentimiento para un inquilino al completo](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant) para más información. También hay actualmente una limitación en MSAL: `organizations` solo se permite cuando las credenciales de cliente son un secreto de aplicación (no un certificado).

## <a name="application-configuration-and-instantiation"></a>Creación de instancias y configuración de la aplicación

En las bibliotecas MSAL, las credenciales del cliente (secreto o certificado) se pasan como parámetro de la construcción de la aplicación cliente confidencial.

> [!IMPORTANT]
> Aunque la aplicación sea una aplicación de consola que se ejecute como servicio, si es una aplicación de demonio debe ser una aplicación cliente confidencial.

### <a name="configuration-file"></a>Archivo de configuración

El archivo de configuración define:

- la autoridad o la instancia de nube y el tenantId
- el ClientID que recibió del registro de la aplicación
- un secreto de cliente o un certificado

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

[appsettings. json](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/master/1-Call-MSGraph/daemon-console/appsettings.json) del ejemplo de [demonio de consola de .NET Core](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2).

```JSon
{
  "Instance": "https://login.microsoftonline.com/{0}",
  "Tenant": "[Enter here the tenantID or domain name for your Azure AD tenant]",
  "ClientId": "[Enter here the ClientId for your application]",
  "ClientSecret": "[Enter here a client secret for your application]",
  "CertificateName": "[Or instead of client secret: Enter here the name of a certificate (from the user cert store) as registered with your application]"
}
```

Puede proporcionar un valor de clientSecret o de certificateName. Ambos valores son exclusivos.

# <a name="pythontabpython"></a>[Python](#tab/python)

Al crear un cliente confidencial con secretos de cliente, el archivo de configuración [parameters.json](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/1-Call-MsGraph-WithSecret/parameters.json) en el ejemplo de [demonio de Python](https://github.com/Azure-Samples/ms-identity-python-daemon) es el siguiente.

```Json
{
  "authority": "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here",
  "client_id": "your_client_id",
  "scope": [ "https://graph.microsoft.com/.default" ],
  "secret": "The secret generated by AAD during your confidential app registration",
  "endpoint": "https://graph.microsoft.com/v1.0/users"
}
```

Al crear un cliente confidencial con certificados, el archivo de configuración [parameters.json](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/2-Call-MsGraph-WithCertificate/parameters.json) en el ejemplo de [demonio de Python](https://github.com/Azure-Samples/ms-identity-python-daemon) es el siguiente.

```Json
{
  "authority": "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here",
  "client_id": "your_client_id",
  "scope": [ "https://graph.microsoft.com/.default" ],
  "thumbprint": "790E... The thumbprint generated by AAD when you upload your public cert",
  "private_key_file": "server.pem",
  "endpoint": "https://graph.microsoft.com/v1.0/users"
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

Esta es la clase que se usa en los ejemplos de desarrollo de Java de MSAL para configurar los ejemplos: [TestData](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/public-client/TestData.java).

```Java
public class TestData {

    final static String TENANT_SPECIFIC_AUTHORITY = "https://login.microsoftonline.com/<TenantId>/";
    final static String GRAPH_DEFAULT_SCOPE = "https://graph.microsoft.com/.default";
    final static String CONFIDENTIAL_CLIENT_ID = "";
    final static String CONFIDENTIAL_CLIENT_SECRET = "";
}
```

---

### <a name="instantiation-of-the-msal-application"></a>Creación de instancias de la aplicación MSAL

Para crear una instancia de la aplicación MSAL, tiene que hacer lo siguiente:

- agregar, hacer referencia o importar el paquete MSAL (en función del idioma)
- Después, la construcción es diferente dependiendo de si usa secretos de cliente o certificados (o, como un escenario avanzado, aserciones firmadas)

#### <a name="reference-the-package"></a>Haga referencia al paquete

Haga referencia al paquete MSAL en el código de la aplicación.

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

Agregue el paquete NuGet [Microsoft.IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) a la aplicación.
En MSAL.NET, la aplicación cliente confidencial está representada por la interfaz `IConfidentialClientApplication`.
Use el espacio de nombres MSAL.NET en el código fuente

```CSharp
using Microsoft.Identity.Client;
IConfidentialClientApplication app;
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```python
import msal
```

# <a name="javatabjava"></a>[Java](#tab/java)

```java
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
```

---

#### <a name="instantiate-the-confidential-client-application-with-client-secrets"></a>Creación de una instancia de aplicación cliente confidencial con secretos de cliente

Este es el código para crear una instancia de aplicación cliente confidencial con un secreto de cliente:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance which maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="javatabjava"></a>[Java](#tab/java)

```Java
ConfidentialClientApplication app = ConfidentialClientApplication.builder(
        TestData.CONFIDENTIAL_CLIENT_ID,
        ClientCredentialFactory.create(TestData.CONFIDENTIAL_CLIENT_SECRET))
        .authority(TestData.TENANT_SPECIFIC_AUTHORITY)
        .build();
```

---

#### <a name="instantiate-the-confidential-client-application-with-client-certificate"></a>Creación de una instancia de aplicación cliente confidencial con certificado de cliente

Este es el código para compilar una aplicación con un certificado:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance which maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential={"thumbprint": config["thumbprint"], "private_key": open(config['private_key_file']).read()},
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="javatabjava"></a>[Java](#tab/java)

En MSAL.Java hay dos generadores para crear una instancia de la aplicación cliente confidencial con certificados:

```Java

InputStream pkcs12Certificate = ... ; /* containing PCKS12 formatted certificate*/
string certificatePassword = ... ;    /* contains the password to access the certificate */

ConfidentialClientApplication app = ConfidentialClientApplication.builder(
        TestData.CONFIDENTIAL_CLIENT_ID,
        ClientCredentialFactory.create(pkcs12Certificate, certificatePassword))
        .authority(TestData.TENANT_SPECIFIC_AUTHORITY)
        .build();
```

o

```Java
PrivateKey key = getPrivateKey(); /* RSA private key to sign the assertion */
X509Certificate publicCertificate = getPublicCertificate(); /* x509 public certificate used as a thumbprint */

ConfidentialClientApplication app = ConfidentialClientApplication.builder(
        TestData.CONFIDENTIAL_CLIENT_ID,
        ClientCredentialFactory.create(rsaPrivateKey, publicKeyCertificate))
        .authority(TestData.TENANT_SPECIFIC_AUTHORITY)
        .build();
```

---

#### <a name="advanced-scenario---instantiate-the-confidential-client-application-with-client-assertions"></a>Escenario avanzado: creación de una instancia de aplicación cliente confidencial con aserciones de cliente

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

En lugar de un secreto de cliente o un certificado, la aplicación cliente confidencial también puede demostrar su identidad mediante aserciones de cliente.

MSAL.NET tiene dos métodos para proporcionar aserciones firmadas a la aplicación cliente confidencial:

- `.WithClientAssertion()`
- `.WithClientClaims()`

Cuando use `WithClientAssertion`, tiene que proporcionar un JWT firmado. Este escenario avanzado se detalla en las [aserciones de cliente](msal-net-client-assertions.md).

```CSharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Cuando utilice `WithClientClaims`, MSAL.NET calculará una aserción firmada que contiene las notificaciones esperadas por Azure AD más las notificaciones de cliente adicionales que se quieren enviar.
Este es un fragmento de código sobre cómo hacerlo:

```CSharp
string ipAddress = "192.168.1.2";
var claims = new Dictionary<string, string> { { "client_ip", ipAddress } };
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, claims)
                                          .Build();```
```

De nuevo, para una información más detallada, consulte [aserciones de cliente](msal-net-client-assertions.md).

# <a name="pythontabpython"></a>[Python](#tab/python)

En MSAL Python, puede proporcionar notificaciones de cliente mediante las notificaciones que se firmarán con esta clave privada de `ConfidentialClientApplication`.

```Python
config = json.load(open(sys.argv[1]))

# Create a preferably long-lived app instance which maintains a token cache.
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential={"thumbprint": config["thumbprint"], "private_key": open(config['private_key_file']).read()},
    client_claims = {"client_ip": "x.x.x.x"}
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

Para más información, consulte la documentación de referencia de Python de MSAL para [ConfidentialClientApplication](https://msal-python.readthedocs.io/en/latest/#msal.ClientApplication.__init__).

# <a name="javatabjava"></a>[Java](#tab/java)

Java de MSAL está en versión preliminar pública. Todavía no se admiten aserciones firmadas.

---

## <a name="next-steps"></a>Pasos siguientes

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Aplicación de demonio: adquisición de tokens para la aplicación](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-acquire-token?tabs=dotnet)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Aplicación de demonio: adquisición de tokens para la aplicación](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-acquire-token?tabs=python)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Aplicación de demonio: adquisición de tokens para la aplicación](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-acquire-token?tabs=java)

---
