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
ms.openlocfilehash: f6713326045ebd84f1cd484803fbc725ad798d7b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55882274"
---
# <a name="move-azure-vms-to-another-region"></a>Traslado de máquinas virtuales de Azure a otra región

Hay varios escenarios en los que podría querer trasladar las máquinas virtuales IaaS de Azure existentes de una región a otra: para mejorar la confiabilidad y disponibilidad de las máquinas virtuales existentes, para mejorar la manejabilidad o debido a razones de gobierno, etc., según se indica [aquí](azure-to-azure-move-overview.md). Además de utilizar el servicio [Azure Site Recovery](site-recovery-overview.md) para administrar y coordinar la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure, con el fin de garantizar la continuidad empresarial y la recuperación ante desastres (BCDR), también puede usar Site Recovery para administrar el traslado de máquinas virtuales de Azure a una región secundaria.       

En este tutorial se muestra cómo trasladar las máquinas virtuales de Azure a otra región con Azure Site Recovery. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * [Verificación de los requisitos previos](#verify-prerequisites)
> * [Preparación de las máquinas virtuales de origen](#prepare-the-source-vms)
> * [Preparación de la región de destino](#prepare-the-target-region)
> * [Copia de datos en la región de destino](#copy-data-to-the-target-region)
> * [Prueba de la configuración](#test-the-configuration) 
> * [Realización del traslado](#perform-the-move-to-the-target-region-and-confirm) 
> * [Descarte de los recursos en la región de origen](#discard-the-resource-in-the-source-region) 

> [!IMPORTANT]
> Este documento le guía para trasladar las máquinas virtuales de Azure de una región a otra tal cual; si lo que se requiere es mejorar la disponibilidad al trasladar máquinas virtuales de un conjunto de disponibilidad a máquinas virtuales ancladas por zona de otra región, consulte el tutorial [aquí](move-azure-VMs-AVset-Azone.md).

## <a name="verify-prerequisites"></a>Verificación de los requisitos previos

- Asegúrese de tener las máquinas virtuales de Azure en la región de Azure desde la que va a realizar el traslado.
- Compruebe si [se admite la combinación de región de origen y región de destino que ha elegido](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) y tome una decisión informada sobre la región de destino.
- Asegúrese de entender la [arquitectura y los componentes del escenario](azure-to-azure-architecture.md).
- Revise las [limitaciones y los requisitos de compatibilidad](azure-to-azure-support-matrix.md).
- Compruebe los permisos de la cuenta: Si acaba de crear su cuenta de Azure gratuita, ya es administrador de su suscripción. Si no es administrador de la suscripción, solicite al administrador que le asigne los permisos que necesita. Para habilitar la replicación para una máquina virtual y finalmente copiar los datos mediante Azure Site Recovery, debe tener:

    1. Permisos para crear una máquina virtual en recursos de Azure. El rol integrado "Colaborador de la máquina virtual" tiene estos permisos, que incluyen:
        - Permiso para crear una máquina virtual en el grupo de recursos seleccionado.
        - Permiso para crear una máquina virtual en la red virtual seleccionada
        - Permiso para escribir en la cuenta de almacenamiento seleccionada

    2. También necesita permiso para administrar las operaciones de Azure Site Recovery. El rol "Colaborador de Site Recovery" tiene todos los permisos necesarios para administrar las operaciones de Site Recovery en un almacén de Recovery Services.

## <a name="prepare-the-source-vms"></a>Preparación de las máquinas virtuales de origen

1. Asegúrese de que todos los certificados raíz más recientes estén presentes en las máquinas virtuales de Azure que quiera trasladar. Si los últimos certificados raíz no están presentes, la copia de datos en la región de destino no se puede habilitar debido a las restricciones de seguridad.

    - Para las máquinas virtuales de Windows, instale las actualizaciones de Windows más recientes en la máquina virtual, de modo que todos los certificados raíz de confianza estén en ella. En un entorno desconectado, siga los procesos estándar de actualización de certificados y de Windows Update en su organización.
    - En las máquinas virtuales Linux, para obtener los certificados raíz de confianza y la lista de revocación de certificados en la máquina virtual, siga las instrucciones proporcionadas por su distribuidor de Linux.
2. Asegúrese de que no utiliza un proxy de autenticación para controlar la conectividad de red de las máquinas virtuales que quiere trasladar.
3. Si la máquina virtual que está intentando trasladar no tiene acceso a Internet y utiliza un proxy de firewall para controlar el acceso de salida, compruebe los requisitos [aquí](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).
4. Identifique el diseño de la red de origen y todos los recursos que está utilizando actualmente, incluidos entre otros equilibradores de carga, grupos de seguridad de red y dirección IP pública.

## <a name="prepare-the-target-region"></a>Preparación de la región de destino

1. Compruebe que su suscripción de Azure permite crear máquinas virtuales en la región de destino que se usa para la recuperación ante desastres. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

2. Asegúrese de que su suscripción tiene suficientes recursos para admitir máquinas virtuales con tamaños que se correspondan con las máquinas virtuales de origen. Si está usando Site Recovery para copiar datos en el destino, elija el mismo tamaño para la máquina virtual de destino o el más cercano posible.

3. Asegúrese de que crea un recurso de destino para cada componente identificado en el diseño de la red de origen. Esto es importante para asegurarse de que, tras la migración a la región de destino, las máquinas virtuales tengan toda la funcionalidad y características que tenían en el origen.

    > [!NOTE]
    > Azure Site Recovery automáticamente detecta y crea una red virtual cuando habilita la replicación para la máquina virtual de origen, o también puede crear previamente una red y asignarla a la máquina virtual del flujo de usuario para habilitar la replicación. Pero para cualquier otro recurso, como se menciona a continuación, es necesario crearlo manualmente en la región de destino.

     Consulte los siguientes documentos para crear los recursos de red más utilizados y que considere más relevantes, en función de la configuración de la máquina virtual de origen.

    - [Grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Equilibradores de carga](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    - [Dirección IP pública ](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
    Para cualquier otro componente de red, consulte la [documentación](https://docs.microsoft.com/azure/#pivot=products&panel=network) de red. 

4. Si desea probar la configuración antes de realizar la migración final a la región de destino, [cree manualmente una red sin producción](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) en la región de destino. Esto creará una interferencia mínima con producción y, por este motivo, se recomienda.
    
## <a name="copy-data-to-the-target-region"></a>Copia de datos en la región de destino
En los pasos siguientes aprenderá a utilizar Azure Site Recovery para copiar datos en la región de destino.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Cree el almacén en cualquier región, excepto en la de origen.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Seleccione **Crear un recurso** > **Herramientas de administración** > **Backup and Site Recovery**.
3. En el apartado **Nombre**, especifique el nombre descriptivo **ContosoVMVault**. Si tiene más de una suscripción, seleccione la más adecuada.
4. Cree un grupo de recursos denominado **ContosoRG**.
5. Especifique una región de Azure. Para comprobar las regiones admitidas, consulte la disponibilidad geográfica en [Detalles de precios de Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. En los almacenes de Recovery Services, haga clic en **Información general** > **ContosoVMVault** > **+Replicar**.
7. En **Origen**, seleccione **Azure**.
8. En **Ubicación de origen**, seleccione la región de Azure de origen donde se ejecutan actualmente sus máquinas virtuales.
9. Seleccione el modelo de implementación de Resource Manager. A continuación, seleccione la **suscripción de origen** y el **grupo de recursos de origen**.
10. Haga clic en **Aceptar** para guardar la configuración.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Habilite la replicación de máquinas virtuales de Azure y comience a copiar los datos.

Site Recovery recupera una lista de las máquinas virtuales asociadas a la suscripción y el grupo de recursos.

1. En el siguiente paso, Seleccione la máquina virtual que quiera trasladar. A continuación, haga clic en **Aceptar**.
3. En **Configuración**, haga clic en **Recuperación ante desastres**.
4. En **Configurar recuperación ante desastres** > **Región de destino**, seleccione la región de destino en la que quiere realizar la replicación.
5. Para este tutorial, acepte los valores predeterminados.
6. Haga clic en **Habilitar replicación**. Esto inicia un trabajo para habilitar la replicación de la máquina virtual.

    ![habilitar replicación](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="test-the-configuration"></a>Pruebe la configuración.


1. Vaya al almacén, en **Configuración** > **Elementos replicados**, haga clic en la máquina virtual que desea trasladar a la región de destino y, finalmente, haga clic en el icono **+Conmutación por error de prueba**.
2. En **Conmutación por error**, seleccione el punto de recuperación que usar para la conmutación por error:

   - **Procesado más recientemente**: error de la VM en el último punto de recuperación procesado por el servicio Site Recovery. Se muestra la marca de tiempo. Con esta opción, no se emplea tiempo en el procesamiento de datos, por lo que se proporciona un objetivo de tiempo de recuperación (RTO) bajo.
   - **Más reciente coherente con la aplicación**: esta opción conmuta por error todas las VM en el punto de recuperación más reciente coherente con la aplicación. Se muestra la marca de tiempo.
   - **Personalizado**: seleccione un punto de recuperación.

3. Seleccione la red virtual de Azure de destino a la que desea trasladar las máquinas virtuales de Azure para probar la configuración. 

> [!IMPORTANT]
> Le recomendamos que utilice una red de máquinas virtuales de Azure independiente para la conmutación por error de prueba y no la red de producción en la máquina virtual de destino a la que desea trasladar las máquinas virtuales con el tiempo. Esto se configuró cuando habilitó la replicación.

4. Para comenzar a probar el traslado, haga clic en **Aceptar**. Para realizar el seguimiento del progreso, haga clic en la máquina virtual para abrir sus propiedades. También puede hacer clic en el trabajo **Conmutación por error de prueba** en el nombre del almacén > **Configuración** > **Trabajos** > **Trabajos de Site Recovery**.
5. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal > **Virtual Machines**. Asegúrese de que la máquina virtual está en funcionamiento, tiene el tamaño adecuado y está conectada a la red apropiada.
6. Si desea eliminar la máquina virtual creada como parte de la prueba del traslado, haga clic en **Limpiar conmutación por error de prueba** en el elemento replicado. En **Notas**, registre y guarde las observaciones asociadas a la prueba.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Realice el traslado a la región de destino y confirme.

1.  Vaya al almacén, en **Configuración** > **Elementos replicados**, haga clic en la máquina virtual y, después, en **Conmutación por error**.
2. En **Conmutación por error**, seleccione **Más reciente**. 
3. Seleccione **Apague la máquina antes de comenzar con la conmutación por error**. A continuación, Site Recovery intentará apagar la máquina virtual de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Puede seguir el progreso de la conmutación por error en la página **Trabajos**. 
4. Una vez finalizado el trabajo, compruebe que la máquina virtual aparece en la región de Azure de destino como se esperaba.
5. En **Elementos replicados**, haga clic con el botón derecho en la máquina virtual > **Confirmar**. Esto finaliza el proceso de traslado a la región de destino. Espere hasta que finalice el trabajo de confirmación.

## <a name="discard-the-resource-in-the-source-region"></a>Descarte de los recursos en la región de origen 

1. Vaya a la máquina virtual.  Haga clic en **Deshabilitar replicación**.  Esto detiene el proceso de copiar los datos para la máquina virtual.  

> [!IMPORTANT]
> Es importante realizar este paso para evitar cargos por replicación de ASR.

En caso de que no tenga planes de volver a utilizar ninguno de los recursos de la fuente, continúe con los siguientes pasos.

1. ¿Desea eliminar todos los recursos de red pertinente en la región de origen que se enumeró como parte del paso 4 en [Preparar las máquinas virtuales de origen](#prepare-the-source-vms) 
2. Elimine la cuenta de almacenamiento correspondiente en la región de origen.



## <a name="next-steps"></a>Pasos siguientes

Siguiendo este tutorial ha trasladado una máquina virtual de Azure a otra región de Azure. Ya puede configurar la opción de recuperación ante desastres para la máquina virtual que ha trasladado.

> [!div class="nextstepaction"]
> [Configurar la recuperación ante desastres después de la migración](azure-to-azure-quickstart.md)

