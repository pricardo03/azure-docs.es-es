---
title: 'Visualización del informe de auditoría para roles de Azure AD en PIM: Azure AD | Microsoft Docs'
description: Aprenda a visualizar el historial de auditoría de los roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/13/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d46036efa04b4e0225cad6e8a70cd31ad3c10bd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024184"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>Visualización del historial de auditoría de los roles de Azure AD en PIM

Puede utilizar el historial de auditoría de Privileged Identity Management (PIM) para ver todas las asignaciones de roles y las activaciones de los últimos 30 días para todos los roles con privilegios. Si quiere ver el historial de auditoría completo de la actividad de la organización de Azure Active Directory (Azure AD), incluidos el administrador, el usuario final y la actividad de sincronización, puede usar los [informes de actividad y seguridad de Azure Active Directory](../reports-monitoring/overview-reports.md).

## <a name="determine-your-version-of-pim"></a>Determinar la versión de PIM

Desde noviembre de 2019, la parte de roles de Azure AD de Privileged Identity Management se está actualizando a una nueva versión que coincide con las experiencias de los roles de recursos de Azure. Esta actualización introduce características adicionales y [cambios en la API existente](azure-ad-roles-features.md#api-changes). Mientras se implementa la nueva versión, los procedimientos que seguirá en este artículo dependerán de la versión de Privileged Identity Management que tenga actualmente. Siga los pasos de esta sección para determinar la versión de Privileged Identity Management que tiene. Cuando averigüe la versión de Privileged Identity Management, puede seleccionar los procedimientos de este artículo que coincidan con esa versión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que tenga el rol [Administrador de roles con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator).
1. Abra **Azure AD Privileged Identity Management**. Si tiene un banner en la parte superior de la página de introducción, siga las instrucciones de la pestaña **Nueva versión** de este artículo. De lo contrario, siga las instrucciones de la pestaña **Versión anterior**.

    ![Nueva versión de roles de Azure AD](./media/pim-how-to-add-role-to-user/pim-new-version.png)

# <a name="new-versiontabnew"></a>[Nueva versión](#tab/new)

Siga estos pasos para ver el historial de auditoría de los roles de Azure AD.

## <a name="view-resource-audit-history"></a>Visualización del historial de auditoría de recursos

La auditoría de recursos proporciona una vista de toda la actividad asociada a los roles de Azure AD.

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Roles de Azure AD**.

1. Seleccione **Auditoría de recursos**.

1. Filtre el historial con una fecha predefinida o un intervalo personalizado.

    ![Lista de auditoría de recursos con filtros](media/azure-pim-resource-rbac/rbac-resource-audit.png)

## <a name="view-my-audit"></a>Visualización de mi auditoría

Mi auditoría permite ver la actividad del rol personal.

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Roles de Azure AD**.

1. Seleccione el recurso del que desea ver el historial de auditoría.

1. Seleccione **Mi auditoría**.

1. Filtre el historial con una fecha predefinida o un intervalo personalizado.

    ![Lista de auditoría para el usuario actual](media/azure-pim-resource-rbac/my-audit-time.png)

# <a name="previous-versiontabprevious"></a>[Versión anterior](#tab/previous)

## <a name="view-audit-history"></a>Visualización del historial de auditoría

Siga estos pasos para ver el historial de auditoría de los roles de Azure AD.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que sea miembro del [con Administrador de rol con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator).

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Roles de Azure AD**.

1. Seleccione **Historial de auditoría de roles de directorio**.

    Según el historial de auditoría, se muestra un gráfico de columnas junto con el total de activaciones, el número máximo de activaciones por día y el promedio de activaciones por día.

    ![Directorio de auditoría de roles de directorio](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    En la parte inferior de la página se muestra una tabla con información sobre cada acción del historial de auditoría disponible. Las columnas significan lo siguiente:

    | Columna | Descripción |
    | --- | --- |
    | Time | momento en que se produjo la acción. |
    | Solicitante | usuario que solicitó la activación o el cambio del rol. Si el valor es **Sistema de Azure**, compruebe el historial de auditoría de Azure para más información. |
    | Acción | acciones realizadas por el solicitante. Las acciones pueden incluir Assign, Unassign, Activate, Deactivate o AddedOutsidePIM. |
    | Member | usuario que activa el rol o que está asignado al rol. |
    | Role | rol asignado al usuario o activado por el usuario. |
    | Razonamiento | Texto que escribió en el campo de motivo durante la activación. |
    | Expiration | fecha de expiración de un rol activado. Solo se aplica a las asignaciones de roles aptas. |

1. Para ordenar el historial de auditoría, haga clic en los botones de **tiempo**, **acción** y **rol**.

## <a name="filter-audit-history"></a>Filtrado del historial de auditoría

1. En la parte superior de la página de historial de auditoría, haga clic en el botón **Filtro**.

    Aparecerá el panel **Actualizar parámetros de gráfico**.

1. En **Intervalo de tiempo**, seleccione un intervalo de tiempo.

1. En **Roles**, seleccione las casillas para indicar los roles que desea ver.

    ![Panel Actualizar parámetros de gráfico](media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. Seleccione **Listo** para ver el historial de auditoría filtrado.

---

## <a name="next-steps"></a>Pasos siguientes

- [Visualización de la actividad y del historial de auditoría para los roles de recursos de Azure en Privileged Identity Management](azure-pim-resource-rbac.md)
