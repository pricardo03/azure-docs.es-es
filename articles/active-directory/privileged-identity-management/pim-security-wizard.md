---
title: 'Asistente para seguridad de PIM: Azure | Microsoft Docs'
description: Describe el asistente para seguridad que aparece la primera vez que usa Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 178a4c5e978075f2a59b22a1cccf462138527964
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189086"
---
# <a name="security-wizard-in-pim"></a>Asistente para seguridad de PIM
Si es la primera persona que ejecuta Privileged Identity Management (PIM) de Azure en su organización, se le presentará un asistente. El asistente le ayuda a comprender los riesgos de seguridad de las identidades con privilegios y a usar PIM para reducirlos. No tiene que realizar cambios en las asignaciones de roles existentes en el asistente; si lo prefiere, puede hacerlo más adelante.

## <a name="what-to-expect"></a>Qué esperar
Antes de que su organización comienza a usar PIM, todas las asignaciones de roles son permanentes: los usuarios siempre tienen estos roles aunque en el momento actual no necesiten sus privilegios.  El primer paso del asistente muestra una lista de roles con privilegios elevados y cuántos usuarios tienen actualmente esos roles. Puede explorar en profundidad un rol determinado para conocer mejor a los usuarios en caso de que alguno de ellos no le resulte familiar.

El segundo paso del asistente le ofrece la oportunidad de cambiar las asignaciones de roles del administrador.  

> [!WARNING]
> Es importante que tenga al menos un administrador global, y más de un administrador de roles con privilegios con una cuenta de organización (no una cuenta Microsoft). Si solo hay un administrador de roles con privilegios, la organización no podrá administrar PIM si esa cuenta se elimina.
> Además, mantenga las asignaciones de roles permanente si un usuario tiene una cuenta de Microsoft (una cuenta que usan para iniciar sesión en servicios de Microsoft como Skype y Outlook.com). Si tiene pensado exigir MFA para la activación de ese rol, ese usuario se bloqueará.
> 
> 

Una vez realizados los cambios, ya no se mostrará el asistente. La próxima vez que usted u otro administrador de roles con privilegios use PIM, verá el panel de PIM.  

* Si desea agregar o quitar usuarios a roles, o cambiar las asignaciones de permanentes a aptas, puede obtener más información en [Privileged Identity Management de Azure AD: Incorporación o eliminación de un rol de usuario](pim-how-to-add-role-to-user.md).
* Si desea conceder acceso a más usuarios para administrar PIM, aprenda cómo hacerlo en [Concesión de acceso para administrar Privileged Identity Management de Azure AD](pim-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Pasos siguientes

- [Primer uso de PIM](pim-getting-started.md)
- [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md)
- [Concesión de acceso a otros administradores para administrar PIM](pim-how-to-give-access-to-pim.md)
