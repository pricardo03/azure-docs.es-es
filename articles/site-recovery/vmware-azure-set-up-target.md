---
title: Preparación del entorno de destino para la replicación de VMware en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar el entorno de Azure de destino para la replicación de máquinas virtuales de VMware en Azure.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: cbf9c1a49481de49fb8d9c6599acec7bc3600df3
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837518"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Preparación del entorno de destino para la recuperación ante desastres de máquinas virtuales de VMware o servidores físicos en Azure

En este artículo se describe cómo configurar el entorno de Azure de destino para empezar a replicar máquinas virtuales de VMware o servidores físicos en Azure.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por supuesto lo siguiente:
- Ha creado un almacén de Recovery Services en [Azure Portal](http://portal.azure.com "Azure Portal") para proteger las máquinas de origen.
- Ha configurado el entorno local para replicar las [máquinas virtuales de VMware](vmware-azure-set-up-source.md) o los [servidores físicos](physical-azure-set-up-source.md) en Azure.

## <a name="prepare-target"></a>Preparación del destino

Después de completar el **Paso 1: Selección del objetivo de protección** y el **Paso 2: Preparación del origen**, se le dirigirá al **Paso 3: Destino**

![Preparación del destino](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Suscripción:** en el menú desplegable, seleccione la suscripción en la que quiere replicar las máquinas virtuales o los servidores físicos.
2. **Modelo de implementación:** seleccione el modelo de implementación (clásico o Resource Manager)

Según el modelo de implementación elegido, se ejecuta una validación para asegurarse de que tiene al menos una cuenta de almacenamiento compatible y una red virtual en la conmutación de destino en la que replicar y conmutar por error las máquinas virtuales o los servidores físicos.

Una vez completadas las validaciones correctamente, haga clic en Aceptar para ir al paso siguiente.

Si no tiene una cuenta de almacenamiento de Resource Manager compatible o una red virtual, puede crearla haciendo clic en los botones **+ Cuenta de almacenamiento** o **+ Red** en la parte superior de la página.

## <a name="next-steps"></a>Pasos siguientes
[Configuración de las opciones de replicación](vmware-azure-set-up-replication.md).
