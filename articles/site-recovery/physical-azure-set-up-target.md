---
title: Configuración del entorno de destino para la recuperación ante desastres de servidores físicos locales en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar el entorno de destino de Azure para la recuperación ante desastres de servidores físicos mediante Azure Site Recovery.
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: b89d04a6e2fd11a61de8b56690664f6204c208ad
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209299"
---
# <a name="prepare-target-vmware-to-azure"></a>Preparación del destino (VMware a Azure)

En este artículo se describe cómo preparar el entorno de Azure para comenzar a replicar servidores físicos (x64) que ejecutan Windows o Linux en Azure.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por supuesto lo siguiente:
- Ha creado un almacén de Recovery Services para proteger sus servidores físicos. Puede crear un almacén de Recovery Services desde [Azure Portal](http://portal.azure.com "Azure Portal").
- Ha [configurado el entorno local](physical-azure-disaster-recovery.md) para replicar servidores físicos en Azure.

## <a name="prepare-target"></a>Preparación del destino

Después de completar el **Paso 1: Selección del objetivo de protección** y el **Paso 2: Preparación del origen**, irá al **Paso 3: Destino**.

![Preparación del destino](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Suscripción:** En el menú desplegable, seleccione la suscripción en la que desea replicar los servidores físicos.
2. **Modelo de implementación:** Seleccione el modelo de implementación (clásica o Resource Manager).

Según el modelo de implementación elegido, se ejecuta una validación para asegurarse de que tiene al menos una cuenta de almacenamiento compatible y una red virtual en la suscripción de destino en la que replicar y conmutar por error los servidores físicos.

Una vez completadas las validaciones correctamente, haga clic en Aceptar para ir al paso siguiente.

Si no tiene una cuenta de almacenamiento de Resource Manager compatible o una red virtual, puede crearla haciendo clic en los botones **+ Cuenta de almacenamiento** o **+ Red** en la parte superior de la página.

## <a name="next-steps"></a>Pasos siguientes
[Configuración de las opciones de replicación](vmware-azure-set-up-replication.md).
