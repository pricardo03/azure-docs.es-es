---
title: 'Gobierno de identidades: Azure Active Directory | Microsoft Docs'
description: Azure gobierno de identidades de Active Directory le permite equilibrar la necesidad de su organización para la productividad del empleado y la seguridad con los procesos correctos y visibilidad.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 09/25/2018
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4e2f564de254c2aea779b30286183d0a4e17db6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60350990"
---
# <a name="what-is-azure-ad-identity-governance"></a>¿Qué es la gobernanza de identidades de Azure AD?

La gobernanza de identidades de Azure Active Directory (Azure AD) le permite equilibrar la productividad de los empleados y la seguridad que necesita su organización con la visibilidad y los procesos adecuados. Dispone de funcionalidades que garantizan que los usuarios apropiados tienen el acceso adecuado a los recursos idóneos y le permiten proteger, supervisar y auditar el acceso a los recursos críticos, al mismo tiempo que garantiza la productividad de los empleados.  

La gobernanza de identidades permite a las organizaciones realizar las siguientes operaciones con empleados, asociados, proveedores, servicios y aplicaciones:

- Controlar el ciclo de vida de las identidades.
- Controlar el ciclo de vida de los accesos.
- Proteger las tareas de administración.

Está especialmente diseñado para ayudar a las organizaciones a abordar estas cuatro preguntas claves:

- ¿Qué usuarios deben tener acceso a qué recursos?
- ¿Qué hacen esos usuarios con el acceso concedido?
- ¿La organización cuenta con controles eficaces para administrar el acceso?
- ¿Los auditores pueden comprobar qué controles funcionan?

## <a name="identity-lifecycle"></a>Ciclo de vida de las identidades

La gobernanza de identidades ayuda a las organizaciones a alcanzar un equilibrio entre *productividad* (con qué rapidez puede obtener acceso una persona a los recursos que necesita; por ejemplo, cuando se une a una organización) y *seguridad* (cómo debe cambiar el acceso de esa persona a lo largo del tiempo; por ejemplo, cuando varía su estado laboral).  La administración del ciclo de vida de las identidades es la piedra angular de la gobernanza de identidades y, para que esa gobernanza resulte eficaz a diferentes escalas, es preciso modernizar la infraestructura de administración del ciclo de vida de las identidades en las aplicaciones.

En muchas organizaciones, el ciclo de vida de las identidades de los empleados está relacionado con la representación del usuario en un sistema HCM (administración del capital humano).  Azure AD Premium mantiene automáticamente las identidades de usuario de las personas representadas en Workday tanto en Active Directory como en Azure Active Directory, tal y como se describe en el [tutorial sobre el aprovisionamiento de entrada en Workday (versión preliminar)](../saas-apps/workday-inbound-tutorial.md).  Azure AD Premium incluye también [Microsoft Identity Manager](/microsoft-identity-manager/), que permite importar registros desde los sistemas HCM locales, como SAP, Oracle eBusiness y Oracle PeopleSoft.

Cada vez son más los escenarios en los que es preciso colaborar con personas que están fuera de la organización. La [colaboración B2B de Azure AD](/azure/active-directory/b2b/) permite compartir de forma segura las aplicaciones y servicios corporativos con usuarios invitados y asociados externos de cualquier organización, a la vez que mantiene el control sobre sus propios datos corporativos.

## <a name="access-lifecycle"></a>Ciclo de vida de los accesos

Las organizaciones necesitan un proceso que administre el acceso sin limitarse a lo que se aprovisionó inicialmente, cuando se creó la identidad del usuario.  Además, las organizaciones empresariales deben ser capaces de escalar sus recursos con eficacia para poder desarrollar y aplicar de forma continuada directivas y controles de acceso.

Normalmente, el departamento de TI delega las decisiones sobre la aprobación de los accesos en los responsables de la toma de decisiones de la empresa.  Por otro lado, el departamento de TI puede involucrar él mismo a los usuarios.  Por ejemplo, los usuarios que tienen acceso a los datos confidenciales de los clientes en una aplicación de marketing de una compañía en Europa tienen que conocer las directivas de la compañía. Es posible que los usuarios invitados no conozcan los requisitos de administración de los datos de una organización a la que han sido invitados.

Las organizaciones pueden automatizar el proceso del ciclo de vida de los accesos utilizando determinadas tecnologías, como los [grupos dinámicos](../users-groups-roles/groups-dynamic-membership.md), junto con el aprovisionamiento de usuarios en [aplicaciones SaaS](../saas-apps/tutorial-list.md) o [aplicaciones integradas con SCIM](../manage-apps/use-scim-to-provision-users-and-groups.md).  Las organizaciones también pueden controlar qué [usuarios invitados tienen acceso a las aplicaciones locales](../b2b/hybrid-cloud-to-on-premises.md).  Estos derechos de acceso se pueden revisar periódicamente utilizando [revisiones de acceso de Azure AD](access-reviews-overview.md) recurrentes.

Cuando un usuario intenta acceder a las aplicaciones, Azure AD impone directivas de [acceso condicional](/azure/active-directory/conditional-access/). Por ejemplo, las directivas de acceso condicional pueden incluir mostrando un [términos de uso](../conditional-access/terms-of-use.md) y [garantizar el usuario ha aceptado los términos](../conditional-access/require-tou.md) antes de poder acceder a una aplicación.

## <a name="privileged-access-lifecycle"></a>Ciclo de vida de los accesos con privilegios

Históricamente, otros proveedores concebían el acceso con privilegios como una funcionalidad independiente de la gobernanza de identidades. Sin embargo, en Microsoft, creemos que el control del acceso con privilegios constituye una parte fundamental de la gobernanza de identidades, especialmente si tenemos en cuenta las consecuencias que podría tener para la organización un uso indebido con los derechos de administrador. Es preciso controlar a los empleados, los proveedores y los contratistas que tienen derechos administrativos.

Azure AD Privileged Identity Management (PIM) dispone de controles adicionales que están adaptados para proteger los derechos de acceso de los recursos en Azure AD, Azure y otros servicios en línea de Microsoft.  El acceso Just-In-Time y las funcionalidades para enviar alertas cuando cambia un rol que están disponibles en Azure AD PIM, junto con la autenticación multifactor y el acceso condicional, ofrecen un completo conjunto de controles de gobernanza que le ayudan a proteger los recursos de la compañía (roles de recursos de directorio, de Azure y de Office 365). Al igual que con otras formas de acceso, las organizaciones pueden usar las revisiones de acceso para configurar nuevas certificaciones de acceso periódicas para todos los usuarios con roles de administrador.

## <a name="getting-started"></a>Introducción

Aunque no existe ninguna solución o recomendación perfecta para cada cliente, las configuraciones siguientes constituyen una guía de las directivas de referencia que Microsoft recomienda para garantizar unos recursos más seguros y productivos.

- [Configuraciones de acceso de dispositivos e identidades](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Protección del acceso con privilegios](../users-groups-roles/directory-admin-roles-secure.md)


### <a name="access-reviews"></a>Revisiones de acceso

- [¿Qué es una revisión de acceso?](access-reviews-overview.md)
- [Administración del acceso de los usuarios con las revisiones de acceso](manage-user-access-with-access-reviews.md)
- [Administración del acceso de los invitados con las revisiones de acceso](manage-guest-access-with-access-reviews.md)
- [Iniciar una revisión de acceso de roles de Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md)

### <a name="terms-of-use"></a>Términos de uso

- [¿Qué puedo hacer con los Términos de uso?](../conditional-access/terms-of-use.md)

### <a name="privileged-identity-management"></a>Privileged Identity Management

- [¿Qué es Azure AD PIM?](../privileged-identity-management/pim-configure.md)
