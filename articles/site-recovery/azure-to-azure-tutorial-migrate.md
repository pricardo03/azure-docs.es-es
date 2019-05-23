---
title: Traslado de máquinas virtuales de Azure IaaS a otra región de Azure mediante el servicio Azure Site Recovery | Microsoft Docs
description: Use Azure Site Recovery para mover máquinas virtuales de IaaS de Azure de una región de Azure a otra.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 0d446be664d695af946d46abc48389d4f7be92cd
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65791035"
---
# <a name="move-azure-vms-to-another-region"></a>Traslado de máquinas virtuales de Azure a otra región

Hay varios escenarios en los que puede desear mover las máquinas virtuales de Azure IaaS existentes de una región a otra. Por ejemplo, si desea mejorar la confiabilidad y disponibilidad de las máquinas virtuales existentes, para mejorar la capacidad de administración, o si quiere moverlas por motivos de gobernanza. Para más información, consulte [Introducción al traslado de máquinas virtuales de Azure](azure-to-azure-move-overview.md). 

Puede usar el servicio [Azure Site Recovery](site-recovery-overview.md) para administrar y coordinar la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure para la continuidad empresarial y recuperación ante desastres (BCDR). También puede usar Site Recovery para administrar el movimiento de máquinas virtuales de Azure a una región secundaria.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> 
> * Comprobar los requisitos previos para el traslado
> * Preparar las máquinas virtuales de origen y la región de destino
> * Copiar los datos y habilitar la replicación
> * Probar la configuración y realizar el traslado
> * Eliminar los recursos en la región de origen
> 
> [!NOTE]
> En este tutorial se muestra cómo trasladar las máquinas virtuales de Azure de una región a otra tal cual. Si tiene que mejorar la disponibilidad moviendo las máquinas virtuales en un conjunto de disponibilidad a máquinas virtuales ancladas a la zona en una región distinta, consulte el [tutorial Traslado de máquinas virtuales de Azure a zonas de disponibilidad](move-azure-vms-avset-azone.md).

## <a name="prerequisites"></a>Requisitos previos

- Asegúrese de tener las máquinas virtuales de Azure en la región de Azure desde la que va a realizar el traslado.
- Compruebe si [se admite la combinación de región de origen y región de destino](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) que ha elegido y tome una decisión informada sobre la región de destino.
- Asegúrese de entender la [arquitectura y los componentes del escenario](azure-to-azure-architecture.md).
- Revise las [limitaciones y los requisitos de compatibilidad](azure-to-azure-support-matrix.md).
- Compruebe los permisos de la cuenta. Si ha creado su cuenta de Azure gratis, ya es el administrador de la suscripción. Si no es el administrador de la suscripción, solicite al administrador que le asigne los permisos que necesita. Para habilitar la replicación para una máquina virtual y esencialmente copiar los datos mediante Azure Site Recovery, tiene que tener:

    * Permisos para crear una máquina virtual en recursos de Azure. El rol integrado "Colaborador de la máquina virtual" tiene estos permisos, que incluyen:
        - Permiso para crear una máquina virtual en el grupo de recursos seleccionado.
        - Permiso para crear una máquina virtual en la red virtual seleccionada
        - Permiso para escribir en la cuenta de almacenamiento seleccionada

    * Permisos para administrar las operaciones de Azure Site Recovery. El rol "Colaborador de Site Recovery" tiene todos los permisos necesarios para administrar las operaciones de Site Recovery en un almacén de Recovery Services.

## <a name="prepare-the-source-vms"></a>Preparación de las máquinas virtuales de origen

1. Asegúrese de que todos los certificados raíz más recientes están en las máquinas virtuales de Azure que quiera trasladar. Si los certificados raíz más recientes no están presentes en la máquina virtual, las restricciones de seguridad no permitirán la copia de datos en la región de destino.

    - Para las máquinas virtuales de Windows, instale las actualizaciones de Windows más recientes en la máquina virtual, de modo que todos los certificados raíz de confianza estén en ella. En un entorno desconectado, siga los procesos estándar de actualización de certificados y de Windows Update en su organización.
    - En las máquinas virtuales Linux, para obtener los certificados raíz de confianza y la lista de revocación de certificados en la máquina virtual, siga las instrucciones proporcionadas por su distribuidor de Linux.
1. Asegúrese de que no utiliza un proxy de autenticación para controlar la conectividad de red de las máquinas virtuales que quiere trasladar.
1. Si la máquina virtual que está intentando trasladar no tiene acceso a Internet, o utiliza un proxy de firewall para controlar el acceso de salida, [compruebe los requisitos](azure-to-azure-tutorial-enable-replication.md#set-up-outbound-network-connectivity-for-vms).
1. Identifique el diseño de red de origen y todos los recursos que está usando actualmente. Esto incluye, pero no se limita a los equilibradores de carga, grupos de seguridad de red (NSG), e IP públicas.

## <a name="prepare-the-target-region"></a>Preparación de la región de destino

1. Compruebe que su suscripción de Azure permite crear máquinas virtuales en la región de destino que se usa para la recuperación ante desastres. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

1. Asegúrese de que su suscripción tiene suficientes recursos para admitir máquinas virtuales con tamaños que se correspondan con las máquinas virtuales de origen. Si está usando Site Recovery para copiar datos en el destino, Site Recovery elije el mismo tamaño para la máquina virtual de destino o el más cercano posible.

1. Asegúrese de que crea un recurso de destino para cada componente identificado en el diseño de la red de origen. Este paso es importante para asegurarse de que las máquinas virtuales tengan en la región de destino toda la funcionalidad y características que tenían en la región de origen.

    > [!NOTE]
    > Azure Site Recovery automáticamente detecta y crea una red virtual al habilitar la replicación para la máquina virtual de origen. También puede crear previamente una red y asignarla a la máquina virtual en el flujo de usuario para habilitar la replicación. Como se menciona a continuación, es necesario crear manualmente cualquier otro recurso en la región de destino.

     Para crear los recursos de red más utilizados que considere apropiados, en función de la configuración de la máquina virtual de origen, consulte la siguiente documentación:

   - [Grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
   - [Equilibradores de carga](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
   - [Dirección IP pública](../virtual-network/virtual-network-public-ip-address.md)
    
     Para cualquier otro componente de red, consulte la [documentación de red](https://docs.microsoft.com/azure/#pivot=products&panel=network).

1. Si desea probar la configuración antes de realizar la migración final a la región de destino, [cree manualmente una red sin producción](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) en la región de destino. Se recomienda realizar este paso porque garantiza interferencias mínimas con la red de producción.

## <a name="copy-data-to-the-target-region"></a>Copia de datos en la región de destino
En los pasos siguientes aprenderá a utilizar Azure Site Recovery para copiar datos en la región de destino.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Cree el almacén en cualquier región, excepto en la de origen

1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Recovery Services**.
1. Seleccione **Crear un recurso** > **Herramientas de administración** > **Backup and Site Recovery**.
1. En el apartado **Nombre**, especifique el nombre descriptivo **ContosoVMVault**. Si tiene más de una suscripción, seleccione la apropiada.
1. Cree un grupo de recursos denominado **ContosoRG**.
1. Especifique una región de Azure. Para comprobar las regiones admitidas, consulte la disponibilidad geográfica en [Detalles de precios de Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
1. En los **almacenes de Recovery Services**, haga clic en **Información general** > **ContosoVMVault** > **+Replicar**.
1. En **Origen**, seleccione **Azure**.
1. En **Ubicación de origen**, seleccione la región de Azure de origen donde se ejecutan actualmente sus máquinas virtuales.
1. Seleccione el modelo de implementación de Resource Manager. A continuación, seleccione la **suscripción de origen** y el **grupo de recursos de origen**.
1. Seleccione **Aceptar** para guardar la configuración.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Habilite la replicación de máquinas virtuales de Azure y comience a copiar los datos

Site Recovery recupera una lista de las máquinas virtuales asociadas a la suscripción y el grupo de recursos.

1. En el paso siguiente, seleccione la máquina virtual que desee mover y, después, seleccione **Aceptar**.
1. En **Configuración**, haga clic en **Recuperación ante desastres**.
1. En **Configurar recuperación ante desastres** > **Región de destino**, seleccione la región de destino en la que quiere realizar la replicación.
1. Para este tutorial, acepte los valores predeterminados.
1. Seleccione **Habilitar replicación**. Este paso inicia un trabajo para habilitar la replicación de la máquina virtual.

    ![Habilitar replicación](media/tutorial-migrate-azure-to-azure/settings.png)

## <a name="test-the-configuration"></a>Pruebe la configuración.

1. Vaya al almacén. En **Configuración** > **Elementos replicados**, seleccione la máquina virtual que desea mover a la región de destino y luego seleccione **+ Conmutación por error de prueba**.
1. En **Conmutación por error de prueba**, seleccione el punto de recuperación a usar para la conmutación por error:

   - **Procesado más recientemente**: error de la VM en el último punto de recuperación procesado por el servicio Site Recovery. Se muestra la marca de tiempo. Con esta opción, no se emplea tiempo en el procesamiento de datos, por lo que se proporciona un objetivo de tiempo de recuperación (RTO) bajo.
   - **Más reciente coherente con la aplicación**: esta opción conmuta por error todas las VM en el punto de recuperación más reciente coherente con la aplicación. Se muestra la marca de tiempo.
   - **Personalizado**: seleccione un punto de recuperación.

1. Seleccione la red virtual de Azure de destino a la que desea trasladar las máquinas virtuales de Azure para probar la configuración.
    > [!IMPORTANT]
    > Se recomienda que utilice una red de máquina virtual de Azure independiente para la conmutación por error de prueba. No use la red de producción que se configuró cuando habilitó la replicación y a la que finalmente desea mover las máquinas virtuales.
1. Para comenzar a probar el traslado, haga clic en **Aceptar**. Para realizar el seguimiento del progreso, haga clic en la máquina virtual para abrir sus propiedades. También puede hacer clic en el trabajo **Conmutación por error de prueba** en el nombre del almacén > **Configuración** > **Trabajos** > **Trabajos de Site Recovery**.
1. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal > **Virtual Machines**. Asegúrese de que la máquina virtual está en funcionamiento, tiene el tamaño adecuado y está conectada a la red apropiada.
1. Si desea eliminar la máquina virtual creada como parte de la prueba del traslado, haga clic en **Limpiar conmutación por error de prueba** en el elemento replicado. En **Notas**, registre y guarde las observaciones asociadas a la prueba.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Realice el traslado a la región de destino y confirme

1. Vaya al almacén. En **Configuración** > **Elementos replicados**, seleccione la máquina virtual y luego seleccione **Conmutación por error**.
1. En **Conmutación por error**, seleccione **Más reciente**.
1. Seleccione **Apague la máquina antes de comenzar con la conmutación por error**. A continuación, Site Recovery intentará apagar la máquina virtual de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.
1. Una vez finalizado el trabajo, compruebe que la máquina virtual aparece en la región de Azure de destino como se esperaba.
1. En **Elementos replicados**, haga clic con el botón derecho en la máquina virtual > **Confirmar**. Esto finaliza el proceso de traslado a la región de destino. Espere hasta que el trabajo de confirmación finalice.

## <a name="delete-the-resource-in-the-source-region"></a>Eliminación del recurso en la región de origen

Vaya a la máquina virtual. Seleccione **Deshabilitar replicación**. Este paso detiene el proceso de copiar los datos para la máquina virtual.

> [!IMPORTANT]
> Es importante realizar este paso para evitar cargos por replicación de Azure Site Recovery.

Si no tiene planes para volver a usar ninguno de los recursos de origen, siga estos pasos:

1. Elimine todos los recursos de red pertinentes en la región de origen que se enumeraron como parte del paso 4 en [Preparación de las máquinas virtuales de origen](#prepare-the-source-vms).
1. Elimine la cuenta de almacenamiento correspondiente en la región de origen.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha trasladado una máquina virtual de Azure a otra región de Azure. Ahora puede configurar la opción de recuperación ante desastres para la máquina virtual que ha trasladado.

> [!div class="nextstepaction"]
> [Configurar la recuperación ante desastres después de la migración](azure-to-azure-quickstart.md)

