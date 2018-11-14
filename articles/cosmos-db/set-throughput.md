---
title: Aprovisionamiento del rendimiento de Azure Cosmos DB
description: Aprenda cómo establecer el rendimiento aprovisionado para las bases de datos y los contenedores de Azure Cosmos DB.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: andrl
ms.openlocfilehash: 24b6beec8ecda993667464be5c74dab50fd93201
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278895"
---
# <a name="provision-throughput-for-cosmos-db-containers-and-databases"></a>Aprovisionamiento del rendimiento de contenedores y bases de datos de Cosmos DB

Una base de datos de Cosmos es una unidad de administración de un conjunto de contenedores. Una base de datos consta de un conjunto de contenedores independiente del esquema. Un contenedor de Cosmos es la unidad de escalabilidad de rendimiento y almacenamiento. Un contenedor se divide horizontalmente en un conjunto de máquinas dentro de una región de Azure y se distribuye en todas las regiones de Azure asociadas a su cuenta de Cosmos.

Azure Cosmos DB le permite configurar el rendimiento en dos granularidades: **contenedores de Cosmos** y **bases de datos de Cosmos**.

# <a name="setting-throughput-on-a-cosmos-container"></a>Configuración del rendimiento en un contenedor de Cosmos  

El rendimiento aprovisionado en un contenedor de Cosmos se reserva exclusivamente para el contenedor. El contenedor recibe el rendimiento aprovisionado todo el tiempo. El rendimiento aprovisionado en un contenedor está respaldado financieramente por los contratos de nivel de servicio. Para configurar el rendimiento en un contenedor, consulte [Aprovisionamiento del rendimiento de un contenedor de Azure Cosmos DB](how-to-provision-container-throughput.md).

La configuración del rendimiento aprovisionado en un contenedor es la opción más utilizada. Aunque puede escalar de forma flexible el rendimiento de un contenedor mediante el aprovisionamiento de cualquier cantidad de rendimiento (RU), no se puede especificar selectivamente el rendimiento de las particiones lógicas. Cuando la carga de trabajo que se ejecuta en una partición lógica consume más que el rendimiento que se asignó a la partición lógica específica, las operaciones tendrán una velocidad limitada. Cuando se produce una limitación de velocidad, puede aumentar el rendimiento de todo el contenedor o volver a intentar la operación. Para más información sobre las particiones, consulte [Particiones lógicas](partition-data.md).

Se recomienda configurar el rendimiento en la granularidad del contenedor cuando se quiere un rendimiento garantizado para el contenedor.

El rendimiento aprovisionado en un contenedor de Cosmos se distribuye uniformemente entre todas las particiones lógicas del contenedor. Puesto que una o varias particiones lógicas de un contenedor se hospedan en una partición de recursos, las particiones físicas pertenecen exclusivamente al contenedor y admiten el rendimiento aprovisionado en el contenedor. En la imagen siguiente se muestra cómo una partición de recursos hospeda una o varias particiones lógicas de un contenedor:

![Partición de recursos](./media/set-throughput/resource-partition.png)

# <a name="setting-throughput-on-a-cosmos-database"></a>Configuración del rendimiento en una base de datos de Cosmos

Al aprovisionar el rendimiento en una base de datos de Cosmos, este se comparte entre todos los contenedores de la base de datos, a menos que haya especificado un rendimiento aprovisionado en determinados contenedores. Compartir el rendimiento de la base de datos entre sus contenedores equivale a hospedar una base de datos en un clúster de máquinas. Dado que todos los contenedores de una base de datos comparten los recursos disponibles en una máquina, no obtendrá un rendimiento predecible en ningún contenedor específico. Para configurar el rendimiento en una base de datos, consulte [Aprovisionamiento del rendimiento de una base de datos en Azure Cosmos DB](how-to-provision-database-throughput.md).

La configuración del rendimiento en una base de datos de Cosmos garantiza la recepción del rendimiento aprovisionado todo el tiempo. Dado que todos los contenedores de la base de datos comparten el rendimiento aprovisionado, Cosmos DB no proporciona ninguna garantía de rendimiento predecible para un contenedor determinado de dicha base de datos. La parte del rendimiento que puede recibir un contenedor específico depende de los siguientes factores:

* El número de contenedores.
* La elección de las claves de partición de varios contenedores.
* La distribución de la carga de trabajo en varias particiones lógicas de los contenedores. 

Se recomienda configurar el rendimiento en una base de datos si quiere compartir el rendimiento entre varios contenedores, pero no dedicarlo a ningún contenedor específico. A continuación, se muestran algunos ejemplos donde es preferible aprovisionar el rendimiento en el nivel de base de datos:

* El uso compartido del rendimiento aprovisionado de una base de datos a través de un conjunto de contenedores es útil para una aplicación de varios inquilinos. Cada usuario puede representarse mediante un contenedor de Cosmos diferente.

* El uso compartido del rendimiento aprovisionado de una base de datos a través de un conjunto de contenedores es útil al migrar una base de datos de NoSQL (como MongoDB o Cassandra) hospedada desde un clúster de VM o servidores físicos locales a Cosmos DB. Se puede considerar el rendimiento aprovisionado configurado en la base de datos de Cosmos como un equivalente lógico (pero más rentable y flexible) de la capacidad de proceso del clúster de MongoDB o Cassandra.  

En cualquier momento determinado, el rendimiento asignado a un contenedor dentro de una base de datos se distribuye entre todas las particiones lógicas de dicho contenedor. Cuando hay contenedores que comparten el rendimiento aprovisionado en una base de datos, no se puede aplicar de forma selectiva el rendimiento a un contenedor específico o una partición lógica. Si la carga de trabajo en una partición lógica consume más que el rendimiento que se asignó a una partición lógica específica, las operaciones tendrán una velocidad limitada. Cuando se produce una limitación de velocidad, puede aumentar el rendimiento de todo el contenedor o volver a intentar la operación. Para más información sobre las particiones, consulte [Particiones lógicas](partition-data.md).

Varias particiones lógicas que comparten el rendimiento aprovisionado en una base de datos se pueden hospedar en una partición de recurso único. Mientras que una única partición lógica de un contenedor siempre se encuentra dentro del ámbito de una partición de recursos, las particiones lógicas de "L" en los contenedores de "C" que comparten el rendimiento aprovisionado de una base de datos pueden asignarse y hospedarse en particiones físicas de "R". En la siguiente imagen se muestra cómo una partición de recursos puede hospedar una o varias particiones lógicas que pertenecen a distintos contenedores dentro de una base de datos:

![Partición de recursos](./media/set-throughput/resource-partition2.png)

## <a name="setting-throughput-on-a-cosmos-database-and-a-container"></a>Configuración del rendimiento en un contenedor y una base de datos de Cosmos

Puede combinar los dos modelos, ya que se permite el aprovisionamiento del rendimiento tanto en la base de datos como en el contenedor. En el ejemplo siguiente se muestra cómo aprovisionar el rendimiento en un contenedor y una base de datos de Cosmos:

* Puede crear una base de datos de Cosmos denominada "Z" con rendimiento aprovisionado de RU de "K". 
* A continuación, cree cinco contenedores denominados A, B, C, D y E dentro de la base de datos.
* Puede configurar explícitamente RU de "P" de rendimiento aprovisionado en el contenedor "B".
* El rendimiento de RU de "K" se comparte entre los cuatro contenedores: A, C, D y E. La cantidad exacta de rendimiento disponible para A, C, D o E varía y no hay ningún contrato de nivel de servicio para el rendimiento de cada contenedor individual.
* Se garantiza que el contenedor "B" obtendrá el rendimiento de RU de "P" todo el tiempo y que estará respaldado por los contratos de nivel de servicio.

## <a name="comparison-of-models"></a>Comparación de modelos

|**Cuota**  |**Rendimiento aprovisionado en una base de datos**  |**Rendimiento aprovisionado en un contenedor**|
|---------|---------|---------|
|Unidad de escalabilidad|Contenedor|Contenedor|
|Número mínimo de RU |400 |400|
|Número mínimo de RU por contenedor|100|400|
|Número mínimo de RU necesarias para consumir 1 GB de almacenamiento|40|40|
|Número máximo de RU|Ilimitado (en la base de datos)|Ilimitado (en el contenedor)|
|RU asignadas o disponibles para un contenedor específico|Sin garantías. Las RU asignadas a un contenedor determinado dependen de propiedades como la elección de las claves de partición de los contenedores que comparten el rendimiento, la distribución de la carga de trabajo o el número de contenedores. |Todas las RU configuradas en el contenedor se reservan exclusivamente para el contenedor.|
|Almacenamiento máximo de un contenedor|Ilimitado|Ilimitado|
|Rendimiento máximo por partición lógica de un contenedor|10 000 RU|10 000 RU|
|Almacenamiento máximo (datos + índice) por partición lógica de un contenedor|10 GB|10 GB|

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [particiones lógicas](partition-data.md)
* Más información sobre [cómo aprovisionar rendimiento en un contenedor de Cosmos](how-to-provision-container-throughput.md)
* Más información sobre [cómo aprovisionar rendimiento en una base de datos de Cosmos](how-to-provision-database-throughput.md)

