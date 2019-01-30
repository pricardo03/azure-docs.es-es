---
title: Administración del acceso mediante RBAC y Azure Portal | Microsoft Docs
description: Aprenda a administrar el acceso de usuarios, grupos, entidades de servicio e identidades administradas mediante el control de acceso basado en rol (RBAC) y Azure Portal. Esto incluye cómo enumerar el acceso, conceder acceso y quitar el acceso.
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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c339556353967db26f022384f2cf877962dc6d83
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2019
ms.locfileid: "54412315"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>Administración del acceso mediante RBAC y Azure Portal

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en que se administra el acceso a los recursos en Azure. En este artículo se describe cómo administrar el acceso de usuarios, grupos, entidades de servicio e identidades administradas con RBAC y Azure Portal.

## <a name="open-access-control-iam"></a>Apertura de Control de acceso (IAM)

La hoja **Control de acceso (IAM)**, también conocida como administración de identidades y accesos, aparece en el portal. Para ver o administrar el acceso en el portal, lo primero que se suele hacer es abrir la hoja Control de acceso (IAM) en el ámbito donde se quiere ver o realizar un cambio.

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione el ámbito o recurso que quiere ver o administrar. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico que quiere ver o administrar.

1. Haga clic en **Control de acceso (IAM)**.

    A continuación se muestra un ejemplo de la hoja Control de acceso (IAM) de una suscripción.

    ![Hoja Control de acceso (IAM) para una suscripción](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>Visualización de roles y permisos

Una definición de rol es una colección de permisos que se usan para las asignaciones de roles. Azure tiene más de 70 [roles integrados](built-in-roles.md). Siga estos pasos para ver los roles y permisos que se pueden realizar en el plano de datos y de administración.

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere ver roles y permisos.

1. Haga clic en la pestaña **Roles** para ver una lista de todos los roles integrados y personalizados.

   Puede ver el número de usuarios y grupos asignados a cada rol en este ámbito.

   ![Lista Roles](./media/role-assignments-portal/roles-list.png)

1. Haga clic en un rol individual para ver a quién se le ha asignado y ver también los permisos del rol.

   ![Asignaciones de roles](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>Visualización de asignaciones de roles

Al administrar el acceso, desea saber quién tiene acceso, cuáles son sus permisos y a qué nivel los tiene. Para mostrar el acceso de un usuario, grupo, entidad de servicio o identidad administrad, vea las asignaciones de roles.

### <a name="view-role-assignments-for-a-single-user"></a>Visualización de asignaciones de roles de un solo usuario

Siga estos pasos para ver el acceso de un solo usuario, grupo, entidad de servicio o identidad administrada en un ámbito determinado.

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere ver el acceso.

1. Haga clic en la pestaña **Comprobación de acceso**.

    ![Control de acceso: pestaña Comprobar acceso](./media/role-assignments-portal/access-control-check-access.png)

1. En la lista **Buscar**, seleccione el tipo de entidad de seguridad cuyo acceso quiere comprobar.

1. En el cuadro de búsqueda, escriba una cadena para buscar nombres para mostrar, direcciones de correo electrónico o identificadores de objeto en el directorio.

    ![Lista de selección de Comprobar acceso](./media/role-assignments-portal/check-access-select.png)

1. Haga clic en la entidad de seguridad para abrir el panel **Asignaciones**.

    ![Panel de asignaciones](./media/role-assignments-portal/check-access-assignments.png)

    En este panel puede ver los roles asignados a la entidad de seguridad seleccionada y el ámbito. Si hay alguna asignación denegada en este ámbito o heredada de este ámbito, se mostrarán.

### <a name="view-all-role-assignments-at-a-scope"></a>Visualización de todas las asignaciones de roles en un ámbito

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere ver el acceso.

1. Haga clic en la pestaña **Asignaciones de roles** (o haga clic en el botón **Ver** en el icono Ver asignaciones de roles) para ver todas las asignaciones de roles en este ámbito.

   ![Control de acceso: pestaña Asignaciones de roles](./media/role-assignments-portal/access-control-role-assignments.png)

   En la pestaña Asignaciones de roles puede ver quién tiene acceso a este ámbito. Observe que el ámbito de algunos roles es **este recurso**, mientras que el de otros es **(heredado)**  de otro ámbito. El acceso se asigna específicamente a este recurso o se hereda de una asignación en el ámbito principal.

## <a name="add-a-role-assignment"></a>Adición de una asignación de roles

En RBAC, para conceder acceso, asigne un rol a un usuario, grupo, entidad de servicio o identidad administrada. Siga estos pasos para conceder acceso en distintos ámbitos.

### <a name="assign-a-role-at-a-scope"></a>Asignación de un rol en un ámbito

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere conceder el acceso.

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

1. Haga clic en **Agregar asignación de roles** para abrir el panel con ese nombre.

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Panel Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment.png)

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, seleccione un usuario, grupo, entidad de servicio o identidad administrada. Si no ve la entidad de seguridad en la lista, puede escribir en el cuadro **Seleccionar** para nombres para mostrar, direcciones de correo electrónico e identificadores de objeto en el directorio.

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la entidad de seguridad en el ámbito seleccionado.

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Asignación de un usuario como administrador de una suscripción

Para que un usuario sea administrador de una suscripción de Azure, asígnele el rol [Propietario](built-in-roles.md#owner) en el ámbito de la suscripción. El rol Propietario da al usuario acceso completo a todos los recursos de la suscripción, incluido el derecho a delegar este acceso a otros. Estos pasos son los mismos que para la asignación de cualquier otro rol.

1. En Azure Portal, haga clic en **Todos los servicios** y luego en **Suscripciones**.

1. Haga clic en la suscripción a la que quiere conceder acceso.

1. Haga clic en **Control de acceso (IAM)**.

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles para esta suscripción.

1. Haga clic en **Agregar asignación de roles** para abrir el panel con ese nombre.

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Panel Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment.png)

1. En la lista desplegable **Rol**, seleccione el rol **Propietario**.

1. En la lista **Seleccionar**, seleccione un usuario. Si no ve el usuario en la lista, puede escribir en el cuadro **Seleccionar** para buscar el directorio de nombres para mostrar y direcciones de correo electrónico.

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, al usuario se le asigna el rol Propietario en el ámbito de la suscripción.

## <a name="remove-role-assignments"></a>Eliminación de asignaciones de roles

En RBAC, para quitar el acceso hay que quitar una asignación de roles. Siga estos pasos para quitar el acceso.

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere quitar el acceso.

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles para esta suscripción.

1. En la lista de asignaciones de roles, agregue una marca de verificación a la entidad de seguridad con la asignación de roles que desee quitar.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Haga clic en **Quitar**.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment.png)

1. En el mensaje de eliminación de asignación de roles que aparece, haga clic en **Sí**.

    Las asignaciones de roles heredadas no se pueden quitar. Si necesita quitar una asignación de roles heredada, debe hacerlo en el ámbito en el que se creó. En la columna **Ámbito**, junto a **(Heredado)**, hay un vínculo que lo dirige al ámbito donde se asignó este rol. Vaya al ámbito indicado ahí para quitar la asignación de roles.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Concesión de acceso a un usuario mediante RBAC y Azure Portal](quickstart-assign-role-user-portal.md)
* [Tutorial: Concesión de acceso a un usuario mediante RBAC y Azure PowerShell](tutorial-role-assignments-user-powershell.md)
* [Solución de problemas de RBAC en Azure](troubleshooting.md)
* [Organización de los recursos con grupos de administración de Azure](../azure-resource-manager/management-groups-overview.md)
