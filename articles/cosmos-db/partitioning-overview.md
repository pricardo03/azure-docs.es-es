---
title: Creación de particiones en Azure Cosmos DB
description: Información general sobre la creación de particiones en Azure Cosmos DB
ms.author: mjbrown
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 20e869d30f9e7b8102b723870f0102e041e064d7
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042281"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Creación de particiones en Azure Cosmos DB

La creación de particiones es la técnica empleada por Cosmos DB para escalar contenedores individuales en una base de datos para satisfacer las necesidades de rendimiento de la aplicación. Mediante el uso de la creación de particiones, los elementos de un contenedor se dividen en distintos subconjuntos, que se denominan particiones lógicas. Las particiones lógicas se crean en función del valor de una propiedad de clave de partición asociada con cada elemento.

Una partición lógica es un subconjunto de elementos distintos de un contenedor. Los elementos de una partición lógica se identifican por el valor de clave de partición que se comparte entre todos los elementos de la partición lógica.  Por ejemplo, considere un contenedor que incluye documentos, y cada documento tiene una propiedad `UserID`.  Si `UserID` actúa como la partición de clave para los elementos de un contenedor y hay 1000 valores `UserID` exclusivos, se crearán 1000 particiones lógicas del contenedor.

Cada elemento de un contenedor tiene una **clave de partición** que determina la **partición lógica** del elemento, y cada elemento tiene también un **identificador de elemento** (que es único dentro de una partición lógica).  El **índice** de un elemento lo identifica de forma exclusiva y se forma mediante la combinación de la clave de partición y el identificador del elemento.

Elegir una clave de partición es una decisión importante que afecta al rendimiento de la aplicación.  Para más información, vea el artículo [Elegir una clave de partición](partitioning-overview.md#choose-partitionkey) para obtener instrucciones detalladas.

## <a name="logical-partition-management"></a>Administración de particiones lógicas

Cosmos DB administra de forma transparente y automática la ubicación de las particiones lógicas en particiones físicas (infraestructura de servidor) para satisfacer de manera eficaz las necesidades de escalabilidad y rendimiento del contenedor. A medida que aumentan los requisitos de rendimiento y almacenamiento de la aplicación, Cosmos DB mueve las particiones lógicas para distribuir automáticamente la carga entre un número más elevado de servidores. Para más información sobre cómo Cosmos DB administra las particiones, vea el artículo de [particiones lógicas](partition-data.md). No es necesario comprender estos detalles para compilar o ejecutar las aplicaciones.

Cosmos DB usa la creación de particiones por hash para distribuir las particiones lógicas entre las particiones físicas.  Cosmos DB aplica un algoritmo hash al valor de la clave de partición de un elemento, y ese resultado hash determina la partición física. Cosmos DB asigna el espacio de claves de los hash de claves de partición uniformemente entre las "N" particiones físicas.

Las consultas que tienen acceso a datos dentro de una sola partición son más rentables que las consultas que tienen acceso a varias particiones. Las transacciones (en procedimientos almacenados o desencadenadores) solo se permiten con elementos dentro de una única partición lógica.  

## <a id="choose-partitionkey"></a>Elegir una clave de partición

Al elegir una clave de partición, tenga en cuenta los siguientes detalles:

* En una única partición lógica se admite un límite máximo de 10 GB de almacenamiento.  

* Los contenedores con particiones se configuran con un rendimiento mínimo de 400 RU/s. Las solicitudes a la misma clave de partición no pueden exceder el rendimiento asignado a una partición. Si superan el rendimiento asignado, se limitará la velocidad de las solicitudes. Por tanto, es importante elegir una clave de partición que no dé como resultado "zonas activas" dentro de la aplicación.

* Elija una clave de partición que distribuya la carga de trabajo de manera uniforme entre todas las particiones y también a lo largo del tiempo.  La elección de la clave de partición debe equilibrar la necesidad de consultas o transacciones de partición eficaces con el objetivo de distribuir los elementos entre varias particiones para conseguir escalabilidad.

* Elija una clave de partición que tenga una amplia gama de valores y patrones de acceso que se distribuyan uniformemente entre las particiones lógicas. La idea básica es distribuir los datos y la actividad en el contenedor entre el conjunto de particiones lógicas, para que los recursos de rendimiento y almacenamiento de datos se puedan distribuir entre las particiones lógicas.

* Los candidatos para las claves de partición pueden incluir las propiedades que aparecen con frecuencia como un filtro en las consultas. Las consultas se pueden enrutar de manera eficaz si se incluye la clave de partición en el predicador de filtro.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre las [particiones](partition-data.md).
* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md)
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md)
