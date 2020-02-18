---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 667d017e8febcf1abcc1cfe21ecfaa43ae75ea6d
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170103"
---
|Nombre |Descripción |Efectos |Versión |
|---|---|---|---|
|[SKU de máquina virtual permitidas](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMSkusAllowed_Deny.json). |Esta directiva permite especificar un conjunto de SKU de máquina virtual que su organización puede implementar. |Denegar |1.0.0 |
|[Auditoría de máquinas virtuales sin la recuperación ante desastres configurada](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/RecoveryServices_DisasterRecovery_Audit.json) |Audita las máquinas virtuales que no tienen configurada la recuperación ante desastres. Para más información acerca de la recuperación ante desastres, visite https://aka.ms/asr-doc. |auditIfNotExists |1.0.0 |
|[Auditar las máquinas virtuales que no utilizan discos administrados](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMRequireManagedDisk_Audit.json) |Esta directiva audita las máquinas virtuales que no utilizan discos administrados. |auditoría |1.0.0 |
|[Implementar la extensión IaaSAntimalware predeterminada de Microsoft para Windows Server](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMAntimalwareExtension_Deploy.json) |Esta directiva implementa una extensión de Microsoft IaaSAntimalware con una configuración predeterminada cuando una VM no está configurada con la extensión de antimalware. |deployIfNotExists |1.0.0 |
|[Los registros de diagnóstico de Virtual Machine Scale Sets deben estar habilitados](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/ServiceFabric_and_VMSS_AuditVMSSDiagnostics.json) |Se recomienda habilitar los registros para que ese seguimiento de actividad se pueda volver a crear cuando se necesiten investigaciones en caso de incidente o riesgo. |AuditIfNotExists, Disabled |1.0.0 |
|[Microsoft Antimalware para Azure debe estar configurado para actualizar automáticamente las firmas de protección](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VirtualMachines_AntiMalwareAutoUpdate_AuditIfNotExists.json). |Esta directiva audita cualquier máquina virtual de Windows que no esté configurada con la actualización automática de firmas de protección de Microsoft Antimalware. |AuditIfNotExists, Disabled |1.0.0 |
|[La extensión IaaSAntimalware de Microsoft debe implementarse en servidores Windows](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/WindowsServers_AntiMalware_AuditIfNotExists.json). |Esta directiva audita cualquier VM de Windows Server sin la extensión IaaSAntimalware de Microsoft implementada. |AuditIfNotExists, Disabled |1.0.0 |
|[Solo deben instalarse las extensiones de máquina virtual aprobadas](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VirtualMachines_ApprovedExtensions_Audit.json) |Esta directiva rige las extensiones de máquina virtual que no están aprobadas. |Audit, Deny, Disabled |1.0.0 |
|[Exigir la aplicación automática de revisiones de imágenes del sistema operativo en Virtual Machine Scale Sets](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/VMSSOSUpgradeHealthCheck_Deny.json) |Esta directiva requiere que se habilite la aplicación automática de revisiones de imagen de sistema operativo en Virtual Machine Scale Sets para que se apliquen mensualmente las revisiones de seguridad más recientes con el fin de que las máquinas virtuales estén siempre protegidas. |deny |1.0.0 |
|[Los discos sin asignar deben cifrarse](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/UnattachedDisk_Encryption_Audit.json) |Esta directiva audita cualquier disco sin adjuntar y que no tenga el cifrado habilitado. |Audit, Disabled |1.0.0 |
|[Se deben migrar las máquinas virtuales a nuevos recursos de Azure Resource Manager](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Compute/ClassicCompute_Audit.json) |Use el nuevo Azure Resource Manager para sus máquinas virtuales para proporcionar mejoras de seguridad como las siguientes: mayor control de acceso (RBAC), mejor auditoría, gobernanza e implementación basados en ARM, acceso a identidades administradas, acceso a secretos de Key Vault, autenticación basada en Azure AD y compatibilidad con etiquetas y grupos de recursos para facilitar la administración de seguridad. |Audit, Deny, Disabled |1.0.0 |
