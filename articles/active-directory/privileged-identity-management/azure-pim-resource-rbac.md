---
title: Visualización de la actividad y del historial de auditoría para los roles de recursos de Azure en PIM | Microsoft Docs
description: Visualización de la actividad y del historial de auditoría para los roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 01/24/2019
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9afbbad9b956d41252ff9121c99a91669c71aaab
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56211740"
---
# <a name="view-activity-and-audit-history-for-azure-resource-roles-in-pim"></a>Visualización de la actividad y del historial de auditoría para los roles de recursos de Azure en PIM

Con Privileged Identity Management (PIM) de Azure Active Directory (Azure AD), puede ver la actividad, las activaciones y el historial de auditoría de los roles de recursos de Azure dentro de su organización. Esto incluye las suscripciones, los grupos de recursos e incluso las máquinas virtuales. Todos los recursos de Azure Portal que aprovechan la funcionalidad de control de acceso basado en rol (RBAC) de Azure también pueden aprovechar todas las ventajas de las funcionalidades de seguridad y administración del ciclo de vida que ofrece PIM.

## <a name="view-activity-and-activations"></a>Visualización de la actividad y activaciones

Para ver las acciones que realizó un usuario específico en varios recursos, puede ver la actividad de los recursos de Azure asociada a un período de activación específico.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

1. Haga clic en el recurso para el que desea ver la actividad y las activaciones.

1. Haga clic en **Roles** o en **Miembros**.

1. Haga clic en un usuario.

    Observará una vista gráfica de las acciones del usuario en los recursos de Azure por fecha. También muestra las activaciones de roles recientes durante ese mismo período de tiempo.

    ![Detalles del usuario](media/azure-pim-resource-rbac/rbac-user-details.png)

1. Haga clic en una activación de un rol específico para ver los detalles y la actividad de los recursos de Azure correspondiente al tiempo de actividad de ese usuario.

    ![Seleccionar activación de rol](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="export-role-assignments-with-children"></a>Exportación de asignaciones de roles con elementos secundarios

Puede tener un requisito de cumplimiento en el que debe proporcionar una lista completa de las asignaciones de roles a los auditores. PIM le permite consultar las asignaciones de roles en un recurso específico, que incluye asignaciones de roles para todos los recursos secundarios. Anteriormente, a los administradores les resultaba difícil obtener una lista completa de asignaciones de roles para una suscripción y tenían que exportar dichas asignaciones para cada recurso específico. Con PIM, puede consultar todas las asignaciones de roles activas y elegibles de una suscripción, incluidas las asignaciones de roles para todos los recursos y grupos de recursos.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

1. Haga clic en el recurso para el que desea exportar asignaciones de roles, como una suscripción.

1. Haga clic en **Miembros**.

1. Haga clic en **Exportar** para abrir el panel de exportación de pertenencias.

    ![Panel de exportación de pertenencias](media/azure-pim-resource-rbac/export-membership.png)

1. Haga clic en **Exportar todos los miembros** para exportar todas las asignaciones de roles en un archivo CSV.

    ![Archivo CSV de exportación](media/azure-pim-resource-rbac/export-csv.png)

## <a name="view-resource-audit-history"></a>Visualización del historial de auditoría de recursos

La auditoría de recursos proporciona una vista de todas las actividades de rol del recurso.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

1. Haga clic en el recurso para el que desea ver el historial de auditoría.

1. Haga clic en **Auditoría de recursos**.

1. Filtre el historial con una fecha predefinida o un intervalo personalizado.

    ![Filtrar auditoría de recursos](media/azure-pim-resource-rbac/rbac-resource-audit.png)

1. En **Tipo de auditoría**, seleccione **Activate (Assigned + Activated)** (Activar [Asignado + Activado]).

    ![Detalles de actividad](media/azure-pim-resource-rbac/rbac-audit-activity.png)

1. En **Acción**, haga clic en **(actividad)** para que un usuario vea el detalle de la actividad de ese usuario en los recursos de Azure.

    ![Detalles de actividad del usuario](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="view-my-audit"></a>Visualización de mi auditoría

Mi auditoría permite ver la actividad del rol personal.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Recursos de Azure**.

1. Haga clic en el recurso para el que desea ver el historial de auditoría.

1. Haga clic en **Mi auditoría**.

1. Filtre el historial con una fecha predefinida o un intervalo personalizado.

    ![Actividad de rol personal](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de recursos de Azure en PIM](pim-resource-roles-assign-roles.md)
- [Aprobación o rechazo de solicitudes de roles de recursos de Azure en PIM](pim-resource-roles-approval-workflow.md)
- [Visualización del historial de auditorías de los roles de directorios de Azure AD en PIM](pim-how-to-use-audit-log.md)
