---
title: Traslado de máquinas virtuales de Azure a una nueva suscripción o grupo de recursos | Microsoft Docs
description: Use Azure Resource Manager para trasladar máquinas virtuales a un nuevo grupo de recursos o a una nueva suscripción.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 095ed1c8d2328b1eb391042125526696ba8cda49
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723368"
---
# <a name="move-guidance-for-virtual-machines"></a>Guía del traslado de máquinas virtuales

En este artículo se describen los escenarios que actualmente no se admiten y los pasos para trasladar máquinas virtuales con copia de seguridad.

## <a name="scenarios-not-supported"></a>Escenarios no admitidos

Todavía no se admiten los siguientes escenarios:

* Las instancias de Managed Disks en Availability Zones no se pueden trasladar a una suscripción diferente.
* Los recursos de Virtual Machines con certificado almacenados en Key Vault se pueden trasladar al nuevo grupo de recursos en la misma suscripción, pero no entre suscripciones.
* No es posible trasladar Virtual Machine Scale Sets con equilibrador de carga o IP pública de SKU estándar.
* Las máquinas virtuales creadas a partir de recursos de Marketplace con planes adjuntos no se pueden mover entre suscripciones o grupos de recursos. Desaprovisione el recurso en la suscripción activa y vuelva a implementarlo en la nueva suscripción.
* Las máquinas virtuales de una red virtual existente, pero no tiene intención de trasladar todos los recursos en la red virtual.

## <a name="virtual-machines-with-azure-backup"></a>Máquinas virtuales con Azure Backup

Para mover máquinas virtuales configuradas con la copia de seguridad de Azure, use la siguiente solución alternativa:

* Busque la ubicación de la máquina virtual.
* Busque un grupo de recursos con el patrón de nombres siguiente: `AzureBackupRG_<location of your VM>_1`, por ejemplo, AzureBackupRG_westus2_1
* Si está en Azure Portal, active "Mostrar tipos ocultos"
* Si se encuentra en PowerShell, use el cmdlet `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1`
* Si está en la CLI, use `az resource list -g AzureBackupRG_<location of your VM>_1`
* Busque el recurso con el tipo `Microsoft.Compute/restorePointCollections` que tiene el patrón de nombres `AzureBackup_<name of your VM that you're trying to move>_###########`
* Elimine este recurso. Esta operación elimina solo los puntos de recuperación instantáneos, no los datos de copia de seguridad que se encuentran en el almacén.
* Después de finalizar la eliminación, puede trasladar el almacén y la máquina virtual a la suscripción de destino. Tras el traslado, puede continuar realizando las copias de seguridad sin pérdida de datos.
* Para más información sobre cómo mover los almacenes de Recovery Services para realizar copias de seguridad, consulte [Limitaciones de Recovery Services](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="next-steps"></a>Pasos siguientes

Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../resource-group-move-resources.md).
