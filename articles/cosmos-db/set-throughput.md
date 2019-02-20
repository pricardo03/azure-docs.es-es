---
title: Aprovisionamiento del rendimiento en contenedores y bases de datos de Azure Cosmos
description: Aprenda cómo establecer el rendimiento aprovisionado para las bases de datos y los contenedores de Azure Cosmos.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: andrl
ms.openlocfilehash: 439b48c271260e9744bb9c9ca0e2b21e61cf4687
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005070"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Aprovisionar rendimiento en contenedores y bases de datos

Una base de datos de Azure Cosmos es una unidad de administración de un conjunto de contenedores. Una base de datos consta de un conjunto de contenedores independiente del esquema. Un contenedor de Azure Cosmos es la unidad de escalabilidad de rendimiento y almacenamiento. Un contenedor se divide horizontalmente en un conjunto de máquinas dentro de una región de Azure y se distribuye en todas las regiones de Azure asociadas a su cuenta de Azure Cosmos.

Con Azure Cosmos DB, puede configurar el rendimiento en dos granularidades:
 
- Contenedores de Azure Cosmos
- Bases de datos de Azure Cosmos

## <a name="set-throughput-on-a-container"></a>Establecimiento del rendimiento en un contenedor  

El rendimiento aprovisionado en un contenedor de Azure Cosmos se reserva exclusivamente para el contenedor. El contenedor recibe el rendimiento aprovisionado todo el tiempo. El rendimiento aprovisionado en un contenedor está respaldado financieramente por los contratos de nivel de servicio. Para configurar el rendimiento en un contenedor, consulte [Aprovisionamiento del rendimiento en un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).

La configuración del rendimiento aprovisionado en un contenedor es la opción más utilizada. Puede escalar elásticamente el rendimiento de un contenedor con el aprovisionamiento de cualquier cantidad de rendimiento mediante el uso de unidades de solicitud (RU). Pero no puede especificar de forma selectiva el rendimiento de las particiones lógicas. 

Si la carga de trabajo que se ejecuta en una partición lógica consume más que el rendimiento que se asignó a la partición lógica específica, las operaciones tienen una velocidad limitada. Cuando se produce una limitación de velocidad, puede aumentar el rendimiento de todo el contenedor o volver a intentar las operaciones. Para más información sobre las particiones, consulte [Particiones lógicas](partition-data.md).

Se recomienda configurar el rendimiento en la granularidad del contenedor cuando se quiere un rendimiento garantizado para dicho contenedor.

El rendimiento aprovisionado en un contenedor de Azure Cosmos se distribuye uniformemente entre todas las particiones lógicas del contenedor. Puesto que una o varias particiones lógicas de un contenedor se hospedan en una partición física, las particiones físicas pertenecen exclusivamente al contenedor y admiten el rendimiento aprovisionado en dicho contenedor. 

En la imagen siguiente se muestra cómo una partición física hospeda una o varias particiones lógicas de un contenedor:

![Partición física](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>Establecimiento del rendimiento en una base de datos

Al aprovisionar el rendimiento en una base de datos de Azure Cosmos, dicho rendimiento se comparte entre todos los contenedores de la base de datos. Una excepción es si ha especificado un rendimiento aprovisionado en determinados contenedores. Compartir el rendimiento de la base de datos entre sus contenedores equivale a hospedar una base de datos en un clúster de máquinas. Dado que todos los contenedores de una base de datos comparten los recursos disponibles en una máquina, no obtendrá un rendimiento predecible en ningún contenedor específico. Para configurar el rendimiento en una base de datos, consulte [Aprovisionamiento del rendimiento en una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).

La configuración del rendimiento en una base de datos de Azure Cosmos garantiza la recepción del rendimiento aprovisionado todo el tiempo. Dado que todos los contenedores de la base de datos comparten el rendimiento aprovisionado, Azure Cosmos DB no proporciona ninguna garantía de rendimiento predecible para un contenedor determinado de dicha base de datos. La parte del rendimiento que puede recibir un contenedor específico depende de los siguientes factores:

* El número de contenedores.
* La elección de las claves de partición de varios contenedores.
* La distribución de la carga de trabajo en varias particiones lógicas de los contenedores. 

Se recomienda configurar el rendimiento en una base de datos si quiere compartir el rendimiento entre varios contenedores, pero no dedicarlo a ningún contenedor específico. 

En los siguientes ejemplos se muestra dónde es preferible aprovisionar el rendimiento en el nivel de base de datos:

* El uso compartido del rendimiento aprovisionado de una base de datos a través de un conjunto de contenedores es útil para una aplicación de varios inquilinos. Cada usuario puede representarse mediante un contenedor de Azure Cosmos diferente.

* El uso compartido del rendimiento aprovisionado de una base de datos a través de un conjunto de contenedores es útil al migrar una base de datos de NoSQL, como MongoDB o Cassandra, hospedada en un clúster de máquinas virtuales o desde servidores físicos locales a Azure Cosmos DB. Puede considerar el rendimiento aprovisionado configurado en la base de datos de Azure Cosmos como un equivalente lógico, pero más rentable y flexible, de la capacidad de proceso del clúster de MongoDB o Cassandra.  

Todos los contenedores creados dentro de una base de datos con rendimiento aprovisionado deben crearse con una clave de partición. En cualquier momento determinado, el rendimiento asignado a un contenedor dentro de una base de datos se distribuye entre todas las particiones lógicas de dicho contenedor. Cuando hay contenedores que comparten el rendimiento aprovisionado en una base de datos, el rendimiento no se puede aplicar de forma selectiva a un contenedor específico o a una partición lógica. 

Si la carga de trabajo en una partición lógica consume más que el rendimiento que se asignó a una partición lógica específica, las operaciones tendrán una velocidad limitada. Cuando se produce una limitación de velocidad, puede aumentar el rendimiento de todo el contenedor o volver a intentar las operaciones. Para más información sobre las particiones, consulte [Particiones lógicas](partition-data.md).

Varias particiones lógicas que comparten el rendimiento aprovisionado en una base de datos se pueden hospedar en una partición física única. Mientras que una única partición lógica de un contenedor siempre se encuentra dentro del ámbito de una partición física, las particiones lógicas de "L" en los contenedores de "C" que comparten el rendimiento aprovisionado de una base de datos pueden asignarse a particiones físicas de "R" y hospedarse en ellas. 

En la siguiente imagen se muestra cómo una partición física puede hospedar una o varias particiones lógicas que pertenecen a distintos contenedores dentro de una base de datos:

![Partición física](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>Establecimiento del rendimiento en un contenedor y una base de datos

Puede combinar los dos modelos. Se permite el aprovisionamiento del rendimiento tanto en la base de datos como en el contenedor. En el ejemplo siguiente se muestra cómo aprovisionar el rendimiento en un contenedor y una base de datos de Azure Cosmos:

* Puede crear una base de datos de Azure Cosmos denominada "Z" con rendimiento aprovisionado de RU de "K". 
* A continuación, cree cinco contenedores denominados A, B, C, D y E dentro de la base de datos.
* Puede configurar explícitamente RU de "P" de rendimiento aprovisionado en el contenedor denominado "B".
* El rendimiento de RU de "K" se comparte entre los cuatro contenedores: A, C, D y E. La cantidad exacta de rendimiento disponible para A, C, D o E varía. No hay ningún Acuerdo de Nivel de Servicio para el rendimiento de cada contenedor individual.
* Se garantiza que el contenedor denominado "B" obtendrá el rendimiento de RU de "P" todo el tiempo. Estará respaldado por los Acuerdos de Nivel de Servicio.

## <a name="comparison-of-models"></a>Comparación de modelos

|**Cuota**  |**Rendimiento aprovisionado en una base de datos**  |**Rendimiento aprovisionado en un contenedor**|
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

