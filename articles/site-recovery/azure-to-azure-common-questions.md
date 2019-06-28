---
title: Preguntas comunes sobre la recuperación ante desastres de Azure a Azure con Azure Site Recovery
description: En este artículo se responde a preguntas sobre la recuperación ante desastres de máquinas virtuales de Azure a otra región de Azure mediante Azure Site Recovery.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 04/29/2019
ms.topic: conceptual
ms.author: asgan
ms.openlocfilehash: 271e3c31c3e08d170add84ca4995f4876d4d3a33
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753777"
---
# <a name="common-questions-azure-to-azure-disaster-recovery"></a>Preguntas frecuentes: Recuperación ante desastres de Azure a Azure

En este artículo se proporcionan respuestas a preguntas comunes sobre la recuperación ante desastres de máquinas virtuales de Azure a otra región de Azure mediante [Site Recovery](site-recovery-overview.md). 


## <a name="general"></a>General

### <a name="how-is-site-recovery-priced"></a>¿Cómo se fija el precio de Site Recovery?
Consulte los detalles de los [precios de Azure Site Recovery](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/).
### <a name="how-does-the-free-tier-for-azure-site-recovery-work"></a>¿Cómo funciona el nivel Gratis de Azure Site Recovery?
Todas las instancias protegidas con Azure Site Recovery son gratuitas los primeros 31 días de protección. A partir del 32º día, la protección de la instancia se factura conforme a las tarifas que se indican.
### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>Durante los primeros 31 días, ¿puedo incurrir en otros cargos de Azure?
Sí, aunque Azure Site Recovery sea gratuito los primeros 31 días de una instancia protegida, puede incurrir en cargos por Azure Storage, transacciones de almacenamiento y transferencia de datos. Una máquina virtual recuperada también puede incurrir en cargos por proceso de Azure. Obtenga detalles completos sobre los precios [aquí](https://azure.microsoft.com/pricing/details/site-recovery).

### <a name="where-can-i-find-best-practices-for-azure-vm-disaster-recovery"></a>¿Dónde puedo encontrar los procedimientos recomendados para la recuperación ante desastres de máquinas virtuales de Azure? 
1. [Descripción de la arquitectura de Azure a Azure](azure-to-azure-architecture.md)
2. [Revisión de las configuraciones admitidas y no admitidas](azure-to-azure-support-matrix.md)
3. [Configuración de la recuperación ante desastres de máquinas virtuales de Azure](azure-to-azure-how-to-enable-replication.md)
4. [Ejecución de una conmutación por error de prueba](azure-to-azure-tutorial-dr-drill.md)
5. [Conmutación por error y conmutación por recuperación a la región principal](azure-to-azure-tutorial-failover-failback.md)

### <a name="how-is-capacity-guaranteed-in-the-target-region"></a>¿Cómo se garantiza la capacidad en la región de destino?
El equipo de Site Recovery trabaja con el equipo de administración de la capacidad de Azure para planear una capacidad de infraestructura suficiente y ayudar a garantizar que las máquinas virtuales protegidas por Site Recovery se implementarán correctamente en la región de destino cuando se inicie la conmutación por error.

## <a name="replication"></a>Replicación

### <a name="can-i-replicate-vms-enabled-through-azure-disk-encryption"></a>¿Puedo replicar máquinas virtuales habilitadas a través de Azure Disk Encryption?
Sí, puede replicarlas. Vea el artículo [Replicación de máquinas virtuales habilitadas para Azure Disk Encryption (ADE) en otra región de Azure](azure-to-azure-how-to-enable-replication-ade-vms.md). Actualmente, Azure Site Recovery solo admite máquinas virtuales de Azure con el sistema operativo Windows y que estén habilitadas para el cifrado con aplicaciones de Azure Active Directory (Azure AD).

### <a name="can-i-replicate-vms-to-another-subscription"></a>¿Se pueden replicar máquinas virtuales en otra suscripción?
Sí, puede replicar máquinas virtuales de Azure en otra suscripción que esté en el mismo inquilino de Azure AD.
Configurar la recuperación ante desastres [entre suscripciones](https://azure.microsoft.com/blog/cross-subscription-dr) es sencillo. Puede seleccionar otra suscripción en el momento de la replicación.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>¿Se pueden replicar máquinas virtuales de Azure ancladas por zona en otra región?
Sí, puede [replicar máquinas virtuales ancladas por zona](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) en otra región.

### <a name="can-i-exclude-disks"></a>¿Se pueden excluir discos?

Sí, puede excluir discos en el momento de la protección mediante PowerShell. Para más información, consulte este [artículo](azure-to-azure-exclude-disks.md).

### <a name="can-i-add-new-disks-to-replicated-vms-and-enable-replication-for-them"></a>¿Puedo agregar nuevos discos a máquinas virtuales replicadas y habilitar la replicación para ellos?

Sí, esto se admite para máquinas virtuales de Azure con discos administrados. Cuando se agrega un nuevo disco a una máquina virtual de Azure que está habilitada para la replicación, el estado de replicación de la máquina virtual muestra una advertencia, con una nota que especifica que uno o más discos en la máquina virtual están disponibles para la protección. Puede habilitar la replicación para los discos agregados.
- Si habilita la protección para los discos agregados, la advertencia desaparecerá después de la replicación inicial.
- Si decide no habilitar la replicación para el disco, puede seleccionar descartar la advertencia.
- Cuando se conmuta por error una máquina virtual a la que agrega un disco y habilita la replicación para él, los puntos de replicación mostrarán los discos que están disponibles para la recuperación. Por ejemplo, si una máquina virtual tiene un único disco y agrega uno nuevo, los puntos de replicación que se crearon antes de agregar el disco mostrarán que el punto de replicación se compone de "1 de 2 discos".

Site Recovery no admite la eliminación en caliente de un disco de una máquina virtual replicada. Si quita un disco de la máquina virtual, deberá deshabilitar la replicación y luego volver a habilitarla para la máquina virtual.


### <a name="how-often-can-i-replicate-to-azure"></a>¿Con qué frecuencia se puede replicar en Azure?
La replicación es continua cuando se replican máquinas virtuales de Azure en otra región de Azure. Para más información, consulte [Arquitectura de recuperación ante desastres de Azure a Azure](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process).

### <a name="can-i-replicate-virtual-machines-within-a-region-i-need-this-to-migrate-vms"></a>¿Se pueden replicar máquinas virtuales de una misma región? ¿Es necesario para migrar máquinas virtuales?
No puede usar una solución de recuperación ante desastres de Azure a Azure para replicar máquinas virtuales de una misma región.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>¿Se pueden replicar máquinas virtuales en cualquier región de Azure?
Con Site Recovery, puede replicar y recuperar máquinas virtuales entre dos regiones cualesquiera dentro del mismo clúster geográfico. Los clústeres geográficos se definen teniendo presente la latencia y la soberanía de datos. Para obtener más información, consulte la [matriz de regiones admitidas](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) de Site Recovery.

### <a name="does-site-recovery-require-internet-connectivity"></a>¿Site Recovery requiere conectividad a Internet?

No, Site Recovery no requiere conectividad a Internet. Pero sí requiere acceso a los intervalos IP y direcciones URL de Site Recovery, como se menciona en [este artículo](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges).

### <a name="can-i-replicate-the-application-having-separate-resource-group-for-separate-tiers"></a>¿Puedo replicar la aplicación con grupos de recursos independientes para niveles independientes?
Sí, puede replicar la aplicación y mantener también la configuración de recuperación ante desastres en un grupo de recursos independiente.
Por ejemplo, si tiene una aplicación con los niveles de aplicación, base de datos y web en grupos de recursos independientes, debe hacer clic tres veces en el [asistente para replicación](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication#enable-replication) para proteger todos los niveles. Site Recovery replicará estos tres niveles en tres grupos de recursos diferentes.

## <a name="replication-policy"></a>Directiva de replicación

### <a name="what-is-a-replication-policy"></a>¿Qué es una directiva de replicación?
Define la configuración del historial de retención de los puntos de recuperación y la frecuencia de instantáneas coherentes con la aplicación. De forma predeterminada, Azure Site Recovery crea una nueva directiva de replicación con la configuración predeterminada de:

* 24 horas para el historial de retención de puntos de recuperación.
* 60 minutos para la frecuencia de las instantáneas coherentes con la aplicación.

[Más información](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#configure-replication-settings).

### <a name="what-is-a-crash-consistent-recovery-point"></a>¿Qué es un punto de recuperación coherente con los bloqueos?
Un punto de recuperación coherente con los bloqueos representa los datos en disco como si la máquina virtual se hubiera bloqueado o el cable de alimentación se hubiera desconectado del servidor en el momento en que se ha tomado la instantánea. No incluye nada de lo que estaba en la memoria cuando se tomó la instantánea.

Hoy en día, la mayoría de las aplicaciones se pueden recuperar bien a partir de instantáneas coherentes con los bloqueos. Un punto de recuperación coherente con los bloqueos suele ser suficiente para aplicaciones y sistemas operativos que no tienen bases de datos, como los servidores de archivos, los servidores DHCP y los servidores de impresión.

### <a name="what-is-the-frequency-of-crash-consistent-recovery-point-generation"></a>¿Cuál es la frecuencia de generación de puntos de recuperación coherentes frente al bloqueo?
Site Recovery crea un punto de recuperación coherente con los bloqueos cada 5 minutos.

### <a name="what-is-an-application-consistent-recovery-point"></a>¿Qué es un punto de recuperación coherente con la aplicación?
Los puntos de recuperación coherentes con la aplicación se crean a partir de instantáneas coherentes con la aplicación. Los puntos de recuperación coherentes con la aplicación capturan los mismos datos que las instantáneas coherentes con los bloqueos, con la adición de todos los datos en memoria y todas las transacciones en curso.
Debido a su contenido adicional, las instantáneas coherentes con la aplicación son las más complejas y las que más tardan en ejecutarse. Se recomiendan puntos de recuperación coherentes con la aplicación para sistemas operativos de bases de datos y aplicaciones como SQL Server.

### <a name="what-is-the-impact-of-application-consistent-recovery-points-on-application-performance"></a>¿Cómo afectan los puntos de recuperación coherentes con la aplicación en el rendimiento de la aplicación?
Teniendo en cuenta que los puntos de recuperación coherentes con la aplicación capturan todos los datos en memoria y en curso, se requiere un marco de trabajo como VSS en las ventanas para poner la aplicación en modo inactivo. Si esto se hace con mucha frecuencia, puede afectar al rendimiento si la carga de trabajo ya está muy ocupada. Normalmente, se recomienda no utilizar una frecuencia baja para los puntos de recuperación coherentes con la aplicación para las cargas de trabajo que no sean de la base de datos e, incluso para este tipo de carga de trabajo, una hora es suficiente.

### <a name="what-is-the-minimum-frequency-of-application-consistent-recovery-point-generation"></a>¿Cuál es la frecuencia mínima de generación de puntos de recuperación coherentes frente a la aplicación?
Site Recovery puede crear un punto de recuperación coherente con la aplicación con una frecuencia mínima de 1 hora.

### <a name="how-are-recovery-points-generated-and-saved"></a>¿Cómo se generan y guardan los puntos de recuperación?
Para comprender cómo genera Site Recovery los puntos de recuperación, veamos un ejemplo de una directiva de replicación que tiene un período de retención del punto de recuperación de 24 horas y una instantánea de frecuencia coherente con la aplicación de 1 hora.

Site Recovery crea un punto de recuperación coherente con los bloqueos cada 5 minutos. El usuario no puede cambiar esta frecuencia. Por lo tanto, durante la última hora, el usuario tendrá 12 puntos coherentes con los bloqueos y 1 punto coherente con la aplicación entre los que elegir. Conforme transcurre el tiempo, Site Recovery elimina todos los puntos de recuperación de hace más de una hora y solo guarda un punto de recuperación por hora.

La siguiente captura de pantalla ilustra el ejemplo. En la captura de pantalla:

1. Durante un tiempo inferior a la última hora, hay puntos de recuperación con una frecuencia de 5 minutos.
2. Durante un tiempo superior a una hora, Site Recovery conserva solo un punto de recuperación.

   ![Lista de puntos de recuperación generados](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>¿Hasta cuánto tiempo atrás puedo recuperar?
El punto de recuperación más antiguo que puede usar es de 72 horas.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-a-problem-prevents-site-recovery-from-generating-recovery-points-for-more-than-24-hours-will-my-previous-recovery-points-be-lost"></a>¿Qué ocurrirá si tengo una directiva de replicación de 24 horas y un problema impide que Site Recovery genere puntos de recuperación durante más de 24 horas? ¿Se perderán mis puntos de recuperación anteriores?
No, Site Recovery mantendrá todos los puntos de recuperación anteriores. Dependiendo del periodo de retención de puntos de recuperación, 24 horas en este caso, Site Recovery reemplaza el punto más antiguo solo si hay una generación de nuevos puntos. En este caso, como no habrá ningún nuevo punto de recuperación generado como consecuencia de algún problema, todos los puntos antiguos permanecerán intactos una vez que finalice el periodo de retención.

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Después de habilitar la replicación en una máquina virtual, ¿cómo puedo cambiar la directiva de replicación?
Vaya a **Almacén de Site Recovery** > **Site Recovery Infrastructure (Infraestructura de Site Recovery)**  > **Directivas de replicación**. Seleccione la directiva que quiera editar y guarde los cambios. Todos los cambios se aplicarán también en todas las replicaciones existentes.

### <a name="are-all-the-recovery-points-a-complete-copy-of-the-vm-or-a-differential"></a>¿Todos los puntos de recuperación son una copia completa de la máquina virtual o diferencial?
El primer punto de recuperación que se genera tiene la copia completa. Los puntos de recuperación sucesivos tienen los cambios diferenciales.

### <a name="does-increasing-the-retention-period-of-recovery-points-increase-the-storage-cost"></a>¿Al aumentar el período de retención de los puntos de recuperación se aumenta el costo de almacenamiento?
Sí. Si aumenta el período de retención de 24 a 72 horas, Site Recovery guardará los puntos de recuperación durante 48 horas adicionales. El tiempo adicional implicará cargos de almacenamiento. Por ejemplo, si un único punto de recuperación tiene cambios diferenciales de 10 GB y un costo de 0,16 $ al mes por GB, esto implicaría unos cargos adicionales de 1,6 $ * 48 al mes.

## <a name="multi-vm-consistency"></a>Coherencia con múltiples máquinas virtuales

### <a name="what-is-multi-vm-consistency"></a>¿Qué es la coherencia con múltiples máquinas virtuales?
Significa asegurarse de que el punto de recuperación sea coherente en todas las máquinas virtuales replicadas.
Site Recovery proporciona una opción de "Coherencia con varias máquinas virtuales" que, cuando se selecciona, crea un grupo de replicación para replicar juntas todas las máquinas que forman parte del grupo.
Todas las máquinas virtuales tendrán puntos de recuperación compartidos coherentes con los bloqueos y coherentes con la aplicación cuando conmuten por error.
Repase el tutorial para [habilitar la coherencia con múltiples máquinas virtuales](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#enable-replication-for-a-vm).

### <a name="can-i-failover-single-virtual-machine-within-a-multi-vm-consistency-replication-group"></a>¿Puedo conmutar por error una sola máquina virtual de un grupo de replicación de coherencia con múltiples máquinas virtuales?
Al seleccionar la opción "Coherencia con varias máquinas virtuales", indica que la aplicación depende de todas las máquinas virtuales de un grupo. Por lo tanto, no se permite la conmutación por error de una sola máquina virtual.

### <a name="how-many-virtual-machines-can-i-replicate-as-a-part-of-a-multi-vm-consistency-replication-group"></a>¿Cuántas máquinas virtuales puedo replicar como parte de un grupo de replicación de coherencia con varias máquinas virtuales?
Puede replicar 16 máquinas virtuales juntas en un grupo de replicación.

### <a name="when-should-i-enable-multi-vm-consistency-"></a>¿Cuándo debo habilitar la coherencia con varias máquinas virtuales?
La habilitación de la coherencia entre con varias VM puede afectar al rendimiento de la carga de trabajo porque consume gran cantidad de CPU. Debe usarse únicamente si las máquinas ejecutan la misma carga de trabajo y necesita coherencia entre varias máquinas. Por ejemplo, si tiene dos instancias de SQL Server y dos servidores web en una aplicación, debe tener coherencia con varias máquinas virtuales solo para las instancias de SQL Server.


## <a name="failover"></a>Conmutación por error

### <a name="how-is-capacity-assured-in-target-region-for-azure-vms"></a>¿Cómo se garantiza la capacidad en la región de destino de las máquinas virtuales de Azure?
El equipo de Site Recovery trabaja con el equipo de administración de la capacidad de Azure para planear capacidad de infraestructura suficiente a fin de ayudar a garantizar que las máquinas virtuales habilitadas para la recuperación ante desastres se implementarán correctamente en la región de destino cuando se inicie la conmutación por error.

### <a name="is-failover-automatic"></a>¿La conmutación por error es automática?

La conmutación por error no es automática. Puede iniciar las conmutaciones por error con solo un clic en el portal o bien puede usar [PowerShell](azure-to-azure-powershell.md) para desencadenar una conmutación por error.

### <a name="can-i-retain-a-public-ip-address-after-failover"></a>¿Se puede conservar una dirección IP pública después de la conmutación por error?

La dirección IP pública de la aplicación de producción no se puede retener tras la conmutación por error.
- Las cargas de trabajo que nos llegan como parte del proceso de conmutación por error deben tener asignado un recurso de dirección IP pública de Azure disponible en la región de destino.
- Puede realizar este paso manualmente o automatizarlo con un plan de recuperación.
- Aprenda a [configurar direcciones IP públicas tras la conmutación por error](concepts-public-ip-address-with-site-recovery.md#public-ip-address-assignment-using-recovery-plan).  

### <a name="can-i-retain-a-private-ip-address-during-failover"></a>¿Se puede conservar una dirección IP privada durante la conmutación por error?
Sí, puede conservar una dirección IP privada. De forma predeterminada, cuando se habilita la recuperación ante desastres para máquinas virtuales de Azure, Site Recovery crea recursos de destino en función de la configuración de los recursos de origen. En el caso de las máquinas virtuales de Azure configuradas con direcciones IP estáticas, Site Recovery trata de aprovisionar la misma dirección IP para la máquina virtual de destino, si no está en uso.
Aprenda sobre cómo [conservar las direcciones IP durante la conmutación por error](site-recovery-retain-ip-azure-vm-failover.md).

### <a name="after-failover-why-is-the-server-assigned-a-new-ip-address"></a>Después de la conmutación por error, ¿por qué se asigna al servidor una nueva dirección IP?

Site Recovery trata de proporcionar la dirección IP en el momento de la conmutación por error. Si otra máquina virtual se lleva esa dirección, Site Recovery establece la siguiente dirección IP disponible como destino.
Aprenda a [configurar asignación de red y direccionamiento IP para redes virtuales](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms).

### <a name="what-are-latest-lowest-rpo-recovery-points"></a>¿Qué son los puntos de recuperación **Más reciente (RPO más bajo)** ?
La opción **Más reciente (RPO más bajo)** procesa primero todos los datos que se han enviado al servicio Site Recovery para crear un punto de recuperación para cada máquina virtual antes de conmutarla por error a dicho punto de recuperación. Esta opción ofrece el objetivo de punto de recuperación mínimo, ya que la máquina virtual creada después de la conmutación por error tiene todos los datos replicados en Site Recovery al desencadenarse la conmutación por error.

### <a name="do-latest-lowest-rpo-recovery-points-have-an-impact-on-failover-rto"></a>¿Los puntos de recuperación con la opción **Más reciente (RPO más bajo)** afectan al RTO de conmutación por error?
Sí. Site Recovery procesa todos los datos pendientes antes de conmutar por error, por lo que esta opción tiene un mayor objetivo de tiempo de recuperación (RTO) en comparación con otras opciones.

### <a name="what-does-the-latest-processed-option-in-recovery-points-mean"></a>¿Qué significa la opción **Procesado más recientemente** en los puntos de recuperación?
La opción **Procesado más recientemente** realiza una conmutación por error de todas las máquinas virtuales del plan al último punto de recuperación procesado por Site Recovery. Para ver el último punto de recuperación de una máquina virtual específica, seleccione **Puntos de recuperación más recientes** en la configuración de la máquina virtual. Esta opción proporciona un objetivo de tiempo de recuperación bajo, ya que no se invierte tiempo en el procesamiento de datos sin procesar.

### <a name="what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>¿Qué ocurre si mi región primaria experimenta una interrupción inesperada?
Puede desencadenar una conmutación por error después de la interrupción. Site Recovery no necesita conectividad desde la región primaria para realizar la conmutación por error.

### <a name="what-is-a-rto-of-a-vm-failover-"></a>¿Qué es un RTO de una conmutación por error de una máquina virtual?
Site Recovery tiene un [objetivo de tiempo de recuperación de 2 horas según el contrato de nivel de servicio](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/). Sin embargo, la mayoría de las veces, Site Recovery conmuta por error las máquinas virtuales en cuestión de minutos. Para calcular el objetivo de tiempo de recuperación, vaya a los trabajos de conmutación por error, que muestran el tiempo que tardó en aparecer la VM. Para el objetivo de tiempo de recuperación del plan de recuperación, consulte la siguiente sección.

## <a name="recovery-plans"></a>Planes de recuperación

### <a name="what-is-a-recovery-plan"></a>¿Qué es un plan de recuperación?
Un plan de recuperación en Site Recovery coordina la recuperación de conmutación por error de máquinas virtuales. Ayuda a que la recuperación sea coherente y precisa, repetible y automatizada. Un plan de recuperación aborda las siguientes necesidades del usuario:

- Definición de un grupo de máquinas virtuales que conmutan por error de forma conjunta
- Definición de las dependencias entre las máquinas virtuales para que la aplicación aparezca con precisión
- Automatización de la recuperación junto con acciones manuales personalizadas para lograr tareas distintas de la conmutación por error de las máquinas virtuales

[Obtenga más información](site-recovery-create-recovery-plans.md) sobre los planes de recuperación.

### <a name="how-is-sequencing-achieved-in-a-recovery-plan"></a>¿Cómo se logra la secuenciación en un plan de recuperación?

En un plan de recuperación, puede crear varios grupos para lograr la secuenciación. Todos los grupos conmutan por error a la vez. Las máquinas virtuales que forman parte del mismo grupo conmutan por error a la vez, y luego otro grupo. Para obtener información sobre cómo modelar una aplicación mediante el uso de un plan de recuperación, consulte [Acerca de los planes de recuperación](recovery-plan-overview.md#model-apps).

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>¿Cómo se puede encontrar el RTO de un plan de recuperación?
Para comprobar el RTO de un plan de recuperación, haga una conmutación por error de prueba del plan de recuperación y vaya a **Trabajos de Site Recovery**.
En el ejemplo siguiente, el trabajo denominado SAPTestRecoveryPlan tardó 8 minutos y 59 segundos en conmutar por error todas las máquinas virtuales y realizar acciones especificadas.

![Lista de trabajos de Site Recovery](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>¿Se pueden agregar automáticamente runbooks al plan de recuperación?
Sí, puede integrar runbooks de Azure Automation en el plan de recuperación. [Más información](site-recovery-runbook-automation.md).

## <a name="reprotection-and-failback"></a>Reprotección y conmutación por recuperación

### <a name="after-a-failover-from-the-primary-region-to-a-disaster-recovery-region-are-vms-in-a-dr-region-protected-automatically"></a>Después de realizar una conmutación por error de la región primaria en la región de recuperación ante desastres, ¿se protegen de forma automática las máquinas virtuales en una región de recuperación ante desastres?
No. No, al [conmutar por error](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) las máquinas virtuales de Azure desde una región a otra, las máquinas virtuales se inician en la región de recuperación ante desastres con un estado desprotegido. Para conmutar por recuperación las máquinas virtuales en la región primaria, tiene que [volver a protegerlas](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) en la región secundaria.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-the-secondary-region-to-the-primary-region"></a>En el momento de la reprotección, ¿Site Recovery replica los datos completos de la región secundaria en la región primaria?
Depende de la situación. Por ejemplo, si la máquina virtual de la región de origen existe, solo se sincronizan los cambios entre el disco de origen y el disco de destino. Site Recovery calcula las copias de seguridad diferenciales mediante la comparación de los discos y, a continuación, transfiere los datos. Este proceso suele tardar unas horas. Para obtener más información sobre lo que ocurre durante la reprotección, consulte [Reprotección de máquinas virtuales de Azure conmutadas por error en la región principal]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection).

### <a name="how-much-time-does-it-take-to-fail-back"></a>¿Cuánto se tarda en conmutar por recuperación?
Después de la reprotección, la cantidad de tiempo para la conmutación por recuperación es normalmente similar al tiempo que fue necesario para la conmutación por error de la región primaria a una región secundaria.

## <a name="capacity"></a>Capacidad

### <a name="how-is-capacity-assured-in-target-region-for-azure-vms"></a>¿Cómo se garantiza la capacidad en la región de destino de las máquinas virtuales de Azure?
El equipo de Site Recovery trabaja con el equipo de administración de la capacidad de Azure para planear capacidad de infraestructura suficiente a fin de ayudar a garantizar que las máquinas virtuales habilitadas para la recuperación ante desastres se implementarán correctamente en la región de destino cuando se inicie la conmutación por error.

### <a name="does-site-recovery-work-with-reserved-instances"></a>¿Funciona Site Recovery con instancias reservadas?
Sí, puede comprar [instancias reservadas](https://azure.microsoft.com/pricing/reserved-vm-instances/) en la región de recuperación ante desastres, que se usarán en las operaciones de conmutación por error de Site Recovery. </br> No se necesita ninguna configuración adicional.


## <a name="security"></a>Seguridad

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>¿Se envían mis datos de replicación al servicio de Site Recovery?
No, Site Recovery no intercepta los datos replicados ni tiene información sobre lo que se ejecuta en las máquinas virtuales. Únicamente se envían los metadatos necesarios para coordinar la replicación y la conmutación por error al servicio Site Recovery.  
Site Recovery está certificado según la norma ISO 27001:2013, 27018, además de HIPAA y DPA, y está completando sus evaluaciones de SOC2 y FedRAMP JAB.

### <a name="does-site-recovery-encrypt-replication"></a>¿Site Recovery cifra la replicación?
Sí, se admite tanto el cifrado en tránsito como el [cifrado en reposo de Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption).

## <a name="next-steps"></a>Pasos siguientes
* [Revise](azure-to-azure-support-matrix.md) los requisitos de compatibilidad.
* [Configure](azure-to-azure-tutorial-enable-replication.md) la replicación de Azure en Azure.
- Si tiene alguna pregunta después de leer el artículo, publíquela en el [Foro de Azure Recovery Services](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).
