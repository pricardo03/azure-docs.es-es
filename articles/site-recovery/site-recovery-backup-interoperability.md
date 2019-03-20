---
title: 'Azure Site Recovery: interoperabilidad de copia de seguridad | Microsoft Docs'
description: Proporciona información general sobre cómo Azure Site Recovery y Azure Backup se pueden usar conjuntamente.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/26/2019
ms.author: sideeksh
ms.openlocfilehash: 6658ab8c967c70ac1deaeba3d1dfeac602515591
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2019
ms.locfileid: "57731869"
---
# <a name="about-site-recovery-and-backup-interoperability"></a>Acerca de la interoperabilidad de copia de seguridad y recuperación de sitio

Este artículo proporcionan instrucciones para usar correctamente la recuperación ante desastres de copia de seguridad de máquina virtual de IaaS de Azure y máquina virtual de Azure.

## <a name="azure-backup"></a>Azure Backup

Azure Backup ayuda a proteger los datos de servidores locales, máquinas virtuales, cargas de trabajo virtualizadas, servidores SQL Server, servidores de SharePoint y mucho más. Azure Site Recovery organiza y administra la recuperación ante desastres para máquinas virtuales de Azure, las máquinas virtuales locales y servidores físicos.

## <a name="azure-site-recovery"></a>Azure Site Recovery

Es posible configurar la copia de seguridad de Azure y Azure Site Recovery en una máquina virtual o un grupo de máquinas virtuales. Ambos productos son interoperables. Algunos escenarios donde es importante la interoperabilidad entre las copias de seguridad y Azure Site Recovery son los siguientes:

### <a name="file-backuprestore"></a>Copia de seguridad/restauración de archivos

Si la copia de seguridad y replicación son ambos habilitados, y se realiza una copia de seguridad, no hay ningún problema con la restauración de los archivos en la máquina virtual del origen o el grupo de máquinas virtuales. La replicación continuará como de costumbre con ningún cambio en el estado de replicación.

### <a name="disk-backuprestore"></a>Copia de seguridad y restauración de disco

Si Restaurar el disco de la copia de seguridad de protección de la máquina virtual debe habilitarse de nuevo.

### <a name="vm-backuprestore"></a>Copia de seguridad/restauración de máquinas virtuales

No se admite la copia de seguridad y restauración de una máquina virtual o un grupo de máquinas virtuales. Protección para que funcione, debe habilitarse de nuevo.

**Escenario** | **¿Compatible con Azure Site Recovery?** | **Solución alternativa, si existe**  
--- | --- | ---
Copia de seguridad del archivo o carpeta | Sí | No aplicable
Copia de seguridad de disco | No actualmente | Deshabilitar y habilitar la protección
Copia de seguridad de máquina virtual | Sin  | Deshabilitar y habilitar la protección
