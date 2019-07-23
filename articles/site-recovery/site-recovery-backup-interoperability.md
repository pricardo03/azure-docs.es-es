---
title: Compatibilidad para usar Azure Site Recovery con Azure Backup | Microsoft Docs
description: Proporciona información general sobre cómo Azure Site Recovery y Azure Backup se pueden usar conjuntamente.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2019
ms.author: sideeksh
ms.openlocfilehash: e902f70225ec0eb0caa98f7e19a16c87220cb6f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61035782"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Compatibilidad para usar Site Recovery con Azure Backup

En este artículo se resume la compatibilidad para usar el [servicio Site Recovery](site-recovery-overview.md) junto con el [servicio Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview).

**Acción** | **Compatibilidad de Site Recovery** | **Detalles**
--- | --- | ---
**Implementación conjunta de servicios** | Compatible | Los servicios son interoperables y se pueden configurar juntos.
**Copia de seguridad/restauración de archivos** | Compatible | Cuando la copia de seguridad y la replicación están habilitadas para una VM y se realizan copias de seguridad, no hay ningún problema en la restauración de archivos en las VM de origen ni en el grupo de VM. La replicación continúa como de costumbre sin ningún cambio en el estado de la replicación.
**Copia de seguridad/restauración del disco** | No compatible actualmente | Si restaura un disco del que se ha realizado una copia de seguridad, deberá deshabilitar la replicación y luego volver a habilitarla para la VM.
**Copia de seguridad/restauración de la VM** | No compatible actualmente | Si la copia de seguridad o la restauración de una VM o un grupo de VM, deberá deshabilitar y volver a habilitar la replicación para la VM.  


