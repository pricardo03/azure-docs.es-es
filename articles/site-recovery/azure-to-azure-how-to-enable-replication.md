---
title: Configuración de la replicación de máquinas virtuales de Azure en Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar la replicación de máquinas virtuales de Azure de una región de Azure a otra mediante Site Recovery.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: 79ad4b3598c227ad2c2e3b76562cf95a30e82ad3
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101554"
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Replicación de máquinas virtuales de Azure en otra región de Azure



En este artículo se describe cómo habilitar la replicación de máquinas virtuales de Azure de una región de Azure a otra.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se supone que ya configuró Site Recovery para este escenario como se describe en [Configuración de la recuperación ante desastres para las máquinas virtuales de Azure en una región secundaria de Azure (versión preliminar)](azure-to-azure-tutorial-enable-replication.md). Asegúrese de haber preparado los requisitos previos y de haber creado el almacén de Recovery Services.



## <a name="enable-replication"></a>Habilitar replicación

Habilite la replicación. En este procedimiento se supone que la región principal de Azure es Asia Pacífico, y la secundaria es Asia Suroriental.

1. En el almacén, haga clic en **+ Replicar**.
2. Especifique los siguientes campos:
    - **Origen**: el punto de origen de las máquinas virtuales, que en este caso es **Azure**.
    - **Ubicación de origen**: la región de Azure desde la que se quieren proteger las máquinas virtuales. En este ejemplo, la ubicación de origen es "Asia Oriental".
    - **Modelo de implementación**: el modelo de implementación de Azure de las máquinas de origen.
    - **Suscripción de origen**: suscripción a la que pertenecen sus máquinas virtuales de origen. Puede tratarse de cualquier suscripción dentro del mismo inquilino de Azure Active Directory donde exista el almacén de Recovery Services.
    - **Grupos de recursos**: el grupo de recursos al que pertenecen las máquinas virtuales de origen. Todas las máquinas virtuales del grupo de recursos seleccionado se enumeran para su protección en el paso siguiente.

    ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. En **Máquinas virtuales > Seleccionar máquinas virtuales**, haga clic en cada máquina virtual que quiera replicar y selecciónela. Solo puede seleccionar aquellas máquinas en las que se pueda habilitar la replicación. A continuación, haga clic en **Aceptar**.
    ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. En **Configuración**, tiene la opción de configurar el sitio de destino:

    - **Ubicación de destino**: ubicación donde se replicarán los datos de la máquina virtual de origen. Según la ubicación de las máquinas seleccionadas, Site Recovery proporcionará la lista de regiones de destino adecuadas. Se recomienda mantener como la ubicación de destino la misma ubicación del almacén de Recovery Services.
    - **Suscripción de destino**: la suscripción de destino utilizada para la recuperación ante desastres. De forma predeterminada, la suscripción de destino será la misma que la suscripción de origen.
    - **Grupo de recursos de destino**: el grupo de recursos al que pertenecen todas las máquinas virtuales replicadas. De forma predeterminada, Azure Site Recovery crea un nuevo grupo de recursos en la región de destino con un nombre con el sufijo "asr". En caso de que el grupo de recursos que cree Azure Site Recovery ya exista, se vuelve a usar. También puede elegir personalizarlo tal como se muestra en la sección siguiente. La ubicación del grupo de recursos de destino puede ser cualquier región de Azure excepto la región en la que se hospedan las máquinas virtuales de origen.
    - **Red virtual de destino**: de manera predeterminada, Site Recovery crea una red virtual en la región de destino con un nombre que tiene el sufijo "asr". Esta se asigna a la red de origen y se usa para todas las protecciones futuras. [más información](site-recovery-network-mapping-azure-to-azure.md) sobre la asignación de red.
    - **Cuentas de almacenamiento de destino (si la máquina virtual de origen no usa discos administrados)**: de forma predeterminada, Site Recovery crea una cuenta de almacenamiento de destino mediante la imitación de la configuración de almacenamiento de la máquina virtual de origen. En caso de que ya exista una cuenta de almacenamiento, esta se vuelve a usar.
    - **Discos administrados de réplica (si la máquina virtual de origen utiliza discos administrados)**: Site Recovery crea discos administrados de réplica en la región de destino para reflejar los discos administrados de la máquina virtual de origen con el mismo tipo de almacenamiento (Standard o Premium) que el disco administrado de la máquina virtual de origen.
    - **Cuentas de almacenamiento en caché**: Site Recovery necesita una cuenta de almacenamiento adicional denominada almacenamiento en caché en la región de origen. Todos los cambios que se producen en las máquinas virtuales de origen se siguen y se envían a la cuenta de almacenamiento en caché antes de su replicación en la ubicación de destino.
    - **Conjuntos de disponibilidad de destino**: de forma predeterminada, Azure Site Recovery crea un conjunto de disponibilidad en la región de destino con un nombre con el sufijo "asr" para la parte de máquina virtual de un conjunto de disponibilidad de la región de origen. En caso de que el conjunto de disponibilidad que cree Azure Site Recovery ya exista, se vuelve a usar.
    - **Zonas de disponibilidad de destino**: de forma predeterminada, Site Recovery asigna el mismo número de zona que la región de origen en la región de destino si esta admite zonas de disponibilidad.

    Si la región de destino no admite zonas de disponibilidad, las máquinas virtuales de destino se configurarán como instancias únicas de forma predeterminada. Si es necesario, puede configurar esas máquinas virtuales para que formen parte de los conjuntos de disponibilidad de la región de destino. Para ello, haga clic en "Personalizar".

    >[!NOTE]
    >No puede cambiar el tipo de disponibilidad (instancia única, zona de disponibilidad o conjunto de disponibilidad) después de habilitar la replicación. Tendrá que deshabilitar y habilitar la replicación para cambiar el tipo de disponibilidad.
    >
    
    - **Directiva de replicación**: define la configuración del historial de retención del punto de recuperación y la frecuencia de instantánea coherente con la aplicación. De manera predeterminada, Azure Site Recovery crea una directiva de replicación nueva con la configuración predeterminada de 24 horas para la retención del punto de recuperación y 60 minutos para la frecuencia de instantánea coherente con la aplicación.

    ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)
  
## <a name="customize-target-resources"></a>Personalizar los recursos de destino

Puede modificar la configuración de destino predeterminada utilizada por Site Recovery.

1. Haga clic en la opción **Personalizar:** que se encuentra al lado de "Suscripción de destino" para modificar la suscripción de destino predeterminada. Seleccione la suscripción en la lista de todas las suscripciones disponibles en el mismo inquilino de Azure Active Directory (AAD).

2. Haga clic en **Personalizar:** para modificar la configuración predeterminada:
    - En **Grupo de recursos de destino**, seleccione el grupo de recursos en la lista de todos los grupos de recursos que existen en la ubicación de destino de la suscripción.
    - En **Red virtual de destino**, seleccione la red en una lista de todas las redes virtuales en la ubicación de destino.
    - En **Conjunto de disponibilidad**, puede agregar la configuración del conjunto de disponibilidad a la máquina virtual en caso de que forme parte de un conjunto de disponibilidad de la región de origen.
    - En **Cuentas de almacenamiento de destino**, seleccione la cuenta que desea usar.

        ![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/customize.PNG)
1. Haga clic en **Personalizar:** para modificar la configuración de replicación.
   - En **Coherencia con múltiples máquinas virtuales**, seleccione las máquinas virtuales que desea replicar juntas. 
   - Todas las máquinas de un grupo de replicación tendrán puntos de recuperación compartidos coherentes con los bloqueos y coherentes con la aplicación cuando conmutan por error. Habilitar la coherencia de múltiples máquinas virtuales puede afectar al rendimiento de la carga de trabajo (ya que consume mucha CPU) y solo debe utilizarse si las máquinas ejecutan la misma carga de trabajo y necesita coherencia entre varias máquinas. Por ejemplo, si una aplicación tiene 2 máquinas virtuales SQL y 2 servidores web, solo debe agregar máquinas virtuales SQL como parte del grupo de replicación.
   - Puede elegir tener un máximo de 16 máquinas virtuales en un grupo de replicación.
   - Si habilita la coherencia entre varias máquinas virtuales, las máquinas del grupo de replicación se comunican entre sí a través del puerto 20004. Asegúrese de que no haya ninguna aplicación de firewall que bloquee la comunicación interna entre las máquinas virtuales en el puerto 20004. Si desea que las máquinas virtuales de Linux formen parte de un grupo de replicación, asegúrese de que el tráfico saliente en el puerto 20004 se abra manualmente según las instrucciones de la versión específica de Linux.
![Habilitar replicación](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)
    
2. Haga clic en **Crear recurso de destino** > **Habilitar replicación**.
3. Después de que las máquinas virtuales están habilitadas para la replicación, puede comprobar el estado de mantenimiento de las máquinas virtuales en **Elementos replicados**.

>[!NOTE]
>Durante la replicación inicial, el estado de la máquina virtual puede tardar un tiempo en actualizarse, sin ningún progreso. Haga clic en el botón **Actualizar** para ver el estado más reciente.
>

# <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
