---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 53494bc9642a3df91492d2353a6aa3c6875c1fff
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170133"
---
|Nombre |Descripción |Efectos |Versión |
|---|---|---|---|
|[Implementar la configuración de diagnóstico para Key Vault en el Centro de eventos](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Key%20Vault/KeyVault_DiagnosticLog_Deploy.json) |Implementa la configuración de diagnóstico para que Key Vault se transmita a un Centro de eventos regional cuando se cree o actualice cualquier instancia de Key Vault a la que falte esta configuración de diagnóstico. |deployIfNotExists |1.0.0 |
|[Los registros de diagnóstico en Key Vault deben estar habilitados](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Key%20Vault/KeyVault_AuditDiagnosticLog_Audit.json) |Permite auditar la habilitación de registros de diagnóstico. Esto le permite volver a crear seguimientos de actividad con fines de investigación en caso de incidente de seguridad o riesgo para la red. |AuditIfNotExists, Disabled |1.0.0 |
|[Los objetos del almacén de claves deben poder recuperarse](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Key%20Vault/KeyVault_Recoverable_Audit.json) |Esta directiva audita si los objetos de Key Vault no se pueden recuperar. La característica de eliminación temporal ayuda a mantener de forma eficaz los recursos durante un período de retención determinado (90 días) incluso después de una operación de eliminación y, al mismo tiempo, ofrece la apariencia de que el objeto se ha eliminado. Cuando la protección de purgas está activada, un almacén o un objeto en estado eliminado no se puede purgar hasta que ha transcurrido el período de retención de 90 días. Estos almacenes y objetos todavía se pueden recuperar, lo garantiza a los clientes que se seguirá la directiva de retención. |Audit, Disabled |1.0.0 |
