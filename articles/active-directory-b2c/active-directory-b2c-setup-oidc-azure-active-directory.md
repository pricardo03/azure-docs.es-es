---
title: 'Configuración del inicio de sesión para una organización de Azure Active Directory: Azure Active Directory B2C'
description: Configuración del inicio de sesión para una determinada organización de Azure Active Directory en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 58c6d1b032f5b492c5641ff51da80426124069b1
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68716776"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Configuración del inicio de sesión para una determinada organización de Azure Active Directory en Azure Active Directory B2C

>[!NOTE]
> Esta característica está en versión preliminar pública. No use esta característica en entornos de producción.

Para usar una cuenta de Azure Active Directory (Azure AD) como [proveedor de identidades](active-directory-b2c-reference-oauth-code.md) en Azure AD B2C, tiene que crear una aplicación que la represente. En este artículo se muestra cómo habilitar el inicio de sesión para los usuarios de una organización de Azure AD específica mediante un flujo de usuario en Azure AD B2C.

## <a name="create-an-azure-ad-app"></a>Creación de una aplicación de Azure AD

Si desea habilitar el inicio de sesión para los usuarios de una organización específica de Azure AD, es preciso registrar una aplicación en el inquilino de Azure AD de la organización, que no es el mismo que el inquilino de Azure AD B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD. Seleccione el **filtro de directorio y suscripciones** del menú superior y elija el directorio que contiene el inquilino de Azure AD. No es el mismo que el inquilino de Azure AD B2C.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Nuevo registro**.
5. Escriba un nombre para la aplicación. Por ejemplo, `Azure AD B2C App`.
6. Acepte la selección de **Solo las cuentas de este directorio organizativo** para esta aplicación.
7. Para la **URI de redirección**, acepte el valor de **Web** y escriba la siguiente dirección URL en minúscula, donde `your-B2C-tenant-name` se reemplaza por el nombre del inquilino de Azure AD B2C. Por ejemplo, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Todas las direcciones URL ahora deberían estar utilizando [b2clogin.com](b2clogin.md).

8. Haga clic en **Registrar**. Copie el **Id. de aplicación (cliente)** , ya que lo usará más adelante.
9. Seleccione **Certificados y secretos** y **Nuevo secreto de cliente**.
10. Escriba un nombre para el secreto de cliente. Por ejemplo, `Azure AD B2C App Secret`.
11. Seleccione el período de expiración. Para esta aplicación, acepte la selección de **En 1 año**.
12. Seleccione **Agregar** y copie el valor del nuevo secreto de cliente que se muestra para usarlo más tarde.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Configuración de Azure AD como proveedor de identidades

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el **filtro de directorio y suscripciones** del menú superior y elija el directorio que contiene el inquilino de Azure AD B2C.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
3. Seleccione **Proveedores de identidades** y, después, **Agregar**.
4. Escriba un **nombre**. Por ejemplo, escriba: `Contoso Azure AD`.
5. Seleccione **Tipo de proveedor de identidades**, seleccione **OpenID Connect (versión preliminar)** y haga clic en **Aceptar**.
6. Seleccione **Configurar este proveedor de identidades**.
7. En **URL de metadatos**, escriba la dirección URL siguiente, sustituyendo `your-AD-tenant-domain` por el nombre de dominio del inquilino de Azure AD. Por ejemplo: `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

8. En **Id. de cliente**, escriba el identificador de aplicación que ha registrado previamente y, en **Secreto de cliente**, escriba el secreto de cliente que anotó anteriormente.
9. También puede escribir un valor en **Sugerencia de dominio**. Por ejemplo, `ContosoAD`. Este es el valor que se usa al hacer referencia a este proveedor de identidades mediante *domain_hint* en la solicitud.
10. Haga clic en **OK**.
11. Seleccione **Asignar las notificaciones de este proveedor de identidades** y establezca las siguientes notificaciones:

    - En **Id. de usuario**, escriba `oid`.
    - En **Nombre para mostrar**, escriba `name`.
    - En **Nombre propio**, escriba `given_name`.
    - En **Apellido**, escriba `family_name`.
    - En **Correo electrónico**, escriba `unique_name`.

12. Haga clic en **Aceptar** y luego en **Crear** para guardar la configuración.
