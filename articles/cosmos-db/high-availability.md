---
title: Alta disponibilidad en Azure Cosmos DB
description: En este artículo se describe cómo Azure Cosmos DB ofrece una alta disponibilidad
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 74e2d7901d127c9dd7edd8509e5bba082c4ad220
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978967"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Alta disponibilidad con Azure Cosmos DB

Azure Cosmos DB replica con transparencia los datos en todas las regiones de Azure asociadas con la cuenta de Cosmos. Cosmos DB emplea diferentes capas de redundancia para los datos, tal como se muestra en la imagen siguiente:

![Creación de particiones físicas](./media/high-availability/cosmosdb-data-redundancy.png)

- Los datos dentro de contenedores de Cosmos [con particiones horizontales](partitioning-overview.md).

- En cada una de las regiones, cada partición está protegida por un conjunto de réplicas con todas las escrituras replicadas y confirmadas de forma duradera por la mayoría de las réplicas. Las réplicas se distribuyen entre unos 10 o 20 dominios de error como máximo.

- Se replica cada partición de todas las regiones. Cada región contiene todas las particiones de datos de un contenedor de Cosmos y puede aceptar operaciones de escritura y servir lecturas.  

Si su cuenta de Cosmos se distribuyen entre *N* regiones de Azure, habrá al menos *N* x 4 copias de todos los datos. Además de proporcionar acceso a datos de baja latencia y escalar el rendimiento de escritura o lectura en las regiones asociadas con su cuenta de Cosmos, tener más regiones (mayor *N*) mejora la disponibilidad.  

## <a name="slas-for-availability"></a>SLA para disponibilidad

Como base de datos distribuida globalmente, Cosmos DB proporciona contratos de nivel de servicio completos que abarcan rendimiento, latencia en el percentil 99, coherencia y alta disponibilidad. En la tabla siguiente se muestran las garantías de alta disponibilidad proporcionadas por Cosmos DB para cuentas de una y varias regiones. Para lograr alta disponibilidad, siempre que configure las cuentas de Cosmos para tener varias regiones de escritura.

|Tipo de operación  | Región única |Varias regiones (escrituras de región única)|Varias regiones (escrituras de varias regiones) |
|---------|---------|---------|-------|
|Escrituras    | 99,99    |99,99   |99,999|
|Lecturas     | 99,99    |99,999  |99,999|

> [!NOTE]
> En la práctica, la disponibilidad de escritura real de obsolescencia limitada, sesión, prefijo coherente y modelos de coherencia eventual es significativamente mayor que los SLA publicados. La disponibilidad de lectura real para todos los niveles de coherencia es significativamente mayor que los SLA publicados.

## <a name="high-availability-with-cosmos-db-in-the-event-of-regional-outages"></a>Alta disponibilidad con Cosmos DB en caso de interrupciones regionales

Interrupciones regionales son algo habitual, y Azure Cosmos DB garantiza que la base de datos siempre es de alta disponibilidad. Los siguientes detalles capturar el comportamiento de Cosmos DB durante una interrupción, dependiendo de la configuración de la cuenta de Cosmos:

- Con Cosmos DB, antes de que el cliente confirme una operación de escritura, un cuórum de réplicas dentro de la región que acepta las operaciones de escritura confirma permanentemente los datos.

- Las cuentas de varias regiones configuradas con varias regiones de escritura seguirán teniendo una alta disponibilidad para las escrituras y lecturas. Las conmutaciones por error regionales son instantáneas y no requieren cambios de la aplicación.

- **Cuentas de varias regiones con una región de solo escritura (interrupción de la región de escritura):** durante una interrupción de la región de escritura, estas cuentas permanecerán con alta disponibilidad para las lecturas. Sin embargo, para operaciones de escritura debe **"Habilitar la conmutación automática por error"** en su Cosmos cuenta para la conmutación por error de la región afectada en otra región. La conmutación por error se producirá en el orden de prioridad de regiones que especificó. Cuando la región afectada se vuelve a conectar, los datos no replicados presentes en la región de escritura afectado durante la interrupción está disponible a través de la [conflictos fuente](how-to-manage-conflicts.md#read-from-conflict-feed). Las aplicaciones pueden leer los conflictos de fuente, resuelva los conflictos de acuerdo con la lógica específica de la aplicación y la escritura diferida de los datos actualizados en el contenedor de Cosmos según corresponda. Una vez que se recupera la región de escritura previamente afectada, se convierte en disponible automáticamente como una región de lectura. Se puede invocar una conmutación por error manual y configurar la región afectada como la región de escritura. Vuelva a hacer una conmutación por error manual mediante el uso de [CLI de Azure o Azure portal](how-to-manage-database-account.md#manual-failover). **No se produce ninguna pérdida de datos ni de disponibilidad** antes, durante o después de la conmutación por error manual. La aplicación sigue teniendo alta disponibilidad. 

- **Cuentas de varias regiones con una región de solo escritura (interrupción de la región de lectura):** durante una interrupción de la región de lectura, estas cuentas permanecerán con alta disponibilidad para lecturas y escrituras. La región afectada se desconecta automáticamente de la región de escritura y se marcará como sin conexión. El [SDK de Cosmos DB](sql-api-sdk-dotnet.md) leerá redirigir las llamadas a la siguiente región disponible en la lista de regiones preferidas. Si ninguna de las regiones de la lista de las regiones preferidas está disponible, las llamadas se devuelven automáticamente a la región actual de escritura. No es necesario realizar ningún cambio en el código de su aplicación para gestionar la interrupción de la región de lectura. Finalmente, cuando la región afectada se vuelve a conectar, la región de lectura previamente afectada se sincronizará automáticamente con la región de escritura actual y estará disponible de nuevo para atender las solicitudes de lectura. Las siguientes lecturas se redirigen a la región recuperada sin necesidad de realizar cambios en el código de la aplicación. Durante la conmutación por error y nueva combinación de una región errónea anterior, sigue Cosmos DB cumpliendo su garantía de coherencia de lectura.

- Las cuentas de una sola región pueden perder disponibilidad después de una interrupción regional. Siempre se recomienda configurar **al menos dos regiones** (si es posible, al menos dos escribir regiones) con su cuenta de Cosmos para garantizar una alta disponibilidad en todo momento.

- Incluso en un evento extremadamente poco frecuente y desafortunado cuando la región de Azure es irrecuperable permanentemente, no hay ninguna pérdida de datos si se configura la cuenta de Cosmos de varias regiones con el nivel de coherencia predeterminado de *seguro*. En el caso de una región de escritura irrecuperable permanentemente, para las cuentas de Cosmos de varias regiones configuradas con la coherencia de obsolescencia limitada, la ventana de pérdida potencial de datos está restringida a la ventana de obsolescencia (*K* o *T*); sesión, los niveles de coherencia de prefijo coherente y posible, la ventana de pérdida potencial de datos está restringida a un máximo de cinco segundos.

## <a name="building-highly-available-applications"></a>Compilación de aplicaciones de alta disponibilidad

- Para garantizar la disponibilidad de escritura y lectura, configure la cuenta de Cosmos para abarcar al menos dos regiones con varias regiones de escritura. Esta configuración ofrecerá la máxima disponibilidad, latencia más baja y mejor escalabilidad tanto para lecturas y escrituras están respaldado por los SLA. Para obtener más información, vea cómo [configurar la cuenta de Cosmos con varias regiones de escritura](tutorial-global-distribution-sql-api.md).

- Para cuentas de Cosmos de varias regiones que estén configuradas con una región única de escritura, [habilite la conmutación por error automática mediante la CLI de Azure o Azure Portal](how-to-manage-database-account.md#automatic-failover). Después de habilitar la conmutación automática por error, siempre que se produzca un desastre regional, Cosmos DB conmutarán por error automáticamente su cuenta.  

- Incluso si su cuenta de Cosmos es de alta disponibilidad, es posible que la aplicación no se haya diseñado correctamente para seguir teniendo alta disponibilidad. Para probar la disponibilidad alta-to-end de la aplicación, invoca periódicamente la [conmutación por error manual mediante la CLI de Azure o Azure portal](how-to-manage-database-account.md#manual-failover), como parte de sus pruebas de aplicaciones o la recuperación ante desastres (DR) aumenta.

- En un entorno de base de datos distribuida de forma global, existe una relación directa entre el nivel de coherencia y la durabilidad de los datos se produce una interrupción en toda la región. A medida que desarrolle el plan de continuidad empresarial, tendrá que saber el tiempo máximo aceptable para que la aplicación se recupere por completo tras un evento de interrupción. El tiempo necesario para que una aplicación se recupere totalmente se conoce como "objetivo de tiempo de recuperación (RTO)". También debe conocer el período máximo de actualizaciones de datos recientes que la aplicación puede tolerar perder al recuperarse después de un evento de interrupción. El período de tiempo de las actualizaciones que se puede permitir perder se conoce como objetivo de punto de recuperación (RPO). Para ver el RPO y el RTO de Azure Cosmos DB, consulte [Durabilidad de datos y los niveles de coherencia](consistency-levels-tradeoffs.md#rto).

## <a name="next-steps"></a>Pasos siguientes

Ahora puede pasar a la lectura de los artículos siguientes:

* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Escalado del rendimiento aprovisionado globalmente](scaling-throughput.md)
* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)
* [Cómo configurar la cuenta de Cosmos con varias regiones de escritura](how-to-multi-master.md)
