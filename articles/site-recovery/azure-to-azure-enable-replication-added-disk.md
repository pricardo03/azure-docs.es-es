---
title: Habilitar la replicación de un disco que se ha agregado a una máquina virtual de Azure replicado por Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo habilitar la replicación de un disco que se ha agregado a una máquina virtual de Azure que está habilitado para la recuperación ante desastres con Azure Site Recovery
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: asgang
ms.openlocfilehash: 69122ffe9cefa3e1b9c6c8fbadfa80492ebebbde
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64928068"
---
# <a name="enable-replication-for-a-disk-added-to-an-azure-vm"></a>Habilitar la replicación de un disco que se ha agregado a una máquina virtual de Azure


En este artículo se describe cómo habilitar la replicación de los discos de datos que se agregan a una máquina virtual de Azure que ya está habilitado para la recuperación ante desastres en otra región de Azure, mediante [Azure Site Recovery](site-recovery-overview.md).

Habilitar la replicación para un disco que se agrega a una máquina virtual se admite para máquinas virtuales de Azure con discos administrados.

Cuando se agrega un nuevo disco a una máquina virtual de Azure que se replica en otra región de Azure, ocurre lo siguiente:

-   Estado de replicación de la máquina virtual muestra una advertencia y una nota en el portal le informa de que uno o más discos están disponibles para la protección.
-   Si habilita la protección para los discos agregados, la advertencia desaparecerá después de la replicación inicial del disco.
-   Si decide no habilitar la replicación para el disco, puede seleccionar para descartar la advertencia.

![Nuevo disco se ha agregado](./media/azure-to-azure-enable-replication-added-disk/newdisk.png)



## <a name="before-you-start"></a>Antes de comenzar

En este artículo se da por supuesto que ya ha configurado la recuperación ante desastres para la máquina virtual a la que va a agregar el disco. Si no lo ha hecho, siga el [tutorial de recuperación ante desastres en Azure](azure-to-azure-tutorial-enable-replication.md). 

## <a name="enable-replication-for-an-added-disk"></a>Habilitar la replicación de un disco agregado 

Para habilitar la replicación para un disco agregado, haga lo siguiente:

1. En el almacén > **elementos replicados**, haga clic en la máquina virtual al que agregó el disco.
2. Haga clic en **discos**y, a continuación, seleccione el disco de datos para el que desea habilitar la replicación (estos discos tienen un **no protegido** estado).
3.  En **los detalles del disco**, haga clic en **habilitar la replicación**.

    ![Habilitar la replicación de disco agregado](./media/azure-to-azure-enable-replication-added-disk/enabled-added.png)

Una vez que se ejecuta el trabajo de habilitación de replicación y la replicación inicial finaliza, se quita la advertencia de mantenimiento de replicación para el problema de disco.



# <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
