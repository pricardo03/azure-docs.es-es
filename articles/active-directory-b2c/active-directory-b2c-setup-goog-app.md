---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de Google mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Google en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: af8a727bf9733b744b1ed429420cd8fde6e3e6f1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176138"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Google mediante Azure Active Directory B2C

## <a name="create-a-google-application"></a>Creación de una aplicación de Google

Para usar una cuenta de Google como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de Google, puede obtenerla en [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Inicie sesión en [Google Developers Console](https://console.developers.google.com/) e inicie sesión con las credenciales de su cuenta de Google.
2. Seleccione **Create project** (Crear proyecto) y luego haga clic en **Create** (Crear). Si creó proyectos antes, seleccione la lista de proyectos y, a continuación, seleccione **New Project** (Nuevo proyecto).
3. Escriba un **Nombre de proyecto**, haga clic en **Crear** y asegúrese de que está usando el nuevo proyecto.
3. Seleccione **Credentials** (Credenciales) en el menú izquierdo y, a continuación, seleccione **Create credentials** > **Oauth client ID** (Crear credenciales, Id. de cliente de Oauth).
4. Seleccione **Configure consent screen** (Configurar pantalla de consentimiento).
5. Seleccione o especifique una **Dirección de correo electrónico** válida, proporcione un **Nombre de producto** para mostrar a los usuarios, agregue `b2clogin.com` en **Dominios autorizados** y, a continuación, haga clic en **Guardar**.
6. En **Application type** (Tipo de aplicación), seleccione **Web application** (Aplicación web).
7. Escriba un **Name** (nombre) para la aplicación, escriba `https://your-tenant-name.b2clogin.com` en **Authorized JavaScript origins** (Orígenes de JavaScript autorizados) y `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **Authorized redirect URIs** (URI de redirección autorizados). Reemplace `your-tenant-name` por el nombre del inquilino. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C.
8. Haga clic en **Create**(Crear).
9. Copie los valores de **Client ID** y **Client Secret**. Necesitará ambos para configurar Google como proveedor de identidades de su inquilino. **Secreto del cliente** es una credencial de seguridad importante.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Configuración de una cuenta de Google como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Escriba la información de **Name** (Nombre). Por ejemplo, escriba *Google*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades), seleccione **Google** y haga clic en **OK** (Aceptar).
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba el identificador de cliente que anotó anteriormente como el valor de **Client ID** y el secreto de cliente que registró como **Client Secret** de la aplicación de Google que creó anteriormente.
8. Haga clic en **OK** (Aceptar) y, luego, en **Create** (Crear) para guardar la configuración de Google.

