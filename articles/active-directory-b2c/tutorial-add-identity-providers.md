---
title: 'Tutorial: Agregar proveedores de identidades a las aplicaciones: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo agregar proveedores de identidades a las aplicaciones en Azure Active Directory B2C mediante Azure Portal.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/01/2019
ms.author: davidmu
ms.openlocfilehash: 2a1843f941c6abc46928b38a66025fa87c4bcea5
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757379"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>Tutorial: Incorporación de proveedores de identidades a las aplicaciones en Azure Active Directory B2C

En las aplicaciones, es posible que quiera permitir que los usuarios inicien sesión con distintos proveedores de identidades. Un *proveedor de identidades* crea, mantiene y administra la información de identidad al tiempo que proporciona servicios de autenticación a las aplicaciones. Puede agregar proveedores de identidades compatibles con Azure Active Directory (Azure AD) B2C a los [flujos de usuario](active-directory-b2c-reference-policies.md) mediante Azure Portal.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear las aplicaciones del proveedor de identidades
> * Agregar los proveedores de identidades al inquilino
> * Agregar los proveedores de identidades al flujo del usuario

Por lo general, solo se usa un proveedor de identidades en las aplicaciones, pero existe la opción de agregar más. En este tutorial se muestra cómo agregar un proveedor de identidades de Azure AD y un proveedor de identidades de Facebook a la aplicación. Agregar ambos tipos de proveedores de identidades a la aplicación es opcional. También puede agregar otros proveedores de identidades, como [Amazon](active-directory-b2c-setup-amzn-app.md), [Github](active-directory-b2c-setup-github-app.md), [Google](active-directory-b2c-setup-goog-app.md), [LinkedIn](active-directory-b2c-setup-li-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md) o [Twitter](active-directory-b2c-setup-twitter-app.md). 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

[Cree un flujo de usuario](tutorial-create-user-flows.md) para permitir a los usuarios registrarse e iniciar sesión en la aplicación. 

## <a name="create-applications"></a>Crear aplicaciones

Las aplicaciones del proveedor de identidades proporcionó el identificador y la clave para habilitar la comunicación con el inquilino de Azure AD B2C. En esta sección del tutorial, puede crear una aplicación de Azure AD y una aplicación de Facebook desde la cual se obtienen los identificadores y las claves para agregar los proveedores de identidades al inquilino. Si va a agregar solo uno de los proveedores de identidades, solo debe crear la aplicación correspondiente a ese proveedor.

### <a name="create-an-azure-active-directory-application"></a>Creación de una aplicación de Azure Active Directory

Para habilitar el inicio de sesión de los usuarios de Azure AD, deberá registrar una aplicación dentro del inquilino de Azure AD. El inquilino de Azure AD no es el mismo que el usuario de Azure AD B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD. Para ello, haga clic en el **filtro de directorio y suscripción** que se encuentra en el menú superior y elija el directorio que contiene el inquilino de Azure AD.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Nuevo registro de aplicaciones**.
5. Escriba un nombre para la aplicación. Por ejemplo, `Azure AD B2C App`.
6. En **Tipo de aplicación**, seleccione `Web app / API`.
7. Para la **dirección URL de inicio de sesión**, escriba la siguiente dirección URL en minúscula, donde `your-B2C-tenant-name` se reemplaza por el nombre del inquilino de Azure AD B2C.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```
    
    Por ejemplo, `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
    
    Todas las direcciones URL ahora deberían estar utilizando [b2clogin.com](b2clogin.md).

8. Haga clic en **Create**(Crear). Copie el **Identificador de aplicación**, ya que lo usará más adelante.
9. Seleccione la aplicación y **Configuración**.
10. Seleccione **Claves**, escriba la descripción de la clave, seleccione una duración y haga clic en **Guardar**. Copie el valor de la clave para que pueda usarlo más adelante en este tutorial.

### <a name="create-a-facebook-application"></a>Creación de una aplicación de Facebook

Para usar una cuenta de Facebook como proveedor de identidades en Azure AD B2C, debe crear una aplicación en Facebook. Si aún no tiene una cuenta de Facebook, puede obtenerla en [https://www.facebook.com/](https://www.facebook.com/).

1. Inicie sesión en [Facebook for developers](https://developers.facebook.com/) con las credenciales de su cuenta de Facebook.
2. Si aún no lo ha hecho, debe registrarse como desarrollador de Facebook. Para hacerlo, seleccione **Register** (Registrarte) en la esquina superior derecha de la página, acepte las directivas de Facebook y complete los pasos de registro.
3. Seleccione **My Apps** (Mis aplicaciones) y haga clic en **Add a New App** (Agregar una nueva aplicación). 
4. Especifique el valor de **Display Name** (Nombre para mostrar) y un valor de **Contact Email** (Correo electrónico de contacto) válido.
5. Haga clic en **Create App ID** (Crear id. de aplicación). Es posible que se le pida aceptar las directivas de la plataforma de Facebook y completar una comprobación de seguridad en línea.
6. Seleccione **Settings** (Configuración)  > **Basic** (Básica).
7. Elija una **Categoría**, por ejemplo, `Business and Pages`. Este valor es obligatorio para Facebook, pero no se usa para Azure AD B2C.
8. En la parte inferior de la página, seleccione **Add Platform** (Agregar plataforma) y, después, seleccione **Website** (Sitio web).
9. En **URL del sitio**, escriba `https://your-tenant-name.b2clogin.com/`, pero sustituya `your-tenant-name` por el nombre del inquilino. Escriba una dirección URL en **Privacy Policy URL** (URL de directiva de privacidad), por ejemplo `http://www.contoso.com`. La dirección URL de directiva es una página que sirve para proporcionar información de privacidad de la aplicación.
10. Seleccione **Save changes** (Guardar los cambios).
11. En la parte superior de la página, copie el valor de **App ID** (Id. de la aplicación). 
12. Haga clic en **Show** (Mostrar) y copie el valor de **App Secret** (Secreto de la aplicación). Use ambos para configurar Facebook como proveedor de identidades de su inquilino. El **secreto de la aplicación** es una credencial de seguridad importante.
13. Seleccione **Products** (Productos) y, después, seleccione **Set up** (Configurar) en **Facebook Login** (Inicio de sesión de Facebook).
14. Seleccione **Settings** (configuración) en **Facebook Login** (Inicio de sesión de Facebook).
15. En **Valid OAuth redirect URIs** (URI de redireccionamiento OAuth válidos), escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Reemplace `your-tenant-name` por el nombre del inquilino. Haga clic en **Save Changes** (Guardar cambios) en la parte inferior de la página.
16. Para que la aplicación de Facebook esté disponible en Azure AD B2C, haga clic en el selector de **estado** que se encuentra en la esquina superior derecha de la página y establézcalo en **On**. Haga clic en **Confirmar**. En este momento el estado debería cambiar de **Desarrollo** a **Activo**.

## <a name="add-the-identity-providers"></a>Actualización de los proveedores de identidades

Después de crear la aplicación para el proveedor de identidades que quiere agregar, agregue el proveedor de identidades al inquilino.

### <a name="add-the-azure-active-directory-identity-provider"></a>Incorporación del proveedor de identidades de Azure Active Directory

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino de Azure AD B2C.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione **Proveedores de identidades** y, después, **Agregar**.
4. Escriba un **nombre**. Por ejemplo, escriba *Contoso Azure AD*.
5. Seleccione **Tipo de proveedor de identidades**, seleccione **Open ID Connect (versión preliminar)** y haga clic en **Aceptar**.
6. Haga clic en **Configurar este proveedor de identidades**.
7. En **URL de metadatos**, escriba la dirección URL siguiente, sustituyendo `your-AD-tenant-domain` por el nombre de dominio del inquilino de Azure AD.

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Por ejemplo, `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

8. En **Identificador de cliente**, escriba el identificador de aplicación que ha registrado previamente y en **Secreto de cliente**, escriba el valor de la clave que anotó anteriormente.
9. También puede escribir un valor en **Sugerencia de dominio**. Por ejemplo, `ContosoAD`. 
10. Haga clic en **OK**.
11. Seleccione **Asignar las notificaciones de este proveedor de identidades** y establezca las siguientes notificaciones:
    
    - En **Id. de usuario**, escriba `oid`.
    - En **Nombre para mostrar**, escriba `name`.
    - En **Nombre propio**, escriba `given_name`.
    - En **Apellido**, escriba `family_name`.
    - En **Correo electrónico**, escriba `unique_name`.

12. Haga clic en **Aceptar** y luego en **Crear** para guardar la configuración.

### <a name="add-the-facebook-identity-provider"></a>Incorporación del proveedor de identidades de Facebook

1. Seleccione **Proveedores de identidades** y, después, **Agregar**.
2. Escriba un **nombre**. Por ejemplo, escriba *Facebook*.
3. Haga clic en **Tipo de proveedor de identidades**, seleccione **Facebook** y haga clic en **Aceptar**.
4. Seleccione **Configurar este proveedor de identidades** y escriba el identificador de aplicación que anotó anteriormente como el **identificador de cliente**. Escriba el secreto de la aplicación que anotó como el **Secreto de cliente**.
5. Haga clic en **Aceptar** y, después, en **Crear** para guardar la configuración de Facebook.

## <a name="update-the-user-flow"></a>Actualización del flujo de usuario

En el tutorial que completó como parte de los requisitos previos, creó un flujo de usuario para registro e inicio de sesión denominado *B2C_1_signupsignin1*. En esta sección, puede agregar los proveedores de identidades al flujo de usuario *B2C_1_signupsignin1*.

1. Seleccione **Flujos de usuario (directivas)** y, a continuación, seleccione el flujo *B2C_1_signupsignin1*.
2. Seleccione **Proveedores de identidades**, seleccione los proveedores de identidades de **Facebook** y **Contoso Azure AD** que agregó.
3. Seleccione **Guardar**.

### <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. En la página Información general del flujo de usuario que creó, seleccione **Ejecutar flujo de usuario**.
2. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
3. Haga clic en **Ejecutar flujo de usuario** y luego inicie sesión con un proveedor de identidades que haya agregado anteriormente.
4. Repita los pasos del 1 al 3 con los otros proveedores de identidades que agregó.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Crear las aplicaciones del proveedor de identidades
> * Agregar los proveedores de identidades al inquilino
> * Agregar los proveedores de identidades al flujo del usuario

> [!div class="nextstepaction"]
> [Personalización de la interfaz de usuario de las aplicaciones en Azure Active Directory B2C](tutorial-customize-ui.md)