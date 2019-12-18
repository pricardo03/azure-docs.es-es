---
title: 'Directivas comunes de acceso condicional: Azure Active Directory'
description: Directivas comunes de acceso condicional para organizaciones
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3d85850fb18b80490bba44b293ece7765124133
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74846120"
---
# <a name="common-conditional-access-policies"></a>Directivas de acceso condicional habituales

Las directivas de protección de línea de base son buenas, pero muchas organizaciones necesitan una flexibilidad mayor de la que ofrecen. Por ejemplo, muchas organizaciones necesitan poder excluir cuentas específicas, como las cuentas de administración de acceso de emergencia, de las directivas de acceso condicional que requieren autenticación multifactor. En esas organizaciones, se pueden usar las directivas comunes a las que se hace referencia en este artículo.

![Directivas de acceso condicional en Azure Portal](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Cuentas de acceso de emergencia

En los siguientes artículos encontrará más información sobre las cuentas de acceso de emergencia y por qué son importantes: 

* [Administración de cuentas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md)
* [Crear una estrategia de administración de control de acceso resistente con Azure Active Directory](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Directivas habituales implementadas por organizaciones

* [Exigir autenticación multifactor para administradores](howto-conditional-access-policy-admin-mfa.md)
* [Exigir autenticación multifactor para la administración de Azure](howto-conditional-access-policy-azure-management.md)
* [Exigir autenticación multifactor para todos los usuarios](howto-conditional-access-policy-all-users-mfa.md)
* [Bloquear la autenticación heredada](howto-conditional-access-policy-block-legacy.md)
* [Acceso condicional basado en el riesgo (se necesita Azure AD Premium P2)](howto-conditional-access-policy-risk.md)
* [Exigir ubicaciones de confianza para el registro de la autenticación multifactor](howto-conditional-access-policy-registration.md)
* [Bloquear el acceso por ubicación](howto-conditional-access-policy-location.md)
* [Requerir dispositivo compatible](howto-conditional-access-policy-compliant-device.md)

## <a name="next-steps"></a>Pasos siguientes

- [Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional.](troubleshoot-conditional-access-what-if.md)
- [Uso del modo de solo informe de acceso condicional para determinar el impacto de las nuevas decisiones de directiva.](concept-conditional-access-report-only.md)
