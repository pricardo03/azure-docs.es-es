---
title: Plantillas de Azure Resource Manager para Azure Backup
description: Ejemplos de Azure Backup PowerShell
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 04/18/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: ffd153490938c232bdf8b5d4e40a2d43ced9f57b
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063276"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Plantillas de Azure Resource Manager para Azure Backup

En la tabla siguiente se incluyen vínculos a las plantillas de Resource Manager para su uso con los almacenes de Recovery Services y las características de Azure Backup. Para obtener información sobre la sintaxis y las propiedades de JSON, consulte [Tipos de recursos Microsoft.RecoveryServices](/azure/templates/microsoft.recoveryservices/allversions).

|   |   |
|---|---|
|**Almacén de Recovery Services** | |
| [Creación de un almacén de Recovery Services](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Cree un almacén de Recovery Services. El almacén se puede usar con Azure Backup y Azure Site Recovery. |
|**Copia de seguridad de máquinas virtuales**| |
| [Copia de seguridad de máquinas virtuales de Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Use el almacén de Recovery Services existente y la directiva de Backup para hacer una copia de seguridad de las máquinas virtuales de Resource Manager en el mismo grupo de recursos.|
| [Copia de seguridad de máquinas virtuales de IaaS en el almacén de Recovery Services](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Plantilla para realizar la copia de seguridad de máquinas virtuales de los modelos clásico y de Resource Manager. |
| [Creación de una directiva de copia de seguridad semanal para máquinas virtuales de IaaS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | La plantilla crea un almacén de Recovery Services y una directiva de copia de seguridad semanal que se usa para realizar la copia de seguridad de máquinas virtuales de los modelos clásico y de Resource Manager.|
| [Creación de una directiva de copia de seguridad diaria para máquinas virtuales de IaaS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | La plantilla crea un almacén de Recovery Services y una directiva de copia de seguridad diaria que se usa para realizar la copia de seguridad de máquinas virtuales de los modelos clásico y de Resource Manager.|
| [Implementación de una máquina virtual de Windows Server con copia de seguridad habilitada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | La plantilla crea una máquina virtual de Windows Server y un almacén de Recovery Services con la directiva de copia de seguridad predeterminada habilitada.|
|**Supervisión de trabajos de copia de seguridad** |  |
| [Uso de Log Analytics para supervisar Azure Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | La plantilla implementa la supervisión de Log Analytics para Azure Backup lo cual le permite supervisar los trabajos de copia de seguridad y restauración, las alertas de copia de seguridad y el almacenamiento en la nube que se usa en los almacenes de Recovery Services.|  
|   |   |

