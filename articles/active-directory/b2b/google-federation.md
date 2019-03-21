---
title: Agregar Google como proveedor de identidades para B2B - Azure Active Directory | Microsoft Docs
description: Federación con Google para permitir el inicio de sesión en sus aplicaciones de Azure AD con las cuentas de Gmail propias de los usuarios
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d61f233b2eb901bcf1e6b5b4ff147893f918e8f
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58293318"
---
# <a name="add-google-as-an-identity-provider-for-b2b-guest-users"></a>Incorporación de Google como proveedor de identidades para los usuarios invitados de B2B

Gracias a la configuración de la federación con Google puede permitir a los usuarios invitados iniciar sesión en sus aplicaciones y recursos compartidos con sus propias cuentas de Google, sin tener que crear cuentas Microsoft (cuentas de servicio administradas) o de Azure AD.  
> [!NOTE]
> Los usuarios invitados de Google deben iniciar sesión con un vínculo que incluye el contexto del inquilino (por ejemplo, `https://myapps.microsoft.com/?tenantid=<tenant id>` o `https://portal.azure.com/<tenant id>`, o, en el caso de un dominio comprobado, `https://myapps.microsoft.com/<verified domain>.onmicrosoft.com`). Los vínculos directos a aplicaciones y los recursos también funcionan siempre que incluyan el contexto del inquilino. Actualmente, los usuarios invitados no pueden iniciar sesión con puntos de conexión sin contexto de inquilino. Por ejemplo, el uso de `https://myapps.microsoft.com`, `https://portal.azure.com` o el punto de conexión común de los equipos producirá un error.
 
## <a name="what-is-the-experience-for-the-google-user"></a>¿Cuál es la experiencia del usuario de Google?
Cuando envíe una invitación a un usuario de Gmail de Google, el usuario invitado debe acceder a sus aplicaciones o recursos compartidos mediante un vínculo que incluya el contexto del inquilino. Su experiencia varía en función de si ya han iniciado sesión en Google:
  - Si el usuario invitado no ha iniciado sesión en Google, se le pedirá que lo haga.
  - Si el usuario invitado ya ha iniciado sesión en Google, se pedirá que elija la cuenta que desea usar. Deben elegir la cuenta donde hayan recibido la invitación.

Si el usuario invitado ve un error de "encabezado demasiado largo", puede intentar eliminar las cookies, o abrir una ventana privada o de incógnito y probar a iniciar sesión de nuevo.

![Captura de pantalla muestra el inicio de sesión de Google en la página](media/google-federation/google-sign-in.png)

## <a name="step-1-configure-a-google-developer-project"></a>Paso 1: configuración de un proyecto de desarrollador de Google
En primer lugar, cree un nuevo proyecto en la consola de desarrolladores de Google para obtener un identificador y un secreto de cliente que pueda agregar después a Azure AD. 
1. Vaya a las API de Google de https://console.developers.google.com e inicie sesión con su cuenta de Google. Se recomienda utilizar una cuenta de Google compartida con el equipo.
2. Cree un nuevo proyecto: en el panel, seleccione **Crear proyecto** y, a continuación, **Crear**. En la página Nuevo proyecto, escriba un **Nombre de proyecto** y seleccione **Crear**.
   
   ![Captura de pantalla que muestra una nueva página de proyecto de Google](media/google-federation/google-new-project.png)

3. Asegúrese de que el nuevo proyecto está seleccionado en el menú del proyecto. A continuación, abra el menú de la esquina superior izquierda y seleccione **APIs & Services** (API y servicios)  > **Credentials** (Credenciales).

   ![Opción de credenciales de la captura de pantalla con la API de Google](media/google-federation/google-api.png)
 
4. Seleccione la pestaña **Pantalla de consentimiento de OAuth** y especifique un **nombre de aplicación**. (Deje intactos los valores restantes).

   ![Opción de pantalla de consentimiento de la captura de pantalla muestra la OAuth de Google](media/google-federation/google-oauth-consent-screen.png)

5. Desplácese hasta la sección **Dominios autorizados**  y escriba microsoftonline.com.

   ![Captura de pantalla que muestra la sección de dominios autorizado](media/google-federation/google-oauth-authorized-domains.png)

6. Seleccione **Guardar**.

7. Elija la pestaña **Credentials** (Credenciales). En el menú **Create credentials** (Crear credenciales), elija **OAuth client ID** (Id. de cliente de OAuth).

   ![Captura de pantalla muestra las APIs Google crea la opción de credenciales](media/google-federation/google-api-credentials.png)

8. En **Application type** (Tipo de aplicación), elija **Web application** (Aplicación web) y, en **Authorized redirect URIs** (URI de redireccionamiento autorizados), escriba los siguientes:
   - `https://login.microsoftonline.com` 
   - `https://login.microsoftonline.com/te/<directory id>/oauth2/authresp` <br>(donde `<directory id>` es el identificador del directorio)
   
     > [!NOTE]
     > Para buscar el identificador de directorio, vaya a https://portal.azure.com y, en **Azure Active Directory**, elija **Properties** (Propiedades) y copie el valor de **Directory ID** (Id. de directorio).

   ![Captura de pantalla muestra el indicador sección de URI de redireccionamiento](media/google-federation/google-create-oauth-client-id.png)

9. Seleccione **Crear**. Copie el identificador y el secreto de cliente, que se usarán al agregar el proveedor de identidades en el portal de Azure AD.

   ![Captura de pantalla muestra al cliente de OAuth de secreto de cliente y el Id.](media/google-federation/google-auth-client-id-secret.png)

## <a name="step-2-configure-google-federation-in-azure-ad"></a>Paso 2: configuración de la federación de Google en Azure AD 
Ahora podrá establecer el identificador y el secreto de cliente, ya sea escribiéndolo en el portal de Azure AD o con PowerShell. No olvide probar la configuración de la federación de Google; para ello, invítese a usted mismo mediante una dirección de Gmail e intente canjear la invitación con su cuenta de invitado de Google. 

#### <a name="to-configure-google-federation-in-the-azure-ad-portal"></a>Configuración de la federación de Google en el portal de Azure AD 
1. Vaya a [Azure Portal](https://portal.azure.com). En el panel izquierdo, seleccione **Azure Active Directory**. 
2. Seleccione **Relaciones organizativas**.
3. Seleccione **Proveedores de identidades** y haga clic en el botón de **Google**.
4. Escriba un nombre. A continuación, escriba el identificador y el secreto de cliente que obtuvo anteriormente. Seleccione **Guardar**. 

   ![Captura de pantalla muestra la página del proveedor de identidades de Google agregar](media/google-federation/google-identity-provider.png)

#### <a name="to-configure-google-federation-by-using-powershell"></a>Configuración de la federación de Google con PowerShell
1. Instale la versión más reciente de Azure AD PowerShell para el módulo Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Ejecute el siguiente comando: `Connect-AzureAD`.
3. En el símbolo del sistema, inicie sesión con la cuenta de administrador global administrada.  
4. Ejecute el siguiente comando: 
   
   `New-AzureADMSIdentityProvider -Type Google -Name Google -ClientId [Client ID] -ClientSecret [Client secret]`
 
   > [!NOTE]
   > Use el identificador y secreto de cliente de la aplicación que creó en el "Paso 1: configuración de un proyecto de desarrollador de Google". Para más información, consulte el artículo sobre [New AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/new-azureadmsidentityprovider?view=azureadps-2.0-preview). 
 
## <a name="how-do-i-remove-google-federation"></a>¿Cómo se quita la federación de Google?
La configuración de la federación de Google se puede eliminar. Si lo hace, los usuarios invitados de Google que hayan canjeado su invitación ya no podrán iniciar sesión, pero podrán acceder a sus recursos de nuevo si los elimina del directorio y los vuelve a invitar. 
 
### <a name="to-delete-google-federation-in-the-azure-ad-portal"></a>Para eliminar la federación de Google desde el portal de Azure AD: 
1. Vaya a [Azure Portal](https://portal.azure.com). En el panel izquierdo, seleccione **Azure Active Directory**. 
2. Seleccione **Relaciones organizativas**.
3. Seleccione **proveedores de identidades**.
4. En el **Google** línea, seleccione el menú contextual (**...** ) y, a continuación, seleccione **eliminar**. 
   
   ![Captura de pantalla muestra la opción de eliminación del proveedor de identidades sociales](media/google-federation/google-social-identity-providers.png)

1. Seleccione **Sí** para confirmar la eliminación. 

### <a name="to-delete-google-federation-by-using-powershell"></a>Eliminación de la federación de Google con PowerShell: 
1. Instale la versión más reciente de Azure AD PowerShell para el módulo Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Ejecute `Connect-AzureAD`.  
4. En el símbolo del sistema, inicie sesión con la cuenta de administrador global administrada.  
5. Escriba el comando siguiente:

    `Remove-AzureADMSIdentityProvider -Id Google-OAUTH`

   > [!NOTE]
   > Para más información, consulte [Remove-AzureADMSIdentityProvider](https://docs.microsoft.com/powershell/module/azuread/Remove-AzureADMSIdentityProvider?view=azureadps-2.0-preview). 
