---
title: Niveles de coherencia y API de Azure Cosmos DB
description: Descripción de los niveles de coherencia entre las API de Azure Cosmos DB.
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.reviewer: sngun
ms.openlocfilehash: 7a8617ae2b01fc89a4c957b8610164a2b53a16f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60892491"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Niveles de coherencia y API de Azure Cosmos DB

Azure Cosmos DB proporciona compatibilidad nativa con conexión compatible con el protocolo de API para las bases de datos más populares. Estos incluyen Apache Cassandra, MongoDB, Gremlin y Azure Table storage. Estas bases de datos no se ofrece con precisión los modelos de coherencia definidos o basado en SLA garantías para los niveles de coherencia. Suelen proporcionar únicamente un subconjunto de los cinco modelos de coherencia que ofrece Azure Cosmos DB. 

Cuando se usa la API de SQL, API de Gremlin y Table API, se utiliza el nivel de coherencia predeterminado configurado en la cuenta de Azure Cosmos. 

Cuando se usa la API de Cassandra API o Azure Cosmos DB para MongoDB, las aplicaciones obtener un conjunto completo de los niveles de coherencia que ofrece Apache Cassandra y MongoDB, respectivamente, con incluso más fuerte coherencia y garantías de durabilidad. Este documento muestra los niveles de coherencia de Azure Cosmos DB correspondientes para Apache Cassandra y MongoDB niveles de coherencia.


## <a id="cassandra-mapping"></a>Asignación entre niveles de coherencia de Apache Cassandra y Azure Cosmos DB

A diferencia de la base de datos AzureCosmos, Apache Cassandra no proporciona garantías de coherencia definidos con precisión forma nativa.  En su lugar, Cassandra Apache proporciona un nivel de coherencia de escritura y un nivel de coherencia de lectura, para habilitar los compromisos de coherencia, disponibilidad y la latencia alta. Cuando se usa la API Cassandra de Azure Cosmos DB: 

* El nivel de coherencia de escritura de Apache Cassandra se asigna al nivel de coherencia predeterminado configurado en la cuenta de Azure Cosmos. 

* Azure Cosmos DB se asignará dinámicamente el nivel de coherencia de lectura, especificado por el controlador de cliente de Cassandra en uno de los niveles de coherencia de Azure Cosmos DB configurados dinámicamente en una solicitud de lectura. 

En la tabla siguiente se muestra cómo se asignan los niveles de coherencia de Cassandra nativos para los niveles de coherencia de Azure Cosmos DB cuando se usa la API de Cassandra:  

[![Asignación de modelo de coherencia de Cassandra](./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png)](./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png#lightbox)

## <a id="mongo-mapping"></a>La asignación entre los niveles de coherencia de MongoDB y Azure Cosmos DB

A diferencia de Azure Cosmos DB, MongoDB nativa no proporciona garantías de coherencia definidos con precisión. En su lugar, MongoDB nativo permite a los usuarios configurar las siguientes garantías de coherencia: una preocupación de escritura, un problema de lectura y la directiva de isMaster - para dirigir las operaciones de lectura a las réplicas principales o secundarias para lograr el nivel de coherencia deseado. 

Cuando se usa la API de Azure Cosmos DB para MongoDB, el controlador de MongoDB trata la región de escritura como la réplica principal y todas las demás regiones se leen la réplica. Puede elegir qué región asociada con su cuenta de Azure Cosmos como una réplica principal. 

Al usar API de Azure Cosmos DB para MongoDB:

* La preocupación de escritura se asigna al nivel de coherencia predeterminado configurado en la cuenta de Azure Cosmos.
 
* Azure Cosmos DB se asignará dinámicamente la preocupación de lectura especificada por el controlador de cliente de MongoDB a uno de los niveles de coherencia de Azure Cosmos DB se configura dinámicamente en una solicitud de lectura. 

* Puede anotar una región específica asociada con su cuenta de Azure Cosmos como "Maestra" mediante la realización de la región que la primera región de escritura. 

En la tabla siguiente se muestra cómo MongoDB nativo escritura o lectura cuestiones se asignan a los niveles de coherencia de Azure Cosmos cuando se usa la API de Azure Cosmos DB para MongoDB:

[![Asignación de modelo de coherencia de MongoDB](./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png)](./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Lea más acerca de los niveles de coherencia y la compatibilidad entre las API de Azure Cosmos DB con las API de código abierto: Consulte los artículos siguientes:

* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Características de MongoDB que admite la API de Azure Cosmos DB para MongoDB](mongodb-feature-support.md)
* [Características de Apache Cassandra admitidas por Cassandra API de Azure Cosmos DB](cassandra-support.md)