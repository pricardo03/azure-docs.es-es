---
title: Alta disponibilidad en Azure Cosmos DB
description: En este artículo se describe cómo Azure Cosmos DB ofrece una alta disponibilidad
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 74eee3d164e7ee3831f292568da9cf0620e576e5
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399283"
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

- **Cuentas de varias regiones con una región de solo escritura (interrupción de la región de escritura):** durante una interrupción de la región de escritura, estas cuentas permanecerán con alta disponibilidad para las lecturas. Sin embargo, para operaciones de escritura debe **"Habilitar la conmutación automática por error"** en su Cosmos cuenta para la conmutación por error de la región afectada en otra región. La conmutación por error se producirá en el orden de prioridad de regiones que especificó. Cuando la región afectada se vuelve a conectar, los datos sin duplicar presentes en la región de escritura afectado durante la interrupción está disponible a través de la [conflictos fuente](how-to-manage-conflicts.md#read-from-conflict-feed). Las aplicaciones pueden leer los conflictos de fuente, resuelva los conflictos de acuerdo con la lógica específica de la aplicación y la escritura diferida de los datos actualizados en el contenedor de Cosmos según corresponda. Una vez que se recupera la región de escritura previamente afectada, se convierte en disponible automáticamente como una región de lectura. Se puede invocar una conmutación por error manual y configurar la región afectada como la región de escritura. Vuelva a hacer una conmutación por error manual mediante el uso de [CLI de Azure o Azure portal](how-to-manage-database-account.md#manual-failover). **No se produce ninguna pérdida de datos ni de disponibilidad** antes, durante o después de la conmutación por error manual. La aplicación sigue teniendo alta disponibilidad. 

- **Cuentas de varias regiones con una región de solo escritura (interrupción de la región de lectura):** durante una interrupción de la región de lectura, estas cuentas permanecerán con alta disponibilidad para lecturas y escrituras. La región afectada se desconecta automáticamente de la región de escritura y se marcará como sin conexión. El [SDK de Cosmos DB](sql-api-sdk-dotnet.md) leerá redirigir las llamadas a la siguiente región disponible en la lista de regiones preferidas. Si ninguna de las regiones de la lista de las regiones preferidas está disponible, las llamadas se devuelven automáticamente a la región actual de escritura. No es necesario realizar ningún cambio en el código de su aplicación para gestionar la interrupción de la región de lectura. Finalmente, cuando la región afectada se vuelve a conectar, la región de lectura previamente afectada se sincronizará automáticamente con la región de escritura actual y estará disponible de nuevo para atender las solicitudes de lectura. Las siguientes lecturas se redirigen a la región recuperada sin necesidad de realizar cambios en el código de la aplicación. Durante la conmutación por error y nueva combinación de una región errónea anterior, sigue Cosmos DB cumpliendo su garantía de coherencia de lectura.

- Las cuentas de una sola región pueden perder disponibilidad después de una interrupción regional. Siempre se recomienda configurar **al menos dos regiones** (si es posible, al menos dos escribir regiones) con su cuenta de Cosmos para garantizar una alta disponibilidad en todo momento.

- Incluso en un evento poco frecuente y desafortunado cuando la región de Azure es irrecuperable permanentemente, no hay ninguna pérdida de datos si se configura la cuenta de Cosmos de varias regiones con el nivel de coherencia predeterminado de *seguro*. En el caso de una región de escritura irrecuperable permanentemente, para las cuentas de Cosmos de varias regiones configuradas con la coherencia de obsolescencia limitada, la ventana de pérdida potencial de datos está restringida a la ventana de obsolescencia (*K* o *T*); sesión, los niveles de coherencia de prefijo coherente y posible, la ventana de pérdida potencial de datos está restringida a un máximo de cinco segundos. 

## <a name="availability-zone-support"></a>Compatibilidad con la zona de disponibilidad

Azure Cosmos DB es un servicio de base de datos distribuida globalmente y varios maestros que proporciona alta disponibilidad y resistencia durante las interrupciones regionales. Además para cruzar la resistencia de la región, ahora puede habilitar **redundancia de zona** al seleccionar una región para asociar a la base de datos de Azure Cosmos. 

Con el soporte técnico de la zona de disponibilidad, Azure Cosmos DB garantizará réplicas se colocan en varias zonas dentro de una región determinada para proporcionar alta disponibilidad y resistencia durante errores zonales. No hay ningún cambio a la latencia y los otros SLA en esta configuración. En el caso de error de una sola zona, redundancia de zona proporciona durabilidad total de los datos con el RPO = 0 y la disponibilidad con RTO = 0. 

Redundancia de zona es una *funcionalidad complementaria* a la [réplica con varios maestros](how-to-multi-master.md) característica. Redundancia de zona por sí solo no es confiable para lograr resistencia regional. Por ejemplo, si se produce una interrupción regional o acceso de baja latencia en las regiones, se recomienda para tener varias regiones de escritura, además de redundancia de zona. 

Al configurar las escrituras en varias regiones para la cuenta de Azure Cosmos, se puede optar por redundancia de zona sin ningún costo adicional. En caso contrario, consulte la nota siguiente sobre los precios de soporte técnico de redundancia de zona. Puede habilitar la redundancia de zona en una región existente de la cuenta de Azure Cosmos quitando la región y agregarlo de nuevo con la redundancia de zona habilitada.

Esta característica está disponible en las siguientes regiones de Azure:

* Sur de Reino Unido 2
* Sudeste asiático 

> [!NOTE] 
> Habilitación de las zonas de disponibilidad de una sola región de cuenta de Azure Cosmos dará como resultado los gastos que son equivalentes a la adición de una región adicional a su cuenta. Para obtener más información sobre los precios, consulte el [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/) y [costo de varias regiones de Azure Cosmos DB](optimize-cost-regions.md) artículos. 

En la tabla siguiente se resume la funcionalidad de alta disponibilidad de diversas configuraciones de cuenta: 

|KPI  |Sola región sin zonas de disponibilidad (no-AZ)  |Única región con zonas de disponibilidad (AZ)  |Varias regiones con zonas de disponibilidad (AZ 2 regiones): valor recomendado más |
|---------|---------|---------|---------|
|Escribir el SLA de disponibilidad     |   99,99%      |    99,99%     |  99,999 %  |
|SLA de disponibilidad de lectura   |   99,99%      |   99,99%      |  99,999 %       |
|Precio  |  Tasa de facturación de una sola región |  Tasa de facturación de zona de disponibilidad de una sola región |  Tasa de facturación de varias regiones       |
|Errores de zona: pérdida de datos   |  Pérdida de datos  |   Sin pérdida de datos |   Sin pérdida de datos  |
|Errores de zona: disponibilidad |  Pérdida de disponibilidad  | Sin pérdida de disponibilidad  |  Sin pérdida de disponibilidad  |
|Latencia de lectura    |  Entre regiones    |   Entre regiones   |    Bajo  |
|Latencia de escritura    |   Entre regiones   |  Entre regiones    |   Bajo   |
|Interrupción regional: pérdida de datos    |   Pérdida de datos      |  Pérdida de datos       |   Pérdida de datos <br/><br/> Cuando uso limitado la coherencia de obsolescencia limitada con multi master y más de una región, pérdida de datos se limita a la obsolescencia limitada configurado en su cuenta. <br/><br/> Pérdida de datos durante una interrupción regional puede evitarse mediante la configuración de coherencia con varias regiones. Esta opción conlleva ciertos inconvenientes que afectan al rendimiento y disponibilidad.      |
|Interrupción regional: disponibilidad  |  Pérdida de disponibilidad       |  Pérdida de disponibilidad       |  Sin pérdida de disponibilidad  |
|Throughput    |  Rendimiento aprovisionado de X RU/s      |  Rendimiento aprovisionado de X RU/s       |  2 x rendimiento aprovisionado de RU/s <br/><br/> Este modo de configuración requiere dos veces la cantidad de rendimiento en comparación con una sola región con zonas de disponibilidad porque hay dos regiones.   |

Puede habilitar la redundancia de zona al agregar una región a las cuentas de Cosmos Azure nuevas o existentes. Actualmente, solo puede habilitar la redundancia de zona mediante el uso de plantillas de Azure Resource Manager o PowerShell. Para habilitar la redundancia de zona en su cuenta de Azure Cosmos, debe establecer el `isZoneRedundant` marca `true` para una ubicación específica. Puede establecer esta marca dentro de la propiedad de las ubicaciones. Por ejemplo, el siguiente fragmento de código de powershell permite la redundancia de zona para la región "Sudeste asiático":

```powershell
$locations = @( 
    @{ "locationName"="Southeast Asia"; "failoverPriority"=0; "isZoneRedundant"= "true" }, 
    @{ "locationName"="East US"; "failoverPriority"=1 } 
) 
```

## <a name="building-highly-available-applications"></a>Compilación de aplicaciones de alta disponibilidad

- Para garantizar la disponibilidad de escritura y lectura, configure la cuenta de Cosmos para abarcar al menos dos regiones con varias regiones de escritura. Esta configuración ofrecerá la máxima disponibilidad, latencia más baja y mejor escalabilidad tanto para lecturas y escrituras están respaldado por los SLA. Para obtener más información, vea cómo [configurar la cuenta de Cosmos con varias regiones de escritura](tutorial-global-distribution-sql-api.md).

- Para cuentas de Cosmos de varias regiones que estén configuradas con una región única de escritura, [habilite la conmutación por error automática mediante la CLI de Azure o Azure Portal](how-to-manage-database-account.md#automatic-failover). Después de habilitar la conmutación automática por error, siempre que se produzca un desastre regional, Cosmos DB conmutarán por error automáticamente su cuenta.  

- Incluso si su cuenta de Cosmos es de alta disponibilidad, es posible que la aplicación no se haya diseñado correctamente para seguir teniendo alta disponibilidad. Para probar la disponibilidad alta-to-end de la aplicación, invoca periódicamente la [conmutación por error manual mediante la CLI de Azure o Azure portal](how-to-manage-database-account.md#manual-failover), como parte de sus pruebas de aplicaciones o la recuperación ante desastres (DR) aumenta.

- En un entorno de base de datos distribuida globalmente, hay una relación directa entre la durabilidad de datos y el nivel de coherencia en presencia de una interrupción de toda la región. A medida que desarrolle el plan de continuidad empresarial, tendrá que saber el tiempo máximo aceptable para que la aplicación se recupere por completo tras un evento de interrupción. El tiempo necesario para que una aplicación se recupere totalmente se conoce como "objetivo de tiempo de recuperación (RTO)". También debe conocer el período máximo de actualizaciones de datos recientes que la aplicación puede tolerar perder al recuperarse después de un evento de interrupción. El período de tiempo de las actualizaciones que se puede permitir perder se conoce como objetivo de punto de recuperación (RPO). Para ver el RPO y el RTO de Azure Cosmos DB, consulte [Durabilidad de datos y los niveles de coherencia](consistency-levels-tradeoffs.md#rto).

## <a name="next-steps"></a>Pasos siguientes

Ahora puede pasar a la lectura de los artículos siguientes:

* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Escalado del rendimiento aprovisionado globalmente](scaling-throughput.md)
* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)
* [Cómo configurar la cuenta de Cosmos con varias regiones de escritura](how-to-multi-master.md)
