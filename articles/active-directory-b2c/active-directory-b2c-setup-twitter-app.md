---
title: Configuración de la suscripción y del inicio de sesión con una cuenta de Twitter mediante Azure Active Directory B2C | Microsoft Docs
description: Permita suscribirse e iniciar sesión a los clientes con cuentas de Twitter en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6d8e9245e95c08aad69cd05f338b6260e554469b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337797"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta de Twitter mediante Azure Active Directory B2C

## <a name="create-a-twitter-application"></a>Crear una aplicación de Twitter

Para usar una cuenta de Twitter como proveedor de identidades en Azure Active Directory (Azure AD) B2C, debe crear una aplicación en su inquilino que la represente. Si aún no tiene una cuenta de Twitter, puede obtenerla en [https://twitter.com/signup](https://twitter.com/signup).

1. Inicie sesión en [Aplicaciones de Twitter](https://apps.twitter.com/) con sus credenciales de Twitter.
2. Seleccione **Crear nueva aplicación**.
3. Escriba el **nombre**, la **descripción** y el **sitio web**.
4. Escriba `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/{policyId}/oauth1/authresp` en **Callback URL** (Direcciones URL de devolución de llamada). Reemplace **{tenant}** por el nombre de su inquilino (por ejemplo, contosob2c) y **{policyId}** por el identificador de la directiva (por ejemplo, b2c_1_policy). Debe agregar una dirección URL de devolución de llamada para todas las directivas que usan la cuenta de Twitter. 
5. Acepte el **Acuerdo para desarrolladores** y seleccione **Crear su aplicación de Twitter**.
7. Seleccione la pestaña **Claves y tokens de acceso** .
8. Copie el valor de **Clave de consumidor** y **Secreto de consumidor**. Necesitará los dos para configurar la cuenta de Twitter como proveedor de identidades de su inquilino.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configuración de Twitter como proveedor de identidades del inquilino

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que está usando el directorio que contiene su inquilino de Azure AD B2C cambiando a él en la esquina superior derecha de Azure Portal. Seleccione la información de la suscripción y, después, seleccione **Cambiar directorio**. 

    ![Cambiar al inquilino de Azure AD B2C](./media/active-directory-b2c-setup-twitter-app/switch-directories.png)

    Elija el directorio que contiene el inquilino.

    ![Selección de directorio](./media/active-directory-b2c-setup-twitter-app/select-directory.png)

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Proveedores de identidades** y, después, **Agregar**.
5. Proporcione un **nombre**. Por ejemplo, escriba *Twitter*.
6. Seleccione **Tipo de proveedor de identidades**, seleccione **Twitter** y haga clic en **Aceptar**.
7. Seleccione **Configurar este proveedor de identidades** y escriba la Clave de consumidor para el **Identificador de cliente** y el **Secreto de consumidor** para el **Secreto de cliente**.
8. Haga clic en **Aceptar** y luego en **Crear** para guardar la configuración de Twitter.