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
ms.date: 10/22/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c4a157d8d5bcd281ca9fee488e58c455034e898
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74022063"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>Visualización del historial de auditoría de los roles de Azure AD en PIM

Puede utilizar el historial de auditoría de Privileged Identity Management (PIM) para ver todas las asignaciones de roles y las activaciones de los últimos 30 días para todos los roles con privilegios. Si quiere ver el historial de auditoría completo de la actividad de la organización de Azure Active Directory (Azure AD), incluidos el administrador, el usuario final y la actividad de sincronización, puede usar los [informes de actividad y seguridad de Azure Active Directory](../reports-monitoring/overview-reports.md).

## <a name="view-audit-history"></a>Visualización del historial de auditoría

Siga estos pasos para ver el historial de auditoría de los roles de Azure AD.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con un usuario que sea miembro del [con Administrador de rol con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de Azure AD**.

1. Haga clic en **Historial de auditoría de roles de directorio**.

    Según el historial de auditoría, se muestra un gráfico de columnas junto con el total de activaciones, el número máximo de activaciones por día y el promedio de activaciones por día.

    ![Directorio de auditoría de roles de directorio](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    En la parte inferior de la página se muestra una tabla con información sobre cada acción del historial de auditoría disponible. Las columnas significan lo siguiente:

    | Columna | DESCRIPCIÓN |
    | --- | --- |
    | Hora | momento en que se produjo la acción. |
    | Solicitante | usuario que solicitó la activación o el cambio del rol. Si el valor es **Sistema de Azure**, compruebe el historial de auditoría de Azure para más información. |
    | . | acciones realizadas por el solicitante. Las acciones pueden incluir Assign, Unassign, Activate, Deactivate o AddedOutsidePIM. |
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

1. Haga clic en **Listo** para ver el historial de auditoría filtrado.

## <a name="next-steps"></a>Pasos siguientes

- [Visualización de la actividad y del historial de auditoría para los roles de recursos de Azure en Privileged Identity Management](azure-pim-resource-rbac.md)
