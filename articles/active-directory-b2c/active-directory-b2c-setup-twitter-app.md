---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de Twitter mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Twitter en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f0f0b8e0cbb5fbab81a07a28a9d4a2c264be6545
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52719868"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Twitter mediante Azure Active Directory B2C

## <a name="create-an-application"></a>Creación de una aplicación

Para usar Twitter como proveedor de identidades en Azure AD B2C, tiene que crear una aplicación de Twitter. Si aún no tiene una cuenta de Twitter, puede obtenerla en [https://twitter.com/signup](https://twitter.com/signup).

1. Inicie sesión en el sitio web para [desarrolladores de Twitter](https://developer.twitter.com/en/apps) con las credenciales de la cuenta de Twitter.
2. Seleccione **Create an app** (Crear una aplicación).
3. Escriba un **nombre de aplicación** y una **descripción de aplicación**.
4. En **URL del sitio web**, escriba `https://your-tenant.b2clogin.com`. Reemplace `your-tenant` por el nombre del inquilino. Por ejemplo, https://contosob2c.b2clogin.com.
5. Para la **URL de devolución de llamada**, escriba `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-user-flow-Id/oauth1/authresp`. Sustituya `your-tenant` por el nombre del inquilino y `your-user-flow-Id` por el identificador del flujo de usuario. Por ejemplo, `b2c_1A_signup_signin_twitter`. Cuando especifique el nombre de inquilino, escriba todas las letras en minúscula, aunque se haya definido con letras en mayúscula en Azure AD B2C.
6. En la parte inferior de la página, lea y acepte los términos y seleccione **Crear**.
7. En la página **Detalles de la aplicación**, seleccione **Editar > Editar detalles**, active la casilla **Enable Sign in with Twitter** (Habilitar inicio de sesión con Twitter) y luego seleccione **Guardar**.
8. Seleccione **Claves y tokens** y registre los valores de **clave de API de consumidor** y **clave secreta de API de consumidor** que se van a usar más adelante.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configuración de Twitter como proveedor de identidades del inquilino

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Proporcione un **nombre**. Por ejemplo, escriba *Twitter*.
6. Seleccione **Tipo de proveedor de identidades**, seleccione **Twitter** y haga clic en **Aceptar**.
7. Seleccione **Configurar este proveedor de identidades** y escriba la clave de API para el **Identificador de cliente** y la clave secreta de API para el **Secreto de cliente**.
8. Haga clic en **Aceptar** y luego en **Crear** para guardar la configuración de Twitter.