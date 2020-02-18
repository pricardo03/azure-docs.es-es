---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 7aa6abb1a5fa9c969ca5e6d0730bed3b461fe81b
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170293"
---
|Nombre |Descripción |Efectos |Versión |
|---|---|---|---|
|[SKU permitidas de cuentas de almacenamiento](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/AllowedStorageSkus_Deny.json) |Esta directiva permite especificar un conjunto de SKU de cuenta de almacenamiento que su organización puede implementar. |Denegar |1.0.0 |
|[Auditar el acceso de red sin restricciones a cuentas de almacenamiento](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |Audite el acceso de red no restringido en la configuración de firewall de su cuenta de almacenamiento. Si se configuran reglas de red, solo las aplicaciones de redes permitidas pueden acceder a la cuenta de almacenamiento. Para permitir la conexión desde clientes específicos locales o de Internet, se puede conceder acceso al tráfico procedente de redes virtuales de Azure específicas o a intervalos de direcciones IP de Internet públicas. |Audit, Disabled |1.0.0 |
|[Implementar Advanced Threat Protection en las cuentas de almacenamiento](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/StorageAdvancedThreatProtection_Deploy.json) |Esta directiva habilita Advanced Threat Protection en las cuentas de almacenamiento. |DeployIfNotExists, Disabled |1.0.0 |
|[El almacenamiento con redundancia geográfica debe estar habilitado para las cuentas de almacenamiento](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/GeoRedundant_StorageAccounts_Audit.json) |Esta directiva audita las cuentas de almacenamiento que no tienen habilitado el almacenamiento con redundancia geográfica. |Audit, Disabled |1.0.0 |
|[Se debe habilitar la transferencia segura a las cuentas de almacenamiento](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |Permite auditar el requisito de transferencia segura en la cuenta de almacenamiento. La transferencia segura es una opción que obliga a la cuenta de almacenamiento a aceptar solamente solicitudes de conexiones seguras (HTTPS). El uso de HTTPS garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como los de tipo "Man in the middle", interceptación y secuestro de sesión |Audit, Deny, Disabled |1.0.0 |
|[Las cuentas de almacenamiento deben permitir el acceso desde los servicios de Microsoft de confianza](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/StorageAccess_TrustedMicrosoftServices_Audit.json) |Algunos servicios de Microsoft que interactúan con las cuentas de almacenamiento funcionan desde redes a las que no se puede conceder acceso a través de reglas de red. Para ayudar a que este tipo de servicio funcione según lo previsto, permita que el conjunto de servicios de Microsoft de confianza omita las reglas de red. Estos servicios usarán luego una autenticación sólida para acceder a la cuenta de almacenamiento. |Audit, Deny, Disabled |1.0.0 |
|[Se deben migrar las cuentas de almacenamiento a los nuevos recursos de Azure Resource Manager](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Classic_AuditForClassicStorages_Audit.json) |Use el nuevo Azure Resource Manager para las cuentas de almacenamiento a fin de proporcionar mejoras de seguridad como las siguientes: mayor control de acceso (RBAC), mejor auditoría, gobernanza e implementación basados en Azure Resource Manager, acceso a las identidades administradas, acceso a los secretos de Key Vault, autenticación basada en Azure AD y compatibilidad con las etiquetas y los grupos de recursos para facilitar la administración de seguridad. |Audit, Deny, Disabled |1.0.0 |
