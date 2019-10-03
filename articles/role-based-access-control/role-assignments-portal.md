---
title: Administración del acceso a los recursos de Azure mediante RBAC y Azure Portal | Microsoft Docs
description: Aprenda a administrar el acceso a recursos de Azure de usuarios, grupos, entidades de servicio e identidades administradas mediante el control de acceso basado en rol (RBAC) y Azure Portal. Esto incluye cómo enumerar el acceso, conceder acceso y quitar el acceso.
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
ms.date: 02/24/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 1e9a53c41535c17de2d56227012160c7f6eb25c6
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71337618"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>Administración del acceso a los recursos de Azure mediante RBAC y Azure Portal

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en la que se administra el acceso a los recursos de Azure. En este artículo se describe cómo administrar el acceso mediante Azure Portal. Si necesita administrar el acceso a Azure Active Directory, consulte [Visualización y asignación de roles de administrador en Azure Active Directory](../active-directory/users-groups-roles/directory-manage-roles-portal.md).

## <a name="prerequisites"></a>Requisitos previos

Para agregar y quitar las asignaciones de roles, debe tener:

- Permisos `Microsoft.Authorization/roleAssignments/write` y `Microsoft.Authorization/roleAssignments/delete`, como [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator) o [propietario](built-in-roles.md#owner)

## <a name="overview-of-access-control-iam"></a>Introducción a Control de acceso (IAM)

**Control de acceso (IAM)** es la hoja que usa para administrar el acceso a los recursos de Azure. También se conoce como administración de identidades y acceso y aparece en varias ubicaciones en Azure Portal. A continuación se muestra un ejemplo de la hoja Control de acceso (IAM) de una suscripción.

![Hoja Control de acceso (IAM) para una suscripción](./media/role-assignments-portal/access-control-numbers.png)

En la siguiente tabla se describe para qué se usan algunos de los elementos:

| # | Elemento | Uso |
| --- | --- | --- |
| 1 | Recursos donde Control de acceso (IAM) se abre | Identificar el ámbito (suscripción en este ejemplo) |
| 2 | Botón **Agregar** | Agregar asignación de roles |
| 3 | Pestaña **Comprobar acceso** | Ver las asignaciones de roles de un solo usuario |
| 4 | Pestaña **Asignaciones de roles** | Ver las asignaciones de roles en el ámbito actual |
| 5 | Pestaña **Roles** | Ver todos los roles y permisos |

Para ser más efectivo con la hoja Control de acceso (IAM), es útil si puede responder las siguientes tres preguntas cuando intenta administrar el acceso:

1. **¿Quién necesita acceso?**

    Quién se refiere a un usuario, grupo, entidad de servicio o identidad administrada. Esto también se denomina una *entidad de seguridad*.

1. **¿Qué permisos necesitan?**

    Los permisos se agrupan en roles. Puede seleccionar en una lista de varios roles integrados.

1. **¿Dónde necesitan acceso?**

    Dónde se refiere al conjunto de recursos al que se aplica el acceso. Dónde puede ser un grupo de administración, una suscripción, un grupo de recursos o un único recurso, como una cuenta de almacenamiento. Esto se denomina el *ámbito*.

## <a name="open-access-control-iam"></a>Apertura de Control de acceso (IAM)

Lo primero que debe decidir es dónde abrir la hoja Control de acceso (IAM). Depende de los recursos para los que desea administrar el acceso. ¿Desea administrar el acceso para todo en un grupo de administración, todo en una suscripción, todo en un grupo de recursos o un solo recurso?

1. En Azure Portal, haga clic en **Todos los servicios** y luego seleccione el ámbito. Por ejemplo, puede seleccionar **Grupos de administración**, **Suscripciones**, **Grupos de recursos** o un recurso.

1. Haga clic en el recurso específico.

1. Haga clic en **Control de acceso (IAM)** .

    A continuación se muestra un ejemplo de la hoja Control de acceso (IAM) de una suscripción. Si realiza algún cambio en el control de acceso aquí, se aplicará a toda la suscripción.

    ![Hoja Control de acceso (IAM) para una suscripción](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>Visualización de roles y permisos

Una definición de rol es una colección de permisos que se usan para las asignaciones de roles. Azure tiene más de 70 [roles integrados en los recursos de Azure](built-in-roles.md). Siga estos pasos para ver los roles y permisos disponibles.

1. Abra **Control de acceso (IAM)** en cualquier ámbito.

1. Haga clic en la pestaña **Roles** para ver una lista de todos los roles integrados y personalizados.

   Puede ver el número de usuarios y grupos asignados a cada rol en el ámbito actual.

   ![Lista Roles](./media/role-assignments-portal/roles-list.png)

1. Haga clic en un rol individual para ver a quién se le ha asignado y ver también los permisos del rol.

   ![Asignaciones de roles](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>Visualización de asignaciones de roles

Al administrar el acceso, desea saber quién tiene acceso, cuáles son sus permisos y en qué ámbito los tiene. Para mostrar el acceso de un usuario, grupo, entidad de servicio o identidad administra, vea sus asignaciones de roles.

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

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

   ![Control de acceso: pestaña Asignaciones de roles](./media/role-assignments-portal/access-control-role-assignments.png)

   En la pestaña Asignaciones de roles puede ver quién tiene acceso a este ámbito. Observe que el ámbito de algunos roles es **este recurso**, mientras que el de otros es **(heredado)**  de otro ámbito. El acceso se asigna específicamente a este recurso o se hereda de una asignación en el ámbito principal.

## <a name="add-a-role-assignment"></a>Adición de una asignación de roles

En RBAC, para conceder acceso, asigne un rol a un usuario, grupo, entidad de servicio o identidad administrada. Siga estos pasos para conceder acceso en distintos ámbitos.

### <a name="assign-a-role-at-a-scope"></a>Asignación de un rol en un ámbito

1. Abra **Control de acceso (IAM)** en un ámbito como, por ejemplo, grupo de administración, suscripción, grupo de recursos o recurso, en donde quiere conceder el acceso.

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles en este ámbito.

1. Haga clic en **Agregar** > **Agregar asignación de roles** para abrir el panel Agregar asignación de roles.

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú Agregar](./media/role-assignments-portal/add-menu.png)

   ![Panel Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment.png)

1. En la lista desplegable **Rol**, seleccione un rol como **Colaborador de la máquina virtual**.

1. En la lista **Seleccionar**, seleccione un usuario, grupo, entidad de servicio o identidad administrada. Si no ve la entidad de seguridad en la lista, puede escribir en el cuadro **Seleccionar** para nombres para mostrar, direcciones de correo electrónico e identificadores de objeto en el directorio.

1. Haga clic en **Guardar** para asignar el rol.

   Transcurridos unos instantes, se asigna el rol a la entidad de seguridad en el ámbito seleccionado.

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Asignación de un usuario como administrador de una suscripción

Para que un usuario sea administrador de una suscripción de Azure, asígnele el rol [Propietario](built-in-roles.md#owner) en el ámbito de la suscripción. El rol Propietario da al usuario acceso completo a todos los recursos de la suscripción, incluido el derecho a delegar este acceso a otros. Estos pasos son los mismos que para la asignación de cualquier otro rol.

1. En Azure Portal, haga clic en **Todos los servicios** y luego en **Suscripciones**.

1. Haga clic en la suscripción a la que quiere conceder acceso.

1. Haga clic en **Control de acceso (IAM)** .

1. Haga clic en la pestaña **Asignaciones de roles** para ver todas las asignaciones de roles para esta suscripción.

1. Haga clic en **Agregar** > **Agregar asignación de roles** para abrir el panel Agregar asignación de roles.

   Si no tiene permisos para asignar roles, la opción Agregar asignación de roles se deshabilitará.

   ![Menú Agregar](./media/role-assignments-portal/add-menu.png)

   ![Panel Agregar asignación de roles](./media/role-assignments-portal/add-role-assignment.png)

1. En la lista desplegable **Rol**, seleccione el rol **Propietario**.

1. En la lista **Seleccionar**, seleccione un usuario. Si no ve el usuario en la lista, puede escribir en el cuadro **Seleccionar** para buscar en el directorio por nombres para mostrar y direcciones de correo electrónico.

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

    Las asignaciones de roles heredadas no se pueden quitar. Si necesita quitar una asignación de roles heredada, debe hacerlo en el ámbito en el que se creó. En la columna **Ámbito**, junto a **(Heredado)** , hay un vínculo que lo dirige al ámbito donde se asignó este rol. Vaya al ámbito indicado ahí para quitar la asignación de roles.

   ![Mensaje de eliminación de asignación de roles](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante RBAC y Azure Portal](quickstart-assign-role-user-portal.md)
* [Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante RBAC y Azure PowerShell](tutorial-role-assignments-user-powershell.md)
* [Solución de problemas del control de acceso basado en rol para recursos de Azure](troubleshooting.md)
* [Organización de los recursos con grupos de administración de Azure](../governance/management-groups/overview.md)
