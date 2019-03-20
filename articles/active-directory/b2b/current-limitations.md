---
title: Limitaciones de colaboración de B2B - Azure Active Directory | Microsoft Docs
description: Limitaciones actuales de la colaboración B2B de Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7bf4d1d0f510ccad614452db74c291f6c61dcf54
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672311"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Limitaciones de colaboración B2B de Azure AD
La colaboración B2B de Active Directory (Azure AD) de Azure está sujeta actualmente a las limitaciones descritas en este artículo.

## <a name="possible-double-multi-factor-authentication"></a>Posibilidad de doble autenticación multifactor
Con B2B de Azure AD, puede aplicar la autenticación multifactor en la organización del recurso (la organización que invita). Los motivos de este enfoque se detallan en [Acceso condicional para usuarios de colaboración B2B](conditional-access.md). Si un asociado ya tiene instalada la autenticación multifactor y la exige, los usuarios del asociado podrían tener que realizar la autenticación una vez en su organización principal y otra vez en la suya.

## <a name="instant-on"></a>Activación instantánea
En los flujos de colaboración B2B, hemos agregado usuarios al directorio y los hemos actualizado dinámicamente durante el canje de invitación, la asignación de aplicaciones y así sucesivamente. Las actualizaciones y escrituras se producen normalmente en una instancia de directorio y se deben replicar en todas las instancias. La replicación se completa una vez que se actualicen todas las instancias. En ocasiones, cuando el objeto se escribe o se actualiza en una instancia y la llamada para recuperar este objeto es a otra instancia, pueden producirse latencias en la replicación. Si esto sucede, actualice o vuelva a intentarlo. Si está escribiendo una aplicación mediante la API, los reintentos con algún retroceso es una buena práctica defensiva para mitigar este problema.

## <a name="azure-ad-directories"></a>Directorios de Azure AD
Azure AD B2B está sujeto a los límites de directorio del servicio Azure AD. Para obtener más información sobre el número de directorios que puede crear un usuario y el número de directorios al que puede pertenecer un usuario o usuario invitado, consulte [Restricciones y límites del servicio Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions).

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos sobre la colaboración de B2B de Azure AD:

- [¿Qué es la colaboración B2B de Azure AD?](what-is-b2b.md)
- [Delegación de las invitaciones de colaboración B2B](delegate-invitations.md)

