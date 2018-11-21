---
title: Rendimiento y unidades de solicitud en Azure Cosmos DB
description: Obtenga información sobre cómo especificar y estimar los requisitos de las unidades de solicitud en Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: rimman
ms.openlocfilehash: 709cd16c7eee30dd2d88ea87b5f704ad20530ffb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621350"
---
# <a name="request-units-in-azure-cosmos-db"></a>Unidades de solicitud en Azure Cosmos DB

Con Azure Cosmos DB, paga por el rendimiento que aprovisiona y el almacenamiento que consume cada hora. Se debe aprovisionar rendimiento para garantizar que haya disponible suficientes recursos del sistema para la base de datos de Cosmos en todo momento, a fin de alcanzar o superar el Acuerdo de Nivel de Servicio de Cosmos DB.

Cosmos DB admite una variedad de API (SQL, MongoDB, Cassandra, Gremlin y Table). Cada API tiene su propio conjunto de operaciones de base de datos, que abarcan desde sencillas lecturas y escrituras de punto hasta consultas complejas. Cada operación de base de datos consume recursos del sistema en función de la complejidad de la operación.  El costo de todas las operaciones de base de datos lo normaliza Cosmos DB y se expresa en términos de unidades de solicitud (RU). El costo para leer un elemento de 1 KB es una unidad de solicitud (1 RU). A todas las demás operaciones de base de datos se les asigna de forma similar un costo en términos de unidades de solicitud. Independientemente de la API que se usa para interactuar con el contenedor de Cosmos y la operación de base de datos (escritura, lectura o consulta), los costos siempre se miden en términos de unidades de solicitud.

RU/s se puede considerar como la moneda del rendimiento. RU/s es una divisa basada en la velocidad, que abstrae los recursos del sistema, como CPU, IOPS y memoria, necesarios para realizar las operaciones de base de datos compatibles con Cosmos DB. En la imagen siguiente se muestran las unidades de solicitud consumidas por distintas operaciones de base de datos:

![Unidades de solicitud consumidas por operaciones de base de datos](./media/request-units/request-units.png)

Para administrar y planear la capacidad, Cosmos DB garantiza que el número de RU para una operación de base de datos determinada en un conjunto de datos concreto sea determinista. Puede realizar un seguimiento del número de RU consumidas por cualquier operación de base de datos examinando el encabezado de respuesta. Una vez comprendidos los factores que afectan a los cargos de unidad de solicitud y los requisitos de rendimiento de la aplicación, se puede ejecutar la aplicación de forma rentable.

Aunque se factura por hora, el aprovisionamiento del número de RU para la aplicación se realiza por segundos en incrementos de 100 RU/s. Para escalar el rendimiento aprovisionado para la aplicación, puede aumentar o disminuir el número de unidades de solicitud (en incrementos o decrementos de 100 RU) en cualquier momento, ya sea mediante programación o en Azure Portal.

Puede aprovisionar el rendimiento en dos granularidades diferentes: 

* **Contenedores**. Para más información, vea [cómo aprovisionar rendimiento en un contenedor de Cosmos](how-to-provision-container-throughput.md).
* **Bases de datos**. Para más información, vea [cómo aprovisionar rendimiento en una base de datos de Cosmos](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>Consideraciones de la unidad de solicitud

Al calcular el número de RU/s para aprovisionar, es importante tener en cuenta los siguientes factores:

* **Tamaño del elemento**: a medida que el tamaño de un elemento aumenta, el número de unidades de solicitud consumidas para leer o escribir el elemento también aumenta.

* **Indexación de documentos**: de forma predeterminada, cada elemento se indexa automáticamente. Se consumen menos unidades de solicitud si decide no indexar algunos elementos de un contenedor.

* **Recuento de propiedades de elemento**: suponiendo que se aplica la indexación predeterminada en todas las propiedades, el número de RU consumidas para escribir un elemento aumenta a medida que aumenta el recuento de propiedades del elemento.

* **Propiedades indexadas**: una directiva de índice en cada contenedor determina qué propiedades se indexan de forma predeterminada. Puede reducir el consumo de unidades de solicitud para operaciones de escritura limitando el número de las propiedades indexadas.

* **Coherencia de los datos**: los niveles de coherencia alta y de obsolescencia limitada consumen aproximadamente dos veces más unidades de solicitud al realizar operaciones de lectura en comparación con las que consumen los niveles de coherencia relajada.

* **Modelos de consulta**: la complejidad de una consulta afecta a la cantidad de unidades de solicitud consumidas en una operación. El número de resultados de consulta, el número y la naturaleza de los predicados, el número de funciones definidas por el usuario, el tamaño de los datos de origen, el tamaño del conjunto de resultados y las proyecciones afectan al costo de las operaciones de consulta. Cosmos DB garantiza que la misma consulta de los mismos datos costará siempre el mismo número de unidades de solicitud en ejecución repetidas.

* **Uso de scripts**: las consultas, los procedimientos almacenados y los desencadenadores consumen unidades de solicitud según la complejidad de las operaciones que se llevando a cabo. Cuando desarrolle la aplicación, inspeccione el encabezado de cargo de solicitud para entender mejor cuánta capacidad de unidad de solicitud consume cada operación.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre el [aprovisionamiento del rendimiento de contenedores y bases de datos de Cosmos DB](set-throughput.md)
* Más información sobre las [particiones lógicas](partition-data.md)
* Más información sobre el [escalado global del rendimiento aprovisionado](scaling-throughput.md)
* Más información sobre [cómo aprovisionar rendimiento en un contenedor de Cosmos](how-to-provision-container-throughput.md)
* Más información sobre [cómo aprovisionar rendimiento en una base de datos de Cosmos](how-to-provision-database-throughput.md)
