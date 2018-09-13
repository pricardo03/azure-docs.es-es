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
ms.openlocfilehash: fb52bc92c86261831d0e8d8e9e863a4863fe8fb9
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666896"
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

## <a name="azure-resource-role-approval-workflow"></a>Flujo de trabajo de aprobación de roles de recursos de Azure

Con el flujo de trabajo de aprobación de PIM para roles de recursos de Azure, los administradores pueden proteger o restringir aún más el acceso a los recursos críticos. Es decir, los administradores pueden solicitar aprobación para activar asignaciones de roles.

El concepto de una jerarquía de recursos es único de los roles de recursos de Azure. Esta jerarquía permite la herencia de asignaciones de roles de un objeto de recurso primario a todos los recursos secundarios del contenedor primario. 

Por ejemplo: Bob, un administrador de recursos, usa PIM para asignar a Alice como miembro apto para el rol de Propietario de la suscripción de Contoso. Con esta asignación, Alice es un propietario apto de todos los contenedores del grupo de recursos de la suscripción de Contoso. Alice también es un propietario apto de todos los recursos (como máquinas virtuales) de cada grupo de recursos de la suscripción.

Supongamos que hay tres grupos de recursos en la suscripción a Contoso: Fabrikam Test, Fabrikam Dev y Fabrikam Prod. Cada uno de ellos contiene una única máquina virtual.

La configuración de PIM se configura para cada rol de un recurso. A diferencia de las asignaciones, la configuración no se hereda y se aplica estrictamente al rol del recurso.

Siguiendo con el ejemplo: Bob usa PIM para exigir a todos los miembros del rol Propietario de la suscripción de Contoso que soliciten aprobación para activarse. Para ayudar a proteger los recursos del grupo de recursos Fabrikam Prod, Bob además exige aprobación de los miembros del rol Propietario de este recurso. Los roles de propietario de Fabrikam Test y Fabrikam Dev no necesitan aprobación para activarse.

Cuando Alice solicita la activación de su rol Propietario de la suscripción de Contoso, se debe aprobar o denegar su solicitud antes de que se active en el rol. Además, si Alice decide [limitar el ámbito de su activación](pim-resource-roles-activate-your-roles.md) al grupo de recursos Fabrikam Prod, un aprobador debe aprobar o denegar dicha solicitud. Pero si Alice decide limitar el ámbito de su activación a Fabrikam Test o Fabrikam Dev, o ambos, no se requiere aprobación.

Es posible que el flujo de trabajo de aprobación no sea necesario para todos los miembros de un rol. Piense en un escenario en que la organización contrata a varios subcontratados para que ayuden en el desarrollo de una aplicación que se va a ejecutar en una suscripción de Azure. Como administrador de recursos, quiere que los empleados tengan acceso apto sin necesidad de aprobación, pero que los subcontratados deban solicitar aprobación. Para configurar el flujo de trabajo de aprobación solo para los subcontratados, puede crear un rol personalizado con los mismos permisos que el rol asignado a los empleados. Puede exigir aprobación para activar ese rol personalizado. [Más información acerca de los roles personalizados](pim-resource-roles-custom-role-policy.md).

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de recursos de Azure en PIM](pim-resource-roles-assign-roles.md)
