---
title: 'Requisitos de licencia para usar PIM: Azure | Microsoft Docs'
description: Describe los requisitos de licencia necesarios para usar Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/16/2019
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: ad64fba0288aa5663e008484d6f0d2cbdff0bca6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187297"
---
# <a name="license-requirements-to-use-pim"></a>Requisitos de licencia para usar PIM

Para Azure Active Directory (Azure AD) Privileged Identity Management (PIM), un directorio debe tener una licencia válida. Además, deben asignarse licencias a los administradores y usuarios pertinentes. En este artículo se describen los requisitos de licencia para usar PIM.

## <a name="prerequisites"></a>Requisitos previos

Para usar PIM, el directorio debe tener una de las siguientes licencias de pago o de evaluación:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Para más información, consulte [¿Qué es Azure Active Directory?](../fundamentals/active-directory-whatis.md)

## <a name="which-users-must-have-licenses"></a>¿Qué usuarios deben tener licencias?

Cada administrador o usuario que interactúa con, o recibe un beneficio de PIM, debe tener una licencia. Algunos ejemplos son:

- Los administradores con roles de Azure AD administrados mediante PIM
- Los administradores con roles de recurso de Azure AD administrados mediante PIM
- Los administradores asignados al rol de administrador de roles con privilegios
- Los usuarios asignados como aptos para roles de directorio que se pueden administrar con PIM
- Los usuarios que pueden aprobar o rechazar solicitudes en PIM
- Los usuarios asignados a un rol de recursos de Azure con asignaciones Just-In-Time o directas (basadas en tiempo)  
- Los usuarios asignados a una revisión de acceso
- Los usuarios que realizan revisiones de acceso

Para más información acerca de cómo asignar licencias a los usuarios, consulte [Asignación o eliminación de licencias mediante el portal de Azure Active Directory](../fundamentals/license-users-groups.md).

## <a name="what-happens-when-a-license-expires"></a>¿Qué sucede cuando expira una licencia?

Si expira una licencia de Azure AD Premium P2, EMS E5 o una licencia de evaluación, las características de PIM ya no estarán disponibles en el directorio:

- Las asignaciones de roles permanentes para roles de Azure AD no se verán afectadas.
- El servicio PIM de Azure Portal, así como los cmdlets de Graph API y las interfaces de PowerShell de PIM, ya no estarán disponibles para que los usuarios activen roles con privilegios, administren el acceso con privilegios o realicen revisiones de acceso de roles con privilegios.
- Se quitarán las asignaciones de roles elegibles de roles de Azure AD, puesto que los usuarios ya no podrán activar roles con privilegios.
- Las revisiones de acceso en curso de roles de Azure AD finalizarán y las opciones de configuración de PIM se eliminarán.
- PIM no volverá a enviar mensajes de correo electrónico en los cambios de asignación de roles.

## <a name="next-steps"></a>Pasos siguientes

- [Primer uso de PIM](pim-getting-started.md)
- [Roles que no puede administrar en PIM](pim-roles.md)
