---
title: Configuración del inicio de sesión de cuentas de Azure Active Directory con una directiva integrada en Azure Active Directory B2C | Microsoft Docs
description: Configure el inicio de sesión de cuentas de Azure Active Directory con una directiva integrada en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9b9754c9087f2d0064cc1aa75e76520731dfb3a9
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242865"
---
# <a name="set-up-sign-in-azure-active-directory-accounts-a-built-in-policy-in-azure-active-directory-b2c"></a>Configuración del inicio de sesión de cuentas de Azure Active Directory con una directiva integrada en Azure Active Directory B2C

>[!NOTE]
> Esta característica está en versión preliminar pública. No use esta característica en entornos de producción.

En este artículo se muestra cómo habilitar el inicio de sesión para los usuarios de una organización de Azure Active Directory (Azure AD) específica mediante una directiva integrada en Azure Active Directory (Azure AD) B2C.

## <a name="create-an-azure-ad-app"></a>Creación de una aplicación de Azure AD

Para habilitar el inicio de sesión para los usuarios de una organización específica de Azure AD, es preciso registrar una aplicación en el inquilino de Azure AD de la organización, que no es el mismo que su inquilino de Azure AD B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD. Para ello, haga clic en el filtro de directorio y suscripción que se encuentra en el menú superior y elija el directorio que contiene el inquilino de Azure AD.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Nuevo registro de aplicaciones**.
5. Escriba un nombre para la aplicación. Por ejemplo, `Azure AD B2C App`.
6. En **Tipo de aplicación**, seleccione `Web app / API`.
7. Para la **dirección URL de inicio de sesión**, escriba la siguiente dirección URL en minúscula, donde `your-B2C-tenant-name` se reemplaza por el nombre del inquilino de Azure AD B2C. Por ejemplo, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Todas las direcciones URL ahora deberían estar utilizando [b2clogin.com](b2clogin.md).

8. Haga clic en **Create**(Crear). Copie el **Identificador de aplicación**, ya que lo usará más adelante.
9. Seleccione la aplicación y **Configuración**.
10. Seleccione **Claves**, escriba la descripción de la clave, seleccione una duración y haga clic en **Guardar**. Copie el valor de la clave que se muestra, ya que lo usará más adelante.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Configuración de Azure AD como proveedor de identidades

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino de Azure AD B2C.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione **Proveedores de identidades** y, después, **Agregar**.
4. Escriba un **nombre**. Por ejemplo, escriba "Contoso Azure AD".
5. Seleccione **Tipo de proveedor de identidades**, seleccione **Open ID Connect (versión preliminar)** y haga clic en **Aceptar**.
6. Haga clic en **Configurar este proveedor de identidades**.
7. En **URL de metadatos**, escriba la dirección URL siguiente, sustituyendo `your-AD-tenant-domain` por el nombre de dominio del inquilino de Azure AD. Por ejemplo: `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

8. En **Identificador de cliente**, escriba el identificador de aplicación que ha registrado previamente y en **Secreto de cliente**, escriba el valor de la clave que anotó anteriormente.
9. También puede escribir un valor en **Sugerencia de dominio**. Por ejemplo, `ContosoAD`. Este es el valor que se usa al hacer referencia a este proveedor de identidades mediante *domain_hint* en la solicitud. 
10. Haga clic en **OK**.
11. Seleccione **Asignar las notificaciones de este proveedor de identidades** y establezca las siguientes notificaciones:
    
    - En **Id. de usuario**, escriba `oid`.
    - En **Nombre para mostrar**, escriba `name`.
    - En **Nombre propio**, escriba `given_name`.
    - En **Apellido**, escriba `family_name`.
    - En **Correo electrónico**, escriba `unique_name`.

12. Haga clic en **Aceptar** y luego en **Crear** para guardar la configuración.
