---
title: Configuración de roles de recurso de Azure en PIM | Microsoft Docs
description: Aprenda a configurar los roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 901eb5ef43ddb2840ed7a3d83fc08f2f05849461
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189742"
---
# <a name="configure-azure-resource-role-settings-in-pim"></a>Configuración de roles de recurso de Azure en PIM

Al configurar el rol, se define la configuración predeterminada que se aplica a las asignaciones en el entorno de Privileged Identity Management (PIM). Para definir esta configuración para el recurso, seleccione la pestaña **Configuración de roles** en el panel izquierdo. También puede seleccionar el botón de la configuración de rol en la barra de acción (en cualquier rol) para ver las opciones actuales.

## <a name="overview"></a>Información general

Con el flujo de trabajo de aprobación de Privileged Identity Management (PIM) para roles de recursos de Azure, los administradores pueden proteger o restringir aún más el acceso a los recursos críticos. Es decir, los administradores pueden solicitar aprobación para activar asignaciones de roles. 

El concepto de una jerarquía de recursos es único de los roles de recursos de Azure. Esta jerarquía permite la herencia de asignaciones de roles de un objeto de recurso primario a todos los recursos secundarios del contenedor primario. 

Por ejemplo: Bob, un administrador de recursos, usa PIM para asignar a Alice como miembro apto para el rol de Propietario de la suscripción de Contoso. Con esta asignación, Alice es un propietario apto de todos los contenedores del grupo de recursos de la suscripción de Contoso. Alice también es un propietario apto de todos los recursos (como máquinas virtuales) de cada grupo de recursos de la suscripción. 

Supongamos que hay tres grupos de recursos en la suscripción a Contoso: Fabrikam Test, Fabrikam Dev y Fabrikam Prod. Cada uno de ellos contiene una única máquina virtual.

La configuración de PIM se configura para cada rol de un recurso. A diferencia de las asignaciones, la configuración no se hereda y se aplica estrictamente al rol del recurso. [Obtenga más información sobre asignaciones aptas y visibilidad de recursos](pim-resource-roles-eligible-visibility.md).

Siguiendo con el ejemplo: Bob usa PIM para exigir a todos los miembros del rol Propietario de la suscripción de Contoso que soliciten aprobación para activarse. Para ayudar a proteger los recursos del grupo de recursos Fabrikam Prod, Bob además exige aprobación de los miembros del rol Propietario de este recurso. Los roles de propietario de Fabrikam Test y Fabrikam Dev no necesitan aprobación para activarse.

Cuando Alice solicita la activación de su rol Propietario de la suscripción de Contoso, se debe aprobar o denegar su solicitud antes de que se active en el rol. Además, si Alice decide [limitar el ámbito de su activación](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) al grupo de recursos Fabrikam Prod, un aprobador debe aprobar o denegar dicha solicitud. Pero si Alice decide limitar el ámbito de su activación a Fabrikam Test o Fabrikam Dev, o ambos, no se requiere aprobación.

Es posible que el flujo de trabajo de aprobación no sea necesario para todos los miembros de un rol. Piense en un escenario en que la organización contrata a varios subcontratados para que ayuden en el desarrollo de una aplicación que se va a ejecutar en una suscripción de Azure. Como administrador de recursos, quiere que los empleados tengan acceso apto sin necesidad de aprobación, pero que los subcontratados deban solicitar aprobación. Para configurar el flujo de trabajo de aprobación solo para los subcontratados, puede crear un rol personalizado con los mismos permisos que el rol asignado a los empleados. Puede exigir aprobación para activar ese rol personalizado. [Más información acerca de los roles personalizados](pim-resource-roles-custom-role-policy.md).

Para configurar el flujo de trabajo de aprobación y especificar quién puede aprobar o rechazar solicitudes, use los siguientes procedimientos.

## <a name="require-approval-to-activate"></a>Solicitud de aprobación para activar

1. Vaya a PIM en Azure Portal y seleccione un recurso de la lista.

   ![Panel "Recursos de Azure" con un recurso seleccionado](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. En el panel izquierdo, seleccione **Configuración de roles**.

3. Busque un rol, selecciónelo y luego seleccione **Editar** para modificar la configuración.

   ![Botón "Editar" del rol de operador](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. En la sección **Activación**, active la casilla **Se requiere aprobación para activar**.

   ![Sección "Activación" de Configuración de roles](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Especificación de aprobadores

Haga clic en **Seleccionar aprobadores** para abrir el panel **Seleccione un usuario o grupo**.

>[!NOTE]
>Debe seleccionar al menos un usuario o grupo para actualizar la configuración. No hay aprobadores predeterminados.

Los administradores de recursos pueden agregar cualquier combinación de usuarios y grupos a la lista de aprobadores. 

![Panel "Seleccione un usuario o grupo" con un usuario seleccionado](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Solicitud de aprobación para activar

La solicitud de aprobación no tiene ningún efecto sobre el procedimiento que debe seguir un miembro para realizar la activación. [Revise los pasos para activar un rol](pim-resource-roles-activate-your-roles.md).

Si un miembro ha solicitado la activación de un rol que exige aprobación y el rol ha dejado de ser necesario, el miembro puede cancelar su solicitud en PIM.

Para cancelar, vaya a PIM y seleccione **Mis solicitudes**. Busque la solicitud y seleccione **Cancelar**.

![Panel "Mis solicitudes"](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="next-steps"></a>Pasos siguientes

- [Exigencia de autenticación multifactor para los roles de recurso de Azure en PIM](pim-resource-roles-require-mfa.md)
- [Configuración de alertas de seguridad para los roles de recurso de Azure en PIM](pim-resource-roles-configure-alerts.md)
