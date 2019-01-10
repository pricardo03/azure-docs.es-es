---
title: Introducción a Azure Cosmos DB
description: Información acerca de Azure Cosmos DB. Esta base de datos de varios modelos y distribución global se ha creado con latencia baja, escalabilidad elástica y alta disponibilidad, y ofrece compatibilidad nativa con datos de NoSQL.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: overview
ms.date: 12/18/2018
ms.author: sngun
ms.openlocfilehash: b384bc51ac371ef75f5128c92f7e4b8d7f45ecc6
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034954"
---
# <a name="welcome-to-azure-cosmos-db"></a>Bienvenido a Azure Cosmos DB

Las aplicaciones actuales deben estar siempre en línea y tener una alta capacidad de respuesta. Para lograr baja latencia y alta disponibilidad, las instancias de estas aplicaciones deben implementarse en centros de datos que están cerca de sus usuarios. Es necesario que las aplicaciones respondan en tiempo real a grandes cambios, en cuanto a uso, en las horas punta, que almacenen volúmenes de datos que cada vez son mayores y que dichos datos estén a disposición de los usuarios en milisegundos.

Azure Cosmos DB es un servicio de base de datos con varios modelos distribuido de forma global de Microsoft. Con tan solo un clic, Azure Cosmos DB permite escalar de forma elástica e individual el rendimiento y el almacenamiento en cualquiera de las regiones geográficas de Azure. Puede escalar de forma elástica el rendimiento y almacenamiento y sacar provecho del rápido acceso a datos (menos de 10 milisegundos) mediante la API que prefiera entre SQL, MongoDB, Cassandra, Tables o Gremlin. Cosmos DB proporciona completos [acuerdos de nivel de servicio](https://aka.ms/acdbsla) (SLA) con garantías de rendimiento, latencia, disponibilidad y consistencia, algo que no pueden ofrecer los restantes servicios de base de datos.

También puede [probar gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) sin ninguna suscripción de Azure, de forma gratuita y sin compromiso.

> [!div class="nextstepaction"]
> [Probar gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB es el servicio de base de datos de distribución global de Microsoft con escalado elástico horizontal, baja latencia garantizada, cinco modelos de coherencia y los SLA más completos garantizados.](./media/introduction/azure-cosmos-db.png)

## <a name="key-benefits"></a>Ventajas principales

### <a name="turnkey-global-distribution"></a>Distribución global inmediata

Cosmos DB permite compilar aplicaciones con gran capacidad de respuesta y alta disponibilidad en todo el mundo. Cosmos DB replica de forma transparente sus datos allá donde estén sus usuarios, con el fin de que estos puedan interactuar con una réplica de los datos más cercanos a ellos.

Cosmos DB permite agregar o quitar cualquiera de las regiones de Azure de su cuenta de Cosmos en cualquier momento, con un solo clic de botón. Cosmos DB replicará perfectamente sus datos en todas las regiones asociadas con su cuenta de Cosmos y mientras su aplicación seguirá estando altamente disponible gracias a las funcionalidades de hospedaje múltiple del servicio.

Para más información, consulte el artículo acerca de la [distribución global](distribute-data-globally.md).

### <a name="always-on"></a>Always "On"

En virtud de la integración profunda con la infraestructura de Azure y la [replicación transparente de la arquitectura multimaestro](global-dist-under-the-hood.md), Cosmos DB proporciona una [alta disponibilidad](high-availability.md) del 99,999 % tanto para lecturas como para las escrituras. Cosmos DB también proporciona la capacidad para invocar mediante programación (o a través del Portal) la conmutación por error regional de su cuenta de Cosmos. Esta funcionalidad le ayuda a asegurarse de que aunque se pueda realizar automáticamente la conmutación por error de una base de datos de Cosmos, el resto de la aplicación también está diseñada para que se realice la conmutación por error si se produce un desastre regional.

### <a name="elastic-scalability-of-throughput-and-storage-worldwide"></a>Escalabilidad elástica del rendimiento y almacenamiento en todo el mundo

Cosmos DB se ha diseñado con creación de particiones horizontales y replicación de arquitectura multimaestro y ofrece una escalabilidad elástica sin precedentes para las operaciones de lectura y escritura en todo el mundo. Puede escalar verticalmente de forma elástica entre miles y cientos de millones de solicitudes por segundo en todo el mundo, con una sola llamada API y pagar solo por el rendimiento (y almacenamiento) que necesite. Esta funcionalidad le ayuda a tratar solucionar cargas de trabajo inesperadas de las de forma predecible sin tener que aprovisionar recursos de más por estos picos de actividad. Consulte [Creación de particiones en Azure Cosmos DB](partitioning-overview.md), [Aprovisionamiento del rendimiento en contenedores y bases de datos](set-throughput.md) y [Escalado del rendimiento para contenedores de Azure Cosmos DB](scaling-throughput.md).

### <a name="guaranteed-low-latency-at-99th-percentile-worldwide"></a>Baja latencia garantizada en el percentil 99, en todo el mundo

Mediante Cosmos DB se pueden crear aplicaciones de escala planetaria con una gran capacidad de respuesta. Con el nuevo protocolo de replicación de arquitectura multimaestro y un [motor de base de datos optimizado para escritura](index-policy.md) y sin bloqueos, Cosmos DB garantiza latencias inferiores a 10 ms para las operaciones de lectura y escritura (indexadas) en el percentil 99, en todo el mundo. Esta funcionalidad permite la ingesta sostenida de datos y consultas increíblemente rápidas en aplicaciones con alta capacidad de respuesta.

### <a name="precisely-defined-multiple-consistency-choices"></a>Varias opciones de coherencia definidas con precisión

Ya no es preciso que haga [concesiones extremas entre coherencia, disponibilidad, latencia y programabilidad](consistency-levels-tradeoffs.md). El protocolo de replicación de arquitectura multimaestro de Cosmos DB se ha diseñado meticulosamente para ofrecer [cinco opciones de coherencia bien definidas](consistency-levels.md) (segura, obsolescencia limitada, prefijo coherente, sesión y posible) para disponer de un modelo de programación intuitivo con baja latencia y alta disponibilidad para aplicaciones distribuidas globalmente.

### <a name="no-schema-or-index-management"></a>Sin administración de índices o esquemas

Mantener la sincronización entre los índices y el esquema de una base de datos y el esquema de una aplicación es especialmente difícil en las aplicaciones distribuidas globalmente. Sin embargo, con Cosmos DB no es preciso ocuparse de esquemas ni de índices. El motor de base de datos es totalmente independiente del esquema.  Al no ser necesario administrar esquemas ni índices, tampoco es preciso preocuparse por el tiempo de inactividad de las aplicaciones durante la migración de esquemas. Cosmos DB [indexa automáticamente todos los datos](index-policy.md) (no se necesitan esquemas ni índices) y atiende rápidamente las consultas.

### <a name="battle-tested-database-service"></a>Servicio de base de datos probado en el campo

Cosmos DB es un servicio fundamental en Azure. Durante casi una década, muchos productos de Microsoft han utilizado Cosmos DB para aplicaciones críticas a escala global, entre las que se incluyen Skype, Xbox, Office 365, Azure, etc. En la actualidad, Cosmos DB es uno de los servicios de crecimiento más rápido de Azure y lo usan muchos clientes externos y aplicaciones que requieren escalado flexible o replicación inmediata de arquitectura multimaestro, multirregión y con varios centros de datos para ofrecer baja latencia y alta disponibilidad de operaciones de lectura y escritura.

### <a name="ubiquitous-regional-presence"></a>Presencia en todas las regiones

Cosmos DB está disponible en todas las regiones de Azure del mundo, lo que incluye más de 54 regiones en la nube pública, Azure China 21Vianet, Azure Alemania, Azure Government y Azure Government para Department of Defense (DoD). Consulte [Presencia regional de Cosmos DB](regional-presence.md).

### <a name="secure-by-default-and-enterprise-ready"></a>Seguro de forma predeterminada y preparado para la empresa

Cosmos DB tiene la certificación de un [amplia gama de estándares de cumplimiento](compliance.md). Además, todos los datos de Cosmos DB se cifran tanto en reposo como en movimiento. Cosmos DB proporciona autorización a nivel de fila y cumple los estrictos estándares de seguridad.

### <a name="significant-tco-savings"></a>Ahorro considerable en TCO

Dado que Cosmos DB es un servicio totalmente administrado, no es preciso administrar y operar complejas implementaciones en varios centro de datos y actualizaciones del software de la base de datos ni pagar el soporte técnico, las licencias o las operaciones. Consulte [Optimización de costos con Cosmos DB](total-cost-ownership.md).

### <a name="industry-leading-comprehensive-slas"></a>Acuerdos de nivel de servicio extraordinarios

Cosmos DB es el primer y único servicio que ofrece [excelentes y completos acuerdos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/cosmos-db/) que incluyen una alta disponibilidad del 99,999 %, una latencia de lectura y escritura en el percentil 99, y rendimiento y coherencia garantizados.

### <a name="apache-spark--cosmos-db--operational-analytics-at-global-scale"></a>Apache Spark + Cosmos DB = análisis operativo en tiempo real

[Spark](spark-connector.md) se puede ejecutar en los datos almacenados en Cosmos DB. Esta funcionalidad permite realizar análisis operativo de baja latencia a escala global, sin que ello afecte a las cargas de trabajo transaccionales que operan directamente en Cosmos DB.

### <a name="develop-applications-for-cosmos-db-using-popular-nosql-apis"></a>Desarrollo de aplicaciones para Cosmos DB mediante API de NoSQL muy conocidas

Cosmos DB ofrece varias API para actualizar y consultar los datos almacenados en una base de datos de Cosmos. De forma predeterminada, [se puede utilizar SQL](how-to-sql-query.md) para actualizar y consultar los datos de las bases de datos de Cosmos.

Cosmos DB también implementa los protocolos de transferencia de [Cassandra](cassandra-introduction.md), [MongoDB](mongodb-introduction.md), [Gremlin](graph-introduction.md) y [Azure Table Storage](table-introduction.md) directamente en el servicio. Esto le permite seleccionar controladores de cliente (y herramientas) para las API de NoSQL que se usan con frecuencia directamente en su base de datos de Cosmos. Al admitir los protocolos de transferencia de las API de NoSQL que se usan con frecuencia, Cosmos DB le permite:

* Migrar fácilmente la aplicación a Cosmos DB, al tiempo que conserva partes importantes de la lógica de aplicación.
* Mantener la aplicación portátil y permanecer independiente de proveedores de nube.
* Obtener los mejores acuerdos de nivel de servicio del sector con respaldo financiero para las API de NoSQL comunes. 
* Escalar de forma elástica el rendimiento y el almacenamiento aprovisionados para sus bases de datos en función de sus necesidades y pagar solo el rendimiento y el almacenamiento que necesita, lo que se traduce en un importante ahorro de costos.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Soluciones que se benefician de Azure Cosmos DB

Cualquier [aplicación web, móvil, de juegos y de IoT](use-cases.md) que deba controlar grandes cantidades de datos, lecturas y escrituras en una escala [global](distribute-data-globally.md) con tiempos de respuesta casi en tiempo real para una variedad de datos se beneficiará de la alta disponibilidad, el elevado rendimiento, la latencia baja y la coherencia ajustable [garantizados](https://azure.microsoft.com/support/legal/sla/cosmos-db/) de Azure Cosmos DB. Conozca cómo se puede aplicar Azure CosmosDB a los sectores de [IoT y telemática](use-cases.md#iot-and-telematics), [comercio minorista y marketing](use-cases.md#retail-and-marketing), [juegos](use-cases.md#gaming) y [aplicaciones web y móviles](use-cases.md#web-and-mobile-applications).

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de las funcionalidades de [distribución global](distribute-data-globally.md) y [creación de particiones](partitioning-overview.md) de Cosmos DB.

Comience a usar Azure Cosmos DB con una de nuestras guías rápidas:

* [Introducción a SQL API de Azure Cosmos DB](create-sql-api-dotnet.md)
* [Introducción a la API de Azure Cosmos DB para MongoDB](create-mongodb-nodejs.md)
* [Introducción a la API Cassandra de Azure Cosmos DB](create-cassandra-dotnet.md)
* [Introducción a Gremlin API de Azure Cosmos DB](create-graph-dotnet.md)
* [Introducción a Table API de Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Pruebe gratis Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/)
