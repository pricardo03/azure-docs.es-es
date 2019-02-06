---
title: Solución de problemas de pertenencia dinámica para grupos| Microsoft Docs
description: Solución de problemas para la pertenencia dinámica para grupos en Azure AD.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: ab5a3b00d063dfdd42e67247bb2cdc37866d0164
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164136"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Solución de problemas relacionados con las pertenencias dinámicas para grupos

**He configurado una regla en un grupo pero las pertenencias no se actualizan en el grupo**.<br/>Compruebe los valores de atributos de usuario en la regla: ¿hay usuarios que cumplen la regla? Si todo es correcto, el grupo tardará un poco en rellenarse. Dependiendo del tamaño del inquilino, el grupo puede tardar hasta 24 horas en rellenarse por primera vez o después de un cambio de la regla.

**He configurado una regla, pero ahora se han quitado los miembros existentes de la regla**.<br/> Este es el comportamiento esperado. Los miembros existentes del grupo se quitan cuando una regla se habilita o se cambia. Los usuarios devueltos tras la evaluación de la regla se agregan como miembros al grupo.

**No veo los cambios en la pertenencia al instante cuando agrego o cambio una regla, ¿por qué pasa esto?**<br/>La evaluación de pertenencia dedicada se realiza periódicamente en un proceso asincrónico en segundo plano. La duración del proceso viene determinada por el número de usuarios del directorio y el tamaño del grupo creado como resultado de la regla. Normalmente, los directorios con un número pequeño de usuarios verán los cambios en la pertenencia al grupo en unos pocos minutos. Los directorios con un gran número de usuarios pueden tardar 30 minutos o más en completarse.

**Se detectó una error de procesamiento de la regla**<br/>En la tabla siguiente se enumeran los errores de reglas de pertenencia dinámica más habituales y se indica cómo corregirlos.

| Error del analizador de reglas | Uso con error | Uso corregido |
| --- | --- | --- |
| Error: no se admite el atributo. |(user.invalidProperty -eq "Value") |(user.department -eq "value")<br/><br/>Asegúrese de que el atributo se encuentra en la [lista de propiedades admitidas](groups-dynamic-membership.md#supported-properties). |
| Error: no se admite el operador en el atributo. |(user.accountEnabled -contains true) |(user.accountEnabled -eq true)<br/><br/>El operador usado no se admite para el tipo de propiedad (en este ejemplo, -contains no se puede usar con el tipo boolean). Utilice los operadores correctos para el tipo de propiedad. |
| Error: error de compilación de consulta. | 1. (user.department -eq "Sales") (user.department -eq "Marketing")<br>2.  (user.userPrincipalName -match "*@domain.ext") | 1. Falta el operador. Use -and u -or para predicados de combinación<br>(user.department -eq "Sales") -or (user.department -eq "Marketing")<br>2. Error en la expresión regular usada con -match<br>(user.userPrincipalName -match ".*@domain.ext")<br>o bien (user.userPrincipalName -match "@domain.ext$") |

## <a name="next-steps"></a>Pasos siguientes

Estos artículos proporcionan información adicional sobre Azure Active Directory.

* [Administración del acceso a los recursos con grupos de Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Administración de aplicaciones en Azure Active Directory](../manage-apps/what-is-application-management.md)
* [¿Qué es Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integración de las identidades locales con Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
