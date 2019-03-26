---
title: Aspectos técnicos de la distribución global con Azure Cosmos DB
description: En este artículo se ofrecen detalles técnicos relacionados con la distribución global de Azure Cosmos DB
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/24/2019
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: a599be52575ed06cdb3a3713fc2f0915ab2f6c2b
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407494"
---
# <a name="global-data-distribution-with-azure-cosmos-db---under-the-hood"></a>Aspectos técnicos de la distribución de datos global con Azure Cosmos DB

Azure Cosmos DB es un servicio fundamental en Azure, por lo que se implementa en todas las regiones de Azure en todo el mundo como público, soberana, departamento de defensa (DoD) y nubes de gobierno. En un centro de datos, implementamos y administramos Azure Cosmos DB en sellos masivos de máquinas, cada uno con almacenamiento local dedicado. En un centro de datos, Azure Cosmos DB se implementa en muchos clústeres, cada uno de los cuales puede ejecutar varias generaciones de hardware. Las máquinas dentro de un clúster normalmente se reparten entre 10 y 20 dominios de error para alta disponibilidad dentro de una región. La siguiente imagen muestra la topología del sistema de distribución global de Cosmos DB:

![Topología del sistema](./media/global-dist-under-the-hood/distributed-system-topology.png)

**Distribución global en Azure Cosmos DB es llave en mano:** En cualquier momento, con unos pocos clics, o mediante programación con una sola llamada API, puede agregar o quitar las regiones geográficas asociadas con la base de datos de Cosmos. Una base de datos Cosmos, a su vez, consta de un conjunto de contenedores de Cosmos. En Cosmos DB, los contenedores sirven de unidades lógicas de distribución y escalabilidad. Las colecciones, tablas y gráficos que se crean son (internamente) tan solo contenedores de Cosmos. Los contenedores son completamente independiente del esquema y proporcionan un ámbito para una consulta. Los datos de un contenedor de Cosmos se indexan automáticamente tras su ingesta. Indexación automática permite a los usuarios consultar los datos sin las complicaciones de administración de esquemas o índices, especialmente en una configuración distribuida globalmente.  

- En una región determinada, los datos dentro de un contenedor se distribuyen mediante el uso de una clave de partición, que proporcionan y administran de manera transparente por las particiones físicas subyacentes (*distribución local*).  

- Cada partición física también se replica entre regiones geográficas (*distribución global*). 

Cuando una aplicación con Cosmos DB de manera flexible escala el rendimiento en un contenedor de Cosmos o consume más almacenamiento, Cosmos DB controla de forma transparente las operaciones de administración de particiones (dividir, clonar, eliminar) en todas las regiones. Independientemente del escalado, la distribución o los errores, Cosmos DB continúa proporcionando una sola imagen de sistema de los datos incluidos en los contenedores, que se distribuyen globalmente en cualquier cantidad de regiones.  

Como se muestra en la siguiente imagen, se distribuyen los datos dentro de un contenedor en dos dimensiones: dentro de una región como entre regiones, en todo el mundo:  

![Particiones físicas](./media/global-dist-under-the-hood/distribution-of-resource-partitions.png)

Una partición física se implementa mediante un grupo de réplicas, denominadas un *conjunto de réplicas*. Cada equipo aloja cientos de réplicas que corresponden a varias particiones físicas dentro de un conjunto fijo de procesos, tal como se muestra en la imagen anterior. Las réplicas que corresponden a las particiones físicas se colocan y su carga se equilibra de forma dinámica en las máquinas de un clúster y los centros de datos de una región.  

Una réplica pertenece de forma exclusiva a un inquilino de Azure Cosmos DB. Cada réplica hospeda una instancia del [motor de base de datos](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) de Cosmos DB, que administra los recursos, así como los índices asociados. El motor de base de datos de Cosmos DB funciona en un sistema tipo basado en la secuencia de registro de átomos (ARS). El motor es el concepto de un esquema, Desdibuja el límite entre los valores de instancia y de estructura de registros independiente. Cosmos DB logra la independencia total del esquema indexando todo automáticamente tras la ingesta de datos de forma eficaz, lo que permite a los usuarios consultar sus datos distribuidos globalmente sin tener que ocuparse de la administración de esquemas o de índices.

El motor de base de datos de Cosmos consta de componentes que se incluyen la implementación de varios elementos primitivos de coordinación, tiempos de ejecución de lenguaje, el procesador de consultas, almacenamiento y subsistemas responsables del almacenamiento transaccional de indización y la indexación de datos, respectivamente. Para proporcionar durabilidad y alta disponibilidad, el motor de base de datos conserva sus datos y el índice en discos SSD y los replica entre las instancias de motor de base de datos de los conjuntos de réplicas, respectivamente. Los inquilinos más grandes se corresponden con una escala mayor del rendimiento y almacenamiento y tener más grande o más réplicas, o ambos. Todos los componentes del sistema son completamente asincrónicos: no se bloquea ningún subproceso y el trabajo que realiza cada uno es de corta duración, sin incurrir en ningún cambio de subproceso innecesario. La limitación de frecuencia y la contrapresión se asocian en toda la pila desde el control de admisión a todas las rutas de acceso de E/S. Motor de base de datos de COSMOS está diseñado para aprovechar la simultaneidad específica y proporcionan un alto rendimiento mientras trabaja en frugales cantidades de recursos del sistema.

Distribución global de COSMOS DB se basa en dos abstracciones claves – *conjuntos de réplicas* y *conjuntos de particiones*. Un conjunto de réplicas es un bloque Lego modular para la coordinación, mientras que un conjunto de particiones es una superposición dinámica de una o varias particiones físicas distribuidas geográficamente. Para entender el funcionamiento de la distribución global, es preciso comprender estas dos abstracciones clave. 

## <a name="replica-sets"></a>Conjuntos de réplicas

Una partición física se materializa como un grupo de réplicas autoadministradas cuya carga se equilibra de forma dinámica que se reparten entre varios dominios de error, llamados conjunto de réplicas. Este conjunto implementa de forma colectiva el protocolo de máquina de estado replicado para que los datos de la partición física tengan una alta disponibilidad y sean duraderos y fuertemente coherentes. La pertenencia al conjunto de réplicas *N* es dinámico: mantiene fluctúa entre *nmín* y *Nmáx* en función de los errores, las operaciones administrativas y el tiempo por error réplicas para volver a generar o recuperar. En función de los cambios de pertenencia, el protocolo de replicación también reconfigura el tamaño de los cuórums de lectura y escritura. Para distribuir uniformemente el rendimiento que se asigna a una determinada partición física, se emplean dos ideas: 

- En primer lugar, el costo de procesar las solicitudes de escritura en el líder es mayor que el costo de aplicar las actualizaciones en el indicador. Consecuentemente, al líder se le presupuestan más recursos del sistema que a los seguidores. 

- En segundo lugar, en la medida de lo posible, el cuórum de lectura para un nivel de coherencia especificado se compone exclusivamente de las réplicas de los seguidores. Evitamos ponernos en contacto con el líder para el servicio de lecturas a menos que sea absolutamente necesario. Empleamos una serie de ideas desde el estudio realizado en la relación de [carga y capacidad](https://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) en los sistemas basados en quórum para el [cinco modelos de coherencia](consistency-levels.md) que Cosmos DB admite.  

## <a name="partition-sets"></a>Conjuntos de particiones

Un grupo de particiones físicas, uno por cada uno de los configurados con las regiones de base de datos de Cosmos, se compone para administrar el mismo conjunto de claves replicadas en todas las regiones configuradas. Este primitivo de mayor coordinación se llama conjunto de particiones: una superposición dinámica distribuida geográficamente de particiones físicas que administran un conjunto determinado de claves. Mientras que una partición física especificada (es decir, un conjunto de réplicas) se limita al ámbito de un clúster, un conjunto de particiones puede abarcar clústeres, centros de datos y regiones geográficas, tal y como se muestra en la imagen siguiente:  

![Conjuntos de particiones](./media/global-dist-under-the-hood/dynamic-overlay-of-resource-partitions.png)

Puede ver un conjunto de particiones como un "superconjunto de réplicas" geográficamente disperso, que consta de varios conjuntos de réplicas que poseen el mismo conjunto de claves. Similar a un conjunto de réplicas, la pertenencia a un conjunto particiones también es dinámica, varía en función de las operaciones implícitas de administración de partición física para agregar o quitar nuevas particiones en un conjunto determinado de particiones (por ejemplo, cuando se escala horizontalmente el rendimiento en un contenedor, se agrega o quita una región en la base de datos Cosmos o se producen errores). En virtud de que cada una de las particiones (de un conjunto de particiones) administra la pertenencia a conjuntos de particiones en su propio conjunto de réplicas, la pertenencia está completamente descentralizada y tiene una alta disponibilidad. Durante la reconfiguración de un conjunto de particiones, también se establece la topología de la superposición entre las particiones físicas. La topología se selecciona de forma dinámica en función del nivel de coherencia, la distancia geográfica y el ancho de banda de red disponible entre las particiones físicas de origen y las de destino.  

El servicio le permite configurar sus bases de datos de Cosmos con una sola región de escritura o varias de ellas y, según la opción, los conjuntos de particiones están configurados para aceptar las escrituras exactamente en una o en todas las regiones. El sistema emplea un protocolo de consensos anidado de dos niveles: un nivel funciona en las réplicas de un conjunto de réplicas de una partición física que acepta las escrituras, mientras que el otro funciona en el nivel de un conjunto de particiones para ofrecer garantías de ordenación completas para todas las escrituras confirmadas en el conjunto de particiones. Este consenso anidado multicapa es fundamental para la implementación de nuestros estrictos contratos de nivel de servicio de alta disponibilidad, así como para la implementación de los modelos de coherencia, que Cosmos DB ofrece a sus clientes.  

## <a name="conflict-resolution"></a>Resolución de conflictos

Nuestro diseño para la propagación de actualizaciones, la resolución de conflictos y el seguimiento de causalidades está inspirado en el anterior trabajo sobre [algoritmos epidémicos](https://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) y el sistema [Bayou](https://zoo.cs.yale.edu/classes/cs422/2013/bib/terry95managing.pdf). Mientras que los kernels de las ideas han sobrevivido y proporcionan un cómodo marco de referencia para comunicar el diseño del sistema de Cosmos DB, también han experimentado una transformación significativa al aplicarlos al sistema de Cosmos DB. Esto era necesario, ya que los sistemas anteriores no estaban diseñados ni con la regulación de recursos ni con la escala en la que Cosmos DB necesita funcionar ni tampoco para proporcionar las funcionalidades (por ejemplo, coherencia de obsolescencia limitada) y los estrictos y completos contratos de nivel de servicio que Cosmos DB ofrece a sus clientes.  

Recuerde que un conjunto de particiones se distribuye entre varias regiones y sigue el protocolo de replicación (arquitectura multimaestro) de Cosmos DB para replicar los datos entre las particiones físicas que componen un conjunto de particiones determinado. Cada partición física (de un conjunto de particiones) acepta las escrituras y suele ofrecer lecturas a los clientes que son locales en esa región. Las escrituras aceptadas por una partición física de una región se confirman permanentemente y tienen una alta disponibilidad en la partición física antes de ser reconocidas por el cliente. Se trata de escrituras provisionales que se propagan a otras particiones físicas en el conjunto de particiones mediante un canal de antientropía. Los clientes pueden solicitar escrituras provisionales o confirmadas pasando un encabezado de la solicitud. La propagación de antientropía (incluida la frecuencia de propagación) es dinámica, según la topología del conjunto de particiones, la proximidad regional de las particiones físicas y el nivel de coherencia configurado. En un conjunto de particiones, Cosmos DB sigue un esquema de confirmación principal con una partición de árbitro seleccionada dinámicamente. La selección de árbitro es dinámica y forma parte integral de la reconfiguración del conjunto de particiones según la topología de la superposición. Se garantiza que las escrituras confirmadas (incluidas las actualizaciones de varias filas o por lotes) están totalmente ordenadas. 

Empleamos relojes vectoriales codificados (que contienen relojes lógicos y de id. de región que corresponden a cada nivel de consenso en el conjunto de réplicas y el conjunto de particiones, respectivamente) para que el seguimiento de causalidades y los vectores de versión detecten y resuelvan conflictos relativos a las actualizaciones. La topología y el algoritmo de selección de homólogos se han diseñado para garantizar un almacenamiento fijo y mínimo y una sobrecarga de red mínima de los vectores de versión. El algoritmo garantiza la propiedad de convergencia estricta.  

En las bases de datos de Cosmos configuradas con varias regiones de escritura, el sistema ofrece varias directivas de resolución de conflictos automáticas flexibles para que los desarrolladores elijan entre ellas, incluidas: 

- Últimos casos de escritura correcta (LWW) que, de forma predeterminada, usa una propiedad de marca de tiempo definida por el sistema (que se basa en el protocolo de reloj de sincronización de hora). Cosmos DB también permite especificar cualquier otra propiedad numérica personalizada que se vaya a usar para la resolución de conflictos.  
- Directiva de resolución de conflictos personalizada (expresada a través de procedimientos de combinación) que se ha diseñado para la conciliación de conflictos con la semántica definida por la aplicación. Estos procedimientos se invocan tras la detección de los conflictos de escritura-escritura bajo los auspicios de una transacción de base de datos en el servidor. El sistema garantiza exactamente una vez la ejecución de un procedimiento de combinación como parte del protocolo de confirmación. Hay varios ejemplos disponibles con los que puede practicar.  

## <a name="consistency-models"></a>Modelos de coherencia

Independientemente de si configura su base de datos de Cosmos con una o con varias regiones de escritura, puede elegir entre cinco modelos de coherencia bien definidos. Con la recién agregada compatibilidad para permitir varias regiones de escritura, a continuación se indican algunos aspectos notables de los niveles de coherencia:  

Como antes, la coherencia de obsolescencia limitada garantiza que todas las lecturas van a estar dentro de prefijos k o segundos t desde la última escritura en cualquiera de las regiones. Además, se garantiza que las lecturas con coherencia de obsolescencia limitada son monotónicas y con garantías de prefijo coherente. El protocolo de antientropía funciona con limitación de frecuencia y garantiza que los prefijos no se acumulen y que la resistencia en los escritos no tenga que aplicarse. Como antes, la coherencia de sesión garantiza las garantías de lectura monotónica, de escritura monotónica, de lectura de sus propias escrituras (RYW), de escritura tras lectura y de prefijo coherente en todo el mundo. Para las bases de datos configuradas con una gran coherencia, los beneficios de la arquitectura multimaestro (esto es, baja latencia de escritura y alta disponibilidad de escritura) no se aplican debido a la replicación sincrónica entre regiones.

La semántica de los cinco modelos de consistencia en Cosmos DB se describe [aquí](consistency-levels.md) y se muestra matemáticamente con especificaciones de TLA+ de alto nivel [aquí](https://github.com/Azure/azure-cosmos-tla).

## <a name="next-steps"></a>Pasos siguientes

A continuación, aprenda a configurar la distribución global mediante los siguientes artículos:

* [Configuración de los clientes para el hospedaje múltiple](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Agregar o eliminar regiones de una cuenta de base de datos](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Creación de una directiva personalizada de resolución de conflictos para cuentas de API SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
