---
title: Inicio de sesión por los usuarios y llamada a Microsoft Graph API desde una aplicación de Android | Microsoft Docs
description: Aprenda a hacer que los usuarios inicien sesión y a llamar a Microsoft Graph API desde una aplicación de Android.
services: active-directory
documentationcenter: android
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 7f7ae858f633a910b796f544ed69a582e749beaf
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53311072"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Guía de inicio rápido: Inicie sesión de los usuarios y llame a Microsoft Graph API desde una aplicación de Android

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Si está desarrollando una aplicación para Android, Microsoft hace que los usuarios puedan iniciar sesión en Azure Active Directory (Azure AD) de forma fácil y directa. Azure AD permite que la aplicación acceda a los datos de usuario a través de Microsoft Graph o de su propia API web protegida.

La biblioteca para Android, Biblioteca de Autenticación de Azure AD (ADAL), permite que la aplicación pueda empezar a usar [Microsoft Azure Cloud](https://cloud.microsoft.com) y [Microsoft Graph API](https://developer.microsoft.com/graph) al ser compatible con [cuentas de Microsoft Azure Active Directory](https://azure.microsoft.com/services/active-directory/) con OAuth 2.0 y OpenID Connect estándar del sector.

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

* Obtener un token de Microsoft Graph
* Actualizar un token
* Llamar a Microsoft Graph
* Cerrar la sesión del usuario

## <a name="prerequisites"></a>Requisitos previos

Para comenzar, necesitará a un inquilino de Azure AD en el que pueda crear usuarios y registrar una aplicación. Si aún no tiene un inquilino, [descubra cómo conseguir uno](quickstart-create-new-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Escenario: Realice el inicio de sesión de usuarios y llame a Microsoft Graph

![Topología](./media/quickstart-v1-android/active-directory-android-topology.png)

Puede usar esta aplicación para todas las cuentas de Azure AD. Es compatible con escenarios únicos y multiinquilino (que se describen en pasos). Muestra cómo puede compilar aplicaciones para conectarse con usuarios empresariales y acceder a sus datos de Azure + O365 a través de Microsoft Graph. Durante el flujo de autenticación, los usuarios finales deberán iniciar sesión y aceptar los permisos de la aplicación y, en algunos casos, es posible que un administrador tenga que dar su consentimiento a la aplicación. La mayoría de la lógica de este ejemplo muestra cómo autenticar un usuario final y realizar una llamada básica a Microsoft Graph.

## <a name="sample-code"></a>Código de ejemplo

El código de ejemplo completo se puede encontrar [en GitHub](https://github.com/Azure-Samples/active-directory-android).

```Java
// Initialize your app with MSAL
AuthenticationContext mAuthContext = new AuthenticationContext(
        MainActivity.this,
        AUTHORITY,
        false);


// Perform authentication requests
mAuthContext.acquireToken(
    getActivity(),
    RESOURCE_ID,
    CLIENT_ID,
    REDIRECT_URI,
    PromptBehavior.Auto,
    getAuthInteractiveCallback());

// ...

// Get tokens to call APIs like the Microsoft Graph
mAuthResult.getAccessToken()
```

## <a name="step-1-register-and-configure-your-app"></a>Paso 1: Registrar y configurar la aplicación

Debe haber registrado una aplicación cliente nativa con Microsoft mediante [Azure Portal](https://portal.azure.com).

1. Acceder al registro de la aplicación
    - Acceda a [Azure Portal](https://aad.portal.azure.com).
    - Seleccione ***Azure Active Directory*** > ***Registros de aplicaciones***.

2. Creación de la aplicación
    - Seleccione **Nuevo registro de aplicaciones**.
    - Escriba un nombre de aplicación en el campo **Nombre**.
    - En **Tipo de aplicación**, seleccione: **Nativa**.
    - En **URI de redirección**, escriba `http://localhost`.

3. Configurar Microsoft Graph
    - Seleccione **Configuración > Permisos necesarios**.
    - Seleccione **agregar** y, en **Seleccionar una API**, seleccione ***Microsoft Graph***.
    - Seleccione el permiso **Iniciar sesión y leer el perfil de usuario** y, luego, presione **Selecciona** para guardar.
        - Este permiso se asigna al ámbito `User.Read`.
    - Opcional: en **Permisos necesarios > Windows Azure Active Directory**, quite el permiso seleccionado **Iniciar sesión y leer el perfil de usuario**. Esto evita que la página de consentimiento de usuario muestre el permiso dos veces.

4. ¡Enhorabuena! La aplicación se ha configurado correctamente. En la siguiente sección, necesitará:
    - `Application ID`
    - `Redirect URI`

## <a name="step-2-get-the-sample-code"></a>Paso 2: Obtención del código de ejemplo

1. Clonar el código.
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Abra el ejemplo en Android Studio.
    - Seleccione **Abrir un proyecto existente de Android Studio**.

## <a name="step-3-configure-your-code"></a>Paso 3: Configurar el código

Puede encontrar toda la configuración de este ejemplo de código en el archivo ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java***.

1. Reemplace la constante `CLIENT_ID` con el valor `ApplicationID`.
2. Reemplace la constante `REDIRECT URI` con el valor `Redirect URI` que configuró anteriormente (`http://localhost`).

## <a name="step-4-run-the-sample"></a>Paso 4: Ejecución del ejemplo

1. Seleccione **Compilar > Limpiar el proyecto**.
2. Seleccione **Ejecutar > ejecutar aplicación**.
3. La aplicación debe compilarse y mostrar la experiencia del usuario básica. Al hacer clic en el botón `Call Graph API`, se solicita un inicio de sesión y después, en modo silencioso, se llama a Microsoft Graph API con el nuevo token.

## <a name="next-steps"></a>Pasos siguientes

1. Consulte la [wiki de ADAL para Android](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) para obtener más información sobre los mecanismos de biblioteca y cómo configurar nuevos escenarios y funcionalidades.
2. En escenarios nativo, la aplicación usará un elemento WebView incrustado y no saldrá de la aplicación. El valor `Redirect URI` puede ser arbitrario.
3. ¿Encontró algún problema o tiene preguntas? Puede crear un problema o una entrada en Stackoverflow con la etiqueta `azure-active-directory`.

### <a name="cross-app-sso"></a>SSO entre aplicaciones

Obtenga información sobre la [Habilitación de SSO entre aplicaciones en Android mediante ADAL](howto-v1-enable-sso-android.md).

### <a name="auth-telemetry"></a>Telemetría de autenticación

La biblioteca ADAL expone telemetría de autenticación para ayudar a los desarrolladores de aplicaciones a comprender cómo se comportan sus aplicaciones para poder crear mejores experiencias. Esto le permite obtener un inicio de sesión correcto, usuarios activos y varias otras conclusiones interesantes. Para usar telemetría de autenticación, los desarrolladores de aplicaciones necesitan establecer un servicio de telemetría para agregar y almacenar eventos.

Para obtener más información sobre telemetría de autenticación, consulte [Telemetría de autenticación de ADAL para Android](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry).
