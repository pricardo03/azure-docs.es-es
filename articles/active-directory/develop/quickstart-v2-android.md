---
title: Inicio rápido de la Plataforma de identidad de Microsoft para Android | Azure
description: Aprenda la forma en que las aplicaciones de Android pueden llamar a una API que requiera tokens de acceso mediante el punto de conexión de la Plataforma de identidad de Microsoft.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1f174229da565627c0e5791f53031b338880cb3
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495318"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Inicio rápido: Inicie sesión de los usuarios y llame a Microsoft Graph API desde una aplicación de Android

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido hay código de ejemplo que muestra cómo una aplicación de Android puede iniciar sesión en cuentas personales, de trabajo y educativas, obtener un token de acceso y llamar a Microsoft Graph API.

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Requisitos previos**
> * Android Studio 3+
> * Se requiere Android 21+ 


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Registro y descarga de la aplicación de inicio rápido
> Tiene dos opciones para comenzar con la aplicación de inicio rápido:
> * [Rápido] [Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Manual] [Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
> Registro de aplicaciones
> 1. Vaya al nuevo panel de [Azure Portal: Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AndroidQuickstartPage/sourceType/docs).
> 1. Escriba un nombre para la aplicación y seleccione **Registrar**.
> 1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación con un solo clic.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo
>
> #### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación
> Para registrar la aplicación y agregar la información de registro de la aplicación a la solución de forma manual, siga estos pasos:
>
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
> 1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
> 1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
> 1. Seleccione **Nuevo registro**.
> 1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
>      - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `Android-Quickstart`.
>      - Presione el botón `Register`.
> 1. Vaya a `Authentication` > `Redirect URIs` > `Suggested Redirect URIs for public clients` y seleccione el URI de redireccionamiento con el formato **msal{AppId}://auth**. Guarde el cambio.


> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>Paso 1: Configuración de la aplicación
> Para que el ejemplo de código de esta guía rápida funcione, debe agregar una dirección URL de respuesta como **msal {AppId}://auth** (donde {AppId} es el identificador de la aplicación).
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-android/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto

* [Descargue el proyecto de Android Studio](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Paso 3: Configuración del proyecto

> [!div renderon="docs"]
> Si ha seleccionado la opción 1 anterior, puede omitir estos pasos. Abra el proyecto en Android Studio y ejecute la aplicación. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Extraiga y abra el proyecto en Android Studio.
> 1. En **app** > **res** > **raw**, abra **auth_config.json**.
> 1. Edite **auth_config.json** y reemplace `client_id` y `tenant_id`:
>    ```javascript
>    "client_id" : "Enter_the_Application_Id_Here",
>    "type": "Enter_the_Audience_Info_Here",
>    "tenant_id" : "Enter_the_Tenant_Info_Here"
>    ```
> 1. En **app** > **manifests**, abra el archivo **AndroidManifest.xml**.
> 1. Agregue la siguiente actividad en el nodo **manifest\application**. Este código permite a Microsoft devolver la llamada a la aplicación:   
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
> 
>            <!--Add in your scheme/host from registered redirect URI-->
>            <!--By default, the scheme should be similar to 'msal[appId]' -->
>            <data android:scheme="msalEnter_The_Application_Id_Here"
>                android:host="auth" />
>        </intent-filter>
>    </activity>
>    ```

> [!div renderon="docs"]
> 1. Extraiga y abra el proyecto en Android Studio.
> 1. En **app** > **res** > **raw**, abra **auth_config.json**.
> 1. Edite **auth_config.json** y reemplace `client_id` y `redirect_uri`:
>    ```javascript
>    "client_id" : "ENTER_YOUR_APPLICATION_ID",
>    "redirect_uri": "ENTER_YOUR_REDIRECT_URI", 
>     ```
> 1. En **app** > **manifests**, abra el archivo **AndroidManifest.xml**.
> 1. Agregue la siguiente actividad en el nodo **manifest\application**. El fragmento de código registra una **BrowserTabActivity** para permitir que el sistema operativo reanude la aplicación después de completar la autenticación:
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
> 
>            <!--Add in your scheme/host from registered redirect URI-->
>            <!--By default, the scheme should be similar to 'msal[appId]' -->
>            <data android:scheme="msal<ENTER_YOUR_APPLICATION_ID>"
>                android:host="auth" />
>        </intent-filter>
>    </activity>
>    ```
> 1. Reemplace `<ENTER_THE_APPLICATION_ID_HERE>` con el *id. de aplicación* de su aplicación. Si necesita encontrar el *id. de aplicación*, vaya a la página *Información general*.

## <a name="more-information"></a>Más información

Lea las secciones siguientes para obtener más información acerca de esta guía de inicio rápido.

### <a name="msal"></a>MSAL

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar tokens de acceso a una API protegida por la Plataforma de identidad de Microsoft. Puede usar Gradle para instalarla si agrega lo siguiente en la sección **Dependencies** (Dependencias) de **Gradle Scripts** > **build.gradle (Module: app)**:

```gradle  
implementation 'com.android.volley:volley:1.1.1'
implementation 'com.microsoft.identity.client:msal:0.2.+'
```

### <a name="msal-initialization"></a>Inicialización de MSAL

Puede agregar la referencia de MSAL con el código siguiente:

```java
import com.microsoft.identity.client.*;
```

A continuación, realice la inicialización de MSAL con el siguiente código:

```java
    sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);
```

> |Donde: ||
> |---------|---------|
> |`R.raw.auth_config` | Este archivo contiene las configuraciones de la aplicación incluido el identificador de la aplicación o de cliente, audiencia de inicio de sesión y varias otras opciones de personalización. |

### <a name="requesting-tokens"></a>Solicitud de tokens

MSAL tiene dos métodos para adquirir tokens: `acquireToken` y `acquireTokenSilentAsync`

#### <a name="getting-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

En algunas situaciones, es necesario forzar a los usuarios a que interactúen con el punto de conexión de la Plataforma de identidad de Microsoft, lo que cambia de contexto al explorador del sistema, ya sea para validar las credenciales del usuario o para obtener su consentimiento. Estos son algunos ejemplos:

* La primera vez que los usuarios inician sesión en la aplicación
* Cuando los usuarios deben volver a escribir sus credenciales porque la contraseña expiró
* Cuando la aplicación solicita acceso a un recurso para el cual el usuario necesita dar su consentimiento
* Cuando se requiere la autenticación en dos fases

```java
sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
```

> |Donde:||
> |---------|---------|
> | `SCOPES` | Contiene los ámbitos que se solicitan (es decir, `{ "user.read" }` para Microsoft Graph o `{ "<Application ID URL>/scope" }` para las API web personalizadas (por ejemplo, `api://<Application ID>/access_as_user`) |
> | `getAuthInteractiveCallback` | Es la devolución de llamada que se ejecuta cuando la aplicación recupera el control después de la autenticación |

#### <a name="getting-a-user-token-silently"></a>Obtención de un token de usuario en silencio

No desea pedirle al usuario que valide sus credenciales cada vez que necesite acceder a un recurso. La mayor parte del tiempo, quiere que la renovación y adquisición de tokens ocurra sin la interacción del usuario. Puede usar el método `AcquireTokenSilentAsync` para obtener tokens que permiten acceder a recursos protegidos después del método `acquireToken` inicial:

```java
List<IAccount> accounts = sampleApp.getAccounts();
if (sample.size() == 1) {
    sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
} else {
    // No or multiple accounts
}
```

> |Donde:||
> |---------|---------|
> | `SCOPES` | Contiene los ámbitos que se solicitan (es decir, `{ "user.read" }` para Microsoft Graph o `{ "<Application ID URL>/scope" }` para las API web personalizadas (por ejemplo, `api://<Application ID>/access_as_user`) |
> | `accounts.get(0)` | Contiene la cuenta para la que está intentando obtener tokens de forma silenciosa |
> | `getAuthInteractiveCallback` | Es la devolución de llamada que se ejecuta cuando la aplicación recupera el control después de la autenticación |

## <a name="next-steps"></a>Pasos siguientes

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Más información sobre los pasos para crear la aplicación que se usa en esta guía de inicio rápido

Visite el tutorial de Android para acceder a una guía completa paso a paso sobre la creación de aplicaciones y nuevas características, que incluye una explicación completa de esta guía de inicio rápido.

> [!div class="nextstepaction"]
> [Tutorial para llamar a Graph API desde Android](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>Wiki de la biblioteca MSAL para Android

Obtenga más información acerca de la biblioteca MSAL para Android:

> [!div class="nextstepaction"]
> [Wiki de la biblioteca MSAL para Android](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
