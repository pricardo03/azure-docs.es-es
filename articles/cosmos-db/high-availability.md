---
title: Alta disponibilidad en Azure Cosmos DB
description: En este artículo se describe cómo Azure Cosmos DB ofrece una alta disponibilidad
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: fc818d2d7db60a8def99c2ad635580253dc795e0
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109765"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Alta disponibilidad con Azure Cosmos DB

Azure Cosmos DB replica con transparencia los datos en todas las regiones de Azure asociadas con la cuenta de Cosmos. Cosmos DB emplea diferentes capas de redundancia para los datos, tal como se muestra en la imagen siguiente:

![Creación de particiones físicas](./media/high-availability/cosmosdb-data-redundancy.png)

- Los datos en los contenedores de Cosmos se dividen horizontalmente.

- En cada una de las regiones, cada partición está protegida por un conjunto de réplicas con todas las escrituras replicadas y confirmadas de forma duradera por la mayoría de las réplicas. Las réplicas se distribuyen entre unos 10 o 20 dominios de error como máximo.

- Se replica cada partición de todas las regiones. Cada región contiene todas las particiones de datos de un contenedor de Cosmos y puede aceptar operaciones de escritura y servir lecturas.  

Si la cuenta de Cosmos se distribuye entre N regiones de Azure, habrá al menos N x cuatro copias de todos los datos. Además de proporcionar acceso de baja latencia a los datos y escalar el rendimiento de lectura y escritura en todas las regiones asociadas con la cuenta de Cosmos, tener más regiones (N superior) también mejora la disponibilidad.  

## <a name="slas-for-availability"></a>SLA para disponibilidad

Como base de datos distribuida globalmente, Cosmos DB proporciona contratos de nivel de servicio completos que abarcan rendimiento, latencia en el percentil 99, coherencia y alta disponibilidad. En la tabla siguiente se muestran las garantías de alta disponibilidad proporcionadas por Cosmos DB para cuentas de una y varias regiones. Para lograr alta disponibilidad, configure las cuentas de Cosmos para que tengan varias regiones de escritura.

|Tipo de operación  | Región única |Varias regiones (escrituras de región única)|Varias regiones (escrituras de varias regiones) |
|---------|---------|---------|-------|
|Escrituras    | 99,99    |99,99   |99,999|
|Lecturas     | 99,99    |99,999  |99,999|

> [!NOTE]
> En la práctica, la disponibilidad de escritura real para modelos de posible coherencia, prefijo coherente, sesión y obsolescencia limitada son significativamente mayores que los SLA publicados. La disponibilidad de lectura real para todos los niveles de coherencia es significativamente mayor que los SLA publicados.

## <a name="high-availability-with-cosmos-db-in-the-face-of-regional-outages"></a>Alta disponibilidad con Cosmos DB frente a interrupciones regionales

Las interrupciones regionales son algo habitual, y Azure Cosmos DB garantiza que su base de datos estará siempre disponible. Los siguientes detalles capturan el comportamiento de Cosmos DB durante una interrupción, en función de la configuración de la cuenta de Cosmos:

- Con Cosmos DB, antes de que el cliente confirme una operación de escritura, un cuórum de réplicas dentro de la región que acepta las operaciones de escritura confirma permanentemente los datos.

- Las cuentas de varias regiones configuradas con varias regiones de escritura seguirán teniendo una alta disponibilidad para las escrituras y lecturas. Las conmutaciones por error regionales son instantáneas y no requieren cambios de la aplicación.

- Cuentas de varias regiones con una región de solo escritura: durante una interrupción de la región de escritura, estas cuentas permanecerán con alta disponibilidad para las lecturas. Sin embargo, para operaciones de escritura debe "habilitar la conmutación automática por error" en la cuenta de Cosmos para conmutar por error la región afectada a otra región asociada. La conmutación por error se producirá en el orden de prioridad de regiones que especificó. Finalmente, cuando la región afectada vuelva a estar en línea, los datos no replicados presentes en la región de escritura afectado durante la interrupción estarán disponibles a través de la fuente de conflictos. Las aplicaciones pueden leer la fuente de conflictos, resolver los conflictos de acuerdo con la lógica específica de la aplicación y escribir los datos actualizados de nuevo en el contenedor de Cosmos según corresponda. Una vez que se recupera la región de escritura previamente afectada, se convierte en disponible automáticamente como una región de lectura. Se puede invocar una conmutación por error manual y configurar la región afectada como la región de escritura. Puede hacer una conmutación por error manual mediante la [CLI de Azure o Azure Portal](how-to-manage-database-account.md#manual-failover). **No se produce ninguna pérdida de datos ni de disponibilidad** antes, durante o después de la conmutación por error manual. La aplicación sigue teniendo alta disponibilidad. 

- Cuentas de varias regiones con una región de solo escritura: durante una interrupción de la región de lectura, estas cuentas permanecerán con alta disponibilidad para lecturas y escrituras. La región afectada se desconecta automáticamente de la región de escritura y se marcará como sin conexión. El SDK de Cosmos DB redirigirá las llamadas de lectura a la siguiente región disponible en la lista de regiones preferidas. Si ninguna de las regiones de la lista de las regiones preferidas está disponible, las llamadas se devuelven automáticamente a la región actual de escritura. No es necesario realizar ningún cambio en el código de su aplicación para gestionar la interrupción de la región de lectura. Finalmente, cuando la región afectada se vuelve a conectar, la región de lectura previamente afectada se sincronizará automáticamente con la región de escritura actual y estará disponible de nuevo para atender las solicitudes de lectura. Las siguientes lecturas se redirigen a la región recuperada sin necesidad de realizar cambios en el código de la aplicación. Durante la conmutación por error y cuando se vuelva a unir una región previamente errónea, Cosmos DB seguirá cumpliendo las garantías de coherencia de lectura.

- Las cuentas de una sola región pueden perder disponibilidad después de una interrupción regional. Se recomienda configurar al menos dos regiones (si es posible, al menos dos regiones de escritura) con su cuenta de Cosmos para garantizar alta disponibilidad en todo momento.

- Incluso en un caso extremadamente poco frecuente y desafortunado en que la región de Azure sea irrecuperable de forma permanente, no hay ninguna pérdida potencial de datos si se configura la cuenta de Cosmos de varias regiones con el nivel de coherencia predeterminado, que es seguro. En el caso de una región de escritura irrecuperable de forma permanente, si las cuentas de Cosmos de varias regiones están configuradas con coherencia de obsolescencia limitada, la ventana de pérdida potencial de datos está restringida a la ventana de obsolescencia; para los niveles de coherencia de sesión, prefijo coherente y posible, la ventana de pérdida potencial de datos está restringida a cinco segundos como máximo.

## <a name="building-highly-available-applications"></a>Compilación de aplicaciones de alta disponibilidad

- Para garantizar la disponibilidad de escritura y lectura, configure la cuenta de Cosmos para abarcar al menos dos regiones con varias regiones de escritura. Esta configuración proporciona disponibilidad, latencia más baja y escalabilidad para lecturas y escrituras respaldadas por SLA. Para obtener más información, vea cómo [configurar la cuenta de Cosmos con varias regiones de escritura](tutorial-global-distribution-sql-api.md). Para configurar la arquitectura multimaestro en las aplicaciones, vea [How to configure multi-master](how-to-multi-master.md) (Configuración de una arquitectura multimaestro).

- Para cuentas de Cosmos de varias regiones que estén configuradas con una región única de escritura, [habilite la conmutación por error automática mediante la CLI de Azure o Azure Portal](how-to-manage-database-account.md#automatic-failover). Después de habilitar la conmutación automática por error, siempre que se produzca un desastre regional, Cosmos DB conmutarán por error automáticamente su cuenta.  

- Incluso si su cuenta de Cosmos es de alta disponibilidad, es posible que la aplicación no se haya diseñado correctamente para seguir teniendo alta disponibilidad. Para probar la alta disponibilidad de un extremo a otro de la aplicación, invoque periódicamente la [conmutación por error manual mediante la CLI de Azure o Azure Portal](how-to-manage-database-account.md#manual-failover) como parte de sus pruebas de aplicaciones o de las maniobras de recuperación ante desastres (DR).


A medida que desarrolle el plan de continuidad empresarial, tendrá que saber el tiempo máximo aceptable para que la aplicación se recupere por completo tras un evento de interrupción. El tiempo necesario para que una aplicación se recupere totalmente se conoce como el objetivo de tiempo de recuperación (RTO). También debe conocer el período máximo de actualizaciones de datos recientes que la aplicación puede tolerar perder al recuperarse después de un evento de interrupción. El período de tiempo de las actualizaciones que se puede permitir perder se conoce como objetivo de punto de recuperación (RPO).

En la tabla siguiente se muestra el RPO y el RTO para los escenarios más comunes.

|Número de regiones |Configuración |Nivel de coherencia|RPO |RTO |
|---------|---------|---------|-------|-------|
|1    | *    |*   | < 240 minutos | < 1 semana |
|>1     | Replicación de un solo maestro | Sesión, prefijo coherente, eventual | < 15 minutos | < 15 minutos |
|>1     | Replicación de un solo maestro | De obsolescencia entrelazada | K & T | < 15 minutos |
|>1     | Replicación de varios maestros | Sesión, prefijo coherente, eventual | < 15 minutos | 0 |
|>1     | Replicación de varios maestros | De obsolescencia entrelazada | K & T | 0 |
|>1     | * | Alta | 0 | < 15 minutos |

## <a name="next-steps"></a>Pasos siguientes

En el artículo siguiente, obtendrá información acerca de cómo escalar el rendimiento:

* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Escalado del rendimiento aprovisionado globalmente](scaling-throughput.md)
* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)
* [Configuración de una arquitectura multimaestro en las aplicaciones](how-to-multi-master.md)
