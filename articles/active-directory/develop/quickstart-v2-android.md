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
ms.date: 04/26/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45252cc4d45e96c2bde4a4600630ea578a8d3009
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64946725"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Inicio rápido: Inicie sesión de los usuarios y llame a Microsoft Graph API desde una aplicación de Android

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido hay código de ejemplo que muestra cómo una aplicación de Android puede iniciar sesión en cuentas personales, de trabajo y educativas, obtener un token de acceso y llamar a Microsoft Graph API.

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Requisitos previos**
> * Android Studio 
> * Se requiere Android 16+ 


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
> 1. Vaya a la página [Registros de aplicaciones](https://aka.ms/MobileAppReg) de la plataforma de identidad de Microsoft para desarrolladores.
> 1. Seleccione **Nuevo registro**.
> 1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
>      - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `AndroidQuickstart`.
>      - Puede omitir otras configuraciones en esta página. 
>      - Presione el botón `Register`.
> 1. Haga clic en la nueva aplicación y vaya a `Authentication` > `Add Platform` > `Android`.    
>      - Escriba el nombre del paquete del proyecto de Android Studio. 
>      - Genere un hash de firma. Consulte el portal para obtener instrucciones.
> 1. Seleccione `Configure` y guarde el archivo JSON de la ***Configuración de MSAL*** para su uso posterior. 

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>Paso 1: Configuración de la aplicación
> Para que el código de ejemplo de esta guía de inicio rápido funcione, debe agregar un identificador URI de redirección compatible con el agente de autenticación. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-android/green-check.png) La aplicación está configurada con estos atributos

#### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto

* [Descargar el código de ejemplo](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Paso 3: Configuración del proyecto

> [!div renderon="docs"]
> Si ha seleccionado la opción 1 anterior, puede omitir estos pasos. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Extraiga y abra el proyecto en Android Studio.
> 1. En **app** > **src** > **main** > **res** > **raw**, abra **auth_config.json**.
> 1. Edite el archivo **auth_config.json** y reemplácelo por el archivo JSON de Azure Portal. Si, en su lugar, desea realizar los cambios manualmente:
>    ```javascript
>    {
>       "client_id" : "Enter_the_Application_Id_Here",
>       "authorization_user_agent" : "DEFAULT",
>       "redirect_uri" : "Enter_the_Redirect_Uri_Here",
>       "authorities" : [
>          {
>             "type": "AAD",
>             "audience": {
>                "type": "Enter_the_Audience_Info_Here",
>                "tenant_id": "Enter_the_Tenant_Info_Here"
>             }
>          }
>       ]
>    }
> 1. Inside **app** > **manifests**, open  **AndroidManifest.xml**.
> 1. Paste the following activity to the **manifest\application** node: 
>    ```xml
>    <!--Intent filter to catch Microsoft's callback after Sign In-->
>    <activity
>        android:name="com.microsoft.identity.client.BrowserTabActivity">
>        <intent-filter>
>            <action android:name="android.intent.action.VIEW" />
>            <category android:name="android.intent.category.DEFAULT" />
>            <category android:name="android.intent.category.BROWSABLE" />
>            <data android:scheme="msauth"
>                android:host="Enter_the_Package_Name"
>                android:path="/Enter_the_Signature_Hash" />
>        </intent-filter>
>    </activity>
>    ```
> > 1. Ejecute la aplicación. 

> [!div renderon="docs"]
> 1. Extraiga y abra el proyecto en Android Studio.
> 1. En **app** > **res** > **raw**, abra **auth_config.json**.
> 1. Edite el archivo **auth_config.json** y reemplácelo por el archivo JSON de Azure Portal. Si, en su lugar, desea realizar estos cambios manualmente:
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
>            <data android:scheme="msauth"
>                android:host="Enter_the_Package_Name"
>                android:path="/Enter_the_Decoded_Signature_Hash" />
>        </intent-filter>
>    </activity>
>    ```
> 1. Reemplace `Enter_the_Package_Name` y `Enter_the_Signature_Hash` por los valores que registró en Azure Portal. 
> 1. Ejecute la aplicación. 

## <a name="more-information"></a>Más información

Lea las secciones siguientes para obtener más información acerca de esta guía de inicio rápido.

### <a name="getting-msal"></a>Introducción a MSAL

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar tokens de acceso a una API protegida por la Plataforma de identidad de Microsoft. Puede usar Gradle 3.0+ para instalarla si agrega lo siguiente en la sección **Dependencies** (Dependencias) de **Gradle Scripts** > **build.gradle (Module: app)** :

```gradle  
implementation 'com.android.volley:volley:1.1.1'
implementation 'com.microsoft.identity.client:msal:0.3.+'
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
> |`R.raw.auth_config` | Este archivo contiene las configuraciones de la aplicación, incluidos el identificador de la aplicación o de cliente, las audiencia de inicio de sesión, el URI de redirección y varias otras opciones de personalización. |

### <a name="requesting-tokens"></a>Solicitud de tokens

MSAL tiene dos métodos para adquirir tokens: `acquireToken` y `acquireTokenSilentAsync`

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Obtención de un token de forma interactiva

Algunas situaciones requieren que los usuarios interactúen con la plataforma de identidad de Microsoft. En estos casos, puede que sea necesario que el usuario final seleccione su cuenta, escriba sus credenciales o dé su consentimiento a los permisos solicitados por la aplicación. Por ejemplo, 

* La primera vez que los usuarios inician sesión en la aplicación
* Si un usuario restablece su contraseña, deberá escribir sus credenciales. 
* Si se revoca el consentimiento. 
* Si la aplicación requiere explícitamente el consentimiento. 
* Cuando la aplicación solicita acceso a un recurso por primera vez.
* Cuando se requieren MFA u otras directivas de acceso condicional.

```java
sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
```

> |Donde:||
> |---------|---------|
> | `SCOPES` | Contiene los ámbitos que se solicitan (es decir, `{ "user.read" }` para Microsoft Graph o `{ "<Application ID URL>/scope" }` para las API web personalizadas (por ejemplo, `api://<Application ID>/access_as_user`) |
> | `getAuthInteractiveCallback` | Es la devolución de llamada que se ejecuta cuando la aplicación recupera el control después de la autenticación |

#### <a name="acquiretokensilent-getting-a-user-token-silently"></a>acquireTokenSilent: Obtención de un token de usuario en silencio

Las aplicaciones no requieren que sus usuarios inicien sesión cada vez que soliciten un token. Si el usuario ya ha iniciado sesión, este método permite que las aplicaciones soliciten tokens de forma silenciosa.

```java
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (!accounts.isEmpty()) {
                sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
            } else {
                /* No accounts */
            }
        }
    });
```

> |Donde:||
> |---------|---------|
> | `SCOPES` | Contiene los ámbitos que se solicitan (es decir, `{ "user.read" }` para Microsoft Graph o `{ "<Application ID URL>/scope" }` para las API web personalizadas (por ejemplo, `api://<Application ID>/access_as_user`) |
> | `getAccounts(...)` | Contiene la cuenta para la que está intentando obtener tokens de forma silenciosa |
> | `getAuthSilentCallback()` | Es la devolución de llamada que se ejecuta cuando la aplicación recupera el control después de la autenticación |

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
