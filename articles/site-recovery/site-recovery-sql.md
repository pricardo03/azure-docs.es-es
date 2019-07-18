---
title: Configuración de la recuperación ante desastres para SQL Server con SQL Server y Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar la recuperación ante desastres para SQL Server mediante SQL Server y Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/30/2019
ms.author: sutalasi
ms.openlocfilehash: 1c44b10b54a5f58dff1aecf36c3633cc8ffbd8f0
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491773"
---
# <a name="set-up-disaster-recovery-for-sql-server"></a>Configuración de la recuperación ante desastres para SQL Server

En este artículo se describe cómo proteger el back-end de SQL Server de una aplicación con una combinación de tecnologías de continuidad empresarial y recuperación ante desastres (BCDR) de SQL Server y [Azure Site Recovery](site-recovery-overview.md).

Antes de empezar, asegúrese de que comprende las funciones de recuperación ante desastres de SQL Server, incluidos los clústeres de conmutación por error, los Grupos de disponibilidad AlwaysOn, la creación de reflejo de la base de datos, el trasvase de registros, la replicación geográfica activa y los grupos de conmutación por error automática.

## <a name="dr-recommendation-for-integration-of-sql-server-bcdr-technologies-with-site-recovery"></a>Recomendaciones de recuperación ante desastres para la integración de las tecnologías de BCDR de SQL Server con Site Recovery.

La elección de una tecnología de BCDR para servidores SQL Server debe basarse en las necesidades RTO y RPO, según la tabla siguiente. Una vez realizada esa opción, Site Recovery se puede integrar con la operación de conmutación por error de esa tecnología con el fin de coordinar la recuperación de toda la aplicación.

**Tipo de implementación** | **Tecnología de BCDR** | **RTO esperado para SQL** | **RPO esperado para SQL** |
--- | --- | --- | ---
SQL Server en máquinas virtuales IaaS de Azure o en un entorno local| **[Grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017)** | Equivalente al tiempo necesario para convertir la réplica secundaria en principal. | La replicación es asincrónica a la réplica secundaria, por lo tanto, se produce pérdida de datos.
SQL Server en máquinas virtuales IaaS de Azure o en un entorno local| **[Clústeres de conmutación por error (FCI AlwaysOn)](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server?view=sql-server-2017)** | Equivalente al tiempo necesario para realizar la conmutación por error entre los nodos. | Usa almacenamiento compartido, por lo tanto, está disponible la misma vista de instancia de almacenamiento en la conmutación por error.
SQL Server en máquinas virtuales IaaS de Azure o en un entorno local| **[Creación de reflejo (modo de alto rendimiento)](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server?view=sql-server-2017)** | Equivalente al tiempo necesario para forzar el servicio, que utiliza el servidor reflejado como servidor en espera semiactiva. | La replicación es asincrónica. La base de datos reflejada puede retrasarse un poco respecto a la base de datos principal. La diferencia normalmente es pequeña, sin embargo, puede ser considerable si el sistema del servidor principal o reflejado está bajo un período de mucha carga.<br></br>El trasvase de registros puede complementar la creación de reflejo de la base de datos y es una alternativa conveniente para la creación de reflejo de la base de datos asincrónica.
SQL como PaaS en Azure<br></br>(Grupos elásticos, servidores de bases de datos SQL) | **Replicación geográfica activa** | 30 segundos una vez que se desencadena<br></br>Cuando se activa la conmutación por error a una de las bases de datos secundarias, las demás bases de datos secundarias se vinculan automáticamente a la nueva base de datos principal. | RPO de 5 segundos<br></br>La replicación geográfica activa aprovecha la tecnología AlwaysOn de SQL Server para replicar de forma asincrónica las transacciones confirmadas en la base de datos principal a una base de datos secundaria mediante aislamiento de instantáneas. <br></br>Se garantiza que los datos secundarios nunca tengan transacciones parciales.
SQL como PaaS configurado con replicación geográfica activa en Azure<br></br>(Instancia administrada de SQL Database, grupos elásticos, servidores de base de datos SQL) | **Grupos de conmutación por error automática** | RTO de 1 hora | RPO de 5 segundos<br></br>Los grupos de conmutación por error automática proporcionan la semántica de grupo sobre la replicación geográfica activa, pero se usa el mismo mecanismo de replicación asincrónico.
SQL Server en máquinas virtuales IaaS de Azure o en un entorno local| **Replicación con Azure Site Recovery** | Normalmente, menos de 15 minutos. [Obtenga más información](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) sobre el Acuerdo de Nivel de Servicio de RTO que proporciona Azure Site Recovery. | 1 hora para la coherencia de la aplicación y 5 minutos para la coherencia de bloqueo. 

> [!NOTE]
> Estas son algunas consideraciones importantes al proteger las cargas de trabajo SQL con Azure Site Recovery:
> * Azure Site Recovery es independiente de la aplicación y, por lo tanto, puede proteger cualquier versión de SQL Server que se implemente en un sistema operativo compatible. [Más información](vmware-physical-azure-support-matrix.md#replicated-machines).
> * Puede usar Site Recovery para cualquier implementación en Azure, Hyper-V, VMware o infraestructura física. Siga la [guía](site-recovery-sql.md#how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2) al final del documento sobre cómo proteger el clúster de SQL Server con Azure Site Recovery.
> * Asegúrese de que la frecuencia de cambio de datos (bytes de escritura por segundo) observada en la máquina está dentro de los [límites de Site Recovery](vmware-physical-azure-support-matrix.md#churn-limits). Para máquinas con Windows, puede ver esto en la pestaña Rendimiento en el Administrador de tareas. Observe la velocidad de escritura de cada disco.
> * Azure Site Recovery admite la replicación de las Instancias de clúster de conmutación por error en Espacios de almacenamiento directo. [Más información](azure-to-azure-how-to-enable-replication-s2d-vms.md).
 

## <a name="disaster-recovery-of-application"></a>Recuperación ante desastres de la aplicación

**Azure Site Recovery organiza la prueba de conmutación por error y la conmutación por error de toda la aplicación con la ayuda de los Planes de recuperación.** 

Hay algunos requisitos previos para garantizar que Plan de recuperación está completamente personalizado en función de sus necesidades. Normalmente, las implementaciones de SQL Server necesitan un directorio de Active Directory. También es necesaria la conectividad de la capa de aplicación.

### <a name="step-1-set-up-active-directory"></a>Paso 1: Configuración de Active Directory

Configure Active Directory en el sitio de recuperación secundario para que SQL Server se ejecute correctamente.

* **Empresas pequeñas**: con un pequeño número de aplicaciones y un solo controlador de dominio para el sitio local, si desea conmutar por error todo el sitio, se recomienda utilizar la replicación de Site Recovery para replicar el controlador de dominio en el centro de datos secundario o en Azure.
* **Empresas medianas y grandes**: si tiene un gran número de aplicaciones, un bosque de Active Directory y desea que la conmutación por error se realice por aplicaciones o cargas de trabajo, se recomienda configurar un controlador de dominio adicional en el centro de datos secundario o en Azure. Si usa grupos de disponibilidad AlwaysOn para recuperar en un sitio remoto, se recomienda configurar otro controlador de dominio adicional en el sitio secundario o en Azure, que se utilizará para la instancia de SQL Server recuperada.

Las instrucciones de este artículo suponen que un controlador de dominio está disponible en la ubicación secundaria. [Más información](site-recovery-active-directory.md) sobre la protección de Active Directory con Site Recovery.

### <a name="step-2-ensure-connectivity-with-other-application-tiers-and-web-tier"></a>Paso 2: garantía de la conectividad con otras capas de aplicación y el nivel web

Asegúrese de que, una vez que el nivel de base de datos está en marcha en la región de Azure de destino, se tiene conectividad con la aplicación y el nivel web. Se han de seguir unos pasos necesarios de antemano con el fin de validar la conectividad con la prueba de conmutación por error.

Comprenda cómo puede diseñar aplicaciones para las consideraciones de conectividad con un par de ejemplos:
* [Diseño de una aplicación para la recuperación ante desastres en la nube](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* [Estrategias de recuperación ante desastres de grupos elásticos](../sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)

### <a name="step-3-integrate-with-always-on-active-geo-replication-or-auto-failover-groups-for-application-failover"></a>Paso 3: integración con AlwaysOn, replicación geográfica activa o grupos de conmutación por error automática para la conmutación por error de la aplicación

Las tecnologías de BCDR Always On, replicación geográfica activa y grupos de conmutación por error automática tienen réplicas secundarias de SQL Server que se ejecutan en la región de Azure de destino. Por lo tanto, el primer paso para la conmutación por error de la aplicación es convertir esta réplica en principal (suponiendo que ya tiene un controlador de dominio en la base de datos secundaria). Puede que este paso no sea necesario si elige realizar una conmutación por error automática. Solo una vez completada la conmutación por error de la base de datos, debe realizar la conmutación por error de las capas de aplicación o los niveles web.

> [!NOTE] 
> Si ha protegido las máquinas de SQL con Azure Site Recovery, basta crear un grupo de recuperación de estas máquinas y agregar la conmutación por error en el plan de recuperación.

[Cree un plan de recuperación](site-recovery-create-recovery-plans.md) con máquinas virtuales del nivel web y de aplicación. Siga los pasos siguientes para agregar la conmutación por error del nivel de base de datos:

1. Importe scripts en su cuenta de Azure Automation. Esta cuenta contiene los scripts para conmutar por error grupos de disponibilidad de SQL en una [máquina virtual de Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) y una [máquina virtual clásica](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Implementación en Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. Agregue ASR-SQL-FailoverAG como acción previa del primer grupo del plan de recuperación.

1. Siga las instrucciones disponibles en el script para crear una variable de automatización para proporcionar el nombre de los grupos de disponibilidad.

### <a name="step-4-conduct-a-test-failover"></a>Paso 4: realización de una prueba de conmutación por error.

Algunas tecnologías de BCDR, como SQL Always On, no admiten de forma nativa la conmutación por error de prueba. Por lo tanto, se recomienda el siguiente enfoque **solo cuando se integre con estas tecnologías**:

1. Configure [Azure Backup](../backup/backup-azure-arm-vms.md) en la máquina virtual que hospeda la réplica del grupo de disponibilidad en Azure.

1. Antes de desencadenar la conmutación por error del plan de recuperación, recupere la máquina virtual a partir de la copia de seguridad realizada en el paso anterior.

    ![Restauración de datos de Azure Backup](./media/site-recovery-sql/restore-from-backup.png)

1. [Fuerce un quórum](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) en la máquina virtual restaurada de la copia de seguridad.

1. Actualice la IP del agente de escucha a una dirección IP disponible en la red de conmutación por error de prueba.

    ![Actualización de la dirección IP del agente de escucha](./media/site-recovery-sql/update-listener-ip.png)

1. Conecte el agente de escucha.

    ![Conexión del agente de escucha](./media/site-recovery-sql/bring-listener-online.png)

1. Cree un equilibrador de carga con una dirección IP creada en el grupo de direcciones IP de front-end correspondiente a cada agente de escucha de grupo de disponibilidad y con la máquina virtual SQL agregada en el grupo de back-end.

     ![Creación del equilibrador de carga: grupo de direcciones IP de front-end](./media/site-recovery-sql/create-load-balancer1.png)

    ![Creación del equilibrador de carga: grupo de direcciones IP de back-end](./media/site-recovery-sql/create-load-balancer2.png)

1. Agregue conmutación por error de la capa de aplicación, seguida del nivel web, en este plan de recuperación en grupos de recuperación posteriores. 
1. Realice una prueba de conmutación por error del plan de recuperación para probar la conmutación por error de un extremo a otro de la aplicación.

## <a name="steps-to-do-a-failover"></a>Pasos para realizar una conmutación por error

Una vez que ha agregado el script en el plan de recuperación en el paso 3 y lo ha validado mediante una conmutación por error de prueba con un enfoque especializado en el paso 4, puede realizar una conmutación por error del plan de recuperación creado en el paso 3.

Tenga en cuenta que los pasos de la conmutación por error para las capas de aplicación y los niveles de web deben ser los mismos en la conmutación por error de prueba y los planes de recuperación de conmutación por error.

## <a name="how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2"></a>Cómo proteger un clúster de SQL Server (Edición estándar o SQL Server 2008 R2)

Para un clúster que ejecuta SQL Server Standard Edition o SQL Server 2008 R2, se recomienda utilizar la replicación de Site Recovery para proteger SQL Server.

### <a name="azure-to-azure-and-on-premises-to-azure"></a>Azure en Azure y local en Azure

Site Recovery no proporciona la compatibilidad con clústeres invitados al replicar en una región de Azure. SQL Server tampoco proporciona una solución de recuperación ante desastres de bajo costo para la edición Standard. En este escenario, se recomienda proteger el clúster de SQL Server en un servidor SQL Server independiente en una ubicación principal y recuperarlo en la ubicación secundaria.

1. Configure una instancia de SQL Server independiente adicional en la región de Azure principal o en el sitio local.
1. Configure esta instancia para actuar como un reflejo para las bases de datos que desea proteger. Configure el reflejo en modo de alta seguridad.
1. Configure Site Recovery en el sitio principal ([Azure](azure-to-azure-tutorial-enable-replication.md), [Hyper-V](site-recovery-hyper-v-site-to-azure.md) o [máquinas virtuales de VMware o servidores físicos)](site-recovery-vmware-to-azure-classic.md).
1. Utilice la replicación de Site Recovery para replicar la nueva instancia de SQL Server en el sitio secundario. Como es una copia de alta seguridad de reflejo, se sincronizará con el clúster principal, pero se replicará con la replicación de Site Recovery.


![Clúster estándar](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Consideraciones de la conmutación por recuperación

Para los clústeres de SQL Server Standard, la conmutación por recuperación después de una conmutación por error no planeada requiere realizar una copia de seguridad de SQL Server y una restauración de la instancia reflejada en el clúster original y, a continuación, restablecer el reflejo.

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

### <a name="how-does-sql-get-licensed-when-protected-with-azure-site-recovery"></a>¿Cómo obtiene la licencia SQL cuando está protegido con Azure Site Recovery?
La replicación de Azure Site Recovery para SQL Server está cubierta en la ventaja de Software Assurance para la recuperación ante desastres en todos los escenarios de Azure Site Recovery (desde entornos locales hasta la recuperación ante desastres de Azure o la recuperación ante desastres de IaaS de Azure entre regiones). [Más información](https://azure.microsoft.com/pricing/details/site-recovery/)

### <a name="will-azure-site-recovery-support-my-sql-version"></a>¿Azure Site Recovery es compatible con mi versión de SQL?
Azure Site Recovery es independiente de la aplicación. Por lo tanto, puede proteger cualquier versión de SQL Server que se implemente en un sistema operativo compatible. [Más información](vmware-physical-azure-support-matrix.md#replicated-machines)

## <a name="next-steps"></a>Pasos siguientes
* [Obtenga información](site-recovery-components.md) acerca de la arquitectura de Site Recovery.
* Para servidores SQL Server en Azure, obtenga más información sobre las [soluciones de alta disponibilidad](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions) para la recuperación en la región secundaria de Azure.
* Para SQL Database en Azure, obtenga más información sobre las opciones de [continuidad del negocio](../sql-database/sql-database-business-continuity.md) y [alta disponibilidad](../sql-database/sql-database-high-availability.md) para la recuperación en la región secundaria de Azure.
* Para máquinas SQL Server en el entorno local, [obtenga más información](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#hybrid-it-disaster-recovery-solutions) acerca de las opciones de alta disponibilidad para la recuperación en Azure Virtual Machines.
