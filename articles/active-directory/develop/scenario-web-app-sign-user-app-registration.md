---
title: 'Aplicación Web que inician sesión los usuarios (registro de aplicación): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que inician sesión los usuarios (registro de aplicación)
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
ms.openlocfilehash: e6f512911811535818f4ad857c5c3b956870f619
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074551"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>Aplicación Web que los usuarios - registro de la aplicación inicia sesión

Esta página explica los detalles de registro de aplicación para una aplicación web que los usuarios inicia sesión.

Para registrar la aplicación, puede usar:

- El [web inicios rápidos de la aplicación](#register-an-app-using-the-quickstarts) -además de ser una excelente primera experiencia con la creación de una aplicación, guías de inicio rápido en el portal de Azure contienen un botón denominado **realizar este cambio para mí**. Puede usar este botón para establecer las propiedades que necesita, incluso para una aplicación existente. Deberá adaptar los valores de estas propiedades en su caso. En concreto, la dirección URL de API web para la aplicación probablemente será diferente de la propuesta de forma predeterminada, esto afectará también el URI de cierre de sesión.
- El portal de Azure para [registrar manualmente la aplicación](#register-an-app-using-azure-portal)
- Herramientas de línea de comandos y PowerShell

## <a name="register-an-app-using-the-quickstarts"></a>Registrar una aplicación con las guías de inicio rápido

Si navega a este vínculo, puede crear bootstrap la creación de la aplicación web:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

### <a name="register-an-app-using-azure-portal"></a>Registrar una aplicación mediante Azure portal

> [!NOTE]
> es diferente en función de si la aplicación se ejecuta en la nube pública de Microsoft Azure o en una nube soberana o nacional que use el portal. Para obtener más información, consulte [nubes nacionales](./authentication-national-cloud.md#app-registration-endpoints)

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft. Como alternativa, inicie sesión en el portal de Azure de elección de la nube nacional.
1. Si la cuenta proporciona acceso a más de un inquilino, seleccione su cuenta en la esquina superior derecha y establezca la sesión del portal en Azure AD que desee de inquilinos.
1. En el panel de navegación izquierdo, seleccione el **Azure Active Directory** de servicio y, a continuación, seleccione **registros de aplicaciones** > **nuevo registro**.
1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
   - Elija los tipos de cuenta compatibles para la aplicación (consulte [tipos de cuenta compatibles](./v2-supported-account-types.md))
   - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `AspNetCore-WebApp`.
   - En **dirección URL de respuesta**, agregue la dirección URL de respuesta de la aplicación, por ejemplo `https://localhost:44321/`y seleccione **registrar**.
1. Seleccione el menú **Autenticación** y, a continuación, agregue la siguiente información:
- En **Dirección URL de respuesta**, agregue `https://localhost:44321/signin-oidc` y seleccione **Registrar**.
- En el **configuración avanzada** sección, establezca **cerrar sesión URL** a `https://localhost:44321/signout-oidc`.
- En **Concesión implícita**, marque **Tokens de identificador**.
- Seleccione **Guardar**.

### <a name="register-an-app-using-powershell"></a>Registrar una aplicación mediante PowerShell

> [!NOTE]
> Actualmente Azure AD PowerShell solo crea las aplicaciones con los siguientes tipos de cuenta compatibles:
>
> - MyOrg (cuentas en este directorio de organización)
> - AnyOrg (cuentas en el directorio de cualquier organización).
>
> Si desea crear una aplicación que los usuarios inicia sesión con sus Accounts personales de Microsoft (por ejemplo, Skype, XBox, Outlook.com), primero puede crear una aplicación multiempresa (admite los tipos de cuenta = cuentas en el directorio de cualquier organización) y, a continuación, cambiar el `signInAudience` propiedad en el manifiesto de aplicación desde el portal de Azure. Esto se explica detalladamente en el paso [1.3](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) del tutorial de ASP.NET Core (y se puede generalizar para aplicaciones web en cualquier lenguaje).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la aplicación de código](scenario-web-app-sign-user-app-configuration.md)
