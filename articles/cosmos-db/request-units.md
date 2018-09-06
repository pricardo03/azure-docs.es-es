---
title: 'Unidades de solicitud y estimación de rendimiento: Azure Cosmos DB | Microsoft Docs'
description: Obtenga información sobre cómo entender, especificar y estimar los requisitos de la unidad de solicitud en Azure Cosmos DB.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: rimman
ms.openlocfilehash: 66beeb2cc724f75d17a4c155f1cdb888153e8fbf
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286772"
---
# <a name="request-units-in-azure-cosmos-db"></a>Unidades de solicitud en Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) es la base de datos con varios modelos y de distribución global de Microsoft. Con Azure Cosmos DB, no tendrá que alquilar máquinas virtuales, implementar software ni supervisar bases de datos. Los mejores ingenieros de Microsoft usan y supervisan de forma continua Azure Cosmos DB para ofrecer disponibilidad, rendimiento y protección de datos universales. Puede acceder a los datos mediante las API de su elección, como las API de [SQL](documentdb-introduction.md), [MongoDB](mongodb-introduction.md) y [Table](table-introduction.md), y a los grafos mediante la [API de Gremlin](graph-introduction.md). Todas las API se admiten de forma nativa. 

La moneda de Azure Cosmos DB es la *unidad de solicitud (RU)*. Con las unidades de solicitud, no es necesario reservar capacidades de lectura/escritura ni aprovisionar CPU, memoria e IOPS. Azure Cosmos DB admite varias API con distintas operaciones, que van desde lecturas y escrituras sencillas a consultas de grafos complejas. Puesto que no todas las solicitudes son iguales, se les asigna una cantidad regularizada de unidades de solicitud según la cantidad de procesamientos necesario para atender la solicitud. El número de unidades de solicitud para una operación es determinista. Puede realizar el seguimiento del número de unidades de solicitud que se consumen en cualquier operación en Azure Cosmos DB mediante un encabezado de respuesta. 

Para proporcionar un rendimiento predecible, debe reservar el rendimiento en unidades de 100 RU/segundo. Puede [calcular las necesidades de rendimiento](request-units.md#estimating-throughput-needs) mediante el uso la [calculadora de unidades de solicitud](https://www.documentdb.com/capacityplanner) de Azure Cosmos DB.

![Calculadora de rendimiento][5]

Después de leer este artículo, podrá responder a las preguntas siguientes:

* ¿Qué son las unidades de solicitud y los cargos de solicitud en Azure Cosmos DB?
* ¿Cómo se puede especificar la capacidad de unidad de solicitud para un contenedor o un conjunto de contenedores en Azure Cosmos DB?
* ¿Cómo puedo estimar mis necesidades de unidad de solicitud de la aplicación?
* ¿Qué ocurre si supero la capacidad de unidad de solicitud para un contenedor o un conjunto de contenedores en Azure Cosmos DB?

Como Azure Cosmos DB es una base de datos de varios modelos, es importante tener en cuenta que la información de este artículo es aplicable a todos los modelos de datos y API de Azure Cosmos DB. En este artículo se usan términos genéricos como *contenedor* para hacer referencia genérica a una colección o grafo, y *elemento* para hacer referencia genérica a una tabla, documento, nodo o entidad.

## <a name="request-units-and-request-charges"></a>Unidades de solicitud y cargos de solicitud

Azure Cosmos DB ofrece un rendimiento predecible y rápido mediante la reserva de recursos para satisfacer las necesidades de rendimiento de la aplicación. Los patrones de carga y acceso de las aplicaciones cambian con el paso del tiempo. Azure Cosmos DB puede ayudarle a aumentar o reducir fácilmente la cantidad de rendimiento reservado disponible para su aplicación.

Con Azure Cosmos DB, el rendimiento reservado se especifica en términos de procesamiento de unidades de solicitud por segundo. Las unidades de solicitud se pueden considerar como la moneda de rendimiento. Se reserva un número garantizado de unidades de solicitud para que estén disponibles para su aplicación por segundo. Cada operación de Azure Cosmos DB, lo que incluye la escritura de un documento, la realización de una consulta y la actualización de un documento, consume CPU, memoria e IOPS. Es decir, cada operación implica un cargo de solicitud, que se expresa en unidades de solicitud. Cuando se comprenden los factores que afectan a los cargos de unidad de solicitud y los requisitos de rendimiento de la aplicación, se puede ejecutar la aplicación de la manera más rentable posible. 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Aislamiento del rendimiento en las bases de datos distribuidas globalmente

Si replica la base de datos en más de una región, Azure Cosmos DB proporciona aislamiento del rendimiento para garantizar que el uso de las unidades de solicitud en una región no afecta a su uso en otra. Por ejemplo, si escribe datos en una región y lee datos de otra, las unidades de solicitud que se usan para realizar la operación de escritura en la región A no se quitan de las unidades de solicitud que se usa en la operación de lectura de la región B. Las unidades de solicitud no se dividen entre las regiones en las que ha implementado la base de datos. Cada región en la que se replica la base de datos tiene la cantidad total de unidades de solicitud aprovisionadas. Para más información, consulte [Cómo se distribuyen datos globalmente con Azure Cosmos DB](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>Consideraciones de la unidad de solicitud
Al estimar el número de unidades de solicitud que se aprovisionan, es importante tener en cuenta las siguientes variables:

* **Tamaño del elemento**. Cuando aumenta el tamaño, también aumenta el número de unidades de solicitud que se usan para leer o escribir los datos.
* **Recuento de propiedades del elemento**. Suponiendo la indexación predeterminada de todas las propiedades, las unidades consumidas para escribir un documento, nodo o entidad aumentan conforme aumenta el recuento de propiedades.
* **Coherencia de datos**. Al usar los modelos de coherencia de datos, como Alta y Obsolescencia limitada, se consumen unidades de solicitud adicionales para leer elementos.
* **Propiedades indexadas**. Una directiva de índice en cada contenedor determina qué propiedades se indexan de forma predeterminada. Puede reducir el consumo de unidades de solicitud para operaciones de escritura limitando el número de las propiedades indexadas o habilitando la indexación diferida.
* **Indexación de documentos**. De forma predeterminada, cada elemento se indexa automáticamente. Consumirá menos unidades de solicitud si decide no indexar algunos elementos.
* **Patrones de consultas**. La complejidad de una consulta afecta a la cantidad de unidades de solicitud consumidas en una operación. El número de resultados de consulta, el número y la naturaleza de los predicados, el número de funciones definidas por el usuario, el tamaño de los datos de origen y las proyecciones afectan todos al costo de las operaciones de consulta.
* **Uso de script**. Las consultas, procedimientos almacenados y desencadenadores consumen unidades de solicitud según la complejidad de las operaciones que se están llevando a cabo. Cuando desarrolle la aplicación, inspeccione el encabezado de cargo de solicitud para entender mejor cómo cada operación consume capacidad de unidad de solicitud.

## <a name="estimating-throughput-needs"></a>Estimación de necesidades de rendimiento
Una unidad de solicitud es una medida normalizada del costo de procesamiento de solicitudes. Una única unidad de solicitud representa la capacidad de procesamiento necesaria para leer (mediante una vinculación automática o identificador) un solo elemento de 1 KB que consta de 10 valores de propiedad únicos (excluidas las propiedades del sistema). Una solicitud para crear (insertar), reemplazar o eliminar el mismo elemento consume más procesamiento del servicio y, por tanto, necesita más unidades de solicitud. 

> [!NOTE]
> La línea de base de una unidad de solicitud de un elemento de 1 KB corresponde a una operación GET sencilla mediante una vinculación automática o un identificador del elemento.
> 
> 

Por ejemplo, esta es una tabla que muestra el número de unidades de solicitud que se deben aprovisionar para elementos con tres tamaños diferentes (1 KB, 4 KB y 64 KB) y en dos niveles de rendimiento diferentes (500 lecturas/segundo + 100 escrituras/segundo y 500 lecturas/segundo + 500 escrituras/segundo). En este ejemplo, la coherencia de datos se establece en **Sesión**, y la directiva de indexación se establece en **Ninguna**.

| Tamaño del elemento | Lecturas/segundo | Escrituras/segundo | Unidades de solicitud
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) = 1000 RU/s
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) = 3000 RU/s
| 4 KB | 500 | 100 | (500 * 1,3) + (100 * 7) = 1350 RU/s
| 4 KB | 500 | 500 | (500 * 1,3) + (500 * 7) = 4150 RU/s
| 64 KB | 500 | 100 | (500 * 10) + (100 * 48) = 9800 RU/s
| 64 KB | 500 | 500 | (500 * 10) + (500 * 48) = 29 000 RU/s


### <a name="use-the-request-unit-calculator"></a>Uso de la calculadora de unidades de solicitud
Para ayudarle a ajustar las estimaciones de rendimiento, puede usar una [calculadora de unidades de solicitud](https://www.documentdb.com/capacityplanner) basada en web. Con esta calculadora podrá estimar los requisitos de unidades de solicitud para operaciones típicas, como:

* Creaciones (escrituras) de elementos
* Lecturas de elementos
* Eliminaciones de elementos
* Actualizaciones de elementos

La herramienta también permite calcular las necesidades de almacenamiento de datos en función de los elementos de ejemplo que proporcione.

Uso de la herramienta

1. Cargue uno o más elementos representativos (por ejemplo, un documento JSON de ejemplo).
   
    ![Carga de elementos en la calculadora de unidades de solicitud][2]
2. Para calcular los requisitos de almacenamiento de datos, escriba el número total de elementos (como documentos, filas o vértices) que espera almacenar.
3. Escriba el número de operaciones de creación, lectura, actualización y eliminación que necesita (por segundo). Para calcular los cargos en materia de unidad de solicitud de las operaciones de actualización de elementos, cargue una copia del elemento de ejemplo del paso 1 que incluya actualizaciones de campo típicas. Por ejemplo, si las actualizaciones de elementos suelen modificar dos propiedades denominadas *lastLogin* y *userVisits*, copie un elemento de ejemplo, actualice los valores de esas dos propiedades y luego cargue el elemento copiado.
   
    ![Especificar los requisitos de rendimiento en la calculadora de unidades de solicitud][3]
4. Seleccione **Calcular** y, a continuación, examine los resultados.
   
    ![Resultados de la calculadora de unidades de solicitud][4]

> [!NOTE]
> Si dispone de tipos de elementos que varían considerablemente en cuanto a tamaño y número de propiedades indexadas, cargue un ejemplo de cada *tipo* de elemento típico en la herramienta y calcule los resultados.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Uso del encabezado de respuesta de cargo de solicitud de Azure Cosmos DB
Cada respuesta del servicio de Azure Cosmos DB incluye un encabezado personalizado (`x-ms-request-charge`) que contiene las unidades de solicitud consumidas por una solicitud determinada. También puede acceder a este encabezado mediante los SDK de Azure Cosmos DB. En el SDK de .NET, **RequestCharge** es una propiedad del objeto **ResourceResponse**. Para las consultas, el Explorador de datos de Azure Cosmos DB en Azure Portal proporciona la información de carga de solicitud para las consultas ejecutadas. Para aprender a obtener y establecer el rendimiento mediante el uso de distintas API varios modelos consulte el artículo acerca de la [obtención y el establecimiento del rendimiento en Azure Cosmos DB](set-throughput.md).

Un método para calcular la cantidad de rendimiento reservado que necesita la aplicación es registrar los cargos de unidad de solicitud asociados a la ejecución de operaciones típicas, frente a un elemento representativo que usa la aplicación. A continuación, calcule el número de operaciones que crea que se realizarán cada segundo. Asegúrese de medir e incluir también las consultas típicas y el uso de scripts de Azure Cosmos DB.

> [!NOTE]
> Si dispone de tipos de elementos que varían considerablemente en cuanto a tamaño y número de propiedades indexadas, registre el cargo de unidad de solicitud de operación aplicable asociado a cada *tipo* de elemento típico.
> 
> 

Por ejemplo, estos son los pasos que puede seguir:

1. Registre el cargo de la unidad de solicitud de creación (inserción) de un elemento típico. 
2. Registre el cargo de la unidad de solicitud de lectura de un elemento típico.
3. Registre el cargo de la unidad de solicitud de actualización de un elemento típico.
4. Registre el cargo de la unidad de solicitud de consultas de elementos comunes y típicas.
5. Registre el cargo de unidad de solicitud de los scripts personalizados (procedimientos almacenados, desencadenadores y funciones definidas por el usuario) que usa la aplicación.
6. Calcule las unidades de solicitud necesarias según el número estimado de operaciones que se prevé ejecutar cada segundo.

## <a name="a-request-unit-estimate-example"></a>Un ejemplo de estimación de la unidad de solicitud
Tenga en cuenta el siguiente documento, que tiene un tamaño aproximado de 1 KB:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Los documentos se han minimizado en Azure Cosmos DB, por lo que el tamaño calculado del sistema del documento anterior es ligeramente inferior a 1 KB.
> 
> 

En la siguiente tabla se muestran los cargos aproximados de unidad de solicitud de operaciones típicas en este elemento. (En el cargo aproximado de unidad de solicitud se asume que el nivel de coherencia de la cuenta se establece en **Sesión** y que todos los elementos se indexan automáticamente).

| Operación | Cargo de la unidad de solicitud |
| --- | --- |
| Crear elemento |~15 RU |
| Lectura de elemento |~1 RU |
| Consulta de elemento por identificador |~2,5 RU |

En la siguiente tabla se muestran los cargos aproximados de unidad de solicitud para las consultas típicas usadas en la aplicación:

| Consultar | Cargo de la unidad de solicitud | Número de elementos devueltos |
| --- | --- | --- |
| Selección de alimentos por identificador |~2,5 RU |1 |
| Selección de alimentos por fabricante |~7 RU |7 |
| Selección por grupo de alimentos y clasificación por peso |~70 RU |100 |
| Selección de los 10 alimentos más importantes en un grupo de alimentos |~10 RU |10 |

> [!NOTE]
> Los cargos de unidad de solicitud varían según el número de elementos devueltos.
> 
> 

Con esta información, puede hacer una estimación de los requisitos de unidad de solicitud para esta aplicación dado el número de operaciones y consultas que espera por segundo:

| Operación o consulta | Número estimado por segundo | Unidades de solicitud necesarias |
| --- | --- | --- |
| Crear elemento |10 |150 |
| Lectura de elemento |100 |100 |
| Selección de alimentos por fabricante |25 |175 |
| Selección por grupo de alimentos |10 |700 |
| Selección de los 10 principales |15 |150 en total |

En este caso, se espera un requisito de rendimiento medio de 1,275 RU/s. Redondeando hasta los 100 más cercanos, se pueden aprovisionar 1300 RU/s para el contenedor (o conjunto de contenedores) de esta aplicación.

## <a id="RequestRateTooLarge"></a> Superación de los límites de rendimiento reservados en Azure Cosmos DB
El consumo de las unidades de solicitud se evalúa por segundo. En el caso de las aplicaciones que superan la tasa de unidades de solicitud aprovisionadas, la tasa de las solicitudes se limita hasta que cae por debajo del nivel de rendimiento aprovisionado. Cuando una solicitud tiene la tasa limitada, el servidor finaliza la solicitud de manera preventiva con `RequestRateTooLargeException` (código de estado HTTP 429) y devuelve el encabezado `x-ms-retry-after-ms`. El encabezado indica la cantidad de tiempo, en milisegundos, que el usuario debe esperar antes de reintentar la solicitud.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Si usa las consultas de LINQ y del SDK de cliente de .NET la mayor parte del tiempo, nunca se las tendrá que ver con esta excepción. La versión actual del SDK de cliente de .NET captura implícitamente esta respuesta, respeta el encabezado retry-after especificado por el servidor y reintenta automáticamente la solicitud. A menos que varios clientes obtengan acceso a la cuenta al mismo tiempo, el siguiente reintento se realizará correctamente.

Si tiene más de un cliente que funciona de manera acumulativa por encima de la tasa de solicitud, el comportamiento de reintento predeterminado puede no ser suficiente y el cliente producirá una excepción `DocumentClientException` con el código de estado 429 en la aplicación. En casos como este, es posible que desee controlar el comportamiento y la lógica de reintento en las rutinas de control de errores de la aplicación, o bien aumentar el rendimiento aprovisionado en el contenedor (o el conjunto de contenedores).

## <a name="next-steps"></a>Pasos siguientes
 
- Aprenda cómo [establecer y obtener el rendimiento en Azure Cosmos DB](set-throughput.md) mediante Azure Portal y los SDK.
- Aprenda sobre las [pruebas de escalado y rendimiento con Azure Cosmos DB](performance-testing.md).
- Para más información sobre el rendimiento reservado con bases de datos de Azure Cosmos DB, consulte [Precios de Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) y [Particionamiento de datos en Azure Cosmos DB](partition-data.md).
- Para más información sobre Azure Cosmos DB, consulte la [documentación de Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/). 

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png

