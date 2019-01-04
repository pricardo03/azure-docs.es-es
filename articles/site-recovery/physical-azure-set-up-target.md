---
title: Configuración del entorno de destino para la recuperación ante desastres de servidores físicos locales en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar el entorno de destino de Azure para la recuperación ante desastres de servidores físicos mediante Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 43276aad26bc06400c1bc4b5feaace0d5646c213
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849248"
---
# <a name="prepare-target-vmware-to-azure"></a>Preparación del destino (VMware a Azure)

En este artículo se describe cómo preparar el entorno de Azure para comenzar a replicar servidores físicos (x64) que ejecutan Windows o Linux en Azure.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por supuesto lo siguiente:
- Ha creado un almacén de Recovery Services para proteger sus servidores físicos. Puede crear un almacén de Recovery Services desde [Azure Portal](http://portal.azure.com "Azure Portal").
- Ha [configurado el entorno local](physical-azure-disaster-recovery.md) para replicar servidores físicos en Azure.

## <a name="prepare-target"></a>Preparación del destino

Después de completar el **Paso 1: Selección del objetivo de protección** y el **Paso 2: Preparación del origen**, irá al **Paso 3: Destino**

![Preparación del destino](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Suscripción:** en el menú desplegable, seleccione la suscripción en la que desea replicar los servidores físicos.
2. **Modelo de implementación:** seleccione el modelo de implementación (clásico o Resource Manager)

Según el modelo de implementación elegido, se ejecuta una validación para asegurarse de que tiene al menos una cuenta de almacenamiento compatible y una red virtual en la suscripción de destino en la que replicar y conmutar por error los servidores físicos.

Una vez completadas las validaciones correctamente, haga clic en Aceptar para ir al paso siguiente.

Si no tiene una cuenta de almacenamiento de Resource Manager compatible o una red virtual, puede crearla haciendo clic en los botones **+ Cuenta de almacenamiento** o **+ Red** en la parte superior de la página.

## <a name="next-steps"></a>Pasos siguientes
[Configuración de las opciones de replicación](vmware-azure-set-up-replication.md).
