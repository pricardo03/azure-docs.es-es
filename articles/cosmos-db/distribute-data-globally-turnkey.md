---
title: Cómo Azure Cosmos DB permite la distribución global inmediata | Microsoft Docs
description: Obtenga más información sobre replicación geográfica, arquitectura multimaestro, conmutación por error y recuperación de datos a escala mundial con bases de datos globales de Azure Cosmos DB, un servicio de base de datos con varios modelos distribuido globalmente.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: 1aa0fa3d4d9e1821a6980d9334456a78eba7bfbc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956921"
---
# <a name="how-azure-cosmos-db-enables-turnkey-global-distribution"></a>Cómo Azure Cosmos DB permite la distribución global inmediata
Azure Cosmos DB ofrece las siguientes funcionalidades, que permiten escribir fácilmente aplicaciones distribuidas a escala mundial. Estas funcionalidades están disponibles a través de las [API de REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) basadas en el proveedor de recursos de Azure Cosmos DB, así como en Azure Portal.

## <a id="GlobalDistribution"></a>Distribución global

### <a id="RegionalPresence"></a>Presencia regional ubicua 
La presencia geográfica de Azure crece constantemente y aumenta el número de [regiones nuevas](https://azure.microsoft.com/regions/) en línea. Azure Cosmos DB está clasificado como *servicio fundamental* en Azure y está disponible en todas las nuevas regiones de Azure de forma predeterminada. Esto permite asociar una región geográfica a una cuenta de base de datos de Azure Cosmos DB en cuanto Azure abra la nueva región para la empresa.

### <a id="MultiMasterSupport"></a>Configuración de arquitectura multimaestro
Azure Cosmos DB proporciona compatibilidad nativa del lado servidor para varias regiones maestras que participan por igual en un modelo de escritura en cualquier lugar. Esta compatibilidad proporciona menos de 10 ms de latencia de escritura y una disponibilidad de escritura del 99,999 % según los [Acuerdo de Nivel de Servicio (SLA) con respaldo financiero](https://azure.microsoft.com/support/legal/sla/cosmos-db/). La arquitectura multimaestro está disponible para todas las API, incluidas [SQL](sql-api-introduction.md), [MongoDB](mongodb-introduction.md), [Cassandra](cassandra-introduction.md), [Graph](graph-introduction.md) y [Table](table-introduction.md), así como en todos los lenguajes de SDK de Cosmos DB. Azure Cosmos DB admite 4 niveles de coherencia (obsolescencia limitada, sesión, prefijo coherente y posible) para las cuentas con funcionalidad de arquitectura multimaestro.

### <a id="UnlimitedRegionsPerAccount"></a>Asociación de un número ilimitado de regiones a una cuenta de base de datos de Azure Cosmos DB
Azure Cosmos DB permite asociar cualquier número de regiones de Azure con su cuenta de base de datos de Azure Cosmos DB. Fuera de las restricciones de geovallado (por ejemplo, China o Alemania), no hay limitaciones en el número de regiones que se pueden asociar a una cuenta de base de datos de Azure Cosmos DB. La siguiente ilustración muestra una cuenta de base de datos configurada para abarcar 25 regiones de Azure.

![Cuenta de base de datos de Azure Cosmos DB que abarca 25 regiones de Azure](./media/distribute-data-globally-turnkey/spanning-regions.png)


### <a id="PolicyBasedGeoFencing"></a>Geovallado basado en directivas
Azure Cosmos DB está diseñado para admitir la geovalla basada en directivas. El geovallado es un componente importante para asegurar las restricciones de cumplimiento y regulación de datos, y puede evitar la asociación de una región específica a una cuenta. Entre los ejemplos de geovalla se incluyen los siguientes (aunque hay más): limitación del ámbito de distribución global a las regiones que estén dentro de una nube soberana (por ejemplo, China y Alemania) o dentro de un límite impositivo de gobierno (por ejemplo, Australia). Las directivas se controlan mediante los metadatos de la suscripción de Azure.

### <a id="DynamicallyAddRegions"></a>Incorporación y eliminación dinámicas de regiones
Azure Cosmos DB permite agregar (asociar) o quitar (desasociar) regiones en una cuenta de base de datos en cualquier momento (vea la [ilustración anterior](#UnlimitedRegionsPerAccount)). Gracias a la replicación paralela de los datos entre particiones, Azure Cosmos DB garantiza que cuando se agregue una nueva región, esta estará disponible para las operaciones en un plazo de 30 minutos en cualquier lugar del mundo (suponiendo que los datos tengan 100 TB o menos). 

### <a id="FailoverPriorities"></a>Prioridades de conmutación por error
Cuando no se utiliza la arquitectura multimaestro, los clientes pueden controlar la secuencia exacta de las conmutaciones por error regionales en los casos de una interrupción, Azure Cosmos DB permite asociar una *prioridad* a diversas regiones asociadas a la cuenta de la base de datos (vea la ilustración siguiente). Azure Cosmos DB garantiza que la secuencia de la conmutación por error automática se produce en el orden de prioridad que se haya especificado. Para obtener más información sobre las conmutaciones por error regionales, vea [Conmutaciones por error regionales automáticas para la continuidad empresarial en Azure Cosmos DB](regional-failover.md). La siguiente imagen muestra cómo un inquilino de Azure Cosmos DB puede configurar el orden de prioridad de la conmutación por error (panel derecho) para las regiones asociadas a una cuenta de la base de datos:

![Configuración de prioridades de conmutación por error con Azure Cosmos DB](./media/distribute-data-globally-turnkey/failover-priorities.png)

### <a id="ConsistencyLevels"></a>Varios modelos de coherencia bien definidos para bases de datos distribuidas globalmente
Azure Cosmos DB admite [varios modelos de coherencia bien definidos, intuitivos y prácticos](consistency-levels.md) respaldados por Acuerdos de Nivel de Servicio. Puede elegir un modelo de coherencia concreto (de la lista de opciones disponibles), en función de la carga de trabajo o de los escenarios.

### <a id="TunableConsistency"></a>Coherencia optimizable para bases de datos replicadas globalmente
Azure Cosmos DB permite invalidar y moderar la opción de coherencia predeterminada en función de cada solicitud mediante programación en tiempo de ejecución.

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Regiones de escritura y lectura configurables dinámicamente
Azure Cosmos DB permite configurar las regiones (asociadas a la base de datos) como regiones de "lectura", "escritura" o "lectura/escritura".

### <a id="ElasticallyScaleThroughput"></a>Rendimiento de escalado elástico en regiones de Azure
Para escalar elásticamente un contenedor de Azure Cosmos DB, es preciso aprovisionar el rendimiento mediante programación. El rendimiento se aplica a todas las regiones en que está distribuido el contenedor de Azure Cosmos DB.

### <a id="GeoLocalReadsAndWrites"></a>Escrituras y lecturas geolocalizadas
La principal ventaja de una base de datos distribuida globalmente es que ofrece acceso con latencia baja a datos en cualquier lugar del mundo. Azure Cosmos DB ofrece lecturas de latencia baja (menos de 10 milisegundos) y escribe en el percentil 99 en todo el mundo. Garantiza que todas las lecturas se atiendan desde la región de lectura más cercana (local). Para atender una solicitud de lectura, se usa el cuórum local de la región en la que se emite la lectura. Lo mismo se aplica a las escrituras. Las operaciones de escritura solo se reconocen después de que una mayoría de las réplicas hayan confirmado de manera duradera la escritura localmente, pero sin que sea preciso canalizarlas en réplicas remotas para confirmar las escrituras. Dicho de otro modo, el protocolo de replicación de Azure Cosmos DB opera con la asunción de que los cuórums de lectura y escritura son siempre locales en la región en la que se emitió la solicitud.

### <a id="ManualFailover"></a>Conmutación por error manual
Azure Cosmos DB permite desencadenar la conmutación por error de la cuenta de base de datos para validar las propiedades de disponibilidad *de un extremo a otro* de toda la aplicación (más allá de la base de datos). Puesto que se garantizan las propiedades de seguridad y actividad de la detección de errores y la elección de líder, Azure Cosmos DB garantiza *que no habrá pérdida de datos* en las operaciones de conmutación por error manual iniciadas por un inquilino.

### <a id="AutomaticFailover"></a>Conmutación por error automática
Azure Cosmos DB admite la conmutación por error automática en caso de una o varias interrupciones regionales. Durante una conmutación por error regional, Azure Cosmos DB mantiene sus acuerdos de nivel de servicio de latencia de lectura, disponibilidad del tiempo de actividad, coherencia y rendimiento. Azure Cosmos DB ofrece un límite superior con respecto al tiempo que puede tardar en completarse una operación de conmutación por error automática. Esta es la ventana de la potencial pérdida de datos durante la interrupción regional.

### <a id="GranularFailover"></a>Diseñado para diferentes granularidades de conmutación por error
En la actualidad, las funcionalidades de conmutación por error automática y manual se exponen en la granularidad de la cuenta de la base de datos. Tenga en cuenta que, internamente, el servicio Azure Cosmos DB está diseñado para ofrecer conmutación por error *automática* con la granularidad más precisa de una base de datos, contenedor o incluso partición (de un contenedor que posee un intervalo de claves). 

### <a id="MultiHomingAPIs"></a>Multiconexión en Azure Cosmos DB

En un escenario de arquitectura multimaestro que tenga una región de escritura y varias réplicas de lectura, Azure Cosmos DB permite interactuar con una base de datos usando puntos de conexión lógicos (independientes de la región) o físicos (específicos de regiones). El uso de puntos de conexión lógicos garantiza que la aplicación puede alojarse de forma transparente con multiconexión en caso de conmutación por error. El último, un punto de conexión físico, proporciona un control específico de la aplicación para redirigir las lecturas y escrituras a regiones concretas. En escenarios con varias regiones habilitadas para escritura, los usuarios deben especificar los puntos de conexión específicos de la región con regiones agregadas en orden de prioridad con fines de uso y redirecciones.

Encontrará información sobre cómo configurar preferencias de lectura para la [API SQL](../cosmos-db/tutorial-global-distribution-sql-api.md), [Table API](../cosmos-db/tutorial-global-distribution-table.md) y [API de MongoDB](../cosmos-db/tutorial-global-distribution-mongodb.md) en estos artículos.

### <a id="TransparentSchemaMigration"></a>Migración transparente y coherente del índice y esquema de la base de datos 
Azure Cosmos DB es totalmente [independiente del esquema](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). El diseño exclusivo de su motor de base de datos permite a Azure Cosmos DB indexar de forma automática y sincrónica todos los datos que ingiere sin necesidad de que el usuario aporte ningún esquema ni índices secundarios. Esto le permite recorrer en iteración la aplicación distribuida globalmente rápidamente, sin tener que preocuparse de la migración del índice y del esquema de la base de datos ni coordinar los lanzamientos de aplicaciones de varias fases de cambios de esquema. Azure Cosmos DB garantiza que los cambios en directivas de indexación que realiza explícitamente el usuario no generan una degradación del rendimiento ni de la disponibilidad.  

### <a id="ComprehensiveSLAs"></a>Acuerdos de Nivel de Servicio completos (algo más que la alta disponibilidad)
Al ser un servicio de base de datos distribuido globalmente, Azure Cosmos DB ofrece un Acuerdo de Nivel de Servicio exhaustivo y bien definido para la **disponibilidad**, la **latencia**, el **rendimiento** y la **coherencia** de la base de datos que se ejecuta a nivel global, independientemente del número de regiones asociadas a esta.  

## <a id="LatencyGuarantees"></a>Garantías de latencia
La principal ventaja de un servicio de base de datos distribuido globalmente como Azure Cosmos DB es ofrecer acceso con baja latencia a datos en cualquier lugar del mundo. Azure Cosmos DB ofrece una baja latencia garantizada en el percentil 99 para realizar diversas operaciones de base de datos. El protocolo de replicación que Azure Cosmos DB emplea garantiza que las operaciones de la base de datos (lecturas y escrituras) siempre se realizan en la región más cercana a la del cliente. El Acuerdo de Nivel de Servicio de latencia de Azure Cosmos DB ofrece garantías en el percentil 99 para lecturas y escrituras indexadas (sincrónicamente) y consulta diversos tamaños de solicitud y respuesta. Las garantías de latencia de las escrituras incluyen confirmaciones de cuórum de mayoría duradero en la región local.

### <a id="LatencyAndConsistency"></a>Relación de la latencia con la coherencia 
Para que un servicio distribuido globalmente ofrezca una coherencia segura en una instalación distribuida globalmente, necesita replicar las escrituras de forma sincrónica o realizar lecturas entre regiones de forma sincrónica. La velocidad de la luz y la confiabilidad de la red de área extensa dictan que esta coherencia segura dará como resultado latencias más altas y una menor disponibilidad de las operaciones de base de datos. Por lo tanto, a fin de garantizarle latencias bajas en el percentil 99 y una disponibilidad del 99,99 % para todas las cuentas de una sola región y todas las cuentas de varias regiones con coherencia moderada, y un 99,999 % de disponibilidad de lectura en todas las cuentas de base de datos de varias regiones, el servicio debe emplear la replicación asincrónica. Esto, a su vez, requiere que el servicio también ofrezca [modelos de coherencia no estrictos y bien definidos](consistency-levels.md): que no lleguen a ser seguros (para ofrecer garantía de disponibilidad y latencia baja), pero más seguras que una coherencia "eventual" (para ofrecer un modelo de programación intuitivo).

Azure Cosmos DB garantiza que no se requiere una operación de lectura para ponerse en contacto con réplicas de varias regiones para ofrecer la garantía de un nivel de coherencia específico. También garantiza que no se bloquea una operación de escritura en el momento en que los datos se replican por todas las regiones (es decir, las operaciones de escritura se replican de manera asincrónica por las regiones). Para las cuentas de base de datos de varias regiones hay disponibles niveles de coherencia segura y moderada. 

### <a id="LatencyAndAvailability"></a>Relación de la latencia con la disponibilidad 
La latencia y la disponibilidad son las dos caras de la misma moneda. Hablamos de la latencia de la operación en estado estable y de disponibilidad en caso de errores y particiones de red. Desde la perspectiva de las aplicaciones, una operación de base de datos que se ejecute lentamente no se puede diferencia de una base de datos que no esté disponible. 

Para diferenciar una latencia elevada de una falta de disponibilidad, Azure Cosmos DB ofrece un límite superior absoluto para la latencia de varias operaciones de base de datos. Si una operación de base de datos tarda en completarse más tiempo que el que establece el límite superior, Azure Cosmos DB devuelve un error de tiempo de espera. El Acuerdo de Nivel de Servicio de disponibilidad de Azure Cosmos DB garantiza de que los tiempos de espera se cuentan con respecto a este acuerdo. 

### <a id="LatencyAndThroughput"></a>Relación de la latencia con el rendimiento
Azure Cosmos DB no obliga a elegir entre latencia y rendimiento. Respeta el Acuerdo de Nivel de Servicio de latencia en el percentil 99 y de ofrecer el rendimiento que ha aprovisionado. 

## <a id="ConsistencyGuarantees"></a>Garantías de coherencia
Aunque el [modelo de coherencia segura](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) es el estándar de oro de la programación de datos, obliga a pagar el precio de una latencia más alta (en estado estable) y de una disponibilidad reducida (en caso de errores). 

Azure Cosmos DB ofrece un modelo de programación bien definido para que justifique la coherencia de los datos replicados. Para que pueda compilar con facilidad aplicaciones distribuidas globalmente con funcionalidad de hospedaje múltiple, los modelos de coherencia que expone Azure Cosmos DB están diseñados para no depender de la región desde la que se atienden las operaciones de lectura y escritura. 

El Acuerdo de Nivel de Servicio de coherencia de Azure Cosmos DB garantiza que el 100 % de las solicitudes de lectura cumplirán la garantía de coherencia en el modelo de coherencia especificado por el usuario (la cuenta de la base de datos o el nivel de la solicitud). Se considera que una solicitud de lectura ha cumplido el Acuerdo de Nivel de Servicio de coherencia si se satisfacen todas las garantías de coherencia asociadas al nivel de coherencia. En la tabla siguiente se capturan los modelos de coherencia y las garantías del modelo de coherencia. Cada uno de estos modelos de coherencia garantizan un Acuerdo de Nivel de Servicio (SLA) del 100 %. 

|Modelo de coherencia  | Características  |
|---------|---------|
|Alta |  Linearizable  |
|Uso vinculado  |  Lectura monotónica (dentro de una región), prefijo coherente y obsolescencia limitada &lt; K,T   |
|Sesión  |  Leer su propia escritura, lectura monotónica y prefijo coherente    |
|Prefijo coherente  | Prefijo coherente  |

### <a id="ConsistencyAndAvailability"></a>Relación de la coherencia con la disponibilidad
El [resultado de imposibilidad](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) del [teorema de CAP](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) demuestra que es totalmente imposible que un sistema permanezca disponible y ofrezca coherencia linearizable en caso de errores. El servicio de base de datos debe ser CP o AP, donde los sistemas CP renuncian a la disponibilidad en favor de una coherencia linearizable, mientras que los sistemas AP renuncian a la [coherencia linearizable](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) en favor de disponibilidad. Azure Cosmos DB nunca infringe el modelo de coherencia solicitado, lo que hace que formalmente sea un sistema CP. Sin embargo, en la práctica, la coherencia no es una propuesta de todo o nada, ya que hay varios modelos de coherencia bien definidos a lo largo de todo el espectro de coherencia que va de la coherencia linearizable a la eventual. Azure Cosmos DB identifica varios de los modelos de coherencia moderada que son aplicables a escenarios de la vida real y responden a un modelo de uso intuitivo. Azure Cosmos DB contempla los inconvenientes de la disponibilidad de coherencia y ofrece [varios modelos de coherencia moderados pero bien definidos](consistency-levels.md), así como una disponibilidad del 99,99 % para todas las cuentas de base de datos de una sola región y un 99,999 % de disponibilidad de lectura y escritura en todas las cuentas de base de datos de varias regiones. 

### <a id="ConsistencyAndAvailability"></a>Relación de la coherencia con la latencia
Una variación más exhaustiva del teorema de CAP se llama [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), que también tiene en cuenta las compensaciones de latencia y coherencia en estado estable. Indica que, en estado estable, el sistema de base de datos debe elegir entre coherencia y latencia. Con varios modelos de coherencia moderada (respaldados por cuórums de escritura, lectura local y replicación asincrónica), Azure Cosmos DB garantiza que todas las operaciones de lectura y escritura son locales en las regiones de lectura y escritura, respectivamente. Esto permite que Azure Cosmos DB ofrezca garantías de baja latencia dentro de la región para los modelos de coherencia especificados.  

### <a id="ConsistencyAndThroughput"></a>Relación de la consistencia con el rendimiento
Dado que la implementación de un modelo de coherencia específico depende de la elección de un [tipo de cuórum](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), el rendimiento también varía en función del modelo de coherencia elegido. Por ejemplo, en Azure Cosmos DB, el cargo de RU para lecturas con coherencia fuerte es aproximadamente el *doble* que el de lecturas posiblemente coherentes. En este caso, necesitará aprovisionar el doble de RU para lograr el mismo rendimiento. La siguiente imagen muestra la relación de la capacidad de lectura con un modelo de coherencia específico en Azure Cosmos DB:

![Relación entre coherencia y rendimiento](./media/distribute-data-globally-turnkey/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Garantías de rendimiento 
Azure Cosmos DB permite escalar elásticamente el rendimiento (así como el almacenamiento) en diferentes regiones en función de las necesidades o la demanda. La siguiente imagen muestra un único contenedor de Azure Cosmos DB con particiones horizontales (en tres particiones de recursos dentro de una región) y luego distribución global en tres regiones de Azure:

![Contenedores particionados y distribuidos de Azure Cosmos DB](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

Un contenedor de Azure Cosmos DB se distribuye en dos dimensiones: i) dentro de una región y ii) entre regiones. Este es el procedimiento: 

* **Distribución local**: en una sola región, un contenedor de Azure Cosmos DB se escala horizontalmente en cuanto a *particiones de recursos*. Cada partición de recursos administra un conjunto de claves y tiene una alta coherencia y disponibilidad, que se representa físicamente por cuatro réplicas, también conocidas como *conjunto de réplicas*, y la replicación del estado de la máquina entre las réplicas. Azure Cosmos DB es un sistema de recursos regido globalmente donde una partición de recursos es responsable de entregar su cuota de rendimiento del presupuesto de recursos del sistema que se le asignen. El escalado de un contenedor de Azure Cosmos DB es transparente para los usuarios. Azure Cosmos DB administra las particiones de recursos y las divide y combina según sea necesario a medida que cambien los requisitos de almacenamiento y rendimiento. 
* **Distribución global**: si es una base de datos de varias regiones, cada una de las particiones de recursos se distribuye entre esas regiones. Las particiones de recursos que poseen el mismo conjunto de claves en diversas regiones forman el *conjunto de particiones* (vea la [ilustración anterior](#ThroughputGuarantees)).  Las particiones de recursos dentro de un conjunto de particiones se coordinan mediante el uso de replicación del estado de la máquina en varias regiones asociadas a la base de datos. En función del nivel de coherencia configurado, las particiones de recursos de un conjunto de particiones se configuran dinámicamente mediante diferentes topologías (por ejemplo, estrella, cadena de margarita, árbol, etc.). 

En virtud de una administración de particiones con gran capacidad de respuesta, del equilibrio de carga y de una regulación de recursos estricta, Azure Cosmos DB permite escalar elásticamente el rendimiento entre varias regiones de Azure asociadas a un contenedor o base de datos de Azure Cosmos DB. El cambio del rendimiento aprovisionado es una operación en tiempo de ejecución en Azure Cosmos DB. De similar manera a otras operaciones de base de datos, Azure Cosmos DB garantiza el límite superior absoluto en la latencia de la solicitud para cambiar el rendimiento aprovisionado. Por ejemplo, la siguiente ilustración muestra un contenedor de un cliente con rendimiento aprovisionado elásticamente (oscila entre un millón y diez millones de solicitudes por segundo en dos regiones) en función de la demanda.

![Rendimiento aprovisionado elásticamente de Azure Cosmos DB](./media/distribute-data-globally-turnkey/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Relación del rendimiento con la coherencia 
Es lo mismo que se describió en [Relación de la coherencia con el rendimiento](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Relación del rendimiento con la disponibilidad
Azure Cosmos DB sigue manteniendo su disponibilidad alta cuando se realizan cambios en el rendimiento aprovisionado. Azure Cosmos DB administra de forma transparente las particiones de recursos (y realiza operaciones de división, combinación y clonación) y se asegura de que las operaciones no degraden el rendimiento o la disponibilidad, mientras que la aplicación aumenta o disminuye elásticamente el rendimiento. 

## <a id="AvailabilityGuarantees"></a>Garantías de disponibilidad
Azure Cosmos DB ofrece un Acuerdo de Nivel de Servicio con disponibilidad del 99,99 % para todas las cuentas de base de datos de una sola región y todas las cuentas de varias regiones con coherencia moderada, y disponibilidad del 99,999 % para todas las cuentas de base de datos de varias regiones. Como ya se ha descrito, las garantías de disponibilidad de Azure Cosmos DB incluyen un límite superior absoluto de la latencia de todas las operaciones de datos y del plano de control. Las garantías de disponibilidad no cambian con el número de regiones ni con la distancia geográfica entre regiones. Las garantías de disponibilidad son aplicables tanto a la conmutación por error manual como a la automática. Azure Cosmos DB ofrece API de multiconexión transparentes que garantizan que la aplicación puede funcionar en puntos de conexión lógicos y puede enrutar de forma transparente las solicitudes a la nueva región en caso de conmutación por error, o bien puede especificar puntos de conexión regionales concretos en orden de prioridad de uso. No es preciso volver a implementar la aplicación si se produce una conmutación por error regional y los Acuerdos de Nivel de Servicio se mantienen en todo momento.

### <a id="AvailabilityAndConsistency"></a>Relación de la disponibilidad con la coherencia, la latencia y el rendimiento
La relación de la disponibilidad con la coherencia, la latencia y el rendimiento se describe en las secciones [Relación de la coherencia con la disponibilidad](#ConsistencyAndAvailability), [Relación de la latencia con la disponibilidad](#LatencyAndAvailability) y [Relación del rendimiento con la disponibilidad](#ThroughputAndAvailability). 

## <a id="CustomerFacingSLAMetrics"></a>Métricas de Acuerdo de Nivel de Servicio orientadas al cliente
Azure Cosmos DB expone de forma transparente las métricas de rendimiento, latencia, coherencia y disponibilidad. A estas métricas se puede acceder mediante programación y a través de Azure Portal (vea la siguiente ilustración). También se pueden configurar alertas en varios umbrales mediante Azure Application Insights. La siguiente imagen muestra métricas de coherencia, latencia, rendimiento y disponibilidad están disponibles de forma transparente para todos los inquilinos:
 
![Métricas del Acuerdo de Nivel de Servicio visibles para clientes de Azure Cosmos DB](./media/distribute-data-globally-turnkey/customer-slas.png)

## <a id="Next Steps"></a>Pasos siguientes

* [Ventajas clave de la distribución global de Azure Cosmos DB](distribute-data-globally-benefits.md)

* [Cómo configurar la replicación global de bases de datos de Azure Cosmos DB](tutorial-global-distribution-sql-api.md)

* [Cómo habilitar la arquitectura multimaestro para cuentas de Azure Cosmos DB](enable-multi-master.md)

* [Cómo funcionan las conmutaciones por error manuales y automáticas en Azure Cosmos DB](regional-failover.md)

* [Descripción de resolución de conflictos en Azure Cosmos DB](multi-master-conflict-resolution.md)

* [Uso de la arquitectura multimaestro con las bases de datos NoSQL de código abierto](multi-master-oss-nosql.md)


## <a id="References"></a>Referencias
1. Eric Brewer. [Towards Robust Distributed Systems](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) (Hacia unos sistemas distribuidos sólidos)
2. Eric Brewer. [CAP Twelve Years Later – How the rules have changed](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf) (CAP doce años después, cómo han cambiado las reglas)
3. Gilbert, Lynch. - [Brewer&amp;#39;s Conjecture and Feasibility of Consistent, Available, Partition Tolerant Web Services](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) (Conjetura de Brewer y viabilidad de servicios web coherentes, disponibles y tolerantes a particiones)
4. Daniel Abadi. [Consistency Tradeoffs in Modern Distributed Database Systems Design](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf) (Equilibrios de coherencia en el diseño de los modernos sistemas de bases de datos distribuidas)
5. Martin Kleppmann. [Please stop calling databases CP or AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html) (Deje de llamar a las bases de datos CP o AP)
6. Peter Bailis et al. [Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf) [Obsolescencia limitada probabilística (PBS) para cuórums parciales prácticos].
7. Naor and Wool. [Load, Capacity and Availability in Quorum Systems](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) (Carga, capacidad y disponibilidad en sistemas de quórum)
8. Herlihy and Wing. [Lineralizability: A correctness condition for concurrent objects](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) (Linearización: una condición de corrección para objetos concurrentes)
9. [Acuerdo de Nivel de Servicio de Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
