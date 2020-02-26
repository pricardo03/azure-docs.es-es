---
title: Escalado elástico con Cassandra API en Azure Cosmos DB
description: Conozca las opciones disponibles para escalar una cuenta de Cassandra API de Azure Cosmos DB y sus ventajas y desventajas
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: thvankra
ms.openlocfilehash: e2967a6d12fba2d81dad9de31e7476a027a39d1c
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77468837"
---
# <a name="elastically-scale-an-azure-cosmos-db-cassandra-api-account"></a>Escalado elástico de una cuenta de Cassandra API de Azure Cosmos DB

Hay varias opciones para explorar la naturaleza elástica de la API de Azure Cosmos DB para Cassandra. Para comprender cómo realizar un escalado eficaz en Azure Cosmos DB, es importante comprender cómo aprovisionar la cantidad correcta de unidades de solicitud (RU/s) para tener en cuenta las demandas de rendimiento del sistema. Para más información sobre las unidades de solicitud, consulte el artículo [Unidades de solicitud](request-units.md). 

![Unidades de solicitud consumidas por operaciones de base de datos](./media/request-units/request-units.png)

## <a name="handling-rate-limiting-429-errors"></a>Administración de la limitación de frecuencia (errores 429)

Azure Cosmos DB devolverá errores de frecuencia limitada (429) si los clientes consumen más recursos (RU/s) que la cantidad que se ha aprovisionado. Cassandra API de Azure Cosmos DB traslada estas excepciones como errores sobrecargados en el protocolo nativo de Cassandra. 

Si el sistema no es sensible a la latencia, puede que baste con administrar la limitación de frecuencia del rendimiento mediante reintentos. Consulte el [ejemplo de código de Java](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) para saber cómo administrar la limitación de frecuencia de forma transparente mediante la [extensión de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-cassandra-extensions) para la [directiva de reintentos de Cassandra](https://docs.datastax.com/en/developer/java-driver/4.4/manual/core/retries/) en Java. También puede usar la [extensión de Spark](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) para administrar la limitación de frecuencia.

## <a name="manage-scaling"></a>Administración del escalado

Si necesita reducir la latencia, existe un espectro de opciones para administrar la escala y aprovisionar el rendimiento (RU) en Cassandra API:

* [Manualmente mediante Azure Portal](#use-azure-portal)
* [Mediante programación con las características del plano de control](#use-control-plane)
* [Mediante programación con comandos de CQL con un SDK específico](#use-cql-queries)
* [Dinámicamente mediante Autopilot](#use-autopilot)

En las siguientes secciones se explican las ventajas y desventajas de cada enfoque. Luego, puede decidir cuál es la mejor estrategia para equilibrar las necesidades de escalado del sistema, el costo general y las necesidades de eficiencia de la solución.

## <a id="use-azure-portal"></a>Uso de Azure Portal

Puede escalar los recursos en la cuenta de Cassandra API de Azure Cosmos DB mediante Azure Portal. Para más información, consulte el artículo sobre el [aprovisionamiento del rendimiento en contenedores y bases de datos](set-throughput.md). En este artículo se explican las ventajas relativas de configurar el rendimiento en el nivel de [base de datos](set-throughput.md#set-throughput-on-a-database) o [contenedor](set-throughput.md#set-throughput-on-a-container) en Azure Portal. Los términos "base de datos" y "contenedor" que se mencionan en estos artículos se asignan a "espacio" y "tabla", respectivamente, en Cassandra API.

La ventaja de este método es que es una forma sencilla de administrar la capacidad de rendimiento de la base de datos. Sin embargo, el inconveniente es que, en muchos casos, el enfoque de escalado puede requerir que ciertos niveles de automatización sean tanto económicos como de alto rendimiento. En las secciones siguientes se explican los escenarios y métodos pertinentes.

## <a id="use-control-plane"></a>Uso del plano de control

La API de Azure Cosmos DB para Cassandra ofrece la posibilidad de ajustar el rendimiento mediante programación con nuestras diversas características de plano de control. Puede encontrar instrucciones y ejemplos en los artículos sobre [Azure Resource Manager](manage-cassandra-with-resource-manager.md), [PowerShell](powershell-samples-cassandra.md) y la [CLI de Azure](cli-samples-cassandra.md).

La ventaja de este método es que se puede automatizar el escalado o la reducción vertical de los recursos en función de un temporizador para tener en cuenta la actividad máxima o los períodos de baja actividad. Eche un vistazo a nuestro ejemplo [aquí](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler) sobre cómo hacerlo mediante Azure Functions y PowerShell.

Uno de los inconvenientes de este enfoque podría ser que no puede responder a las necesidades cambiantes e imprevisibles de escalado en tiempo real. En su lugar, puede que necesite aprovechar el contexto de la aplicación en el sistema, en el nivel de cliente o SDK, o bien mediante [Autopilot](provision-throughput-autopilot.md).

## <a id="use-cql-queries"></a>Uso de consultas de CQL con un SDK específico

Puede escalar el sistema de forma dinámica con código mediante la ejecución de los [comandos CQL ALTER](cassandra-support.md#keyspace-and-table-options) para la base de datos o el contenedor dados.

La ventaja de este enfoque es que permite responder a las necesidades de escalado de forma dinámica y de una manera personalizada que se adapte a su aplicación. Con este enfoque, todavía puede aprovechar los cargos y tarifas estándar de RU/s. Si las necesidades de escalado del sistema son mayormente predecibles (alrededor del 70 % o más), el uso del SDK con CQL puede ser un método más rentable de escalado automático que Autopilot. El inconveniente de este enfoque es que puede ser bastante complejo implementar los reintentos, al tiempo que la limitación de frecuencia puede aumentar la latencia.

## <a id="use-autopilot"></a>Uso de Autopilot

Además de la forma manual o mediante programación de aprovisionar el rendimiento, también puede configurar contenedores de Azure Cosmos en modo Autopilot. El modo Autopilot se escalará automáticamente y al instante hasta sus necesidades de consumo dentro de los intervalos de RU especificados sin poner en peligro los Acuerdos de Nivel de Servicio. Para más información, consulte el artículo [Creación de contenedores y bases de datos de Azure Cosmos en modo Autopilot](provision-throughput-autopilot.md).

La ventaja de este enfoque es que es la forma más fácil de administrar las necesidades de escalado del sistema. Garantiza que no se aplica limitación de frecuencia **dentro de los intervalos de RU configurados**. El inconveniente es que, si las necesidades de escalado en el sistema son predecibles, Autopilot puede ser una manera menos rentable de administrar las necesidades de escalado que el uso del plano de control o el nivel de SDK mencionados anteriormente.

## <a name="next-steps"></a>Pasos siguientes

- Introducción a la [creación de una cuenta de Cassandra API, una base de datos y una tabla](create-cassandra-api-account-java.md) usando una aplicación de Java
