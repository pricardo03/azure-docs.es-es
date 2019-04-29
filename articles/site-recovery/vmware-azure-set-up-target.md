---
title: Preparación del entorno de destino para la replicación de VMware en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar el entorno de Azure de destino para la replicación de máquinas virtuales de VMware en Azure.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/03/2019
ms.author: mayg
ms.openlocfilehash: e75d4b1701944e206fcf6ded2dcb6d1e1fbc77cb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60723821"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Preparación del entorno de destino para la recuperación ante desastres de máquinas virtuales de VMware o servidores físicos en Azure

En este artículo se describe cómo configurar el entorno de Azure de destino para empezar a replicar máquinas virtuales de VMware o servidores físicos en Azure.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por supuesto lo siguiente:
- Ha creado un almacén de Recovery Services en [Azure Portal](https://portal.azure.com "Azure Portal") para proteger las máquinas de origen.
- Ha configurado el entorno local para replicar las [máquinas virtuales de VMware](vmware-azure-set-up-source.md) o los [servidores físicos](physical-azure-set-up-source.md) en Azure.

## <a name="prepare-target"></a>Preparación del destino

Después de completar el **Paso 1: Selección del objetivo de protección** y el **Paso 2: Preparación del origen**, se le dirigirá al **Paso 3: Destino**

![Preparación del destino](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Suscripción:** en el menú desplegable, seleccione la suscripción en la que quiere replicar las máquinas virtuales o los servidores físicos.
2. **Modelo de implementación:** seleccione el modelo de implementación (clásico o Resource Manager)

Según el modelo de implementación elegido, se ejecuta una validación para garantizar que tengan al menos una red virtual en la suscripción de destino para la replicación y conmutación por error la máquina virtual o servidor físico en.

Una vez completadas las validaciones correctamente, haga clic en Aceptar para ir al paso siguiente.

Si no tiene una red virtual, puede crearla haciendo clic en el **+ red** situado en la parte superior de la página.

## <a name="next-steps"></a>Pasos siguientes
[Configuración de las opciones de replicación](vmware-azure-set-up-replication.md).
