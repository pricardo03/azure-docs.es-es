---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 59a47ca7d3b3b42a5822e61c37ee90cb238c0778
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170053"
---
|Nombre |Descripción |Efectos |Versión |
|---|---|---|---|
|[Se deben habilitar los registros de diagnóstico en las cuentas de Batch](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Batch/Batch_AuditDiagnosticLog_Audit.json) |Permite auditar la habilitación de registros de diagnóstico. Esto le permite volver a crear seguimientos de actividad con fines de investigación en caso de incidente de seguridad o riesgo para la red. |AuditIfNotExists, Disabled |2.0.0 |
|[Las reglas de alerta de métricas deben configurarse en las cuentas de Batch](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Batch/Batch_AuditMetricAlerts_Audit.json) |Permite auditar la configuración de reglas de alertas de métricas en una cuenta de Batch para habilitar la métrica requerida. |AuditIfNotExists, Disabled |1.0.0 |
