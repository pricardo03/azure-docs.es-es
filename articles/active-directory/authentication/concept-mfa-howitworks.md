---
title: 'Azure Multi-factor Authentication - cómo funciona: Azure Active Directory'
description: Azure Multi-Factor Authentication ayuda a proteger el acceso a los datos y las aplicaciones, además de satisfacer la demanda de los usuarios de un proceso de inicio de sesión simple.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7328fb958774b5e17511d046e914cc5612e8a96d
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58310938"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Funcionamiento: Azure Multi-Factor Authentication

La seguridad de la comprobación en pos pasos se basa en el enfoque por niveles. El uso de varias fases de autenticación supone un reto importante para los atacantes. Aunque un atacante consiga descifrar la contraseña de usuario, no servirá de nada si no dispone también del método de autenticación adicional. Funciona mediante la solicitud de dos, o más, de los siguientes métodos de autenticación:

* Un elemento que conoce (normalmente una contraseña).
* Un elemento del que dispone (un dispositivo de confianza que no se puede duplicar con facilidad, como un teléfono).
* Un elemento físico que le identifica (biométrica).

<center>

![Imagen de los métodos de autenticación conceptual](./media/concept-mfa-howitworks/methods.png)</center>

Azure Multi-Factor Authentication (MFA) ayuda a proteger el acceso a los datos y aplicaciones, y al mismo tiempo mantiene la simplicidad para los usuarios. Proporciona más seguridad, ya que requiere una segunda forma de autenticación y ofrece autenticación segura a través de una gama de [método de autenticación](concept-authentication-methods.md) fáciles de usar. Puede que los usuarios se vean desafiados por la MFA en función de las decisiones de configuración que tome un administrador.

## <a name="how-to-get-multi-factor-authentication"></a>¿Cómo se consigue Multi-Factor Authentication?

Multi-Factor Authentication forma parte de las siguientes ofertas:

* **Licencias de Azure Active Directory Premium**: uso completo del servicio Azure Multi-Factor Authentication (nube) o del servidor Azure Multi-Factor Authentication (local).
   * **Servicio Azure MFA (nube)** - **Esta opción es la ruta de acceso recomendada para las implementaciones nuevas**. Azure MFA en la nube no requiere ninguna infraestructura local y se puede utilizar tanto con los usuarios federados como los que están solo en la nube.
   * **Servidor de Azure MFA**: si la organización desea administrar los elementos de la infraestructura asociada y ha implementado AD FS en el entorno local de este modo puede ser una opción.
* **Multi-Factor Authentication para Office 365**: un subconjunto de las funcionalidades de Azure Multi-Factor Authentication están disponibles como parte de la suscripción. Para más información acerca de MFA para Office 365, consulte el artículo [Plan de Multi-Factor Authentication para implementaciones de Office 365](https://support.office.com/article/plan-for-multi-factor-authentication-for-office-365-deployments-043807b2-21db-4d5c-b430-c8a6dee0e6ba).
* **Administradores globales de Azure Active Directory**: un subconjunto de las funcionalidades de capacidades de Azure Multi-Factor Authentication está disponible como medio de protección de las cuentas de administrador global.

> [!NOTE]
> A partir del 1 de septiembre de 2018, los nuevos clientes ya no pueden comprar Azure Multi-factor Authentication como oferta independiente. La autenticación multifactor seguirá siendo una característica disponible en las licencias de Azure AD Premium.

## <a name="supportability"></a>Compatibilidad

Dado que la mayoría de los usuarios están acostumbrados a usar solo las contraseñas para realizar la autenticación, es importante que la organización comunique el proceso a todos los usuarios. Esto reducir la probabilidad de que los usuarios llamen al departamento de soporte técnico para problemas poco importantes relacionados con MFA. Sin embargo, hay algunos escenarios en los que es necesario deshabilitar temporalmente MFA. Use las instrucciones siguientes para entender cómo administrar estos escenarios:

* Entrene al personal de soporte técnico para administrar escenarios en los que el usuario no puede iniciar sesión porque no tiene acceso a sus métodos de autenticación o porque estos no funcionan correctamente.
   * Mediante las directivas de acceso condicional del servicio Azure MFA, el personal de soporte técnico puede agregar un usuario a un grupo que se excluye de una directiva que requiere MFA.
   * El personal de soporte técnico puede habilitar una omisión por única vez temporal para los usuarios del servidor de Azure MFA que permita que cualquier usuario se autentique sin la verificación en dos pasos. La omisión es temporal y expira una vez que ha pasado el número especificado de segundos.   
* Considere el uso de direcciones IP de confianza o de ubicaciones con nombre como una forma de minimizar los mensajes de la verificación en dos pasos. Esta característica ofrece a los administradores de un inquilino administrado o federado la posibilidad de omitir la verificación en dos pasos para los usuarios que inician sesión desde una ubicación de red de confianza como la intranet de su organización.
* Implemente [Azure AD Identity Protection](../active-directory-identityprotection.md) y desencadene la verificación en dos pasos en función de los eventos de riesgo.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga un [plan de implementación](https://aka.ms/MFADeploymentPlan) de MFA paso a paso

- Obtenga detalles acerca de [cómo otorgar licencias a los usuarios](concept-mfa-licensing.md)

- Obtenga información acerca de la [versión que se debe implementar](concept-mfa-whichversion.md)

- Obtenga respuestas a las [preguntas más frecuentes](multi-factor-authentication-faq.md)
