---
title: Creación de particiones en Azure Cosmos DB
description: Información general de creación de particiones en Azure Cosmos DB.
ms.author: rimman
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.openlocfilehash: e88be8e7b94566ff94dd94a8679f8ade9d54c0b6
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762332"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Creación de particiones en Azure Cosmos DB

Azure Cosmos DB usa la creación de particiones para escalar contenedores individuales en una base de datos para satisfacer las necesidades de rendimiento de la aplicación. En las particiones, los elementos de un contenedor se dividen en distintos subconjuntos llamados *particiones lógicas*. Las particiones lógicas se forman en función del valor de un *clave de partición* que está asociado con cada elemento en un contenedor. Todos los elementos de una partición lógica tienen el mismo valor de clave de partición.

Por ejemplo, un contenedor contiene elementos. Cada elemento tiene un valor único para el `UserID` propiedad. Si `UserID` actúa como la partición de clave para los elementos en el contenedor y hay 1,000 único `UserID` , 1000 particiones lógicas se crean los valores para el contenedor.

Además de una clave de partición que determina la partición lógica del elemento, cada elemento de un contenedor tiene un *del identificador de elemento* (único dentro de una partición lógica). Combinación de la clave de partición y el identificador del elemento crea el elemento *índice*, que identifica de forma única el elemento.

[Elegir una clave de partición](partitioning-overview.md#choose-partitionkey) es una decisión importante que afecta al rendimiento de la aplicación.

## <a name="managing-logical-partitions"></a>Administración de particiones lógicas

Azure Cosmos DB transparente y automática administra la colocación de las particiones lógicas en las particiones físicas para satisfacer las necesidades de escalabilidad y rendimiento del contenedor de forma eficaz. A medida que aumentan los requisitos de rendimiento y el almacenamiento de una aplicación, Azure Cosmos DB mueve las particiones lógicas para la carga se propagan automáticamente a través de un mayor número de servidores. 

Azure Cosmos DB usa la creación de particiones por hash para distribuir las particiones lógicas entre las particiones físicas. Azure Cosmos DB aplica un algoritmo hash al valor de un elemento clave de partición. El resultado hash determina la partición física. A continuación, Azure Cosmos DB asigna el espacio de claves de partición de los hash de claves uniformemente entre las particiones físicas.

Las consultas que tienen acceso a datos dentro de una partición lógica única son más rentables que las consultas que tienen acceso a varias particiones. Se permiten transacciones (en procedimientos almacenados o desencadenadores) solo con respecto a elementos en una única partición lógica.

Para obtener más información acerca de cómo Azure Cosmos DB administra las particiones, vea [particiones lógicas](partition-data.md). (No es necesario conocer los detalles internos para compilar o ejecutar las aplicaciones, pero agregan aquí para un lector curiosidad).

## <a id="choose-partitionkey"></a>Elegir una clave de partición

El siguiente es una buena guía para elegir una clave de partición:

* Una única partición lógica tiene un límite de 10 GB de almacenamiento.  

* Los contenedores de Cosmos Azure tienen un rendimiento mínimo de 400 unidades de solicitud por segundo (RU/s). Las solicitudes a la misma clave de partición no pueden superar el rendimiento que se asigna a una partición. Si las solicitudes superan el rendimiento asignado, las solicitudes son velocidad limitada. Por tanto, es importante elegir una clave de partición que no dé como resultado "zonas activas" dentro de la aplicación.

* Elija una clave de partición que tenga una amplia gama de valores y patrones de acceso que se distribuyan uniformemente entre las particiones lógicas. Esto ayuda a distribuir los datos y la actividad en el contenedor en el conjunto de particiones lógicas, por lo que se pueden distribuir los recursos de almacenamiento de datos y rendimiento en las particiones lógicas.

* Elija una clave de partición que se distribuye la carga de trabajo uniformemente entre todas las particiones y uniformemente a través del tiempo. La elección de clave de partición debe equilibrar la necesidad de partición eficiente consultas y transacciones frente al objetivo de distribuir los elementos en varias particiones para lograr escalabilidad.

* Candidatos para las claves de partición podrían incluir propiedades que aparecen con frecuencia como un filtro en las consultas. Las consultas se pueden enrutar de manera eficaz si se incluye la clave de partición en el predicador de filtro.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [partición y escalado horizontal en Azure Cosmos DB](partition-data.md).
* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md).
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md).
