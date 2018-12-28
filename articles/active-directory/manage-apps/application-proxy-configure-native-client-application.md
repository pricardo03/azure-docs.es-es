---
title: 'Publicación de aplicaciones cliente nativas: Azure AD | Microsoft Docs'
description: Explica cómo habilitar las aplicaciones cliente nativas para comunicarse con el conector del proxy de la aplicación de Azure AD para proporcionar acceso remoto seguro a las aplicaciones locales.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: a73e652c74f9d88f1e066de190834fc033c13cf0
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135533"
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Habilitación de las aplicaciones cliente nativas para interactuar con el proxy de la aplicación

Además de las aplicaciones web, el proxy de la aplicación de Azure Active Directory también puede utilizarse para publicar las aplicaciones cliente nativas que se configuran con la Biblioteca de autenticación de Azure AD (ADAL). Las aplicaciones cliente nativas son distintas de las aplicaciones web porque se instalan en un dispositivo, mientras se tiene acceso a las aplicaciones web a través de un explorador. 

El proxy de la aplicación admite aplicaciones cliente nativas al aceptar tokens emitidos por Azure AD que se envían en el encabezado. El servicio de proxy de la aplicación realiza la autenticación en nombre de los usuarios. Esta solución no utiliza tokens de aplicación para la autenticación. 

![Relación entre los usuarios finales, Azure Active Directory y las aplicaciones publicadas](./media/application-proxy-configure-native-client-application/richclientflow.png)

Utilice la biblioteca de Autenticación de Azure AD, que se encarga de la autenticación y admite muchos de los entornos de cliente, para publicar aplicaciones nativas. El proxy de la aplicación se adapta a la [aplicación nativa para el escenario de Web API](../develop/native-app.md). 

Este artículo le guiará por los cuatro pasos para publicar una aplicación nativa con el proxy de aplicación y la biblioteca de Autenticación de Azure AD. 

## <a name="step-1-publish-your-application"></a>Paso 1: Publicación de la aplicación
Publique su aplicación de proxy al igual que haría con cualquier otra aplicación y asigne a los usuarios el acceso a la aplicación. Para más información, consulte [Publicación de aplicaciones mediante el proxy de aplicación de Azure AD](application-proxy-add-on-premises-application.md).

## <a name="step-2-configure-your-application"></a>Paso 2: Configuración de la aplicación
Configure su aplicación nativa de la manera siguiente:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Vaya a **Azure Active Directory** > **Registros de aplicaciones**.
3. Seleccione **Nuevo registro de aplicaciones**.
4. Especifique un nombre para la aplicación, seleccione **Nativo** como tipo de aplicación y proporcione el URI de redirección para la aplicación. 

   ![Creación de un registro de aplicaciones](./media/application-proxy-configure-native-client-application/create.png)
5. Seleccione **Crear**.

Para más información sobre cómo crear un registro de aplicación, consulte [Integración de aplicaciones con Azure Active Directory](../develop/quickstart-v1-integrate-apps-with-azure-ad.md).


## <a name="step-3-grant-access-to-other-applications"></a>Paso 3: Conceder acceso a otras aplicaciones
Habilite la aplicación nativa para que se exponga a otras aplicaciones en el directorio:

1. Todavía en **Registros de aplicaciones**, seleccione la aplicación nativa que acaba de crear.
2. Seleccione **Permisos de API**.
3. Seleccione **Agregar un permiso**.
4. Abra el primer paso, **Seleccionar una API**.
5. Use la barra de búsqueda para encontrar la aplicación de proxy de la aplicación que ha publicado en la primera sección. Elija la aplicación y haga clic en **Seleccionar**. 

   ![Búsqueda de la aplicación de proxy](./media/application-proxy-configure-native-client-application/select_api.png)
6. Abra el segundo paso, **Seleccionar permisos**.
7. Utilice la casilla para conceder el acceso de la aplicación nativa a la aplicación de proxy y, después, haga clic en **Seleccionar**.

   ![Concesión de acceso a la aplicación de proxy](./media/application-proxy-configure-native-client-application/select_perms.png)
8. Seleccione **Listo**.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>Paso 4: Editar la biblioteca de autenticación de Active Directory
Edite el código de la aplicación nativa en el contexto de autenticación de Active Directory Authentication Library (ADAL) para incluir el siguiente texto:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = await authContext.AcquireTokenAsync("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

Las variables del código de ejemplo deben reemplazarse como sigue:

* **Tenant ID** puede encontrarse en Azure Portal. Vaya a **Azure Active Directory** > **Propiedades** y copie el identificador de directorio. 
* **External URL** es la dirección URL de front-end que ha especificado en la aplicación proxy. Para buscar este valor, vaya a la sección **Proxy de la aplicación** de la aplicación de proxy.
* La variable **App ID** de la aplicación nativa se encuentra en la página **Propiedades** de la aplicación nativa.
* **Redirect URI of the native app** se puede encontrar en la página**URI de redirección** de la aplicación nativa.

Una vez que se edita la ADAL con estos parámetros, los usuarios deberían poder autenticarse en las aplicaciones cliente nativas incluso si están fuera de la red corporativa. 

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el flujo de la aplicación nativa, consulte el escenario de [Aplicación nativa a API web](../develop/native-app.md).

Más información sobre cómo configurar el [inicio de sesión único para el proxy de la aplicación](what-is-single-sign-on.md#single- sign-on-methods)
