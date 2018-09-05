---
title: Asignación de roles de recursos de Azure en PIM | Microsoft Docs
description: Aprenda a asignar roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
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
ms.openlocfilehash: 7019a6f97a9590d3b652584015f3077f4ed075af
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188927"
---
# <a name="assign-azure-resource-roles-in-pim"></a>Asignación de roles de recursos de Azure en PIM

Azure AD PIM puede administrar los roles incorporados de los recursos de Azure, así como los roles personalizados, entre ellos, los siguientes:

- Propietario
- Administrador de acceso de usuario
- Colaborador
- Administrador de seguridad
- Y muchos otros

>[!NOTE]
Los usuarios o los miembros de un grupo asignados a los roles Propietario o Administrador de acceso de usuario y los administradores globales que permiten la administración de suscripciones de Azure AD son administradores de recursos. Estos administradores pueden asignar roles, configurar opciones de rol y revisar el acceso con PIM a los recursos de Azure. Consulte la lista de [roles integrados en los recursos de Azure](../../role-based-access-control/built-in-roles.md).

## <a name="assign-roles"></a>Asignación de roles

Para asignar un usuario o grupo a un rol mientras se está viendo el panel **Roles**, seleccione el rol y luego **Agregar usuario**. 

![Panel "Roles" con el botón "Agregar usuario"](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

También puede seleccionar **Agregar usuario** desde el panel **Miembros**.

![Panel "Miembros" con el botón "Agregar usuario"](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Si va a agregar un usuario o un grupo desde el panel **Miembros**, tiene que: 

1. Elegir un rol en el panel **Seleccionar un rol** antes de seleccionar un usuario o un grupo.

   ![Panel "Seleccionar un rol"](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Elija un usuario o grupo del directorio.

3. Elija el tipo de asignación adecuado en el menú desplegable: 

   - **Just-In-Time**: proporciona acceso apto pero no persistente al rol durante un período determinado o indefinidamente (si se ha configurado en la configuración del rol) al usuario o los miembros del grupo. 
   - **Directa**: no requiere que el usuario o los miembros del grupo activen la asignación del rol (esto se conoce como acceso persistente). Se recomienda usar la asignación directa para el uso a corto plazo, cuando el acceso ya no sea necesario una vez completada la tarea. Ejemplos de esto son los turnos de guardia y las actividades sujetas a limitación temporal.

4. Si la asignación debe ser permanente (siempre apta para una asignación Just-In-Time o siempre activa para una asignación directa), active la casilla situada debajo del cuadro **Tipo de asignación**.

   ![Panel "Configuración de pertenencia" con el cuadro "Tipo de asignación" y la casilla relacionada](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

   >[!NOTE]
   >Si otro administrador ha especificado la duración máxima de cada tipo de asignación en la configuración del rol, la casilla podría no admitir modificación.

   Para especificar una duración de asignación específica, desactive la casilla y modifique los cuadros de fecha y hora de inicio o finalización.

   ![Panel "Configuración de pertenencia" con cuadros de fecha de inicio, hora de inicio, fecha de finalización y hora de finalización](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Administración de asignaciones de roles

Los administradores pueden administrar las asignaciones de roles mediante la selección de **Roles** o **Miembros** en el panel izquierdo. La selección de **Roles** permite a los administradores limitar el ámbito de sus tareas de administración a un rol concreto. La selección de **Miembros** muestra todas las asignaciones de roles de usuario y grupo del recurso.

![Panel "Roles"](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![Panel "Miembros"](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Si tiene un rol pendiente de activación, se muestra un banner de notificación en la parte superior del panel al ver la pertenencia.


## <a name="modify-existing-assignments"></a>Modificación de las asignaciones existentes

Para modificar las asignaciones existentes desde la vista de detalles del usuario o grupo, seleccione **Cambiar configuración** en la barra de acciones. Cambie el tipo de asignación a **Just-In-Time** o **Directa**.

![Panel "Detalles de usuario" con el botón "Cambiar configuración"](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de roles de recurso de Azure en PIM](pim-resource-roles-configure-role-settings.md)
- [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md)
