---
title: 'Configuración de tokens: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo configurar las opciones de compatibilidad y la duración de token en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 2455b8c12ed042af3a06a158a4e5c60a0aee748b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60384041"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Configuración de tokens de Azure Active Directory B2C

En este artículo, obtendrá información sobre cómo configurar el [duración y la compatibilidad de un token](active-directory-b2c-reference-tokens.md) en Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Requisitos previos

[Cree un flujo de usuario](tutorial-create-user-flows.md) para permitir a los usuarios registrarse e iniciar sesión en la aplicación.

## <a name="configure-token-lifetime"></a>Configurar la duración del token

Puede configurar la duración del token en cualquier flujo de usuario.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene al inquilino de Azure AD B2C. Seleccione el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino de Azure AD B2C.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **flujos de usuario (directivas)**.
5. Abra el flujo de usuario que creó anteriormente. 
6. Seleccione **Propiedades**.
7. En **vigencia del Token**, ajuste las propiedades siguientes para satisfacer las necesidades de su aplicación:

    ![Configurar la duración del token](./media/configure-tokens/token-lifetime.png)

8. Haga clic en **Save**(Guardar).

## <a name="configure-token-compatibility"></a>Configurar la compatibilidad de token

1. Seleccione **flujos de usuario (directivas)**.
2. Abra el flujo de usuario que creó anteriormente. 
3. Seleccione **Propiedades**.
4. En **configuración de compatibilidad de Token**, ajuste las propiedades siguientes para satisfacer las necesidades de su aplicación:

    ![Configurar la compatibilidad de token](./media/configure-tokens/token-compatibility.png)

5. Haga clic en **Save**(Guardar).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo [usar los tokens de acceso](active-directory-b2c-access-tokens.md).



