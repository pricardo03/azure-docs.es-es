---
title: Escalado del rendimiento para contenedores de Azure Cosmos DB
description: En este artículo se describe cómo Azure Cosmos DB escala elásticamente el rendimiento
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: f930b5c478cc880952b4559be4c6647b260efcf2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243489"
---
# <a name="globally-scale-provisioned-throughput"></a>Escalado global del rendimiento aprovisionado 

En Azure Cosmos DB, rendimiento aprovisionado se representa como solicitud unidades por segundo (RU/s o la forma plural RU). RU mide el coste de las operaciones de lectura y escritura en el contenedor de Cosmos, tal como se muestra en la imagen siguientes:

![Unidades de solicitud](./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png)

Puede aprovisionar RU en un contenedor de Cosmos o en una base de datos de Cosmos. RU aprovisionadas en un contenedor están disponibles exclusivamente para las operaciones realizadas en ese contenedor. Las RU aprovisionadas en una base de datos se comparten entre todos los contenedores de esa base de datos (excepto con los contenedores con RU asignadas exclusivamente).

Para escalar elásticamente de rendimiento aprovisionado, puede aumentar o disminuir las RU/s aprovisionadas en cualquier momento. Para obtener más información, consulte [procedimientos de aprovisionamiento del rendimiento](set-throughput.md) y escalar bases de datos y contenedores de Cosmos. Para escalar globalmente de rendimiento, puede agregar o quitar regiones de la cuenta de Cosmos en cualquier momento. Para obtener más información, consulte [Incorporación o eliminación de regiones de una cuenta de base de datos](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Asociación de varias regiones con una cuenta de Cosmos es importante en muchos escenarios, para lograr una latencia baja y [alta disponibilidad](high-availability.md) en todo el mundo.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>Cómo se distribuye el rendimiento aprovisionado entre las regiones

Si aprovisiona *'R'* RUs en un contenedor de Cosmos (o base de datos), Cosmos DB garantiza que *'R'* RU están disponibles en *cada* región asociada con su cuenta de Cosmos. Cada vez que agregue una nueva región a su cuenta de Cosmos DB aprovisiona automáticamente *'R'* RUs en la región recién agregada. Se garantiza que las operaciones realizadas en el contenedor de Cosmos obtener *'R'* RUs en cada región. No se pueden asignar RU de forma selectiva a una región específica. Las unidades de solicitud aprovisionadas en un contenedor de Cosmos (o base de datos) se aprovisionan en todas las regiones asociadas con su cuenta de Cosmos.

Suponiendo que un contenedor de Cosmos está configurado con *'R'* RU y hay *"n"* regiones asociadas con la cuenta de Cosmos, a continuación:

- Si la cuenta de Cosmos está configurada con una región de escritura única, las unidades de solicitud total disponible globalmente en el contenedor = *R* x *N*.

- Si la cuenta de Cosmos está configurada con varias regiones de escritura, las unidades de solicitud total disponible globalmente en el contenedor = *R* x (*N*+ 1). El texto adicional *R* RUs se aprovisionan automáticamente en conflictos de actualización de proceso y entropía contra el tráfico entre las regiones.

La elección del [modelo de coherencia](consistency-levels.md) también afecta a la capacidad de proceso. Puede obtener aproximadamente 2 veces el rendimiento de lectura para los niveles de coherencia más moderados (p. ej., *sesión*, *prefijo coherente* y *eventual* coherencia) en comparación con niveles de coherencia más fuertes (p. ej., *obsolescencia* o *seguro* coherencia).

## <a name="next-steps"></a>Pasos siguientes

A continuación puede aprender a configurar el rendimiento en un contenedor o base de datos:

* [Obtener y configurar rendimiento para contenedores y bases de datos](set-throughput.md) 

