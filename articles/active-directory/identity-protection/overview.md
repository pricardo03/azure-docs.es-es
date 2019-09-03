---
title: Azure Active Directory Identity Protection | Microsoft Docs
description: Aprenda cómo Azure AD Identity Protection permite limitar la capacidad de un atacante para aprovechar una identidad o un dispositivo en peligro y asegurar una identidad o un dispositivo que antes fue sospechoso o que se sabe que estuvo en peligro.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 01/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b89cab41061376fc1d8b4cbffc8fe87b9677688
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125672"
---
# <a name="what-is-azure-active-directory-identity-protection"></a>¿Qué es Azure Active Directory Identity Protection?

Azure Active Directory [Identity](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis#terminology) Protection permite a las organizaciones configurar respuestas automatizadas a las acciones sospechosas detectadas relacionadas con las identidades de los usuarios.

## <a name="get-started"></a>Primeros pasos

Microsoft lleva más de una década con identidades basadas en la nube protegidas. Con Azure Active Directory Identity Protection en su entorno, puede usar los mismos sistemas de protección que Microsoft utiliza para proteger las identidades.

La mayoría de las infracciones de seguridad tienen lugar cuando los atacantes obtienen acceso a un entorno mediante el robo de identidad de un usuario. Con los años, los atacantes son cada vez más eficaces al aprovechar las brechas de otros fabricantes y el uso de ataques de suplantación de identidad sofisticados. Una vez que un atacante obtiene acceso aunque sea a una cuenta de usuario con pocos privilegios, es relativamente sencillo para ellos obtener acceso a recursos importantes de la empresa mediante el desplazamiento lateral.

Como consecuencia de esto, debe:

- Proteger todas las identidades independientemente de su nivel de privilegios
- Evitar de forma proactiva que se haga mal uso de las identidades que están en peligro.

Descubrir las identidades en peligro no es tarea fácil. Azure Active Directory utiliza algoritmos y heurística de aprendizaje automático adaptable para detectar anomalías e incidentes sospechosos que indican identidades que pueden estar en peligro. Con estos datos, Identity Protection genera informes y alertas que le permiten evaluar los problemas y tomar las acciones de corrección o mitigación adecuadas.

Azure Active Directory Identity Protection es más que una herramienta de supervisión e informes. Para proteger las identidades de la organización, puede configurar directivas basadas en riesgos que respondan automáticamente a problemas detectados si se alcanza un nivel de riesgo específico. Estas directivas, además de otros controles de acceso condicional proporcionados por Azure Active Directory y [Enterprise Mobility + Security](https://docs.microsoft.com/enterprise-mobility-security/) (EMS), pueden bloquear automáticamente o iniciar acciones de corrección adaptables que incluyen el restablecimiento de contraseñas y el cumplimiento de la autenticación multifactor.

### <a name="identity-protection-capabilities"></a>Funcionalidades de Identity Protection

**Detección de vulnerabilidades y cuentas peligrosas:**  

- Proporcionar recomendaciones personalizadas para mejorar la posición de seguridad general al resaltar los puntos vulnerables
- Cálculo de los niveles de riesgo de inicio de sesión
- Calcular los niveles de riesgo del usuario

**Investigación de las detecciones de riesgo:**

- Envío de notificaciones de las detecciones de riesgo
- Investigación de las detecciones de riesgo con información pertinente y contextual
- Proporcionar los flujos de trabajo básicos para realizar un seguimiento de las investigaciones
- Proporcionar un fácil acceso a las acciones de corrección, como el restablecimiento de contraseñas

**Directivas de acceso condicional basadas en riesgos:**

- Directiva para mitigar inicios de sesión peligrosos al bloquear inicios de sesión o solicitar desafíos de la autenticación multifactor.
- Directiva para proteger o bloquear cuentas de usuario peligrosas
- Directiva para exigir que los usuarios se registren en la autenticación multifactor

## <a name="identity-protection-roles"></a>Roles de Identity Protection

Para equilibrar la carga de las actividades de administración en torno a su implementación de Identity Protection, puede asignar varios roles. Azure AD Identity Protection admite tres roles de directorio:

| Role | Puede hacer | No puede hacer |
| :-- | --- | --- |
| Administrador global | Acceso completo a Identity Protection, incorporar Identity Protection| |
| Administrador de seguridad | Acceso completo a Identity Protection | Incorporar Identity Protection, restablecer contraseñas para un usuario |
| Lector de seguridad | Acceso de solo lectura a Identity Protection | Incorporar Identity Protection, remediar usuarios, configurar directivas, restablecer contraseñas |

Para obtener más detalles, consulte [Asignación de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)

## <a name="detection"></a>Detección

### <a name="vulnerabilities"></a>Puntos vulnerables

Azure Active Directory Identity Protection analiza la configuración y detecta las vulnerabilidades que pueden afectar a las identidades de usuario. Para conocer más detalles, consulte [Vulnerabilidades detectadas por Azure Active Directory Identity Protection](vulnerabilities.md).

### <a name="risk-detections"></a>Detecciones de riesgo

Azure Active Directory utiliza algoritmos y heurística de aprendizaje automático adaptable para detectar acciones sospechosas que están relacionadas con las identidades de usuario. El sistema crea un registro para cada acción sospechosa detectada. Estos registros se conocen también como detecciones de riesgo.  
Para más información, consulte [Eventos de riesgo de Azure Active Directory](../active-directory-identity-protection-risk-events.md).

## <a name="investigation"></a>Investigación

Normalmente su viaje a través de Identity Protection empieza en el panel de Identity Protection.

![Corrección](./media/overview/1000.png "Corrección")

El panel proporciona acceso a:

- Informes como **Usuarios marcados en riesgo**, **Eventos de riesgo** y **Vulnerabilidades**
- Opciones como la configuración de sus **directivas de seguridad**, **notificaciones** y el **registro de la autenticación multifactor**.

Normalmente, es el punto de partida para la investigación, que es el proceso de revisión de las actividades, los registros y otra información pertinente relacionada con una detección de riesgo para decidir si son necesarios pasos de mitigación o de corrección. En la investigación, también se busca averiguar cómo se ha puesto en peligro la identidad, así como comprender cómo se usó la identidad en peligro.

Puede vincular las actividades de investigación a las [notificaciones](notifications.md) que Azure Active Directory Identity Protection envía por correo electrónico.

## <a name="policies"></a>Directivas

Para implementar respuestas automatizadas, Azure Active Directory Identity Protection ofrece tres directivas:

- [Directiva de registro de autenticación multifactor](howto-mfa-policy.md)
- [Directiva de riesgo de usuario](howto-user-risk-policy.md)
- [Directiva de riesgo de inicio de sesión](howto-sign-in-risk-policy.md)

## <a name="license-requirements"></a>Requisitos de licencia

[!INCLUDE [Active Directory P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Channel 9: Azure AD and Identity Show: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
- [Habilitación de Azure Active Directory Identity Protection](enable.md)
