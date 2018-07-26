---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de Google mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Google en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: dd0bf50d73b70e37195e8e5e45336b68e4e883e7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915646"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Google mediante Azure Active Directory B2C

## <a name="create-a-google-application"></a>Creación de una aplicación de Google

Para usar una cuenta de Google como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de Google, puede obtenerla en [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Inicie sesión en [Google Developers Console](https://console.developers.google.com/) e inicie sesión con las credenciales de su cuenta de Google.
2. Seleccione **Create project** (Crear proyecto) y luego haga clic en **Create** (Crear). Si creó proyectos antes, seleccione la lista de proyectos y, a continuación, seleccione **New Project** (Nuevo proyecto).
3. Escriba un **nombre de proyecto** y luego haga clic en **Create** (Crear).
3. Seleccione **Credentials** (Credenciales) en el menú izquierdo y, a continuación, seleccione **Create credentials** > **Oauth client ID** (Crear credenciales, Id. de cliente de Oauth).
4. Seleccione **Configure consent screen** (Configurar pantalla de consentimiento).
5. Seleccione o especifique una **Email address** (Dirección de correo electrónico), proporcione un **Product name shown to users** (Nombre de producto mostrado a los usuarios) y haga clic en **Save** (Guardar).
6. En **Application type** (Tipo de aplicación), seleccione **Web application** (Aplicación web).
7. Escriba un **Name** (nombre) para la aplicación, escriba `https://login.microsoftonline.com` en **Authorized JavaScript origins** (Orígenes de JavaScript autorizados) y `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` en **Authorized redirect URIs** (URI de redirección autorizados). Reemplace **{tenant}** por el nombre de su inquilino (por ejemplo, contosob2c.onmicrosoft.com).
8. Haga clic en **Create**(Crear).
9. Copie los valores de **Client ID** y **Client Secret**. Necesitará ambos para configurar Google como proveedor de identidades de su inquilino. **Secreto del cliente** es una credencial de seguridad importante.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Configuración de una cuenta de Google como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que está usando el directorio que contiene su inquilino de Azure AD B2C cambiando a él en la esquina superior derecha de Azure Portal. Seleccione la información de la suscripción y, después, seleccione **Cambiar directorio**. 

    ![Cambiar al inquilino de Azure AD B2C](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Elija el directorio que contiene el inquilino.

    ![Selección de directorio](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, seleccione **Agregar**.
5. Escriba la información de **Name** (Nombre). Por ejemplo, escriba *Google*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades), seleccione **Google** y haga clic en **OK** (Aceptar).
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba el identificador de cliente que anotó anteriormente como el valor de **Client ID** y el secreto de cliente que registró como **Client Secret** de la aplicación de Google que creó anteriormente.
8. Haga clic en **OK** (Aceptar) y, luego, en **Create** (Crear) para guardar la configuración de Google.

