---
title: Configuración de recuperación ante desastres para máquinas virtuales de Hyper-V locales en nubes VMM en Azure con Site Recovery | Microsoft Docs
description: Obtenga información sobre cómo configurar la recuperación ante desastres de máquinas virtuales de Hyper-V locales en nubes de System Center VMM en Azure mediante Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8c3c9347a027cccfaef6def84bfdc4c83555e98a
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966494"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Configuración de la recuperación ante desastres de las máquinas virtuales de Hyper-V locales en nubes de VMM en Azure

En este artículo se describe cómo habilitar la replicación de máquinas virtuales de Hyper-V locales administradas por System Center Virtual Machine Manager (VMM), para la recuperación ante desastres en Azure mediante el uso de la [Azure Site Recovery](site-recovery-overview.md) service. Si no usa VMM, [siga este tutorial](hyper-v-azure-tutorial.md) en su lugar.

Este es el tercer tutorial de una serie que muestra cómo configurar la recuperación ante desastres en Azure para máquinas virtuales locales de VMware. En el tutorial anterior, hemos [preparado el entorno de Hyper-V local](hyper-v-prepare-on-premises-tutorial.md) para recuperación ante desastres en Azure.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Seleccionar el origen y destino de la replicación.
> * Configurar el entorno de replicación de origen, incluidos los componentes de Site Recovery en el entorno local y el entorno de replicación de destino.
> * Configurar la asignación de red para la asignación entre las redes de VM de VMM y redes virtuales de Azure.
> * Crear una directiva de replicación.
> * Habilitar la replicación para una máquina virtual.

> [!NOTE]
> Los tutoriales muestran la ruta de implementación más sencilla para un escenario. Usan opciones predeterminadas siempre que es posible y no muestran todos los valores y las rutas de acceso posibles. Para obtener instrucciones detalladas, consulte los artículos en el **guías de procedimientos** sección de la [documentación sobre Site Recovery](https://docs.microsoft.com/en-us/azure/site-recovery).

## <a name="before-you-begin"></a>Antes de empezar

Este es el tercer tutorial de una serie. Se supone que ya ha completado las tareas en los tutoriales anteriores:

1. [Preparación de Azure](tutorial-prepare-azure.md)
2. [Preparación de un entorno de Hyper-V local](tutorial-prepare-on-premises-hyper-v.md)

## <a name="select-a-replication-goal"></a>Selección de un objetivo de replicación

1. En el portal de Azure, vaya a **almacenes de Recovery Services** y seleccione el almacén. Hemos preparado el almacén **ContosoVMVault** en el tutorial anterior.
2. En **Introducción**, seleccione **Site Recovery**y, a continuación, seleccione **preparar la infraestructura**.
3. En **objetivo de protección** > **donde están ubicadas las máquinas?**, seleccione **local**.
4. En **¿En dónde quiere replicar las máquinas?**, seleccione **En Azure**.
5. En **son las máquinas virtualizadas?**, seleccione **Sí, con Hyper-V**.
6. En **está usando System Center VMM para administrar los hosts de Hyper-V?**, seleccione **Sí**.
7.  Seleccione **Aceptar**.

    ![Objetivo de replicación](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Confirmación del planeamiento de la implementación

1. En **Planeamiento de implementación**, si planea una implementación de gran tamaño, descargue Deployment Planner para Hyper-V desde el vínculo de la página. [Obtenga más información](hyper-v-deployment-planner-overview.md) acerca del planeamiento de implementaciones de Hyper-V.
2. Para este tutorial, no necesitamos Deployment Planner. En **¿completó el planeamiento de implementación?**, seleccione **lo haré más tarde**y, a continuación, seleccione **Aceptar**.

## <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Al configurar el entorno de origen, instala al proveedor de Azure Site Recovery en el servidor VMM y registra el servidor en el almacén. Instale al agente Azure Recovery Services en cada host de Hyper-V.

1. En **preparar la infraestructura**, seleccione **origen**.
2. En **preparar origen**, seleccione **+ VMM** para agregar un servidor VMM. En **Agregar servidor**, compruebe que aparezca **System Center VMM server** (Servidor VMM de System Center) en **Tipo de servidor**.
3. Descargue el programa de instalación del proveedor de Microsoft Azure Site Recovery.
4. Descargue la clave de registro del almacén. Necesitará esta clave al ejecutar el programa de instalación del proveedor. La clave será válida durante cinco días a partir del momento en que se genera.
5. Descargue al instalador del agente de Microsoft Azure Recovery Services.

    ![Descargar el proveedor, la clave de registro y el agente](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Instalación del proveedor en el servidor VMM

1. En el Asistente para la instalación del proveedor de Azure Site Recovery > **Microsoft Update**, decida usar Microsoft Update para comprobar las actualizaciones del proveedor.
2. En **instalación**, acepte la ubicación de instalación predeterminada para el proveedor y seleccione **instalar**.
3. Después de la instalación, en el Asistente de registro de Microsoft Azure Site Recovery > **configuración del almacén**, seleccione **examinar**y en **el archivo de clave**, seleccione la clave del almacén de archivos que ha descargado.
4. Especifique la suscripción de Azure Site Recovery y el nombre del almacén (**ContosoVMVault**). Especifique un nombre descriptivo para identificar el servidor VMM en el almacén.
5. En **Configuración de proxy**, seleccione **Conectar directamente con Azure Site Recovery sin un servidor proxy**.
6. Acepte la ubicación predeterminada del certificado que se usa para cifrar los datos. Los datos cifrados se descifrarán cuando se realice la conmutación por error.
7. En **Sincronizar metadatos en la nube**, seleccione **Sincronizar los metadatos de la nube con el portal Site Recovery**. Esta acción debe ocurrir solamente una vez en cada servidor. Después, seleccione **Registrar**.
8. Una vez que el servidor está registrado en el almacén, seleccione **finalizar**.

Después de que finalice el registro, los metadatos del servidor se recuperan mediante Azure Site Recovery y el servidor VMM se muestra en **Site Recovery Infrastructure** (Infraestructura de Site Recovery).

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Instale al agente de Recovery Services en hosts de Hyper-V

Instale al agente en cada host de Hyper-V que contienen máquinas virtuales que desea replicar.

1. En el Asistente para la instalación de Microsoft Azure Recovery Services agente > **comprobación de requisitos previos**, seleccione **siguiente**. Los requisitos previos que falten se instalarán automáticamente.
2. En **configuración de la instalación**, acepte la ubicación de instalación y la ubicación de caché. La unidad de caché necesita al menos 5 GB de almacenamiento. Se recomienda una unidad con 600 GB o más de espacio disponible. Luego, seleccione **Instalar**.
3. En **instalación**, cuando finalice la instalación, seleccione **cerrar** para finalizar el asistente.

    ![Instalación del agente](./media/hyper-v-vmm-azure-tutorial/mars-install.png)

## <a name="set-up-the-target-environment"></a>Configuración del entorno de destino

1. Seleccione **Preparar infraestructura** > **Destino**.
2. Seleccione la suscripción y el grupo de recursos (**ContosoRG**)donde se crearán las máquinas virtuales de Azure después de la conmutación por error.
3. Seleccione el **Resource Manager** modelo de implementación.

Site Recovery comprueba que tiene una o más redes y cuentas de Azure Storage compatibles.

## <a name="configure-network-mapping"></a>Configurar asignación de red

1. En **infraestructura de Site Recovery** > **asignaciones de red** > **asignación de red**, seleccione el **+ asignación de red** icono.
2. En **Agregar asignación de red**, seleccione el servidor VMM de origen. Seleccione **Azure** como destino.
3. Compruebe la suscripción y el modelo de implementación después de la conmutación por error.
4. En **Red de origen**, seleccione la red de máquinas virtuales de origen local.
5. En **Red de destino**, seleccione la red de Azure en la que se ubicarán las máquinas virtuales de Azure replicadas cuando estas se creen después de la conmutación por error. Después, seleccione **Aceptar**.

    ![Asignación de red](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Configurar una directiva de replicación

1. Seleccione **preparar infraestructura** > **la configuración de replicación** > **+ crear y asociar**.
2. En **Crear y asociar directiva**, especifique un nombre de directiva. Usamos **ContosoReplicationPolicy**.
3. Deje la configuración predeterminada y seleccione **Aceptar**.
    - **Frecuencia de copia** indica que, después de la replicación inicial, los datos diferenciales se replicarán cada cinco minutos.
    - **Retención de punto de recuperación** indica que cada punto de recuperación se conservan durante dos horas.
    - **Frecuencia de instantánea coherente con la aplicación** indica que cada hora se crearán puntos de recuperación que contengan instantáneas coherentes con la aplicación.
    - **Hora de inicio de la replicación inicial** indica que la replicación inicial comenzará de inmediato.
    - **Cifrar datos almacenados en Azure** se establece en el valor predeterminado (**desactivar**) e indica que no se cifran los datos en reposo en Azure.
4. Una vez creada la directiva, seleccione **Aceptar**. Cuando se crea una nueva directiva, se asocia automáticamente con la nube de VMM.

## <a name="enable-replication"></a>Habilitar replicación

1. En **replicar la aplicación**, seleccione **origen**.
2. En **Origen**, seleccione la nube de VMM. Después, seleccione **Aceptar**.
3. En **destino**, compruebe el destino (Azure), la suscripción del almacén y seleccione el **Resource Manager** modelo.
4. Seleccione el **contosovmsacct1910171607** cuenta de almacenamiento y la **ContosoASRnet** red de Azure.
5. En **máquinas virtuales** > **seleccione**, seleccione la máquina virtual que desea replicar. Después, seleccione **Aceptar**.

   Puede hacer un seguimiento del progreso de la acción **Habilitar protección** en **Trabajos** > **Trabajos de Site Recovery**. Después de la **finalizar protección** trabajo finaliza, la replicación inicial completada y la máquina virtual está preparada para conmutación por error.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Exploración de la recuperación ante desastres](tutorial-dr-drill-azure.md)
