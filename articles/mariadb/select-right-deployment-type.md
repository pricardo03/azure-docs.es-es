---
title: Selección del tipo de implementación adecuada para Azure Database for MariaDB
description: En este artículo se describen los aspectos que se deben tener en cuenta antes de seguir adelante con la infraestructura como servicio (IaaS) o la plataforma como servicio (PaaS) para su instancia de Azure Database for MariaDB.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: fa87748719e2d3775034e5a2850281cf8f1a0e8a
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817383"
---
# <a name="choose-the-right-mariadb-server-option-in-azure"></a>Selección de la opción adecuada del servidor MariaDB en Azure

Con Azure, las cargas de trabajo del servidor MariaDB se pueden ejecutar en una máquina virtual de infraestructura hospedada (IaaS) o como un servicio hospedado (PaaS). PaaS dispone de varias opciones de implementación y, dentro de cada una, hay niveles de servicio. Al decidir entre PaaS o IaaS, debe determinar si quiere administrar la base de datos, aplicar revisiones y realizar copias de seguridad, o si quiere delegar estas operaciones en Azure.</br>

En función de su respuesta a esa pregunta, tenga en cuenta las siguientes opciones: <br/>

**Azure Database for MariaDB** es un motor de base de datos MariaDB totalmente administrado basado en la versión estable de la edición Community. Esta base de datos relacional como servicio (DBaaS), hospedada en la nube de Azure, se engloba en la categoría del sector denominada Plataforma como servicio (PaaS). Con una instancia administrada de MariaDB en Azure, pude usar características y funcionalidades integradas que requieren una configuración extensiva al usar MariaDB Server (tanto de forma local como en una máquina virtual de Azure). Al usar MariaDB como servicio, paga por el uso de opciones de escalado vertical u horizontal a fin de aumentar la potencia de forma ininterrumpida. Además, a diferencia del servidor MariaDB independiente, Azure Database for MariaDB tiene características adicionales como alta disponibilidad, inteligencia y administración integradas. <br/><br/>
**MariaDB en máquinas virtuales de Azure** se engloba en la categoría del sector denominada Infraestructura como servicio (IaaS) y permite ejecutar el servidor MariaDB en una máquina virtual totalmente administrada en la nube de Azure. Todas las versiones y ediciones recientes de MariaDB se pueden instalar en una máquina virtual IaaS. La diferencia más importante con Azure Database for MariaDB es que MariaDB en máquinas virtuales de Azure permite el control sobre el motor de base de datos. Sin embargo, este control suele venir acompañado de la responsabilidad adicional de administrar las máquinas virtuales y numerosas tareas de DBA, como el mantenimiento o la aplicación de revisiones, la recuperación y el diseño de alta disponibilidad del servidor de bases de datos, entre otras.

Las principales diferencias entre estas opciones se mencionan en la siguiente tabla:

|            | **Azure Database for MariaDB** | **MariaDB en máquinas virtuales de Azure**    |
|:-------------------|:-----------------------------|:--------------------|
| **Acuerdo de Nivel de Servicio**                | Ofrece un Acuerdo de Nivel de Servicio del 99,99 % de disponibilidad.| Hasta el 99,95 % de disponibilidad con dos o más instancias del mismo conjunto de disponibilidad. <br/>El 99,9 % con una máquina virtual de instancia única con Premium Storage. <br/> El 99,99 % con zona de disponibilidad con dos o más instancias en dos o más conjuntos de disponibilidad.<br/> Nota: [Acuerdo de Nivel de Servicio de máquina virtual](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) |
| **Aplicación de revisiones del sistema operativo**        | Automático  | Administrado por los clientes |
|**Aplicación de revisiones de MariaDB**     | Automático  | Administrado por los clientes |
| **Alta disponibilidad** | El modelo de alta disponibilidad se basa en los mecanismos de conmutación por error integrados cuando se produce una interrupción en el nivel de nodo. En tales casos, el servicio crea automáticamente una instancia y asocia el almacenamiento a esta nueva instancia. | El cliente diseña, implementa, prueba y mantiene la alta disponibilidad. Para ello, se puede emplear Always-On (clústeres de conmutación por error o replicación de grupo), trasvase de registros y replicación transaccional, según la versión del motor de MariaDB en uso.|
| **Redundancia de zona** | No se admite actualmente. | Las máquinas virtuales de Azure se pueden configurar para que se ejecuten en distintas zonas de disponibilidad. En el caso de una solución local, se espera que los clientes creen, administren y mantengan su propio centro de datos secundario.|
| **Escenarios híbridos** | [Replicación de datos de entrada](https://docs.microsoft.com/azure/MariaDB/concepts-data-in-replication) permite sincronizar datos de un servidor MariaDB externo con el servicio Azure Database for MariaDB. El servidor externo puede ser local, de máquinas virtuales o un servicio de base de datos hospedado por otros proveedores de nube.  <br/> <br/> La característica de [réplica de lectura](https://docs.microsoft.com/azure/postgresql/concepts-read-replicas) le permite replicar datos de un servidor Azure Database for MariaDB (maestro) en un máximo de cinco servidores de solo lectura (réplicas) de la misma región de Azure o entre regiones. Las réplicas de solo lectura se actualizan de forma asincrónica mediante la tecnología de replicación binlog.   <br/> <br/> Nota: La replicación de lectura entre regiones se encuentra actualmente en versión preliminar pública.| Administrado por los clientes <br/>
| **Copia de seguridad y restauración** | Crea automáticamente [copias de seguridad del servidor](https://docs.microsoft.com/azure/MariaDB/concepts-backup#backups) y las almacena en el almacenamiento con redundancia local o con redundancia geográfica configurado por el usuario. El servicio realiza copias de seguridad completas, diferenciales y del registro de transacciones. | Administrado por los clientes |
| **Supervisión de operaciones de base de datos** | Permite a los clientes [definir alertas](https://docs.microsoft.com/azure/MariaDB/concepts-monitoring) en la operación de base de datos y realizar acciones al alcanzar los umbrales. | Administrado por los clientes |
| **Protección contra amenazas avanzada** | Proporciona [Advanced Threat Protection](https://docs.microsoft.com/azure/MariaDB/howto-database-threat-protection-portal) que detecta actividades anómalas que indican intentos poco habituales y posiblemente dañinos de acceder a sus bases de datos o aprovechar sus vulnerabilidades. <br/> <br/> Nota: La protección contra amenazas avanzada está actualmente en versión preliminar pública.| Los clientes deben compilarla por sí mismos.
| **Copias de seguridad (recuperación ante desastres)** | Almacena copias de seguridad automatizadas en el usuario configurado en [almacenamiento con redundancia geográfica o con redundancia local](https://docs.microsoft.com/azure/MariaDB/howto-restore-server-portal). Las copias de seguridad pueden usarse también para restaurar un servidor a un momento dado. El período de retención se puede establecer de 7 a 35 días. La restauración se puede realizar mediante Azure Portal. <br/> | Totalmente administradas por el cliente, lo que incluye, entre otras tareas, la programación, la prueba, el archivado, el almacenamiento y la retención. Una opción adicional es usar Azure Recovery Services Vault para realizar la copia de seguridad de máquinas virtuales de Azure y bases de datos en máquinas virtuales (en versión preliminar). |
| **Recomendación de rendimiento** | Proporciona a los clientes [recomendaciones de rendimiento](https://techcommunity.microsoft.com/t5/Azure-Database-for-MariaDB/Azure-brings-intelligence-and-high-performance-to-Azure-Database/ba-p/769110) proactivas basadas en la telemetría de uso para ayudar a optimizar las cargas de trabajo. <br/> <br/> Nota: La recomendación de rendimiento se encuentra actualmente en versión preliminar pública. | Administrado por los clientes |


## <a name="business-motivations-for-choosing-paas-or-iaas"></a>Motivaciones empresariales para elegir PaaS o IaaS

Hay varios factores que pueden influir en la decisión para elegir PaaS o IaaS para hospedar las bases de datos MariaDB:

### <a name="cost"></a>Coste

Si se encuentra en una startup con falta de medios o en un equipo de una compañía bien establecida que opera con restricciones presupuestarias, los fondos limitados suelen ser el factor principal a la hora de decidir cómo hospedar las bases de datos. En esta sección se describen los conceptos básicos de facturación y licencias en Azure con respecto a Azure Database for MariaDB y MariaDB en máquinas virtuales de Azure:

#### <a name="billing"></a>Facturación

Actualmente, Azure Database for MariaDB está disponible como servicio en varios niveles con recursos de distintos precios, que se facturan todos por hora según una tarifa fija. Para acceder a la información más reciente sobre los niveles de servicio, los tamaños de proceso y las cantidades de almacenamiento admitidos actualmente, consulte el [modelo de compra basado en núcleos virtuales](https://docs.microsoft.com/azure/MariaDB/concepts-pricing-tiers). Los niveles de servicio y los tamaños de proceso se pueden ajustar de forma dinámica para satisfacer las necesidades variables de rendimiento de la aplicación. Se le factura por el tráfico saliente de Internet según las [tarifas de transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) habituales.

Con Azure Database for MariaDB, Microsoft configura el software de base de datos, le aplica revisiones y lo actualiza automáticamente, lo que reduce los costos de administración. Además, sus capacidades de [copia de seguridad integrada](https://docs.microsoft.com/azure/MariaDB/concepts-backup) ayudan a obtener un ahorro significativo, sobre todo, cuando se tiene gran cantidad de base de datos. Con MariaDB en máquinas virtuales de Azure, puede elegir y ejecutar cualquiera de las versiones de MariaDB. Con independencia de la versión de MariaDB que se use, se paga por la máquina virtual aprovisionada y los costos del tipo de licencia específico que se usa con MariaDB.

Azure Database for MariaDB proporciona alta disponibilidad integrada para cualquier tipo de interrupción de nivel de nodo y mantiene al mismo tiempo el Acuerdo de Nivel de Servicio del 99,99 % del servicio. Sin embargo, para conseguir una alta disponibilidad (HA) de la base de datos en las máquinas virtuales, el cliente debe recorrer las opciones de alta disponibilidad disponibles en la base de datos MariaDB, como la [replicación de MariaDB](https://mariadb.com/kb/en/library/setting-up-replication/). El uso de una opción de alta disponibilidad admitida no proporciona un Acuerdo de Nivel de Servicio adicional, pero le permite lograr un 99,99 % de disponibilidad de la base de datos a cambio de costo adicional y sobrecarga administrativa.

Para obtener más información sobre precios, consulte los siguientes recursos:
* [Precios de Azure Database for MariaDB](https://azure.microsoft.com/pricing/details/MariaDB/)
* [Precios de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Administración

En muchas empresas, la decisión de pasar a un servicio en la nube tiene que ver más con la posibilidad de reducir la complejidad de administración. Con IaaS y PaaS, Microsoft administra la infraestructura subyacente y replica automáticamente todos los datos para proporcionar recuperación ante desastres, configura y actualiza el software de base de datos, administra el equilibrio de carga y realiza una conmutación por error transparente en caso de error del servidor.

* **Con Azure Database for MariaDB**, puede seguir administrando la base de datos, pero ya no necesita administrar el motor de la base de datos, el sistema operativo ni el hardware. Las bases de datos y los inicios de sesión, el ajuste de índices y consultas, así como la auditoría y la seguridad, son ejemplos de elementos que puede seguir administrando. Además, la configuración de alta disponibilidad en otro centro de datos no requiere configuración y administración, o si acaso algunos ajustes mínimos.<br/><br/>
* **Con MariaDB en máquinas virtuales de Azure**, tiene un control completo sobre la configuración del sistema operativo y la instancia de servidor MariaDB. Con una máquina virtual, usted decide cuándo actualizar el software del sistema operativo y de la base de datos, y cuándo instalar cualquier otro software adicional, por ejemplo, una aplicación antivirus. Se proporcionan algunas características automatizadas para simplificar considerablemente la aplicación de revisiones, las copias de seguridad y la alta disponibilidad. Además, se puede controlar el tamaño de la máquina virtual, el número de discos y sus configuraciones de almacenamiento. Para más información, consulte los tamaños de máquinas virtuales y servicios en la nube en Azure.

### <a name="time-to-move-to-azure-br"></a>Es hora de cambiar a Azure <br/>
* **Azure Database for MariaDB** es la solución adecuada para las aplicaciones diseñadas para la nube cuando la productividad del desarrollador y un plazo de comercialización rápido de las nuevas soluciones son factores críticos. Con una funcionalidad de tipo DBA mediante programación, el servicio es perfecto para arquitectos y desarrolladores de la nube puesto que reduce la necesidad de administrar el sistema operativo y la base de datos subyacentes.<br/><br/>
* **MariaDB en máquinas virtuales de Azure** es perfecto para aplicaciones nuevas o existentes que requieren una base de datos MariaDB, o el acceso a sus características, en Windows o Linux, y se quiere evitar el tiempo y los gastos de adquirir nuevo hardware local. Esta opción también es una buena solución para migrar aplicaciones y bases de datos locales existentes a Azure tal cual, en casos en los que una instancia de Azure Database for MariaDB no es una buena solución. Dado que no es necesario cambiar las capas de presentación, aplicación y datos, se ahorra tiempo y presupuesto a la hora de renovar la arquitectura de la solución existente. Así, puede centrarse en migrar todas las soluciones a Azure y en abordar algunas optimizaciones de rendimiento que pueden requerir la plataforma de Azure.

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Precios de Azure Database for MariaDB](https://azure.microsoft.com/pricing/details/MariaDB/).
* Comenzar por [crear el primer servidor](https://review.docs.microsoft.com/azure/MariaDB/quickstart-create-MariaDB-server-database-using-azure-portal).

