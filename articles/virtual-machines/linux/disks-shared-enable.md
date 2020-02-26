---
title: Habilitación de los discos compartidos para Azure Managed Disks
description: Configuración de un disco administrado de Azure con discos compartidos (versión preliminar) para poder compartirlo en varias VM
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 4562567aea667dd7efadbcfb033e782ec9418e3f
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77472067"
---
# <a name="enable-shared-disk"></a>Habilitación del disco compartido

En este artículo se describe cómo habilitar la característica de discos compartidos (versión preliminar) para Azure Managed Disks. Los discos compartidos de Azure (versión preliminar) son una nueva característica de Azure Managed Disks que permite adjuntar un disco administrado en varias máquinas virtuales (VM) al mismo tiempo. Si adjunta un disco administrado en varias VM, podrá implementar nuevas aplicaciones en clúster o migrar las existentes a Azure. 

Si busca información conceptual sobre los discos administrados que tienen habilitados discos compartidos, consulte [Discos compartidos de Azure](disks-shared.md).
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]