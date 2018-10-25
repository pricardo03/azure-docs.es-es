---
title: Configuración del inicio de sesión de cuentas de Azure Active Directory con una directiva integrada en Azure Active Directory B2C | Microsoft Docs
description: Configure el inicio de sesión de cuentas de Azure Active Directory con una directiva integrada en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5f51fbff11412324ad167d49202f7215cefb5ac2
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49076925"
---
# <a name="set-up-sign-in-azure-active-directory-accounts-a-built-in-policy-in-azure-active-directory-b2c"></a>Configuración del inicio de sesión de cuentas de Azure Active Directory con una directiva integrada en Azure Active Directory B2C

>[!NOTE]
> Esta característica está en versión preliminar pública. No use esta característica en entornos de producción.

En este artículo se muestra cómo habilitar el inicio de sesión para los usuarios de una organización de Azure Active Directory (Azure AD) específica mediante una directiva integrada en Azure Active Directory (Azure AD) B2C.

## <a name="create-an-azure-ad-app"></a>Creación de una aplicación de Azure AD

Para habilitar el inicio de sesión para los usuarios de una organización específica de Azure AD, es preciso registrar una aplicación en el inquilino de Azure AD de la organización.

>[!NOTE]
>En las instrucciones siguientes, se usa `Contoso.com` para el inquilino de Azure AD de la organización y `fabrikamb2c.onmicrosoft.com` como inquilino de Azure AD B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD (contoso.com). Para ello, haga clic en el filtro de directorio y suscripción que se encuentra en el menú superior y elija el directorio que contiene el inquilino de Azure AD.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal y busque y seleccione **Registros de aplicaciones**.
4. Seleccione **Nuevo registro de aplicaciones**.
5. Escriba un nombre para la aplicación. Por ejemplo, `Azure AD B2C App`.
6. En **Tipo de aplicación**, seleccione `Web app / API`.
7. En cuanto a la **dirección URL de inicio de sesión**, escriba la dirección URL siguiente en minúscula, donde `your-tenant` se reemplazará por el nombre del inquilino de Azure AD B2C (fabrikamb2c.onmicrosoft.com):

    ```
    https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp
    ```

    Todas las direcciones URL ahora deberían estar utilizando [b2clogin.com](b2clogin.md).

8. Haga clic en **Create**(Crear). Copie el **Identificador de aplicación**, ya que lo usará más adelante.
9. Seleccione la aplicación y, a continuación, seleccione **Configuración**.
10. Seleccione **Claves**, escriba la descripción de la clave, seleccione una duración y, a continuación, haga clic en **Guardar**. Copie el valor de la clave que se muestra, ya que lo usará más adelante.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Configurar Azure AD como proveedor de identidades del inquilino

1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C (fabrikamb2c.onmicrosoft.com). Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino de Azure AD B2C.
2. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal y busque y seleccione **Azure AD B2C**.
3. Seleccione **Proveedores de identidades** y, después, **Agregar**.
4. Escriba un **nombre**. Por ejemplo, escriba "Contoso Azure AD".
5. Seleccione **Tipo de proveedor de identidades**, seleccione **Open ID Connect (versión preliminar)** y haga clic en **Aceptar**.
6. Haga clic en **Configurar este proveedor de identidades**.
7. En **URL de metadatos**, escriba la dirección URL siguiente, donde `your-tenant` se sustituye por el nombre del inquilino de Azure AD:

    ```
    https://login.microsoftonline.com/your-tenant/.well-known/openid-configuration
    ```
8. En **Identificador de cliente**, escriba el identificador de aplicación que ha registrado previamente y en **Secreto de cliente**, escriba el valor de la clave que anotó anteriormente.
9. También puede escribir un valor en **Sugerencia de dominio** (por ejemplo, `ContosoAD`). Este es el valor que se usa al hacer referencia a este proveedor de identidades mediante *domain_hint* en la solicitud. 
10. Haga clic en **OK**.
11. Seleccione **Asignar las notificaciones de este proveedor de identidades** y establezca las siguientes notificaciones:
    
    - En **Id. de usuario**, escriba `oid`.
    - En **Nombre para mostrar**, escriba `name`.
    - En **Nombre propio**, escriba `given_name`.
    - En **Apellido**, escriba `family_name`.
    - En **Correo electrónico**, escriba `unique_name`.

12. Haga clic en **Aceptar** y luego en **Crear** para guardar la configuración.
