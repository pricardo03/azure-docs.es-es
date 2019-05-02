---
title: 'Tutorial: Concesión de acceso a una API web de Node.js desde una aplicación de escritorio mediante Azure Active Directory B2C | Microsoft Docs'
description: Tutorial acerca de cómo usar Active Directory B2C para proteger una API web de Node.js y llamarla desde una aplicación de escritorio de .NET.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: ef48c0f78083217581594b481b15a74a49fef4f9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64715841"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Tutorial: Concesión de acceso a una API web de Node.js desde una aplicación de escritorio mediante Azure Active Directory B2C

En este tutorial se muestra cómo llamar a un recurso de API web de Node.js protegido de Azure Active Directory (Azure AD) B2C desde una aplicación de escritorio de Windows Presentation Foundation (WPF).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Actualizar el ejemplo para que use la aplicación

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Complete los pasos y requisitos previos en [Tutorial: Habilitación de la autenticación de una aplicación de escritorio con cuentas mediante Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app.md).

## <a name="add-a-web-api-application"></a>Incorporar una aplicación de API web

Los recursos de API web tienen que registrarse en el inquilino antes de que puedan aceptar y responder a solicitudes de recursos protegidos por aplicaciones cliente que presenten un token de acceso. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y **Agregar**.
5. Escriba un nombre para la aplicación. Por ejemplo, *webapi1*.
6. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
7. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. En este tutorial, el ejemplo se ejecuta localmente y escucha en el puerto `https://localhost:5000`.
8. En **URI de id. de aplicación**, escriba el identificador usado para la API web. Se genera el identificador URI completo, incluido el dominio. Por ejemplo, `https://contosotenant.onmicrosoft.com/api`.
9. Haga clic en **Create**(Crear).
10. En la página de propiedades, registre el identificador de la aplicación que usará cuando configure la aplicación web.

## <a name="configure-scopes"></a>Configuración de ámbitos

Los ámbitos proporcionan una manera de controlar el acceso a los recursos protegidos. La API web utiliza los ámbitos para implementar el control de acceso basado en el ámbito. Por ejemplo, algunos usuarios pueden tener tanto acceso de lectura como de escritura, mientras que otros usuarios pueden tener permisos de solo lectura. En este tutorial, definirá los permisos de lectura para la API web.

1. Seleccione **Aplicaciones** y, a continuación, *webapi1*.
2. Seleccione **Ámbitos publicados**.
3. Como **ámbito**, escriba `Hello.Read` y para la descripción, escriba `Read access to hello`.
4. Como **ámbito**, escriba `Hello.Write` y para la descripción, escriba `Write access to hello`.
5. Haga clic en **Save**(Guardar).

Los ámbitos publicados se pueden utilizar para conceder a una aplicación cliente permiso para la API web.

## <a name="grant-permissions"></a>Concesión de permisos

Para llamar a una API web protegida desde una aplicación, deberá conceder permisos de aplicación a la API. En el tutorial de requisitos previos, ha creado una aplicación web en Azure AD B2C denominada *app1*. Puede usar esta aplicación para llamar a la API web.

1. Seleccione **Aplicaciones** y, a continuación, *nativeapp1*.
2. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
3. En el menú desplegable **Seleccionar API**, seleccione *webapi1*.
4. En el menú desplegable **Seleccionar ámbitos**, seleccione los ámbitos **Hello.Read** y **Hello.Write** que definió previamente.
5. Haga clic en **OK**.

Un usuario se autentica con Azure AD B2C para usar la aplicación web de escritorio WPF. La aplicación de escritorio obtiene una concesión de autorización de Azure AD B2C para acceder a la API web protegida.

## <a name="configure-the-sample"></a>Configuración del ejemplo

Ahora que se ha registrado la API web y tiene ámbitos definidos, debe configurar el código de la API web para usar el inquilino de Azure AD B2C. En este tutorial, configurará una aplicación web de ejemplo Node.js que puede descargar desde GitHub. 

[Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) o clone la aplicación web de ejemplo desde GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```
El ejemplo de API web de Node.js usa la biblioteca de Passport.js para permitir que Azure AD B2C proteja las llamadas a la API. 

1. Abra el archivo `index.js` .
2. Configure el ejemplo con la información de registro del inquilino de Azure AD B2C. Cambie las siguientes líneas de código:

    ```nodejs
    var tenantID = "<your-tenant-name>.onmicrosoft.com";
    var clientID = "<application-ID>";
    var policyName = "B2C_1_signupsignin1";
    ```

## <a name="run-the-sample"></a>Ejecución del ejemplo

1. Inicie un símbolo del sistema de Node.js.
2. Cambie al directorio que contiene el ejemplo de Node.js. Por ejemplo, `cd c:\active-directory-b2c-javascript-nodejs-webapi`.
3. Ejecute los comandos siguientes:
    ```
    npm install && npm update
    ```
    ```
    node index.js
    ```

### <a name="run-the-desktop-application"></a>Ejecución de la aplicación de escritorio

1. Abra la solución **active-directory-b2c-wpf** en Visual Studio.
2. Presione **F5** para ejecutar la aplicación de escritorio.
3. Inicie sesión con la dirección de correo electrónico y la contraseña que usó en el tutorial [Authenticate users with Azure Active Directory B2C in a desktop app tutorial](active-directory-b2c-tutorials-desktop-app.md) (Autenticación de los usuarios con Azure Active Directory B2C en una aplicación de escritorio).
4. Haga clic en el botón **Llamada API**. 

La aplicación de escritorio realiza una solicitud a la API web y obtiene una respuesta con el nombre para mostrar del usuario que ha iniciado la sesión. Su aplicación de escritorio protegida llama a la API web protegida en el inquilino de Azure AD B2C.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Actualizar el ejemplo para que use la aplicación

> [!div class="nextstepaction"]
> [Tutorial: Adición de proveedores de identidades a las aplicaciones en Azure Active Directory B2C](tutorial-add-identity-providers.md)
