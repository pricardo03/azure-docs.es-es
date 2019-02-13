---
title: Configuración de las directivas de riesgo en Azure Active Directory Identity Protection (actualizado) | Microsoft Docs
description: Cómo configurar las directivas de riesgo en Azure Active Directory Identity Protection (actualizado).
services: active-directory
keywords: azure active directory identity protection, detección de aplicaciones en la nube, administración de aplicaciones, seguridad, riesgo, nivel de riesgo, punto vulnerable, directiva de seguridad
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: f9ffbebee06ab7b9e010c7e6c84fee533611fb38
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567662"
---
# <a name="how-to-configure-risk-policies-in-azure-active-directory-identity-protection-refreshed"></a>Instrucciones: Configuración de las directivas de riesgo en Azure Active Directory Identity Protection (actualizado)


Azure AD detecta eventos de riesgo que son indicadores de identidades potencialmente en peligro. Mediante la configuración de directivas de riesgo, puede definir respuestas automatizadas a los resultados de detección:

- Con la directiva de riesgo de inicio de sesión, puede configurar una respuesta a eventos de riesgo en tiempo real que se detecten durante el inicio de sesión del usuario. 
- Con la directiva de riesgo del usuario, puede configurar una respuesta a todos los riesgos de usuario activo que se han detectado para un usuario a lo largo del tiempo.  


## <a name="what-is-the-sign-in-risk-policy"></a>¿Qué es la directiva de riesgo de inicio de sesión?

AD Azure analiza cada inicio de sesión de un usuario. El objetivo del análisis es detectar acciones sospechosas que se realicen con el inicio de sesión. Por ejemplo, ¿el inicio de sesión se realiza con una dirección IP anónima o el inicio de sesión comenzó en una ubicación desconocida? En Azure AD, las acciones sospechosas que el sistema puede detectar también se conocen como eventos de riesgo. En función de los eventos de riesgo detectados durante un inicio de sesión, Azure AD calcula un valor. El valor representa la probabilidad (baja, media, alta) de que el inicio de sesión no proceda de un usuario legítimo. Esta probabilidad se denomina **nivel de riesgo de inicio de sesión**.

La directiva de riesgo de inicio de sesión es una respuesta automatizada que puede configurar para un nivel de riesgo de inicio de sesión específico. En la respuesta, puede bloquear el acceso a los recursos o enviar un desafío de autenticación multifactor (MFA) para obtener acceso.

   
## <a name="how-do-i-access-the-sign-in-risk-policy"></a>¿Cómo puedo acceder a la directiva de riesgo de inicio de sesión?
   
En la hoja **Azure AD Identity Protection**, en la sección [Configurar](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy), está la directiva de riesgo de inicio de sesión.
   
![Directiva de riesgo de inicio de sesión](./media/howto-configure-risk-policies/1014.png "Directiva de riesgo de inicio de sesión")


## <a name="sign-in-risk-policy-settings"></a>Configuración de la directiva de riesgo de inicio de sesión

Al configurar la directiva de riesgo de inicio de sesión, deberá establecer:

- Usuarios y grupos a los que se aplica la directiva:

    ![Usuarios y grupos](./media/howto-configure-risk-policies/11.png)

- El nivel de riesgo de inicio de sesión que activa la directiva:

    ![Nivel de riesgo del inicio de sesión](./media/howto-configure-risk-policies/12.png)

- El tipo de acceso que desea aplicar cuando se alcance el nivel de riesgo de inicio de sesión:  

    ![Access](./media/howto-configure-risk-policies/13.png)

- Estado de la directiva:

    ![Aplicar directiva](./media/howto-configure-risk-policies/14.png)


El cuadro de diálogo de configuración de directivas ofrece la opción de calcular el impacto de la configuración.

![Impacto estimado](./media/howto-configure-risk-policies/15.png)

## <a name="what-you-should-know-about-sign-in-risk-policies"></a>Qué debe saber acerca de las directivas de riesgo de inicio de sesión

Puede configurar una directiva de seguridad de riesgo de inicio de sesión para requerir MFA:

![Requerir MFA](./media/howto-configure-risk-policies/16.png)

Sin embargo, por seguridad, esta configuración solo funciona para los usuarios que ya se hayan registrado para MFA. La protección de identidad bloquea a los usuarios con un requisito de MFA si aún no están registrados en MFA.

Si desea requerir MFA para los inicios de sesión con riesgo, haga lo siguiente:

1. Habilite la [directiva de registro de la autenticación multifactor](#multi-factor-authentication-registration-policy) para los usuarios afectados.

2. Requiera que los usuarios afectados inicien una sesión sin riesgo para llevar a cabo el registro en MFA.

Tras completar estos pasos, se garantiza que la autenticación multifactor sea necesaria para un inicio de sesión de riesgo.

La directiva de riesgo de inicio de sesión:

- Se aplica a todo el tráfico del explorador e inicios de sesión mediante una autenticación moderna.

- No afecta a aplicaciones que utilizan protocolos de seguridad anteriores al deshabilitar el punto de conexión WS-Trust en el IDP federado, como ADFS.


Para obtener información general de la experiencia de usuario relacionada, consulte:

* [Recuperación de inicios de sesión peligrosos](flows.md#risky-sign-in-recovery)
* [Inicios de sesión peligrosos bloqueados](flows.md#risky-sign-in-blocked)  
* [Experiencias de inicio de sesión con Azure AD Identity Protection](flows.md)  









## <a name="what-is-a-user-risk-policy"></a>¿Qué es una directiva de riesgo de usuario?

AD Azure analiza cada inicio de sesión de un usuario. El objetivo del análisis es detectar acciones sospechosas que se realicen con el inicio de sesión. En Azure AD, las acciones sospechosas que el sistema puede detectar también se conocen como eventos de riesgo. Aunque algunos eventos de riesgo se pueden detectar en tiempo real, también hay eventos de riesgo que requieren más tiempo. Por ejemplo, para detectar un viaje imposible a ubicaciones inusuales, el sistema requiere un período de aprendizaje inicial de 14 días para conocer el comportamiento normal de un usuario. Hay varias opciones para resolver los eventos de riesgo detectados. Por ejemplo, puede resolver los eventos de riesgo individuales manualmente, o puede resolverlos con una directiva de acceso condicional de riesgo de inicio de sesión o de riesgo de usuario.

Todos los eventos de riesgo que se han detectado para un usuario y no se resuelven, se conocen como eventos de riesgo activos. Los eventos de riesgo activos que están asociados con un usuario se conocen como riesgos de usuario. Según el riesgo de usuario, Azure AD calcula una probabilidad (baja, media, alta) de que un usuario esté en peligro. La probabilidad se denomina nivel de riesgo de usuario.

![Riesgos de usuario](./media/howto-configure-risk-policies/11031.png)

La directiva de riesgo de usuario es una respuesta automatizada que puede configurar para un nivel de riesgo de usuario específico. Con una directiva de riesgo de usuario, puede bloquear el acceso a los recursos o requerir un cambio de contraseña para que la cuenta de usuario vuelva a estar limpia.


## <a name="how-do-i-access-the-user-risk-policy"></a>¿Cómo puedo acceder a la directiva de riesgo del usuario?
   
En la página **Azure AD Identity Protection**, en la sección [Configurar](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy), está la directiva de riesgo de usuario.
   
![Directiva de riesgo de usuario](./media/howto-configure-risk-policies/11014.png)



## <a name="user-risk-policy-settings"></a>Configuración de la directiva de riesgo de usuario

Al configurar la directiva de riesgo de usuario, deberá establecer:

- Usuarios y grupos a los que se aplica la directiva:

    ![Usuarios y grupos](./media/howto-configure-risk-policies/111.png)

- El nivel de riesgo de inicio de sesión que activa la directiva:

    ![Nivel de riesgo del usuario](./media/howto-configure-risk-policies/112.png)

- El tipo de acceso que desea aplicar cuando se alcance el nivel de riesgo de inicio de sesión:  

    ![Access](./media/howto-configure-risk-policies/113.png)

- Estado de la directiva:

    ![Aplicar directiva](./media/howto-configure-risk-policies/114.png)

El cuadro de diálogo de configuración de directivas proporciona una opción para calcular el impacto de la configuración.

![Impacto estimado](./media/howto-configure-risk-policies/115.png)

## <a name="what-you-should-know-about-user-risk-polices"></a>Qué debe saber acerca de las directivas de riesgo de usuario

Puede establecer una directiva de seguridad de riesgo de usuario para impedir que los usuarios inicien sesión en función del nivel de riesgo.

![Bloqueo](./media/howto-configure-risk-policies/116.png)


El bloqueo de un inicio de sesión:

* Evita la generación de nuevos eventos de riesgo del usuario para el usuario afectado.
* Permite a los administradores corregir manualmente los eventos de riesgo que afectan a la identidad del usuario y restaurarla a un estado seguro.


























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

 [Channel 9: Azure AD and Identity Show: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

