---
title: 'Tutorial: Habilitación de la autenticación en una aplicación cliente nativa mediante Azure Active Directory B2C | Microsoft Docs'
description: Tutorial acerca de cómo usar Azure Active Directory B2C para proporcionar inicio de sesión de usuario en una aplicación de escritorio de .NET.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 9a3c11c7303d467a1a993c49d983131639683239
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064875"
---
# <a name="tutorial-enable-authentication-in-a-native-client-application-using-azure-active-directory-b2c"></a>Tutorial: Habilitación de la autenticación en una aplicación cliente nativa mediante Azure Active Directory B2C

En este tutorial se muestra cómo usar Azure Active Directory B2C (Azure AD B2C) para registrar e iniciar sesión con usuarios en una aplicación de escritorio de Windows Presentation Foundation (WPF). Azure AD B2C permite que las aplicaciones puedan autenticarse en cuentas de las redes sociales, cuentas de empresa y cuentas de Azure Active Directory mediante protocolos estándar abiertos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar la aplicación cliente nativa
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- [Cree flujos de usuario](tutorial-create-user-flows.md) para habilitar las experiencias de usuario en la aplicación.
- [Instale Visual Studio 2019](https://www.visualstudio.com/downloads/) con las cargas de trabajo de **desarrollo de escritorio de .NET** y de **desarrollo web y de ASP.NET**.

## <a name="add-the-native-client-application"></a>Agregar la aplicación cliente nativa

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y **Agregar**.
5. Escriba un nombre para la aplicación. Por ejemplo, *nativeapp1*.
6. En **Incluir aplicación web o API web**, seleccione **No**.
7. En **Incluir cliente nativo**, seleccione **Sí**.
8. En **URI de redirección**, escriba un URI de redirección válido con un esquema personalizado. Hay dos aspectos importantes que se deben tener en cuenta al elegir un URI de redirección:

    - **Único**: el esquema del identificador URI de redirección debe ser único para todas las aplicaciones. En el ejemplo `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` es el esquema. Se debe seguir este patrón. Si dos aplicaciones comparten el mismo esquema, el usuario tiene opción de elegir una aplicación. Si el usuario realiza una elección incorrecta, no será posible iniciar sesión.
    - **Completo**: el URI de redirección debe tener un esquema y una ruta de acceso. La ruta de acceso debe contener al menos una barra diagonal después del dominio. Por ejemplo, `//contoso/` sirve y `//contoso` produce un error. Asegúrese de que el URI de redirección no incluya caracteres especiales, como caracteres de subrayado.

9. Haga clic en **Create**(Crear).
10. En la página de propiedades, registre el identificador de la aplicación que usará cuando configure el ejemplo.

## <a name="configure-the-sample"></a>Configuración del ejemplo

En este tutorial, va a configurar un ejemplo que puede descargar desde GitHub. La aplicación de escritorio WPF de ejemplo muestra el registro y el inicio de sesión, y llama a una API web protegida en Azure AD B2C. [Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [examine el repositorio](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) o clone el ejemplo de GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Para cambiar la configuración de la aplicación, reemplace `<your-tenant-name>` por el nombre del inquilino y reemplace`<application-ID`> por el identificador de aplicación que registró.

1. Abra la solución `active-directory-b2c-wpf` en Visual Studio.
2. En el proyecto `active-directory-b2c-wpf`, abra el archivo **App.xaml.cs** y realice las siguientes actualizaciones:

    ```csharp
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "<application-ID>";
    ```

3. Actualice la variable **PolicySignUpSignIn** con el nombre del flujo de usuario que ha creado.

    ```csharp
    public static string PolicySignUpSignIn = "B2C_1_signupsignin1";
    ```

## <a name="run-the-sample"></a>Ejecución del ejemplo

Presione **F5** para compilar y ejecutar el ejemplo.

### <a name="sign-up-using-an-email-address"></a>Registro con una dirección de correo electrónico

1. Haga clic en **Iniciar sesión** para registrarse como un usuario. Esto usa el flujo de usuario **B2C_1_signupsignin1**.
2. Azure AD B2C presenta una página de inicio de sesión con un vínculo de registro. Como no tiene aún una cuenta, haga clic en el vínculo **Registrarse ahora**.
3. El flujo de trabajo del registro presenta una página para recopilar y verificar la identidad del usuario con una dirección de correo electrónico. El flujo de trabajo de registro también recopila la contraseña del usuario y los atributos requeridos definidos en el flujo de usuario.

    Utilice una dirección de correo electrónico válida y valídela mediante el código de verificación. Establezca una contraseña. Especifique valores para los atributos solicitados.

    ![Página de registro que se muestra como parte del flujo de trabajo de inicio de sesión o registro](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.PNG)

4. Haga clic en **Crear** para crear una cuenta local en el inquilino de Azure AD B2C.

Ahora el usuario puede utilizar su dirección de correo electrónico para iniciar sesión y usar la aplicación de escritorio.

> [!NOTE]
> Si hace clic en el botón **Call API** (Llamar API), aparecerá el error "No autorizado". Dicho error aparece se intenta acceder a un recurso desde el inquilino de demostración. Puesto que su token de acceso solo es válido para su inquilino de Azure AD, la llamada de la API no tiene autorización. Pase al siguiente tutorial para crear una API web protegida para el inquilino.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Agregar la aplicación cliente nativa
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso a una API web de Node.js desde una aplicación de escritorio mediante Azure Active Directory B2C](active-directory-b2c-tutorials-spa-webapi.md)
