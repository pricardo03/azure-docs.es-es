---
title: Configuración de la directiva de riesgo de inicio de sesión en Azure Active Directory Identity Protection | Microsoft Docs
description: Obtenga información sobre cómo configurar la directiva de riesgo de inicio de sesión de Azure AD Identity Protection.
services: active-directory
keywords: azure active directory identity protection, detección de aplicaciones en la nube, administración de aplicaciones, seguridad, riesgo, nivel de riesgo, punto vulnerable, directiva de seguridad
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: markvi
ms.reviewer: raluthra
ms.collection: M365-identity-device-management
ms.openlocfilehash: da3f03cfcf821fa093e9f21cf9a11a2d99d67683
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57999045"
---
# <a name="how-to-configure-the-sign-in-risk-policy"></a>Instrucciones: Configuración de la directiva de riesgo de inicio de sesión

Azure Active Directory detecta [tipos de eventos de riesgo](../reports-monitoring/concept-risk-events.md#risk-event-types) en tiempo real y sin conexión. Cada evento de riesgo que se haya detectado durante el inicio de sesión de un usuario contribuye a un concepto lógico, llamado inicio de sesión peligroso. Un inicio de sesión peligroso es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.


## <a name="what-is-the-sign-in-risk-policy"></a>¿Qué es la directiva de riesgo de inicio de sesión?

AD Azure analiza cada inicio de sesión de un usuario. El objetivo del análisis es detectar acciones sospechosas que se realicen con el inicio de sesión. Por ejemplo, ¿el inicio de sesión se realiza con una dirección IP anónima o el inicio de sesión comenzó en una ubicación desconocida? En Azure AD, las acciones sospechosas que el sistema puede detectar también se conocen como eventos de riesgo. En función de los eventos de riesgo detectados durante un inicio de sesión, Azure AD calcula un valor. El valor representa la probabilidad (baja, media, alta) de que el inicio de sesión no proceda de un usuario legítimo. Esta probabilidad se denomina **nivel de riesgo de inicio de sesión**.

La directiva de riesgo de inicio de sesión es una respuesta automatizada que puede configurar para un nivel de riesgo de inicio de sesión específico. En la respuesta, puede bloquear el acceso a los recursos o enviar un desafío de autenticación multifactor (MFA) para obtener acceso.

   
## <a name="how-do-i-access-the-sign-in-risk-policy"></a>¿Cómo puedo acceder a la directiva de riesgo de inicio de sesión?
   
En la hoja **Azure AD Identity Protection**, en la sección [Configurar](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy), está la directiva de riesgo de inicio de sesión.
   
![Directiva de riesgo de inicio de sesión](./media/howto-sign-in-risk-policy/1014.png "Directiva de riesgo de inicio de sesión")


## <a name="policy-settings"></a>Configuración de directiva

Al configurar la directiva de riesgo de inicio de sesión, deberá establecer:

- Usuarios y grupos a los que se aplica la directiva:

    ![Usuarios y grupos](./media/howto-sign-in-risk-policy/11.png)

- El nivel de riesgo de inicio de sesión que activa la directiva:

    ![Nivel de riesgo del inicio de sesión](./media/howto-sign-in-risk-policy/12.png)

- El tipo de acceso que desea aplicar cuando se alcance el nivel de riesgo de inicio de sesión:  

    ![Access](./media/howto-sign-in-risk-policy/13.png)

- Estado de la directiva:

    ![Aplicar directiva](./media/howto-sign-in-risk-policy/14.png)


El cuadro de diálogo de configuración de directivas ofrece la opción de calcular el impacto de la configuración.

![Impacto estimado](./media/howto-sign-in-risk-policy/15.png)

## <a name="what-you-should-know"></a>Qué debería saber

Puede configurar una directiva de seguridad de riesgo de inicio de sesión para requerir MFA:

![Requerir MFA](./media/howto-sign-in-risk-policy/16.png)

Sin embargo, por seguridad, esta configuración solo funciona para los usuarios que ya se hayan registrado para MFA. La protección de identidad bloquea a los usuarios con un requisito de MFA si aún no están registrados en MFA.

Si desea requerir MFA para los inicios de sesión con riesgo, haga lo siguiente:

1. Habilite la [directiva de registro de la autenticación multifactor](howto-mfa-policy.md) para los usuarios afectados.

2. Pida a los usuarios afectados que inicien una sesión sin riesgo para llevar a cabo el registro en MFA.

Tras completar estos pasos, se garantiza que la autenticación multifactor sea necesaria para un inicio de sesión de riesgo.

La directiva de riesgo de inicio de sesión:

- Se aplica a todo el tráfico del explorador e inicios de sesión mediante una autenticación moderna.

- No afecta a aplicaciones que utilizan protocolos de seguridad anteriores al deshabilitar el punto de conexión WS-Trust en el IDP federado, como ADFS.


Para obtener información general de la experiencia de usuario relacionada, consulte:

* [Recuperación de inicios de sesión peligrosos](flows.md#risky-sign-in-recovery)
* [Inicios de sesión peligrosos bloqueados](flows.md#risky-sign-in-blocked)  
* [Experiencias de inicio de sesión con Azure AD Identity Protection](flows.md)  

## <a name="best-practices"></a>Procedimientos recomendados

Elegir un umbral **Alto** reduce el número de veces que una directiva se desencadena y minimiza el impacto en los usuarios.  

Sin embargo, excluye de la directiva a los usuarios marcados con un riesgo **Bajo** y **Medio**, por lo que es posible que no impida que un atacante aproveche una identidad en peligro.

Al establecer la directiva:

- Excluya a los usuarios que no tengan o no puedan tener la autenticación multifactor.

- Excluya a los usuarios que tengan configuraciones regionales en las que no resulte práctico habilitar la directiva (por ejemplo, no tienen acceso al departamento de soporte técnico).

- Excluya a los usuarios que es probable que generen una gran cantidad de falsos positivos (desarrolladores, analistas de seguridad).

- Utilice un umbral **Alto** durante la puesta en servicio inicial de la directiva, o bien si debe minimizar los desafíos que ven los usuarios finales.

- Use un umbral **Bajo** si su organización requiere un mayor grado de seguridad. Seleccionar un umbral **Bajo** presenta desafíos adicionales de inicio de sesión del usuario, pero aumenta la seguridad.

El valor predeterminado recomendado para la mayoría de las organizaciones es configurar una regla para un umbral **Medio** con el fin de lograr un equilibrio entre la facilidad de uso y la seguridad.






## <a name="next-steps"></a>Pasos siguientes

Para obtener una visión general de Azure AD Identity Protection, consulte la [Introducción a Azure AD Identity Protection](overview.md).
