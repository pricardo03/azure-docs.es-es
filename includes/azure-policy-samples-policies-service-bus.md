---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/05/2020
ms.author: dacoulte
ms.openlocfilehash: 79820d32b4f86d25fa6abe060b43bf6ce587dc37
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78668704"
---
|Nombre |Descripción |Efectos |Versión |GitHub |
|---|---|---|---|---|
|[Todas las reglas de autorización, excepto RootManageSharedAccessKey, se deben eliminar del espacio de nombres de Service Bus](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa1817ec0-a368-432a-8057-8371e17ac6ee) |Los clientes de Service Bus no deben usar una directiva de acceso de nivel de espacio de nombres que proporciona acceso a todas las colas y temas de un espacio de nombres. Para alinearse con el modelo de seguridad con privilegios mínimos, debe crear directivas de acceso en las entidades para que las colas y los temas proporcionen acceso solo a la entidad específica. |Audit, Deny, Disabled |1.0.1 |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditNamespaceAccessRules_Audit.json)
|[Los registros de diagnóstico en Service Bus deben estar habilitados](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff8d36e2f-389b-4ee4-898d-21aeb69a0f45) |Permite auditar la habilitación de registros de diagnóstico. Esto le permite volver a crear seguimientos de actividad con fines de investigación en caso de incidente de seguridad o riesgo para la red. |AuditIfNotExists, Disabled |2.0.0 |[Vínculo](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditDiagnosticLog_Audit.json)
