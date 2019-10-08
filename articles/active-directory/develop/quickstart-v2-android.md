---
title: Inicio rápido de la Plataforma de identidad de Microsoft para Android | Azure
description: Más información acerca cómo las aplicaciones Android pueden llamar a una API que requiere tokens de acceso de un punto de conexión de la plataforma de identidad de Microsoft.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/16/2019
ms.author: twhitney
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: e11e47952f70ce0cd212ca93eff1c38f2b3993a8
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71678047"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Inicio rápido: Inicie sesión de los usuarios y llame a Microsoft Graph API desde una aplicación de Android

En este inicio rápido se usa un código de ejemplo que muestra cómo una aplicación de Android puede iniciar sesión con cuentas personales, de trabajo y educativas, y después obtener un token de acceso y llamar a Microsoft Graph API.

![Captura de pantalla de la aplicación de ejemplo](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Requisitos previos**
> * Android Studio 
> * Se requiere Android 16+

## <a name="step-1-get-the-sample-app"></a>Paso 1: Obtención de la aplicación de ejemplo

[Clonación del código](https://github.com/Azure-Samples/ms-identity-android-java.git).

## <a name="step-2-register-your-application"></a>Paso 2: Registrar su aplicación

Para registrar un objeto de aplicación y agregar la información de registro de este objeto al proyecto de ejemplo manualmente, siga estos pasos:

1. Vaya a [Azure Portal](https://aka.ms/MobileAppReg).
1. Abra la [hoja de Registros de aplicaciones](https://portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) y haga clic en **+Nuevo registro**.
1. Escriba el **nombre** del registro de la aplicación y después, antes de establecer una URI de redirección, haga clic en **Registrar**.
1. En la sección **Administrar**, seleccione **Autenticación** >  **+ Agregar una plataforma** > **Android** (es posible que tenga que seleccionar **Probar la nueva experiencia** cerca de la parte superior de la hoja para ver esta pantalla).
1. Escriba el **nombre del paquete** del proyecto, que es `com.azuresamples.msalandroidapp`.
1. En la sección **Hash de firma** de la página **Configurar su aplicación para Android** , haga clic en **Generating a development Signature Hash** (Se está generando un hash de firma de desarrollo) y copie el comando KeyTool que desee que se use para la plataforma que va a utilizar para desarrollar la aplicación Android.

   > [!Note]
   > KeyTool. exe se instala como parte de Java Development Kit (JDK). También debe instalar la herramienta OpenSSL para ejecutar el comando KeyTool.  Tanto keytool como el directorio OpenSSL\bin deben estar en la ruta de acceso.

1. Ejecute el comando keytool que copió del portal en una ventana de terminal.
1. Escriba el hash de firma generado en el portal en **Hash de firma**.
1. Haga clic en `Configure` y realice una copia de la configuración del valor de **Configuración de MSAL**. Este valor lo copiará y pegará en un archivo de configuración en el paso siguiente. Haga clic en **Done**(Listo).

## <a name="step-3-add-your-app-registration"></a>Paso 3: Adición del registro de aplicaciones

1. Abra el proyecto de ejemplo en Android Studio.
1. En **app** > **res** > **raw**, abra **auth_config_multiple_account.json**.  Pegue el contenido del campo Configuración de MSAL. De esta forma se agregan el identificador de cliente, el identificador de inquilino y el URI de redirección del portal. Será parecido al siguiente, pero con los valores rellenados de identificador de cliente, el identificador de inquilino y URI de redirección rellenados:

    ```json
    {
      "client_id" : "<your_client_id_here>",
      "authorization_user_agent" : "DEFAULT",
      "redirect_uri" : "<your_redirect_uri_here>",
      "account_mode" : "MULTIPLE",
      "broker_redirect_uri_registered": true,
      "authorities" : [
        {
          "type": "AAD",
          "audience": {
            "type": "AzureADandPersonalMicrosoftAccount",
            "tenant_id": "common"
          }
        }
      ]
    }
    ```

1. Abra **app** > **res** > **raw**, open **auth_config_single_account.json** y pegue el contenido de Configuración de MSAL. Será similar al archivo **auth_config_multiple_account.json** anterior.
1. En **app** > **manifests** > **AndroidManifest.xml**, busque la actividad `BrowserTabActivity`. Esta entrada permite a Microsoft volver a llamar a la aplicación una vez completada la autenticación:

    ```xml
    ...
    <activity android:name="com.microsoft.identity.client.BrowserTabActivity">
                <intent-filter>
                    <action android:name="android.intent.action.VIEW" />
    
                    <category android:name="android.intent.category.DEFAULT" />
                    <category android:name="android.intent.category.BROWSABLE" />
    
                    <!--
                        Add in your scheme/host from registered redirect URI
                        note that the leading "/" is required for android:path
                        For Example:
                        <data
                            android:host="com.azuresamples.msalandroidapp"
                            android:path="/1wIqXSqBj7w+h11ZifsnqwgyKrY="
                            android:scheme="msauth" />
                    -->
    
                    <data
                        android:host="YOUR_PACKAGE_NAME - must be registered at https://aka.ms/MobileAppReg"
                        android:path="/YOUR_DECODED_SIGNATURE_HASH - must be registered at https://aka.ms/MobileAppReg"
                        android:scheme="msauth" />
                </intent-filter>
            </activity>
    ```
    
1. Sustituya el nombre del paquete por el que registró en Azure Portal para el valor `android:host=`.  En este caso, será `com.azuresamples.msalandroidapp`.

    > [!IMPORTANT]
    > El valor **android:path** **debe** tener un carácter "/" delante o aparecerá una línea roja debajo del valor y la aplicación de ejemplo no se ejecutará.
     
1. Sustituya el hash de clave que recibió cuando ejecutó keytool y que escribió en Azure Portal para el valor `android:path=`. El hash de firma no debe estar codificado como dirección URL.

## <a name="step-4-run-the-sample-app"></a>Paso 4: Ejecutar la aplicación de ejemplo

Seleccione el emulador o dispositivo en la lista desplegable de **dispositivos disponibles** de Android Studio.

La aplicación de ejemplo se inicia en la pantalla <bpt id="p1">**</bpt>Single Account Mode<ept id="p1">**</ept> (Modo de cuenta única). De forma predeterminada, se proporciona un ámbito predeterminado, **user.read**, que se usa al leer sus propios datos de perfil en la llamada a Microsoft Graph API. La dirección URL de la llamada a Microsoft Graph API se proporciona de forma predeterminada. Ambos elementos se pueden cambiar si se desea.

![Aplicación de ejemplo de MSAL que muestra el uso de una cuenta y de varias](./media/quickstart-v2-android/quickstart-sample-app.png)

Use el menú de la aplicación para cambiar entre los modos de una y varias cuentas.

En el modo de una cuenta única, inicie sesión con una cuenta profesional o personal:

1. Seleccione **Get graph data interactively (Obtener datos de gráfico de forma interactiva)** para pedir al usuario sus credenciales. El resultado de la llamada a Microsoft Graph API aparecerá en la parte inferior de la pantalla.
2. Una vez que haya iniciado sesión, seleccione **Get graph data silently** (Obtener datos de gráfico sin intervención del usuario) para realizar una llamada a Microsoft Graph API sin volver a pedir las credenciales al usuario. El resultado de la llamada a Microsoft Graph API aparecerá en la parte inferior de la pantalla.

En el modo de varias cuentas, puede repetir los mismos pasos.  Además, puede quitar la cuenta con la que se ha iniciado sesión, lo que también quita los tokens almacenados en caché de esa cuenta.

## <a name="how-the-sample-works"></a>Funcionamiento del ejemplo

El código se organiza en fragmentos que muestran cómo escribir una aplicación de MSAL de una y varias cuentas. Los archivos del código se organizan de la siguiente manera:

| Archivo  | Muestra  |
|---------|---------|
| MainActivity | Administra la interfaz de usuario |
| MSGraphRequestWrapper  | Llama a Microsoft Graph API mediante el token proporcionado por MSAL |
| MultipleAccountModeFragment  | Inicializa una aplicación de varias cuentas, carga una cuenta de usuario y obtiene un token para llamar a Microsoft Graph API |
| SingleAccountModeFragment | Inicializa una aplicación de una cuenta, carga una cuenta de usuario y obtiene un token para llamar a Microsoft Graph API |
| res/auth_config_multiple_account.json  | El archivo de configuración de varias cuentas |
| res/auth_config_single_account.json  | El archivo de configuración de una cuenta |
| Gradle Scripts/build.grade (Module:app) | Aquí se agregan las dependencias de la biblioteca MSAL |

Ahora vamos a examinar estos archivos con más detalle y a llamar al código específico de MSAL en cada uno.

### <a name="add-msal-to-the-app"></a>Incorporación de MSAL a la aplicación

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar tokens de acceso a una API protegida por la Plataforma de identidad de Microsoft. Gradle 3.0+ instala la biblioteca cuando se agrega lo siguiente a **Gradle Scripts** (Scripts de Gradle) > **build.gradle (Module: app)** en **Dependencies** (Dependencias):

```gradle  
implementation 'com.microsoft.identity.client:msal:1.0.0'
```

Puede verlo en el proyecto de ejemplo de build.gradle (Module: app):

```java
dependencies {
    ...
    implementation 'com.microsoft.identity.client:msal:1.0.0-RC7'
    ...
}
```

Esto indica a Gradle que descargue y compile MSAL desde Maven Central.

### <a name="msal-imports"></a>Importaciones de MSAL

Las importaciones que son relevantes para la biblioteca MSAL son `com.microsoft.identity.client.*`.  Por ejemplo, verá `import com.microsoft.identity.client.PublicClientApplication;`, que es el espacio de nombres de la clase `PublicClientApplication`, que representa la aplicación cliente pública.

### <a name="singleaccountmodefragmentjava"></a>SingleAccountModeFragment.java

En este archivo se muestra cómo crear una aplicación MSAL de una cuenta y llamar a Microsoft Graph API.

Las aplicaciones de una cuenta las utilizan usuarios individuales.  Por ejemplo, es posible que solo tenga una cuenta con la que inicia sesión en la aplicación de asignación.

#### <a name="single-account-msal-initialization"></a>Inicialización MSAL de cuenta única

En `onCreateView()`, se crea una sola cuenta `PublicClientApplication` mediante la información de configuración almacenada en el archivo `auth_config_single_account.json`.  Así es como se inicializa la biblioteca MSAL para su uso en una aplicación de MSAL de cuenta única:

```java
...
// Creates a PublicClientApplication object with res/raw/auth_config_single_account.json
PublicClientApplication.createSingleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_single_account,
        new IPublicClientApplication.ISingleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(ISingleAccountPublicClientApplication application) {
                /**
                    * This test app assumes that the app is only going to support one account.
                    * This requires "account_mode" : "SINGLE" in the config json file.
                    **/
                mSingleAccountApp = application;
                loadAccount();
            }

            @Override
            public void onError(MsalException exception) {
                displayError(exception);
            }
        });
```

#### <a name="sign-in-a-user"></a>Inicio de sesión de un usuario

El código para iniciar sesión de un usuario está en `initializeUI()`, en el controlador de clics de `signInButton`.

Llame a `signIn()` antes de intentar adquirir tokens. `signIn()` se comporta como si se llamara a `acquireToken()`, lo que genera un aviso interactivo para que el usuario inicie sesión.

El inicio de sesión de un usuario es una operación asincrónica. Se pasa una devolución de llamada que llama a Microsoft Graph API y actualiza la interfaz de usuario una vez que el usuario inicia sesión:

```java
mSingleAccountApp.signIn(getActivity(), null, getScopes(), getAuthInteractiveCallback());
```

#### <a name="sign-out-a-user"></a>Cierre de la sesión de un usuario

El código para cerrar la sesión de un usuario está en `initializeUI()`, en el controlador de clics de `signOutButton`.  El cierre de la sesión de un usuario es una operación asincrónica. Al cerrar la sesión del usuario, también se borra la caché de tokens de esa cuenta. Una vez que la sesión del usuario se ha cerrado, se crea una devolución de llamada para actualizar la interfaz de usuario:

```java
mSingleAccountApp.signOut(new ISingleAccountPublicClientApplication.SignOutCallback() {
    @Override
    public void onSignOut() {
        updateUI(null);
        performOperationOnSignOut();
    }

    @Override
    public void onError(@NonNull MsalException exception) {
        displayError(exception);
    }
});
```

#### <a name="get-a-token-interactively-or-silently"></a>Obtención de tokens de forma interactiva o sin intervención del usuario

Para presentar el menor número de mensajes posible al usuario, lo habitual es que el token se obtenga sin intervención del usuario. Si se produce algún error, intente obtener el token de forma interactiva. La primera vez que la aplicación llama a `signIn()`, actúa como una llamada a `acquireToken()`, que solicitará las credenciales al usuario.

Estas son algunas situaciones en las que puede que se pida al usuario que seleccione su cuenta, escriba sus credenciales o dé su consentimiento a los permisos que ha solicitado la aplicación:

* La primera vez que el usuario inicia sesión en la aplicación.
* Si un usuario restablece su contraseña, deberá escribir sus credenciales.
* Si se revoca el consentimiento.
* Si la aplicación requiere explícitamente el consentimiento.
* Cuando la aplicación solicita acceso a un recurso por primera vez.
* Cuando se requieren MFA u otras directivas de acceso condicional.

El código para obtener un token de forma interactiva, es decir, con una interfaz de usuario que implique al usuario, está en `initializeUI()`, en el controlador de clics `callGraphApiInteractiveButton`:

```java
/**
 * If acquireTokenSilent() returns an error that requires an interaction (MsalUiRequiredException),
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your Single Sign-On refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 */
mSingleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Si el usuario ya ha iniciado sesión,`acquireTokenSilentAsync()` permite a las aplicaciones solicitar tokens de sin intervención del usuario, como se muestra en `initializeUI()`, en el controlador de clics `callGraphApiSilentButton`:

```java
/**
  * Once you've signed the user in,
  * you can perform acquireTokenSilent to obtain resources without interrupting the user.
  */
  mSingleAccountApp.acquireTokenSilentAsync(getScopes(), AUTHORITY, getAuthSilentCallback());
```

#### <a name="load-an-account"></a>Carga de una cuenta

El código para cargar una cuenta está en `loadAccount()`.  La carga de la cuenta del usuario es una operación asincrónica, por lo que las devoluciones de llamada que se controlan cuando la cuenta se carga o cambia, o cuando se produce un error se pasan a MSAL.  El código siguiente también controla `onAccountChanged()`, que se produce cuando se quita una cuenta, el usuario cambia de cuenta, etc.

```java
private void loadAccount() {
    ...

    mSingleAccountApp.getCurrentAccountAsync(new ISingleAccountPublicClientApplication.CurrentAccountCallback() {
        @Override
        public void onAccountLoaded(@Nullable IAccount activeAccount) {
            // You can use the account data to update your UI or your app database.
            updateUI(activeAccount);
        }

        @Override
        public void onAccountChanged(@Nullable IAccount priorAccount, @Nullable IAccount currentAccount) {
            if (currentAccount == null) {
                // Perform a cleanup task as the signed-in account changed.
                performOperationOnSignOut();
            }
        }

        @Override
        public void onError(@NonNull MsalException exception) {
            displayError(exception);
        }
    });
```

#### <a name="call-microsoft-graph"></a>Llamada a Microsoft Graph

Cuando un usuario inicia sesión, la llamada a Microsoft Graph se realiza mediante una solicitud HTTP mediante `callGraphAPI()`. Esta función es un contenedor que simplifica el ejemplo mediante la realización de algunas tareas, como obtener el token de acceso de `authenticationResult` y empaquetar la llamada a MSGraphRequestWrapper y mostrar los resultados de la llamada.

```java
private void callGraphAPI(final IAuthenticationResult authenticationResult) {
    MSGraphRequestWrapper.callGraphAPIUsingVolley(
            getContext(),
            graphResourceTextView.getText().toString(),
            authenticationResult.getAccessToken(),
            new Response.Listener<JSONObject>() {
                @Override
                public void onResponse(JSONObject response) {
                    /* Successfully called graph, process data and send to UI */
                    ...
                }
            },
            new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    ...
                }
            });
}
```

### <a name="auth_config_single_accountjson"></a>auth_config_single_account.json

Es el archivo de configuración de una aplicación MSAL que usa una cuenta única.

Para obtener una explicación de estos campos, consulte [Información acerca del archivo de configuración de Android MSAL](msal-configuration.md).

Observe la presencia de `"account_mode" : "SINGLE"`, que configura esta aplicación para que use una sola cuenta.

```json
{
  "client_id" : "<your_client_id_here>",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "<your_redirect_uri_here>",
  "account_mode" : "SINGLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

### <a name="multipleaccountmodefragmentjava"></a>MultipleAccountModeFragment.java

En este archivo se muestra cómo crear una aplicación MSAL de varias cuentas y llamar a Microsoft Graph API. 

Un ejemplo de aplicación de varias cuentas es una aplicación de correo que permite trabajar con varias cuentas de usuario, como una cuenta profesional y una cuenta personal.

#### <a name="multiple-account-msal-initialization"></a>Inicialización MSAL de varias cuentas

En `onCreateView()`, se crea un objeto de aplicación de varias cuentas (`IMultipleAccountPublicClientApplication`) mediante la información de configuración almacenada en `auth_config_multiple_account.json file`:

```java
// Creates a PublicClientApplication object with res/raw/auth_config_single_account.json
PublicClientApplication.createMultipleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_multiple_account,
        new IPublicClientApplication.IMultipleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(IMultipleAccountPublicClientApplication application) {
                mMultipleAccountApp = application;
                loadAccount();
            }

            @Override
            public void onError(MsalException exception) {
                ...
            }
        });
```

El objeto `MultipleAccountPublicClientApplication` creado se almacena en una variable de miembro de clase, con el fin de que se pueda usar para interactuar con la biblioteca MSAL para adquirir tokens y cargar y quitar la cuenta de usuario.

#### <a name="load-an-account"></a>Carga de una cuenta

Las aplicaciones de varias cuentas habitualmente llaman a `GetAccounts()` para seleccionar la cuenta que se va a usar para las operaciones de MSAL. El código para cargar una cuenta está en `loadAccount()`.  La carga de la cuenta del usuario es una operación asincrónica. Por consiguiente, una devolución de llamada controla las situaciones cuando se la cuenta se carga o se cambia, o cuando se produce un error.

```java
/**
    * Load the currently signed-in account, if there's any.
    * In the shared device mode, if the user is signed out from the device, the app can also perform the clean-up work in onAccountChanged().
    */
private void loadAccount() {
    ...
    mMultipleAccountApp.getAccounts(new IPublicClientApplication.LoadAccountsCallback() {
        @Override
        public void onTaskCompleted(final List<IAccount> result) {
            // You can use the account data to update your UI or your app database.
            accountList = result;
            updateUI(accountList);
        }

        @Override
        public void onError(MsalException exception) {
            displayError(exception);
        }
    });
}
```

#### <a name="get-a-token-interactively-or-silently"></a>Obtención de tokens de forma interactiva o sin intervención del usuario

Estas son algunas situaciones en las que puede que se pida al usuario que seleccione su cuenta, escriba sus credenciales o dé su consentimiento a los permisos que ha solicitado la aplicación:

* La primera vez que los usuarios inician sesión en la aplicación
* Si un usuario restablece su contraseña, deberá escribir sus credenciales. 
* Si se revoca el consentimiento. 
* Si la aplicación requiere explícitamente el consentimiento. 
* Cuando la aplicación solicita acceso a un recurso por primera vez.
* Cuando se requieren MFA u otras directivas de acceso condicional.

Lo habitual es que las aplicaciones de varias cuentas normalmente adquieran los tokens de forma interactiva, es decir, con una interfaz de usuario que implique al usuario, con una llamada a `acquireToken()`.  El código para obtener un token de forma interactiva está en `initializeUI()`, en el controlador de clics de `callGraphApiInteractiveButton`:

```java
/**
 * Acquire token interactively. It will also create an account object for the silent call as a result (to be obtained by getAccount()).
 *
 * If acquireTokenSilent() returns an error that requires an interaction,
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your SSO refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 */
mMultipleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Las aplicaciones no deberían requerir que el usuario inicie sesión cada vez que soliciten un token. Si el usuario ya ha iniciado sesión,`acquireTokenSilentAsync()` permite a las aplicaciones solicitar tokens de sin avisar al usuario, como se muestra en `initializeUI()`, en el controlador de clics `callGraphApiSilentButton`:

```java
/**
 * Performs acquireToken without interrupting the user.
 *
 * This requires an account object of the account you're obtaining a token for.
 * (can be obtained via getAccount()).
 */
mMultipleAccountApp.acquireTokenSilentAsync(getScopes(),
accountList.get(accountListSpinner.getSelectedItemPosition()),
AUTHORITY,
getAuthSilentCallback());
```

#### <a name="remove-an-account"></a>Eliminación de una cuenta

El código para quitar una cuenta, y todos los tokens almacenados en caché de la cuenta, se encuentra en `initializeUI()` en el controlador para el botón para quitar la cuenta. Para poder quitar una cuenta, se necesita un objeto de cuenta, que se obtiene de las funciones de MSAL, como `getAccounts()` y `acquireToken()`. Dado que la eliminación de una cuenta es una operación asincrónica, se proporciona la devolución de llamada `onRemoved` para actualizar la interfaz de usuario.

```java
/**
  * Removes the selected account and cached tokens from this app (or device, if the device is in shared mode).
  */
mMultipleAccountApp.removeAccount(accountList.get(accountListSpinner.getSelectedItemPosition()),
        new IMultipleAccountPublicClientApplication.RemoveAccountCallback() {
            @Override
            public void onRemoved() {
                ...
                /* Reload account asynchronously to get the up-to-date list. */
                loadAccount();
            }

            @Override
            public void onError(@NonNull MsalException exception) {
                displayError(exception);
            }
        });
```

### <a name="auth_config_multiple_accountjson"></a>auth_config_multiple_account.json

Es el archivo de configuración de una aplicación MSAL que usa varias cuentas.

Para obtener una explicación de estos campos, consulte [Información acerca del archivo de configuración de Android MSAL](msal-configuration.md).

A diferencia del archivo de configuración [auth_config_single_account.json](#auth_config_single_accountjson) este archivo de configuración tiene `"account_mode" : "MULTIPLE"` en lugar de `"account_mode" : "SINGLE"`, ya que se trata de una aplicación de varias cuentas.

```json
{
  "client_id" : "<your_client_id_here>",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "<your_redirect_uri_here>",
  "account_mode" : "MULTIPLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

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

Ayúdenos a mejorar la Plataforma de identidad de Microsoft. Rellene una breve encuesta de dos preguntas y háganos saber su opinión.

> [!div class="nextstepaction"]
> [Encuesta sobre la Plataforma de identidad de Microsoft](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
