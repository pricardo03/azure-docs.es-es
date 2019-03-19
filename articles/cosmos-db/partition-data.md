---
title: Creación de particiones y escalado horizontal en Azure Cosmos DB
description: Obtenga información sobre cómo particionar trabajos en Azure Cosmos DB, cómo configurar la creación de particiones y las claves de partición y cómo elegir la clave de partición correcta para su aplicación.
ms.author: mjbrown
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 4c6847d8f870c02228aa14ab9e11c85b091ec48b
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959959"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Creación de particiones y escalado horizontal en Azure Cosmos DB

En este artículo se explica las particiones físicas y lógicas en Azure Cosmos DB. También describe procedimientos recomendados para escalado y creación de particiones. 

## <a name="logical-partitions"></a>Particiones lógicas

Una partición lógica consta de un conjunto de elementos que tienen la misma clave de partición. Por ejemplo, en un contenedor donde todos los elementos contienen una `City` propiedad, puede usar `City` como clave de partición para el contenedor. Grupos de elementos que tienen valores específicos para `City`, tales como `London`, `Paris`, y `NYC`, forman una partición lógica distinta. No tiene que preocuparse acerca de cómo eliminar una partición cuando se eliminan los datos subyacentes.

En Azure Cosmos DB, un contenedor es la unidad fundamental de escalabilidad. Datos que se agregan al contenedor y el rendimiento que se aprovisiona en el contenedor automáticamente son particiones (horizontales) a través de un conjunto de particiones lógicas. Rendimiento y los datos se particionan en función de la clave de partición que especifique para el contenedor de Azure Cosmos DB. Para obtener más información, consulte [crear un contenedor de Azure Cosmos DB](how-to-create-container.md).

Una partición lógica define el ámbito de las transacciones de base de datos. Puede actualizar los elementos dentro de una partición lógica mediante el uso de una transacción con aislamiento de instantánea. Cuando se agregan nuevos elementos al contenedor, se crean nuevas particiones lógicas de forma transparente por el sistema.

## <a name="physical-partitions"></a>Particiones físicas

Un contenedor de Azure Cosmos DB se escala mediante la distribución de datos y el rendimiento a través de un gran número de particiones lógicas. Internamente, se asignan uno o más particiones lógicas en una partición física que consta de un conjunto de réplicas, también se denomina un *conjunto de réplicas*. Cada réplica establecidos que hospede una instancia del motor de base de datos de Azure Cosmos DB. Un conjunto de réplicas hace que los datos almacenados en la partición física duradera, alta disponibilidad y coherente. Una partición física es compatible con la cantidad máxima de unidades de almacenamiento y la solicitud (RU). Cada réplica que conforma la partición física hereda la cuota de almacenamiento de la partición. Todas las réplicas de una partición física colectivamente respaldar el rendimiento que se asigna a la partición física. 

La siguiente imagen muestra cómo se asignan particiones lógicas a particiones físicas distribuidas globalmente:

![Una imagen que se muestra la división de Azure Cosmos DB](./media/partition-data/logical-partitions.png)

Rendimiento aprovisionado para un contenedor se divide uniformemente entre las particiones físicas. Un diseño de clave de partición que no distribuye uniformemente las solicitudes de rendimiento puede crear particiones "activas". Particiones calientes podrían producir en la limitación de velocidad y en un uso ineficaz del rendimiento aprovisionado.

A diferencia de las particiones lógicas, las particiones físicas son una implementación interna del sistema. No se puede controlar el tamaño, ubicación o el número de particiones físicas y no se puede controlar la asignación entre las particiones lógicas y las particiones físicas. Sin embargo, puede controlar el número de particiones lógicas, así como la distribución de datos y rendimiento mediante la elección de la clave de partición correcta.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [elegir una clave de partición](partitioning-overview.md#choose-partitionkey).
* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md).
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md).
* Obtenga información sobre cómo [aprovisionamiento del rendimiento en un contenedor de Azure Cosmos DB](how-to-provision-container-throughput.md).
* Obtenga información sobre cómo [aprovisionamiento del rendimiento en una base de datos de Azure Cosmos DB](how-to-provision-database-throughput.md).
