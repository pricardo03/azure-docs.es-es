---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de Weibo mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Weibo en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 06a79250bac977fc4ade7853594c5307bb11d983
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336952"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Weibo mediante Azure Active Directory B2C

> [!NOTE]
> Esta característica se encuentra en su versión preliminar.
> 

## <a name="create-a-weibo-application"></a>Creación de una aplicación de Weibo

Para usar una cuenta de Weibo como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de Weibo, puede obtenerla en [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).

1. Inicie sesión en el [portal para desarrolladores de Weibo](http://open.weibo.com/) con sus credenciales de cuenta de Weibo.
2. Después de iniciar sesión, seleccione el nombre para mostrar en la esquina superior derecha.
3. En el menú desplegable, seleccione **编辑开发者信息** (Editar información de desarrollador).
4. Escriba la información necesaria y luego seleccione **提交** (Enviar).
5. Complete el proceso de comprobación de correo electrónico.
6. Vaya a la [página de comprobación de identidad](http://open.weibo.com/developers/identity/edit).
7. Escriba la información necesaria y luego seleccione **提交** (Enviar).

### <a name="register-a-weibo-application"></a>Registro de una aplicación de Weibo

1. Vaya a la [página de registro de la aplicación de Weibo](http://open.weibo.com/apps/new).
2. Escriba la información de aplicación necesaria.
3. Seleccione **创建** (Crear).
4. Copie los valores de **Clave de la aplicación** y **Secreto de la aplicación**. Necesitará ambos para agregar el proveedor de identidades a su inquilino.
5. Cargue las fotos y escriba la información necesarias.
6. Seleccione **保存以上信息** (Guardar).
7. Seleccione **高级信息** (Información avanzada).
8. Seleccione **编辑** (Editar) junto al campo de OAuth2.0 **授权设置** (Dirección URL de redireccionamiento).
9. Escriba `https://{tenant_name}.b2clogin.com/te/{tenant_name}.onmicrosoft.com/oauth2/authresp` en OAuth2.0 **授权设置** (Dirección URL de redireccionamiento). Por ejemplo, si `tenant_name` es contoso, establezca la dirección URL para que sea `https://contoso.b2clogin.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Seleccione **提交** (Enviar).  

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Configuración de una cuenta de Weibo como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que está usando el directorio que contiene su inquilino de Azure AD B2C cambiando a él en la esquina superior derecha de Azure Portal. Seleccione la información de la suscripción y, después, seleccione **Cambiar directorio**. 

    ![Cambiar al inquilino de Azure AD B2C](./media/active-directory-b2c-setup-weibo-app/switch-directories.png)

    Elija el directorio que contiene el inquilino.

    ![Selección de directorio](./media/active-directory-b2c-setup-weibo-app/select-directory.png)

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Proporcione un **nombre**. Por ejemplo, escriba *Weibo*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades),  **(Preview)** (Versión preliminar de Weibo ) y haga clic en **OK** (Aceptar).
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba la clave de aplicación que anotó anteriormente como el valor de **Client ID** (Identificador de cliente) y el secreto de aplicación que registró como **Client Secret** (Secreto de cliente) de la aplicación de Weibo que creó anteriormente.
8. Haga clic en **OK** (Aceptar) y luego en **Create** (Crear) para guardar la configuración de Weibo.
