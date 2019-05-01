---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de Weibo mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Weibo en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9701d1583a19be46c4c72a82d9f376a8db0c625c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704247"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Weibo mediante Azure Active Directory B2C

> [!NOTE]
> Esta característica se encuentra en su versión preliminar.
> 

## <a name="create-a-weibo-application"></a>Creación de una aplicación de Weibo

Para usar una cuenta de Weibo como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de Weibo, puede obtenerla en [https://weibo.com/signup/signup.php?lang=en-us](https://weibo.com/signup/signup.php?lang=en-us).

1. Inicie sesión en el [portal para desarrolladores de Weibo](https://open.weibo.com/) con sus credenciales de cuenta de Weibo.
2. Después de iniciar sesión, seleccione el nombre para mostrar en la esquina superior derecha.
3. En el menú desplegable, seleccione **编辑开发者信息** (Editar información de desarrollador).
4. Escriba la información necesaria y luego seleccione **提交** (Enviar).
5. Complete el proceso de comprobación de correo electrónico.
6. Vaya a la [página de comprobación de identidad](https://open.weibo.com/developers/identity/edit).
7. Escriba la información necesaria y luego seleccione **提交** (Enviar).

### <a name="register-a-weibo-application"></a>Registro de una aplicación de Weibo

1. Vaya a la [página de registro de la aplicación de Weibo](https://open.weibo.com/apps/new).
2. Escriba la información de aplicación necesaria.
3. Seleccione **创建** (Crear).
4. Copie los valores de **Clave de la aplicación** y **Secreto de la aplicación**. Necesitará ambos para agregar el proveedor de identidades a su inquilino.
5. Cargue las fotos y escriba la información necesarias.
6. Seleccione **保存以上信息** (Guardar).
7. Seleccione **高级信息** (Información avanzada).
8. Seleccione **编辑** (Editar) junto al campo de OAuth2.0 **授权设置** (Dirección URL de redireccionamiento).
9. Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en OAuth2.0 **授权设置** (Dirección URL de redireccionamiento). Por ejemplo, si el nombre de inquilino es contoso, establezca la dirección URL para que sea `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
10. Seleccione **提交** (Enviar).  

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Configuración de una cuenta de Weibo como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Proporcione un **nombre**. Por ejemplo, escriba *Weibo*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades),  **(Preview)** (Versión preliminar de Weibo ) y haga clic en **OK** (Aceptar).
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba la clave de aplicación que anotó anteriormente como el valor de **Client ID** (Identificador de cliente) y el secreto de aplicación que registró como **Client Secret** (Secreto de cliente) de la aplicación de Weibo que creó anteriormente.
8. Haga clic en **OK** (Aceptar) y luego en **Create** (Crear) para guardar la configuración de Weibo.
