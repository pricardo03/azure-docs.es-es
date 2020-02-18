---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: aa9320ab4a2ff28c185ecef0f525376ceab4d875
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170203"
---
|Nombre |Descripción |Efectos |Versión |
|---|---|---|---|
|[Todas las reglas de autorización, excepto RootManageSharedAccessKey, se deben eliminar del espacio de nombres del centro de eventos](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditNamespaceAccessRules_Audit.json) |Los clientes del Centro de eventos no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso en las entidades para que las colas y los temas proporcionen acceso solo a la entidad específica. |Audit, Deny, Disabled |1.0.1 |
|[Las reglas de autorización de la instancia del centro de eventos deben definirse](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditEventHubAccessRules_Audit.json). |Permite auditar la existencia de reglas de autorización en entidades de Event Hub para conceder acceso con privilegios mínimos. |AuditIfNotExists, Disabled |1.0.0 |
|[Los registros de diagnóstico del centro de eventos deben estar habilitados](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditDiagnosticLog_Audit.json) |Permite auditar la habilitación de registros de diagnóstico. Esto le permite volver a crear seguimientos de actividad con fines de investigación en caso de incidente de seguridad o riesgo para la red. |AuditIfNotExists, Disabled |2.0.0 |
