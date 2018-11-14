---
title: Multi-Factor Authentication en Azure Active Directory B2C | Microsoft Docs
description: Habilitación de Multi-Factor Authentication en las aplicaciones orientadas al consumidor protegidas por Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: eabae0f3575719c6cb93affefe0a393dd13d1439
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014013"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Habilitación de la autenticación multifactor en Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C se integra directamente con [Azure Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) de forma que puede agregar una segunda capa de seguridad a las experiencias de registro e inicio de sesión en las aplicaciones. La autenticación multifactor se habilita sin necesidad de escribir una sola línea de código. Aunque haya creado directivas de registro e inicio de sesión, puede habilitar la autenticación multifactor.

Esta característica ayuda a las aplicaciones a tratar escenarios como los siguientes:

- No se requiere autenticación multifactor para acceder a una aplicación, pero sí para acceder a otra. Por ejemplo, el cliente puede iniciar sesión en una aplicación de seguros de automóvil con una cuenta local o social, pero debe comprobar el número de teléfono antes de acceder a la aplicación principal de seguros registrada en el mismo directorio.
- No se requiere autenticación multifactor para acceder a una aplicación en general, pero sí para acceder a las partes confidenciales que contiene. Por ejemplo, el cliente puede iniciar sesión en una aplicación de banca con una cuenta social o local y comprobar el saldo de las cuentas, pero debe confirmar el número de teléfono antes de realizar una transferencia bancaria.

## <a name="set-multi-factor-authentication"></a>Configuración de la autenticación multifactor

Cuando se crea una directiva, tiene la opción de habilitar la autenticación multifactor.

![Configuración de la autenticación multifactor](./media/active-directory-b2c-reference-mfa/add-policy.png)

Establezca **Estado** en **Activado**.

Puede usar **Ejecutar ahora** en la directiva para comprobar la experiencia. Considere el siguiente escenario:

Se crea una cuenta de cliente en el inquilino antes de que se produzca el paso de autenticación multifactor. Durante el paso, se pide al cliente que proporcione un número de teléfono y que lo compruebe. Si la comprobación es satisfactoria, el número de teléfono se asocia a la cuenta para usarlo más adelante. Incluso si el cliente cancela o abandona, se le puede pedir que compruebe de nuevo un número de teléfono durante el siguiente inicio de sesión con la autenticación multifactor habilitada.

## <a name="add-multi-factor-authentication"></a>Incorporación de la autenticación multifactor

Es posible habilitar la autenticación multifactor en una directiva que creó anteriormente. 

Para habilitar la autenticación multifactor:

1. Abra la directiva y, a continuación, seleccione **Editar**. 
2. Seleccione **Multi-Factor Authentication**.
3. Establezca **Estado** en **Activado**.
4. Haga clic en **Guardar** en la parte superior de la página.


