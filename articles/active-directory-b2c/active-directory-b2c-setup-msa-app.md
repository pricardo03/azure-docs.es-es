---
title: Configuración de la suscripción y del inicio de sesión con una cuenta Microsoft mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas Microsoft en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 338c2a197cb50091c3b272e0ce590341ffda1d7f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341090"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta Microsoft mediante Azure Active Directory B2C

## <a name="create-a-microsoft-account-application"></a>Creación de una aplicación de cuenta Microsoft

Para usar una cuenta Microsoft como proveedor de identidades en Azure Active Directory (Azure AD) B2C, es preciso crear una aplicación en un inquilino que la represente. Si aún no tiene una cuenta Microsoft, puede obtenerla en [https://www.live.com/](https://www.live.com/).

1. Inicie sesión en el [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) con las credenciales de su cuenta Microsoft.
2. En la esquina superior derecha, seleccione **Agregar una aplicación**.
3. Especifique un **nombre** para la aplicación y haga clic en **Crear**.
4. En la página de registro, copie el valor de **Id. de la aplicación**. Dicho valor lo usará para configurar una cuenta Microsoft como proveedor de identidades en su inquilino.
5. Seleccione **Agregar plataforma** y elija **Web**.
6. Escriba `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` en **Direcciones URL de redireccionamiento**. Reemplace **{tenant}** por el nombre de su inquilino (por ejemplo, contosob2c).
7. En **Secretos de aplicación**, haga clic en **Generar nueva contraseña**. Copie la nueva contraseña que se muestra en la pantalla. La necesitará para configurar una cuenta Microsoft como proveedor de identidades en su inquilino. Esta contraseña es una credencial de seguridad importante.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Configuración de una cuenta Microsoft como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que está usando el directorio que contiene su inquilino de Azure AD B2C cambiando a él en la esquina superior derecha de Azure Portal. Seleccione la información de la suscripción y, después, seleccione **Cambiar directorio**. 

    ![Cambiar al inquilino de Azure AD B2C](./media/active-directory-b2c-setup-msa-app/switch-directories.png)

    Elija el directorio que contiene el inquilino.

    ![Selección de directorio](./media/active-directory-b2c-setup-msa-app/select-directory.png)

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Especifique un **nombre**. Por ejemplo, escriba *MSA*.
6. Haga clic en **Tipo de proveedor de identidades**, seleccione **Cuenta Microsoft** y haga clic en **Aceptar**.
7. Seleccione **Configurar este proveedor de identidades** y escriba el identificador de aplicación que anotó anteriormente como **identificador de cliente** y la contraseña que registró como **secreto de cliente** de la aplicación de la cuenta Microsoft que creó antes.
8. Haga clic en **OK** (Aceptar) y en **Create** (Crear) para guardar la configuración de la cuenta Microsoft.

