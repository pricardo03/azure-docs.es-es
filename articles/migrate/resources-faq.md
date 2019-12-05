---
title: Preguntas habituales sobre Azure Migrate
description: Obtenga respuestas a preguntas comunes sobre el servicio Azure Migrate.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: snehaa
ms.openlocfilehash: dc7dff0119ec849b447754ae54a45911038f6c48
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74284472"
---
# <a name="azure-migrate-common-questions"></a>Azure Migrate: Preguntas frecuentes

En este artículo se responden a preguntas habituales sobre Azure Migrate. Si tiene más preguntas después de leer este artículo, publíquelas en el [Foro de Azure Migrate](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>General

### <a name="which-azure-geographies-are-supported"></a>¿Qué geografías de Azure son compatibles?

Revise las geografías compatibles de Azure Migrate para [VM de VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) y para [VM de Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="whats-the-difference-between-azure-migrate-and-site-recovery"></a>¿Cuál es la diferencia entre Azure Migrate y Site Recovery?

Azure Migrate proporciona un centro principal para administrar y realizar el seguimiento de la detección, valoración y migración de las máquinas y cargas de trabajo locales a Azure. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) es una herramienta de recuperación ante desastres. Azure Migrate: La herramienta Server Migration usa cierta funcionalidad de back-end de Site Recovery para la migración mediante lift-and-shift de algunas máquinas locales.

### <a name="how-do-i-delete-an-azure-migrate-project"></a>¿Cómo puedo eliminar un proyecto de Azure Migrate?

Siga [estas instrucciones](how-to-delete-project.md) para eliminar un proyecto. [Revise los recursos](how-to-delete-project.md#created-resources) creados a medida que detecta, evalúa y migra máquinas y cargas de trabajo en un proyecto de Azure Migrate.


## <a name="azure-migrate-appliance"></a>Dispositivo con Azure Migrate

### <a name="how-does-the-appliance-connect-to-azure"></a>¿Cómo se conecta el dispositivo a Azure?

La conexión puede ser a través de Internet o puede usar Azure ExpressRoute con emparejamiento público o de Microsoft.

### <a name="what-network-connectivity-is-needed-for-azure-migrate-server-assessment-and-migration"></a>¿Qué conectividad de red se necesita para la migración y la valoración de servidores de Azure Migrate?

Revise las matrices de compatibilidad de [VMware](migrate-support-matrix-vmware.md) y [Hyper-V](migrate-support-matrix-hyper-v.md) para más información acerca de las direcciones URL y los puertos necesarios para permitir que Azure Migrate se comunique con Azure.

### <a name="can-i-harden-the-appliance-vm-created-with-the-template"></a>¿Se puede proteger la máquina virtual del dispositivo que se creó con la plantilla?

Puede agregar componentes (por ejemplo, un antivirus) a la plantilla, siempre que las reglas de firewall y de comunicación necesarias para el dispositivo de Azure Migrate no se modifiquen.

### <a name="what-data-is-collected-by-the-azure-migrate-appliance"></a>¿Qué datos recopila el dispositivo de Azure Migrate?

Revise los datos recopilados por el dispositivo de la manera siguiente:
- [Datos de rendimiento](migrate-appliance.md#collected-performance-data-vmware) y [metadatos](migrate-appliance.md#collected-metadata-vmware) para VM de VMware.
- [Datos de rendimiento](migrate-appliance.md#collected-performance-data-hyper-v) y [metadatos](migrate-appliance.md#collected-metadata-hyper-v) para VM de Hyper-V.


### <a name="does-appliance-analysis-impact-performance"></a>¿Afecta el análisis del dispositivo al rendimiento?

El dispositivo de Azure Migrate genera perfiles de máquinas locales continuamente para medir los datos de rendimiento de la máquina virtual. Esta generación de perfiles no tiene prácticamente ningún impacto en los hosts de Hyper-V/ESXi ni en vCenter Server.

### <a name="where-and-how-long-is-collected-data-stored"></a>¿Dónde y durante cuánto tiempo se almacenan los datos recopilados?

Los datos que recopila el dispositivo de Azure Migrate se almacenarán en la ubicación de Azure que elija al crear el proyecto de migración. Los datos se almacenan de forma segura en una suscripción de Microsoft y se eliminan al eliminar el proyecto de Azure Migrate.

Para la visualización de dependencias, los datos recopilados se almacenan en los Estados Unidos, en un área de trabajo de Log Analytics creada en la suscripción de Azure. Estos datos se eliminan al eliminar el área de trabajo de Log Analytics en su suscripción. [Obtenga más información](concepts-dependency-visualization.md) acerca de la visualización de dependencia.

### <a name="what-volume-of-data-is-uploaded-during-continuous-profiling"></a>¿Cuál es el volumen de datos que se carga durante la generación de perfiles continua?

El volumen de datos que se envían a Azure Migrate varía en función de varios parámetros. Para darle una idea del volumen, un proyecto de Azure Migrate con 10 máquinas (cada una con un disco y una NIC) envía aproximadamente 50 MB al día. El valor es aproximado, y cambia en función del número de puntos de datos de las NIC y los discos. El aumento de los datos enviados no es lineal si aumenta el número de máquinas, NIC o discos.

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>¿Se cifran los datos en reposo y mientras están en tránsito?

Sí, en los dos casos.
- Los metadatos se envían de manera segura al servicio Azure Migrate por Internet a través de HTTPS.
- Los metadatos se almacenan en una base de datos de [Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) y en [Azure Blob Storage](../storage/common/storage-service-encryption.md) en una suscripción de Microsoft. Los metadatos se cifran en reposo.
- Los datos para el análisis de dependencia también se cifran en tránsito (HTTPS seguro). Se almacenan en un área de trabajo de Log Analytics de la suscripción. También se cifran en reposo.

### <a name="how-does-the-appliance-communicate-with-vcenter-server-and-azure-migrate"></a>¿Cómo se comunica el dispositivo con vCenter Server y Azure Migrate?

1. El dispositivo se conecta a vCenter Server (puerto 443) mediante las credenciales que proporcionó al configurar el dispositivo.
2. El dispositivo usa VMware PowerCLI para realizar consultas en vCenter Server, para recopilar metadatos sobre las máquinas virtuales que administra vCenter Server. Recopila los datos de configuración sobre máquinas virtuales (núcleos, memoria, discos, NIC) y el historial de rendimiento de cada máquina virtual del último mes.
3. Los metadatos recopilados se envían entonces a Azure Migrate: Server Assessment (mediante Internet a través de HTTPS) para la evaluación.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-server-instances"></a>¿Puedo conectar el mismo dispositivo en varias instancias de vCenter Server?

No. Hay una asignación unívoca entre un dispositivo y vCenter Server. Para detectar máquinas virtuales en varias instancias de vCenter Server, debe implementar varios dispositivos.


### <a name="machine-size-changed-can-i-run-the-assessment-again"></a>El tamaño de la máquina cambió. ¿Puedo volver a ejecutar la evaluación?

El dispositivo de Azure Migrate recopila continuamente información sobre el entorno local. Pero una evaluación es una instantánea de un momento dado de las máquinas virtuales locales. Si cambia la configuración de una máquina virtual que quiere evaluar, use la opción de recalcular para actualizar la evaluación con los últimos cambios.

### <a name="how-can-i-perform-discovery-in-a-multitenant-environment"></a>¿Cómo puedo realizar la detección de un entorno multiinquilino?

- En el caso de VMware, si el entorno se comparte entre los inquilinos y no quiere detectar las máquinas virtuales de un inquilino en la suscripción de otro inquilino, cree credenciales de vCenter Server con acceso solo a las máquinas virtuales que quiere detectar. Después, use esas credenciales al iniciar la detección en el dispositivo de Azure Migrate.
- En el caso de Hyper-V, la detección usa credenciales de host de Hyper-V. Si las máquinas virtuales comparten el mismo host de Hyper-V, no hay actualmente ninguna manera de separar la detección.  

### <a name="how-many-vms-can-i-discover-with-a-single-appliance"></a>¿Cuántas máquinas virtuales puedo detectar con un único dispositivo?

Puede detectar hasta 10 000 máquinas virtuales de VMware y hasta 5000 máquinas virtuales de Hyper-V con un único dispositivo de migración. Si tiene más máquinas en su entorno local, aprenda a escalar la valoración de [Hyper-V](scale-hyper-v-assessment.md) y [VMware](scale-vmware-assessment.md).

### <a name="can-i-delete-the-azure-migrate-appliance-from-the-project"></a>¿Puedo eliminar el dispositivo de Azure Migrate del proyecto?

Actualmente no se admite la eliminación del dispositivo del proyecto.

- La única manera de eliminar el dispositivo es eliminar el grupo de recursos que contiene el proyecto de Azure Migrate asociado con el dispositivo.
- Sin embargo, al eliminar el grupo de recursos también se eliminarán otras aplicaciones registradas, el inventario detectado, las evaluaciones y todos los demás componentes de Azure asociados al proyecto en el grupo de recursos.

## <a name="azure-migrate-server-assessment"></a>Evaluación de servidores de Azure Migrate


### <a name="does-azure-migrate-need-vcenter-server-for-vmware-vm-discovery"></a>¿Azure Migrate necesita vCenter Server para detectar una VM de VMWare?

Sí, Azure Migrate necesita vCenter Server para detectar un entorno de VMware. No admite la detección de hosts de ESXi que no administra vCenter Server.

### <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>¿Cuál es la diferencia entre usar Azure Migrate Server Assessment y MAP Toolkit?

Server Assessment proporciona una valoración para ayudar con la preparación de la migración y la evaluación de las cargas de trabajo para la migración a Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) ayuda con otras tareas, como el planeamiento de la migración para versiones más recientes de los sistemas operativos cliente y servidor de Windows y el seguimiento del uso de software. Para esos escenarios, siga usando MAP Toolkit.

### <a name="whats-the-difference-between-server-assessment-and-the-site-recovery-deployment-planner"></a>¿Cuál es la diferencia entre Server Assessment y Site Recovery Deployment Planner?

Server Assessment es una herramienta de planeamiento de la migración. Site Recovery Deployment Planner es una herramienta de planeamiento de recuperación ante desastres.

- **Planeamiento de la migración del entorno local a Azure**: Si planea migrar los servidores locales a Azure, use Server Assessment para planear la migración. Esta solución evalúa las cargas de trabajo locales y proporciona orientación y herramientas que le ayudarán a migrar. Una vez que esté implementado el plan de migración, puede usar herramientas como Azure Migrate Server Migration para migrar las máquinas a Azure.
- **Planeamiento de recuperación ante desastres en Azure**: Si planea configurar la recuperación ante desastres de una ubicación local a Azure con Site Recovery, use Site Recovery Deployment Planner. Deployment Planner realiza una evaluación profunda y específica de Site Recovery de su entorno local con el fin de la recuperación ante desastres. Proporciona recomendaciones sobre la recuperación ante desastres, como la replicación y la conmutación por error.

### <a name="whats-the-difference-between-as-on-premises-and-performance-based-sizing"></a>¿Cuál es la diferencia entre el ajuste de tamaño como local y el ajuste de tamaño basado en el rendimiento?

Con el ajuste como tamaño local, Azure Migrate no tiene en cuenta los datos de rendimiento de la máquina virtual para la evaluación. Evalúa los tamaños de las máquinas virtuales en función de la configuración local. Con el ajuste de tamaño basado en el rendimiento, el ajuste de tamaño se basa en los datos de uso.

- Por ejemplo, si hay una máquina virtual local con 4 núcleos y 8 GB de memoria con un 50 % de uso de CPU y un 50 % de uso de memoria, ocurrirá lo siguiente:
    - El ajuste de tamaño como local recomienda una SKU de máquina virtual de Azure con cuatro núcleos y 8 GB de memoria.
    - Pero el ajuste de tamaño basado en el rendimiento recomendará una SKU de máquina virtual de 2 núcleos y 4 GB de memoria, ya que se tiene en cuenta el porcentaje de uso.

- De forma similar, el ajuste de tamaño de disco depende de dos propiedades de evaluación: criterio de ajuste de tamaño y tipo de almacenamiento.
    - Si el criterio de ajuste de tamaño se basa en el rendimiento y el tipo de almacenamiento es automático, Azure Migrate tiene en cuenta los valores de rendimiento y de IOPS del disco al identificar el tipo de disco de destino (estándar o premium).
    - Si el criterio de ajuste de tamaño está basado en el rendimiento y el tipo de almacenamiento es premium, Azure Migrate recomienda una SKU de disco premium, basada en el tamaño del disco local. Se aplica la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es como local y el tipo de almacenamiento es estándar o premium.

### <a name="does-performance-history-and-utilization-percentile-impact-size-recommendations"></a>¿Qué impacto tiene el historial de rendimiento y el percentil de uso en las recomendaciones de tamaño?

Estas propiedades solo son aplicables para el ajuste de tamaño basado en el rendimiento.

- Azure Migrate recopila el historial de rendimiento de las máquinas locales y lo usa para recomendar el tipo de disco y el tamaño de la máquina virtual en Azure.
- El dispositivo realiza un perfil del entorno local continuamente para recopilar datos de uso en tiempo real cada 20 segundos.
- El dispositivo acumula ejemplos de 20 segundos y crea un único punto de datos cada 15 minutos.
- Para crear el punto de datos, el dispositivo selecciona el valor máximo de todos los ejemplos de 20 segundos.
- Después, el dispositivo envía este punto de datos a Azure.

Cuando se crea una evaluación en Azure, en función de la duración del rendimiento y el valor del percentil del historial de rendimiento, Azure Migrate calcula el valor del uso efectivo y lo usa para ajustar el tamaño.

- Por ejemplo, si establece la duración del rendimiento en 1 día y el valor del percentil en el percentil 95, Azure Migrate ordena los puntos de ejemplo de 15 minutos que envía el recopilador para el último día de manera ascendente y elige el valor del percentil 95 como el uso efectivo.
- El uso del valor de percentil 95 garantiza que se están omitiendo todos los valores atípicos. Se podrían incluir valores atípicos si se usara el percentil 99. Si quiere elegir el uso máximo para el período y no quiere perderse ningún valor atípico, seleccione el percentil 99.

## <a name="server-assessment---dependency-visualization"></a>Server Assessment: visualización de dependencias


### <a name="what-is-dependency-visualization"></a>¿Qué es la visualización de dependencias?

Use la visualización de dependencias para evaluar grupos de máquinas virtuales para la migración con mayor confianza. La visualización de dependencias comprueba las dependencias de las máquinas antes de llevar a cabo una valoración. Este proceso ayuda a asegurar que nada se queda atrás y evita interrupciones inesperadas al migrar a Azure. Azure Migrate usa la solución Service Map en Azure Monitor para habilitar la visualización de dependencias. [Más información](concepts-dependency-visualization.md).

> [!NOTE]
> La visualización de dependencias no está disponible en Azure Government.

### <a name="do-i-need-to-pay-to-use-dependency-visualization"></a>¿Tengo que pagar por usar la visualización de dependencias?
No. [Más información](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre los precios de Azure Migrate.

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>¿Es necesario instalar algo para la visualización de dependencias?

Para usar la visualización de dependencias, debe descargar e instalar agentes en cada máquina local que vaya a evaluar.

Debe instalar los siguientes agentes en cada máquina:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).
- [Dependency Agent](../azure-monitor/platform/agents-overview.md#dependency-agent).
- Si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.

No necesita estos agentes a menos que esté usando la visualización de dependencias.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>¿Puedo usar un área de trabajo existente para la visualización de dependencias?

Sí, puede asociar un área de trabajo existente al proyecto de migración y usarla para la visualización de dependencias. [Más información](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>¿Puedo exportar el informe de visualización de dependencias?

No, el informe de visualización de dependencias no se puede exportar. Sin embargo, Azure Migrate usa Service Map, y puede usar la [API REST de Service Map](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) para obtener las dependencias en formato JSON.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-the-dependency-agent"></a>¿Cómo puedo automatizar la instalación de Microsoft Monitoring Agent (MMA) y Dependency Agent?

Use este script[ para instalar los agentes de dependencia ](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples). Siga estas [instrucciones para instalar MMA](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) mediante la línea de comandos o la automatización. Para MMA, use [este script](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Además de los scripts, también puede utilizar las herramientas de implementación como System Center Configuration Manager o [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) para implementar los agentes.


### <a name="what-operating-systems-are-supported-by-mma"></a>¿Qué sistemas operativos admite MMA?

- Vea la lista de los [sistemas operativos Windows compatibles con MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
- Vea la lista de los [sistemas operativos Linux compatibles con MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="can-i-visualize-dependencies-for-more-than-an-hour"></a>¿Puedo visualizar las dependencias durante más de una hora?
No. Puede visualizar las dependencias durante un máximo de una hora. Puede volver a una fecha determinada del historial, un mes como máximo, pero la duración máxima de la visualización es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Pero puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

### <a name="can-i-use-dependency-visualization-for-groups-of-more-than-10-vms"></a>¿Puedo usar la visualización de dependencias para grupos que contienen más de 10 máquinas virtuales?
Puede [visualizar las dependencias](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) de grupos que tengan un máximo de diez máquinas virtuales. Si tiene un grupo con más de diez máquinas virtuales, se recomienda dividirlo en grupos más pequeños y luego visualizar las dependencias.

## <a name="azure-migrate-server-migration"></a>Migración de servidores de Azure Migrate

### <a name="whats-the-difference-between-azure-migrate-server-migration-and-site-recovery"></a>¿Cuál es la diferencia entre Azure Migrate Server Migration y Site Recovery?

- La migración sin agente de máquinas virtuales de VMware locales es una característica nativa dentro de Azure Migrate Server Migration.
- Azure Migrate Server Migration utiliza el motor de replicación de Azure Site Recovery para la migración de máquinas virtuales de Hyper-V, servidores físicos y máquinas virtuales de VMware basadas en agentes.


## <a name="next-steps"></a>Pasos siguientes
Consulte la sección [Información general de Azure Migrate](migrate-services-overview.md).
