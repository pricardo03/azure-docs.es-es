---
title: ¿Cuáles son los aspectos básicos de la administración de identidad y acceso de Azure? Azure Active Directory | Microsoft Docs
description: Obtenga información sobre las capacidades de protección avanzada y las herramientas adicionales que están disponibles con las ediciones de Azure Active Directory Premium.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/13/2018
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: f7baa29c77ae4af9813bfc755a39cc07288a3ad2
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734682"
---
# <a name="what-are-the-fundamentals-of-azure-identity-and-access-management"></a>¿Cuáles son los aspectos básicos de la administración de identidad y acceso de Azure?
Azure AD Premium es una solución de administración de identidad y acceso basada en la nube con capacidades de protección avanzada. Estas capacidades avanzadas ayudan a proporcionar una identidad segura para todas las aplicaciones, protección de identidad (mejorada con el [gráfico de inteligencia de seguridad de Microsoft](https://www.microsoft.com/security/intelligence)), y [Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md). Azure AD ayuda a proteger las identidades de los usuarios en tiempo real para crear directivas de acceso basadas en los riesgos y adaptables en torno a los datos de su organización.

En este breve vídeo verá una introducción rápida a la protección y administración de identidades de Azure AD:
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

Azure AD también proporciona un conjunto de herramientas que pueden ayudarle a proteger, automatizar y administrar su entorno, incluido el restablecimiento de contraseñas, la administración de usuarios y grupos y las solicitudes de aplicación. Azure AD también puede ayudarle a administrar tanto los dispositivos propiedad del usuario como el acceso y control de aplicaciones de Software como servicio (SaaS).

Para obtener más información sobre los costos de las ediciones de Azure Active Directory Premium y las herramientas asociadas, consulte los [precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Conexión de una instancia local de Active Directory a Azure AD y Office 365
Amplíe su implementación local de Active Directory en la nube al integrar sus directorios locales con Azure AD a través de la [administración de identidades híbridas](https://aka.ms/aadframework). [Azure AD Connect](../connect/active-directory-aadconnect.md) proporciona esta integración, para que sus usuarios cuenten con una sola identidad y un acceso de inicio de sesión único (SSO) tanto a sus recursos locales como a sus servicios en la nube, como Office 365.

Azure AD Connect reemplaza a las versiones anteriores de las herramientas de integración de identidades (como DirSync y los Servicios de sincronización de Microsoft Azure Active Directory) para volverse compatible con sus necesidades de sincronización de identidad entre entornos locales y Azure AD. La sincronización de Azure AD Connect se habilita a través de:

- **Sincronización.** Responsable de la creación de usuarios, grupos y otros objetos. También es responsable de asegurarse de que la información de identidad de los grupos de su entorno local coincide con la de Azure AD. Activar la escritura diferida de contraseñas también ayuda a mantener sus directorios locales sincronizados cuando los usuarios actualizan sus contraseñas en Azure AD.

- Autenticación La elección del método de autenticación correcto es fundamental en la configuración de una solución de identidad híbrida de Azure AD. Puede elegir entre la autenticación en la nube (sincronización de hash de contraseñas o autenticación de paso a través), o la autenticación federada (AD FS) para su organización. Para obtener más información sobre las opciones disponibles, vea [Elegir el método de autenticación adecuado para su solución de identidad híbrida de Azure Active Directory](https://aka.ms/auth-options).

- **Supervisión del estado.** Azure AD Connect Health proporciona supervisión y una ubicación central en Azure Portal para ver esta actividad. Para más información, consulte [Supervisión de la infraestructura de identidad local y los servicios de sincronización en la nube](../connect-health/active-directory-aadconnect-health.md).

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Aumento de la productividad y reducción de los costos del departamento de soporte técnico gracias al autoservicio y al inicio de sesión único
Los usuarios ahorran tiempo cuando tienen un único nombre de usuario y una contraseña, además de una experiencia coherente en todos los dispositivos. También ahorran tiempo al realizar ellos mismos tareas como [restablecer una contraseña olvidada](../user-help/active-directory-passwords-update-your-own-password.md) o solicitar acceso a una aplicación sin tener que depender de la ayuda del departamento de soporte técnico.

Para ampliar el SSO y la experiencia coherente, Azure AD [extiende el entorno local de Active Directory](../connect/active-directory-aadconnect.md) a la nube, lo que permite que los usuarios empleen su cuenta organizativa principal para los dispositivos personales unidos a un dominio, los recursos de la empresa y para todas las aplicaciones web y SaaS que necesitan usar para realizar su trabajo. 

Además, el aprovisionamiento del acceso a la aplicación se puede realizar (o anular ) automáticamente según las pertenencias a grupos y el estado de empleado de un usuario, ayudando a controlar el acceso a las aplicaciones de la galería o a las aplicaciones locales que ha desarrollado y publicado a través de [Azure AD Application Proxy](../manage-apps/application-proxy.md).

## <a name="manage-and-control-access-to-your-organizational-resources"></a>Administración y control del acceso a los recursos de la organización
Las soluciones de administración de identidad y acceso de Microsoft ayudan a proteger el acceso a las aplicaciones y a los recursos tanto en el centro de datos de la organización como en la nube. Esta administración del acceso ayuda a proporcionar niveles adicionales de validación, como [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) y las [directivas de acceso condicional](../conditional-access/overview.md). La supervisión de actividades sospechosas mediante auditorías, alertas e informes de seguridad avanzados también puede ayudar a minimizar los posibles problemas de seguridad.

Utilizar las directivas de acceso condicional de Azure AD Premium le permite crear reglas de acceso basada en directivas para todas las aplicaciones conectadas a AD Azure (aplicaciones SaaS, aplicaciones personalizadas que se ejecutan en la nube o de forma local y aplicaciones web). Azure AD evalúa sus reglas en tiempo real y las aplica cada vez que un usuario intenta acceder a una aplicación. Las directivas de protección de identidad de Azure le permiten realizar automáticamente una acción (bloquear el acceso, forzar la autenticación multifactor o reestablecer las contraseñas de usuario) si se detecta actividad sospechosa.

## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Privileged Identity Management
[Privileged Identity Management (PIM)](../privileged-identity-management/pim-getting-started.md), que se incluye en la segunda edición de Azure Active Directory Premium, ayuda a detectar, restringir y administrar cuentas administrativas y su acceso a los recursos de Azure Active Directory y a otros servicios en línea de Microsoft. PIM también le ayuda a administrar el acceso administrativo a petición durante el período de tiempo que necesita, lo que significa que puede permitir que los administradores soliciten un aumento temporal y autenticado por multifactor de sus privilegios durante un período de tiempo exacto configurado previamente antes de que sus cuentas vuelvan a un estado de usuario normal.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información acerca de la arquitectura de Azure AD, consulte [¿qué es la arquitectura de Azure AD?](active-directory-architecture.md).
