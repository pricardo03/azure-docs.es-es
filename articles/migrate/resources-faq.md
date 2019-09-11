---
title: 'Azure Migrate: Preguntas más frecuentes | Microsoft Docs'
description: Aborda las preguntas más frecuentes sobre Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: snehaa
ms.openlocfilehash: 46c6ac52e1afb6c1619b814580a1059fd3dfedda
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279498"
---
# <a name="azure-migrate-frequently-asked-questions-faq"></a>Azure Migrate: Preguntas más frecuentes

En este artículo se responden a las preguntas más frecuentes sobre Azure Migrate. Si tiene más preguntas después de leer este artículo, publíquelas en el [Foro de Azure Migrate](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>General

### <a name="which-azure-geographies-does-azure-migrate-support"></a>¿Qué geografías de Azure admite Azure Migrate?

Consulte la [lista para VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) y [para Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="whats-the-difference-between-azure-migrate-and-azure-site-recovery"></a>¿Cuál es la diferencia entre Azure Migrate y Azure Site Recovery?

Azure Migrate proporciona un centro principal para iniciar la migración, ejecutar y realizar el seguimiento de la detección y la evaluación de las máquinas y las cargas de trabajo, así como ejecutar y realizar el seguimiento de la migración de las máquinas y las cargas de trabajo a Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) es una herramienta de recuperación ante desastres. Azure Migrate Server Migration utiliza Azure Site Recovery en el back-end para habilitar escenarios de migración para la migración mediante lift-and-shift de máquinas locales.

## <a name="azure-migrate-appliance"></a>Dispositivo con Azure Migrate

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>¿Cómo se conecta el dispositivo de Azure Migrate a Azure?

La conexión puede ser a través de Internet o puede usar Azure ExpressRoute con emparejamiento público o de Microsoft.

### <a name="what-are-the-network-connectivity-requirements-for-azure-migrate-server-assessment-and-migration"></a>¿Qué requisitos de conectividad de red son necesarios para Azure Migrate Server Assessment y Server Migration?

Revise las matrices de compatibilidad de [VMware](migrate-support-matrix-vmware.md) y [Hyper-V](migrate-support-matrix-hyper-v.md) para más información acerca de las direcciones URL y los puertos necesarios para permitir que Azure Migrate se comunique con Azure.

### <a name="can-i-harden-the-appliance-vm-that-i-set-up-with-the-template"></a>¿Se puede proteger la máquina virtual del dispositivo que se configuró con la plantilla?

Puede agregar componentes (por ejemplo, un antivirus) a la plantilla, siempre que las reglas de firewall y de comunicación necesarias para el dispositivo de Azure Migrate no se modifiquen.

### <a name="what-data-is-collected-by-the-azure-migrate-appliance"></a>¿Qué datos recopila el dispositivo de Azure Migrate?

Puede ver los detalles sobre los datos recopilados por el dispositivo de Azure Migrate en la [documentación de Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-appliance#collected-performance-data-vmware).

### <a name="is-there-any-performance-impact-on-the-analyzed-vmware-or-hyper-v-environment"></a>¿Hay algún impacto en el rendimiento en el entorno de VMware o Hyper-V analizado?

El dispositivo de Azure Migrate genera perfiles de máquinas locales continuamente para medir los datos de rendimiento de la máquina virtual. Esta generación de perfiles no tiene prácticamente ningún impacto en los hosts de Hyper-V/ESXi ni en vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>¿Dónde se almacenan los datos recopilados y durante cuánto tiempo?

Los datos que recopila el dispositivo de Azure Migrate se almacenarán en la ubicación de Azure que elija al crear el proyecto de migración. Los datos se almacenan de forma segura en una suscripción de Microsoft y se eliminan al eliminar el proyecto de Azure Migrate.

Para la visualización de dependencias, si instala agentes en las máquinas virtuales, los datos que recopilan los agentes de Dependency Agent se almacenan en los Estados Unidos en un área de trabajo de Log Analytics creada en la suscripción de Azure. Estos datos se eliminan al eliminar el área de trabajo de Log Analytics en su suscripción. Para más información, consulte [Visualización de dependencia](concepts-dependency-visualization.md).

### <a name="what-volume-of-data-is-uploaded-by-the-azure-migrate-appliance-during-continuous-profiling"></a>¿Cuál es el volumen de datos que carga el dispositivo de Azure Migrate durante la generación de perfiles continua?

El volumen de datos que se envían a Azure Migrate varía en función de varios parámetros. Para darle una idea del volumen, un proyecto de Azure Migrate con 10 máquinas (cada una con un disco y una NIC) envía aproximadamente 50 MB al día. Este valor es una aproximación. Cambia en función del número de puntos de datos de las NIC y los discos. (El aumento de los datos enviados no es lineal si aumenta el número de máquinas, NIC o discos).

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>¿Se cifran los datos en reposo y mientras están en tránsito?

Sí, en los dos casos. Los metadatos se envían de manera segura al servicio Azure Migrate por Internet a través de HTTPS. Los metadatos se almacenan en una base de datos de [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) y en [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) en una suscripción de Microsoft. Los metadatos se cifran en reposo.

Los datos que recopilan los agentes de dependencia también se cifran en tránsito (HTTPS seguro). Se almacenan en un área de trabajo de Log Analytics de la suscripción. También se cifran en reposo.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-vcenter-server-and-the-azure-migrate-service"></a>¿Cómo se comunica el dispositivo de Azure Migrate con vCenter Server y con el servicio Azure Migrate?

El dispositivo se conecta a vCenter Server (puerto 443) mediante las credenciales que proporcionó al configurar el dispositivo. Usa VMware PowerCLI para realizar consultas en vCenter Server para recopilar metadatos sobre las máquinas virtuales que administra vCenter Server. Recopila los datos de configuración sobre máquinas virtuales (núcleos, memoria, discos, NIC, etc.) y también el historial de rendimiento de cada máquina virtual del último mes. Los metadatos recopilados se envían posteriormente a Azure Migrate Server Assessment (por Internet a través de HTTPS) para su evaluación.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-server-instances"></a>¿Puedo conectar el mismo dispositivo en varias instancias de vCenter Server?

No. Hay una asignación unívoca entre un dispositivo y vCenter Server. Si tiene que detectar máquinas virtuales en varias instancias de vCenter Server, debe implementar varios dispositivos.


### <a name="i-changed-my-machine-size-can-i-run-the-assessment-again"></a>He cambiado el tamaño de mi máquina. ¿Puedo volver a ejecutar la evaluación?

El dispositivo de Azure Migrate recopila continuamente información sobre el entorno local. Pero una evaluación es una instantánea de un momento dado de las máquinas virtuales locales. Si cambia la configuración de una máquina virtual que quiere evaluar, use la opción Recalcular para actualizar la evaluación con los últimos cambios.

### <a name="how-can-i-perform-discovery-in-a-multitenant-environment-in-azure-migrate-server-assessment"></a>¿Cómo puedo realizar la detección de un entorno multiinquilino en Azure Migrate Server Assessment?

En el caso de VMware, si tiene un entorno compartido entre los inquilinos y no quiere detectar las máquinas virtuales de un inquilino en la suscripción de otro inquilino, cree credenciales de vCenter Server con acceso solo a las máquinas virtuales que quiere detectar. Después, use esas credenciales al iniciar la detección en el dispositivo de Azure Migrate.

En el caso de Hyper-V, la detección usa credenciales de host de Hyper-V. Si las máquinas virtuales comparten el mismo host de Hyper-V, no hay actualmente ninguna manera de separar la detección.  

### <a name="how-many-vms-can-i-discover-with-a-single-migration-appliance"></a>¿Cuántas máquinas virtuales puedo detectar con un único dispositivo de migración?

Puede detectar hasta 10 000 máquinas virtuales de VMware y hasta 5000 máquinas virtuales de Hyper-V con un único dispositivo de migración. Si tiene más máquinas en su entorno local, aprenda a escalar la valoración de [Hyper-V](scale-hyper-v-assessment.md) y [VMware](scale-vmware-assessment.md).

### <a name="can-i-delete-the-azure-migrate-appliance-from-the-project"></a>¿Puedo eliminar el dispositivo de Azure Migrate del proyecto?
Actualmente no se admite la eliminación del dispositivo del proyecto. La única manera de eliminar el dispositivo es eliminar el grupo de recursos que tiene el proyecto de Azure Migrate, asociado con el dispositivo, pero que también eliminará otras aplicaciones registradas, el inventario detectado, las evaluaciones y todos los demás artefactos de Azure. asociado al proyecto en el grupo de recursos.

## <a name="azure-migrate-server-assessment"></a>Evaluación de servidores de Azure Migrate

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>¿Es compatible Azure Migrate Server Assessment con la evaluación de servidores físicos?

No, Azure Migrate actualmente no admite la evaluación de servidores físicos.

### <a name="does-azure-migrate-need-vcenter-server-to-perform-discovery-in-a-vmware-environment"></a>¿Azure Migrate necesita vCenter Server para detectar un entorno de VMware?

Sí, Azure Migrate necesita vCenter Server para detectar un entorno de VMware. No admite la detección de hosts de ESXi que no administra vCenter Server.

### <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>¿Cuál es la diferencia entre usar Azure Migrate Server Assessment y MAP Toolkit?

Azure Migrate Server Assessment proporciona una evaluación de la migración para ayudar con la preparación de la migración y la evaluación de las cargas de trabajo para la migración a Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) ayuda con otras tareas como planeamiento de la migración para versiones más recientes de los sistemas operativos cliente y servidor de Windows y el seguimiento del uso de software. Para esos escenarios, siga usando MAP Toolkit.

### <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-azure-site-recovery-deployment-planner"></a>¿Cuál es la diferencia entre usar Azure Migrate Server Assessment y Azure Site Recovery Deployment Planner?

Azure Migrate Server Assessment es una herramienta de planeamiento de la migración. Azure Site Recovery Deployment Planner es una herramienta de planeamiento de recuperación ante desastres.

**Migración de VMware/Hyper-V a Azure**: Si planea migrar los servidores locales a Azure, use Azure Migrate Server Assessment para planear la migración. Esta solución evalúa las cargas de trabajo locales y proporciona orientación, información y herramientas que le ayudarán a migrar los servidores a Azure. Cuando esté preparado con el plan de migración, puede usar herramientas como Azure Migrate Server Migration para migrar las máquinas a Azure.

**Recuperación ante desastres de VMware o Hyper-V en Azure**: Para la recuperación ante desastres en Azure con Site Recovery, use Site Recovery Deployment Planner para la planeación. Site Recovery Deployment Planner realiza una evaluación profunda específica de Site Recovery de su entorno local. Proporciona recomendaciones que Site Recovery necesita para realizar correctamente las operaciones de recuperación ante desastres, tales como la replicación y la conmutación por error de las máquinas virtuales.

### <a name="does-azure-migrate-support-cost-estimation-for-the-enterprise-agreement-ea-program"></a>¿Admite Azure Migrate la estimación de costos para el programa del Contrato Enterprise?

Actualmente, Azure Migrate no es compatible con la estimación de costos del [programa del Contrato Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support/). La solución alternativa consiste en especificar **pago por uso** como la **oferta** y especificar manualmente el porcentaje de descuento (se aplica a la suscripción) en el campo **Descuento** de las propiedades de evaluación:

  ![Propiedades de la evaluación](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>¿Cuál es la diferencia entre el ajuste de tamaño como local y el ajuste de tamaño basado en el rendimiento?

Con el ajuste como tamaño local, Azure Migrate no tiene en cuenta los datos de rendimiento de la máquina virtual. Ajusta el tamaño de las máquinas virtuales en función de la configuración local. Con el ajuste de tamaño basado en el rendimiento, el ajuste de tamaño se basa en los datos de uso.

Por ejemplo, suponga que hay una máquina virtual local con 4 núcleos y 8 GB de memoria con un 50 % de uso de CPU y un 50 % de uso de memoria. Para esta máquina virtual, el ajuste de tamaño como local recomienda una SKU de máquina virtual de Azure con cuatro núcleos y 8 GB de memoria. Pero el ajuste de tamaño basado en el rendimiento recomienda una SKU de máquina virtual de 2 núcleos y 4 GB de memoria, ya que se tiene en cuenta el porcentaje de uso.

De forma similar, el ajuste de tamaño de disco depende de dos propiedades de evaluación: criterio de ajuste de tamaño y tipo de almacenamiento. Si el criterio de ajuste de tamaño se basa en el rendimiento y el tipo de almacenamiento es automático, Azure Migrate tiene en cuenta los valores de rendimiento y de IOPS del disco al identificar el tipo de disco de destino (estándar o premium).

Si el criterio de ajuste de tamaño está basado en el rendimiento y el tipo de almacenamiento es premium, Azure Migrate recomienda un disco premium. La SKU de disco premium se selecciona en función del tamaño del disco local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es como local y el tipo de almacenamiento es estándar o premium.

### <a name="what-impact-does-performance-history-and-utilization-percentile-have-on-size-recommendations"></a>¿Qué impacto tiene el historial de rendimiento y el percentil de uso en las recomendaciones de tamaño?

Estas propiedades solo son aplicables para el ajuste de tamaño basado en el rendimiento.

Azure Migrate recopila el historial de rendimiento de las máquinas locales y lo usa para recomendar el tipo de disco y el tamaño de la máquina virtual en Azure.

El dispositivo realiza un perfil del entorno local continuamente para recopilar datos de uso en tiempo real cada 20 segundos. El dispositivo acumula ejemplos de 20 segundos y crea un único punto de datos cada 15 minutos. Para crear el punto de datos, el dispositivo selecciona el valor máximo de todos los ejemplos de 20 segundos. Después, el dispositivo envía este punto de datos a Azure.

Cuando se crea una evaluación en Azure, en función de la duración del rendimiento y el valor del percentil del historial de rendimiento, Azure Migrate calcula el valor del uso efectivo y lo usa para ajustar el tamaño.

Por ejemplo, suponga que establece la duración del rendimiento en un día y el valor de percentil en el percentil 95. Azure Migrate ordena los puntos de ejemplo de 15 minutos que envió el recopilador para el día anterior en orden ascendente y selecciona el valor de percentil 95 como uso efectivo.

El valor de percentil 95 garantiza que se están omitiendo todos los valores atípicos. Se podrían incluir valores atípicos si se usara el percentil 99. Si quiere elegir el uso máximo para el período y no quiere perderse ningún valor atípico, seleccione el percentil 99.

### <a name="what-is-dependency-visualization"></a>¿Qué es la visualización de dependencias?

La visualización de dependencias le permite evaluar grupos de máquinas virtuales para la migración con mayor confianza. Comprueba las dependencias de las máquinas antes de llevar a cabo una valoración. La visualización de dependencias ayuda a asegurar que nada se queda atrás para evitar interrupciones inesperadas al migrar a Azure. Azure Migrate usa la solución Service Map en los registros de Azure Monitor para habilitar la visualización de dependencias.

> [!NOTE]
> La visualización de dependencias no está disponible en Azure Government.

### <a name="do-i-need-to-pay-to-use-dependency-visualization"></a>¿Tengo que pagar por usar la visualización de dependencias?

No. Para más información, consulte los [precios de Azure Migrate](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>¿Es necesario instalar algo para la visualización de dependencias?

Para usar la visualización de dependencias, debe descargar e instalar agentes en cada máquina local que vaya a evaluar.

Debe instalar los siguientes agentes en cada máquina:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).
- [Dependency Agent](../azure-monitor/platform/agents-overview.md#dependency-agent).
- Si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.

No necesita estos agentes a menos que esté usando la visualización de dependencias.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>¿Puedo usar un área de trabajo existente para la visualización de dependencias?

Sí, puede asociar un área de trabajo existente al proyecto de migración y usarla para la visualización de dependencias. Para más información, consulte la sección "Cómo funciona" del artículo [Visualización de dependencia](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>¿Puedo exportar el informe de visualización de dependencias?

No, el informe de visualización de dependencias no se puede exportar. Sin embargo, como Azure Migrate usa Service Map para la visualización de dependencias, puede usar la [API REST de Service Map](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) para obtener las dependencias en formato JSON.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-the-dependency-agent"></a>¿Cómo puedo automatizar la instalación de Microsoft Monitoring Agent (MMA) y Dependency Agent?

Use este script[ para instalar los agentes de dependencia ](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples). Siga estas [instrucciones para instalar MMA](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) mediante la línea de comandos o la automatización. Para MMA, use [este script](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Además de los scripts, también puede utilizar las herramientas de implementación como System Center Configuration Manager o [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) para implementar los agentes.

### <a name="what-operating-systems-are-supported-by-mma"></a>¿Qué sistemas operativos admite MMA?

- Vea la lista de los [sistemas operativos Windows compatibles con MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
- Vea la lista de los [sistemas operativos Linux compatibles con MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-operating-systems-are-supported-by-the-dependency-agent"></a>¿Cuáles son los sistemas operativos compatibles con Dependency Agent?

Consulte la lista de los [sistemas operativos Windows y Linux que son compatibles con Azure Monitor para VM](../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems).

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>¿Puedo visualizar las dependencias de Azure Migrate durante más de una hora?
No. Puede visualizar las dependencias durante un máximo de una hora. Puede volver a una fecha determinada del historial, un mes como máximo, pero la duración máxima de la visualización es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Pero puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

### <a name="can-i-use-dependency-visualization-for-groups-that-contain-more-than-10-vms"></a>¿Puedo usar la visualización de dependencias para grupos que contienen más de 10 máquinas virtuales?
Puede [visualizar las dependencias de grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que tengan un máximo de diez máquinas virtuales. Si tiene un grupo con más de diez máquinas virtuales, se recomienda dividirlo en grupos más pequeños y luego visualizar las dependencias.

## <a name="azure-migrate-server-migration"></a>Migración de servidores de Azure Migrate

### <a name="whats-the-difference-between-azure-migrate-server-migration-and-azure-site-recovery"></a>¿Cuál es la diferencia entre Azure Migrate Server Migration y Azure Site Recovery?

Azure Migrate Server Migration utiliza el motor de replicación de Site Recovery para la migración basada en agentes de máquinas virtuales de VMware, la migración de máquinas virtuales de Hyper-V y la migración de servidores físicos a Azure. La opción sin agente para migrar máquinas virtuales de VMware está integrada de forma nativa en Server Migration.

## <a name="next-steps"></a>Pasos siguientes
Consulte la sección [Información general de Azure Migrate](migrate-services-overview.md).
