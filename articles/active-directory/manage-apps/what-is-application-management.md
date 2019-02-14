---
title: Administración de aplicaciones con Azure Active Directory | Microsoft Docs
description: En este artículo se describen las ventajas de la integración de Azure Active Directory con las aplicaciones locales, SaaS y de nube.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: overview
ms.workload: identity
ms.date: 10/30/2018
ms.author: celested
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a65c42ff2d03946c021497e350836623a408ce7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56197528"
---
# <a name="application-management-with-azure-active-directory"></a>Administración de aplicaciones con Azure Active Directory

Azure Active Directory (Azure AD) proporciona un acceso seguro y sin problemas a aplicaciones locales y en la nube. Los usuarios pueden iniciar sesión una vez para acceder a Office 365 y a otras aplicaciones empresariales desde Microsoft, aplicaciones de software como servicio (SaaS), aplicaciones locales y aplicaciones de línea de negocio (LOB). Reduzca los costos administrativos gracias a la automatización del aprovisionamiento de usuarios. Utilice la autenticación multifactor y las directivas de acceso condicional para proporcionar un acceso seguro a las aplicaciones.

![Aplicaciones federadas a través de Azure AD](media/what-is-application-management/app-management-overview.png)

## <a name="why-manage-applications-with-a-cloud-solution"></a>¿Por qué administrar aplicaciones con una solución en la nube?

Las organizaciones suelen tener cientos de aplicaciones de las que los usuarios dependen para poder realizar su trabajo. Los usuarios acceden a estas aplicaciones desde muchos dispositivos y ubicaciones. Cada día se agregan, desarrollan y retiran nuevas aplicaciones. Con tantas aplicaciones y puntos de acceso, es más importante que nunca usar una solución basada en la nube para administrar el acceso de los usuarios a todas las aplicaciones.

## <a name="manage-risk-with-conditional-access-policies"></a>Administración de riesgos con directivas de acceso condicional
El acoplamiento del inicio de sesión único (SSO) de Azure AD con directivas de acceso condicional ofrece niveles altos de seguridad para acceder a las aplicaciones. Las funcionalidades de seguridad incluyen la protección de identidad en la nube, el control de acceso basado en riesgos, la autenticación multifactor nativa y las directivas de acceso condicional. Estas funcionalidades admiten directivas de control detalladas basadas en aplicaciones o en grupos que necesitan niveles más altos de seguridad.

## <a name="improve-productivity-with-single-sign-on"></a>Mejora de la seguridad con el inicio de sesión único
Habilitar el inicio de sesión único (SSO) en las aplicaciones y Office 365 ofrece una experiencia de inicio de sesión superior para los usuarios existentes con la reducción o eliminación de mensajes de inicio de sesión. El entorno del usuario será más coherente y menos confuso sin tantos mensajes o sin la necesidad de administrar varias contraseñas. El grupo de negocios puede administrar y aprobar el acceso mediante una pertenencia dinámica y de autoservicio. Permitir que las personas correctas de la empresa administren el acceso a una aplicación mejora la seguridad del sistema de identidades.

El inicio de sesión único mejora la seguridad. *Sin el inicio de sesión único*, los administradores necesitan crear y actualizar cuentas de usuario para cada aplicación individual, lo que requiere tiempo. Además, los usuarios deben realizar un seguimiento de varias credenciales para acceder a sus aplicaciones. Como resultado, los usuarios tienden a anotar las contraseñas o usar otras soluciones de administración de contraseñas que presentan riesgos para la seguridad de los datos. 

## <a name="address-governance-and-compliance"></a>Abordar el gobierno y el cumplimiento
Con Azure AD, puede supervisar los inicios de sesión en la aplicación mediante informes que usan herramientas de supervisión de eventos e incidentes de seguridad (SIEM). Puede acceder a los informes desde el portal o con las API. Audite mediante programación quién accede a sus aplicaciones y retire el acceso a usuarios inactivos mediante revisiones de acceso.

## <a name="manage-costs"></a>Administrar costos
Al migrar a Azure AD, puede ahorrar costos y las molestias de administrar la infraestructura local. Azure AD también proporciona acceso de autoservicio a las aplicaciones, lo que ahorra tiempo para los administradores y usuarios. El inicio de sesión único elimina las contraseñas específicas de aplicaciones. Esta posibilidad de iniciar sesión solo una vez ahorra costos relacionados con el restablecimiento de contraseñas para las aplicaciones y la pérdida de productividad al recuperar estas.

