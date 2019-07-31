---
title: Aserciones de cliente en la biblioteca de autenticación de Microsoft para .NET | Azure
description: Obtenga información sobre la compatibilidad con las aserciones de cliente para aplicaciones cliente confidenciales en la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
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
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a938f85b2047ea5cceada98df6adba2c560c1a1
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278661"
---
# <a name="confidential-client-assertions"></a>Aserciones de cliente confidenciales
Con el fin de demostrar su identidad, las aplicaciones cliente confidenciales intercambian un secreto con Azure AD. El secreto puede ser:
- un secreto de cliente (contraseña de aplicación).
- un certificado, que se usa para crear una aserción firmada que contiene notificaciones estándar.

Este secreto también puede ser una aserción firmada directamente.

MSAL.NET tiene cuatro métodos para proporcionar credenciales o aserciones a la aplicación cliente confidencial:
- `.WithClientSecret()`
- `.WithCertificate()`
- `.WithSignedAssertion()`
- `.WithClientClaims()`

### <a name="signed-assertions"></a>Aserciones firmadas

Una aserción de cliente firmada adopta la forma de un JWT firmado con la carga que contiene las notificaciones de autenticación necesarias impuestas por Azure AD, codificado en Base64. Para usarla:

```CSharp
string signedClientAssertion = ComputeAssertion();
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientAssertion(signedClientAssertion)
                                          .Build();
```

Las notificaciones esperadas por Azure AD son las siguientes:

Tipo de notificación | Valor | DESCRIPCIÓN
---------- | ---------- | ----------
aud | https://login.microsoftonline.com/{tenantId}/v2.0 | La notificación "aud" (audiencia) identifica los destinatarios para los que está previsto el JWT (en este caso Azure AD) Vea [RFC 7519, sección 4.1.3]
exp | Jueves 27 de junio de 2019 15:04:17 GMT+0200 (Hora de verano romance) | La notificación "exp" (fecha de expiración) identifica la hora de expiración en la que o después de la que el token JWT no debe ser aceptado para su procesamiento. Vea [RFC 7519, Sección 4.1.4]
iss | {ClientID} | La notificación "iss" (emisor) identifica la entidad de seguridad que ha emitido el JWT. El procesamiento de esta notificación es específico de la aplicación. El valor "iss" es una cadena que distingue mayúsculas de minúsculas y que contiene un valor StringOrURI. [RFC 7519, Sección 4.1.1]
jti | (un GUID) | La notificación "jti" (Id. de JWT) proporciona un identificador único para el JWT. El valor del identificador se DEBE asignar de forma que se garantice que hay una probabilidad insignificante de que el mismo valor se asigne por accidente a otro objeto de datos; si en la aplicación se usan varios emisores, también se DEBEN evitar las colisiones entre los valores generados por otros emisores. La notificación "jti" se puede usar para impedir que se vuelva a reproducir el JWT. El valor "jti" es una cadena que distingue mayúsculas de minúsculas. [RFC 7519, Sección 4.1.7]
nbf | Jueves 27 de junio de 2019 14:54:17 GMT+0200 (Hora de verano romance) | La notificación "nbf" (no antes de) identifica la hora antes de la cual no debe ser aceptado el token JWT para su procesamiento. [RFC 7519, Sección 4.1.5]
sub | {ClientID} | La notificación "sub" (asunto) identifica el asunto del JWT. Las notificaciones en un JWT normalmente son instrucciones sobre el asunto. El ámbito del valor de asunto DEBE ser único localmente en el contexto del emisor o ser único globalmente. Vea [RFC 7519, Sección 4.1.2]

Este es un ejemplo de cómo crear estas notificaciones:

```CSharp
private static IDictionary<string, string> GetClaims()
{
      //aud = https://login.microsoftonline.com/ + Tenant ID + /v2.0
      string aud = "https://login.microsoftonline.com/72f988bf-86f1-41af-hd4m-2d7cd011db47/v2.0";

      string ConfidentialClientID = "61dab2ba-145d-4b1b-8569-bf4b9aed5dhb" //client id
      const uint JwtToAadLifetimeInSeconds = 60 * 10; // Ten minutes
      DateTime validFrom = DateTime.UtcNow;
      var nbf = ConvertToTimeT(validFrom);
      var exp = ConvertToTimeT(validFrom + TimeSpan.FromSeconds(JwtToAadLifetimeInSeconds));

      return new Dictionary<string, string>()
           {
                { "aud", aud },
                { "exp", exp.ToString() },
                { "iss", ConfidentialClientID },
                { "jti", Guid.NewGuid().ToString() },
                { "nbf", nbf.ToString() },
                { "sub", ConfidentialClientID }
            };
}
```

Aquí se muestra cómo crear una aserción de cliente firmada:

```CSharp
string Encode(byte[] arg)
{
    char Base64PadCharacter = '=';
    char Base64Character62 = '+';
    char Base64Character63 = '/';
    char Base64UrlCharacter62 = '-';
    char Base64UrlCharacter63 = '_';

    string s = Convert.ToBase64String(arg);
    s = s.Split(Base64PadCharacter)[0]; // RemoveAccount any trailing padding
    s = s.Replace(Base64Character62, Base64UrlCharacter62); // 62nd char of encoding
    s = s.Replace(Base64Character63, Base64UrlCharacter63); // 63rd char of encoding

    return s;
}

string GetAssertion()
{
    //Signing with SHA-256
    string rsaSha256Signature = "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256";
    X509Certificate2 certificate = ReadCertificate(config.CertificateName);

    //Create RSACryptoServiceProvider
    var x509Key = new X509AsymmetricSecurityKey(certificate);
    var privateKeyXmlParams = certificate.PrivateKey.ToXmlString(true);
    var rsa = new RSACryptoServiceProvider();
    rsa.FromXmlString(privateKeyXmlParams);

    //alg represents the desired signing algorithm, which is SHA-256 in this case
    //kid represents the certificate thumbprint
    var header = new Dictionary<string, string>()
         {
              { "alg", "RS256"},
              { "kid", Encode(Certificate.GetCertHash()) }
         };

    //Please see the previous code snippet on how to craft claims for the GetClaims() method
    string token = Encode(Encoding.UTF8.GetBytes(JObject.FromObject(header).ToString())) + "." + Encode(Encoding.UTF8.GetBytes(JObject.FromObject(GetClaims())));

    string signature = Encode(rsa.SignData(Encoding.UTF8.GetBytes(token), new SHA256Cng()));
    string SignedAssertion = string.Concat(token, ".", signature);
    return SignedAssertion;
}
```

### <a name="withclientclaims"></a>WithClientClaims

De forma predeterminada, `WithClientClaims(X509Certificate2 certificate, IDictionary<string, string> claimsToSign, bool mergeWithDefaultClaims = true)` generará una aserción firmada que contiene las notificaciones esperadas por Azure AD más las notificaciones de cliente adicionales que se quieren enviar. Este es un fragmento de código sobre cómo hacerlo.

```CSharp
string ipAddress = "192.168.1.2";
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithAuthority(new Uri(config.Authority))
                                          .WithClientClaims(certificate, 
                                                                      new Dictionary<string, string> { { "client_ip", ipAddress } })
                                          .Build();

```

Si una de las notificaciones del diccionario que se pasa es la misma que una de las notificaciones obligatorias, se tendrá en cuenta el valor de la notificación adicional. Invalidará las notificaciones calculadas por MSAL.NET.

Si quiere proporcionar notificaciones propias, incluidas las obligatorias que espera Azure AD, pase `false` para el parámetro `mergeWithDefaultClaims`.
