---
title: 'Tutorial: Habilitación de la autenticación en una aplicación web mediante Azure Active Directory B2C | Microsoft Docs'
description: Tutorial sobre cómo usar Azure Active Directory B2C para proporcionar inicios de sesión de usuario en una aplicación web de ASP.NET.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: bcfd1ef02c68de7709cb8642b94f23a6884ea156
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464758"
---
# <a name="tutorial-enable-authentication-in-a-web-application-using-azure-active-directory-b2c"></a>Tutorial: Habilitación de la autenticación en una aplicación web mediante Azure Active Directory B2C

En este tutorial se muestra cómo usar Azure Active Directory (Azure AD) B2C para registrar e iniciar sesión con usuarios en una aplicación web de ASP.NET. Azure AD B2C permite que las aplicaciones puedan autenticarse en cuentas de las redes sociales, cuentas de empresa y cuentas de Azure Active Directory mediante protocolos estándar abiertos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Actualizar la aplicación en Azure AD B2C
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- [Cree flujos de usuario](tutorial-create-user-flows.md) para habilitar las experiencias de usuario en la aplicación.
- Instalar [Visual Studio 2019](https://www.visualstudio.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.

## <a name="update-the-application"></a>Actualizar la aplicación

En el tutorial que completó como parte de los requisitos previos, agregó una aplicación web en Azure AD B2C. Para habilitar la comunicación con el ejemplo en este tutorial, deberá agregar un identificador URI de redirección a la aplicación en Azure AD B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y, a continuación, seleccione la aplicación *webapp1*.
5. En **URL de respuesta**, agregue `https://localhost:44316`.
6. Seleccione **Guardar**.
7. En la página de propiedades, registre el identificador de la aplicación que usará cuando configure la aplicación web.
8. Seleccione **Claves**, después, **Generar clave** y, finalmente, **Guardar**. Registre la clave que usará cuando configure la aplicación web.

## <a name="configure-the-sample"></a>Configuración del ejemplo

En este tutorial, va a configurar un ejemplo que puede descargar desde GitHub. El ejemplo usa ASP.NET para proporcionar una lista sencilla de tareas pendientes. El ejemplo utiliza [componentes de middleware de Microsoft OWIN](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/). [Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) o clone el ejemplo de GitHub. Asegúrese de extraer el archivo de ejemplo en una carpeta donde la longitud total de caracteres de la ruta de acceso sea inferior a 260.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Los dos proyectos siguientes están en la solución de ejemplo:

- **TaskWebApp**: para crear y editar una lista de tareas. El ejemplo utiliza el flujo de usuario de **registro o de inicio de sesión** para que los usuarios se registren o inicien sesión.
- **TaskService**: admite la funcionalidad para crear, leer, actualizar y eliminar la lista de tareas. Azure AD B2C protege la API y TaskWebApp la llama.

Puede cambiar el ejemplo para que use la aplicación que está registrada en su inquilino, la cual incluye el identificador de aplicación y la clave que registró anteriormente. También puede configurar los flujos de usuario que ha creado. El ejemplo define los valores de configuración como valores en el archivo Web.config. Para cambiar estos valores:

1. Abra la solución **B2C-WebAPI-DotNet** en Visual Studio.
2. En el proyecto **TaskWebApp**, abra el archivo **Web.config**. Reemplace el valor de `ida:Tenant` con el nombre del inquilino que ha creado. Reemplace el valor de `ida:ClientId` con el identificador que ha registrado. Reemplace el valor de `ida:ClientSecret` con la clave que ha registrado. Debe codificar en XML el secreto de cliente antes de agregarlo al archivo Web.config.
3. En el archivo **Web.config**, reemplace el valor de `ida:SignUpSignInPolicyId` con `b2c_1_signupsignin1`. Reemplace el valor de `ida:EditProfilePolicyId` con `b2c_1_profileediting1`. Reemplace el valor de `ida:ResetPasswordPolicyId` con `b2c_1_passwordreset1`.


## <a name="run-the-sample"></a>Ejecución del ejemplo

1. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **TaskWebApp** y haga clic en **Establecer como proyecto de inicio**.
2. Presione **F5**. Inicia el explorador predeterminado en la dirección del sitio web local `https://localhost:44316/`.

### <a name="sign-up-using-an-email-address"></a>Registro con una dirección de correo electrónico

1. Haga clic en **Registrarse o en Iniciar sesión** para registrarse como un usuario de la aplicación. Se usa el flujo de usuario **b2c_1_signupsignin1**.
2. Azure AD B2C presenta una página de inicio de sesión con un vínculo de registro. Como no tiene aún una cuenta, seleccione **Registrarse ahora**. El flujo de trabajo del registro presenta una página para recopilar y verificar la identidad del usuario con una dirección de correo electrónico. El flujo de trabajo de registro también recopila la contraseña del usuario y los atributos requeridos definidos en el flujo de usuario.
3. Utilice una dirección de correo electrónico válida y valídela mediante el código de verificación. Establezca una contraseña. Especifique valores para los atributos solicitados.

    ![Página de registro que se muestra como parte del flujo de trabajo de inicio de sesión o registro](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.PNG)

4. Haga clic en **Crear** para crear una cuenta local en el inquilino de Azure AD B2C.

Ahora el usuario puede utilizar su dirección de correo electrónico para iniciar sesión y usar la aplicación web.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Actualizar la aplicación en Azure AD B2C
> * Configurar el ejemplo para que use la aplicación
> * Registrarse mediante el flujo de usuario

> [!div class="nextstepaction"]
> [Tutorial: Uso de Azure Active Directory B2C para proteger una API web de ASP.NET](active-directory-b2c-tutorials-web-api.md)
