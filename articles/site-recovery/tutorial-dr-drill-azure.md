---
title: Ejecución de una exploración en profundidad de una recuperación ante desastres de máquinas locales en Azure con Azure Site Recovery | Microsoft Docs
description: Obtenga información sobre la ejecución de la exploración en profundidad de una recuperación ante desastres desde máquinas locales en Azure con Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 7e85226d15b818dda65600760b3950fab9dd7aaf
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58312332"
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



## <a name="verify-vm-properties"></a>Comprobar las propiedades de la máquina virtual

Antes de ejecutar una conmutación por error de prueba, compruebe las propiedades de la máquina virtual y asegúrese de que la [máquina virtual de Hyper-V](hyper-v-azure-support-matrix.md#replicated-vms) o la [máquina virtual de VMware](vmware-physical-azure-support-matrix.md#replicated-machines) cumpla con los requisitos de Azure.

1. En **Elementos protegidos**, haga clic en **Elementos replicados** y luego en la máquina.
2. En el panel **Elemento replicado**, puede ver un resumen de la información de la máquina virtual, el estado de mantenimiento y los puntos de recuperación disponibles más recientes. Haga clic en **Propiedades** para ver más detalles.
3. En **Proceso y red**, puede modificar el nombre de Azure, el grupo de recursos, el tamaño de destino, el conjunto de disponibilidad y la configuración de discos administrados.
4. Puede ver y modificar la configuración de red, incluida la red o subred en la que se va a ubicar la máquina virtual de Azure después de la conmutación por error y la dirección IP que se le va a asignar.
5. En **Discos** puede ver información sobre los discos de datos y el sistema operativo de la máquina virtual.

## <a name="create-a-network-for-test-failover"></a>Creación de una red para conmutación por error de prueba

Se recomienda que para realizar una conmutación por error de prueba, elija una red que esté aislada de la del sitio de recuperación de producción concreto proporcionada en la configuración de **Proceso y red** de cada máquina virtual. De forma predeterminada, cualquier red virtual que se cree en Azure está aislada de otras redes. La red de prueba debe imitar la de producción:

- La red de prueba debe tener el mismo número de subredes que la de producción. Las subredes deben tener los mismos nombres.
- La red de prueba debe usar el mismo intervalo de direcciones IP.
- Actualice el DNS de la red de prueba con la dirección IP especificada para la máquina virtual de DNS en la configuración de **Proceso y red**. Lea [Consideraciones sobre la conmutación por error de prueba para Active Directory](site-recovery-active-directory.md#test-failover-considerations) para más información.

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

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparación para la conexión a las máquinas virtuales de Azure después de la conmutación por error

Si desea conectarse a máquinas virtuales de Azure mediante RDP/SSH después de la conmutación por error, siga los requisitos resumidos en [esta](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) tabla.

Siga los pasos descritos [aquí](site-recovery-failover-to-azure-troubleshoot.md) para solucionar problemas de conectividad tras la conmutación por error.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejecución de una conmutación por error y una conmutación por recuperación entre máquinas de VMware locales](vmware-azure-tutorial-failover-failback.md).
> [Ejecución de una conmutación por error y una conmutación por recuperación entre máquinas de Hyper-V locales](hyper-v-azure-failover-failback-tutorial.md).
