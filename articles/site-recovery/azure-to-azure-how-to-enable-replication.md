---
title: Configuración de la replicación de máquinas virtuales de Azure en Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar la replicación de máquinas virtuales de Azure de una región de Azure a otra mediante Site Recovery.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: asgang
ms.openlocfilehash: 86bd41d518006b0601a5c9d18e5429f76d5a4fc5
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926663"
---
# <a name="replicate-azure-vms-to-another-azure-region"></a>Replicación de máquinas virtuales de Azure en otra región de Azure


En este artículo se describe cómo habilitar la replicación de máquinas virtuales de Azure de una región de Azure a otra.

## <a name="before-you-start"></a>Antes de comenzar

En este artículo se da por supuesto que ha preparado para la implementación de Site Recovery, como se describe en el [tutorial de recuperación ante desastres en Azure](azure-to-azure-tutorial-enable-replication.md).

Requisitos previos deben estar en su lugar, y que debería haber creado un almacén de Recovery Services.


## <a name="enable-replication"></a>Habilitar replicación

Habilite la replicación. En este procedimiento se supone que la región principal de Azure es Asia Pacífico, y la secundaria es Asia Suroriental.

1. En el almacén, haga clic en **+ Replicar**.
2. Especifique los siguientes campos:
   - **Origen**: el punto de origen de las máquinas virtuales, que en este caso es **Azure**.
   - **Ubicación de origen**: La región desde donde desea proteger las máquinas virtuales de Azure. En este ejemplo, la ubicación de origen es "Asia Oriental".
   - **Modelo de implementación**: el modelo de implementación de Azure de las máquinas de origen.
   - **Suscripción de origen**: La suscripción a la que pertenecen las máquinas virtuales de origen. Puede tratarse de cualquier suscripción dentro del mismo inquilino de Azure Active Directory donde exista el almacén de Recovery Services.
   - **Grupos de recursos**: el grupo de recursos al que pertenecen las máquinas virtuales de origen. Todas las máquinas virtuales del grupo de recursos seleccionado se enumeran para su protección en el paso siguiente.

     ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. En **Máquinas virtuales > Seleccionar máquinas virtuales**, haga clic en cada máquina virtual que quiera replicar y selecciónela. Solo puede seleccionar aquellas máquinas en las que se pueda habilitar la replicación. A continuación, haga clic en **Aceptar**.
    ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. En **Configuración**, tiene la opción de configurar el sitio de destino:

   - **Ubicación de destino**: ubicación donde se replicarán los datos de la máquina virtual de origen. Según la ubicación de las máquinas seleccionadas, Site Recovery proporcionará la lista de regiones de destino adecuadas. Se recomienda mantener como la ubicación de destino la misma ubicación del almacén de Recovery Services.
   - **Suscripción de destino**: la suscripción de destino utilizada para la recuperación ante desastres. De forma predeterminada, la suscripción de destino será la misma que la suscripción de origen.
   - **Grupo de recursos de destino**: el grupo de recursos al que pertenecen todas las máquinas virtuales replicadas.
       - De forma predeterminada Site Recovery crea un nuevo grupo de recursos en la región de destino con un sufijo "asr" en el nombre.
       - Si el grupo de recursos creado por Site Recovery ya existe, se vuelve a usar.
       - Puede personalizar la configuración del grupo de recursos.
       - La ubicación del grupo de recursos de destino puede ser cualquier región de Azure, excepto la región en la que se hospedan las VM de origen.
   - **Red virtual de destino**: De forma predeterminada, Site Recovery crea una nueva red virtual en la región de destino con un sufijo "asr" en el nombre. Esta se asigna a la red de origen y se usa para todas las protecciones futuras. [más información](site-recovery-network-mapping-azure-to-azure.md) sobre la asignación de red.
   - **Cuentas de almacenamiento (máquina virtual de origen no usa discos administrados) de destino**: de forma predeterminada, Site Recovery crea una cuenta de almacenamiento de destino mediante la imitación de la configuración de almacenamiento de la máquina virtual de origen. En caso de que ya exista una cuenta de almacenamiento, esta se vuelve a usar.
   - **Discos administrados de réplica (máquina virtual de origen utiliza discos administrados)**: Site Recovery crea nuevos discos administrados de réplica en la región de destino para reflejar los discos administrados de la máquina virtual de origen con el mismo tipo de almacenamiento (estándar o premium) que la máquina virtual de origen disco administrado.
   - **Cuentas de almacenamiento en caché**: Site Recovery necesita una cuenta de almacenamiento adicional denominada almacenamiento en caché en la región de origen. Todos los cambios que se producen en las máquinas virtuales de origen se siguen y se envían a la cuenta de almacenamiento en caché antes de su replicación en la ubicación de destino.
   - **Conjuntos de disponibilidad de destino**: De forma predeterminada, Site Recovery crea un nuevo conjunto de disponibilidad en la región de destino con el sufijo "asr" en el nombre, las máquinas virtuales que forman parte de un conjunto de disponibilidad en la región de origen. Si el conjunto de disponibilidad creado por Site Recovery ya exista, se vuelve a usar.
   - **Zonas de disponibilidad de destino**: de forma predeterminada, Site Recovery asigna el mismo número de zona que la región de origen en la región de destino si esta admite zonas de disponibilidad.

     Si la región de destino no admite zonas de disponibilidad, las máquinas virtuales de destino se configurarán como instancias únicas de forma predeterminada. Si es necesario, puede configurar esas máquinas virtuales para que formen parte de los conjuntos de disponibilidad de la región de destino. Para ello, haga clic en "Personalizar".

     >[!NOTE]
     >No puede cambiar el tipo de disponibilidad (instancia única, zona de disponibilidad o conjunto de disponibilidad) después de habilitar la replicación. Tendrá que deshabilitar y habilitar la replicación para cambiar el tipo de disponibilidad.
     >
    
   - **Directiva de replicación**: define la configuración del historial de retención del punto de recuperación y la frecuencia de instantánea coherente con la aplicación. De manera predeterminada, Azure Site Recovery crea una directiva de replicación nueva con la configuración predeterminada de 24 horas para la retención del punto de recuperación y 60 minutos para la frecuencia de instantánea coherente con la aplicación.

     ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

### <a name="enable-replication-for-added-disks"></a>Habilitar la replicación de los discos agregados

Si se agregan discos a una máquina virtual de Azure para que la replicación está habilitada, ocurre lo siguiente:
-   Estado de replicación de la máquina virtual muestra una advertencia y le informa de una nota que le indica que uno o más discos están disponibles para la protección.
-   Si habilita la protección para los discos agregados, la advertencia desaparecerá después de la replicación inicial del disco.
-   Si decide no habilitar la replicación para el disco, puede seleccionar para descartar la advertencia.

    
    ![Nuevo disco se ha agregado](./media/azure-to-azure-how-to-enable-replication/newdisk.png)

Para habilitar la replicación para un disco agregado, haga lo siguiente:

1.  En el almacén > **elementos replicados**, haga clic en la máquina virtual al que agregó el disco.
2.  Haga clic en **discos**y, a continuación, seleccione el disco de datos para el que desea habilitar la replicación (estos discos tienen un **no protegido** estado).
3.  En **los detalles del disco**, haga clic en **habilitar la replicación**.

    ![Habilitar la replicación de disco agregado](./media/azure-to-azure-how-to-enable-replication/enabled-added.png)

Después se ejecuta el trabajo de habilitar la replicación y finalice la replicación inicial, se quita la advertencia de mantenimiento de replicación para el problema de disco.


  
## <a name="customize-target-resources"></a>Personalizar los recursos de destino

Puede modificar la configuración de destino predeterminada utilizada por Site Recovery.

1. Haga clic en la opción **Personalizar:** que se encuentra al lado de "Suscripción de destino" para modificar la suscripción de destino predeterminada. Seleccione la suscripción en la lista de todas las suscripciones disponibles en el mismo inquilino de Azure Active Directory (AAD).

2. Haga clic en **Personalizar:** para modificar la configuración predeterminada:
    - En **Grupo de recursos de destino**, seleccione el grupo de recursos en la lista de todos los grupos de recursos que existen en la ubicación de destino de la suscripción.
    - En **Red virtual de destino**, seleccione la red en una lista de todas las redes virtuales en la ubicación de destino.
    - En **Conjunto de disponibilidad**, puede agregar la configuración del conjunto de disponibilidad a la máquina virtual en caso de que forme parte de un conjunto de disponibilidad de la región de origen.
    - En **Cuentas de almacenamiento de destino**, seleccione la cuenta que desea usar.

        ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/customize.PNG)
3. Haga clic en **Personalizar:** para modificar la configuración de replicación.
4. En **coherencia de múltiples VM**, seleccione las máquinas virtuales que desea replicar entre sí.
    - Todas las máquinas de un grupo de replicación tendrán puntos de recuperación compartidos coherentes con los bloqueos y coherentes con la aplicación cuando conmutan por error.
    - Habilitación de la coherencia de varias máquinas virtuales puede afectar el rendimiento de carga de trabajo (como mucho la CPU). Sólo debe habilitarse si las máquinas ejecutan la misma carga de trabajo y necesita coherencia entre varias máquinas.
    - Por ejemplo, si una aplicación tiene 2 máquinas virtuales de SQL Server y dos servidores web, debe agregar sólo la VM de SQL Server a un grupo de replicación.
    - Puede tener un máximo de 16 máquinas virtuales en un grupo de replicación.
    - Si habilita la coherencia entre varias máquinas virtuales, las máquinas del grupo de replicación se comunican entre sí a través del puerto 20004.
    - Asegúrese de que no hay ninguna aplicación de firewall que bloquee la comunicación interna entre las máquinas virtuales a través del puerto 20004.
    - Si desea que las máquinas virtuales de Linux para formar parte de un grupo de replicación, asegúrese de que el tráfico saliente en el puerto 20004 se abra manualmente según las instrucciones para la versión específica de Linux.
![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)
    
5. Haga clic en **Crear recurso de destino** > **Habilitar replicación**.
6. Después de que las máquinas virtuales están habilitadas para la replicación, puede comprobar el estado de mantenimiento de las máquinas virtuales en **Elementos replicados**.

>[!NOTE]
>Durante la replicación inicial, el estado de la máquina virtual puede tardar un tiempo en actualizarse, sin ningún progreso. Haga clic en el botón **Actualizar** para ver el estado más reciente.
>

# <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
