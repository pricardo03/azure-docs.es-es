---
title: Aprovisionamiento del rendimiento en contenedores y bases de datos de Azure Cosmos
description: Aprenda cómo establecer el rendimiento aprovisionado para las bases de datos y los contenedores de Azure Cosmos.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.openlocfilehash: ed3a171e60c078975de7003cdf599f0bac62c402
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61330320"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Aprovisionar rendimiento en contenedores y bases de datos

Una base de datos de Azure Cosmos es una unidad de administración de un conjunto de contenedores. Una base de datos consta de un conjunto de contenedores independiente del esquema. Un contenedor de Azure Cosmos es la unidad de escalabilidad de rendimiento y almacenamiento. Un contenedor se divide horizontalmente en un conjunto de máquinas dentro de una región de Azure y se distribuye en todas las regiones de Azure asociadas a su cuenta de Azure Cosmos.

Con Azure Cosmos DB, puede aprovisionar el rendimiento en dos granularidades:
 
- Contenedores de Azure Cosmos
- Bases de datos de Azure Cosmos

## <a name="set-throughput-on-a-container"></a>Establecimiento del rendimiento en un contenedor  

El rendimiento aprovisionado en un contenedor de Azure Cosmos se reserva exclusivamente para ese contenedor. El contenedor recibe el rendimiento aprovisionado todo el tiempo. El rendimiento aprovisionado en un contenedor está respaldado financieramente por los contratos de nivel de servicio. Para obtener información sobre cómo configurar el rendimiento en un contenedor, consulte [aprovisionamiento del rendimiento en un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).

Al establecer el rendimiento aprovisionado en un contenedor es la opción utilizada con frecuencia. Puede escalar elásticamente el rendimiento de un contenedor mediante el aprovisionamiento de cualquier cantidad de rendimiento mediante el uso de [unidades de solicitud (RU)](request-units.md). 

El rendimiento aprovisionado en un contenedor de Azure Cosmos se distribuye uniformemente entre todas las particiones lógicas del contenedor. Forma selectiva no puede especificar el rendimiento de las particiones lógicas. Puesto que una o varias particiones lógicas de un contenedor se hospedan en una partición física, las particiones físicas pertenecen exclusivamente al contenedor y admiten el rendimiento aprovisionado en dicho contenedor. 

Si la carga de trabajo que se ejecuta en una partición lógica consume más que el rendimiento que se ha asignado a esa partición lógica, las operaciones de obtención velocidad limitada. Cuando se produce la limitación de velocidad, puede aumentar el rendimiento aprovisionado para todo el contenedor o vuelva a intentar las operaciones. Para más información sobre las particiones, consulte [Particiones lógicas](partition-data.md).

Se recomienda configurar el rendimiento en la granularidad del contenedor cuando se quiere un rendimiento garantizado para dicho contenedor.

En la imagen siguiente se muestra cómo una partición física hospeda una o varias particiones lógicas de un contenedor:

![Partición física](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>Establecimiento del rendimiento en una base de datos

Al aprovisionar el rendimiento en una base de datos de Azure Cosmos, dicho rendimiento se comparte entre todos los contenedores de la base de datos. Una excepción es si ha especificado un rendimiento aprovisionado en determinados contenedores en la base de datos. Compartir el rendimiento aprovisionado de nivel de base de datos entre sus contenedores es análogo a hospedar una base de datos en un clúster de máquinas. Dado que todos los contenedores de una base de datos comparten los recursos disponibles en una máquina, no obtendrá un rendimiento predecible en ningún contenedor específico. Para obtener información sobre cómo configurar el rendimiento aprovisionado en una base de datos, vea [configurar rendimiento aprovisionado en una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).

Al establecer el rendimiento en una base de datos de Cosmos Azure garantiza que recibirá el rendimiento aprovisionado para esa base de datos de todo el tiempo. Dado que todos los contenedores de la base de datos comparten el rendimiento aprovisionado, Azure Cosmos DB no proporciona ninguna garantía de rendimiento predecible para un contenedor determinado de dicha base de datos. La parte del rendimiento que puede recibir un contenedor específico depende de los siguientes factores:

* El número de contenedores.
* La elección de las claves de partición de varios contenedores.
* La distribución de la carga de trabajo en varias particiones lógicas de los contenedores. 

Se recomienda configurar el rendimiento en una base de datos si quiere compartir el rendimiento entre varios contenedores, pero no dedicarlo a ningún contenedor específico. 

En los siguientes ejemplos se muestra dónde es preferible aprovisionar el rendimiento en el nivel de base de datos:

* El uso compartido del rendimiento aprovisionado de una base de datos a través de un conjunto de contenedores es útil para una aplicación de varios inquilinos. Cada usuario puede representarse mediante un contenedor de Azure Cosmos diferente.

* El uso compartido del rendimiento aprovisionado de una base de datos a través de un conjunto de contenedores es útil al migrar una base de datos de NoSQL, como MongoDB o Cassandra, hospedada en un clúster de máquinas virtuales o desde servidores físicos locales a Azure Cosmos DB. Puede considerar el rendimiento aprovisionado configurado en la base de datos de Azure Cosmos como un equivalente lógico, pero más rentable y flexible, de la capacidad de proceso del clúster de MongoDB o Cassandra.  

Se deben crear todos los contenedores creados dentro de una base de datos con rendimiento aprovisionado con un [clave de partición](partition-data.md). En cualquier momento determinado, el rendimiento asignado a un contenedor dentro de una base de datos se distribuye entre todas las particiones lógicas de dicho contenedor. Cuando haya contenedores que comparten el rendimiento aprovisionado configurado en una base de datos, no se puede aplicar selectivamente el rendimiento a un contenedor específico o una partición lógica. 

Si la carga de trabajo en una partición lógica consume más que el rendimiento que se asignó a una partición lógica específica, las operaciones tendrán una velocidad limitada. Cuando se produce la limitación de velocidad, puede aumentar el rendimiento de la base de datos completa o vuelva a intentar las operaciones. Para más información sobre las particiones, consulte [Particiones lógicas](partition-data.md).

Varias particiones lógicas que pertenecen a distintos contenedores que comparten el rendimiento aprovisionado para una base de datos se pueden hospedar en una única partición física. Mientras que una única partición lógica de un contenedor siempre dentro del ámbito de una partición física, *"L"* particiones lógicas entre *"C"* pueden ser contenedores que comparten el rendimiento aprovisionado de una base de datos asigna y se hospeda en *"R"* particiones físicas. 

En la siguiente imagen se muestra cómo una partición física puede hospedar una o varias particiones lógicas que pertenecen a distintos contenedores dentro de una base de datos:

![Partición física](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>Establecimiento del rendimiento en un contenedor y una base de datos

Puede combinar los dos modelos. Se permite el aprovisionamiento del rendimiento tanto en la base de datos como en el contenedor. En el ejemplo siguiente se muestra cómo aprovisionar el rendimiento en un contenedor y una base de datos de Azure Cosmos:

* Puede crear una base de datos de Azure Cosmos denominada *Z* con rendimiento aprovisionado de *"K"* RU. 
* A continuación, cree cinco contenedores llamados *A*, *B*, *C*, *d.*, y *E* dentro de la base de datos.
* Puede configurar explícitamente *"P"* unidades de solicitud del rendimiento aprovisionado en el contenedor llamado *B*.
* El *"K"* RUs rendimiento se comparte entre los cuatro contenedores *A*, *C*, *d.*, y *E*. La cantidad exacta de rendimiento disponible para *A*, *C*, *d.*, o *E* varía. No hay ningún Acuerdo de Nivel de Servicio para el rendimiento de cada contenedor individual.
* El contenedor llamado *B* se garantiza que obtenga el *"P"* rendimiento RUs todo el tiempo. Estará respaldado por los Acuerdos de Nivel de Servicio.

## <a name="update-throughput-on-a-database-or-a-container"></a>Rendimiento de la actualización en una base de datos o un contenedor

Después de crear un contenedor de Cosmos Azure o en una base de datos, puede actualizar el rendimiento aprovisionado. No hay ningún límite en el rendimiento aprovisionado máximo que puede configurar en la base de datos o el contenedor. El rendimiento aprovisionado mínimo depende de los siguientes factores: 

* El tamaño máximo de los datos que nunca se almacena en el contenedor
* El rendimiento máximo que nunca se aprovisiona en el contenedor
* El número máximo de contenedores de Azure Cosmos que nunca de crear en una base de datos con un rendimiento compartido. 

Puede recuperar el rendimiento mínimo de un contenedor o una base de datos mediante programación utilizando los SDK o ver el valor en el portal de Azure. Al usar el SDK. NET, el [DocumentClient.ReplaceOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet) método le permite escalar el valor de rendimiento aprovisionado. Al usar el SDK de Java, el [RequestOptions.setOfferThroughput](sql-api-java-samples.md#offer-examples) método le permite escalar el valor de rendimiento aprovisionado. 

Al usar el SDK. NET, el [DocumentClient.ReadOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet) método le permite recuperar el rendimiento mínimo de un contenedor o una base de datos. 

Puede escalar el rendimiento aprovisionado de un contenedor o una base de datos en cualquier momento. 

## <a name="comparison-of-models"></a>Comparación de modelos

|**Parámetro**  |**Rendimiento aprovisionado en una base de datos**  |**Rendimiento aprovisionado en un contenedor**|
|---------|---------|---------|
|Número mínimo de RU |400 (después de los cuatro primeros contenedores, cada contenedor adicional requiere un mínimo de 100 RU por segundo). |400|
|Número mínimo de RU por contenedor|100|400|
|Número mínimo de RU necesarias para consumir 1 GB de almacenamiento|40|40|
|Número máximo de RU|Ilimitado, en la base de datos.|Ilimitado, en el contenedor.|
|RU asignadas o disponibles para un contenedor específico|Sin garantías. Las RU asignadas a un contenedor determinado dependen de las propiedades. Las propiedades pueden ser la elección de las claves de partición de contenedores que comparten el rendimiento, la distribución de la carga de trabajo y el número de contenedores. |Todas las RU configuradas en el contenedor se reservan exclusivamente para el contenedor.|
|Almacenamiento máximo de un contenedor|Sin límite.|Sin límite.|
|Rendimiento máximo por partición lógica de un contenedor|10 000 RU|10 000 RU|
|Almacenamiento máximo (datos + índice) por partición lógica de un contenedor|10 GB|10 GB|

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [particiones lógicas](partition-data.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).

