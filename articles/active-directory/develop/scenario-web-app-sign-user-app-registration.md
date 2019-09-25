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
ms.openlocfilehash: 0bdf04014d7b0382913c0a4094f7474686658441
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086698"
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

### <a name="register-an-app-using-azure-portal"></a>Registro de una aplicación mediante Azure Portal

> [!NOTE]
> El portal que se debe usar varía en función de si la aplicación se ejecuta en la nube pública de Microsoft Azure o en una nube soberana o nacional. Para obtener más información, consulte [Nubes nacionales](./authentication-national-cloud.md#app-registration-endpoints).

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft. Como alternativa, inicie sesión en la nube nacional de Azure Portal que elija.
1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
1. En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory** y, después, seleccione **Registros de aplicaciones** > **Nuevo registro**.
1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
   1. Elija los tipos de cuenta admitidos para la aplicación. (Vea [Tipos de cuenta admitidos](./v2-supported-account-types.md)).
   1. En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `AspNetCore-WebApp`.
   1. En **URI de redireccionamiento**, agregue el tipo de aplicación y el destino de URI que aceptará las respuestas de token devueltas después de una autenticación correcta. Por ejemplo, `https://localhost:44321/`.  Seleccione **Registrar**.
1. Seleccione el menú **Autenticación** y, a continuación, agregue la siguiente información:
   1. En **URL de respuesta**, agregue `https://localhost:44321/signin-oidc`.
   1. En la sección **Configuración avanzada**, establezca **Dirección URL de cierre de sesión** en `https://localhost:44321/signout-oidc`.
   1. En **Concesión implícita**, marque **Tokens de identificador**.
   1. Seleccione **Guardar**.

### <a name="register-an-app-using-powershell"></a>Registro de una aplicación mediante PowerShell

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
