---
title: 'Directivas comunes de acceso condicional: Azure Active Directory'
description: Directivas comunes de acceso condicional para organizaciones
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: abcca0d2712a462e4d2ecf9c8023d0cb0e68ad6c
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69576015"
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
* [Bloquear la autenticación heredada](howto-conditional-access-policy-block-legacy.md)
* [Acceso condicional basado en el riesgo (se necesita Azure AD Premium P2)](howto-conditional-access-policy-risk.md)
* [Exigir ubicaciones de confianza para el registro de la autenticación multifactor](howto-conditional-access-policy-registration.md)
* [Bloquear el acceso por ubicación](howto-conditional-access-policy-location.md)
* [Requerir dispositivo compatible](howto-conditional-access-policy-compliant-device.md)

## <a name="next-steps"></a>Pasos siguientes

[Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional](troubleshoot-conditional-access-what-if.md)
