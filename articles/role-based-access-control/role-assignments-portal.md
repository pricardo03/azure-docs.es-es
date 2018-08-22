---
title: Administración del acceso mediante RBAC y Azure Portal | Microsoft Docs
description: Aprenda a administrar el acceso de usuarios, grupos y aplicaciones mediante el control de acceso basado en rol (RBAC) y Azure Portal. Esto incluye cómo enumerar el acceso, conceder acceso y quitar el acceso.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/07/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 97bf33cb882d5a121b9811a8e36a1d26f9a954f8
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715376"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>Administración del acceso mediante RBAC y Azure Portal

[Control de acceso basado en rol (RBAC)](overview.md) es la forma en que se administra el acceso a los recursos en Azure. Este artículo describe cómo administrar el acceso de usuarios, grupos y aplicaciones mediante el uso de RBAC y Azure Portal.

## <a name="list-roles"></a>Lista de roles

Una definición de rol es una colección de permisos que se usan para las asignaciones de roles. Azure tiene más de 70 [roles integrados](built-in-roles.md). Siga estos pasos para enumerar los roles en el portal.

1. En Azure Portal, elija **Todos los servicios** y, después, **Suscripciones**.

1. Elija su suscripción.

1. Haga clic en **Control de acceso (IAM)**.

   ![Opción Roles](./media/role-assignments-portal/list-subscription-access-control.png)

1. Elija **Roles** para ver una lista de todos los roles integrados y personalizados.

   ![Opción Roles](./media/role-assignments-portal/roles-option.png)

   Puede ver el número de usuarios y grupos asignados a cada rol.

   ![Lista Roles](./media/role-assignments-portal/roles-list.png)

## <a name="list-access"></a>Lista de acceso

Al administrar el acceso, desea saber quién tiene acceso, cuáles son sus permisos y a qué nivel los tiene. Para tener acceso a la lista, enumere las asignaciones de roles. Siga estos pasos para enumerar el acceso de los usuarios y el acceso en distintos ámbitos.

### <a name="list-role-assignments-for-a-user"></a>Lista de las asignaciones de rol de un usuario

1. En la lista de navegación, seleccione **Azure Active Directory**.

1. Elija **usuarios** para abrir **Todos los usuarios**.

   ![Hoja Todos los usuarios de Azure Active Directory](./media/role-assignments-portal/aad-all-users.png)

1. Elija un usuario individual en la lista.

1. En la sección **Administrar**, elija **Recursos de Azure**.

   ![Recursos de Azure de usuario de Azure Active Directory](./media/role-assignments-portal/aad-user-azure-resources.png)

   En la hoja Recursos de Azure puede ver las asignaciones de roles del usuario y de la suscripción seleccionados. En esta lista solo se incluye las asignaciones de roles de los recursos que tenga permiso para leer. Por ejemplo, si el usuario tiene también asignaciones de roles que no pueda leer, estas no aparecerán en la lista.

1. Si tiene varias suscripciones, puede elegir la lista desplegable **Suscripción** para ver las asignaciones de roles de otra suscripción.

### <a name="list-role-assignments-for-a-resource-group"></a>Lista de las asignaciones de roles de un grupo de recursos

1. En la lista de navegación, elija **Grupos de recursos**.

1. Elija un grupo de recursos y,después, elija **Control de acceso (IAM)**.

   En la hoja Control de acceso (IAM), también conocido como administración de identidades y acceso, puede ver quién tiene acceso a este grupo de recursos. Observe que el ámbito de algunos roles es **este recurso**, mientras que el de otros es **(heredado)**  de otro ámbito. El acceso se asigna específicamente en el grupo de recursos o se hereda de una asignación en la suscripción principal.

   ![Grupos de recursos](./media/role-assignments-portal/resource-group-access-control.png)

### <a name="list-role-assignments-for-a-subscription"></a>Enumeración de asignaciones de roles de una subscripción

1. En Azure Portal, elija **Todos los servicios** y, después, **Suscripciones**.

1. Elija su suscripción.

1. Haga clic en **Control de acceso (IAM)**.

    En la hoja Control de acceso (IAM), puede ver quién tiene acceso a esta suscripción y su rol.

    ![Hoja Control de acceso (IAM) para una suscripción](./media/role-assignments-portal/subscription-access-control.png)

    Los administradores y coadministradores de una suscripción clásica se consideran los propietarios de la suscripción en el modelo de RBAC.

### <a name="list-role-assignments-for-a-management-group"></a>Lista de asignaciones de rol para un grupo de administración

1. En Azure Portal, elija **Todos los servicios** y, después, **Grupos de administración**.

1. Elija el grupo de administración.

1. Elija **(detalles)** para el grupo de administración seleccionado.

    ![Grupos de administración](./media/role-assignments-portal/management-groups-list.png)

1. Haga clic en **Control de acceso (IAM)**.

    En la hoja Control de acceso (IAM), puede ver quién tiene acceso a este grupo de administración y su rol.

    ![Hoja Control de acceso (IAM) para un grupo de administración](./media/role-assignments-portal/management-groups-access-control.png)

## <a name="grant-access"></a>Conceder acceso

En RBAC, para conceder acceso es preciso crear una asignación de roles. Siga estos pasos para conceder acceso en distintos ámbitos.

### <a name="create-a-role-assignment-at-a-resource-group-scope"></a>Creación de una asignación de roles en el ámbito de un grupo de recursos

1. En la lista de navegación, elija **Grupos de recursos**.

1. Elija un grupo de recursos.

1. Elija **Control de acceso (IAM)** para ver la lista actual de las asignaciones de roles en el ámbito de un grupo de recursos.

   ![Hoja Control de acceso (IAM) para un grupo de recursos](./media/role-assignments-portal/grant-resource-group-access-control.png)

1. Elija **Agregar** para abrir el panel **Agregar permisos**.

   Si no tiene permisos para asignar roles, no verá la opción **Agregar**.

   ![Panel Agregar permisos](./media/role-assignments-portal/add-permissions.png)

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, seleccione un usuario, grupo o aplicación. Si no ve la entidad de seguridad en la lista, puede escribir en el cuadro **Seleccionar** para nombres para mostrar, direcciones de correo electrónico e identificadores de objeto en el directorio.

1. Seleccione **Guardar** para crear la asignación de roles.

   Transcurridos unos instantes, a la entidad de seguridad se le asigna el rol en el ámbito de un grupo de recursos.

### <a name="create-a-role-assignment-at-a-subscription-scope"></a>Creación de una asignación de roles en el ámbito de una suscripción

1. En Azure Portal, elija **Todos los servicios** y, después, **Suscripciones**.

1. Elija su suscripción.

1. Elija **Control de acceso (IAM)** para ver la lista actual de las asignaciones de roles en el ámbito de la suscripción.

   ![Hoja Control de acceso (IAM) para una suscripción](./media/role-assignments-portal/grant-subscription-access-control.png)

1. Elija **Agregar** para abrir el panel **Agregar permisos**.

   Si no tiene permisos para asignar roles, no verá la opción **Agregar**.

   ![Panel Agregar permisos](./media/role-assignments-portal/add-permissions.png)

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, seleccione un usuario, grupo o aplicación. Si no ve la entidad de seguridad en la lista, puede escribir en el cuadro **Seleccionar** para nombres para mostrar, direcciones de correo electrónico e identificadores de objeto en el directorio.

1. Seleccione **Guardar** para crear la asignación de roles.

   Transcurridos unos instantes, a la entidad de seguridad se le asigna el rol en el ámbito de la suscripción.

### <a name="create-a-role-assignment-at-a-management-group-scope"></a>Creación de una asignación de roles en el ámbito de un grupo de administración

1. En Azure Portal, elija **Todos los servicios** y, después, **Grupos de administración**.

1. Elija el grupo de administración.

1. Elija **(detalles)** para el grupo de administración seleccionado.

    ![Grupos de administración](./media/role-assignments-portal/management-groups-list.png)

1. Elija **Control de acceso (IAM)** para ver la lista actual de las asignaciones de roles en el ámbito de la suscripción.

   ![Hoja Control de acceso (IAM) para un grupo de administración](./media/role-assignments-portal/grant-management-groups-access-control.png)

1. Elija **Agregar** para abrir el panel **Agregar permisos**.

   Si no tiene permisos para asignar roles, no verá la opción **Agregar**.

   ![Panel Agregar permisos](./media/role-assignments-portal/add-permissions-management-groups.png)

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de grupo de administración**.

    Para información sobre las acciones admitidas en los grupos de administración para varios roles, consulte [Organización de los recursos con grupos de administración de Azure](../azure-resource-manager/management-groups-overview.md#management-group-access).

1. En la lista **Seleccionar**, seleccione un usuario, grupo o aplicación. Si no ve la entidad de seguridad en la lista, puede escribir en el cuadro **Seleccionar** para nombres para mostrar, direcciones de correo electrónico e identificadores de objeto en el directorio.

1. Seleccione **Guardar** para crear la asignación de roles.

   Transcurridos unos instantes, a la entidad de seguridad se le asigna el rol en el ámbito del grupo de administración.

## <a name="remove-access"></a>Quitar acceso

En RBAC, para quitar el acceso hay que quitar una asignación de roles. Siga estos pasos para quitar el acceso.

### <a name="remove-a-role-assignment"></a>Eliminación de una asignación de rol

1. Abra la hoja **Control de acceso (IAM)**  del grupo de administración, suscripción, grupo de recursos o recurso que tengan la asignación de roles que quiere quitar.

1. En la lista de asignaciones de roles, agregue una marca de verificación a la entidad de seguridad con la asignación de roles que desee quitar.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Elija **Quitar**.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment.png)

1. En el mensaje de eliminación de asignación de roles que aparece, elija **Sí**.

    Las asignaciones de roles heredadas no se pueden quitar. Si necesita quitar una asignación de roles heredada, debe hacerlo en el ámbito en el que se creó. En la columna **Ámbito**, junto a **(Heredado)**, hay un vínculo que lo dirige al ámbito donde se asignó este rol. Vaya al ámbito indicado ahí para quitar la asignación de roles.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Concesión de acceso a un usuario mediante RBAC y Azure Portal](quickstart-assign-role-user-portal.md)
* [Tutorial: Concesión de acceso a un usuario mediante RBAC y Azure PowerShell](tutorial-role-assignments-user-powershell.md)
* [Roles integrados](built-in-roles.md)
* [Organización de los recursos con grupos de administración de Azure](../azure-resource-manager/management-groups-overview.md)
