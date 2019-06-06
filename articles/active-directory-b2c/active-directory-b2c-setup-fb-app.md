---
title: 'Configuración de la suscripción y del inicio de sesión con una cuenta de Facebook: Azure Active Directory B2C | Microsoft Docs'
description: Permita suscribirse e iniciar sesión en sus aplicaciones a los clientes con cuentas de Facebook mediante Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6b90ba89f17b42f4d92c666c3f539fcad3e3227c
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733526"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Facebook mediante Azure Active Directory B2C

## <a name="create-a-facebook-application"></a>Creación de una aplicación de Facebook

Para usar una cuenta de Facebook como [proveedor de identidades](active-directory-b2c-reference-oauth-code.md) en Azure Active Directory (Azure AD) B2C, tiene que crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de Facebook, puede obtenerla en [https://www.facebook.com/](https://www.facebook.com/).

1. Inicie sesión en [Facebook for developers](https://developers.facebook.com/) con las credenciales de su cuenta de Facebook.
2. Si aún no lo ha hecho, debe registrarse como desarrollador de Facebook. Para ello, seleccione **comenzar** en la esquina superior derecha de la página, acepte las directivas de Facebook y complete los pasos de registro.
3. Seleccione **mis aplicaciones** y, a continuación, **agregar nueva aplicación**.
4. Especifique el valor de **Display Name** (Nombre para mostrar) y un valor de **Contact Email** (Correo electrónico de contacto) válido.
5. Haga clic en **Create App ID** (Crear id. de aplicación). Es posible que deba aceptar las políticas de la plataforma Facebook y realizar una comprobación de seguridad en línea.
6. Seleccione **Settings** (Configuración)  > **Basic** (Básica).
7. Elija una **Categoría**, por ejemplo, `Business and Pages`. Este valor es obligatorio para Facebook, pero no se usa para Azure AD B2C.
8. En la parte inferior de la página, seleccione **Add Platform** (Agregar plataforma) y, después, seleccione **Website** (Sitio web).
9. En **URL del sitio**, escriba `https://your-tenant-name.b2clogin.com/`, pero sustituya `your-tenant-name` por el nombre del inquilino. Escriba una dirección URL en **Privacy Policy URL** (URL de directiva de privacidad), por ejemplo `http://www.contoso.com`. La dirección URL de directiva es una página que sirve para proporcionar información de privacidad de la aplicación.
10. Seleccione **Save changes** (Guardar los cambios).
11. En la parte superior de la página, copie el valor de **App ID** (Id. de la aplicación).
12. Haga clic en **Show** (Mostrar) y copie el valor de **App Secret** (Secreto de la aplicación). Use ambos para configurar Facebook como proveedor de identidades de su inquilino. El **secreto de la aplicación** es una credencial de seguridad importante.
13. Seleccione el signo más junto a **productos**y, a continuación, seleccione **configurar** en **inicio de sesión de Facebook**.
14. En **inicio de sesión de Facebook**, seleccione **configuración**.
15. En **Valid OAuth redirect URIs** (URI de redireccionamiento OAuth válidos), escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Reemplace `your-tenant-name` por el nombre del inquilino. Haga clic en **Save Changes** (Guardar cambios) en la parte inferior de la página.
16. Para que su aplicación de Facebook esté disponible para Azure AD B2C, haga clic en el selector de estados situado en la parte superior derecha de la página y seleccione **Activado** para hacer que la aplicación sea pública y, después, haga clic en **Confirmar**.  En este momento el estado debería cambiar de **Desarrollo** a **Activo**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configuración de una cuenta de Facebook como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Escriba un **nombre**. Por ejemplo, escriba *Facebook*.
6. Haga clic en **Tipo de proveedor de identidades**, seleccione **Facebook** y haga clic en **Aceptar**.
7. Seleccione **Configurar este proveedor de identidades** y escriba el identificador de aplicación que anotó anteriormente como **identificador de cliente** y el secreto de aplicación que registró como **secreto de cliente** de la aplicación de Facebook que creó anteriormente.
8. Haga clic en **Aceptar** y, después, en **Crear** para guardar la configuración de Facebook.
