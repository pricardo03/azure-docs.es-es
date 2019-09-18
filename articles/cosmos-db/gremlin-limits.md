---
title: Límites de Gremlin de Azure Cosmos DB
description: Documentación de referencia para las limitaciones en tiempo de ejecución del motor Graph
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: olignat
ms.openlocfilehash: f1fe4cfac22a651f44338986d0a767fe3e1a360b
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910686"
---
# <a name="azure-cosmos-db-gremlin-limits"></a>Límites de Gremlin de Azure Cosmos DB
En este artículo se tratan los límites del motor de Gremlin de Azure Cosmos DB y se explica cómo pueden afectar a los recorridos de los clientes.

Gremlin de Cosmos DB se basa en la infraestructura de Cosmos DB, por lo que todos los límites de [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/concepts-limits) se siguen aplicando. 

## <a name="limits"></a>límites

Cuando se alcanza el límite de Gremlin, el recorrido se cancela con **x-ms-status-code** = 429, lo que indica un error de limitación.

**Recurso**    | **Límite predeterminado** | **Explicación**
--- | --- | ---
*Memoria por solicitud* | **2 GB** | Memoria máxima que puede consumir una solicitud durante el procesamiento. Las solicitudes que necesiten procesar conjuntos de datos de gran tamaño consumirán memoria adicional. Considere la posibilidad de establecer el ámbito de las solicitudes en conjuntos de datos más pequeños para evitar cruzar este límite o usar soluciones OLAP.
*Longitud del script* | **64 KB** | Longitud máxima de un script de recorrido de Gremlin por solicitud.
*Profundidad del operador* | **400** |  Número total de pasos únicos en un recorrido. Por ejemplo, ```g.V().out()``` tiene un recuento de operadores de 2: V() y out(); ```g.V('label').repeat(out()).times(100)``` tiene una profundidad de operadores de 3: V(), repeat() y out(), porque ```.times(100)``` es un parámetro para el operador ```.repeat()```.
*Grado de paralelismo* | **32** | Número máximo de particiones de almacenamiento consultadas en una única solicitud a la capa de almacenamiento. Este límite afectará a los grafos con cientos de particiones.
*Límite de repeticiones* | **32** | Número máximo de iteraciones que puede ejecutar un operador ```.repeat()```. En la mayoría de los casos, cada iteración del paso ```.repeat()``` se ejecuta mediante recorridos centrados en la amplitud, lo que significa que cualquier recorrido se limita a un máximo de 32 saltos entre vértices.
*Tiempo de espera del recorrido* | **30 segundos** | El recorrido se cancelará cuando supere este tiempo. Cosmos DB Graph es una base de datos OLTP en la que la mayoría de los recorridos se completan en cuestión de milisegundos. Para realizar consultas OLAP en Cosmos DB Graph, use [Apache Spark](https://azure.microsoft.com/services/cosmos-db/) con [tramas de datos de grafos](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes) y el [conector de Spark de Cosmos DB](https://github.com/Azure/azure-cosmosdb-spark).
*Límite de predicados* | **20** | Recuento de pasos ```.has()``` o ```.hasNot()``` aplicados en un solo vértice o borde. Cuando se alcanza este límite, el mensaje de error que aparece en la aplicación es ```The SQL query exceeded the maximum number of joins. The allowed limit is 20```. Este es un inconveniente temporal, ya que el equipo está trabajando para superar este límite. 
*Tiempo de espera de la conexión inactiva* | **5 horas** | Cantidad de tiempo que el servidor de Graph mantendrá abierta la conexión de WebSocket sin necesidad de tráfico. Los paquetes persistentes de TCP o las solicitudes persistentes de HTTP no extienden la duración de la conexión más allá de este límite, pero si no se envían, la infraestructura subyacente de Azure puede cerrar la conexión incluso antes. Se considera que el motor Cosmos DB Graph está inactivo si no hay ningún recorrido de Gremlin en ejecución.
*Token de recurso por hora* | **100** | Número de tokens de recursos únicos que usan los clientes de Gremlin para conectarse a la cuenta de Gremlin en una región. Cuando la aplicación supera el límite de tokens único por hora, `"Exceeded allowed resource token limit of 100 that can be used concurrently"` se devolverá en la siguiente solicitud de autenticación.

## <a name="next-steps"></a>Pasos siguientes
* [Encabezados de respuesta de Gremlin en Azure Cosmos DB](gremlin-headers.md) 
* [Tokens de recursos de Azure Cosmos DB con Gremlin](how-to-use-resource-tokens-gremlin.md)
