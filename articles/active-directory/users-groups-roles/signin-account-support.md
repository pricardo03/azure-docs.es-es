---
title: Cómo saber si una página de inicio de sesión de Azure AD acepta cuentas de Microsoft | Microsoft Docs
description: En la pantalla de mensajería refleja la búsqueda de nombre de usuario durante el inicio de sesión
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a62c4d56fbfca34ff6291863149b078f7ddc6680
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288597"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Opciones de inicio de sesión para cuentas de Microsoft en Azure Active Directory

La página de inicio de sesión de Microsoft 365 para Azure Active Directory (Azure AD) admite una o ambas de las cuentas educativas y profesionales o cuentas de Microsoft, según la situación, para admitir:

* Aplicaciones que acepten inicios de sesión con ambos tipos de cuenta
* Organizaciones que aceptan los invitados

## <a name="identification"></a>Identificación
Puede indicar si la página de inicio de sesión de que su organización usa es compatible con las cuentas de Microsoft examinando el texto de sugerencia en el campo de nombre de usuario. Si aparece el texto de la sugerencia "Correo electrónico, teléfono o Skype", la página de inicio de sesión es compatible con las cuentas de Microsoft.

![Diferencia entre las páginas de inicio de sesión de cuenta](./media/signin-account-support/ui-prompt.png)

[Opciones de inicio de sesión adicionales solo funcionan para cuentas personales de Microsoft](index.yml) pero no se puede usar para iniciar sesión para profesionales o educativas de recursos de la cuenta.

## <a name="next-steps"></a>Pasos siguientes

[Personalizar su inicio de sesión de marca](../fundamentals/add-custom-domain.md)