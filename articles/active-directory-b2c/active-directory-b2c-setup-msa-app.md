---
title: Configuración de la cuenta Microsoft en Azure Active Directory B2C | Microsoft Docs
description: Proporcione funciones de registro e inicio de sesión a los consumidores con cuentas Microsoft en las aplicaciones protegidas por Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: be8c691835d2e89d5d305e01f9717e55e3741baf
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712426"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: provisión de registro e inicio de sesión para los consumidores con cuentas Microsoft
## <a name="create-a-microsoft-account-application"></a>Creación de una aplicación de cuenta Microsoft
Para usar una cuenta Microsoft como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación de cuenta Microsoft y suministrarle los parámetros correctos. Necesita una cuenta de Microsoft para hacerlo. Si no tiene ninguna, puede obtener una en [https://www.live.com/](https://www.live.com/).

1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) e inicie sesión con las credenciales de su cuenta Microsoft.
2. Haga clic en **Agregar una aplicación**.
   
    ![Cuenta Microsoft: adición de una aplicación nueva](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Especifique un **nombre** para la aplicación y haga clic en **Crear aplicación**.
   
    ![Cuenta Microsoft: nombre de la aplicación](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Copie el valor de **Id. de aplicación**. Lo necesitará para configurar una cuenta Microsoft como proveedor de identidades de su inquilino.
   
    ![Cuenta Microsoft: Id. de la aplicación](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Haga clic en **Agregar plataforma** y elija **Web**.
   
    ![Cuenta Microsoft: Agregar plataforma](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Cuenta Microsoft: Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Escriba `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` en el campo **URI de redireccionamiento** . Reemplace **{tenant}** por el nombre de su inquilino (por ejemplo, contosob2c.onmicrosoft.com).
   
    ![Cuenta Microsoft: dirección URL de redireccionamiento](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Haga clic en **Generar nueva contraseña** en la sección **Secretos de aplicación**. Copie la nueva contraseña que se muestra en la pantalla. Lo necesitará para configurar una cuenta Microsoft como proveedor de identidades de su inquilino. Esta contraseña es una credencial de seguridad importante.
   
    ![Cuenta Microsoft: Generar nueva contraseña](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Cuenta Microsoft: Nueva contraseña](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Active la casilla **Soporte técnico de SDK de Live** de la sección **Opciones avanzadas**. Haga clic en **Save**(Guardar).
   
    ![Cuenta Microsoft: Soporte técnico de SDK de Live](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Configuración de una cuenta Microsoft como proveedor de identidades de su inquilino
1. Siga estos pasos para [ir a la hoja de características de B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) de Azure Portal.
2. En la hoja de características B2C, haga clic en **Proveedores de identidades**.
3. Haga clic en **+Agregar** en la parte superior de la hoja.
4. Proporcione un **Nombre** descriptivo para la configuración del proveedor de identidades. Por ejemplo, escriba "MSA".
5. Haga clic en **Identity provider type** (Tipo de proveedor de identidades), seleccione **Microsoft account** (Cuenta Microsoft) y haga clic en **OK** (Aceptar).
6. Haga clic en **Configurar este proveedor de identidades** (Set up this identity provider) y proporcione los valores de identificador de aplicación y contraseña de la aplicación de la cuenta Microsoft que creó anteriormente.
7. Haga clic en **OK** (Aceptar) y en **Create** (Crear) para guardar la configuración de la cuenta Microsoft.

