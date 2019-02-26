---
title: Traslado de máquinas virtuales de Azure IaaS a otra región de Azure como máquinas virtuales ancladas por zona mediante el servicio Azure Site Recovery | Microsoft Docs
description: Use Azure Site Recovery para trasladar máquinas virtuales de IaaS de Azure a otra región de Azure como máquinas virtuales ancladas por zona.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 5597f3c017ccf2dbb58b7b6b046720c8f49803c5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312300"
---
# <a name="move-azure-vms-into-availability-zones"></a>Traslado de máquinas virtuales de Azure a zonas de disponibilidad
Las zonas de disponibilidad de Azure protegen las aplicaciones y los datos de errores del centro de datos. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. Para garantizar la resistencia, hay tres zonas independientes como mínimo en todas las regiones habilitadas. La separación física de las zonas de disponibilidad dentro de una región protege las aplicaciones y los datos frente a los errores del centro de datos. Con las zonas de disponibilidad, Azure ofrece el mejor Acuerdo de Nivel de Servicio del sector de tiempo de actividad de máquina virtual, con un 99,99 %. La zona de disponibilidad se admite en regiones seleccionadas, como se ha mencionado [aquí](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones). 

En un escenario en el que ha implementado las máquinas virtuales como de "instancia única" en una región específica y desea mejorar la disponibilidad trasladándolas a la zona de disponibilidad, puede hacerlo con Azure Site Recovery. Esto se puede categorizar más adelante en:

- Traslado de máquinas virtuales de instancia única a las zonas de disponibilidad en una región de destino
- Traslado de máquinas virtuales de un conjunto de disponibilidad en zonas de disponibilidad de región de destino

> [!IMPORTANT]
> Actualmente Azure Site Recovery permite trasladar máquinas virtuales de una región a otra y no admite el traslado dentro de una región. 

## <a name="verify-prerequisites"></a>Verificación de los requisitos previos

- Compruebe si la región de destino [admite la zona de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones): compruebe la [combinación de región de origen y región de destino que ha elegido](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) y tome una decisión informada sobre la región de destino.
- Asegúrese de entender la [arquitectura y los componentes del escenario](azure-to-azure-architecture.md).
- Revise las [limitaciones y los requisitos de compatibilidad](azure-to-azure-support-matrix.md).
- Compruebe los permisos de la cuenta: Si acaba de crear su cuenta de Azure gratuita, ya es administrador de su suscripción. Si no es administrador de la suscripción, solicite al administrador que le asigne los permisos que necesita. Para habilitar la replicación para una máquina virtual y finalmente copiar los datos al destino mediante Azure Site Recovery, debe tener:

    1. Permisos para crear una máquina virtual en recursos de Azure. El rol integrado "Colaborador de la máquina virtual" tiene estos permisos, que incluyen:
        - Permiso para crear una máquina virtual en el grupo de recursos seleccionado.
        - Permiso para crear una máquina virtual en la red virtual seleccionada
        - Permiso para escribir en la cuenta de almacenamiento seleccionada

    2. También necesita permiso para administrar las operaciones de Azure Site Recovery. El rol "Colaborador de Site Recovery" tiene todos los permisos necesarios para administrar las operaciones de Site Recovery en un almacén de Recovery Services.

## <a name="prepare-the-source-vms"></a>Preparación de las máquinas virtuales de origen

1. Las máquinas virtuales deben usar discos administrados si desea trasladarlas a la zona de disponibilidad mediante Site Recovery. Puede convertir máquinas virtuales Windows existentes que usan discos no administrados y puede convertirlas para usar discos administrados con los pasos siguientes que se mencionan [aquí](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks). Para ello, asegúrese de que el conjunto de disponibilidad esté configurado como "Administrado". 
2. Asegúrese de que todos los certificados raíz más recientes estén presentes en las máquinas virtuales de Azure que quiera trasladar. Si los últimos certificados raíz no están presentes, la copia de datos en la región de destino no se puede habilitar debido a las restricciones de seguridad.

3. Para las máquinas virtuales de Windows, instale las actualizaciones de Windows más recientes en la máquina virtual, de modo que todos los certificados raíz de confianza estén en ella. En un entorno desconectado, siga los procesos estándar de actualización de certificados y de Windows Update en su organización.

4. En las máquinas virtuales Linux, para obtener los certificados raíz de confianza y la lista de revocación de certificados en la máquina virtual, siga las instrucciones proporcionadas por su distribuidor de Linux.
5. Asegúrese de que no utiliza un proxy de autenticación para controlar la conectividad de red de las máquinas virtuales que quiere trasladar.

6. Si la máquina virtual que está intentando trasladar no tiene acceso a Internet y utiliza un proxy de firewall para controlar el acceso de salida, compruebe los requisitos [aquí](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

7. Identifique el diseño de la red de origen y todos los recursos que está utilizando actualmente, incluidos equilibradores de carga, grupos de seguridad de red y dirección IP pública para la verificación.

## <a name="prepare-the-target-region"></a>Preparación de la región de destino

1. Compruebe que su suscripción de Azure permite crear máquinas virtuales en la región de destino que se usa para la recuperación ante desastres. Póngase en contacto con el soporte técnico para habilitar la cuota necesaria, si lo necesite.

2. Asegúrese de que su suscripción tiene suficientes recursos para admitir máquinas virtuales con tamaños que se correspondan con las máquinas virtuales de origen. Si está usando Site Recovery para copiar datos en el destino, elija el mismo tamaño para la máquina virtual de destino o el más cercano posible.

3. Asegúrese de que crea un recurso de destino para cada componente identificado en el diseño de la red de origen. Esto es importante para asegurarse de que, tras la migración a la región de destino, las máquinas virtuales tengan toda la funcionalidad y características que tenían en el origen.

    > [!NOTE]
    > Azure Site Recovery automáticamente detecta y crea una cuenta de almacenamiento y una red virtual cuando habilita la replicación para la máquina virtual de origen, o también puede crear previamente estos recursos y asignarlos a la máquina virtual como parte del paso de habilitación de la replicación. Pero para cualquier otro recurso, como se menciona a continuación, es necesario crearlo manualmente en la región de destino.

     Consulte los siguientes documentos para crear los recursos de red más utilizados y que considere más relevantes, en función de la configuración de la máquina virtual de origen.

    - [Grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Equilibradores de carga](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials
        
     - [Dirección IP pública ](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
   Para cualquier otro componente de red, consulte la [documentación](https://docs.microsoft.com/azure/#pivot=products&panel=network) de red. 

> [!IMPORTANT]
> Asegúrese de usar un equilibrador de carga con redundancia de zona en el destino. Puede leer más [aquí](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones).

4. Si desea probar la configuración antes de realizar la migración final a la región de destino, [cree manualmente una red sin producción](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) en la región de destino. Esto creará una interferencia mínima con el entorno de producción y, por este motivo, se recomienda.


> [!NOTE]
> Los pasos que se indican a continuación son para una única máquina virtual, puede ampliar los mismos a varias máquinas virtuales accediendo al almacén de Recovery Services y haciendo clic en +Replicar y seleccionando las máquinas virtuales correspondientes juntas.

## <a name="enable-replication"></a>Habilitar replicación
Los siguientes pasos le guiarán en el uso de Azure Site Recovery para permitir la replicación de datos en la región de destino, antes de que finalmente los traslade a las zonas de disponibilidad.

1. En Azure Portal, haga clic en **Máquinas virtuales** y seleccione la máquina virtual que desea trasladar a las zonas de disponibilidad.
2. En **Operaciones**, haga clic en **Recuperación ante desastres**.
3. En **Configurar recuperación ante desastres** > **Región de destino**, seleccione la región de destino en la que quiere realizar la replicación. Asegúrese de que esta región [admita](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones) zonas de disponibilidad.
![enable-rep-1.PNG](media/azure-vms-to-zones/enable-rep-1.PNG)

1. Seleccione **Siguiente: Configuración avanzada**
2. Elija los valores adecuados para la subscripción de destino, el grupo de recursos de la máquina virtual de destino y la red virtual.
3. En la sección **Disponibilidad**, elija la zona de disponibilidad a la que desea trasladar la máquina virtual. 
> [!NOTE]
> Si no ve la opción para el conjunto de disponibilidad o la zona de disponibilidad, compruebe que se cumplen los [requisitos previos](#prepare-the-source-vms) y que se ha completado la [preparación](#prepare-the-source-vms) de las máquinas virtuales de origen.

   ![enable-rep-2.PNG](media/azure-vms-to-zones/enable-rep-2.PNG)

7. Haga clic en Habilitar réplica. Esto inicia un trabajo para habilitar la replicación de la máquina virtual.

## <a name="verify-settings"></a>Comprobación de la configuración

Cuando haya finalizado el trabajo de replicación, puede comprobar el estado de replicación, modificar la configuración de replicación y probar la implementación.

1. En el menú de la máquina virtual, haga clic en **Recuperación ante desastres**.
2. Puede comprobar el estado de replicación, los puntos de recuperación que se han creado y las regiones de origen y destino en el mapa.

   ![Estado de replicación](media/azure-to-azure-quickstart/replication-status.png)

## <a name="test-the-configuration"></a>Pruebe la configuración.

1. En el menú de la máquina virtual, haga clic en **Recuperación ante desastres**.
2. Haga clic en el icono **Conmutación por error de prueba**.
3. En **Conmutación por error**, seleccione el punto de recuperación que usar para la conmutación por error:

   - **Procesado más recientemente**: error de la VM en el último punto de recuperación procesado por el servicio Site Recovery. Se muestra la marca de tiempo. Con esta opción, no se emplea tiempo en el procesamiento de datos, por lo que se proporciona un objetivo de tiempo de recuperación (RTO) bajo.
   - **Más reciente coherente con la aplicación**: esta opción conmuta por error todas las VM en el punto de recuperación más reciente coherente con la aplicación. Se muestra la marca de tiempo.
   - **Personalizado**: seleccione un punto de recuperación.

3. Seleccione la red virtual de Azure de destino de prueba a la que desea mover las máquinas virtuales de Azure para probar la configuración. 

> [!IMPORTANT]
> Le recomendamos que utilice una red de máquinas virtuales de Azure independiente para la conmutación por error de prueba y no la red de producción de la región de destino a la que desea trasladar las máquinas virtuales con el tiempo.

4. Para comenzar a probar el traslado, haga clic en **Aceptar**. Para realizar el seguimiento del progreso, haga clic en la máquina virtual para abrir sus propiedades. También puede hacer clic en el trabajo **Conmutación por error de prueba** en el nombre del almacén > **Configuración** > **Trabajos** > **Trabajos de Site Recovery**.
5. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal > **Virtual Machines**. Asegúrese de que la máquina virtual está en funcionamiento, tiene el tamaño adecuado y está conectada a la red apropiada.
6. Si desea eliminar la máquina virtual creada como parte de la prueba del traslado, haga clic en **Limpiar conmutación por error de prueba** en el elemento replicado. En **Notas**, registre y guarde las observaciones asociadas a la prueba.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Realice el traslado a la región de destino y confirme.

1.  En el menú de la máquina virtual, haga clic en **Recuperación ante desastres**.
2. Haga clic en el icono de **conmutación por error**.
3. En **Conmutación por error**, seleccione **Más reciente**. 
4. Seleccione **Apague la máquina antes de comenzar con la conmutación por error**. A continuación, Site Recovery intentará apagar la máquina virtual de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Puede seguir el progreso de la conmutación por error en la página **Trabajos**. 
5. Una vez finalizado el trabajo, compruebe que la máquina virtual aparece en la región de Azure de destino como se esperaba.
6. En **Elementos replicados**, haga clic con el botón derecho en la máquina virtual > **Confirmar**. Esto finaliza el proceso de traslado a la región de destino. Espere hasta que finalice el trabajo de confirmación.

## <a name="discard-the-resource-in-the-source-region"></a>Descarte de los recursos en la región de origen 

1. Vaya a la máquina virtual.  Haga clic en **Deshabilitar replicación**.  Esto detiene el proceso de copiar los datos para la máquina virtual.  

> [!IMPORTANT]
> Es importante realizar el paso anterior para evitar cargos por la replicación de Site Recovery después del traslado. La configuración de replicación de origen se limpia automáticamente. Tenga en cuenta que la extensión de Site Recovery que se instala como parte de la replicación no se ha eliminado y tiene que quitarse manualmente. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aumentado la disponibilidad de una máquina virtual de Azure al trasladarla a un conjunto de disponibilidad o zona de disponibilidad. Ya puede configurar la opción de recuperación ante desastres para la máquina virtual que ha trasladado.

> [!div class="nextstepaction"]
> [Configurar la recuperación ante desastres después de la migración](azure-to-azure-quickstart.md)


