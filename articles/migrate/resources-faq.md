---
title: 'Azure Migrate: preguntas más frecuentes (P+F) | Microsoft Docs'
description: Aborda las preguntas más frecuentes sobre Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: snehaa
ms.openlocfilehash: 17cead93325da903161d95b315435d6e7b106dbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61292990"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate: preguntas más frecuentes (P+F)

En este artículo se incluyen las preguntas más frecuentes sobre Azure Migrate. Si tiene más preguntas después de leer este artículo, publíquelas en el [Foro de Azure Migrate](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>General

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>¿Admite Azure Migrate la evaluación de cargas de trabajo solo de VMware?

Sí, Azure Migrate solo admite actualmente la evaluación de cargas de trabajo de VMware. La compatibilidad con Hyper-V está en versión preliminar; regístrese [aquí](https://aka.ms/migratefuture) para obtener acceso a la versión preliminar. La compatibilidad con los servidores físicos se habilitará en el futuro.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>¿Azure Migrate necesita vCenter Server para detectar un entorno de VMware?

Sí, Azure Migrate necesita vCenter Server para detectar un entorno de VMware. No admite la detección de hosts de ESXi que no administra vCenter Server.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>¿En qué se diferencia Azure Migrate de Azure Site Recovery?

Azure Migrate es un servicio de evaluación que le ayuda a detectar las cargas de trabajo locales y planear la migración a Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), además de ser una solución de recuperación ante desastres, le ayuda a migrar las cargas de trabajo locales a máquinas virtuales IaaS en Azure.

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>¿Cuál es la diferencia entre usar Azure Migrate para evaluaciones y MAP Tookit?

[Azure Migrate](migrate-overview.md) permite la evaluación de la migración específicamente para ayudarle en la preparación de la migración y la evaluación de cargas de trabajo en el entorno local a Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=7826) tiene otras funcionalidades como planeamiento de migración para versiones más recientes de los sistemas operativos cliente y servidor de Windows y el seguimiento del uso de software. Para esos escenarios, siga usando MAP Toolkit.


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>¿En qué se diferencia Azure Migrate de Azure Site Recovery Deployment Planner?

Azure Migrate es una herramienta de planeación de migración y Azure Site Recovery Deployment Planner es una herramienta de planeación de recuperación ante desastres (DR).

**Migración de VMware a Azure**: Si piensa migrar las cargas de trabajo locales a Azure, use Azure Migrate para planear la migración. Azure Migrate evalúa las cargas de trabajo local y proporciona orientación, información y mecanismos que le ayudarán a realizar la migración a Azure. Cuando esté preparado para el plan de migración, puede usar servicios como Azure Site Recovery y Azure Database Migration Service para migrar las máquinas a Azure.

**Migración de Hyper-V a Azure**: La versión disponible con carácter general de Azure Migrate admite actualmente la evaluación de las máquinas virtuales de VMware para la migración a Azure. La compatibilidad con Hyper-V está actualmente en versión preliminar con soporte de producción. Si está interesado en probar la versión preliminar, regístrese [aquí](https://aka.ms/migratefuture).

**Recuperación ante desastres de VMware o Hyper-V en Azure**: Si tiene previsto realizar la recuperación ante desastres (DR) en Azure con Azure Site Recovery (Site Recovery), use Site Recovery Deployment Planner para el plan de la recuperación. Site Recovery Deployment Planner realiza una evaluación profunda específica de ASR de su entorno local. Proporciona recomendaciones que Site Recovery necesita para realizar correctamente las operaciones de recuperación ante desastres, tales como la replicación y la conmutación por error de las máquinas virtuales.  

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>¿Qué geografías de Azure con compatibles con Azure Migrate?

Azure Migrate admite actualmente Europa, Estados Unidos y Azure Government como ubicaciones geográficas de proyecto. Aunque solo puede crear proyectos de migración en estas geografías, puede evaluar sus máquinas para [varias ubicaciones de destino](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). La geografía del proyecto solo se usa para almacenar los metadatos detectados.

**Geografía** | **Ubicación de almacenamiento de metadatos**
--- | ---
Azure Government | Gobierno de EE. UU. - Virginia
Asia | Sudeste asiático
Europa | Norte de Europa y Oeste de Europa
Estados Unidos | Centro-oeste de EE. UU. o Este de EE. UU.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>¿Cómo se conecta el sitio local a Azure Migrate?

La conexión puede ser a través de Internet o usar ExpressRoute con emparejamiento público.

### <a name="what-network-connectivity-requirements-are-needed-for-azure-migrate"></a>¿Qué requisitos de conectividad de red son necesarios para Azure Migrate?

Para que las direcciones URL y los puertos necesarios para Azure Migrate se comuniquen con Azure, consulte las [direcciones URL para la conectividad](https://docs.microsoft.com/azure/migrate/concepts-collector#urls-for-connectivity).

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>¿Puedo proteger la configuración de máquina virtual con la plantilla OVA?

Se pueden agregar componentes adicionales (por ejemplo, un antivirus) en la plantilla OVA siempre que las reglas de firewall y de comunicación necesarias para que el dispositivo de Azure Migrate funcionen tal cual.   

### <a name="to-harden-the-azure-migrate-appliance-what-are-the-recommended-antivirus-av-exclusions"></a>Para proteger el dispositivo de Azure Migrate, ¿qué exclusiones del antivirus (AV) se recomiendan?

Debe excluir las carpetas siguientes del dispositivo para el examen del antivirus:

- La carpeta que contiene los archivos binarios del servicio Azure Migrate. Excluya todas las subcarpetas.
  %ProgramFiles%\ProfilerService  
- La aplicación web de Azure Migrate. Excluya todas las subcarpetas.
  %SystemDrive%\inetpub\wwwroot
- La caché local de la base de datos y los archivos de registro. El servicio Azure Migrate necesita acceso RW a esta carpeta.
  %SystemDrive%\Profiler

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

La detección basada en el agente es una opción disponible además de la detección basada en el dispositivo, que ayuda a los clientes a [visualizar dependencias](how-to-create-group-machine-dependencies.md) de las VM locales. Los agentes de dependencias recopilan detalles, como el FQDN, el sistema operativo, la dirección IP, la dirección MAC, los procesos que se ejecutan dentro de la máquina virtual y las conexiones TCP entrantes o salientes de la máquina virtual. La detección basada en el agente es opcional y puede elegir no instalar los agentes si no quiere ver las dependencias de las máquinas virtuales.

### <a name="would-there-be-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>¿Habría algún impacto en el rendimiento en el entorno del host de ESXi analizado?

Con los datos de rendimiento de la generación de perfiles continua, ya no es necesario cambiar el nivel de estadísticas de vCenter Server para ejecutar una evaluación basada en el rendimiento. La aplicación del recopilador generará perfiles de las máquinas locales para medir los datos de rendimiento de las máquinas virtuales. Esto tendría un impacto prácticamente nulo en el rendimiento en los hosts de ESXi, así como en vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>¿Dónde se almacenan los datos recopilados y durante cuánto tiempo?

Los datos que recopila el dispositivo recopilador se almacenan en la ubicación de Azure especificada al crear el proyecto de migración. Los datos se almacenan de forma segura en una suscripción de Microsoft y se eliminan cuando el usuario elimina el proyecto de Azure Migrate.

Para la visualización de dependencias, si instala agentes en las máquinas virtuales, los datos que recopilan los Dependency Agent se almacenan en los Estados Unidos en un área de trabajo de Log Analytics creada en la suscripción del usuario. Estos datos se eliminan al eliminar el área de trabajo de Log Analytics en su suscripción. [Más información](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="what-is-the-volume-of-data-which-is-uploaded-by-azure-migrate-in-the-case-of-continuous-profiling"></a>¿Qué es el volumen de datos que carga Azure Migrate en caso de generación de perfiles continua?

El volumen de datos que se envían a Azure Migrate varia en función de varios parámetros. Para conceder a un número indicativo, un proyecto de diez máquinas (cada uno con un disco y una NIC), tendría que enviar alrededor de 50 MB por día. Se trata de un valor aproximado y podría cambiar en función del número de puntos de datos para las NIC y los discos (los datos enviados no serían lineales si aumenta el número de máquinas, NIC o discos).

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>¿Los datos se cifran en reposo y mientras están en tránsito?

Sí, los datos recopilados se cifran tanto en reposo como mientras están en tránsito. Los metadatos que recopila el dispositivo, se envían de manera segura al servicio Azure Migrate por Internet a través de HTTPS. Los metadatos recopilados se almacenan en [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) y en [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) en una suscripción de Microsoft y se cifran en reposo.

Los datos que recopilan los agentes de dependencias también se cifran en tránsito (canal HTTPS seguro) y se almacenan en un área de trabajo de Log Analytics en la suscripción del usuario. También se cifran en reposo.

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>¿Cómo se comunica el recopilador con vCenter Server y con el servicio Azure Migrate?

El dispositivo recopilador se conecta vCenter Server (puerto 443) mediante las credenciales que el usuario proporcionó en el dispositivo. Realiza consultas a vCenter Server con VMware PowerCLI para recopilar metadatos acerca de las máquinas virtuales que administra vCenter Server. Recopila los datos de configuración sobre máquinas virtuales (núcleos, memoria, discos, NIC, etc.), así como el historial de rendimiento de cada máquina virtual del último mes de vCenter Server. Los metadatos recopilados, se envían a continuación al servicio Azure Migrate (por Internet a través de HTTPS) para su evaluación. [Más información](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>¿Puedo conectar el mismo dispositivo recopilador en varios servidores de vCenter?

Sí, se puede usar un único dispositivo recopilador para detectar varios servidores de vCenter, aunque no al mismo tiempo. Tiene que ejecutar las detecciones una detrás de otra.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>¿Se usa la plantilla OVA en Site Recovery integrada con la utilizada en Azure Migrate?

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

### <a name="what-is-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>¿Cuál es la diferencia entre el ajuste de tamaño como local y el ajuste de tamaño basado en el rendimiento?

Al especificar el criterio de ajuste de tamaño como local, Azure Migrate no tiene en cuenta los datos de rendimiento de las máquinas virtuales y ajusta el tamaño de las máquinas virtuales en función de la configuración local. Si el criterio de ajuste de tamaño se basa en el rendimiento, el ajuste se realiza basándose en datos de uso. Por ejemplo, si hay una máquina virtual local con 4 núcleos y 8 GB de memoria con un 50 % de uso de CPU y un 50 % de uso de memoria. Si el criterio de ajuste de tamaño es como local, se recomienda ajustar el tamaño de una SKU de máquina virtual de Azure con 4 núcleos y 8 GB de memoria. Sin embargo, si el criterio de ajuste de tamaño se basa en el rendimiento, se recomienda una SKU de máquina virtual de 2 núcleos y 4 GB, ya que el porcentaje de uso se tiene en cuenta al recomendar el tamaño. De forma similar, en el caso de los discos, el ajuste de tamaño de disco depende de dos propiedades de evaluación: criterio de ajuste de tamaño y tipo de almacenamiento. Si el criterio de ajuste de tamaño se basa en el rendimiento y el tipo de almacenamiento es automático, los valores de capacidad de proceso y de IOPS del disco se tendrán en cuenta para identificar el tipo de disco de destino (estándar o prémium). Si el criterio de ajuste de tamaño se basa en el rendimiento y el tipo de almacenamiento es prémium, se recomienda un disco prémium. La SKU de disco prémium en Azure se selecciona en función del tamaño del disco local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es como local y el tipo de almacenamiento es estándar o prémium.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>¿Qué impacto tiene el historial de rendimiento y uso de percentil en las recomendaciones de tamaño?

Estas propiedades solo son aplicables para el ajuste de tamaño basado en el rendimiento. Azure Migrate recopila el historial de rendimiento de las máquinas locales y lo usa para recomendar el tipo de disco y el tamaño de la máquina virtual en Azure. La aplicación del recopilador realiza un perfil del entorno local continuamente para recopilar datos de uso en tiempo real cada 20 segundos. El dispositivo acumula ejemplos de 20 segundos y crea un único punto de datos cada 15 minutos. Para crear el punto de datos único, el dispositivo selecciona el valor máximo de todos los ejemplos de 20 segundos y lo envía a Azure. Cuando se crea una evaluación en Azure, en función de la duración del rendimiento y el valor del percentil del historial de rendimiento, Azure Migrate calcula el valor del uso efectivo y lo usa para ajustar el tamaño. Por ejemplo, si ha establecido la duración del rendimiento en 1 día y el valor del percentil en el percentil 95, Azure Migrate usa puntos de ejemplo de 15 min que envía el recopilador para el último día, los ordena de manera ascendente y elige el valor del percentil 95 como el uso efectivo. El valor del percentil 95 garantiza que se van a omitir a los valores atípicos que pueden aparecer si elige el percentil 99. Si quiere elegir el uso máximo para el período y no quiere perderse ningún valor atípico, debe seleccionar el percentil 99.

## <a name="dependency-visualization"></a>Visualización de dependencia

> [!NOTE]
> La funcionalidad de visualización de dependencias no está disponible en Azure Government.

### <a name="what-is-dependency-visualization"></a>¿Qué es la visualización de dependencias?

La visualización de dependencias permite evaluar los grupos de máquinas virtuales para la migración con mayor confianza mediante la comprobación cruzada de dependencias de máquina antes de ejecutar una evaluación. La visualización de dependencias le ayuda a asegurarse de que nada se queda atrás, evitando interrupciones inesperadas al migrar a Azure. Azure Migrate aprovecha la solución Service Map de los registros de Azure Monitor para habilitar la visualización de dependencias.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>¿Tengo que pagar por usar la característica de visualización de dependencias?

No. [Aquí](https://azure.microsoft.com/pricing/details/azure-migrate/) puede encontrar más información sobre los precios de Azure Migrate.

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>¿Es necesario instalar algo para la visualización de dependencias?

Para usar la visualización de dependencias, debe descargar e instalar agentes en cada máquina local que vaya a evaluar.

- Es necesario tener instalado [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) en cada máquina.
- Es necesario tener instalado [Dependency Agent](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) en cada máquina.
- Además, si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la puerta de enlace de Log Analytics.

No necesita estos agentes en las máquinas que quiera evaluar a menos que esté usando la visualización de dependencias.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>¿Puedo usar un área de trabajo existente para la visualización de dependencias?

Sí, Azure Migrate ahora permite asociar un área de trabajo existente para el proyecto de migración y aprovecharla para la visualización de dependencias. [Más información](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>¿Puedo exportar el informe de visualización de dependencias?

No, el informe de visualización de dependencias no se puede exportar. Sin embargo, como Azure Migrate usa Service Map para la visualización de dependencias, puede usar las [API REST de Service Map](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) para obtener las dependencias en formato json.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>¿Cómo puedo automatizar la instalación de Microsoft Monitoring Agent (MMA) y el agente de dependencia?

[Aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) hay un script que puede usar para la instalación del agente de dependencia. [Aquí](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) tiene las instrucciones sobre cómo puede instalar MMA utilizando la línea de comandos o métodos automatizados. Para MMA, también puede aprovechar un script que está disponible [aquí](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab) en Technet.

Además de los scripts, también puede aprovechar las herramientas de implementación como System Center Configuration Manager (SCCM) o [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), etc. para implementar los agentes.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>¿Cuáles son los sistemas operativos compatibles con MMA?

[Aquí](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) tiene una lista de los sistemas operativos Windows compatibles con MMA.
La lista de los sistemas operativos de Linux compatibles con MMA la encontrará [aquí](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>¿Cuáles son los sistemas operativos compatibles con el agente de dependencias?

[Aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) tiene una lista de los sistemas operativos Windows compatibles con el agente de dependencias.
La lista de los sistemas operativos de Linux compatibles con el agente de dependencias la encontrará [aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>¿Puedo visualizar las dependencias de Azure Migrate durante más de una hora?
No, Azure Migrate le permite visualizar las dependencias durante un máximo de una hora. Azure Migrate le permite volver atrás a una fecha concreta en el historial de hasta el último mes, pero el tiempo máximo durante el que se pueden visualizar las dependencias es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Sin embargo, puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>¿Se admite la visualización de dependencias para grupos con más de diez máquinas virtuales?
También puede [visualizar las dependencias de grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que tengan un máximo de diez máquinas virtuales. Si tiene un grupo con más de diez máquinas virtuales, recomendamos dividirlo en grupos más pequeños y visualizar las dependencias.


## <a name="next-steps"></a>Pasos siguientes

- Consulte la sección [Información general de Azure Migrate](migrate-overview.md).
- Obtenga información sobre la [detección y evaluación](tutorial-assessment-vmware.md) de un entorno de VMware.
