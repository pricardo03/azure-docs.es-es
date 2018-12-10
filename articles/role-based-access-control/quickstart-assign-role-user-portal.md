---
title: 'Tutorial: Concesión de acceso a un usuario mediante RBAC y Azure Portal | Microsoft Docs'
description: Use el control de acceso basado en rol (RBAC) para conceder permisos a un usuario mediante la asignación de un rol en Azure Portal.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.openlocfilehash: 8caa5c3b33ac1b483429251e0c1256636c4ece1a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634479"
---
# <a name="tutorial-grant-access-for-a-user-using-rbac-and-the-azure-portal"></a>Tutorial: Concesión de acceso a un usuario mediante RBAC y Azure Portal

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en que se administra el acceso a los recursos en Azure. En este tutorial, se concede acceso a un usuario para crear y administrar máquinas virtuales en un grupo de recursos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conceder acceso a un usuario en el ámbito del grupo de recursos
> * Quitar acceso

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en http://portal.azure.com.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

1. En la lista de navegación, elija **Grupos de recursos**.

1. Haga clic en **Agregar** para abrir la hoja **Grupo de recursos**.

   ![Adición de un grupo de recursos nuevo](./media/quickstart-assign-role-user-portal/resource-group.png)

1. En **Nombre del grupo de recursos**, escriba **rbac-resource-group**.

1. Seleccione una suscripción y una ubicación.

1. Haga clic en **Crear** para crear el grupo de recursos.

1. Haga clic en **Actualizar** para actualizar la lista de grupos de recursos.

   El grupo de recursos nuevo aparece en la lista de grupos de recursos.

   ![Lista de grupos de recursos](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Conceder acceso

En RBAC, para conceder acceso es preciso crear una asignación de roles.

1. En la lista de **Grupos de recursos**, elija el nuevo grupo de recursos **rbac-resource-group**.

1. Haga clic en **Control de acceso (IAM)**.

1. Seleccione la pestaña **Asignaciones de roles** para ver la lista actual de asignaciones de roles.

   ![Hoja Control de acceso (IAM) para un grupo de recursos](./media/quickstart-assign-role-user-portal/access-control.png)

1. Elija **Agregar asignación de roles** para abrir el panel Agregar asignación de roles.

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Panel Agregar asignación de roles](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. En la lista desplegable **Rol**, seleccione **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, se puede seleccionar a usted mismo o a otro usuario.

1. Seleccione **Guardar** para crear la asignación de roles.

   Transcurridos unos instantes, se asigna el rol Colaborador de la máquina virtual al usuario en el ámbito del grupo de recursos rbac-resource-group.

   ![Asignación del rol Colaborador de la máquina virtual](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Quitar acceso

En RBAC, para quitar el acceso hay que quitar una asignación de roles.

1. En la lista de asignaciones de roles, agregue una marca de verificación junto al usuario con el rol Colaborador de la máquina virtual.

1. Elija **Quitar**.

   ![Mensaje de eliminación de asignación de roles](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. En el mensaje de eliminación de asignación de roles que aparece, elija **Sí**.

## <a name="clean-up"></a>Limpieza

1. En la lista de navegación, elija **Grupos de recursos**.

1. Elija **rbac-resource-group** para abrir el grupo de recursos.

1. Haga clic en **Eliminar grupo de recursos** para eliminar el grupo de recursos.

   ![Eliminación de un grupo de recursos](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. En la hoja **¿Está seguro de que desea eliminar?**, escriba el nombre del grupo de recursos: **rbac-resource-group**.

1. Haga clic en **Eliminar** para eliminar el grupo de recursos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso a un usuario mediante RBAC y PowerShell](tutorial-role-assignments-user-powershell.md)

