---
title: Guía de concesión de licencias de colaboración B2B de Azure Active Directory | Microsoft Docs
description: La colaboración B2B de Azure Active Directory no necesita licencias de Azure AD de pago, pero también puede obtener características de pago para los usuarios invitados de B2B
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.openlocfilehash: d80794511f334cd6dc5af418e24fc774b7d8728f
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867517"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Guía de concesión de licencias de colaboración B2B de Azure Active Directory

Con la colaboración negocio a negocio (B2B) de Azure Active Directory (Azure AD), puede invitar a usuarios externos (o "usuarios invitados") a usar sus servicios de Azure AD de pago. Por cada licencia de Azure AD de pago que asigne a un usuario, puede invitar hasta a cinco usuarios invitados con arreglo a la concesión de usuarios externos.

Un usuario invitado es alguien que no es miembro de su organización ni de ninguna de sus filiales. Los usuarios invitados se definen por la relación que guardan con la organización, no por las credenciales que usan para iniciar sesión. De hecho, un usuario invitado puede iniciar sesión con una identidad externa o con credenciales propiedad de su organización.

Los siguientes *no* son usuarios invitados:
- Empleados, contratistas in situ o agentes in situ
- Empleados, contratistas in situ o agentes in situ de sus filiales

La licencia de los usuarios invitados B2B se calcula automáticamente y se notifica según la relación 1:5. Actualmente, no es posible asignar licencias de usuario invitado B2B directamente a los usuarios invitados.

Existen algunas situaciones en las que el usuario invitado no se notifica mediante la concesión de usuarios externos 1:5. Si un usuario invitado ya tiene una licencia de Azure AD de pago en la propia organización del usuario, el usuario no consume una de las licencias de usuario invitado B2B. Además, los usuarios invitados pueden usar características de Azure AD gratuitas sin requisitos de licencia adicionales. Los usuarios invitados tienen acceso a características gratuitas de Azure AD, incluso sin una licencia de Azure AD de pago. 

## <a name="examples-calculating-guest-user-licenses"></a>Ejemplos: Cálculo de licencias de usuario invitado
Una vez que determine cuántos usuarios invitados necesitan acceso a los servicios de Azure AD de pago, asegúrese de tener suficientes licencias de pago de Azure AD para los usuarios invitados en la proporción 1:5 requerida. Estos son algunos ejemplos:

- Quiere invitar a 100 usuarios invitados a las aplicaciones o servicios de Azure AD, y quiere asignar a todos los usuarios invitados administración de acceso y aprovisionamiento. También quiere exigir MFA y acceso condicional para 50 de esos usuarios invitados. Para satisfacer esta combinación, necesitará 10 licencias de Azure AD Basic y 10 licencias de Azure AD Premium P1. Si tiene previsto usar características de Identity Protection con los usuarios invitados, necesitará licencias de Azure AD Premium P2 en la misma proporción 1:5 para incluir a los usuarios invitados.
- Quiere invitar a 60 usuarios y todos ellos necesitan MFA, por lo que debe tener al menos 12 licencias de Azure AD Premium P1. Tiene 10 empleados con licencias de Azure AD Premium P1, lo que permitiría hasta 50 usuarios invitados en la proporción de licencia 1:5. Necesitará comprar dos licencias Premium P1 adicionales para incluir 10 usuarios invitados adicionales.

## <a name="using-the-b2b-collaboration-api-to-invite-users-from-your-affiliates"></a>Uso de la API de colaboración B2B para invitar a usuarios de las filiales

Por definición, un usuario invitado B2B es un usuario externo al que invita a usar sus aplicaciones y servicios de Azure AD de pago. Un empleado, contratista in situ o agente in situ de su empresa o de una de las filiales no reunirá los requisitos para la colaboración B2B, aunque se usen características B2B. Estos son algunos ejemplos: 
- Quiere usar credenciales externas (por ejemplo, una identidad social) para invitar a un usuario que es un empleado de su organización. Este escenario no satisface los requisitos de licencia y no está permitido. Las credenciales externas no convierten a un empleado en usuario externo.  
- Quiere usar las API de B2B para invitar a un usuario de una de las filiales de su organización. Aunque en este escenario se usan API de B2B para invitar al usuario, no se considera colaboración B2B. No satisface los requisitos de licencia porque el usuario de una filial no es un usuario externo. 

En ambos escenarios, la mejor solución es usar la API de B2B para invitar a los usuarios como miembros (invitedUserType = Member) y asignar a cada uno una licencia de Azure AD. 

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes recursos sobre la colaboración B2B de Azure AD:

* [¿Qué es la colaboración B2B de Azure AD?](what-is-b2b.md)
* [Preguntas frecuentes sobre la colaboración B2B de Azure Active Directory (P+F)](faq.md)
