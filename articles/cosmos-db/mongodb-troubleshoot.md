---
title: Solucionar errores comunes de API de Azure Cosmos DB para Mongodb
description: Este documento describe las formas para solucionar los problemas habituales detectados en la API de Azure Cosmos DB para MongoDB.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: roaror
ms.openlocfilehash: 5b3d3993a497240c1ea18f0fcf852c0e834f6e79
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735712"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>Solucionar problemas comunes de API de Azure Cosmos DB para MongoDB

Azure Cosmos DB implementa los protocolos de conexión de bases de datos NoSQL comunes, como MongoDB. Debido a la implementación del protocolo de conexión, puede interactuar transparente con Azure Cosmos DB mediante el SDK de cliente existente, controladores y herramientas que funcionen con bases de datos NoSQL. Azure Cosmos DB no utiliza cualquier código fuente de las bases de datos para proporcionar API compatibles con cable para cualquiera de las bases de datos NoSQL. Cualquier controlador de cliente de MongoDB que entiende las versiones del protocolo de conexión puede conectarse a Azure Cosmos DB.

Aunque las API de Azure Cosmos DB para MongoDB es compatible con la versión 3.2 de protocolo de conexión de MongoDB (los operadores de consulta y las características agregadas en la versión 3.4 disponibles actualmente como versión preliminar), hay algunos códigos de error personalizados que se corresponden con Azure Cosmos DB errores específicos. Este artículo explica los diferentes errores, códigos de error y los pasos para resolver esos errores.

## <a name="common-errors-and-solutions"></a>Errores comunes y soluciones

| Error               | Código  | DESCRIPCIÓN  | Solución  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | El número total de unidades de solicitud consumidas es mayor que la tasa de unidades de solicitud aprovisionadas para la colección y se ha limitado. | Considere la posibilidad de escalar el rendimiento asignado a un contenedor o un conjunto de contenedores desde Azure portal o puede volver a intentar la operación. |
| ExceededMemoryLimit | 16501 | Como se trata de un servicio de varios inquilinos, la operación ha superado la asignación de memoria del cliente. | Reduzca el ámbito de la operación a través de criterios de consulta más restrictivos o póngase en contacto con soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Ejemplo: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| Problemas de versión de conexión de MongoDB | - | Las versiones anteriores de los controladores de MongoDB son no se puede detectar el nombre de la cuenta de Azure Cosmos en las cadenas de conexión. | Anexar *appName = @**accountName** @*  al final de la API de Cosmos DB para la cadena de conexión de MongoDB, donde ***accountName*** es el nombre de cuenta de Cosmos DB . |


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [usar Studio 3T](mongodb-mongochef.md) con la API de Azure Cosmos DB para MongoDB.
- Aprenda a [usar Robo 3T](mongodb-robomongo.md) con la API de Azure Cosmos DB para MongoDB.
- Explore [ejemplos](mongodb-samples.md) de MongoDB con la API de Azure Cosmos DB para MongoDB.

