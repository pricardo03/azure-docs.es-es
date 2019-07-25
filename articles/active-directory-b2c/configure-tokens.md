---
title: 'Configuración de tokens: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo configurar las opciones de compatibilidad y duración de los tokens en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e1163c88a100ebb7500607475ab5740557904137
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66511330"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Configuración de tokens en Azure Active Directory B2C

En este artículo, obtendrá información sobre cómo configurar la [duración y la compatibilidad de un token](active-directory-b2c-reference-tokens.md) en Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Requisitos previos

[Cree un flujo de usuario](tutorial-create-user-flows.md) para permitir a los usuarios registrarse e iniciar sesión en la aplicación.

## <a name="configure-token-lifetime"></a>Configuración de la duración de los tokens

Puede configurar la duración de los tokens en cualquier flujo de usuario.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el **filtro Directorio y suscripción** del menú superior y elija el directorio que contiene el inquilino de Azure AD B2C.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Flujos de usuario (directivas)** .
5. Abra el flujo de usuario que creó anteriormente. 
6. Seleccione **Propiedades**.
7. En **Vigencia del token**, ajuste las propiedades siguientes para satisfacer las necesidades de su aplicación:

    ![Configuración de la duración de los tokens](./media/configure-tokens/token-lifetime.png)

8. Haga clic en **Save**(Guardar).

## <a name="configure-token-compatibility"></a>Configuración de compatibilidad del token

1. Seleccione **Flujos de usuario (directivas)** .
2. Abra el flujo de usuario que creó anteriormente. 
3. Seleccione **Propiedades**.
4. En **Configuración de compatibilidad del token**, ajuste las propiedades siguientes para satisfacer las necesidades de su aplicación:

    ![Configuración de compatibilidad del token](./media/configure-tokens/token-compatibility.png)

5. Haga clic en **Save**(Guardar).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo [usar los tokens de acceso](active-directory-b2c-access-tokens.md).



