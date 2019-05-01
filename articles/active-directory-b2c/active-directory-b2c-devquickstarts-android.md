---
title: Adquisición de un token con una aplicación Android en Azure Active Directory B2C | Microsoft Docs
description: En este artículo se mostrará cómo crear una aplicación Android que usa AppAuth con Azure Active Directory B2C para administrar las identidades de usuario y autenticar usuarios.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: acd0e9616f830d9378709e67f0d05e3ae549700d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703328"
---
# <a name="sign-in-using-an-android-application-in-azure-active-directory-b2c"></a>Inicio de sesión con una aplicación Android en Azure Active Directory B2C

La plataforma Microsoft Identity utiliza estándares abiertos como OAuth2 y OpenID Connect. Estos estándares le permiten usar cualquier biblioteca que quiera integrar con Azure Active Directory B2C. Con el fin de ayudarle a usar otras bibliotecas, hemos creado este tutorial para mostrarle cómo configurar bibliotecas de terceros para conectarse a la plataforma Microsoft Identity. La mayoría de las bibliotecas que implementan la [especificación OAuth2 RFC6749](https://tools.ietf.org/html/rfc6749) pueden conectarse a la plataforma Microsoft Identity.

> [!WARNING]
> No se proporcionan correcciones para bibliotecas de terceros y no se ha realizado ninguna revisión de esas bibliotecas. Este ejemplo usa una biblioteca de terceros llamada AppAuth cuya compatibilidad se ha probado en escenarios básicos con Azure AD B2C. Los problemas y las solicitudes de características deben dirigirse al proyecto de código abierto de la biblioteca. Vea [este](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) artículo para más información.  
>
>

Si no está familiarizado con OAuth2 o con OpenID Connect, es posible que gran parte de esta configuración de ejemplo no tenga mucho sentido para usted. Se recomienda que consulte una breve [información general sobre el protocolo que hemos documentado aquí](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Obtener un directorio de Azure AD B2C

Para poder usar Azure AD B2C, debe crear un directorio o inquilino. Un directorio es un contenedor para todos los usuarios, las aplicaciones, los grupos, etc. Si aún no tiene uno, [cree un directorio B2C](tutorial-create-tenant.md) antes de continuar.

## <a name="create-an-application"></a>Creación de una aplicación

A continuación, debe crear una aplicación en su directorio B2C. Esto proporciona a Azure AD la información que necesita para comunicarse de forma segura con la aplicación. Para crear una aplicación móvil, siga [estas instrucciones](active-directory-b2c-app-registration.md). Asegúrese de:

* Incluir un **cliente nativo** en la aplicación.
* Copiar el **identificador de aplicación** asignado a la aplicación. Lo necesitará más adelante.
* Configurar un **URI de redireccionamiento** del cliente nativo (por ejemplo, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). También lo necesitará más adelante.

## <a name="create-your-user-flows"></a>Creación de flujos de usuario

En Azure AD B2C, cada experiencia del usuario se define mediante un [flujo de usuario](active-directory-b2c-reference-policies.md), que es un conjunto de directivas que controlan el comportamiento de Azure AD. Esta aplicación requiere un flujo de usuario de inicio de sesión y registro. Cuando cree el flujo de usuario, asegúrese de hacer lo siguiente:

* Elegir el **nombre para mostrar** como atributo de registro en el flujo de usuario
* Elegir las notificaciones de aplicación de **nombre para mostrar** e **identificador de objeto** de cada flujo de usuario Puede elegir también otras notificaciones.
* Copiar el **nombre** de cada flujo de usuario después de crearlo Debe tener el prefijo `b2c_1_`.  Necesitará el nombre del flujo de usuario más adelante.

Después de haber creado los flujos de usuario, está listo para compilar la aplicación.

## <a name="download-the-sample-code"></a>Descarga del código de ejemplo

Hemos proporcionado un ejemplo de trabajo que usa AppAuth con Azure AD B2C [en GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Puede descargar el código y ejecutarlo. Puede comenzar rápidamente con su propia aplicación con su propia configuración de Azure AD B2C si sigue las instrucciones que aparecen en el archivo [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

El ejemplo es una modificación del ejemplo que proporciona [AppAuth](https://openid.github.io/AppAuth-Android/). Visite la página para más información sobre AppAuth y sus características.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Modificación de la aplicación para usar Azure AD B2C con AppAuth

> [!NOTE]
> AppAuth admite Android API 16 (Jellybean) y versiones superiores. Se recomienda usar API 23 y superior.
>

### <a name="configuration"></a>Configuración

Puede configurar la comunicación con Azure AD B2C si especifica el URI de detección o tanto los URI del punto de conexión de autorización y del punto de conexión del token. Cualquiera sea el caso, necesitará la siguiente información:

* Id. de inquilino (por ejemplo, contoso.onmicrosoft.com)
* Nombre del flujo de usuario (por ejemplo, B2C\_1\_SignUpIn)

Si elige detectar automáticamente los URI de punto de conexión de autorización y token, deberá recuperar información del URI de detección. El URI de detección se puede generar si se reemplaza Tenant\_ID y Policy\_Name en la siguiente dirección URL:

```java
String mDiscoveryURI = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

A continuación, puede adquirir los URI de punto de conexión de autenticación y token y, además, crear un objeto AuthorizationServiceConfiguration; para ello, ejecute lo siguiente:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

En lugar de usar la detección para obtener los URI de punto de conexión de autorización y token, también puede especificarlos si reemplaza Tenant\_ID y Policy\_Name en las direcciones URL que aparecen a continuación:

```java
String mAuthEndpoint = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://<Tenant_name>.b2clogin.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

Ejecute el código siguiente para crear el objeto AuthorizationServiceConfiguration:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a>Autorización

Después de configurar o recuperar una configuración de servicio de autorización, es posible construir una autorización. Para crear la solicitud, necesitará la información siguiente:

* Id. de cliente (ejemplo, 00000000-0000-0000-0000-000000000000)
* URI de redireccionamiento con un esquema personalizado (por ejemplo, com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)

Ambos elementos se deberían haber guardado cuando [registró la aplicación](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Consulte la [guía de AppAuth](https://openid.github.io/AppAuth-Android/) sobre cómo completar el resto del proceso. Si necesita comenzar rápidamente con una aplicación de trabajo, revise [nuestro ejemplo](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Siga los pasos que aparecen en el archivo [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) para escribir su propia configuración de Azure AD B2C.

