---
title: Primer uso de PIM - Azure | Microsoft Docs
description: Aprenda a empezar a usar Privileged Identity Management de Azure AD en Azure Portal.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 5b3bff27821964648713b02589c941c99e3eb03d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190096"
---
# <a name="start-using-pim"></a>Primer uso de PIM

Con Azure Active Directory (Azure AD) Privileged Identity Management (PIM), puede administrar, controlar y supervisar el acceso dentro de la organización. Este ámbito incluye el acceso a los recursos de Azure, a Azure AD y a otros servicios en línea de Microsoft, como Office 365 o Microsoft Intune.

En este artículo se explica cómo agregar la aplicación PIM (Privileged Identity Management) de Azure AD al panel del Portal de Azure.

## <a name="first-person-to-use-pim"></a>Primera persona que usa PIM

Si es la primera persona que usa PIM en el directorio, se le asignarán automáticamente los roles de [Administrador de seguridad](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) y [Administrador de roles con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) del directorio. Solo los administradores de rol con privilegios pueden administrar las asignaciones de roles de directorio de los usuarios de Azure AD. Además, puede elegir ejecutar el [asistente para la seguridad.](pim-security-wizard.md) Esto le lleva a través de la experiencia inicial de detección y asignación.

## <a name="add-pim-tile-to-the-dashboard"></a>Incorporación de un icono de PIM al panel

Para que abrir PIM sea más fácil, debe agregar un icono de PIM al panel de Azure Portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global de su directorio.

1. Haga clic en **Todos los servicios** y busque el servicio **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management en Todos los servicios](./media/pim-getting-started/pim-all-services-find.png)

1. Haga clic para abrir el Inicio rápido de PIM.

1. Marque **Anclar hoja al panel** para anclar la hoja Inicio rápido de PIM al panel.

    ![Anclar hoja al panel](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    En el panel de Azure, verá un icono como este:

    ![Icono de inicio rápido de PIM](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="navigate-to-your-tasks"></a>Navegación a sus tareas

Una vez que PIM esté configurado, puede usar esta hoja para realizar las tareas de administración de identidades.

![Tareas de nivel superior para PIM (captura de pantalla)](./media/pim-getting-started/pim-quickstart-tasks.png)

| Tarea y administrar | DESCRIPCIÓN |
| --- | --- |
| **Mis roles**  | Muestra una lista de los roles elegibles y activos que tiene asignados. Aquí es donde puede activar cualquier función elegible asignada. |
| **Mis solicitudes** | Muestra las solicitudes pendientes para activar las asignaciones de rol elegibles. |
| **Acceso a las aplicaciones** | Le permite reducir los posibles retrasos y usar un rol inmediatamente después de la activación. |
| **Aprobar solicitudes** | Muestra una lista de las solicitudes realizadas por los usuarios de su directorio para activar roles elegibles que usted tiene designados para aprobar. |
| **Revisar acceso** | Enumera las revisiones de acceso activas que tiene asignadas para completar, tanto si revisa el acceso usted mismo como si lo hace otro usuario. |
| **Roles de directorio de Azure AD** | Muestra un panel y los valores de los administradores de rol con privilegios para administrar las asignaciones de roles de directorio de Azure AD. Este panel está deshabilitado para todos aquellos que no sean administradores de roles con privilegios. Estos usuarios tienen acceso a un panel especial denominado My view (Mi vista). El panel My view (Mi vista) solo muestra información sobre el acceso al panel del usuario, no del inquilino completo. |
| **Recursos de Azure** | Muestra un panel y los valores de los administradores de rol con privilegios para administrar las asignaciones de roles de recursos de Azure. Este panel está deshabilitado para todos aquellos que no sean administradores de roles con privilegios. Estos usuarios tienen acceso a un panel especial denominado My view (Mi vista). El panel My view (Mi vista) solo muestra información sobre el acceso al panel del usuario, no del inquilino completo. |

## <a name="next-steps"></a>Pasos siguientes

- [Activación de mis roles de directorio de Azure AD en PIM](pim-how-to-activate-role.md)
- [Activación de mis roles de recursos de Azure en PIM](pim-resource-roles-activate-your-roles.md)
