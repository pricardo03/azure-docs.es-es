---
title: Creación de particiones y escalado horizontal en Azure Cosmos DB
description: Obtenga información sobre cómo particionar trabajos en Azure Cosmos DB, cómo configurar la creación de particiones y las claves de partición y cómo elegir la clave de partición correcta para su aplicación.
ms.author: rimman
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: 30290652044499ff8e537adc90689f562e1489d2
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65953775"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Creación de particiones y escalado horizontal en Azure Cosmos DB

En este artículo se explica las particiones físicas y lógicas en Azure Cosmos DB. También describe procedimientos recomendados para escalado y creación de particiones. 

## <a name="logical-partitions"></a>Particiones lógicas

Una partición lógica consta de un conjunto de elementos que tienen la misma clave de partición. Por ejemplo, en un contenedor donde todos los elementos contienen una `City` propiedad, puede usar `City` como clave de partición para el contenedor. Grupos de elementos que tienen valores específicos para `City`, tales como `London`, `Paris`, y `NYC`, forman distintas particiones lógicas. No tiene que preocuparse acerca de cómo eliminar una partición cuando se eliminan los datos subyacentes.

En Azure Cosmos DB, un contenedor es la unidad fundamental de escalabilidad. Datos que se agregan al contenedor y el rendimiento que se aprovisiona en el contenedor automáticamente son particiones (horizontales) a través de un conjunto de particiones lógicas. Rendimiento y los datos se particionan en función de la clave de partición que especifique para el contenedor de Azure Cosmos. Para obtener más información, consulte [crear un contenedor de Azure Cosmos](how-to-create-container.md).

Una partición lógica también define el ámbito de transacciones de base de datos. Puede actualizar los elementos dentro de una partición lógica mediante una [transacciones con aislamiento de instantánea](database-transactions-optimistic-concurrency.md). Cuando se agregan nuevos elementos a un contenedor, nuevas particiones lógicas se crean de forma transparente por el sistema.

## <a name="physical-partitions"></a>Particiones físicas

Un contenedor de Azure Cosmos se escala mediante la distribución de datos y el rendimiento a través de un gran número de particiones lógicas. Internamente, se asignan uno o más particiones lógicas en una partición física que consta de un conjunto de réplicas, también se denomina un [ *conjunto de réplicas*](global-dist-under-the-hood.md). Cada réplica establecidos que hospede una instancia del motor de base de datos de Azure Cosmos DB. Un conjunto de réplicas hace que los datos almacenados en la partición física duradera, alta disponibilidad y coherente. Una partición física es compatible con la cantidad máxima de unidades de almacenamiento y la solicitud (RU). Cada réplica que conforma la partición física hereda la cuota de almacenamiento de la partición. Todas las réplicas de una partición física colectivamente respaldar el rendimiento que se asigna a la partición física. 

La siguiente imagen muestra cómo se asignan particiones lógicas a particiones físicas distribuidas globalmente:

![Una imagen que se muestra la división de Azure Cosmos DB](./media/partition-data/logical-partitions.png)

Rendimiento aprovisionado para un contenedor se divide uniformemente entre las particiones físicas. Un diseño de clave de partición que no distribuye uniformemente las solicitudes de rendimiento puede crear particiones "activas". Particiones calientes podrían producir en la limitación de velocidad y en un uso ineficaz del rendimiento aprovisionado y unos costos mayores.

A diferencia de las particiones lógicas, las particiones físicas son una implementación interna del sistema. No se puede controlar el tamaño, ubicación o el número de particiones físicas y no se puede controlar la asignación entre las particiones lógicas y las particiones físicas. Sin embargo, puede controlar el número de particiones lógicas y la distribución de datos, la carga de trabajo y rendimiento por [elegir la clave de partición lógica apropiada](partitioning-overview.md#choose-partitionkey).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre [elegir una clave de partición](partitioning-overview.md#choose-partitionkey).
* Información sobre el [procesamiento aprovisionado en Azure Cosmos DB](request-units.md).
* Información sobre la [distribución global en Azure Cosmos DB](distribute-data-globally.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).
