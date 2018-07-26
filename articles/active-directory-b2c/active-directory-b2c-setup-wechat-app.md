---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de WeChat mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de WeChat en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e88187c5035abc28ca9deecaf8517e8a21e38d1d
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952341"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de WeChat mediante Azure Active Directory B2C

> [!NOTE]
> Esta característica se encuentra en su versión preliminar.
> 

## <a name="create-a-wechat-application"></a>Creación de una aplicación de WeChat

Para usar una cuenta de WeChat como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de WeChat, puede obtener información en [http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registro de una aplicación de WeChat

1. Inicie sesión en [https://open.weixin.qq.com/](https://open.weixin.qq.com/) con sus credenciales de WeChat.
2. Seleccione **管理中心**(centro de administración).
3. Siga los pasos para registrar una nueva aplicación.
4. Escriba `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` en **授权回调域** (Dirección URL de devolución de llamada). Por ejemplo, si `tenant_name` es contoso.onmicrosoft.com, establezca la dirección URL para que sea `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Copie el valor de **APP ID** (Identificador de la aplicación) y de **APP KEY** (Clave de la aplicación). Los necesitará para agregar el proveedor de identidades a su inquilino.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Configuración de WeChat como proveedor de identidades del inquilino

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que está usando el directorio que contiene su inquilino de Azure AD B2C cambiando a él en la esquina superior derecha de Azure Portal. Seleccione la información de la suscripción y, después, seleccione **Cambiar directorio**. 

    ![Cambiar al inquilino de Azure AD B2C](./media/active-directory-b2c-setup-wechat-app/switch-directories.png)

    Elija el directorio que contiene el inquilino.

    ![Selección de directorio](./media/active-directory-b2c-setup-wechat-app/select-directory.png)

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, seleccione **Agregar**.
5. Proporcione un **nombre**. Por ejemplo, escriba *WeChat*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades), **WeChat (Preview)** (Versión preliminar de WeChat) y haga clic en **OK** (Aceptar).
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba el identificador de aplicación que anotó anteriormente como el valor de **Client ID** (Identificador de cliente) y la clave de aplicación que registró como **Client Secret** (Secreto de cliente) de la aplicación de WeChat que creó anteriormente.
8. Haga clic en **OK** (Aceptar) y luego en **Create** (Crear) para guardar la configuración de WeChat.

