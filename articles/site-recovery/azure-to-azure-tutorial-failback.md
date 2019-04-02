---
title: Conmutación por recuperación de las máquinas virtuales de Azure replicadas en una región secundaria de Azure para la recuperación ante desastres con el servicio Azure Site Recovery.
description: Aprenda cómo recuperar por conmutación máquinas virtuales de Azure con el servicio Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c8ce05e644ad556542314b17151b808586734824
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315324"
---
# <a name="fail-back-azure-vms-between-azure-regions"></a>Conmutación por recuperación de máquinas virtuales de Azure entre regiones de Azure

El servicio [Azure Site Recovery](site-recovery-overview.md) contribuye a su estrategia de recuperación ante desastres mediante la administración y la coordinación de la replicación, la conmutación por error y la conmutación por recuperación de máquinas locales y máquinas virtuales (VM) de Azure.

En este tutorial se describe cómo conmutar por recuperación una sola máquina virtual de Azure. Después de conmutar por error, conmutará por recuperación a la región principal cuando esté disponible. En este tutorial, aprenderá a:

> [!div class="checklist"]
> 
> * Conmutar por recuperación la máquina virtual de Azure en la región secundaria.
> * Volver a proteger la máquina virtual de Azure principal en la región secundaria.
> 
> [!NOTE]
> 
> Este tutorial le ayuda a conmutar por error algunas máquinas virtuales a una región de destino y por recuperación a la región de origen con mínimas personalizaciones. Para obtener instrucciones detalladas, consulte los artículos de Guías de procedimientos para máquinas virtuales de Azure.

## <a name="before-you-start"></a>Antes de comenzar

> * Asegúrese de que la máquina virtual está en el estado **Conmutación por error confirmada**.
> * Compruebe que la región primaria está disponible, y que puede crear y tener acceso a los recursos nuevos en ella.
> * Asegúrese de que reprotección está habilitada.

## <a name="fail-back-to-the-primary-region"></a>Conmutación por recuperación a la región principal

Tras volver a proteger las máquinas virtuales, puede conmutar por recuperación a la región primaria, si es preciso.

1. En el almacén, haga clic en **Elementos replicados** y seleccione la máquina virtual que se ha vuelto a proteger.

    ![Conmutación por recuperación a la región primaria](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Haga clic en **Conmutación por error de prueba** para realizar una conmutación por error de prueba a la región primaria.
4. Seleccione el punto de recuperación y la red virtual para la conmutación por error de prueba y haga clic en **Aceptar**. Puede revisar la máquina virtual de prueba creada en la región primaria.
5. Una vez que la conmutación por error de prueba se realice correctamente, haga clic en **Limpiar conmutación por error de prueba** para limpiar los recursos creados en la región de origen para la conmutación por error de prueba.
6. En **Elementos replicados**, seleccione la máquina virtual y haga clic en **Conmutación por error**.
7. En **Conmutación por error**, seleccione un punto de recuperación al que conmutar por error.
    - **Más reciente (opción predeterminada)**: procesa todos los datos en el servicio Site Recovery y proporciona el objetivo de punto de recuperación (RPO) más bajo.
    - **Procesado más recientemente**: revierte la máquina virtual al último punto de recuperación procesado por Site Recovery.
    - **Personalizado**: conmuta por error a un punto de recuperación concreto. Esta opción es útil para realizar una conmutación por error de prueba.

8. Seleccione **Apague la máquina antes de comenzar con la conmutación por error** si desea que Site Recovery intente apagar las máquinas virtuales de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Tenga en cuenta que Site Recovery no es un origen limpio después de la conmutación por error.
9. Siga el progreso de la conmutación por error en la página **Trabajos**.
10. Después de la conmutación por error, valide la máquina virtual iniciando sesión en ella. Puede cambiar el punto de recuperación según sea necesario.
11. Después de haber comprobado la conmutación por error, haga clic en **Confirmar la conmutación por error**. Al hacerlo, se eliminarán los puntos de recuperación disponibles. La opción Cambiar punto de recuperación ya no está disponible.
12. La máquina virtual debe mostrarse como conmutada por error y conmutada por recuperación.

    ![Máquina virtual en regiones primarias y secundarias](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Las máquinas virtuales de recuperación ante desastres permanecerán en estado apagado y desasignado. Esto es así intencionadamente, ya que Site Recovery guarda la información de la máquina virtual, que puede ser útil para la conmutación por error desde la región primaria a la secundaria más adelante. No se cobra por las máquinas virtuales desasignadas, por lo que deben conservarse tal cual.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga más información](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) sobre el flujo de reprotección.
