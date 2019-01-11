---
title: 'Preguntas comunes: recuperación ante desastres de Azure a Azure con Azure Site Recovery | Microsoft Docs'
description: En este artículo se resumen las preguntas comunes al configurar la recuperación ante desastres de máquinas virtuales de Azure a otra región de Azure mediante Azure Site Recovery.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: 6fe7152e43640a809ab9f4de39b1c6b599975a20
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969954"
---
# <a name="common-questions---azure-to-azure-replication"></a>Preguntas comunes: replicación de Azure a Azure

En este artículo se proporcionan respuestas a preguntas comunes que podemos observar al implementar la recuperación ante desastres de máquinas virtuales de Azure en otra región de Azure. Si tiene alguna pregunta después de leer el artículo, publíquela en el [Foro de Azure Recovery Services](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>General
### <a name="how-is-site-recovery-priced"></a>¿Cómo se fija el precio de Site Recovery?
Consulte los detalles de los [precios de Azure Site Recovery](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/).

### <a name="how-to-configure-site-recovery-on-azure-vms-what-are-the-best-practices"></a>Cómo configurar Site Recovery en máquinas virtuales de Azure. ¿Cuáles son los procedimientos recomendados?
1. [Descripción de la arquitectura de Azure a Azure](azure-to-azure-architecture.md)
2. [Revisión de las configuraciones admitidas y no admitidas](azure-to-azure-support-matrix.md)
3. [Configuración de la recuperación ante desastres de máquinas virtuales de Azure](azure-to-azure-how-to-enable-replication.md)
4. [Ejecución de una conmutación por error de prueba](azure-to-azure-tutorial-dr-drill.md)
5. [Conmutación por error y por recuperación a la región primaria](azure-to-azure-tutorial-failover-failback.md)

## <a name="replication"></a>Replicación

### <a name="can-i-replicate-azure-disk-encryption-enabled-vms"></a>¿Se pueden replicar máquinas virtuales habilitadas para Azure Disk Encryption?
Sí, puede replicarlas. Consulte este [artículo](azure-to-azure-how-to-enable-replication-ade-vms.md) para habilitar la replicación de máquinas virtuales habilitadas para Azure Disk Encryption (ADE). Tenga en cuenta que actualmente Azure Site Recovery solo admite máquinas virtuales de Azure con el sistema operativo Windows y que estén habilitadas para el cifrado con la aplicación de Azure AD.

### <a name="can-i-replicate-vms-to-another-subscription"></a>¿Se pueden replicar máquinas virtuales en otra suscripción?
Sí, puede replicar máquinas virtuales de Azure en otra suscripción que esté en el mismo inquilino de Azure Active Directory.
Configurar la recuperación ante desastres [entre suscripciones](https://azure.microsoft.com/blog/cross-subscription-dr) es sencillo. Puede seleccionar otra suscripción en el momento de la replicación.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>¿Se pueden replicar máquinas virtuales de Azure ancladas por zona en otra región?
Sí, puede [replicar máquinas virtuales ancladas por zona](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) en otra región.

### <a name="can-i-exclude-disks"></a>¿Se pueden excluir discos?

Sí, puede excluir discos en el momento de la protección mediante PowerShell. Consulte la [guía de PowerShell](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine) para excluir discos.

###<a name="how-often-can-i-replicate-to-azure"></a>¿Con qué frecuencia se puede replicar en Azure?
La replicación es continua cuando se replican máquinas virtuales de Azure en otra región de Azure. Consulte la arquitectura de replicación de [Azure a Azure](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process) para comprender los detalles.

### <a name="can-i-replicate-virtual-machines-within-a-same-region-i-need-this-to-migrate-vms"></a>¿Se pueden replicar máquinas virtuales de una misma región? ¿Es necesario para migrar máquinas virtuales?
No puede usar la solución de recuperación ante desastres de Azure a Azure para replicar máquinas virtuales de una misma región.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>¿Se pueden replicar máquinas virtuales en cualquier región de Azure?
Con Site Recovery, puede replicar y recuperar máquinas virtuales entre dos regiones cualesquiera dentro del mismo clúster geográfico. Los clústeres geográficos se definen teniendo presente la latencia y la soberanía de datos. Para obtener más información, consulte la [matriz de regiones admitidas](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) de Site Recovery.

### <a name="does-site-recovery-require-internet-connectivity"></a>¿Site Recovery requiere conectividad a Internet?

No, Site Recovery no requiere conectividad a Internet, sino acceso a direcciones URL e intervalos IP de Site Recovery como se menciona en este [artículo](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges).

## <a name="replication-policy"></a>Directiva de replicación

### <a name="what-is-a-replication-policy"></a>¿Qué es una directiva de replicación?
define la configuración del historial de retención del punto de recuperación y la frecuencia de instantánea coherente con la aplicación. De manera predeterminada, Azure Site Recovery crea una directiva de replicación nueva con la configuración predeterminada de 24 horas para la retención del punto de recuperación y 60 minutos para la frecuencia de instantánea coherente con la aplicación.

### <a name="what-is-crash-consistent-recovery-point"></a>¿Qué es un punto de recuperación coherente con los bloqueos?
Un punto de recuperación coherente con los bloqueos representa los datos en disco como si la máquina virtual se hubiera bloqueado o el cable de alimentación se hubiera desconectado del servidor en el momento en que se ha tomado la instantánea. No incluye nada de lo que estaba en la memoria cuando se tomó la instantánea. Hoy en día, la mayoría de las aplicaciones se pueden recuperar bien a partir de instantáneas coherentes con los bloqueos. Un punto de recuperación coherente con los bloqueos suele ser suficiente para aplicaciones y sistemas operativos que no tienen bases de datos, como los servidores de archivos, los servidores DHCP, los servidores de impresión, etc.

### <a name="what-is-application-consistent-recovery-point"></a>¿Qué es un punto de recuperación coherente con la aplicación? 
Los puntos de recuperación coherentes con la aplicación se crean a partir de instantáneas coherentes con la aplicación que capturan los mismos datos que las instantáneas coherentes con los bloqueos, con la adición de todos los datos en memoria y todas las transacciones en curso. Debido a su contenido adicional, las instantáneas coherentes con la aplicación son las más complejas y las que más tardan en ejecutarse. Se recomiendan puntos de recuperación coherentes con la aplicación para aplicaciones y sistemas operativos de bases de datos como SQL.

### <a name="how-are-recovery-points-generated-and-saved"></a>¿Cómo se generan y guardan los puntos de recuperación?
Para comprender cómo genera Site Recovery los puntos de recuperación, veamos un ejemplo de la directiva de replicación, que tiene un período de retención del punto de recuperación de 24 horas y una instantánea de frecuencia coherente con la aplicación de 1 hora.
Site Recovery crea un punto coherente con los bloqueos cada 5 minutos y el usuario no tiene ningún control para cambiar esta frecuencia. Por lo tanto, durante la última hora, el usuario tendrá 12 puntos coherentes con los bloqueos y 1 punto coherente con la aplicación entre los que elegir. Conforme transcurre el tiempo, Site Recovery elimina todos los puntos de recuperación de hace más de una hora y solo guarda un punto de recuperación por hora.
A modo de ilustración, en la captura de pantalla siguiente:


1. Durante un tiempo inferior a la última hora, hay puntos de recuperación con una frecuencia de 5 minutos.
2. Durante un tiempo superior a la última hora, podemos ver que solo se mantiene un punto de recuperación por hora.

  ![generación de puntos de recuperación](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>¿Hasta cuánto tiempo atrás puedo recuperar?
El punto de recuperación más antiguo que puede usar es de 72 horas.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-there-is-no-recovery-points-generation-due-to-some-issue-for-more-than-24-hours-does-my-previous-recovery-points-will-be-pruned"></a>¿Qué ocurrirá si tengo una directiva de replicación de 24 horas y no hay ninguna generación de puntos de recuperación debido a algún problema durante más de 24 horas? ¿Se eliminarán mis puntos de recuperación anteriores?
No, en este caso, Site Recovery mantendrá todos los puntos de recuperación anteriores. 

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Después de habilitar la replicación en una máquina virtual, ¿cómo puedo cambiar la directiva de replicación? 
Vaya a Almacén de Site Recovery > Site Recovery Infrastructure (Infraestructura de Site Recovery) > Directivas de replicación. Seleccione la directiva que quiera editar y guarde los cambios. Todos los cambios se aplicarán también en todas las replicaciones existentes. 

### <a name="are-all-the-recovery-points-complete-copy-of-the-vm-or-differential"></a>¿Todos los puntos de recuperación son una copia completa de la máquina virtual o diferencial?
En el caso de la replicación inicial, el primer punto de recuperación que se genera tendrá la copia completa y los puntos de recuperación sucesivos tendrán los cambios diferenciales.

### <a name="does-increasing-recovery-points-retention-windows-increases-the-storage-cost"></a>¿Al aumentar los períodos de retención de puntos de recuperación se aumenta el costo de almacenamiento?
Sí, si aumenta el período de retención de 24 a 72 horas, Site Recovery guardará los puntos de recuperación durante 48 horas adicionales, lo que implicará cargos de almacenamiento. Por ejemplo, si un único punto de recuperación tiene cambios diferenciales de 10 GB y un costo de 0,16 $ al mes por GB, esto implicaría unos cargos adicionales de 1,6 $ * 48 al mes.

## <a name="failover"></a>Conmutación por error

### <a name="is-failover-automatic"></a>¿La conmutación por error es automática?

La conmutación por error no es automática. Puede iniciar las conmutaciones por error con solo un clic en el portal, o bien puede usar Site Recovery [PowerShell](azure-to-azure-powershell.md) para desencadenar una conmutación por error. 

### <a name="can-i-retain-public-ip-address-after-failover"></a>¿Se puede conservar la dirección IP pública después de la conmutación por error?

La dirección IP pública de la aplicación de producción **no se pueden retenerse en la conmutación por error**. Las cargas de trabajo que nos llegan como parte del proceso de conmutación por error deben tener asignado un recurso de dirección IP pública de Azure disponible en la región de destino. Este paso puede realizarse manualmente o se automatiza con planes de recuperación. Consulte este [artículo](https://docs.microsoft.com/azure/site-recovery/concepts-public-ip-address-with-site-recovery#public-ip-address-assignment-using-recovery-plan) para asignar la dirección IP pública con el plan de recuperación.  

### <a name="can-i-retain-private-ip-address-during-failover"></a>¿Se puede conservar la dirección IP privada durante la conmutación por error?
Sí, puede conservar la dirección IP privada. De forma predeterminada, cuando se habilita la recuperación ante desastres para máquinas virtuales de Azure, Site Recovery crea recursos de destino en función de la configuración de los recursos de origen. En el caso de las máquinas virtuales de Azure configuradas con direcciones IP estáticas, Site Recovery trata de aprovisionar la misma dirección IP para la máquina virtual de destino, si no está en uso. Consulte este [artículo](site-recovery-retain-ip-azure-vm-failover.md) para conservar la dirección IP privada en diferentes condiciones.

### <a name="after-failover-the-server-does-not-have-the-same-ip-address-as-the-source-vm-why-is-it-assigned-with-a-new-ip-address"></a>Después de la conmutación por error, el servidor no tiene la misma dirección IP que la máquina virtual de origen. ¿Por qué se asigna con una nueva dirección IP?

Site Recovery trata de proporcionar la dirección IP de la mejor manera posible en el momento de la conmutación por error. En caso de que la esté usando otra máquina virtual, la siguiente dirección IP disponible se establece como destino. Para obtener una explicación completa de cómo controla Site Recovery el direccionamiento, [consulte este artículo](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-network-mapping#set-up-ip-addressing-for-target-vms).

### <a name="what-does-latestlowest-rpo-recovery-points-means"></a>¿Qué significan los puntos de recuperación con la opción Más reciente (RPO más bajo)?
esta opción procesa primero todos los datos que se han enviado al servicio Site Recovery para crear un punto de recuperación para cada máquina virtual antes de conmutarla por error a dicho punto de recuperación. Esta opción ofrece el objetivo de punto de recuperación (RPO) mínimo, ya que la máquina virtual de Azure creada después de la conmutación por error tiene todos los datos replicados en Site Recovery al desencadenarse la conmutación por error.

### <a name="does-latest-lowest-rpo-recovery-points-have-impact-on-failover-rto"></a>¿Los puntos de recuperación con la opción Más reciente (RPO más bajo) afectan al RTO de conmutación por error?
Sí, ya que Site Recovery procesará todos los datos pendientes antes de la conmutación por error, esta opción tendrá un RTO más alto en comparación con otras.

### <a name="what-does-latest-processed-option-in-recovery-points-mean"></a>¿Qué significa la opción Procesado más recientemente en los puntos de recuperación?
esta opción realiza una conmutación por error de todas las máquinas virtuales al último punto de recuperación procesado por Site Recovery. Para ver el último punto de recuperación de una máquina virtual específica, seleccione Puntos de recuperación más recientes en la configuración de la máquina virtual. Esta opción proporciona un objetivo de tiempo de recuperación (RTO) bajo, ya que no se invierte tiempo en el procesamiento de datos sin procesar.

### <a name="if-im-replicating-between-two-azure-regions-what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Si se replica entre dos regiones de Azure, ¿qué ocurre si la región primaria experimenta una interrupción inesperada?
Puede desencadenar una conmutación por error después de la interrupción. Site Recovery no necesita conectividad desde la región primaria para realizar la conmutación por error.

## <a name="recovery-plan"></a>Plan de recuperación

### <a name="what-is-a-recovery-plan-"></a>¿Qué es un plan de recuperación?
Los planes de recuperación en Site Recovery coordinan la recuperación de conmutación por error de máquinas virtuales. Ayuda a que la recuperación sea coherente y precisa, repetible y automatizada. Un plan de recuperación aborda las siguientes necesidades del usuario:

- Definición de un grupo de máquinas virtuales que conmutan por error de forma conjunta.
- Definición de las dependencias entre las máquinas virtuales para que la aplicación aparezca con precisión.
- Automatización de la recuperación junto con acciones manuales personalizadas de modo que también se pueden lograr tareas distintas de la conmutación por error de las máquinas virtuales.

[Obtenga más información](site-recovery-create-recovery-plans.md) sobre los planes de recuperación.

### <a name="how-does-sequencing-is-achieved-in-a-recovery-plan"></a>¿Cómo se logra la secuenciación en un plan de recuperación?

En un plan de recuperación, puede crear varios grupos para lograr la secuenciación. Cada grupo conmuta por error de uno en uno, lo que significa que las máquinas virtuales que forman parte del mismo grupo conmutarán por error juntas seguidas de otro grupo. Compruebe cómo [modelar la aplicación mediante el plan de recuperación](https://review.docs.microsoft.com/azure/site-recovery/recovery-plan-overview?branch=pr-en-us-61681#model-apps). 

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>¿Cómo se puede encontrar el RTO de un plan de recuperación?
Para comprobar el RTO de un plan de recuperación, haga una conmutación por error de prueba del plan de recuperación y vaya a Trabajos de Site Recovery.
Por ejemplo, como se muestra a continuación, el plan de recuperación de prueba de SAP tardó 8 minutos y 59 segundos en conmutar por error todas las máquinas virtuales y realizar todas las acciones especificadas.

  ![com-error](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>¿Se pueden agregar automáticamente runbooks al plan de recuperación?
Sí, puede integrar runbooks de automatización de Azure en el plan de recuperación. [Más información](site-recovery-runbook-automation.md)

## <a name="reprotect-and-failback"></a>Reprotección y conmutación por recuperación 

### <a name="after-doing-a-failover-from-primary-region-to-disaster-recovery-region-does-vms-in-a-dr-region-gets-protected-automatically"></a>Después de realizar una conmutación por error de la región primaria en la región de recuperación ante desastres, ¿se protegen de forma automática las máquinas virtuales en una región de recuperación ante desastres?
No, al [conmutar por error](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) las máquinas virtuales de Azure desde una región a otra, las máquinas virtuales se inician en la región de recuperación ante desastres con un estado desprotegido. Para conmutar por recuperación las máquinas virtuales en la región primaria, tiene que [volver a protegerlas](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) en la región secundaria.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-secondary-region-to-primary-region"></a>En el momento de la reprotección, ¿Site Recovery replica los datos completos de la región secundaria en la región primaria?
Depende de la situación, por ejemplo, si la máquina virtual de la región de origen existe, solo se sincronizan los cambios entre el disco de origen y el disco de destino. Las copias de seguridad diferenciales se calculan comparando los discos y, a continuación, se transfieren. Esto suele tardar unas horas en completarse. Consulte este [artículo]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) para obtener información detallada sobre lo que ocurre durante la reprotección.

### <a name="how-much-time-does-it-take-to-failback"></a>¿Cuánto se tarda en conmutar por recuperación?
Una vez que ha finalizado la reprotección, se suele tardar un tiempo similar en conmutar por error de la región primaria a la región secundaria. 

## <a name="security"></a>Seguridad
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>¿Se envían mis datos de replicación al servicio de Site Recovery?
No, Site Recovery no intercepta los datos replicados ni tiene información sobre lo que se ejecuta en las máquinas virtuales. Únicamente se envían los metadatos necesarios para coordinar la replicación y la conmutación por error al servicio Site Recovery.  
Site Recovery está certificado según la norma ISO 27001:2013, 27018, además de HIPAA y DPA, y está completando sus evaluaciones de SOC2 y FedRAMP JAB.

### <a name="does-site-recovery-encrypt-replication"></a>¿Site Recovery cifra la replicación?
Sí, se admite tanto el cifrado en tránsito como el [cifrado en Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption).

## <a name="next-steps"></a>Pasos siguientes
* [Revise](azure-to-azure-support-matrix.md) los requisitos de compatibilidad.
* [Configure](azure-to-azure-tutorial-enable-replication.md) la replicación de Azure en Azure.
