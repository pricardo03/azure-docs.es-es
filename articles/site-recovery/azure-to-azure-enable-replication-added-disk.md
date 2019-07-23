---
title: Habilitar la replicación de un disco agregado a una VM de Azure replicada por Azure Site Recovery | Microsoft Docs
description: En este artículo, se describe cómo habilitar la replicación de un disco agregado a VM de Azure que está habilitada para la recuperación ante desastres con Azure Site Recovery.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: asgang
ms.openlocfilehash: 4a262a3a0c32516988890a6afc6eef34d8655c89
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671868"
---
# <a name="enable-replication-for-a-disk-added-to-an-azure-vm"></a>Habilitar la replicación de un disco agregado a una VM de Azure


En este artículo, se describe cómo habilitar la replicación de un disco de datos agregado a VM de Azure que ya ha sido habilitada para la recuperación ante desastres en otra región de Azure con [Azure Site Recovery](site-recovery-overview.md).

Habilitar la replicación para un disco que se agrega a una VM se admite para VM de Azure con discos administrados.

Cuando agrega un nuevo disco a una VM de Azure que se replica en otra región de Azure, ocurre lo siguiente:

-   El mantenimiento de la replicación de la VM muestra una advertencia, y una nota en el portal le informa de que están disponibles uno o más discos para protección.
-   Si habilita la protección para los discos agregados, la advertencia desaparece después de la replicación inicial del disco.
-   Si decide no habilitar la replicación para el disco, puede seleccionar descartar la advertencia.

![Nuevo disco agregado](./media/azure-to-azure-enable-replication-added-disk/newdisk.png)



## <a name="before-you-start"></a>Antes de comenzar

En este artículo se da por supuesto que ya ha configurado la recuperación ante desastres para la VM a la que va a agregar el disco. Si no lo ha hecho, siga el [tutorial de recuperación ante desastres de Azure a Azure](azure-to-azure-tutorial-enable-replication.md). 

## <a name="enable-replication-for-an-added-disk"></a>Habilitar la replicación para un disco agregado 

Para habilitar la replicación para un disco agregado, haga lo siguiente:

1. En el almacén > **Elementos replicados**, haga clic en la VM a la que agregó el disco.
2. Haga clic en **Discos** y, luego, seleccione el disco de datos para el que quiere habilitar la replicación (estos discos tienen un estado de **No protegido**).
3.  En **Detalles del disco**, haga clic en **Habilitar replicación**.

    ![Habilitación de la replicación para un disco agregado](./media/azure-to-azure-enable-replication-added-disk/enabled-added.png)

Después de la ejecución del trabajo de habilitación de la replicación y cuando la replicación inicial finaliza, se elimina la advertencia de mantenimiento de replicación de la incidencia del disco.



# <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
