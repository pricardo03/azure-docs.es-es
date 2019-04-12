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
ms.date: 04/10/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d26ff0f9259e3531259673f94fe477444cc786b
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59491602"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Opciones de inicio de sesión para cuentas de Microsoft en Azure Active Directory

La página de inicio de sesión de Microsoft 365 para Azure Active Directory (Azure AD) admite cuentas educativas y profesionales o cuentas de Microsoft, pero según la situación del usuario, podría ser una u otra o ambas. Por ejemplo, la página de inicio de sesión de Azure AD es compatible con:

* Aplicaciones que acepten inicios de sesión con ambos tipos de cuenta
* Organizaciones que aceptan los invitados

## <a name="identification"></a>Identificación
Puede indicar si la página de inicio de sesión de que su organización usa es compatible con las cuentas de Microsoft examinando el texto de sugerencia en el campo de nombre de usuario. Si aparece el texto de la sugerencia "Correo electrónico, teléfono o Skype", la página de inicio de sesión es compatible con las cuentas de Microsoft.

![Diferencia entre las páginas de inicio de sesión de cuenta](./media/signin-account-support/ui-prompt.png)

[Opciones de inicio de sesión adicionales solo funcionan para cuentas personales de Microsoft](https://azure.microsoft.com/updates/microsoft-account-signin-options/ ) pero no se puede usar para iniciar sesión para profesionales o educativas de recursos de la cuenta.

## <a name="next-steps"></a>Pasos siguientes

[Personalizar su inicio de sesión de marca](../fundamentals/add-custom-domain.md)