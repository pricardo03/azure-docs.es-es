---
title: Acceso a las API con Azure AD Application Proxy localmente
description: Proxy de aplicación de Azure Active Directory permite que las aplicaciones nativas acceder de forma segura a las API y lógica de negocios hospedar de forma local o en las máquinas virtuales en la nube.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: 47f6678f8d18d734176d964f18a6febecea957ab
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481420"
---
# <a name="secure-access-to-on-premises-apis-with-azure-ad-application-proxy"></a>Proteger el acceso a API locales con Azure AD Application Proxy

Puede tener lógica de negocios API que se ejecutan de forma local u hospedado en máquinas virtuales en la nube. Las aplicaciones nativas de Android, iOS, Mac o Windows necesitan interactuar con los puntos de conexión de API para usar datos o proporcionar la interacción del usuario. Proxy de aplicación de Azure AD y el [Azure Active Directory Authentication bibliotecas (ADAL)](/azure/active-directory/develop/active-directory-authentication-libraries) deje que sus aplicaciones nativas obtener acceso seguro a las API de forma local. Azure Active Directory Application Proxy es una solución más rápida y más segura que la apertura de puertos de firewall y controlar la autenticación y autorización en el nivel de aplicación. 

Este artículo le guiará a través de una solución de Azure AD Application Proxy para hospedar un servicio de API web que pueden tener acceso las aplicaciones nativas. 

## <a name="overview"></a>Información general

El siguiente diagrama muestra una manera tradicional para publicar API locales. Este enfoque requiere abrir puertos de entrada 80 y 443.

![Acceso a la API tradicional](./media/application-proxy-secure-api-access/overview-publish-api-open-ports.png)

El diagrama siguiente muestra cómo puede usar a Azure AD Application Proxy para publicar API de forma segura sin tener que abrir los puertos de entrada:

![Acceso de API de Proxy de aplicación de AD Azure](./media/application-proxy-secure-api-access/overview-publish-api-app-proxy.png)

El Proxy de aplicación de Azure AD constituye la espina dorsal de la solución, trabaja como un punto de conexión público para el acceso de API y que proporciona autenticación y autorización. Puede tener acceso a las API desde una amplia variedad de plataformas mediante el uso de la [ADAL](/azure/active-directory/develop/active-directory-authentication-libraries) bibliotecas. 

Puesto que la autorización y autenticación de Azure AD Application Proxy se basan en Azure AD, puede usar el acceso condicional de Azure AD para asegurarse de que solo los dispositivos de confianza pueden tener acceso a las API publicadas a través de Proxy de aplicación. Use Azure AD Join o Unidos a Azure AD híbrido para escritorios y administrados con Intune para dispositivos. También puede aprovechar las características de Azure Active Directory Premium, como la autenticación multifactor de Azure y la seguridad de aprendizaje de seguridad de máquina de [Azure Identity Protection](/azure/active-directory/active-directory-identityprotection).

## <a name="prerequisites"></a>Requisitos previos

Para seguir este tutorial, necesita:

- Acceso de administrador a un directorio de Azure, con una cuenta que puede crear y registrar las aplicaciones
- La API web de ejemplo y aplicaciones de cliente nativo de [https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp](https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp) 

## <a name="publish-the-api-through-application-proxy"></a>Publicar la API a través de Proxy de aplicación

Para publicar una API fuera de la intranet a través del Proxy de aplicación, siga el mismo patrón que para publicar aplicaciones web. Para más información, consulte [Tutorial: Agregar una aplicación de forma local para el acceso remoto a través del Proxy de aplicación en Azure Active Directory](application-proxy-add-on-premises-application.md).

Para publicar la API de web SecretAPI a través del Proxy de aplicación:

1. Generar y publicar el proyecto de ejemplo SecretAPI como una aplicación web ASP.NET en el equipo local o intranet. Asegúrese de que puede tener acceso a la aplicación web localmente. 
   
1. En el [portal Azure](https://portal.azure.com), seleccione **Azure Active Directory** en el panel de navegación izquierdo. A continuación, en el **Introducción** página, seleccione **aplicaciones empresariales**.
   
1. En la parte superior de la **aplicaciones empresariales - todas las aplicaciones** página, seleccione **nueva aplicación**.
   
1. En el **agregar una aplicación** página, en **agregar su propia aplicación**, seleccione **aplicación local**. 
   
1. Si no tiene instalado un conector de Proxy de aplicación, se le pedirá que instalarlo. Seleccione **descargar conector del Proxy de aplicación** para descargar e instalar el conector. 
   
1. Una vez haya instalado el conector del Proxy de aplicación, en el **agregar su propia aplicación en el entorno local** página:
   
   1. Escriba *SecretAPI* junto a **nombre**.
      
   1. Escriba la dirección URL que se use para tener acceso a la API desde dentro de su intranet junto a **dirección Url interna**. 
      
   1. Asegúrese de que **autenticación previa** está establecido en **Azure Active Directory**. 
      
   1. Seleccione **agregar** en la parte superior de la página y espere a que la aplicación que se va a crear.
   
   ![Agregar aplicación de API](./media/application-proxy-secure-api-access/3-add-api-app.png)
   
1. En el **aplicaciones empresariales - todas las aplicaciones** página, seleccione el **SecretAPI** app. 
   
1. En el **SecretAPI - información general sobre** página, seleccione **propiedades** en el panel de navegación izquierdo.
   
1. No desea que las API que estén disponibles para los usuarios finales de la **MyApps** panel, así que establezca **Visible para los usuarios** a **No** en la parte inferior de la **propiedades**página y, a continuación, seleccione **guardar**.
   
   ![No es visible para los usuarios](./media/application-proxy-secure-api-access/5-not-visible-to-users.png)
   
Ha publicado la API web a través de Azure AD Application Proxy. Ahora, agregue los usuarios que pueden acceder a la aplicación. 

1. En el **SecretAPI - información general sobre** página, seleccione **usuarios y grupos** en el panel de navegación izquierdo.
   
1. En el **usuarios y grupos** página, seleccione **Agregar usuario**.  
   
1. En el **Agregar asignación** página, seleccione **usuarios y grupos**. 
   
1. En el **usuarios y grupos** página, busque y seleccione los usuarios que pueden acceder a la aplicación, al menos, incluido usted mismo. Después de seleccionar todos los usuarios, seleccione **seleccione**. 
   
   ![Seleccionar y asignar el usuario](./media/application-proxy-secure-api-access/7-select-admin-user.png)
   
1. En el **Agregar asignación** página, seleccione **asignar**. 

> [!NOTE]
> Las API que usan la autenticación integrada de Windows pueden requerir [pasos adicionales](/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd).

## <a name="register-the-native-app-and-grant-access-to-the-api"></a>Registrar la aplicación nativa y conceder acceso a la API

Aplicaciones nativas son programas desarrollados para usar un dispositivo o plataforma concreta. Antes de la aplicación nativa puede conectarse y tener acceso a una API, debe registrarlo en Azure AD. Los pasos siguientes muestran cómo registrar una aplicación nativa y darle acceso a la API publican a través de Proxy de aplicación web.

Para registrar la aplicación nativa AppProxyNativeAppSample:

1. En Azure Active Directory **Introducción** página, seleccione **registros de aplicaciones**y en la parte superior de la **registros de aplicaciones** panel, seleccione **nuevo registro** .
   
1. En el **registrar una aplicación** página:
   
   1. En **nombre**, escriba *AppProxyNativeAppSample*. 
      
   1. En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft). 
      
   1. En **dirección URL de redireccionamiento**, lista desplegable y seleccione **cliente público (mobile y desktop)** y, a continuación, escriba *https:\//appproxynativeapp*. 
      
   1. Seleccione **registrar**y espere a que la aplicación se registre correctamente. 
      
      ![Nuevo registro de aplicaciones](./media/application-proxy-secure-api-access/8-create-reg-ga.png)
   
Ya ha registrado la aplicación AppProxyNativeAppSample en Azure Active Directory. Para dar acceso de la aplicación nativa a la API web de SecretAPI:

1. En Azure Active Directory **Introducción** > **registros de aplicaciones** página, seleccione el **AppProxyNativeAppSample** app. 
   
1. En el **AppProxyNativeAppSample** página, seleccione **permisos de API** en el panel de navegación izquierdo. 
   
1. En el **permisos de API** página, seleccione **agregar un permiso**.
   
1. En la primera **permisos de solicitud API** página, seleccione el **API que usa mi organización** ficha y, a continuación, busque y seleccione **SecretAPI**. 
   
1. En el siguiente **permisos de solicitud API** , seleccione la casilla de verificación junto a **user_impersonation**y, a continuación, seleccione **agregar permisos**. 
   
    ![Selección de una API](./media/application-proxy-secure-api-access/10-secretapi-added.png)
   
1. En el **permisos de API** página, puede seleccionar **conceder consentimiento del Administrador de Contoso** para impedir que otros usuarios tengan individualmente dé su consentimiento a la aplicación. 

## <a name="configure-the-native-app-code"></a>Configure el código de aplicación nativa

El último paso es configurar la aplicación nativa. El siguiente fragmento de código desde el *Form1.cs* archivo en la aplicación de ejemplo NativeClient hace que la biblioteca ADAL adquirir el token para solicitar la llamada API y adjuntarlo como portador al encabezado de la aplicación. 
   
   ```csharp
       AuthenticationResult result = null;
       HttpClient httpClient = new HttpClient();
       authContext = new AuthenticationContext(authority);
       result = await authContext.AcquireTokenAsync(todoListResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
       
       // Append the token as bearer in the request header.
       httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
       
       // Call the API.
       HttpResponseMessage response = await httpClient.GetAsync(todoListBaseAddress + "/api/values/4");
   
       // MessageBox.Show(response.RequestMessage.ToString());
       string s = await response.Content.ReadAsStringAsync();
       MessageBox.Show(s);
   ```
   
Para configurar la aplicación nativa para conectarse a Azure Active Directory y llamar a la API de Proxy de App, actualice los valores de marcador de posición en el *App.config* archivo de la aplicación de ejemplo NativeClient con valores de Azure AD: 

- Pegar la **Id. de directorio (inquilino)** en el `<add key="ida:Tenant" value="" />` campo. Puede encontrar y copiar este valor (GUID) de la **Introducción** página de cualquiera de sus aplicaciones. 
  
- Pegue el AppProxyNativeAppSample **Id. de aplicación (cliente)** en el `<add key="ida:ClientId" value="" />` campo. Puede encontrar y copiar este valor (GUID) de la AppProxyNativeAppSample **Introducción** página.
  
- Pegue el AppProxyNativeAppSample **URI de redireccionamiento** en el `<add key="ida:RedirectUri" value="" />` campo. Puede encontrar y copiar este valor (URI) de la AppProxyNativeAppSample **autenticación** página. 
  
- Pegue el SecretAPI **URI de Id. de aplicación** en el `<add key="todo:TodoListResourceId" value="" />` campo. Puede encontrar y copiar este valor (URI) de la SecretAPI **exponer una API** página.
  
- Pegue el SecretAPI **URL de página principal** en el `<add key="todo:TodoListBaseAddress" value="" />` campo. Puede encontrar y copiar este valor (una dirección URL) de la SecretAPI **Branding** página.

Después de configurar los parámetros, compile y ejecute la aplicación nativa. Cuando se selecciona el **sesión** botón, la aplicación le permite iniciar sesión y, a continuación, se muestra una pantalla de éxito para confirmar que TI correctamente conectado a la SecretAPI.

![Correcto](./media/application-proxy-secure-api-access/success.png)

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Agregar una aplicación de forma local para el acceso remoto a través del Proxy de aplicación en Azure Active Directory](application-proxy-add-on-premises-application.md)
- [Inicio rápido: Configurar una aplicación cliente para tener acceso a las API web](../develop/quickstart-configure-app-access-web-apis.md)
- [Cómo habilitar las aplicaciones cliente nativas interactuar con aplicaciones de proxy](application-proxy-configure-native-client-application.md)
