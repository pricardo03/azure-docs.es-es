---
title: 'Azure Migrate: preguntas más frecuentes (P+F) | Microsoft Docs'
description: Aborda las preguntas más frecuentes sobre Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: snehaa
ms.openlocfilehash: bf591306220b2c8c1e6c8166686836d96432fc7d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68856276"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate: preguntas más frecuentes (P+F)

En este artículo se incluyen las preguntas más frecuentes sobre Azure Migrate. Si tiene más preguntas después de leer este artículo, publíquelas en el [Foro de Azure Migrate](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>General

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>¿Qué geografías de Azure con compatibles con Azure Migrate?

Puede encontrar la lista de [VMware aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) y la de [Hyper-V aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>¿En qué se diferencia Azure Migrate de Azure Site Recovery?

Azure Migrate proporciona un centro principal para iniciar, ejecutar y realizar el seguimiento de la detección, valoración y migración de las máquinas y cargas de trabajo a Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) es una herramienta de recuperación ante desastres. Azure Migrate Server Migration aprovecha Azure Site Recovery en el back-end para habilitar escenarios de migración para la migración mediante lift-and-shift de máquinas locales.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Dispositivo de Azure Migrate (VMware/servidores físicos)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>¿Cómo se conecta el dispositivo de Azure Migrate a Azure?

La conexión puede ser a través de Internet o puede usar ExpressRoute con emparejamiento público o de Microsoft.

### <a name="what-network-connectivity-requirements-are-needed-for-azure-migrate-server-assessment-and-migration"></a>¿Qué requisitos de conectividad de red son necesarios para Azure Migrate Server Assessment y Server Migration?

Para que las direcciones URL y los puertos necesarios para Azure Migrate se comuniquen con Azure, revise las matrices de compatibilidad [VMWare](migrate-support-matrix-vmware.md) y [Hyper-V](migrate-support-matrix-hyper-v.md).

### <a name="can-i-harden-the-appliance-vm-i-set-up-with-the-template"></a>¿Se puede proteger la máquina virtual del dispositivo que se configuró con la plantilla?

Se pueden agregar componentes adicionales (por ejemplo, un antivirus) en la plantilla, siempre que las reglas de firewall y de comunicación necesarias para el dispositivo de Azure Migrate no se modifiquen.   

### <a name="what-data-is-collected-by-azure-migrate-appliance"></a>¿Qué datos recopila el dispositivo de Azure Migrate?

Puede obtener detalles sobre los datos recopilados por el dispositivo de Azure Migrate [aquí](https://docs.microsoft.com/azure/migrate/migrate-appliance#collected-performance-data-vmware).

### <a name="is-there-any-performance-impact-on-the-analyzed-vmware-or-hyper-v-environment"></a>¿Hay algún impacto en el rendimiento en el entorno de VMware o Hyper-V analizado?

Con la generación de perfiles continua de datos de rendimiento, el dispositivo de Azure Migrate genera perfiles de máquinas locales para medir los datos de rendimiento de la máquina virtual. Esto tiene un impacto prácticamente nulo en el rendimiento en los hosts de Hyper-V/ESXi, así como en vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>¿Dónde se almacenan los datos recopilados y durante cuánto tiempo?

Los datos que recopila el dispositivo de Azure Migrate se almacenan en la ubicación de Azure especificada al crear el proyecto de migración. Los datos se almacenan de forma segura en una suscripción de Microsoft y se eliminan al eliminar el proyecto de Azure Migrate.

Para la visualización de dependencias, si instala agentes en las máquinas virtuales, los datos que recopilan los agentes de Dependency Agent se almacenan en los Estados Unidos en un área de trabajo de Log Analytics creada en la suscripción de Azure. Estos datos se eliminan al eliminar el área de trabajo de Log Analytics en su suscripción. [Más información](concepts-dependency-visualization.md).

### <a name="what-is-the-volume-of-data-uploaded-by-azure-migrate-appliance-during-continuous-profiling"></a>¿Qué es el volumen de datos que carga el dispositivo de Azure Migrate durante la generación de perfiles continua?

El volumen de datos que se envían a Azure Migrate varía en función de varios parámetros. Para dar un número indicativo, un proyecto de Azure Migrate con 10 máquinas (cada una con un disco y una NIC), envía aproximadamente 50 MB al día. Se trata de un valor aproximado, que cambia en función del número de puntos de datos para las NIC y los discos (los datos enviados no son lineales si aumenta el número de máquinas, NIC o discos).

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>¿Los datos están cifrados en reposo y en tránsito?

Sí, en ambos casos. Los metadatos se envían de manera segura al servicio Azure Migrate por Internet a través de HTTPS. Los metadatos se almacenan en [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) y en [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) en una suscripción de Microsoft y se cifran en reposo.

Los datos que recopilan los agentes de dependencias también se cifran en tránsito (HTTPS seguro) y se almacenan en un área de trabajo de Log Analytics en la suscripción del usuario. También se cifran en reposo.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>¿Cómo se comunica el dispositivo de Azure Migrate con vCenter Server y con el servicio Azure Migrate?

El dispositivo se conecta vCenter Server (puerto 443) mediante las credenciales proporcionadas al configurar el dispositivo. Realiza consultas a vCenter Server con VMware PowerCLI para recopilar metadatos sobre las máquinas virtuales que administra vCenter Server. Recopila los datos de configuración sobre máquinas virtuales (núcleos, memoria, discos, NIC, etc.), así como el historial de rendimiento de cada máquina virtual del último mes. Los metadatos recopilados se envían a continuación a Azure Migrate Server Assessment (por Internet a través de HTTPS) para su valoración.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-servers"></a>¿Puedo conectar el mismo dispositivo en varios servidores de vCenter?

No. Hay una asignación unívoca entre un dispositivo y vCenter Server. Si tiene que detectar máquinas virtuales en varias instancias de vCenter Server, debe implementar varios dispositivos. 


### <a name="i-changed-my-machine-size-can-i-rerun-an-assessment"></a>He cambiado el tamaño de mi máquina. ¿Puedo volver a ejecutar una valoración?

El dispositivo de Azure Migrate recopila continuamente información sobre el entorno local. Pero una evaluación es una instantánea de un momento dado de las máquinas virtuales locales. Si cambia la configuración de una máquina virtual que quiere evaluar, use la opción "Recalcular" para actualizar la valoración con los últimos cambios.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate-server-assessment"></a>¿Cómo se puede detectar un entorno de varios inquilinos en Azure Migrate Server Assessment?

En el caso de VMware, si tiene un entorno compartido entre los inquilinos y no quiere detectar las máquinas virtuales de un inquilino en la suscripción de otro inquilino, cree credenciales de vCenter Server con acceso solo a las máquinas virtuales que quiere detectar. Después, use las credenciales al iniciar la detección en el dispositivo de Azure Migrate.

En el caso de Hyper-V, la detección usa las credenciales de host de Hyper-V; si las máquinas virtuales comparten el mismo host de Hyper-V, actualmente no hay ninguna manera de separar la detección.  

### <a name="how-many-vms-can-be-discovered-using-a-single-migration-appliance"></a>¿Cuántas máquinas virtuales se pueden detectar con un único dispositivo de migración?

Puede detectar hasta 10 000 máquinas virtuales de VMware y hasta 5000 máquinas virtuales de Hyper-V con un único dispositivo de migración.  Si tiene más máquinas en su entorno local, aprenda a escalar la valoración de [Hyper-V](scale-hyper-v-assessment.md) y [VMware](scale-vmware-assessment.md).

## <a name="azure-migrate-server-assessment"></a>Azure Migrate: Server Assessment

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>¿Azure Migrate: Server Assessment es compatible con la valoración de servidores físicos?

No, Azure Migrate actualmente no admite la valoración de servidores físicos.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>¿Azure Migrate necesita vCenter Server para detectar un entorno de VMware?

Sí, Azure Migrate necesita vCenter Server para detectar un entorno de VMware. No admite la detección de hosts de ESXi que no administra vCenter Server.

### <a name="whats-the-difference-between-using-azure-migrate-server-assessment-and-the-map-toolkit"></a>¿Cuál es la diferencia entre el uso de Azure Migrate: Server Assessment y MAP Toolkit?

Azure Migrate: Server Assessment proporciona una valoración de la migración para ayudar con la preparación de la migración y la valoración de las cargas de trabajo para la migración a Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) tiene otras funcionalidades como planeamiento de migración para versiones más recientes de los sistemas operativos cliente y servidor de Windows y el seguimiento del uso de software. Para esos escenarios, siga usando MAP Toolkit.

### <a name="how-is-azure-migrate-server-assessment-different-from-azure-site-recovery-deployment-planner"></a>¿En qué se diferencia Azure Migrate: Server Assessment de Azure Site Recovery Deployment Planner?

Azure Migrate: Server Assessment es una herramienta de planeamiento de la migración. Azure Site Recovery Deployment Planner es una herramienta de planeamiento de recuperación ante desastres.

- **Migración de VMware/Hyper-V a Azure**: Si piensa migrar los servidores locales a Azure, use la herramienta Azure Migrate: Server Assessment para el planeamiento de la migración. La herramienta evalúa las cargas de trabajo locales y proporciona orientación, información y mecanismos que le ayudarán a realizar la migración a Azure. Una vez que haya terminado de planear la migración, puede usar herramientas como Azure Migrate: Server Migration, para migrar las máquinas a Azure.
- **Recuperación ante desastres de VMware o Hyper-V en Azure**: Para la recuperación ante desastres en Azure con Site Recovery, use Site Recovery Deployment Planner para el plan de la recuperación. Site Recovery Deployment Planner realiza una evaluación profunda específica de Site Recovery de su entorno local. Proporciona recomendaciones que Site Recovery necesita para realizar correctamente las operaciones de recuperación ante desastres, tales como la replicación y la conmutación por error de las máquinas virtuales.

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>¿Azure Migrate es compatible con la estimación de costes basada en Contrato Enterprise (EA)?

Actualmente, Azure Migrate no es compatible con la estimación de costes de la [oferta para Contrato Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support/). La solución alternativa consiste en especificar pago por uso como la oferta y especificar manualmente el porcentaje de descuento (se aplica a la suscripción) en el campo "Descuento" de las propiedades de valoración.

  ![Descuento](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>¿Cuál es la diferencia entre el ajuste de tamaño como local y el ajuste de tamaño basado en el rendimiento?

- En el ajuste como tamaño local, Azure Migrate no tiene en cuenta los datos de rendimiento de la máquina virtual. Ajusta el tamaño de las máquinas virtuales en función de la configuración local. - En el ajuste de tamaño basado en el rendimiento, el ajuste de tamaño se basa en los datos de uso.
- Por ejemplo, si una máquina virtual local tiene 4 núcleos y 8 GB de memoria con un uso de CPU del 50 % y un uso de memoria del 50 %, como ajuste de tamaño local recomienda una SKU de máquina virtual de Azure con 4 núcleos y 8 GB de memoria. Pero el ajuste de tamaño basado en el rendimiento recomienda una SKU de máquina virtual de 2 núcleos y 4 GB, ya que se tiene en cuenta el porcentaje de uso.
- De forma similar, el ajuste de tamaño de disco depende de dos propiedades de evaluación: criterio de ajuste de tamaño y tipo de almacenamiento.
= Si el criterio de ajuste de tamaño se basa en el rendimiento y el tipo de almacenamiento es automático, los valores de capacidad de proceso y de IOPS del disco se tendrán en cuenta al identificar el tipo de disco de destino (estándar o premium).
- Si el criterio de ajuste de tamaño está basado en el rendimiento y el tipo de almacenamiento es premium, se recomienda un disco premium. La SKU de disco premium se selecciona en función del tamaño del disco local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es como local y el tipo de almacenamiento es estándar o premium.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>¿Qué impacto tiene el historial de rendimiento y uso de percentil en las recomendaciones de tamaño?

Estas propiedades solo son aplicables para el ajuste de tamaño basado en el rendimiento.

- Azure Migrate recopila el historial de rendimiento de las máquinas locales y lo usa para recomendar el tipo de disco y el tamaño de la máquina virtual en Azure.
- El dispositivo realiza un perfil del entorno local continuamente para recopilar datos de uso en tiempo real cada 20 segundos. El dispositivo acumula ejemplos de 20 segundos y crea un único punto de datos cada 15 minutos. Para crear el punto de datos único, el dispositivo selecciona el valor máximo de todos los ejemplos de 20 segundos y lo envía a Azure.
- Cuando se crea una evaluación en Azure, en función de la duración del rendimiento y el valor del percentil del historial de rendimiento, Azure Migrate calcula el valor del uso efectivo y lo usa para ajustar el tamaño.
- Por ejemplo, si establece la duración del rendimiento en 1 día y el valor del percentil en el percentil 95, Azure Migrate usa puntos de ejemplo de 15 minutos que envía el recopilador para el último día, los ordena de manera ascendente y elige el valor del percentil 95 como el uso efectivo.
- El valor del percentil 95 garantiza que se van a omitir los valores atípicos que pueden aparecer si elige el percentil 99. Si quiere elegir el uso máximo para el período y no quiere perderse ningún valor atípico, debe seleccionar el percentil 99.

### <a name="what-is-dependency-visualization"></a>¿Qué es la visualización de dependencias?

La visualización de dependencias le permite evaluar grupos de máquinas virtuales para la migración con mayor confianza. Comprueba las dependencias de las máquinas antes de llevar a cabo una valoración. La visualización de dependencias ayuda a asegurar que nada se queda atrás y evita interrupciones inesperadas al migrar a Azure. Azure Migrate aprovecha la solución Service Map de los registros de Azure Monitor para habilitar la visualización de dependencias.

> [!NOTE]
> La funcionalidad de visualización de dependencias no está disponible en Azure Government.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>¿Tengo que pagar por usar la característica de visualización de dependencias?

No. [Más información](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre los precios de Azure Migrate.

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>¿Es necesario instalar algo para la visualización de dependencias?

Para usar la visualización de dependencias, debe descargar e instalar agentes en cada máquina local que vaya a evaluar.

- Es necesario tener instalado [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) en cada máquina.
- Es necesario tener instalado [Dependency Agent](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) en cada máquina.
- Además, si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.

No necesita estos agentes a menos que esté usando la visualización de dependencias.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>¿Puedo usar un área de trabajo existente para la visualización de dependencias?

Sí, puede asociar un área de trabajo existente para el proyecto de migración y aprovecharla para la visualización de dependencias. [Más información](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>¿Puedo exportar el informe de visualización de dependencias?

No, el informe de visualización de dependencias no se puede exportar. Sin embargo, como Azure Migrate usa Service Map para la visualización de dependencias, puede usar las [API REST de Service Map](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) para obtener las dependencias en formato json.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>¿Cómo puedo automatizar la instalación de Microsoft Monitoring Agent (MMA) y el agente de dependencia?

[Use este script](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) para la instalación de los agentes. [Siga estas instrucciones](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) para instalar MMA mediante la línea de comandos o la automatización. En el caso de MMA, aproveche [este script](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Además de los scripts, también puede usar las herramientas de implementación como System Center Configuration Manager o [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), etc. para implementar los agentes.

### <a name="what-operating-systems-are-supported-by-mma"></a>¿Qué sistemas operativos admite MMA?

- [Revise](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) la lista de los sistemas operativos Windows compatibles con MMA.
- [Revise](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) la lista de los sistemas operativos Linux compatibles con MMA.

### <a name="what-are-the-operating-systems-supported-by-the-dependency-agent"></a>¿Cuáles son los sistemas operativos compatibles con Dependency Agent?

[Revise](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) los sistemas operativos Windows compatibles con Dependency Agent.
[Revise](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems) los sistemas operativos Linux compatibles con Dependency Agent.

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>¿Puedo visualizar las dependencias de Azure Migrate durante más de una hora?
No, puede visualizar las dependencias durante un máximo de una hora. Puede volver a una fecha determinada del historial, hasta el último mes, pero la duración máxima de visualización es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Pero puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>¿Se admite la visualización de dependencias para grupos con más de diez máquinas virtuales?
Puede [visualizar las dependencias para grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que tengan un máximo de diez máquinas virtuales. Si tiene un grupo con más de diez máquinas virtuales, recomendamos dividirlo en grupos más pequeños y luego visualizar las dependencias.

## <a name="azure-migrate-server-migration"></a>Azure Migrate: Server Migration

### <a name="how-is-azure-migrate-server-migration-different-from-azure-site-recovery"></a>¿En qué se diferencia Azure Migrate: Server Migration de Azure Site Recovery?

Azure Migrate: Server Migration aprovecha el motor de replicación de Site Recovery para la migración basada en agentes de máquinas virtuales de VMware, la migración de máquinas virtuales de Hyper-V y la migración de servidores físicos a Azure. La opción sin agente para migrar máquinas virtuales de VMware está integrada de forma nativa en Server Migration.

## <a name="next-steps"></a>Pasos siguientes
Consulte la sección [Información general de Azure Migrate](migrate-services-overview.md).
