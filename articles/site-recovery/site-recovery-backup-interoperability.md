---
title: Compatibilidad con el uso de Azure Site Recovery con Azure Backup | Microsoft Docs
description: Proporciona información general sobre cómo Azure Site Recovery y Azure Backup se pueden usar conjuntamente.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2019
ms.author: sideeksh
ms.openlocfilehash: e902f70225ec0eb0caa98f7e19a16c87220cb6f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035782"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Compatibilidad con el uso de Site Recovery con Azure Backup

En este artículo se resume la compatibilidad para usar el [servicio Site Recovery](site-recovery-overview.md) junto con el [servicio Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview).

**Acción** | **Compatibilidad de Site Recovery** | **Detalles**
--- | --- | ---
**Implementar servicios juntos** | Compatible | Los servicios son interoperables y se pueden configurar.
**Copia de seguridad/restauración de archivos** | Compatible | Cuando copia de seguridad y la replicación están habilitadas para una máquina virtual y las copias de seguridad, no hay ningún problema en la restauración de archivos en el lado del origen de las máquinas virtuales, o grupo de máquinas virtuales. La replicación continúa como de costumbre con ningún cambio en el estado de replicación.
**Copia de seguridad y restauración de disco** | No se admite actual | Si restaura un copia de seguridad de disco, deberá deshabilitar y volver a habilitar la replicación para la máquina virtual de nuevo.
**Copia de seguridad/restauración de máquinas virtuales** | No se admite actual | Si la copia de seguridad o restaurar una máquina virtual o un grupo de máquinas virtuales, deberá deshabilitar y volver a habilitar la replicación para la máquina virtual.  


