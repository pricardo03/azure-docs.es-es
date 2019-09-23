---
title: ¿Qué es el acceso condicional en Azure Active Directory?
description: Descubra los motivos por los que el acceso condicional está en el centro del nuevo plano de control basado en identidades.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 09/17/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0463ffad87d00421c2fcb5c8357406d5f692144
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71075317"
---
# <a name="what-is-conditional-access"></a>¿Qué es el acceso condicional?

El perímetro de seguridad moderno ahora se extiende más allá de la red de una organización, incluye tanto la identidad del usuario como la del dispositivo. Las organizaciones pueden usar estas señales de identidad como parte de sus decisiones de control de acceso. 

El acceso condicional es la herramienta que usa Azure Active Directory para reunir las señales, tomar decisiones y aplicar las directivas de la organización. El acceso condicional está en el centro del nuevo plano de control basado en identidades.

![Señal condicional conceptual más la decisión de obtener el cumplimiento](./media/overview/conditional-access-signal-decision-enforcement.png)

En su forma más simple, las directivas de acceso condicional son instrucciones if-then; si un usuario desea tener acceso a un recurso, deben completar una acción. Ejemplo: un responsable de nóminas desea acceder a la aplicación de nóminas y es necesario para realizar la autenticación multifactor para tener poder hacerlo.

Los administradores se enfrentan a dos objetivos principales:

- Capacitar a los usuarios para ser productivos donde sea y cuando sea.
- Proteger los recursos de la organización.

Mediante el uso de directivas de acceso condicional puede aplicar los controles de acceso correctos cuando sea necesario para mantener la organización segura y no interferir con los usuarios cuando no se necesita.

![Flujo del proceso de acceso condicional conceptual](./media/overview/conditional-access-overview-how-it-works.png)

Las directivas de acceso condicional se aplican una vez que se completa la autenticación en una fase. El acceso condicional no está pensado como primera línea de defensa de una organización en escenarios como los ataques por denegación de servicio (DoS), pero puede usar señales de estos eventos para determinar el acceso.

## <a name="common-signals"></a>Señales comunes

Entre las señales comunes que puede tener en cuenta el acceso condicional al tomar una decisión sobre una directiva se incluyen las siguientes:

- Pertenencia a un usuario o grupo
   - Las directivas pueden dirigirse a usuarios y grupos concretos, lo que proporciona a los administradores un mayor control sobre el acceso.
- Información de la ubicación de la IP
   - Las organizaciones pueden crear intervalos de direcciones IP de confianza que se pueden usar al tomar decisiones sobre directivas. 
   - Los administradores pueden especificar que se bloquee o se permita el tráfico de intervalos de IP de países completos.
- Dispositivo
   - Los usuarios con dispositivos de plataformas concretas o marcados con un estado concreto se pueden usar al aplicar directivas de acceso condicional.
- Application
   - Los usuarios que intentan acceder a aplicaciones específicas pueden desencadenar distintas directivas de acceso condicional. 
- Detección de riesgo calculado y en tiempo real
   - La integración de señales con Azure AD Identity Protection permite que las directivas de acceso condicional identifiquen un comportamiento de inicio de sesión peligroso. Luego, las directivas pueden obligar a los usuarios a realizar cambios de contraseña o a usar la autenticación multifactor para reducir su nivel de riesgo, o a bloquear su acceso hasta que algún administrador lleve a cabo una acción manual.
- Microsoft Cloud App Security (MCAS)
   - Permite el control y la supervisión en tiempo real de las sesiones y el acceso a las aplicaciones de usuario, lo que aumenta la visibilidad y el control sobre el acceso y las actividades realizadas dentro del entorno de nube.

## <a name="common-decisions"></a>Decisiones comunes

- Bloquear acceso
   - Decisión más restrictiva
- Conceder acceso
   - Decisión menos restrictiva, puede requerir una o varias de las opciones siguientes:
      - Requerir autenticación multifactor
      - Requerir que el dispositivo esté marcado como compatible
      - Requerir un dispositivo conectado a Azure AD híbrido
      - Requerir aplicación cliente aprobada
      - Requerir una directiva de protección de aplicaciones (versión preliminar)

## <a name="commonly-applied-policies"></a>Directivas que se aplican habitualmente

Muchas organizaciones tienen problemas de acceso comunes y las directivas de acceso condicional pueden servir de ayuda al respecto. Dicha ayuda se concreta en:

- Requerir la autenticación multifactor a los usuarios con roles administrativos
- Requerir la autenticación multifactor para las tareas de administración de Azure
- Bloquear los inicios de sesión a los usuarios que intenten usar protocolos de autenticación heredados
- Requerir ubicaciones de confianza para el registro de Azure Multi-Factor Authentication
- Bloquear o conceder el acceso desde ubicaciones concretas
- Bloquear de comportamientos de inicio de sesión peligrosos
- Requerir dispositivos administrados por la organización para aplicaciones concretas

## <a name="license-requirements"></a>Requisitos de licencia

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

Los clientes con [licencias de Microsoft 365 Empresa](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) también tienen acceso a características de acceso condicional. 

## <a name="next-steps"></a>Pasos siguientes

[Creación de una directiva de acceso condicional paso a paso](concept-conditional-access-policies.md)

Para saber cómo implementar el acceso condicional en su entorno, consulte [Planeamiento de la implementación del acceso condicional en Azure Active Directory](plan-conditional-access.md).

[Información acerca de Identity Protection](../identity-protection/overview-v2.md)

[Información acerca de Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security)

[Información acerca de Microsoft Intune](https://docs.microsoft.com/intune/index)