---
title: Conmutación por recuperación de las máquinas virtuales de Azure replicadas en una región secundaria de Azure para la recuperación ante desastres con el servicio Azure Site Recovery.
description: Aprenda cómo recuperar por conmutación máquinas virtuales de Azure con el servicio Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c8be547790452774992b9226ca8010532263aaff
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2019
ms.locfileid: "70814519"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>Conmutación por recuperación de una máquina virtual de Azure entre regiones de Azure

El servicio [Azure Site Recovery](site-recovery-overview.md) contribuye a su estrategia de recuperación ante desastres mediante la administración y la coordinación de la replicación, la conmutación por error y la conmutación por recuperación de máquinas locales y máquinas virtuales (VM) de Azure.

En este tutorial se describe cómo conmutar por recuperación una sola máquina virtual de Azure. Después de conmutar por error, tendrá que conmutar por recuperación a la región primaria cuando esté disponible. En este tutorial, aprenderá a:

> [!div class="checklist"]
> 
> * Conmutar por recuperación la máquina virtual en la región secundaria.
> * Volver a proteger la máquina virtual principal en la región secundaria.
> 
> [!NOTE]
> 
> Este tutorial le ayuda a conmutar por error algunas máquinas virtuales a una región de destino y por recuperación a la región de origen con mínimas personalizaciones. Si desea instrucciones más detalladas, consulte las [guías paso a paso para máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/).

## <a name="before-you-start"></a>Antes de comenzar

* Asegúrese de que la máquina virtual está en el estado **Conmutación por error confirmada**.
* Compruebe que la región primaria está disponible, y que puede crear y tener acceso a los recursos nuevos en ella.
* Asegúrese de que la reprotección está habilitada.

## <a name="fail-back-to-the-primary-region"></a>Conmutación por recuperación a la región principal

Una vez que las máquinas virtuales vuelven a estar protegidas, puede conmutar por recuperación a la región primaria, si es preciso.

1. En el almacén, haga clic en **Elementos replicados** y seleccione la máquina virtual que se ha vuelto a proteger.

    ![Conmutación por recuperación a la región primaria](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

2. En **Elementos replicados**, seleccione la máquina virtual y luego seleccione **Conmutación por error**.
3. En **Conmutación por error**, seleccione un punto de recuperación en el que realizar la conmutación por error:
    - **Más reciente (opción predeterminada)** : procesa todos los datos en el servicio Site Recovery y proporciona el objetivo de punto de recuperación (RPO) más bajo.
    - **Procesado más recientemente**: revierte la máquina virtual al último punto de recuperación procesado por Site Recovery.
    - **Personalizado**: conmuta por error a un punto de recuperación concreto. Esta opción es útil para realizar una conmutación por error de prueba.
4. Seleccione **Shut down machine before beginning failover** (Apagar la máquina antes de comenzar con la conmutación por error) si desea que Site Recovery intente apagar las máquinas virtuales de la región de recuperación ante desastres antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. 
5. Siga el progreso de la conmutación por error en la página **Trabajos**.
6. Una vez terminada la conmutación por error, valide la máquina virtual iniciando sesión en ella. Puede cambiar el punto de recuperación según sea necesario.
7. Después de haber comprobado la conmutación por error, seleccione **Commit the failover** (Confirmar la conmutación por error). Al hacerlo, se eliminarán los puntos de recuperación disponibles. La opción Cambiar punto de recuperación ya no está disponible.
8. La máquina virtual debe mostrarse como conmutada por error y conmutada por recuperación.

    ![Máquina virtual en regiones primarias y secundarias](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

## <a name="next-steps"></a>Pasos siguientes

[Obtenga más información](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) sobre el flujo de reprotección.
