---
title: Visualización del historial de auditoría para los roles de recurso de Azure en PIM | Microsoft Docs
description: Aprenda a visualizar el historial de auditoría para los roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: c0536423e9640f78149b612ec66b0a07cdcf24bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189318"
---
# <a name="view-audit-history-for-azure-resource-roles-in-pim"></a>Visualización del historial de auditoría para los roles de recurso de Azure en PIM

Auditoría de recursos proporciona una vista de todas las actividades de rol del recurso. Puede filtrar la información con una fecha predefinida o un intervalo personalizado.
![Filtrar información](media/azure-pim-resource-rbac/rbac-resource-audit.png)

La auditoría de recursos también proporciona acceso rápido a los detalles de la actividad de un usuario. En **Tipo de auditoría**, seleccione **Activar**. Seleccione **(actividad)** para ver las acciones del usuario en los recursos de Azure.
![Detalles de actividad](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Más detalles de actividad](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="my-audit"></a>Mi auditoría

Mi auditoría proporciona una vista de la actividad personal de roles de un usuario. Puede filtrar la información con una fecha predefinida o un intervalo personalizado.
![Actividad de un rol personal](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Visualización de la activación y de la actividad de los recursos de Azure

Para ver las acciones que realizó un usuario específico en varios recursos, puede revisar la actividad de los recursos de Azure asociada a un período de activación específico. Primero, seleccione el usuario de la vista **Miembros** o de la lista de miembros de un rol específico. El resultado muestra una vista gráfica de las acciones del usuario en los recursos de Azure por fecha. También muestra las activaciones de roles recientes durante ese mismo período de tiempo.

![Detalles del usuario](media/azure-pim-resource-rbac/rbac-user-details.png)

Al seleccionar la activación de un rol específico se muestran los detalles de la activación del rol y la actividad de los recursos de Azure correspondiente al tiempo de actividad de ese usuario.

![Seleccionar activación de rol](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="next-steps"></a>Pasos siguientes

- [Visualización del historial de auditorías de los roles de directorios de Azure AD en PIM](pim-how-to-use-audit-log.md)
