---
title: 'Transmisión de un token de acceso a través de un flujo de usuario a la aplicación: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo se puede pasar un token de acceso para proveedores de identidades de OAuth2.0 como una notificación en un flujo de usuario a la aplicación en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: db4b799aa31a4132609b0dd158b65070fb2474b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510966"
---
# <a name="pass-an-access-token-through-a-user-flow-to-your-application-in-azure-active-directory-b2c"></a>Transmisión de un token de acceso a través de un flujo de usuario a la aplicación en Azure Active Directory B2C

> [!NOTE]
> Esta característica actualmente está en su versión preliminar pública.

Un [flujo de usuario](active-directory-b2c-reference-policies.md) en Azure Active Directory (Azure AD) B2C proporciona una oportunidad a los usuarios para registrarse o iniciar sesión con un proveedor de identidades. Cuando esto sucede, Azure AD B2C recibe un [token de acceso](active-directory-b2c-reference-tokens.md) del proveedor de identidades. Azure AD B2C usa ese token para recuperar información sobre el usuario. Habilite una notificación en el flujo de usuario para pasar el token a través de las aplicaciones que registre en Azure AD B2C.

Azure AD B2C actualmente solo admite pasar el token de acceso de proveedores de identidades de [OAuth 2.0](active-directory-b2c-reference-oauth-code.md), incluidos [Facebook](active-directory-b2c-setup-fb-app.md) y [Google](active-directory-b2c-setup-goog-app.md). Para todos los demás proveedores de identidades, la notificación se devuelve en blanco.

## <a name="prerequisites"></a>Requisitos previos

- La aplicación debe usar un [flujo de usuario v2](user-flow-versions.md).
- El flujo de usuario se configura con un proveedor de identidades de OAuth 2.0.

## <a name="enable-the-claim"></a>Habilitación de la notificación

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el **filtro de directorio y suscripciones** del menú superior y elija el directorio que contiene la suscripción.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. Seleccione **Flujos de usuario (directivas)** y, a continuación, elija el flujo de usuario. Por ejemplo, **B2C_1_signupsignin1**.
5. Seleccione **Notificaciones de aplicación**.
6. Habilite la notificación **Token de acceso del proveedor de identidades**.

    ![Habilitación de la notificación Token de acceso del proveedor de identidades](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-app-claim.png)

7. Haga clic en **Guardar** para guardar el flujo de usuario.

## <a name="test-the-user-flow"></a>Prueba del flujo de usuario

Al probar las aplicaciones en Azure AD B2C, puede ser útil que el token de Azure AD B2C se devuelva a `https://jwt.ms` para revisar las notificaciones en él.

1. En la página Introducción del flujo de usuario, seleccione **Ejecutar flujo de usuario**.
2. En **Aplicación**, seleccione la aplicación que registró anteriormente. Para ver el token en el ejemplo siguiente, **URL de respuesta** debe mostrar `https://jwt.ms`.
3. Haga clic en **Ejecutar flujo de usuario**y, a continuación, inicie sesión con sus credenciales de cuenta. Debería ver el token de acceso del proveedor de identidades en la notificación **idp_access_token**.

    Debería ver algo parecido al siguiente ejemplo:

    ![Token descodificado](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-token.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte la [información general sobre los tokens de Azure AD B2C](active-directory-b2c-reference-tokens.md).




