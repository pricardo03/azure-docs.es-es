---
title: 'Asignaciones aptas y visibilidad de recursos en PIM: Azure | Microsoft Docs'
description: Describe cómo asignar miembros que sean aptos para los roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
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
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 3551c3231a94f8a844d26a713cbf171ca7653815
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189221"
---
# <a name="eligible-assignments-and-resource-visibility-in-pim"></a>Asignaciones aptas y visibilidad de recursos en PIM

Privileged Identity Management (PIM) para los roles de recursos de Azure proporciona una mayor seguridad para las organizaciones con recursos de Azure críticos. Los administradores de recursos pueden usar PIM para asignar miembros aptos a los roles de recursos. Obtenga más información sobre los distintos tipos y estados de asignación para los roles de recursos de Azure de las siguientes secciones. 

## <a name="assignment-types"></a>Tipos de asignación

PIM para Azure Resources ofrece dos tipos de asignación distintos:

- Apto
- Active

Las asignaciones aptas requieren que el miembro del rol realice una acción para usarlo. Estas acciones pueden incluir pasar correctamente una comprobación de autenticación multifactor, proporcionar una justificación de negocios o solicitar la aprobación de los aprobadores designados.

Las asignaciones activas no requieren que el miembro realice ninguna acción para usar el rol. Los miembros asignados como activos tienen siempre los privilegios asignados al rol.

## <a name="assignment-duration"></a>Duración de la asignación

Los administradores de recursos pueden elegir entre dos opciones para cada tipo de asignación al configurar PIM en un rol. Estas opciones se convierten en la duración máxima predeterminada cuando se asigna un rol a un miembro en PIM. 

Un administrador puede elegir uno de estos tipos de asignación:

- Allow permanent eligible assignment (Permitir la asignación apta permanente)
- Allow permanent active assignment (Permitir la asignación activa permanente)

Por otro lado, un administrador puede elegir uno de estos tipos de asignación:

- Expire eligible assignments after (Expirar las asignaciones aptas después de)
- Expire active assignments after (Expirar las asignaciones activas después de)

Si un administrador de recursos elige **Permitir la asignación apta permanente** o **Permitir la asignación activa permanente**, todos los administradores que asignan miembros al recurso tienen la posibilidad de asignar pertenencias permanentes.

Si un administrador de recursos selecciona la opción **Expirar las asignaciones aptas después de** o **Expirar las asignaciones activas después de**, el administrador de recursos controla el ciclo de vida de la asignación al exigir que todas las asignaciones tengan una fecha de inicio y de finalización especificada.

> [!NOTE] 
> Los administradores de recursos pueden renovar todas las asignaciones que tienen una fecha de finalización específica. Asimismo, los miembros pueden iniciar solicitudes de autoservicio para [extender o renovar las asignaciones](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Estados de asignación

PIM para recursos de Azure tiene dos estados de asignación distintos que aparecen en la pestaña **Roles activos** en las vistas **Mis roles**, **Roles** y **Miembros** de PIM. Estos estados son:

- Asignado
- Activado

Cuando vea una pertenencia que aparezca en **Roles activos**, podrá usar el valor de la columna **ESTADO** para diferenciar entre los usuarios **asignados** como activos y los usuarios que **activaron** una asignación apta y que ahora están activos.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de recursos de Azure en PIM](pim-resource-roles-assign-roles.md)
