---
title: Auditar roles de recurso para los recursos de Azure mediante Privileged Identity Management | Microsoft Docs
description: Se explica cómo obtener una vista de toda la actividad de rol para un recurso determinado.
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
ms.openlocfilehash: 8f328a609d696886ed452589d3cdfb5f45aec62a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621290"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Auditar roles de recurso para los recursos de Azure mediante Privileged Identity Management 

Auditoría de recursos proporciona una vista de todas las actividades de rol del recurso. Puede filtrar la información con una fecha predefinida o un intervalo personalizado.
![Filtrar información](media/azure-pim-resource-rbac/rbac-resource-audit.png)

La auditoría de recursos también proporciona acceso rápido a los detalles de la actividad de un usuario. En **Tipo de auditoría**, seleccione **Activar**. Seleccione **(actividad)** para ver las acciones del usuario en los recursos de Azure.
![Detalles de actividad](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Más detalles de actividad](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Mi auditoría

Mi auditoría proporciona una vista de la actividad personal de roles de un usuario. Puede filtrar la información con una fecha predefinida o un intervalo personalizado.
![Actividad de un rol personal](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Consulta de la activación y la actividad de los recursos de Azure

Para ver las acciones que realizó un usuario específico en varios recursos, puede revisar la actividad de los recursos de Azure asociada a un período de activación específico. Primero, seleccione el usuario de la vista **Miembros** o de la lista de miembros de un rol específico. El resultado muestra una vista gráfica de las acciones del usuario en los recursos de Azure por fecha. También muestra las activaciones de roles recientes durante ese mismo período de tiempo.

![Detalles del usuario](media/azure-pim-resource-rbac/rbac-user-details.png)

Al seleccionar la activación de un rol específico se muestran los detalles de la activación del rol y la actividad de los recursos de Azure correspondiente al tiempo de actividad de ese usuario.

![Seleccionar activación de rol](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

