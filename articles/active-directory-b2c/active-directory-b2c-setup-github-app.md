---
title: 'Configuración de la suscripción y del inicio de sesión con una cuenta de GitHub: Azure Active Directory B2C | Microsoft Docs'
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de GitHub en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 88d92e081a7b852035dd2b7d3bc9e4e29fefaddd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66508578"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de GitHub mediante Azure Active Directory B2C

> [!NOTE]
> Esta característica se encuentra en su versión preliminar.
> 

Para usar una cuenta de GitHub como [proveedor de identidades](active-directory-b2c-reference-oauth-code.md) en Azure Active Directory (Azure AD) B2C, tiene que crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de GitHub, puede obtenerla en [https://www.github.com/](https://www.github.com/).

## <a name="create-a-github-oauth-application"></a>Creación de una aplicación GitHub OAuth

1. Inicie sesión en el sitio Web [para desarrolladores de GitHub](https://github.com/settings/developers) con sus credenciales de GitHub.
2. Seleccione **OAuth Apps** (Aplicaciones OAuth) y elija **New OAuth App** (Nueva aplicación OAuth).
3. Escriba un **nombre de aplicación** y la **dirección URL de la página principal**.
4. Escriba la información de `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **Authorization callback URL** (Dirección URL de devolución de llamada de autorización). Reemplace `your-tenant-name` por el nombre del inquilino de Azure AD B2C. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C.
5. Haga clic en **Register application** (Registrar aplicación).
6. Copie los valores de **Client ID** y **Client Secret**. Necesitará los dos para agregar el proveedor de identidades a su inquilino.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Configuración de una cuenta de GitHub como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Especifique un **nombre**. Por ejemplo, escriba *GitHub*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades), **GitHub (Preview)** (Versión preliminar de GitHub) y haga clic en **OK** (Aceptar).
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba el identificador de cliente que anotó anteriormente como el valor de **Client ID** y el secreto de cliente que registró como **Client Secret** de la aplicación de GitHub que creó anteriormente.
8. Haga clic en **OK** (Aceptar) y en **Create** (Crear) para guardar la configuración de GitHub.
