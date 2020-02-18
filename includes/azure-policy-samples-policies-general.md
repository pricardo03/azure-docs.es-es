---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 8319ec2bf3965ee30db3e7d4ba1346bd7a3dd7d4
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169892"
---
|Nombre |Descripción |Efectos |Versión |
|---|---|---|---|
|[Ubicaciones permitidas](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/AllowedLocations_Deny.json) |Esta directiva permite restringir las ubicaciones que la organización puede especificar al implementar los recursos. Úsela para aplicar los requisitos de cumplimiento de replicación geográfica. Excluye los grupos de recursos, Microsoft.AzureActiveDirectory/b2cDirectories, y recursos que usan la región "global". |deny |1.0.0 |
|[Ubicaciones permitidas para grupos de recursos](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/ResourceGroupAllowedLocations_Deny.json) |Esta directiva permite restringir las ubicaciones en las que la organización puede crear grupos de recursos. Úsela para aplicar los requisitos de cumplimiento de replicación geográfica. |deny |1.0.0 |
|[Tipos de recursos permitidos](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/AllowedResourceTypes_Deny.json) |Esta directiva le permite especificar los tipos de recursos que puede implementar su organización. La directiva solo se aplicará a los tipos de recursos que admitan los valores "tags" y "location". Para restringir todos los recursos, duplique esta directiva y cambie el valor de "mode" a "All". |deny |1.0.0 |
|[La ubicación del recurso de auditoría coincide con la del grupo de recursos](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/ResourcesInResourceGroupLocation_Audit.json) |Auditoría cuya ubicación de recursos coincide con la ubicación de su grupo de recursos. |auditoría |1.0.0 |
|[Auditar el uso de reglas de RBAC personalizadas](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json) |Permite auditar roles integrados, como "propietario, colaborador, lector" en lugar de roles RBAC personalizados, que son propensos a errores de auditoría. El uso de roles personalizados se trata como una excepción y requiere una revisión rigurosa y el modelado de amenazas. |Audit, Disabled |1.0.0 |
|[No deben existir roles de propietario de suscripción personalizados](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json). |Esta directiva garantiza que no existe ningún rol de propietario de suscripción personalizado. |Audit, Disabled |1.0.0 |
|[Tipos de recursos no permitidos](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/InvalidResourceTypes_Deny.json) |Esta directiva permite especificar los tipos de recursos que su organización no puede implementar. |Denegar |1.0.0 |
