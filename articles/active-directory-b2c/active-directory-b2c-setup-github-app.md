---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de GitHub mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de GitHub en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7f8b2c6dc570f7a610c0d661da0c6df7491647bd
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182185"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de GitHub mediante Azure Active Directory B2C

> [!NOTE]
> Esta característica se encuentra en su versión preliminar.
> 

Para usar una cuenta de GitHub como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de GitHub, puede obtenerla en [https://www.github.com/](https://www.github.com/).

## <a name="create-a-github-oauth-application"></a>Creación de una aplicación GitHub OAuth

1. Inicie sesión en el sitio Web [para desarrolladores de GitHub](https://github.com/settings/developers) con sus credenciales de GitHub.
2. Seleccione **OAuth Apps** (Aplicaciones OAuth) y elija **New OAuth App** (Nueva aplicación OAuth).
3. Escriba un **nombre de aplicación** y la **dirección URL de la página principal**.
4. Escriba la información de `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` en **Authorization callback URL** (Dirección URL de devolución de llamada de autorización). Reemplace `your-tenant-name` por el nombre del inquilino de Azure AD B2C.
5. Haga clic en **Register application** (Registrar aplicación).
6. Copie los valores de **Client ID** y **Client Secret**. Necesitará los dos para agregar el proveedor de identidades a su inquilino.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Configuración de una cuenta de GitHub como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **Directory and subscription filter** (Filtro de directorio y suscripción) en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Proporcione un **nombre**. Por ejemplo, escriba *GitHub*.
6. Seleccione **Identity provider type** (Tipo de proveedor de identidades), **GitHub (Preview)** (Versión preliminar de GitHub) y haga clic en **OK** (Aceptar).
7. Seleccione **Set up this identity provider** (Configurar este proveedor de identidades) y escriba el identificador de cliente que anotó anteriormente como el valor de **Client ID** y el secreto de cliente que registró como **Client Secret** de la aplicación de GitHub que creó anteriormente.
8. Haga clic en **OK** (Aceptar) y en **Create** (Crear) para guardar la configuración de GitHub.