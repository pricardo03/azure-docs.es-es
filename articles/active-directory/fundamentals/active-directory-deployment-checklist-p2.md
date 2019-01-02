---
title: 'Lista de comprobación de la característica de licencia Premium P2: Azure Active Directory | Microsoft Docs'
description: Lista de comprobación de la implementación de la característica Azure Active Directory Premium P2 en 30 días, 90 días y más.
services: active-directory
ms.service: active-directory
ms.component: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.custom: it-pro, seodec18
ms.openlocfilehash: 61cefe762b639dcc58c2e10fc845dd9729cb2cb2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53078249"
---
# <a name="azure-active-directory-premium-p2-licensing-feature-checklist"></a>Lista de comprobación de la característica de licencia de Azure Active Directory Premium P2

Puede parecer abrumador implementar Azure Active Directory (Azure AD) para su organización y mantener la seguridad. En este artículo se identifican algunas tareas comunes que los clientes encuentran útiles. Normalmente, los clientes completan estas tareas en un período de 30 días, 90 días o más para mejorar su posición de seguridad. Incluso las organizaciones que ya han implementado Azure AD pueden usar esta lista de comprobación para asegurarse de que sacan el máximo partido de su inversión.

Una infraestructura de identidades bien planeada y ejecutada prepara el terreno para que únicamente los usuarios y los dispositivos autenticados obtengan acceso de forma más segura a los datos y a las cargas de trabajo de productividad.

## <a name="prerequisites"></a>Requisitos previos

En esta guía, se supone que dispone de las licencias de Azure AD Premium P2, Enterprise Mobility + Security E5, Microsoft 365 E5 o un conjunto de licencias similar.

[Licencias de Azure AD](https://azure.microsoft.com/pricing/details/active-directory/)

[Microsoft 365 Enterprise](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)

[Enterprise Mobility + Security](https://www.microsoft.com/en-us/licensing/product-licensing/enterprise-mobility-security)

## <a name="plan-and-deploy-day-1-30"></a>Planeamiento e implementación: Día 1 a 30

- Designación de más de un administrador global (cuenta excepcional)
   - [Administración de cuentas administrativas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md)
- Activación de Azure AD Privileged Identity Management (PIM) para ver informes
   - [Primer uso de PIM](../privileged-identity-management/pim-getting-started.md)
- Uso de roles administrativos no globales siempre que sea posible
   - [Asignación de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)
- Autenticación
   - [Implementación del autoservicio de restablecimiento de contraseña](../authentication/howto-sspr-deployment.md)
   - Implementación de la protección con contraseña de Azure AD (versión preliminar)
      - [Elimine las contraseñas incorrectas de su organización](../authentication/concept-password-ban-bad.md)
      - [Aplicación de Protección con contraseña de Azure AD para Windows Server Active Directory](../authentication/concept-password-ban-bad-on-premises.md)
   - Configuración de directivas de bloqueo de cuentas
      - [Bloqueo inteligente de Azure Active Directory](../authentication/howto-password-smart-lockout.md)
      - [AD FS Extranet Lockout and Extranet Smart Lockout](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection) (Bloqueo de extranet de AD FS y bloqueo inteligente de extranet)
   - [Implementación de Azure AD Multi-Factor Authentication mediante directivas de acceso condicional](../authentication/howto-mfa-getstarted.md)
   - [Habilitación del registro convergente para autoservicio de restablecimiento de contraseña y Azure AD Multi-Factor Authentication (versión preliminar)](../authentication/concept-registration-mfa-sspr-converged.md)
   - [Habilitación de Azure Active Directory Identity Protection](../identity-protection/enable.md)
      - [Uso de eventos de riesgo para desencadenar cambios de contraseñas y Multi-Factor Authentication](../authentication/tutorial-risk-based-sspr-mfa.md)
   - [Guía de contraseñas](https://www.microsoft.com/research/publication/password-guidance/)
      - Mantener un requisito de una longitud mínima de ocho caracteres ya no es necesariamente mejor.
      - Eliminar los requisitos de composición de caracteres.
      - [Eliminar los restablecimientos de contraseña periódicos obligatorios para las cuentas de usuario.](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire)
- Sincronización de los usuarios desde Active Directory local
   - [Instalación de Azure AD Connect](../connect/active-directory-aadconnect-select-installation.md)
   - [Implementación de la sincronización de hash de contraseñas](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)
   - [Implementación de la escritura diferida de contraseñas](../authentication/howto-sspr-writeback.md)
   - [Implementación de Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md)
- [Asignación de licencias a usuarios según su pertenencia a un grupo en Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)

## <a name="plan-and-deploy-day-31-90"></a>Planeamiento e implementación: Día 31 a 90

- [Planeamiento del acceso de usuarios invitados](../b2b/what-is-b2b.md)
   - [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](../b2b/add-users-administrator.md)
   - [Permitir o bloquear invitaciones a usuarios B2B procedentes de determinadas organizaciones](../b2b/allow-deny-list.md)
   - [Conceder a los usuarios B2B de Azure AD acceso a las aplicaciones locales](../b2b/hybrid-cloud-to-on-premises.md)
- Toma de decisiones sobre la estrategia de administración del ciclo de vida del usuario
- [Decisión acerca de la estrategia de administración de dispositivos](../devices/overview.md)
   - [Escenarios de uso y consideraciones de implementación de Azure AD Join](../devices/azureadjoin-plan.md)
- [Habilitación de Windows Hello para empresas en su organización](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization)

## <a name="plan-and-deploy-day-90-and-beyond"></a>Planeamiento e implementación: Día 90 y más

- [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md)
   - [Configuración de roles de directorio de Azure AD en PIM](../privileged-identity-management/pim-how-to-change-default-settings.md)
   - [Asignación de roles de directorio de Azure AD en PIM](../privileged-identity-management/pim-how-to-add-role-to-user.md)
- [Completar una revisión de acceso para los roles de directorio de Azure AD en PIM](../privileged-identity-management/pim-how-to-start-security-review.md)
- Administración del ciclo de vida del usuario holísticamente
   - Azure AD tiene un enfoque para administrar el ciclo de vida de las identidades
   - Eliminación de los pasos manuales del ciclo de vida de la cuenta del empleado para evitar el acceso no autorizado:
      - Sincronización de identidades desde un origen fiable (sistema de RR. HH.) en Azure AD. (vínculo a los sistemas de RR. HH. compatibles)
      - [Uso de grupos dinámicos para asignar automáticamente usuarios a grupos en función de sus atributos de RR. HH. (o su origen fiable), como el departamento, el título, la región y otros atributos](../users-groups-roles/groups-dynamic-membership.md).
      - [Uso del aprovisionamiento de administración del acceso basado en grupos para aprovisionar usuarios automáticamente para las aplicaciones SaaS](../manage-apps/what-is-access-management.md).

## <a name="next-steps"></a>Pasos siguientes

[Configuraciones de acceso de dispositivos e identidades](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[Directivas comunes de acceso a dispositivos e identidades](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
