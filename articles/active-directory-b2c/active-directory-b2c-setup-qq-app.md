---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de QQ mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de QQ en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 38e17308f7aab5b28509d7a3509be0de844ee94f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60316126"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de QQ mediante Azure Active Directory B2C

> [!NOTE]
> Esta característica se encuentra en su versión preliminar.
> 

## <a name="create-a-qq-application"></a>Creación de una aplicación de QQ

Para usar una cuenta de QQ como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de QQ, puede obtenerla en [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>Registro para el programa para desarrolladores de QQ

1. Inicie sesión en el [portal para desarrolladores de QQ](http://open.qq.com) e inicie sesión con sus credenciales de la cuenta de QQ.
2. Después de iniciar sesión, vaya a [http://open.qq.com/reg](http://open.qq.com/reg) para registrarse como un programador.
3. Seleccione **个人** (Desarrollador individual).
4. Escriba la información necesaria y luego seleccione **下一步** (Paso siguiente).
5. Complete el proceso de comprobación de correo electrónico. Debe esperar unos días para recibir la aprobación después de registrarse como desarrollador. 

### <a name="register-a-qq-application"></a>Registro de una aplicación QQ

1. Vaya a [https://connect.qq.com/index.html](https://connect.qq.com/index.html).
2. Seleccione **应用管理** (Administración de aplicaciones).
5. Seleccione **创建应用**(Crear aplicación) y escriba la información necesaria.
7. Escriba `https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp` en **授权回调域** (Dirección URL de devolución de llamada). Por ejemplo, si `tenant_name` es Contoso, establezca la dirección URL para que sea `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
8. Seleccione **创建应用** (Crear aplicación).
9. En la página de confirmación, seleccione **应用管理** (Administración de aplicaciones) para volver a la página de administración de aplicaciones.
10. Seleccione **查看** (Ver) junto a la aplicación que acaba de crear.
11. Seleccione **修改**(Editar).
12. Copie el valor de **APP ID** (Identificador de la aplicación) y de **APP KEY** (Clave de la aplicación). Necesitará los dos valores para agregar el proveedor de identidades a su inquilino.

## <a name="configure-qq-as-an-identity-provider"></a>Configuración de QQ como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Proporcione un **nombre**. Por ejemplo, escriba *QQ*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades), **QQ (Preview)** (Versión preliminar de QQ) y haga clic en **OK** (Aceptar).
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba el identificador de aplicación que anotó anteriormente como el valor de **Client ID** (Identificador de cliente) y la clave de aplicación que registró como **Client Secret** (Secreto de cliente) de la aplicación de QQ que creó anteriormente.
8. Haga clic en **Aceptar** y luego en **Crear** para guardar la configuración de QQ.

