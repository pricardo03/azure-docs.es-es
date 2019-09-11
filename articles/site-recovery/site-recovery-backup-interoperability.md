---
title: Compatibilidad para usar Azure Site Recovery con Azure Backup
description: Proporciona información general sobre cómo Azure Site Recovery y Azure Backup se pueden usar conjuntamente.
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: sideeksh
ms.openlocfilehash: 2078aa33ca735b91b5923d94028a286427f107a6
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70146869"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Compatibilidad para usar Site Recovery con Azure Backup

En este artículo se resume la compatibilidad para usar el [servicio Site Recovery](site-recovery-overview.md) junto con el [servicio Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview).

**Acción** | **Compatibilidad de Site Recovery** | **Detalles**
--- | --- | ---
**Implementación conjunta de servicios** | Compatible | Los servicios son interoperables y se pueden configurar juntos.
**Copia de seguridad/restauración de archivos** | Compatible | Cuando la copia de seguridad y la replicación están habilitadas para una VM y se realizan copias de seguridad, no hay ningún problema en la restauración de archivos en las VM de origen ni en el grupo de VM. La replicación continúa como de costumbre sin ningún cambio en el estado de la replicación.
**Copia de seguridad/restauración del disco** | No compatible actualmente | Si restaura un disco del que se ha realizado una copia de seguridad, deberá deshabilitar la replicación y luego volver a habilitarla para la VM.
**Copia de seguridad/restauración de la VM** | No compatible actualmente | Si la copia de seguridad o la restauración de una VM o un grupo de VM, deberá deshabilitar y volver a habilitar la replicación para la VM.  


