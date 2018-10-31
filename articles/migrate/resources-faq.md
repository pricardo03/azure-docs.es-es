---
title: 'Azure Migrate: preguntas más frecuentes (P+F) | Microsoft Docs'
description: Aborda las preguntas más frecuentes sobre Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: snehaa
ms.openlocfilehash: e39cf260cc4931fc0dddc4922479522cb521d08e
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407068"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate: preguntas más frecuentes (P+F)

En este artículo se incluyen las preguntas más frecuentes sobre Azure Migrate. Si tiene más preguntas después de leer este artículo, publíquelas en el [Foro de Azure Migrate](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>General

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>¿Admite Azure Migrate la evaluación de cargas de trabajo solo de VMware?

Sí, Azure Migrate solo admite actualmente la evaluación de cargas de trabajo de VMware. La compatibilidad con los servidores físicos e Hyper-V se habilitará en el futuro.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>¿Azure Migrate necesita vCenter Server para detectar un entorno de VMware?

Sí, Azure Migrate necesita vCenter Server para detectar un entorno de VMware. No admite la detección de hosts de ESXi que no administra vCenter Server.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>¿En qué se diferencia Azure Migrate de Azure Site Recovery?

Azure Migrate es un servicio de evaluación que le ayuda a detectar las cargas de trabajo locales y planear la migración a Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), además de ser una solución de recuperación ante desastres, le ayuda a migrar las cargas de trabajo locales a máquinas virtuales IaaS en Azure.

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>¿Cuál es la diferencia entre usar Azure Migrate para evaluaciones y MAP Tookit?

[Azure Migrate](migrate-overview.md) permite la evaluación de la migración específicamente para ayudarle en la preparación de la migración y la evaluación de cargas de trabajo en el entorno local a Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=7826) tiene otras funcionalidades. Por ejemplo, el plan de la migración de las versiones más recientes de los sistemas operativos cliente y servidor de Windows, el seguimiento del uso de software, etc. Para esos escenarios, siga usando MAP Toolkit.


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>¿En qué se diferencia Azure Migrate de Azure Site Recovery Deployment Planner?

Azure Migrate es una herramienta de planeación de migración y Azure Site Recovery Deployment Planner es una herramienta de planeación de recuperación ante desastres (DR).

**Migración de VMware a Azure**: si piensa migrar las cargas de trabajo locales a Azure, use Azure Migrate para planear la migración. Azure Migrate evalúa las cargas de trabajo local y proporciona orientación, información y mecanismos que le ayudarán a realizar la migración a Azure. Cuando esté preparado para el plan de migración, puede usar servicios como Azure Site Recovery y Azure Database Migration Service para migrar las máquinas a Azure.

**Migración de Hyper-V a Azure**: Azure Migrate solo admite actualmente la evaluación de las máquinas virtuales de VMware para la migración a Azure. La compatibilidad con Hyper-V está en el mapa de ruta de Azure Migrate. Mientras tanto, puede utilizar Site Recovery Deployment Planner. Una vez habilitada la compatibilidad con Hyper-V en Azure Migrate, puede usar Azure Migrate para planear la migración de las cargas de trabajo de Hyper-V.

**Recuperación ante desastres de VMware o Hyper-V en Azure**: si tiene previsto realizar la recuperación ante desastres (DR) en Azure con Azure Site Recovery (Site Recovery), use Site Recovery Deployment Planner para el plan de la recuperación. Site Recovery Deployment Planner realiza una evaluación profunda específica de ASR de su entorno local. Proporciona recomendaciones que Site Recovery necesita para realizar correctamente las operaciones de recuperación ante desastres, tales como la replicación y la conmutación por error de las máquinas virtuales.  

### <a name="which-azure-regions-are-supported-by-azure-migrate"></a>¿Qué regiones de Azure con compatibles con Azure Migrate?

Azure Migrate admite actualmente las ubicaciones de proyecto de migración de Este de EE. UU. y Centro-oeste de EE. UU. Aunque solo puede crear proyectos de migración en Centro-oeste de EE. UU. y Este de EE. UU., puede evaluar sus máquinas para [varias ubicaciones de destino](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). La ubicación del proyecto solo se usa para almacenar los datos detectados.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>¿Cómo se conecta el sitio local a Azure Migrate?

La conexión puede ser a través de Internet o usar ExpressRoute con emparejamiento público.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>¿Puedo proteger la configuración de máquina virtual con la plantilla .OVA?

Se pueden agregar componentes adicionales (por ejemplo, un antivirus) en la plantilla .OVA siempre que las reglas de firewall y de comunicación necesarias para que el dispositivo de Azure Migrate funcionen tal cual.   

## <a name="discovery"></a>Detección

### <a name="what-data-is-collected-by-azure-migrate"></a>¿Qué datos recopila Azure Migrate?

Azure Migrate admite dos tipos de detección: la detección basada en el dispositivo y la detección basada en el agente.
La detección basada en el dispositivo recopila metadatos acerca de las máquinas virtuales locales. La lista completa de los metadatos que recopila el dispositivo es la siguiente:

**Datos de configuración del máquina virtual**
- Nombre para mostrar de la máquina virtual (en vCenter)
- Ruta de acceso de inventario de la máquina virtual (host, clúster o carpeta en vCenter)
- Dirección IP
- Dirección MAC
- Sistema operativo
- Número de núcleos, discos, NIC
- Tamaño de memoria, tamaños de disco

**Datos de rendimiento de la máquina virtual**
- Uso de CPU
- Uso de la memoria
- Para cada disco conectado a la máquina virtual:
  - Rendimiento de lectura de disco
  - Rendimiento de escrituras de disco
  - Operaciones de lectura de disco por segundo
  - Operaciones de escritura de disco por segundo
- Para cada adaptador de red conectado a la máquina virtual:
  - Red interna
  - Red externa

La detección basada en el agente es una opción disponible además de la detección basada en el dispositivo, que ayuda a los clientes a [visualizar dependencias](how-to-create-group-machine-dependencies.md) de las máquinas virtuales locales. Los agentes de dependencias recopilan detalles, como el FQDN, el sistema operativo, la dirección IP, la dirección MAC, los procesos que se ejecutan dentro de la máquina virtual y las conexiones TCP entrantes o salientes de la máquina virtual. La detección basada en el agente es opcional y puede elegir no instalar los agentes si no quiere ver las dependencias de las máquinas virtuales.

### <a name="would-there-be-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>¿Habría algún impacto en el rendimiento en el entorno del host de ESXi analizado?

En el caso del [enfoque de detección de una sola vez](https://docs.microsoft.com/azure/migrate/concepts-collector#discovery-methods), para recopilar los datos de rendimiento, el nivel de estadísticas en vCenter Server debería establecerse en 3. Si se establece en este nivel, se recopilaría una gran cantidad de datos de resolución de problemas, que se almacenarían en la base de datos de vCenter Server. Por lo tanto, podría provocar algunos problemas de rendimiento en vCenter Server. Habrá un efecto insignificante en el host de ESXi.

Hemos introducido la generación de perfiles continua de datos de rendimiento (que se encuentra en versión preliminar). Con la generación de perfiles continua, ya no es necesario cambiar el nivel de estadísticas de vCenter Server para ejecutar una evaluación basada en el rendimiento. La aplicación del recopilador ahora generará perfiles de las máquinas locales para medir los datos de rendimiento de las máquinas virtuales. Esto tendría un impacto prácticamente nulo en el rendimiento en los hosts de ESXi, así como en vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>¿Dónde se almacenan los datos recopilados y durante cuánto tiempo?

Los datos que recopila el dispositivo recopilador se almacenan en la ubicación de Azure especificada al crear el proyecto de migración. Los datos se almacenan de forma segura en una suscripción de Microsoft y se eliminan cuando el usuario elimina el proyecto de Azure Migrate.

Para la visualización de dependencias, si instala agentes en las máquinas virtuales, los datos que recopilan los Dependency Agent se almacenan en los Estados Unidos en un área de trabajo de Log Analytics creada en la suscripción del usuario. Estos datos se eliminan al eliminar el área de trabajo de Log Analytics en su suscripción. [Más información](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>¿Los datos se cifran en reposo y mientras están en tránsito?

Sí, los datos recopilados se cifran tanto en reposo como mientras están en tránsito. Los metadatos que recopila el dispositivo, se envían de manera segura al servicio Azure Migrate por Internet a través de HTTPS. Los metadatos recopilados se almacenan en [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) y en [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) en una suscripción de Microsoft y se cifran en reposo.

Los datos que recopilan los agentes de dependencias también se cifran en tránsito (canal HTTPS seguro) y se almacenan en un área de trabajo de Log Analytics en la suscripción del usuario. También se cifran en reposo.

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>¿Cómo se comunica el recopilador con vCenter Server y con el servicio Azure Migrate?

El dispositivo recopilador se conecta vCenter Server (puerto 443) mediante las credenciales que el usuario proporcionó en el dispositivo. Realiza consultas a vCenter Server con VMware PowerCLI para recopilar metadatos acerca de las máquinas virtuales que administra vCenter Server. Recopila los datos de configuración sobre máquinas virtuales (núcleos, memoria, discos, NIC, etc.), así como el historial de rendimiento de cada máquina virtual del último mes de vCenter Server. Los metadatos recopilados, se envían a continuación al servicio Azure Migrate (por Internet a través de HTTPS) para su evaluación. [Más información](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>¿Puedo conectar el mismo dispositivo recopilador en varios servidores de vCenter?

Sí, se puede usar un único dispositivo recopilador para detectar varios servidores de vCenter, aunque no al mismo tiempo. Tiene que ejecutar las detecciones una detrás de otra.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>¿Se usa la plantilla .OVA en Site Recovery integrada con la utilizada en Azure Migrate?

En este momento no hay ninguna integración. La plantilla .OVA de Site Recovery se utiliza para configurar un servidor de configuración de Site Recovery para la replicación del servidor físico o de máquina virtual de VMware. La plantilla .OVA de Azure Migrate se usa para detectar las máquinas virtuales de VMware administradas por un servidor vCenter, en la evaluación de la migración.

### <a name="i-changed-my-machine-size-can-i-rerun-the-assessment"></a>He cambiado el tamaño de mi máquina. ¿Puedo volver a ejecutar la evaluación?

Si modifica la configuración de una máquina virtual que desea evaluar, desencadene la detección de nuevo antes de usar el dispositivo recopilador. En el dispositivo, utilice la opción **Volver a iniciar la recopilación** para ello. Una vez realizada la recopilación, seleccione la opción **Recalcular** para la evaluación en el portal, para obtener resultados actualizados de la evaluación.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>¿Cómo se puede detectar un entorno de varios inquilinos en Azure Migrate?

Si tiene un entorno que se comparte entre los inquilinos y no desea detectar las máquinas virtuales de un inquilino en la suscripción de otro inquilino, puede usar el campo Ámbito en el dispositivo recopilador para definir el ámbito de la detección. Si los inquilinos comparten hosts, cree una credencial que tenga acceso de solo lectura solamente a las máquinas virtuales que pertenecen al inquilino específico y, luego, use esta credencial en el dispositivo recopilador y especifique el ámbito como el host para realizar la detección. Como alternativa, también puede crear carpetas en vCenter Server (por ejemplo, carpeta1 para inquilino1 y carpeta2 para inquilino2), en el host compartido, mover las máquinas virtuales de inquilino1 a carpeta1 y las de inquilino2 a carpeta2 y, por último, definir el ámbito de las detecciones en el recopilador en consecuencia especificando la carpeta apropiada.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>¿Cuántas máquinas virtuales se pueden detectar en un solo proyecto de migración?

Puede detectar 1500 máquinas virtuales en un solo proyecto de migración. Si tiene varias máquinas en su entorno local, [obtenga más información](how-to-scale-assessment.md) sobre cómo puede detectar un entorno de gran tamaño en Azure Migrate.

## <a name="assessment"></a>Evaluación

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>¿Azure Migrate es compatible con la estimación de costes basada en Contrato Enterprise (EA)?

Actualmente, Azure Migrate no es compatible con la estimación de costes de la [oferta para Contrato Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support/). La solución alternativa consiste en especificar pago por uso como la oferta y especificar manualmente el porcentaje de descuento (se aplica a la suscripción) en el campo "Descuento" de las propiedades de valoración.

  ![Descuento](./media/resources-faq/discount.png)
  

## <a name="dependency-visualization"></a>Visualización de dependencia

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>¿Tengo que pagar por usar la característica de visualización de dependencias?

Azure Migrate está disponible sin costo adicional. [Aquí](https://azure.microsoft.com/pricing/details/azure-migrate/) puede encontrar más información sobre los precios de Azure Migrate.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>¿Puedo usar un área de trabajo existente para la visualización de dependencias?

Sí, Azure Migrate ahora permite asociar un área de trabajo existente para el proyecto de migración y aprovecharla para la visualización de dependencias. [Más información](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>¿Puedo exportar el informe de visualización de dependencias?

No, el informe de visualización de dependencias no se puede exportar. Sin embargo, como Azure Migrate usa Service Map para la visualización de dependencias, puede usar las [API REST de Service Map](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) para obtener las dependencias en formato json.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>¿Cómo puedo automatizar la instalación de Microsoft Monitoring Agent (MMA) y el agente de dependencia?

[Aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) hay un script que puede usar para la instalación del agente de dependencia. Para MMA, [aquí](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab) hay un script disponible en TechNet que puede aprovechar.

Además de los scripts, también puede aprovechar las herramientas de implementación como System Center Configuration Manager (SCCM) o [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), etc. para implementar los agentes.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>¿Cuáles son los sistemas operativos compatibles con MMA?

[Aquí](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) tiene una lista de los sistemas operativos Windows compatibles con MMA.
La lista de los sistemas operativos de Linux compatibles con MMA la encontrará [aquí](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>¿Cuáles son los sistemas operativos compatibles con el agente de dependencias?

[Aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) tiene una lista de los sistemas operativos Windows compatibles con el agente de dependencias.
La lista de los sistemas operativos de Linux compatibles con el agente de dependencias la encontrará [aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>¿Puedo visualizar las dependencias de Azure Migrate durante más de una hora?
No, Azure Migrate le permite visualizar las dependencias durante un máximo de una hora. Azure Migrate le permite volver atrás a una fecha concreta en el historial de hasta el último mes, pero el tiempo máximo durante el que se pueden visualizar las dependencias es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>¿Se admite la visualización de dependencias para grupos con más de diez máquinas virtuales?
Puede [visualizar las dependencias de grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que tengan un máximo de diez máquinas virtuales. Si tiene un grupo con más de diez, recomendamos dividirlo en grupos más pequeños y visualizar las dependencias.


## <a name="next-steps"></a>Pasos siguientes

- Consulte la sección [Información general de Azure Migrate](migrate-overview.md).
- Obtenga información sobre la [detección y evaluación](tutorial-assessment-vmware.md) de un entorno de VMware.
