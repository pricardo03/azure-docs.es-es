---
title: Ejecución de una exploración en profundidad de una recuperación ante desastres de máquinas locales en Azure con Azure Site Recovery | Microsoft Docs
description: Obtenga información sobre la ejecución de la exploración en profundidad de una recuperación ante desastres desde máquinas locales en Azure con Azure Site Recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: af8062fc0134975542c8a5ec420c790f33996154
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920178"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Ejecución de un simulacro de recuperación ante desastres en Azure

En este artículo se muestra cómo ejecutar una exploración en profundidad de una recuperación ante desastres de una máquina local en Azure, mediante una conmutación por error de prueba. Una exploración en profundidad valida su estrategia de replicación sin pérdida de datos.

Este es el cuarto tutorial de una serie que muestra cómo configurar la recuperación ante desastres en Azure para máquinas virtuales locales de VMware o máquinas virtuales de Hyper-V.

En este tutorial se da por supuesto que ha completado los tres primeros tutoriales: 
    - En el [primer tutorial](tutorial-prepare-azure.md), se configuraron los componentes de Azure necesarios para la recuperación ante desastres de VMware.
    - En el [segundo tutorial](vmware-azure-tutorial-prepare-on-premises.md), se prepararon los componentes locales para la recuperación ante desastres y se repasaron los requisitos previos.
    - En el [tercer tutorial](vmware-azure-tutorial.md) se configuró y habilitó la replicación de nuestras máquinas virtuales locales de VMware.
    - Los tutoriales están diseñados para mostrarle la **ruta de implementación más sencilla para un escenario**. Usan opciones predeterminadas siempre que es posible y no muestran todos los valores y las rutas de acceso posibles. Para más información acerca de los pasos de la conmutación por error de prueba, lea la [guía de procedimientos](site-recovery-test-failover-to-azure.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar una red aislada para la conmutación por error de prueba
> * Preparar la conexión a la máquina virtual de Azure después de una conmutación por error
> * Ejecutar una conmutación por error de prueba para una sola máquina

Este tutorial

## <a name="verify-vm-properties"></a>Comprobación de las propiedades de la máquina virtual

Antes de ejecutar una conmutación por error de prueba, compruebe las propiedades de la máquina virtual y asegúrese de que la [máquina virtual de Hyper-V](hyper-v-azure-support-matrix.md#replicated-vms) o la [máquina virtual de VMware](vmware-physical-azure-support-matrix.md#replicated-machines) cumpla con los requisitos de Azure.

1. En **Elementos protegidos**, haga clic en **Elementos replicados** y luego en la máquina.
2. En el panel **Elemento replicado**, puede ver un resumen de la información de la máquina virtual, el estado de mantenimiento y los puntos de recuperación disponibles más recientes. Haga clic en **Propiedades** para ver más detalles.
3. En **Proceso y red**, puede modificar el nombre de Azure, el grupo de recursos, el tamaño de destino, el conjunto de disponibilidad y la configuración de discos administrados.
4. Puede ver y modificar la configuración de red, incluida la red o subred en la que se va a ubicar la máquina virtual de Azure después de la conmutación por error y la dirección IP que se le va a asignar.
5. En **Discos** puede ver información sobre los discos de datos y el sistema operativo de la máquina virtual.

## <a name="run-a-test-failover-for-a-single-vm"></a>Ejecutar una conmutación por error de prueba en una sola máquina virtual

Cuando se ejecuta una conmutación por error de prueba, ocurre lo siguiente:

1. Se ejecuta una comprobación de requisitos previos para garantizar que se dan todas las condiciones necesarias para la conmutación por error.
2. La conmutación por error procesa los datos, de modo que se pueda crear una máquina virtual de Azure. Si se selecciona el último punto de recuperación, se crea un punto de recuperación a partir de los datos.
3. Se crea una VM de Azure mediante los datos procesados en el paso anterior.

Ejecute la conmutación por error de prueba de la manera siguiente:

1. En **Configuración** > **Elementos replicados**, haga clic en la VM > **+Probar conmutación por error**.
2. Seleccione el punto de recuperación **Procesado más recientemente** para este tutorial. Se realiza una conmutación por error de la máquina virtual al último punto en el tiempo disponible. Se muestra la marca de tiempo. Con esta opción, no se empleó tiempo en el procesamiento de datos, por lo que se proporciona un objetivo de tiempo de recuperación (RTO) bajo.
3. En **Probar conmutación por error**, seleccione la red de Azure de destino a la que se conectarán las máquinas virtuales de Azure después de la conmutación por error.
4. Haga clic en **Aceptar** para iniciar la conmutación por error. Para realizar el seguimiento del progreso, haga clic en la máquina virtual para abrir sus propiedades. También puede hacer clic en el trabajo **Conmutación por error de prueba** en el nombre del almacén > **Configuración** > **Trabajos** >
   **Trabajos de Site Recovery**.
5. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal > **Virtual Machines**. Compruebe que la VM tiene el tamaño adecuado, que está conectada a la red correspondiente y que se está ejecutando.
6. Ahora debería poder conectarse a la VM replicada en Azure.
7. Para eliminar máquinas virtuales de Azure que se crearon durante la conmutación por error de prueba, haga clic en **Limpieza de conmutación por error de prueba** en la máquina virtual. En **Notas**, registre y guarde las observaciones asociadas a la conmutación por error de prueba.

En algunos escenarios, la conmutación por error requiere un procesamiento adicional que tarda aproximadamente de ocho a diez minutos en completarse. Puede observar tiempos de conmutación por error de prueba más largos en las máquinas de VMware Linux, las máquinas virtuales de VMware que no tienen el servicio DHCP habilitado y las máquinas virtuales de VMware que no tienen los siguientes controladores de arranque: storvsc, vmbus, storflt, intelide, atapi.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejecución de una conmutación por error y una conmutación por recuperación entre máquinas de VMware locales](vmware-azure-tutorial-failover-failback.md).
> [Ejecución de una conmutación por error y una conmutación por recuperación entre máquinas de Hyper-V locales](hyper-v-azure-failover-failback-tutorial.md).
