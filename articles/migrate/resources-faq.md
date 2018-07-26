---
title: 'Azure Migrate: preguntas más frecuentes (P+F) | Microsoft Docs'
description: Aborda las preguntas más frecuentes sobre Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: snehaa
ms.openlocfilehash: 3f035f38b1ad68e9e39d151ffad3fc650a0a1d80
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952756"
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

Azure Migrate admite actualmente las ubicaciones de proyecto de migración de Este de EE. UU. y Centro-oeste de EE. UU. Tenga en cuenta que aunque solo puede crear proyectos de migración en Centro-oeste de EE. UU. y Este de EE. UU., puede evaluar sus máquinas para [varias ubicaciones de destino](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). La ubicación del proyecto solo se usa para almacenar los datos detectados.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>¿Cómo se conecta el sitio local a Azure Migrate?

La conexión puede ser a través de Internet o usar ExpressRoute con emparejamiento público.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>¿Puedo proteger la configuración de máquina virtual con la plantilla .OVA?

Se pueden agregar componentes adicionales (por ejemplo, un antivirus) en la plantilla .OVA siempre que las reglas de firewall y de comunicación necesarias para que el dispositivo de Azure Migrate funcionen tal cual.   

## <a name="discovery-and-assessment"></a>Detección y valoración

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

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>¿Dónde se almacenan los datos recopilados y durante cuánto tiempo?

Los datos que recopila el dispositivo recopilador se almacenan en la ubicación de Azure especificada al crear el proyecto de migración. Los datos se almacenan de forma segura en una suscripción de Microsoft y se eliminan cuando el usuario elimina el proyecto de Azure Migrate.

Para la visualización de dependencias, si instala a agentes en las máquinas virtuales, los datos que recopilan los agentes de dependencias se almacenan en los Estados Unidos en un área de trabajo de OMS creada en la suscripción del usuario. Estos datos se eliminan al eliminar el área de trabajo OMS en su suscripción. [Más información](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

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

## <a name="dependency-visualization"></a>Visualización de dependencia

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>¿Tengo que pagar por usar la característica de visualización de dependencias?

Azure Migrate está disponible sin costo adicional. [Aquí](https://azure.microsoft.com/pricing/details/azure-migrate/) puede encontrar más información sobre los precios de Azure Migrate.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>¿Puedo usar un área de trabajo existente para la visualización de dependencias?

Azure Migrate no admite el uso de un área de trabajo existente para la visualización de dependencias, pero Microsoft Monitoring Agent (MMA) admite el hospedaje múltiple y le permite enviar datos a varias áreas de trabajo. Así, si ya tiene los agentes implementados y configurados en un área de trabajo, puede usar el hospedaje múltiple y el agente MMA, y configurarlos para el área de trabajo de Azure Migrate (además del área de trabajo existente) y ponerlos en marcha. [Aquí](https://blogs.technet.microsoft.com/msoms/2016/05/26/oms-log-analytics-agent-multi-homing-support/) encontrará un blog sobre cómo habilitar el hospedaje múltiple en un agente MMA.

## <a name="next-steps"></a>Pasos siguientes

- Consulte la sección [Información general de Azure Migrate](migrate-overview.md).
- Obtenga información sobre la [detección y evaluación](tutorial-assessment-vmware.md) de un entorno de VMware.
