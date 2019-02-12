---
title: 'Tutorial: Habilitación de la autenticación en una aplicación de página única mediante Azure Active Directory B2C | Microsoft Docs'
description: Tutorial acerca de cómo usar Azure Active Directory B2C para proporcionar inicio de sesión de usuario a una aplicación de una sola página (JavaScript).
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 813c7131ff0a56e843e728cd78fff969b1d90fcc
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55756332"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-using-azure-active-directory-b2c"></a>Tutorial: Habilitación de la autenticación en una aplicación de página única mediante Azure Active Directory B2C

En este tutorial se muestra cómo usar Azure Active Directory (Azure AD) B2C para que los usuarios se registren e inicien sesión en una aplicación de página única (SPA). Azure AD B2C permite que las aplicaciones puedan autenticarse en cuentas de las redes sociales, cuentas de empresa y cuentas de Azure Active Directory mediante protocolos estándar abiertos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Actualizar la aplicación en Azure AD B2C
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Cree flujos de usuario](tutorial-create-user-flows.md) para habilitar las experiencias de usuario en la aplicación. 
* Instalar [Visual Studio 2017](https://www.visualstudio.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.
* Instale el [SDK 2.0.0 de .NET Core](https://www.microsoft.com/net/core) o posterior.
* Instalar [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>Actualizar la aplicación

En el tutorial que completó como parte de los requisitos previos, agregó una aplicación web en Azure AD B2C. Para habilitar la comunicación con el ejemplo en este tutorial, deberá agregar un identificador URI de redirección a la aplicación en Azure AD B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y, a continuación, seleccione la aplicación *webapp1*.
5. En **URL de respuesta**, agregue `http://localhost:6420`.
6. Seleccione **Guardar**.
7. En la página de propiedades, registre el identificador de la aplicación que usará cuando configure la aplicación web.
8. Seleccione **Claves**, después, **Generar clave** y, finalmente, **Guardar**. Registre la clave que usará cuando configure la aplicación web.

## <a name="configure-the-sample"></a>Configuración del ejemplo

En este tutorial, va a configurar un ejemplo que puede descargar desde GitHub. Este ejemplo muestra cómo una aplicación de página única puede usar Azure AD B2C para el registro del usuario, el inicio de sesión y llamar a una API web protegida.

[Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) o clone el ejemplo de GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

Para cambiar estos valores:

1. Abra el archivo `index.html` del ejemplo.
2. Configure el ejemplo con el identificador y la clave de la aplicación que registró anteriormente. Cambie las siguientes líneas de código reemplazando los valores por los nombres del directorio y las API:

    ```javascript
    // The current application coordinates were pre-registered in a B2C directory.
    var applicationConfig = {
        clientID: '<Application ID>',
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
    };
    ```

    El nombre del flujo de usuario que se ha utilizado en este tutorial es **B2C_1_signupsignin1**. Si va a usar otro nombre de flujo de usuario distinto, úselo en el valor `authority`.

## <a name="run-the-sample"></a>Ejecución del ejemplo

1. Inicie un símbolo del sistema de Node.js.
2. Cambie al directorio que contiene el ejemplo de Node.js. Por ejemplo, `cd c:\active-directory-b2c-javascript-msal-singlepageapp`.
3. Ejecute los comandos siguientes:

    ```
    npm install && npm update
    node server.js
    ```

    La ventana de la consola muestra el número de puerto de donde se hospeda la aplicación.
    
    ```
    Listening on port 6420...
    ```

4. Use un explorador para ir a la dirección `http://localhost:6420` y ver la aplicación.

El ejemplo admite el registro, inicio de sesión, edición de un perfil y restablecimiento de una contraseña. Este tutorial ilustra cómo un usuario se registra mediante una dirección de correo electrónico.

### <a name="sign-up-using-an-email-address"></a>Registro con una dirección de correo electrónico

1. Haga clic en **Iniciar sesión** para iniciar sesión como usuario de la aplicación. Aquí se utiliza el flujo de usuario **B2C_1_signupsignin1** que ha definido en un paso anterior.
2. Azure AD B2C presenta una página de inicio de sesión con un vínculo de registro. Como no tiene aún una cuenta, haga clic en el vínculo **Registrarse ahora**. 
3. El flujo de trabajo del registro presenta una página para recopilar y verificar la identidad del usuario con una dirección de correo electrónico. El flujo de trabajo de registro también recopila la contraseña del usuario y los atributos requeridos definidos en el flujo de usuario.

    Utilice una dirección de correo electrónico válida y valídela mediante el código de verificación. Establezca una contraseña. Especifique valores para los atributos solicitados. 

    ![Flujo de trabajo de registro](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Haga clic en **Crear** para crear una cuenta local en el directorio de Azure AD B2C.

Ahora el usuario puede utilizar su dirección de correo electrónico para iniciar sesión y usar la aplicación de página única.

> [!NOTE]
> Tras iniciar sesión, la aplicación muestra el error "permisos insuficientes". Este error aparece porque está intentando acceder a un recurso desde el directorio de la demostración. Como el token de acceso solamente es válido para su directorio de Azure AD, la llamada API no tiene autorización. Continúe en el siguiente tutorial para crear una API web protegida para el directorio.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Actualizar la aplicación en Azure AD B2C
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de una sola página mediante Azure Active Directory B2C](active-directory-b2c-tutorials-spa-webapi.md)
