---
title: Alta disponibilidad en Azure Cosmos DB
description: En este artículo se describe cómo Azure Cosmos DB ofrece una alta disponibilidad
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 38629ed2246f4eb67e4183354fe4feaaaee16805
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305443"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Alta disponibilidad con Azure Cosmos DB

Azure Cosmos DB replica con transparencia los datos en todas las regiones de Azure asociadas con la cuenta de Cosmos. Cosmos DB emplea diferentes capas de redundancia para los datos, tal como se muestra en la imagen siguiente:

![Creación de particiones físicas](./media/high-availability/cosmosdb-data-redundancy.png)

- Los datos de los contenedores de Cosmos se [dividen horizontalmente en particiones](partitioning-overview.md).

- En cada una de las regiones, cada partición está protegida por un conjunto de réplicas con todas las escrituras replicadas y confirmadas de forma duradera por la mayoría de las réplicas. Las réplicas se distribuyen entre unos 10 o 20 dominios de error como máximo.

- Se replica cada partición de todas las regiones. Cada región contiene todas las particiones de datos de un contenedor de Cosmos y puede aceptar operaciones de escritura y servir lecturas.  

Si la cuenta de Cosmos se distribuye entre *N* regiones de Azure, habrá al menos *N* x 4 copias de todos los datos. Además de proporcionar acceso de baja latencia a los datos y escalar el rendimiento de lectura y escritura en todas las regiones asociadas con la cuenta de Cosmos, tener más regiones (*N* superior) mejora aún más la disponibilidad.  

## <a name="slas-for-availability"></a>SLA para disponibilidad

Como base de datos distribuida globalmente, Cosmos DB proporciona contratos de nivel de servicio completos que abarcan rendimiento, latencia en el percentil 99, coherencia y alta disponibilidad. En la tabla siguiente se muestran las garantías de alta disponibilidad proporcionadas por Cosmos DB para cuentas de una y varias regiones. Para lograr alta disponibilidad, configure siempre las cuentas de Cosmos para que tengan varias regiones de escritura.

|Tipo de operación  | Región única |Varias regiones (escrituras de región única)|Varias regiones (escrituras de varias regiones) |
|---------|---------|---------|-------|
|Escrituras    | 99,99    |99,99   |99,999|
|Lecturas     | 99,99    |99,999  |99,999|

> [!NOTE]
> En la práctica, la disponibilidad de escritura real para modelos de posible coherencia, prefijo coherente, sesión y obsolescencia limitada es significativamente mayor que los SLA publicados. La disponibilidad de lectura real para todos los niveles de coherencia es significativamente mayor que los SLA publicados.

## <a name="high-availability-with-cosmos-db-in-the-event-of-regional-outages"></a>Alta disponibilidad con Cosmos DB en caso de interrupciones regionales

Las interrupciones regionales son algo habitual y Azure Cosmos DB garantiza que la base de datos siempre tenga alta disponibilidad. Los siguientes detalles capturan el comportamiento de Cosmos DB durante una interrupción, en función de la configuración de la cuenta de Cosmos:

- Con Cosmos DB, antes de que el cliente confirme una operación de escritura, un cuórum de réplicas dentro de la región que acepta las operaciones de escritura confirma permanentemente los datos.

- Las cuentas de varias regiones configuradas con varias regiones de escritura seguirán teniendo una alta disponibilidad para las escrituras y lecturas. Las conmutaciones por error regionales son instantáneas y no requieren cambios de la aplicación.

- **Cuentas de varias regiones con una sola región de escritura (interrupción de la región de escritura):** 
  * durante una interrupción de la región de escritura, estas cuentas permanecerán con alta disponibilidad para las lecturas. En cambio, para las operaciones de escritura debe **habilitar la conmutación automática por error** en la cuenta de Cosmos para conmutar por error la región afectada a otra región. La conmutación por error se producirá en el orden de prioridad de regiones que especificó. 
  * Cuando la región afectada vuelva a estar en línea, los datos no replicados presentes en la región de escritura afectada durante la interrupción estarán disponibles mediante la [fuente de conflictos](how-to-manage-conflicts.md#read-from-conflict-feed). Las aplicaciones pueden leer la fuente de conflictos, resolver los conflictos de acuerdo con la lógica específica de la aplicación y escribir los datos actualizados de nuevo en el contenedor de Cosmos según corresponda. 
  * Una vez que se recupera la región de escritura previamente afectada, se convierte en disponible automáticamente como una región de lectura. Puede volver a la región recuperada como la región de escritura. Puede cambiar las regiones con la [CLI de Azure o Azure Portal](how-to-manage-database-account.md#manual-failover). **No se produce ninguna pérdida de datos ni de disponibilidad** antes, durante o después de la conmutación por error manual. La aplicación sigue teniendo alta disponibilidad. 

- **Cuentas de varias regiones con una sola región de escritura (interrupción de la región de lectura):** 
  * durante una interrupción de la región de lectura, estas cuentas permanecerán con alta disponibilidad para lecturas y escrituras. 
  * La región afectada se desconecta automáticamente de la región de escritura y se marcará como sin conexión. Los [SDK de Azure Cosmos DB](sql-api-sdk-dotnet.md) redirigirán las llamadas de lectura a la siguiente región disponible en la lista de regiones preferidas. 
  * Si ninguna de las regiones de la lista de las regiones preferidas está disponible, las llamadas se devuelven automáticamente a la región actual de escritura. 
  * No es necesario realizar ningún cambio en el código de su aplicación para gestionar la interrupción de la región de lectura. Finalmente, cuando la región afectada se vuelve a conectar, la región de lectura previamente afectada se sincronizará automáticamente con la región de escritura actual y estará disponible de nuevo para atender las solicitudes de lectura. 
  * Las siguientes lecturas se redirigen a la región recuperada sin necesidad de realizar cambios en el código de la aplicación. Durante la conmutación por error y cuando se vuelva a unir una región previamente errónea, Cosmos DB seguirá cumpliendo las garantías de coherencia de lectura.

- Las cuentas de una sola región pueden perder disponibilidad después de una interrupción regional. Se recomienda configurar siempre **al menos dos regiones** (si es posible, al menos dos regiones de escritura) con su cuenta de Cosmos para garantizar alta disponibilidad en todo momento.

- Incluso en un caso poco frecuente y desafortunado en que la región de Azure sea irrecuperable de forma permanente, no hay ninguna pérdida de datos si se configura la cuenta de Cosmos de varias regiones con el nivel de coherencia predeterminado *Seguro*. En el caso de que haya una región de escritura irrecuperable de forma permanente, si las cuentas de Cosmos de varias regiones están configuradas con coherencia de obsolescencia limitada, la ventana de pérdida potencial de datos está restringida a la ventana de obsolescencia (*K* o *T*); para los niveles de coherencia de sesión, prefijo coherente y posible, la ventana de pérdida potencial de datos está restringida a cinco segundos como máximo. 

## <a name="availability-zone-support"></a>Compatibilidad de zonas de disponibilidad

Azure Cosmos DB es un servicio de base de datos de arquitectura multimaestro que se distribuye globalmente y proporciona alta disponibilidad y resistencia durante las interrupciones regionales. Además de la resistencia entre regiones, ahora puede habilitar la **redundancia de zona** al seleccionar una región que asociar a su base de datos de Azure Cosmos. 

Gracias a la compatibilidad de zonas de disponibilidad, Azure Cosmos DB garantizará que las réplicas estén colocadas en varias zonas de una región determinada para proporcionar alta disponibilidad y resistencia durante errores zonales. No hay ningún cambio en la latencia ni en otros SLA en esta configuración. En el caso de que haya un error de una sola zona, la redundancia de zona proporciona durabilidad total de los datos con el RPO=0 y la disponibilidad con RTO=0. 

La redundancia de zona es una *funcionalidad complementaria* a la característica de [replicación de arquitectura multimaestro](how-to-multi-master.md). No se puede confiar en la redundancia de zona por sí sola para lograr la resistencia regional. Por ejemplo, si se producen interrupciones regionales o un acceso de baja latencia en las regiones, se recomienda tener varias regiones de escritura además de la redundancia de zona. 

Al configurar las escrituras en varias regiones para la cuenta de Azure Cosmos, puede optar por recibir la redundancia de zona sin ningún costo adicional. En caso contrario, consulte la nota siguiente sobre los precios de la compatibilidad de la redundancia de zona. Puede habilitar la redundancia de zona en una región existente de su cuenta de Azure Cosmos si quita la región y la vuelve a agregar con la redundancia de zona habilitada.

Esta característica está disponible en las siguientes regiones de Azure:

* Sur de Reino Unido 2
* Sudeste asiático 
* Este de EE. UU
* Este de EE. UU. 2 
* Centro de EE. UU.

> [!NOTE] 
> Si habilita Availability Zones en una cuenta de Azure Cosmos de una sola región, se ocasionarán cargos equivalentes a agregar una región adicional a su cuenta. Para obtener más información sobre los precios, consulte la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/) y los artículos sobre [costos de varias regiones de Azure Cosmos DB](optimize-cost-regions.md). 

En la tabla siguiente se resume la funcionalidad de alta disponibilidad de varias configuraciones de cuenta: 

|KPI  |Región única sin Availability Zones (sin AZ)  |Región única con Availability Zones (AZ)  |Escrituras en varias regiones con Availability Zones (AZ, 2 regiones): configuración más recomendada |
|---------|---------|---------|---------|
|Contrato de Nivel de Servicio de disponibilidad de escritura     |   99,99%      |    99,99%     |  99,999 %  |
|Contrato de Nivel de Servicio de disponibilidad de lectura   |   99,99%      |   99,99%      |  99,999 %       |
|Precio  |  Tarifa de facturación de una sola región |  Tarifa de facturación de zona de disponibilidad de una sola región |  Tarifa de facturación de varias regiones       |
|Errores de zona: pérdida de datos   |  Pérdida de datos  |   Sin pérdida de datos |   Sin pérdida de datos  |
|Errores de zona: disponibilidad |  Pérdida de disponibilidad  | Sin pérdida de disponibilidad  |  Sin pérdida de disponibilidad  |
|Latencia de lectura    |  Entre regiones    |   Entre regiones   |    Bajo  |
|Latencia de escritura    |   Entre regiones   |  Entre regiones    |   Bajo   |
|Interrupción regional: pérdida de datos    |   Pérdida de datos      |  Pérdida de datos       |   Pérdida de datos <br/><br/> Al usar la coherencia de obsolescencia limitada con la arquitectura multimaestro y más de una región, la pérdida de datos se limita a la obsolescencia limitada configurada en su cuenta. <br/><br/> Se puede evitar la pérdida de datos durante una interrupción regional si se configura la coherencia fuerte con varias regiones. Esta opción conlleva ciertos inconvenientes que afectan al rendimiento y la disponibilidad.      |
|Interrupción regional: disponibilidad  |  Pérdida de disponibilidad       |  Pérdida de disponibilidad       |  Sin pérdida de disponibilidad  |
|Throughput    |  X RU/s de rendimiento aprovisionado      |  X RU/s de rendimiento aprovisionado       |  2X RU/s de rendimiento aprovisionado <br/><br/> Este modo de configuración requiere dos veces la cantidad de rendimiento en comparación con una sola región con Availability Zones porque hay dos regiones.   |

> [!NOTE] 
> Para habilitar la compatibilidad de zona de disponibilidad para una cuenta de Azure Cosmos DB de varias regiones, la cuenta debe tener habilitadas las escrituras multimaestro.


Puede habilitar la redundancia de zona al agregar una región a cuentas de Azure Cosmos nuevas o existentes. Actualmente, solo puede habilitar la redundancia de zona mediante Azure Portal, PowerShell y plantillas de Azure Resource Manager. Para habilitar la redundancia de zona en su cuenta de Azure Cosmos, debe establecer la marca `isZoneRedundant` en `true` para una ubicación específica. Puede establecer esta marca en la propiedad de ubicaciones. Por ejemplo, el siguiente fragmento de código de PowerShell habilita la redundancia de zona de la región "Sudeste Asiático":

```powershell
$locations = @( 
    @{ "locationName"="Southeast Asia"; "failoverPriority"=0; "isZoneRedundant"= "true" }, 
    @{ "locationName"="East US"; "failoverPriority"=1 } 
) 
```

Puede habilitar Availability Zones mediante Azure Portal al crear una cuenta de Azure Cosmos. Cuando cree una cuenta, asegúrese de habilitar la **Redundancia geográfica** y las **Escrituras en varias regiones**, y elija una región en la que se admita Availability Zones: 

![Habilitación de Availability Zones mediante Azure Portal](./media/high-availability/enable-availability-zones-using-portal.png) 

## <a name="building-highly-available-applications"></a>Compilación de aplicaciones de alta disponibilidad

- Para garantizar la disponibilidad de escritura y lectura, configure la cuenta de Cosmos para abarcar al menos dos regiones con varias regiones de escritura. Esta configuración proporcionará la disponibilidad más alta, la latencia más baja y la mejor escalabilidad para lecturas y escrituras respaldadas por los SLA. Para obtener más información, vea cómo [configurar la cuenta de Cosmos con varias regiones de escritura](tutorial-global-distribution-sql-api.md).

- Para cuentas de Cosmos de varias regiones que estén configuradas con una región única de escritura, [habilite la conmutación por error automática mediante la CLI de Azure o Azure Portal](how-to-manage-database-account.md#automatic-failover). Después de habilitar la conmutación automática por error, siempre que se produzca un desastre regional, Cosmos DB conmutarán por error automáticamente su cuenta.  

- Incluso si su cuenta de Cosmos es de alta disponibilidad, es posible que la aplicación no se haya diseñado correctamente para seguir teniendo alta disponibilidad. Para probar la alta disponibilidad de un extremo a otro de la aplicación, invoque periódicamente la [conmutación por error manual mediante la CLI de Azure o Azure Portal](how-to-manage-database-account.md#manual-failover) como parte de sus pruebas de aplicaciones o de las maniobras de recuperación ante desastres (DR).

- En un entorno de base de datos distribuida de forma global, existe una relación directa entre el nivel de coherencia y la durabilidad de los datos si se produce una interrupción en toda la región. A medida que desarrolle el plan de continuidad empresarial, tendrá que saber el tiempo máximo aceptable para que la aplicación se recupere por completo tras un evento de interrupción. El tiempo necesario para que una aplicación se recupere totalmente se conoce como "objetivo de tiempo de recuperación (RTO)". También debe conocer el período máximo de actualizaciones de datos recientes que la aplicación puede tolerar perder al recuperarse después de un evento de interrupción. El período de tiempo de las actualizaciones que se puede permitir perder se conoce como objetivo de punto de recuperación (RPO). Para ver el RPO y el RTO de Azure Cosmos DB, consulte [Durabilidad de datos y los niveles de coherencia](consistency-levels-tradeoffs.md#rto).

## <a name="next-steps"></a>Pasos siguientes

Ahora puede pasar a la lectura de los artículos siguientes:

* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Escalado del rendimiento aprovisionado globalmente](scaling-throughput.md)
* [Distribución global en segundo plano](global-dist-under-the-hood.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)
* [Procedimiento para configurar la cuenta de Cosmos con varias regiones de escritura](how-to-multi-master.md)
