---
title: Escalado del rendimiento para contenedores de Azure Cosmos DB
description: En este artículo se describe cómo Azure Cosmos DB escala elásticamente el rendimiento
services: cosmos-db
author: dharmas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: adbac964e6654e16f6c405b9a5b8669326583f3e
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243950"
---
# <a name="scaling-throughput-in-azure-cosmos-db"></a>Escalado del rendimiento para contenedores de Azure Cosmos DB

En Azure Cosmos DB, el rendimiento aprovisionado se representa como unidades de solicitud por segundo (RU/s, plural: RU). RU mide el coste de las operaciones de lectura y escritura en el contenedor de Cosmos.

![Unidades de solicitud](./media/scale-throughput/figure1.png)

Puede aprovisionar RU en un contenedor de Cosmos o en una base de datos de Cosmos. Las RU aprovisionadas en un contenedor están disponibles exclusivamente para las operaciones que se realicen en ese contenedor. Las RU aprovisionadas en una base de datos se comparten entre todos los contenedores de esa base de datos (excepto con los contenedores con RU asignadas exclusivamente).

Para escalar elásticamente el rendimiento o reducirlo verticalmente, puede aumentar o disminuir el valor de RU/s aprovisionado en cualquier momento. Para obtener más información, consulte cómo aprovisionar rendimiento y escalar elásticamente las base de datos y contenedores de Cosmos. Para escalar globalmente el rendimiento, puede agregar o quitar regiones de su cuenta de Cosmos en cualquier momento. Para obtener más información, consulte cómo agregar o quitar regiones en su cuenta de Cosmos. En muchos escenarios, es importante asociar varias regiones con una cuenta de Cosmos para lograr una latencia baja y [alta disponibilidad](high-availability.md) en todo el mundo.

## <a name="throughput-scaling-details"></a>Detalles de escalado de rendimiento

Si aprovisiona RU de R en un contenedor de Cosmos (o base de datos), Cosmos DB garantiza que las RU de R estarán disponibles en *cada* una de las regiones asociadas con su cuenta de Cosmos. Cada vez que agregue una nueva región a la cuenta de Cosmos, Cosmos DB aprovisiona automáticamente RU de R en la región recién agregada. Se garantiza que las operaciones realizadas en el contenedor de Cosmos obtendrán RU de R en cada una de las regiones asociadas con su cuenta de Cosmos. No se puede asignar de forma selectiva RU en una región específica: las RU aprovisionadas para un contenedor de Cosmos (o base de datos) se aprovisionan para todas las regiones asociadas con la cuenta de Cosmos.

Suponiendo que un contenedor de Cosmos está configurado con RU de R y hay N regiones asociadas con la cuenta de Cosmos, entonces:

- Si la cuenta de Cosmos está configurada con una región de escritura única, el número de RU disponibles globalmente en el contenedor = R x N.
- Si la cuenta de Cosmos está configurada con varias regiones de escritura, el número de RU disponibles globalmente en el contenedor = R x (N+1). Las RU de R adicionales se aprovisionan automáticamente para procesar conflictos de actualización y tráfico de antientropía entre las regiones.

La elección del [modelo de coherencia](consistency-levels.md) también afecta al rendimiento. Aproximadamente, puede obtener el doble de rendimiento de lectura por sesión, prefijo de coherencia y coherencia final en comparación con obsolescencia limitada o coherencia alta.

## <a name="next-steps"></a>Pasos siguientes

A continuación aprenderá a configurar el rendimiento con ayuda del artículo siguiente:

* [Obtener y configurar rendimiento para contenedores y bases de datos](set-throughput.md) 

