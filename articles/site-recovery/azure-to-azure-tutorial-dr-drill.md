---
title: Ejecución de un simulacro de recuperación ante desastres en máquinas virtuales de Azure con Azure Site Recovery
description: Aprenda a ejecutar un simulacro de recuperación ante desastres en máquinas virtuales de Azure de una región de Azure secundaria mediante el servicio Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 817a220e36ac250b1d5a5aa90d0bddbfb155cc26
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091332"
---
# <a name="run-a-disaster-recovery-drill-to-a-secondary-region-for-azure-vms"></a>Ejecución de un simulacro de recuperación ante desastres en máquinas virtuales de Azure de una región secundaria de Azure 

El servicio [Azure Site Recovery](site-recovery-overview.md) contribuye a la estrategia de recuperación ante desastres y continuidad empresarial (BCDR) al mantener sus aplicaciones empresariales al día y disponibles durante interrupciones planeadas y no planeadas. Azure Site Recovery administra y coordina la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure, lo que incluye la replicación, la conmutación por error y la recuperación.

En este tutorial se muestra cómo ejecutar una exploración de recuperación ante desastres en una máquina virtual de Azure, desde una región de Azure a otra, con una conmutación por error de prueba. Mediante una exploración se valida su estrategia de replicación sin ocasionar ninguna pérdida de datos ni tiempo de inactividad, y sin afectar al entorno de producción. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Comprobar los requisitos previos
> * Ejecutar una conmutación por error de prueba en una sola máquina virtual

> [!NOTE]
> Este tutorial le ayuda a realizar un simulacro de recuperación ante desastres con los pasos mínimos; en caso de que desee obtener más información acerca de los diversos aspectos asociados con la realización del simulacro, incluidas las consideraciones de red, la automatización o la solución de problemas, consulte los documentos de procedimientos para las máquinas virtuales de Azure.

## <a name="prerequisites"></a>Requisitos previos

- Antes de ejecutar una conmutación por error de prueba, se recomienda que compruebe las propiedades de la máquina virtual para asegurarse de que todo se ajusta a lo esperado.  Acceda a las propiedades de la máquina virtual en **Elementos replicados**. En la hoja **Información esencial** se detalla la configuración y el estado de las máquinas.
- **Se recomienda usar una red de máquina virtual de Azure independiente en la conmutación por error de prueba** en lugar de la red predeterminada que se configuró cuando habilitó la replicación.
- Según las configuraciones de la red de origen de cada NIC, puede especificar de forma opcional una **subred, dirección IP, dirección IP pública, grupo de seguridad de red o equilibrador de carga interno** para asociar a cada NIC en la configuración de la conmutación por error de prueba en Proceso y red antes de realizar el simulacro de recuperación ante desastres.


## <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

1. En **Configuración** > **Elementos replicados**, haga clic en la VM > icono **+Probar conmutación por error**.

2. En **Conmutación por error**, seleccione el punto de recuperación que usar para la conmutación por error:

    - **Más reciente**: procesa todos los datos en el servicio Site Recovery y proporciona el objetivo de tiempo de recuperación (RTO) más bajo.
    - **Procesado más recientemente**: conmuta por error la máquina virtual en el último punto de recuperación procesado por Site Recovery. Se muestra la marca de tiempo. Con esta opción, no se emplea tiempo en el procesamiento de datos, por lo que se proporciona un objetivo de tiempo de recuperación (RTO) bajo.
   - **Más reciente coherente con la aplicación**: esta opción conmuta por error todas las VM en el punto de recuperación más reciente coherente con la aplicación. Se muestra la marca de tiempo.
   - **Personalizado**: conmuta por error a un punto de recuperación concreto. La personalización solo está disponible cuando se conmuta por error una sola máquina virtual y no para la conmutación por error con un plan de recuperación.

3. Seleccione la red virtual de Azure de destino a la se conectarán las VM de Azure en la región secundaria después de que se produzca la conmutación por error.

    > [!NOTE]
    > La lista desplegable para seleccionar la red virtual de Azure no estará visible si la configuración de conmutación por error de prueba está preconfigurada para el elemento replicado.

4. Para iniciar la conmutación por error, haga clic en **Aceptar**. Para realizar el seguimiento del progreso, haga clic en la máquina virtual para abrir sus propiedades. También puede hacer clic en el trabajo **Conmutación por error de prueba** en el nombre del almacén > **Configuración** > **Trabajos** > **Trabajos de Site Recovery**.
5. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal > **Virtual Machines**. Asegúrese de que la máquina virtual está en funcionamiento, tiene el tamaño adecuado y está conectada a la red apropiada.
6. Para eliminar las máquinas virtuales que se crearon durante la conmutación por error de prueba, haga clic en **Cleanup test failover** (Limpiar conmutación por error de prueba) en el artículo replicado o el plan de recuperación. En **Notas**, registre y guarde las observaciones asociadas a la conmutación por error de prueba.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejecutar una conmutación por error de producción](azure-to-azure-tutorial-failover-failback.md)
