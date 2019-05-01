---
title: Configuración de la recuperación ante desastres de las máquinas virtuales de Hyper-V locales en nubes de VMM en Azure con Azure Site Recovery | Microsoft Docs
description: Obtenga información sobre cómo configurar la recuperación ante desastres de máquinas virtuales de Hyper-V locales en las nubes de VMM de System Center en Azure, con el servicio Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 64559f653ba8a466de7bec10db34383b508e3e4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60947557"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Configuración de la recuperación ante desastres de las máquinas virtuales de Hyper-V locales en nubes de VMM en Azure

En este artículo se describe cómo habilitar la replicación de máquinas virtuales de Hyper-V locales administradas por System Center Virtual Machine Manager (VMM), para la recuperación ante desastres en Azure mediante el [Azure Site Recovery](site-recovery-overview.md) service. Si no usa VMM, a continuación, [siga este tutorial](hyper-v-azure-tutorial.md).

Este es el tercer tutorial de una serie que muestra cómo configurar la recuperación ante desastres en Azure para máquinas virtuales locales de VMware. En el tutorial anterior, hemos [preparado el entorno de Hyper-V local](hyper-v-prepare-on-premises-tutorial.md) para recuperación ante desastres en Azure. 

En este tutorial, aprenderá a:


> [!div class="checklist"]
> * Seleccionar el origen y destino de la replicación.
> * Configurar el entorno de replicación de origen, incluidos los componentes de Site Recovery local y el entorno de replicación de destino.
> * Configurar la asignación de red para realizar asignaciones entre las redes de VM de VMM y las redes virtuales de Azure.
> * Creación de una directiva de replicación
> * Habilitación de la replicación para una máquina virtual


> [!NOTE]
> Los tutoriales muestran la ruta de implementación más sencilla para un escenario. Usan opciones predeterminadas siempre que es posible y no muestran todos los valores y las rutas de acceso posibles. Para obtener instrucciones detalladas, consulte el artículo de la sección de procedimientos de la tabla de contenido de Site Recovery.

## <a name="before-you-begin"></a>Antes de empezar

Este es el tercer tutorial de una serie. En este tutorial se da por hecho que ya ha realizado las tareas de los tutoriales anteriores:

1. [Preparación de Azure](tutorial-prepare-azure.md)
2. [Preparar el entorno local de Hyper-V](tutorial-prepare-on-premises-hyper-v.md) este es el tercer tutorial de una serie. En este tutorial se da por hecho que ya ha realizado las tareas de los tutoriales anteriores:


## <a name="select-a-replication-goal"></a>Selección de un objetivo de replicación

1. En **Almacenes de Recovery Services**, seleccione el almacén. Hemos preparado el almacén **ContosoVMVault** en el tutorial anterior.
2. En **Introducción**, haga clic en **Site Recovery**. Luego, haga clic en **Preparar infraestructura**.
3. En **objetivo de protección** > **donde están ubicadas las máquinas?**, seleccione **local**.
4. En **¿En dónde quiere replicar las máquinas?**, seleccione **En Azure**.
5. En **¿Las máquinas están virtualizadas?**, seleccione **Sí, con Hyper-V**.
6. En **Are you using System Center VMM** (¿Usa System Center VMM?), seleccione **Sí**. A continuación, haga clic en **Aceptar**.

    ![Objetivo de replicación](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)


## <a name="confirm-deployment-planning"></a>Confirmación del planeamiento de la implementación

1. En **Planeamiento de implementación**, si planea una implementación de gran tamaño, descargue Deployment Planner para Hyper-V desde el vínculo de la página. [Obtenga más información](hyper-v-deployment-planner-overview.md) acerca del planeamiento de implementaciones de Hyper-V.
2. Para este tutorial no se necesita Deployment Planner. En **¿completó el planeamiento de implementación?**, seleccione **lo haré más tarde**. A continuación, haga clic en **Aceptar**.


## <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Al configurar el entorno de origen, instale al proveedor de Azure Site Recovery en el servidor VMM y registrar el servidor en el almacén. Instale al agente Azure Recovery Services en cada host de Hyper-V. 

1. En **Preparar infraestructura**, haga clic en **Origen**.
2. En **Preparar origen**, haga clic en **+ VMM** para agregar un servidor VMM. En **Agregar servidor**, compruebe que aparezca **System Center VMM server** (Servidor VMM de System Center) en **Tipo de servidor**.
3. Descargue el programa de instalación del proveedor de Microsoft Azure Site Recovery.
4. Descargue la clave de registro del almacén. Se le solicitará cuando ejecute el programa de instalación del proveedor. La clave será válida durante cinco días a partir del momento en que se genera.
5. Descargue al instalador del agente de Microsoft Azure Recovery Services.

    ![Descargar](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Instalación del proveedor en el servidor VMM

1. En el Asistente para la instalación del proveedor de Azure Site Recovery > **Microsoft Update**, decida usar Microsoft Update para comprobar las actualizaciones del proveedor.
2. En **Instalación**, acepte la ubicación predeterminada de instalación para el proveedor y haga clic en **Instalar**. 
3. Después de la instalación, en el Asistente de registro de Microsoft Azure Site Recovery > **Configuración del almacén**, haga clic en **Examinar** y, en **Archivo de clave**, seleccione el archivo de clave del almacén que descargó.
4. Especifique la suscripción de Azure Site Recovery y el nombre del almacén (**ContosoVMVault**). Especifique un nombre descriptivo para identificar el servidor VMM en el almacén.
5. En **Configuración de proxy**, seleccione **Conectar directamente con Azure Site Recovery sin un servidor proxy**.
6. Acepte la ubicación predeterminada del certificado que se usa para cifrar los datos. Los datos cifrados se descifrarán cuando se realice la conmutación por error.
7. En **Sincronizar metadatos en la nube**, seleccione **Sincronizar los metadatos de la nube con el portal Site Recovery**. Esta acción solo se debe ejecutar una vez en cada servidor. Luego, haga clic en **Registrar**.
8. Una vez que el servidor se haya registrado en el almacén, haga clic en **Finalizar**.

Después de que finalice el registro, los metadatos del servidor se recuperan mediante Azure Site Recovery y el servidor VMM se muestra en **Site Recovery Infrastructure** (Infraestructura de Site Recovery).

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Instale al agente de Recovery Services en hosts de Hyper-V

Instale el agente en cada host de Hyper-V que contiene las máquinas virtuales que desea replicar.

1. En el Asistente para la instalación de Agente de Microsoft Azure Recovery Services > **Comprobación de requisitos previos**, haga clic en **Siguiente**. Todos los requisitos previos que falten se instalarán automáticamente.
2. En **Configuración de la instalación**, acepte la ubicación de instalación y la ubicación de la memoria caché. La unidad de caché necesita al menos 5 GB de almacenamiento. Se recomienda una unidad con 600 GB o más de espacio disponible. Luego haga clic en **Instalar**.
3. En **Instalación**, cuando finalice la instalación, haga clic en **Cerrar** para finalizar el asistente.

    ![Instalación del agente](./media/hyper-v-vmm-azure-tutorial/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>Configuración del entorno de destino

1. Haga clic en **Preparar infraestructura** > **Destino**.
2. Seleccione la suscripción y el grupo de recursos (**ContosoRG**)donde se crearán las máquinas virtuales de Azure después de la conmutación por error.
3. Seleccione el **Resource Manager** modelo de implementación.

Site Recovery comprueba que tiene una o más redes y cuentas de Azure Storage compatibles.


## <a name="configure-network-mapping"></a>Configurar asignación de red

1. En **Infraestructura de Site Recovery** > **Asignaciones de red** > **Asignación de red**, haga clic en el icono de **+Asignación de red**.
2. En **Agregar asignación de red**, seleccione el servidor VMM de origen. Seleccione **Azure** como destino.
3. Compruebe la suscripción y el modelo de implementación después de la conmutación por error.
4. En **Red de origen**, seleccione la red de máquinas virtuales de origen local.
5. En **Red de destino**, seleccione la red de Azure en la que se ubicarán las máquinas virtuales de Azure replicadas cuando estas se creen después de la conmutación por error. A continuación, haga clic en **Aceptar**.

    ![Asignación de red](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Configurar una directiva de replicación

1. Haga clic en **Preparar infraestructura** > **Configuración de la replicación** > **+Crear y asociar**.
2. En **Crear y asociar directiva**, especifique un nombre de directiva. Usamos **ContosoReplicationPolicy**.
3. Deje los valores predeterminados y haga clic en **Aceptar**.
    - **Frecuencia de copia** indica que los datos diferenciales (después de la replicación inicial) se replicarán cada cinco minutos.
    - **Retención de punto de recuperación** indica para cada punto de recuperación se conservan durante dos horas.
    - **Frecuencia de instantánea coherente con la aplicación** indica que cada hora se crearán puntos de recuperación que contengan instantáneas coherentes con la aplicación.
    - **Hora de inicio de la replicación inicial** indica que la replicación inicial comenzará de inmediato.
    - **Cifrar datos almacenados en Azure**: la configuración predeterminada **Off** (Desactivado) indica que los datos en reposo de Azure no están cifrados.
4. Una vez que se cree la directiva, haga clic en **Aceptar**. Cuando se crea una nueva directiva se asocia automáticamente con la nube de VMM.

## <a name="enable-replication"></a>Habilitar replicación

1. En **Replicar la aplicación**, haga clic en **Origen**. 
2. En **Origen**, seleccione la nube de VMM. A continuación, haga clic en **Aceptar**.
3. En **Destino**, compruebe que Azure aparece como el destino, la suscripción del almacén y seleccione el modelo de **Resource Manager**.
4. Seleccione la cuenta de almacenamiento **contosovmsacct1910171607** y la red de Azure **ContosoASRnet**.
5. En **Máquinas virtuales** > **Seleccionar**, seleccione la máquina virtual que desea replicar. A continuación, haga clic en **Aceptar**.

   Puede hacer un seguimiento del progreso de la acción **Habilitar protección** en **Trabajos** > **Trabajos de Site Recovery**. Una vez concluido el trabajo **Finalizar protección**, la replicación inicial finaliza y la máquina virtual está preparada para la conmutación por error.



## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Exploración de la recuperación ante desastres](tutorial-dr-drill-azure.md)
