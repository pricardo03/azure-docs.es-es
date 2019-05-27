---
title: Rendimiento y unidades de solicitud en Azure Cosmos DB
description: Obtenga información sobre cómo especificar y estimar los requisitos de las unidades de solicitud en Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rimman
ms.openlocfilehash: 9615aebd345c957c8e401581ff94735f39ba73c6
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65953541"
---
# <a name="request-units-in-azure-cosmos-db"></a>Unidades de solicitud en Azure Cosmos DB

Con Azure Cosmos DB, paga por el rendimiento que aprovisiona y el almacenamiento que consume cada hora. Debe aprovisionar el rendimiento para asegurarse de que suficientes recursos del sistema están disponibles para la base de datos de Azure Cosmos en todo momento. Necesita los recursos suficientes para cumplir o superar el [SLA de Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Azure Cosmos DB admite varias API, como SQL, MongoDB, Cassandra, Gremlin y Table. Cada API tiene su propio conjunto de operaciones de base de datos. Estas abarcan desde sencillas lecturas y escrituras de punto hasta consultas complejas. Cada operación de base de datos consume recursos del sistema en función de la complejidad de la operación. 

El costo de todas las operaciones de base de datos se normaliza mediante Azure Cosmos DB y se expresa mediante *unidades de solicitud* (o unidades de solicitud, para abreviar). RU/segundo se puede considerar como la moneda del rendimiento. RU/s es una moneda basada en la velocidad, que abstrae los recursos del sistema, como CPU, IOPS y memoria, necesarios para realizar las operaciones de base de datos compatibles con Azure Cosmos DB. 

El costo para leer un 1 KB es elemento 1 unidad de solicitud (o 1 RU). Todas las demás operaciones de base de datos se asignan de forma similar un costo de RU. Con independencia de qué API utilice para interactuar con el contenedor de Azure Cosmos, los costos siempre se miden por RU. Si la operación de base de datos es una escritura, lectura o consulta, los costos siempre se miden en RU.

La siguiente imagen muestra la idea de alto nivel de unidades de solicitud:

![Unidades de solicitud consumidas por operaciones de base de datos](./media/request-units/request-units.png)

Para administrar y planear la capacidad, Azure Cosmos DB garantiza que el número de RU para una operación de base de datos determinada en un conjunto de datos concreto sea determinista. Puede examinar el encabezado de respuesta para realizar un seguimiento del número de RU consumidos por cualquier operación de base de datos. Cuando comprenda el [factores que afectan a los cargos de RU](request-units.md#request-unit-considerations) y requisitos de rendimiento de la aplicación, puede ejecutar la aplicación de forma rentable.

El aprovisionamiento del número de RU para la aplicación se realiza por segundos en incrementos de 100 RU/segundo. Para escalar el rendimiento aprovisionado para la aplicación, puede aumentar o disminuir el número de RU en cualquier momento. Puede escalar en incrementa o disminuye de 100 RU. Puede realizar los cambios mediante programación o en Azure Portal. Se facturan por hora.

Puede aprovisionar el rendimiento en dos granularidades diferentes: 

* **Contenedores**: Para obtener más información, consulte [la manera de aprovisionar el rendimiento en un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* **Las bases de datos**: Para obtener más información, consulte [la manera de aprovisionar el rendimiento en una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>Consideraciones de la unidad de solicitud

Al calcular el número de RU por segundo para aprovisionar, tenga en cuenta los siguientes factores:

* **Tamaño del elemento**: a medida que el tamaño de un elemento aumenta, el número de unidades de solicitud consumidas para leer o escribir el elemento también aumenta.

* **Indexación del elemento**: De forma predeterminada, cada elemento se indexa automáticamente. Se consumen menos unidades de solicitud si decide no indexar algunos elementos de un contenedor.

* **Recuento de propiedades del elemento**: Suponiendo que la indexación predeterminada en todas las propiedades, el número de RU consumido para escribir que un elemento aumenta a medida que aumenta el recuento de propiedades de elemento.

* **Propiedades indexadas**: Una directiva de índice en cada contenedor determina qué propiedades se indexan de forma predeterminada. Para reducir el consumo de unidades de solicitud para operaciones de escritura, limite el número de las propiedades indexadas.

* **Coherencia de los datos**: los niveles de coherencia alta y de obsolescencia limitada consumen aproximadamente dos veces más unidades de solicitud al realizar operaciones de lectura en comparación con las que consumen los niveles de coherencia relajada.

* **Patrones de consultas**: la complejidad de una consulta afecta a la cantidad de unidades de solicitud consumidas en una operación. Entre los factores que influyen en el costo de las operaciones de consulta están los siguientes: 
    
    - El número de resultados de la consulta
    - El número de predicados
    - La naturaleza de los predicados
    - El número de funciones definidas por el usuario
    - El tamaño del origen de datos
    - El tamaño del conjunto de resultados
    - Proyecciones

  Azure Cosmos DB garantiza que la misma consulta de los mismos datos cuesta siempre el mismo número de unidades de solicitud en ejecución repetidas.

* **Uso de scripts**: como las consultas, los procedimientos almacenados y los desencadenadores consumen unidades de solicitud según la complejidad de las operaciones que lleven a cabo. Al desarrollar la aplicación, inspeccione la [encabezado cargo de solicitud](optimize-cost-queries.md#evaluate-request-unit-charge-for-a-query) para comprender mejor la capacidad de RU que consume cada operación.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre el [aprovisionamiento del rendimiento de contenedores y bases de datos de Azure Cosmos](set-throughput.md).
* Más información sobre las [particiones lógicas](partition-data.md).
* Obtenga más información sobre el [escalado global del rendimiento aprovisionado](scaling-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).
* Obtenga información sobre cómo [encontrar el cargo de unidad de solicitud para una operación](find-request-unit-charge.md).
* Obtenga información sobre cómo [optimizan el costo de rendimiento aprovisionado en Azure Cosmos DB](optimize-cost-throughput.md).
* Obtenga información sobre cómo [optimizar lecturas y escrituras de costos en Azure Cosmos DB](optimize-cost-reads-writes.md).
* Obtenga información sobre cómo [optimizar el costo de la consulta en Azure Cosmos DB](optimize-cost-queries.md).
