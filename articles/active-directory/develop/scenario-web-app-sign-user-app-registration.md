---
title: 'Aplicación web que inicia la sesión de los usuarios: registro de la aplicación- Plataforma de identidad de Microsoft'
description: Obtener información sobre cómo compilar una aplicación web que inicie la sesión de los usuarios (registro de la aplicación)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ab7c3699abdd5c094b1b14cd53f76023fa8c1ac
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71309597"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>Aplicación web que inicia la sesión de los usuarios: registro de la aplicación

En esta página se explican los detalles del registro de la aplicación para una aplicación web que inicia la sesión de los usuarios.

Para registrar la aplicación, puede usar lo siguiente:

- Las [guías de inicio rápido de la aplicación web](#register-an-app-using-the-quickstarts): además de ser una excelente primera experiencia con la creación de una aplicación, las guías de inicio rápido de Azure Portal incluyen un botón denominado **Hacer este cambio por mí**. Puede usar este botón para establecer las propiedades que necesita, incluso para una aplicación existente. Deberá adaptar los valores de estas propiedades a su propio caso. En concreto, la dirección URL de la API web para la aplicación probablemente será diferente de la URL propuesta de forma predeterminada, lo cual afectará también al URI de cierre de sesión.
- Azure Portal para [registrar la aplicación manualmente](#register-an-app-using-azure-portal)
- PowerShell y herramientas de línea de comandos

## <a name="register-an-app-using-the-quickstarts"></a>Registro de una aplicación con las guías de inicio rápido

Si navega a este vínculo, puede crear el arranque de la creación de la aplicación web:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

## <a name="register-an-app-using-azure-portal"></a>Registro de una aplicación mediante Azure Portal

> [!NOTE]
> El portal que se debe usar varía en función de si la aplicación se ejecuta en la nube pública de Microsoft Azure o en una nube soberana o nacional. Para obtener más información, consulte [Nubes nacionales](./authentication-national-cloud.md#app-registration-endpoints).


1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft. Como alternativa, inicie sesión en la nube nacional de Azure Portal que elija.
1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
1. En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory** y, después, seleccione **Registros de aplicaciones** > **Nuevo registro**.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

4. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
   1. Elija los tipos de cuenta admitidos para la aplicación. (Vea [Tipos de cuenta admitidos](./v2-supported-account-types.md)).
   1. En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `AspNetCore-WebApp`.
   1. En **URI de redireccionamiento**, agregue el tipo de aplicación y el destino de URI que aceptará las respuestas de token devueltas después de una autenticación correcta. Por ejemplo, `https://localhost:44321/`.  Seleccione **Registrar**.
1. Seleccione el menú **Autenticación** y, a continuación, agregue la siguiente información:
   1. En **URL de respuesta**, agregue `https://localhost:44321/signin-oidc` de tipo "Web".
   1. En la sección **Configuración avanzada**, establezca **Dirección URL de cierre de sesión** en `https://localhost:44321/signout-oidc`.
   1. En **Concesión implícita**, marque **Tokens de identificador**.
   1. Seleccione **Guardar**.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

4. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
   1. Elija los tipos de cuenta admitidos para la aplicación. (Vea [Tipos de cuenta admitidos](./v2-supported-account-types.md)).
   - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `MailApp-openidconnect-v2`.
   - En la sección URI de redirección (opcional), seleccione **Web** en el cuadro combinado y escriba los siguientes URI de redirección: `https://localhost:44326/`.
1. Seleccione **Registrar** para crear la aplicación.
1. Seleccione el menú **Autenticación** y, a continuación, agregue la siguiente información:
   - En la sección **Configuración avanzada** | **Concesión implícita**, marque **Tokens de id.** ya que en este ejemplo es necesario habilitar el [flujo de concesión implícito](v2-oauth2-implicit-grant-flow.md) para iniciar la sesión del usuario.
1. Seleccione **Guardar**.

# <a name="javatabjava"></a>[Java](#tab/java)

4. Cuando aparezca la página **Registrar una aplicación**, escriba un nombre descriptivo para la aplicación, por ejemplo, "java-webapp", seleccione "Cuentas en cualquier directorio organizativo y cuentas Microsoft personales (por ejemplo, Skype, Xbox, Outlook.com)" y elija "Aplicación web o API" como *Tipo de aplicación*.
1. Haga clic en **Registrar** para registrar la aplicación.
1. En el menú de la izquierda, haga clic en **Autenticación** y, en *URI de redirección*, seleccione "Web". Tendrá que escribir dos URI de redirección diferentes: uno para la página de inicio de sesión y otro para la página de usuarios de Graph. En ambos casos, debe usar el mismo host y número de puerto, seguido de "/msal4jsample/secure/aad" para la página de inicio de sesión y de "msal4jsample/graph/users" para la página de usuarios.
 De forma predeterminada, en el ejemplo se usa: 

    - `http://localhost:8080/msal4jsample/secure/aad`. 
    - `http://localhost:8080/msal4jsample/graph/users`

Haga clic en **Guardar**.

# <a name="pythontabpython"></a>[Python](#tab/python)

4. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
   - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `python-webapp`.
   - Cambie **Supported account types** (Tipos de cuenta compatibles) por **Cuentas en cualquier directorio organizativo y cuentas Microsoft personales (por ejemplo, Skype, Xbox, Outlook.com)** .
   - En la sección URI de redirección (opcional), seleccione **Web** en el cuadro combinado y escriba los siguientes URI de redirección: `http://localhost:5000/getAToken`.
1. Seleccione **Registrar** para crear la aplicación.
1. En la página **Información general** de la aplicación, busque el valor de **Id. de aplicación (cliente)** y regístrelo para usarlo más tarde. Lo necesitará para configurar el archivo de configuración de Visual Studio para este proyecto.
1. En la página Introducción de la aplicación, seleccione la sección **Autenticación**.
   - En la sección **Configuración avanzada**, establezca **Dirección URL de cierre de sesión** en `http://localhost:5000/logout`.
1. Seleccione **Guardar**.

---

## <a name="register-an-app-using-powershell"></a>Registro de una aplicación mediante PowerShell

> [!NOTE]
> Actualmente, Azure AD PowerShell solo crea las aplicaciones con los siguientes tipos de cuenta compatibles:
>
> - MyOrg (Solo las cuentas de este directorio organizativo)
> - AnyOrg (Cuentas en cualquier directorio organizativo)
>
> Si quiere crear una aplicación que inicie la sesión de los usuarios con sus cuentas personales de Microsoft (por ejemplo, Skype, XBox o Outlook.com), primero puede crear una aplicación multiinquilino (tipos de cuenta admitidos: cuentas de cualquier directorio de la organización) y, después, cambiar la propiedad `signInAudience` en el manifiesto de aplicación desde Azure Portal. Esto se explica detalladamente en el paso [1.3](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) del tutorial de ASP.NET Core (y se puede generalizar para las aplicaciones web en cualquier lenguaje).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración del código de la aplicación](scenario-web-app-sign-user-app-configuration.md)
