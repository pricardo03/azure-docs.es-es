---
title: Usar la biblioteca de autenticación de Microsoft (MSAL) en las nubes nacionales - plataforma Microsoft identity
description: Biblioteca de autenticación de Microsoft (MSAL) permite a los desarrolladores de aplicaciones adquirir tokens para llamar a las API de web segura. Estas API web puede ser su propia API web, otras APIs Microsoft, las API web de socios o Microsoft Graph. MSAL admite varias plataformas y arquitecturas de aplicación.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9958356cae3c486ecf68e280f33d63c6a537b14
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235265"
---
# <a name="use-msal-in-a-national-cloud-environment"></a>Uso de MSAL en un entorno de nube nacionales

[Nubes nacionales](authentication-national-cloud.md) son instancias físicamente aisladas de Azure. Estas regiones de Azure le permiten asegurarse de que se respetan los requisitos de cumplimiento, soberanía y residencia de datos dentro de los límites geográficos.

Además de la nube de Microsoft en todo el mundo, la biblioteca de autenticación de Microsoft (MSAL) permite a los desarrolladores de aplicaciones en nubes nacionales adquirir tokens con el fin de autenticar y llamar a las API de web segura. Estas API web puede ser otro APIs Microsoft o Microsoft Graph.

Incluida la nube global, Azure Active Directory (Azure AD) se implementa en las nubes nacionales siguientes:  

- Azure Government
- Azure China 21Vianet
- Azure Alemania

Esta guía muestra cómo iniciar sesión en el trabajo y cuentas educativas, obtener un acceso token y llamar a Microsoft Graph API en el [en la nube de Azure Government](https://azure.microsoft.com/global-infrastructure/government/) entorno.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que cumplen estos requisitos previos.

### <a name="choose-the-appropriate-identities"></a>Elija las identidades adecuadas

[Azure Government](https://docs.microsoft.com/azure/azure-government/) las aplicaciones pueden utilizar las identidades de Azure AD Government e identidades pública de Azure AD para autenticar a los usuarios. Dado que puede usar cualquiera de estas identidades, debe decidir qué extremo de la autoridad debe elegir para su escenario:

- Azure AD Public: Suele usar si su organización ya tiene un inquilino pública de Azure AD al soporte técnico de Office 365 (público o GCC) u otra aplicación.
- Gobierno de Azure AD: Suele usar si su organización ya tiene un inquilino de Azure Government AD al soporte técnico de Office 365 (GCC High o DoD) o es crear a un nuevo inquilino en Azure Government de AD.

Una vez decidido, una consideración especial es donde realiza el registro de aplicación. Si elige identidades pública de Azure AD para la aplicación de Azure Government, debe registrar la aplicación en el inquilino de Azure AD pública.

### <a name="get-an-azure-government-subscription"></a>Obtener una suscripción de Azure Government

Para obtener una suscripción de Azure Government, consulte [administración y conectarse a su suscripción en Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-subscriptions).

Si no tiene una suscripción de Azure Government, cree un [cuenta gratuita](https://azure.microsoft.com/global-infrastructure/government/request/) antes de comenzar.

## <a name="javascript"></a>JavaScript

### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación

1. Inicie sesión en el [Azure Portal](https://portal.azure.us/).
    
   Para buscar los extremos del portal de Azure para otras nubes nacionales, consulte [puntos de conexión de registro de aplicación](authentication-national-cloud.md#app-registration-endpoints).

1. Si la cuenta proporciona acceso a más de un inquilino, seleccione su cuenta en la esquina superior derecha y establezca la sesión del portal en Azure AD que desee de inquilinos.
1. Vaya a la [registros de aplicaciones](https://aka.ms/ra/ff) página en la plataforma de identidad de Microsoft para desarrolladores.
1. Cuando se abra la página **Registrar una aplicación**, escriba el nombre de su aplicación.
1. En **admite tipos de cuenta**, seleccione **cuentas en el directorio de cualquier organización**.
1. En el **URI de redireccionamiento** sección, seleccione el **Web** plataforma y establezca el valor a la dirección URL de la aplicación se basa en el servidor web. Consulte las secciones siguientes para obtener instrucciones sobre cómo establecer y obtener la dirección URL de redireccionamiento en Visual Studio y nodo.
1. Seleccione **Registrar**.
1. En la página de **Información general** de la aplicación, anote el valor en **Id. de aplicación (cliente)** .
1. Este tutorial requiere que habilite el [flujo de concesión implícita](v2-oauth2-implicit-grant-flow.md). En el panel izquierdo de la aplicación registrada, seleccione **autenticación**.
1. En **configuración avanzada**, en **concesión implícita**, seleccione el **los tokens de identificador** y **tokens de acceso** casillas de verificación. Los tokens de identificador y tokens de acceso son necesarios porque esta aplicación necesita para los usuarios inicien sesión y llamar a una API.
1. Seleccione **Guardar**.

### <a name="step-2--set-up-your-web-server-or-project"></a>Paso 2:  Configurar el servidor web o proyecto

- [Descargar los archivos de proyecto](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) para un servidor web local, por ejemplo, nodo.

  o

- [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).

A continuación, vaya a [configurar JavaScript SPA](#step-4-configure-your-javascript-spa) para configurar el ejemplo de código antes de ejecutarlo.

### <a name="step-3-use-the-microsoft-authentication-library-to-sign-in-the-user"></a>Paso 3: Use la biblioteca de autenticación de Microsoft para iniciar sesión el usuario

Siga los pasos de la [JavaScript tutorial](tutorial-v2-javascript-spa.md#create-your-project) para crear el proyecto e integrar con MSAL para iniciar sesión en el usuario.

### <a name="step-4-configure-your-javascript-spa"></a>Paso 4: Configuración de JavaScript SPA

En el archivo `index.html` creado durante la instalación del proyecto, agregue la información de registro de aplicación. Agregue el código siguiente en la parte superior, dentro de las etiquetas `<script></script>` en el cuerpo del archivo `index.html`:

```javascript
const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.us/Enter_the_Tenant_Info_Here",
        }
}

const graphConfig = {
        graphEndpoint: "https://graph.microsoft.us",
        graphScopes: ["user.read"],
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

En el código:

- `Enter_the_Application_Id_here` es el **Id. de aplicación (cliente)** valor para la aplicación que se ha registrado.
- `Enter_the_Tenant_Info_Here` se establece en una de las opciones siguientes:
    - Si la aplicación admite **cuentas en el directorio de esta organización**, reemplace este valor con el identificador del inquilino o nombre (por ejemplo, contoso.microsoft.com) del inquilino.
    - Si la aplicación admite **cuentas en el directorio de cualquier organización**, reemplace este valor por `organizations`.
    
    Para buscar los puntos de conexión de autenticación para todas las nubes nacionales, consulte [puntos de conexión de autenticación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints).

    > [!NOTE]
    > No se admiten cuentas personales de Microsoft en las nubes nacionales.
  
- `graphEndpoint` es el punto de conexión de Microsoft Graph para la nube de Microsoft para el gobierno estadounidense.

   Para buscar los puntos de conexión de Microsoft Graph para todas las nubes nacionales, consulte [puntos de conexión de Microsoft Graph en nubes nacionales](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints).

## <a name="net"></a>.NET

Puede usar MSAL.NET a los usuarios inicien sesión, adquirir tokens y llamar a Microsoft Graph API en nubes nacionales.

Los siguientes tutoriales muestran cómo compilar una aplicación Web MVC de .NET Core 2.2. La aplicación usa OpenID Connect para los usuarios inicien sesión con una cuenta profesional y educativa en una organización que pertenece a una nube nacional.

- Para los usuarios inicien sesión y adquirir tokens, siga [este tutorial](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-4-Sovereign#build-an-aspnet-core-web-app-signing-in-users-in-sovereign-clouds-with-the-microsoft-identity-platform).
- Para llamar a Microsoft Graph API, siga [este tutorial](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-4-Sovereign-Call-MSGraph#using-the-microsoft-identity-platform-to-call-the-microsoft-graph-api-from-an-an-aspnet-core-2x-web-app-on-behalf-of-a-user-signing-in-using-their-work-and-school-account-in-microsoft-national-cloud).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

- [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Alemania](https://docs.microsoft.com/azure/germany/)