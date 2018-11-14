---
title: Alta disponibilidad en Azure Cosmos DB
description: En este artículo se describe cómo Azure Cosmos DB ofrece una alta disponibilidad
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 5dc43aa5b98f097bd8bdf927f40b2d3efc878d48
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283502"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Alta disponibilidad en Azure Cosmos DB

Azure Cosmos DB replica con transparencia los datos en todas las regiones de Azure asociadas con la cuenta de Cosmos. Cosmos DB emplea diferentes capas de redundancia para los datos, tal como se muestra en la imagen siguiente:

![Partición de los recursos](./media/high-availability/figure1.png) 

- Los datos en los contenedores de Cosmos se dividen horizontalmente.

- En cada una de las regiones, cada partición está protegida por un conjunto de réplicas con todas las escrituras replicadas y confirmadas de forma duradera por la mayoría de las réplicas. Las réplicas se distribuyen entre unos 10 o 20 dominios de error como máximo.

- Se replica cada partición de todas las regiones. Cada región contiene todas las particiones de datos de un contenedor de Cosmos y puede aceptar operaciones de escritura y servir lecturas.  

Si la cuenta de Cosmos se distribuye entre N regiones de Azure, habrá al menos N x 4 copias de todos los datos. Además de proporcionar acceso de baja latencia a los datos y escalar el rendimiento de lectura y escritura en todas las regiones asociadas con la cuenta de Cosmos, tener más regiones (N superior) también mejora la disponibilidad.  

## <a name="slas-for-availability"></a>SLA para disponibilidad   

Como base de datos distribuida globalmente, Cosmos DB proporciona contratos de nivel de servicio completos que abarcan rendimiento, latencia en el percentil 99, coherencia y alta disponibilidad. En la tabla siguiente, se describen las garantías que pertenecen a la alta disponibilidad que proporciona Cosmos DB para cuentas únicas y de varias regiones. Para lograr alta disponibilidad, configure las cuentas de Cosmos para que tengan varias regiones de escritura.

|Tipo de operación  | Región única |Varias regiones (escrituras de región única)|Varias regiones (escrituras de varias regiones) |
|---------|---------|---------|-------|
|Escrituras    | 99,99    |99,99   |99,999|
|Lecturas     | 99,99    |99,999  |99,999|

> [!NOTE]
> En la práctica, la disponibilidad de escritura real para modelos de posible coherencia, prefijo coherente, sesión y obsolescencia limitada son significativamente mayores que los SLA publicados. La disponibilidad de lectura real para todos los niveles de coherencia es significativamente mayor que los SLA publicados.

## <a name="regional-outages"></a>Interrupciones regionales

Las interrupciones regionales son algo habitual, y Azure Cosmos DB garantiza que su base de datos estará siempre disponible. Los siguientes detalles capturan el comportamiento de Cosmos DB durante una interrupción, en función de la configuración de la cuenta de Cosmos: 

- Las cuentas de varias regiones configuradas con varias regiones de escritura seguirán estando altamente disponibles para las escrituras y lecturas. Las conmutaciones por error regionales son instantáneas y no requieren cambios de la aplicación.

- Cuentas de varias regiones con una región de escritura única: durante una interrupción de región de escritura, estas cuentas seguirán estando altamente disponibles para las lecturas. Sin embargo, para operaciones de escritura debe "habilitar la conmutación automática por error" en la cuenta de Cosmos para conmutar por error la región afectada a otra región asociada. La conmutación por error se producirá en el orden de prioridad de regiones que especificó. Finalmente, cuando la región afectada vuelva a estar en línea, los datos no replicados presentes en la región de escritura afectada durante la interrupción estarán disponibles a través de la fuente de conflictos. Las aplicaciones pueden leer la fuente de conflictos, resolver los conflictos de acuerdo con la lógica específica de la aplicación y escribir los datos actualizados de nuevo en el contenedor de Cosmos según corresponda. Una vez que se recupera la región de escritura previamente afectada, se convierte en disponible automáticamente como una región de lectura. Se puede invocar una conmutación por error manual y devolver la región afectada como región de escritura. Puede hacer una conmutación por error manual mediante la [CLI de Azure o Azure Portal](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account).  

- Cuentas de varias regiones con una región de escritura única: durante una interrupción de región de lectura, estas cuentas seguirán estando altamente disponibles para las lecturas y escrituras. La región afectada se desconecta automáticamente de la región de escritura y se marcará como sin conexión. El SDK de Cosmos DB redirigirá las llamadas de lectura a la siguiente región disponible en la lista de regiones preferidas. Si ninguna de las regiones de la lista de las regiones preferidas está disponible, las llamadas se devuelven automáticamente a la región actual de escritura. No es necesario realizar ningún cambio en el código de su aplicación para gestionar la interrupción de la región de lectura. Finalmente, cuando la región afectada se vuelve a conectar, la región de lectura previamente afectada se sincronizará automáticamente con la región de escritura actual y estará disponible de nuevo para atender las solicitudes de lectura. Las siguientes lecturas se redirigen a la región recuperada sin necesidad de realizar cambios en el código de la aplicación. Durante la conmutación por error y cuando se vuelva a unir una región previamente errónea, Cosmos DB seguirá cumpliendo las garantías de coherencia de lectura.

- Las cuentas de una sola región pueden perder la disponibilidad si se produce una interrupción regional. Se recomienda configurar al menos dos regiones (si es posible, al menos dos regiones de escritura) con su cuenta de Cosmos para garantizar una alta disponibilidad en todo momento.

## <a name="building-highly-available-applications"></a>Compilación de aplicaciones de alta disponibilidad

- Para garantizar la disponibilidad de escritura y lectura, configure la cuenta de Cosmos para abarcar al menos dos regiones con varias regiones de escritura. Esta configuración asegurará la mejor disponibilidad, la latencia más baja y escalabilidad para lecturas y escrituras respaldadas por SLA. Para obtener más información, vea cómo [configurar la cuenta de Cosmos con varias regiones de escritura](tutorial-global-distribution-sql-api.md).

- Para cuentas de Cosmos de varias regiones que estén configuradas con una región única de escritura, [habilite la conmutación por error automática mediante la CLI de Azure o Azure Portal](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account). Después de habilitar la conmutación automática por error, siempre que se produzca un desastre regional, Cosmos DB conmutarán por error automáticamente su cuenta.  

- Incluso si su cuenta de Cosmos es de alta disponibilidad, es posible que la aplicación no se haya diseñado correctamente para seguir teniendo alta disponibilidad. Para garantizar alta disponibilidad de un extremo a otro de la aplicación, invoque periódicamente la [conmutación por error manual mediante el uso de la CLI de Azure o Azure Portal](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account) como parte de sus pruebas de aplicaciones o de las maniobras de recuperación ante desastres (DR). 

## <a name="next-steps"></a>Pasos siguientes

En el artículo siguiente, obtendrá información acerca de cómo escalar el rendimiento:

* [Escalado del rendimiento](scaling-throughput.md)

* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)

* [Scaling provisioned throughput globally](scaling-throughput.md) (Escalado del rendimiento aprovisionado globalmente)

* [Distribución global en segundo plano](global-dist-under-the-hood.md)

* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)


