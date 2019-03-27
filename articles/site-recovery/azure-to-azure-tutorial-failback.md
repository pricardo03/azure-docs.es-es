---
title: Conmutación por recuperación de las máquinas virtuales de Azure IaaS replicadas en una región secundaria de Azure para la recuperación ante desastres con el servicio Azure Site Recovery.
description: Aprenda cómo recuperar por conmutación máquinas virtuales de Azure con el servicio Azure Site Recovery.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: sideeksh
ms.custom: mvc
ms.openlocfilehash: d8721f313907f0e0519dca52f5565853f1c44110
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58089716"
---
# <a name="fail-back-azure-vms-between-azure-regions"></a>Conmutación por recuperación de máquinas virtuales de Azure entre regiones de Azure

El servicio [Azure Site Recovery](site-recovery-overview.md) contribuye a su estrategia de recuperación ante desastres mediante la administración y la coordinación de la replicación, la conmutación por error y la conmutación por recuperación de máquinas locales y máquinas virtuales (VM) de Azure.

En este tutorial se describe cómo conmutar por recuperación una sola máquina virtual de Azure. Después de conmutar por error, conmutará por recuperación a la región principal cuando esté disponible. En este tutorial, aprenderá a:

> [!div class="checklist"]
> 
> * Realizar la conmutación por recuperación de la máquina virtual secundaria
> * Volver a proteger la máquina virtual principal en la región secundaria
> 
> [!NOTE]
> 
> Este tutorial está diseñado para guiar al usuario en los pasos para conmutar por error en una región de destino y por recuperación con la personalización mínima. En caso de que desee obtener más información acerca de los diversos aspectos asociados con la conmutación por error, incluidas las consideraciones de red, la automatización o la solución de problemas, consulte los documentos de procedimientos para las máquinas virtuales de Azure.

## <a name="prerequisites"></a>Requisitos previos

> * Asegúrese de que la máquina virtual está en el estado Conmutación por error confirmada y compruebe que la región principal está disponible, y puede crear y tener acceso a los recursos nuevos en ella.
> * Asegúrese de que reprotección está habilitada.

## <a name="fail-back-to-the-primary-region"></a>Conmutación por recuperación a la región principal

Tras volver a proteger las máquinas virtuales, puede conmutar por recuperación a la región primaria, cuando y como desee.

1. Vaya al almacén de Recovery Services. Haga clic en Elementos replicados y seleccione la máquina virtual que se ha vuelto a proteger.

2. Debe ver lo siguiente. Tenga en cuenta que es similar a la hoja de conmutación por error de prueba y conmutación por error de la región primaria.
![Conmutación por recuperación a la región principal](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

3. Haga clic en Conmutación por error de prueba para volver a realizar una conmutación por error de prueba a la región principal. Elija el punto de recuperación y la red virtual para la conmutación por error de prueba y seleccione Aceptar. Puede ver la máquina virtual de prueba creada en la región primaria, a la que puede acceder e inspeccionar.

4. Una vez que la conmutación por error de prueba se realice correctamente, puede hacer clic en Limpiar conmutación por error de prueba para limpiar los recursos creados en la región de origen para la conmutación por error de prueba.

5. En Elementos replicados, seleccione la máquina virtual que desea conmutar por error > Conmutar por error.

6. En Conmutación por error, seleccione un punto de recuperación al que conmutar por error. Puede seleccionar una de las siguientes opciones:
    1. Más reciente (opción predeterminada): esta opción procesa todos los datos en el servicio Site Recovery y proporciona el objetivo de punto de recuperación (RPO) más bajo.
    2. Procesado más recientemente: esta opción revierte la máquina virtual al punto de recuperación más reciente que el servicio Site Recovery haya procesado.
    3. Personalizado: use esta opción para conmutar por error a un punto de recuperación concreto. Esta opción es útil para realizar una conmutación por error de prueba.

7. Seleccione Apague la máquina antes de comenzar con la conmutación por error si quiere que Site Recovery intente apagar las máquinas virtuales de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Tenga en cuenta que Site Recovery no es un origen limpio después de la conmutación por error.

8. Siga el progreso de la conmutación por error en la página Trabajos.

9. Después de la conmutación por error, valide la máquina virtual iniciando sesión en ella. Si desea volver a otro punto de recuperación para la máquina virtual, puede usar la opción Cambiar punto de recuperación.

10. Realice la acción Confirmar una vez que quede satisfecho con la conmutación por error de la máquina virtual. Al confirmar, se eliminan todos los puntos de recuperación disponibles con el servicio. La opción Cambiar punto de recuperación ya no está disponible.

![Máquina virtual en regiones primarias y secundarias](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

Si ve la captura de pantalla anterior, la máquina virtual "ContosoWin2016" conmuta por error del Centro de EE. UU. al Este de EE. UU. y por recuperación del Este de EE. UU. al Centro de EE. UU.

Recuerde que las máquinas virtuales de recuperación ante desastres permanecerán en estado apagado y desasignado. Este comportamiento se ha diseñado así porque Azure Site Recovery guarda la información de la máquina virtual, que puede resultar útil para la conmutación por error de la región primaria a la secundaria posteriormente. No se cobra por las máquinas virtuales desasignadas, por lo que deben conservarse tal cual.

> [!NOTE]
> Consulte la [sección de procedimientos](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) para más detalles sobre el flujo de trabajo de reprotección y lo que sucede durante esta.
