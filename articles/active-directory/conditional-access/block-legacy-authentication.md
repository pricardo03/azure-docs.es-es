---
title: Bloqueo de la autenticación heredada a Azure Active Directory (Azure AD) con acceso condicional | Microsoft Docs
description: Obtenga información sobre cómo configurar una directiva de acceso condicional en Azure Active Directory (Azure AD) para intentos de acceso de redes que no son de confianza.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: conditional-access
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ddfea3ec7380a36f937052a6a994504ca081f187
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "53022205"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Control de Bloqueo de autenticación heredada a Azure AD con acceso condicional   

Para brindar a los usuarios un acceso sencillo a las aplicaciones en la nube, Azure Active Directory (Azure AD) admite una amplia variedad de protocolos de autenticación, incluida la autenticación heredada. Sin embargo, los protocolos heredados no admiten la autenticación multifactor (MFA). En muchos entornos, MFA es un requisito común para enfrentar el robo de identidad. 

Si el entorno está listo para bloquear la autenticación heredada con el fin de mejorar la protección del inquilino, puede lograr este objetivo con el acceso condicional. En este artículo se explica cómo configurar las directivas de acceso condicional que bloquean la autenticación heredada para el inquilino.



## <a name="prerequisites"></a>Requisitos previos

En este artículo se asume que está familiarizado con: 

- Los [conceptos básicos](overview.md) del acceso condicional de Azure AD 
- Los [procedimientos recomendados](best-practices.md) para configurar las directivas de acceso condicional en Azure Portal



## <a name="scenario-description"></a>Descripción del escenario

Azure AD admite varios de los protocolos de autenticación y autorización usados más comúnmente, incluida la autenticación heredada. La autenticación heredada hace referencia a los protocolos que usan la autenticación básica. Por lo general, estos protocolos no pueden aplicar ningún tipo de autenticación de segundo factor. Estos son algunos de los ejemplos de las aplicaciones basadas en la autenticación heredada:

- Aplicaciones anteriores de Microsoft Office

- Aplicaciones que usan protocolos de correo como POP, IMAP y SMTP

La autenticación de un solo factor (por ejemplo, nombre de usuario y contraseña) ya no es suficiente. No se recomiendan las contraseñas, porque son fáciles de adivinar y porque los usuarios humanos no suelen elegir contraseñas seguras. Las contraseñas también son vulnerables ante una variedad de ataques, como suplantación de identidad (phishing) y difusión de contraseña. Una de las medidas más sencillas que puede tomar para protegerse contra las amenazas para las contraseñas es implementar la autenticación multifactor. Con MFA, incluso si el atacante cuenta con la contraseña del usuario, la contraseña por sí sola no es suficiente para autenticarse de manera correcta y acceder a los datos.

¿Cómo se puede evitar que las aplicaciones que usan la autenticación heredada accedan a los recursos del inquilino? La recomendación es simplemente bloquearlas con una directiva de acceso condicional. Si es necesario, puede permitir que solo ciertos usuarios y ubicaciones de red específicas usen aplicaciones basadas en la autenticación heredada.




## <a name="implementation"></a>Implementación

En esta sección se explica cómo configurar una directiva de acceso condicional para bloquear la autenticación heredada. 

### <a name="block-legacy-authentication"></a>Bloquear la autenticación heredada 

En una directiva de acceso condicional, puede establecer una condición asociada a las aplicaciones cliente que se usan para acceder a los recursos. La condición de aplicaciones cliente le permite restringir el ámbito a las aplicaciones que usan la autenticación heredada mediante la selección de **Otros clientes** en **Aplicaciones móviles y aplicaciones de escritorio**.

![Otros clientes](./media/block-legacy-authentication/01.png)

Para bloquear el acceso a estas aplicaciones, debe seleccionar **Bloquear acceso**.

![Bloquear acceso](./media/block-legacy-authentication/02.png)


### <a name="select-users-and-cloud-apps"></a>Selección de usuarios y aplicaciones en la nube

Si quiere bloquear la autenticación heredada en su organización, probablemente piense que puede hacerlo si selecciona:

- Todos los usuarios

- Todas las aplicaciones en la nube

- Bloquear acceso
 

![Assignments](./media/block-legacy-authentication/03.png)



Azure incluye una característica de seguridad que le impide crear una directiva similar, porque esta configuración infringe los [procedimientos recomendados](best-practices.md) para las directivas de acceso condicional.
 
![Configuración de directiva no compatible](./media/block-legacy-authentication/04.png)


La característica de seguridad es necesaria porque el *bloqueo de todos los usuarios y todas las aplicaciones en la nube* tiene el potencial de impedir que toda la organización inicie sesión en el inquilino. Se debe excluir al menos un usuario para cumplir con el requisito mínimo del procedimiento recomendado. También puede usar 

![Configuración de directiva no compatible](./media/block-legacy-authentication/05.png)


Para satisfacer esta característica de seguridad, excluya un usuario de la directiva. Idealmente, debe definir algunas [cuentas administrativas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md) y excluirlas de la directiva.
 

## <a name="policy-deployment"></a>Implementación de la directiva

Antes de implementar la directiva en el entorno de producción, encárguese de lo siguiente:
 
- **Cuentas de servicio**: identifique las cuentas de usuario que se utilizan como cuentas de servicio o a través de dispositivos, como teléfonos en salas de conferencias. Asegúrese de que estas cuentas tienen contraseñas seguras y agréguelas a un grupo de exclusión.
 
- **Informes de inicio de sesión**: revise el informe de inicio de sesión y busque el tráfico de **otro cliente**. Identifique el uso principal e investigué por qué está en uso. Por lo general, el tráfico lo generan clientes de Office más antiguos que no usan la autenticación moderna, o bien algunas aplicaciones de correo de terceros. Elabore un plan para quitar el uso de estas aplicaciones, o bien, si el impacto es bajo, notifique a los usuarios que ya no pueden usarlas.
 
Para más información, consulte [¿Cómo debe implementar una directiva nueva?](best-practices.md#how-should-you-deploy-a-new-policy)



## <a name="what-you-should-know"></a>Qué debería saber

La configuración de una directiva para **otros clientes** bloquea toda la organización ante determinados clientes como SPConnect. Este bloqueo se produce porque clientes más antiguos se autentican de formas inesperadas. Este problema no aplica a las aplicaciones principales de Office, como los clientes de Office anteriores.

La directiva puede tardar hasta 24 horas en surtir efecto.

Puede seleccionar todos los controles de concesión disponibles para la condición de otros clientes, pero la experiencia del usuario final siempre es la misma: el acceso bloqueado.

Puede configurar todas las demás condiciones junto con la condición de otros clientes.




## <a name="next-steps"></a>Pasos siguientes

Si todavía no sabe cómo configurar las directivas de acceso condicional, consulte [Requerir MFA para aplicaciones específicas con acceso condicional a Azure Active Directory](app-based-mfa.md) para ver un ejemplo.
