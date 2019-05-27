---
title: Optimización del costo de lecturas y escrituras en Azure Cosmos DB
description: En este artículo se explica cómo reducir los costos de Azure Cosmos DB al realizar las operaciones de lectura y escritura en los datos.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: 13ce5ee8b0e2a5d9cc84ea1a408ebba152b46050
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967403"
---
# <a name="optimize-reads-and-writes-cost-in-azure-cosmos-db"></a>Optimización del costo de lecturas y escrituras en Azure Cosmos DB

En este artículo se describe cómo se calcula el costo necesario para leer y escribir datos desde Azure Cosmos DB. Las operaciones de lectura incluyen las operaciones Get en los elementos, y las operaciones de escritura incluyen insertar, reemplazar, eliminar y upsert elementos.  

## <a name="cost-of-reads-and-writes"></a>Costo de lecturas y escrituras

Azure Cosmos DB garantiza un rendimiento predecible en cuanto a rendimiento y latencia con un modelo de rendimiento aprovisionado. El rendimiento aprovisionado se representa en términos de [unidades de solicitud](request-units.md) por segundo o RU/s. Una unidad de solicitud (RU) es una abstracción lógica a través de los recursos de proceso, como CPU, memoria, E/S, etc., que son necesarios para realizar una solicitud. El rendimiento aprovisionado (RU) se reserva y está dedicado a su contenedor o base de datos para ofrecer un rendimiento y latencia predecibles. El rendimiento aprovisionado permite a Azure Cosmos DB ofrecer un rendimiento coherente y predecible, baja latencia garantizada y alta disponibilidad a cualquier escala. Las unidades de solicitud representan la moneda normalizada que simplifica el razonamiento sobre cuántos recursos necesita una aplicación. 

No tiene que pensar en diferenciar unidades de solicitud entre lecturas y escrituras. El modelo de moneda unificado de unidades de solicitud crea eficiencias para usar indistintamente la misma capacidad de rendimiento para lecturas y escrituras. En la tabla siguiente se muestra el costo de lecturas y escrituras en términos de RU/s para los elementos con un tamaño de 1 KB y 100 KB.

|**Tamaño del elemento**  |**Costo de una lectura** |**Costo de una escritura**|
|---------|---------|---------|
|1 KB |1 RU |5 RU |
|100 KB |10 RU |50 RU |

Leer un elemento que tiene un tamaño de 1 KB cuesta 1 RU. Escribir un elemento de 1 KB cuesta 5 RU. Los costos de lectura y escritura corresponden cuando se usa el [nivel de coherencia](consistency-levels.md) de la sesión predeterminada.  Las consideraciones acerca de las RU incluyen: el tamaño del elemento, el recuento de propiedades, la coherencia de los datos, las propiedades indexadas, la indexación y los patrones de consulta.

## <a name="normalized-cost-for-1-million-reads-and-writes"></a>Costo normalizado de 1 millón de lecturas y escrituras

Si aprovisiona 1000 RU/s, se traduce en 3,6 millones de RU/hora y costará 0,08 USD la hora (en Estados Unidos y Europa). Para un elemento de 1 KB, puede realizar 3,6 millones de lecturas o 0,72 millones de escrituras (este valor se calcula como: `3.6 million RU / 5`) por hora con este rendimiento aprovisionado. Normalizado a un millón de lecturas y escrituras, el costo sería de 0,022 USD para 1 millón de lecturas (este valor se calcula como: 0,08 USD/3,6 millones) y 0,111 USD para 1 millón de escrituras (este valor se calcula como: 0,08 USD/0,72 millones).

## <a name="number-of-regions-and-the-request-units-cost"></a>Número de regiones y costo de las unidades de solicitud

El costo de las escrituras es constante, independientemente del número de regiones asociadas con la cuenta de Azure Cosmos. En otras palabras, una escritura de 1 KB costará 5 RU independientemente del número de regiones asociadas con la cuenta. Hay una cantidad no trivial de recursos dedicados a la replicación, la aceptación y el procesamiento del tráfico de replicación en todas las regiones. Para más información sobre la optimización de los costos en varias regiones, consulte el artículo [Optimizing the cost of multi-region Cosmos accounts](optimize-cost-regions.md) (Optimización del costo para cuentas de Cosmos de varias regiones).

## <a name="optimize-the-cost-of-writes-and-reads"></a>Optimización del costo de lecturas y escrituras

Al realizar operaciones de escritura, debe aprovisionar capacidad suficiente para admitir el número de escrituras necesarias por segundo. Puede aumentar el rendimiento aprovisionado mediante el SDK, el portal, la CLI antes de realizar las escrituras y, luego, reducir el rendimiento una vez completadas las escrituras. El rendimiento durante el período de escritura es el rendimiento mínimo necesario para los datos especificados, además del rendimiento necesario para la carga de trabajo de insert, suponiendo que no hay otras cargas de trabajo en ejecución. 

Si se ejecutan otras cargas de trabajo simultáneamente, por ejemplo, consultar/leer/actualizar/eliminar, también debe agregar las unidades de solicitud adicionales necesarias para esas operaciones. Si las operaciones de escritura tienen una velocidad limitada, puede personalizar la directiva de retroceso/reintento mediante los SDK de Azure Cosmos DB. Por ejemplo, puede aumentar la carga hasta que una pequeña tasa de solicitudes obtenga una velocidad limitada. Si se produce el límite de velocidad, la aplicación cliente debe retroceder en las solicitudes con limitación de velocidad para el intervalo de reintento especificado. Antes de reintentar las escrituras, debe tener una cantidad mínima de tiempo entre reintentos. Se incluye compatibilidad con la directiva de reintentos en los SDK de SQL. NET, Java, Node.js y Python, y todas las versiones compatibles de los SDK de .NET Core. 

También puede insertar masivamente datos en Azure Cosmos DB o copiar datos desde cualquier almacén de datos de origen compatible a Azure Cosmos DB mediante [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md). Data Factory se integra de manera nativa con la API Bulk de Azure Cosmos DB para proporcionar el mejor rendimiento de escritura de datos.

## <a name="next-steps"></a>Pasos siguientes

A continuación, puede seguir obteniendo más información sobre la optimización de costos en Azure Cosmos DB con los siguientes artículos:

* Sobre la [optimización para desarrollo y pruebas](optimize-dev-test.md)
* Obtenga más información sobre [la factura de Azure Cosmos DB](understand-your-bill.md).
* Obtenga más información sobre la [optimización del costo de la capacidad del rendimiento](optimize-cost-throughput.md).
* Más información sobre la [optimización del costo de almacenamiento](optimize-cost-storage.md)
* Más información sobre la [optimización del costo de consulta](optimize-cost-queries.md)
* Obtenga más información sobre la [optimización del costo de las cuentas de Azure Cosmos de varias regiones](optimize-cost-regions.md).
