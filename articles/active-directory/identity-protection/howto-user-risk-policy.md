---
title: Configuración de la directiva de riesgo de usuario en Azure Active Directory Identity Protection | Microsoft Docs
description: Obtenga información sobre cómo configurar la directiva de riesgo de usuario de Azure AD Identity Protection.
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
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: raluthra
ms.collection: M365-identity-device-management
ms.openlocfilehash: 409c86556d8132cb913508518be7d5b9f9d35321
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56180205"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Instrucciones: Configuración de la directiva de riesgo del usuario

Con el riesgo de usuario, Azure AD detecta la probabilidad de que una cuenta de usuario esté en peligro. Como administrador, puede configurar una directiva de acceso condicional de riesgo de usuario para responder automáticamente a un nivel de riesgo de usuario específico.
 
En este artículo se proporciona la información necesaria para configurar la directiva de riesgo de usuario.


## <a name="what-is-a-user-risk-policy"></a>¿Qué es una directiva de riesgo de usuario?

AD Azure analiza cada inicio de sesión de un usuario. El objetivo del análisis es detectar acciones sospechosas que se realicen con el inicio de sesión. En Azure AD, las acciones sospechosas que el sistema puede detectar también se conocen como eventos de riesgo. Aunque algunos eventos de riesgo se pueden detectar en tiempo real, también hay eventos de riesgo que requieren más tiempo. Por ejemplo, para detectar un viaje imposible a ubicaciones inusuales, el sistema requiere un período de aprendizaje inicial de 14 días para conocer el comportamiento normal de un usuario. Hay varias opciones para resolver los eventos de riesgo detectados. Por ejemplo, puede resolver los eventos de riesgo individuales manualmente, o puede resolverlos con una directiva de acceso condicional de riesgo de inicio de sesión o de riesgo de usuario.

Todos los eventos de riesgo que se han detectado para un usuario y no se resuelven, se conocen como eventos de riesgo activos. Los eventos de riesgo activos que están asociados con un usuario se conocen como riesgos de usuario. Según el riesgo de usuario, Azure AD calcula una probabilidad (baja, media, alta) de que un usuario esté en peligro. La probabilidad se denomina nivel de riesgo de usuario.

![Riesgos de usuario](./media/howto-user-risk-policy/1031.png)

La directiva de riesgo de usuario es una respuesta automatizada que puede configurar para un nivel de riesgo de usuario específico. Con una directiva de riesgo de usuario, puede bloquear el acceso a los recursos o requerir un cambio de contraseña para que la cuenta de usuario vuelva a estar limpia.


## <a name="how-do-i-access-the-user-risk-policy"></a>¿Cómo puedo acceder a la directiva de riesgo del usuario?
   
En la hoja **Azure AD Identity Protection**, en la sección [Configurar](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy), está la directiva de riesgo de inicio de sesión.
   
![Directiva de riesgo de usuario](./media/howto-user-risk-policy/1014.png)



## <a name="policy-settings"></a>Configuración de directiva

Al configurar la directiva de riesgo de inicio de sesión, deberá establecer:

- Usuarios y grupos a los que se aplica la directiva:

    ![Usuarios y grupos](./media/howto-user-risk-policy/11.png)

- El nivel de riesgo de inicio de sesión que activa la directiva:

    ![Nivel de riesgo del usuario](./media/howto-user-risk-policy/12.png)

- El tipo de acceso que desea aplicar cuando se alcance el nivel de riesgo de inicio de sesión:  

    ![Access](./media/howto-user-risk-policy/13.png)

- Estado de la directiva:

    ![Aplicar directiva](./media/howto-user-risk-policy/14.png)

El cuadro de diálogo de configuración de directivas proporciona una opción para calcular el impacto de la configuración.

![Impacto estimado](./media/howto-user-risk-policy/15.png)

## <a name="what-you-should-know"></a>Qué debería saber

Puede establecer una directiva de seguridad de riesgo de usuario para impedir que los usuarios inicien sesión en función del nivel de riesgo.

![Bloqueo](./media/howto-user-risk-policy/16.png)


El bloqueo de un inicio de sesión:

* Evita la generación de nuevos eventos de riesgo del usuario para el usuario afectado.
* Permite a los administradores corregir manualmente los eventos de riesgo que afectan a la identidad del usuario y restaurarla a un estado seguro.

## <a name="best-practices"></a>Procedimientos recomendados

Elegir un umbral **Alto** reduce el número de veces que una directiva se desencadena y minimiza el impacto en los usuarios.
Sin embargo, excluye de la directiva a los usuarios marcados en riesgo **Bajo** y **Medio**, por lo que es posible que no se protejan las identidades o los dispositivos bajo sospecha o con certeza de haber estado en peligro.

Al establecer la directiva:

* Excluya a los usuarios que es probable que generen una gran cantidad de falsos positivos (desarrolladores, analistas de seguridad).
* Excluya a los usuarios que tengan configuraciones regionales en las que no resulte práctico habilitar la directiva (por ejemplo, no tienen acceso al departamento de soporte técnico).
* Utilice un umbral **Alto** durante la puesta en servicio inicial de la directiva, o bien si debe minimizar los desafíos que ven los usuarios finales.
* Utilice un umbral **Bajo** si su organización requiere un mayor grado de seguridad. Seleccionar un umbral **Bajo** presenta desafíos adicionales de inicio de sesión del usuario, pero aumenta la seguridad.

El valor predeterminado recomendado para la mayoría de las organizaciones es configurar una regla para un umbral **Medio** con el fin de lograr un equilibrio entre la facilidad de uso y la seguridad.

Para obtener información general de la experiencia de usuario relacionada, consulte:

* [Flujo de recuperación de cuentas en peligro](flows.md#compromised-account-recovery).  
* [Flujo de cuentas en peligro bloqueadas](flows.md#compromised-account-blocked).  

**Para abrir el cuadro de diálogo de configuración relacionado, siga estos pasos**:

- En la hoja **Azure AD Identity Protection**, en la sección **Configurar**, haga clic en **Directiva de riesgo de usuario**.

    ![Directiva de riesgo de usuario](./media/howto-user-risk-policy/1009.png "Directiva de riesgo de usuario")




## <a name="next-steps"></a>Pasos siguientes

Para obtener una visión general de Azure AD Identity Protection, consulte la [Introducción a Azure AD Identity Protection](overview.md).
