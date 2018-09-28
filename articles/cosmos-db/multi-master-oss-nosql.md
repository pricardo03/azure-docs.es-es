---
title: Uso de la arquitectura multimaestro Azure Cosmos DB con las bases de datos NoSQL de código abierto
description: ''
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ea3a16cf6b5aa4e46ad401e59aacb4d936a7429a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963907"
---
# <a name="using-azure-cosmos-db-multi-master-with-open-source-nosql-databases"></a>Uso de la arquitectura multimaestro Azure Cosmos DB con las bases de datos NoSQL de código abierto

La compatibilidad con la arquitectura multimaestro Azure Cosmos DB es una implementación nativa del servidor que está disponible para todas las ofertas de NoSQL de código abierto compatibles con Cosmos DB. También pueden acceder todos los SDK compatibles con Cosmos DB.

Azure Cosmos DB es el primer servicio en el mundo que ofrece compatibilidad con la arquitectura multimaestro para estas bases de datos de NoSQL de código abierto.

|Modelo  |Soporte técnico  |
|---------|---------|
|MongoDB  | Activo-activo  |
|Grafo  | Activo-activo |
|Cassandra  | Activo-activo   |

## <a name="use-mongodb-clients-with-multi-master"></a>Usar los clientes de MongoDB con la arquitectura multimaestro

La característica de arquitectura multimaestro está habilitada para las cuentas de API de MongoDB de la misma manera que está habilitada para otras API de Azure Cosmos DB. Después de habilitar la arquitectura multimaestro para las cuentas de API de MongoDB, cada instancia de una aplicación cliente puede seleccionar su región preferida para lecturas y escrituras. Desde la perspectiva del controlador de MongoDB, la región preferida se muestra al cliente como la réplica establecida como principal. De esta manera, todas las regiones de la base de datos distribuida pueden actuar como réplica principal. La arquitectura multimaestro de Azure Cosmos DB le permite reducir considerablemente las latencias de escritura para las aplicaciones de MongoDB distribuidas por todo el mundo. 

### <a name="set-the-primary-region"></a>Establecer la región principal

Cada instancia de un cliente de MongoDB puede seleccionar la región principal anexando `@<preferred_primary_region_name>` al campo "nombre de la aplicación" aceptado por el controlador del cliente de MongoDB. La mayoría de los controladores aceptan este campo en la cadena de conexión, como:

`mongodb://fabrikam:KEY@fabrikam.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

Después de conectarse, puede producirse uno de los casos siguientes:

* Si el nombre de la región preferida está disponible como una región de escritura, Azure Cosmos DB la selecciona como la región principal.

* Si no se proporciona el nombre de la región preferida, Azure Cosmos DB selecciona una región de forma predeterminada como la región principal.

* Si se proporciona el nombre de la región preferida, pero no está disponible como una región de escritura para la cuenta de la base de datos, Azure Cosmos DB seleccionará la región de escritura más cercana que esté disponible como la región principal.

Determinados controladores, como el controlador NodeJS, siempre escribirán primero en el host especificado en la cadena de conexión inicial. Para esos controladores, a fin de asegurar que las escrituras se dirigen a la región preferida, además del nombre de la aplicación, debe modificar el nombre DNS dentro de la cadena de conexión para incluir el nombre de la región. Asegúrese de escribir el nombre de la región sin espacios en blanco. Por ejemplo: 

`mongodb://fabrikam:KEY@fabrikam-westus.documents.azure.com:10255/?ssl=true&replicaSet=globaldb&appname=@West US`

### <a name="conflict-resolution-mode"></a>Modo de resolución de conflictos

El modo de resolución de conflictos para las cuentas de API de MongoDB de Azure Cosmos DB siempre consiste en dar prioridad a la que ha escrito en último lugar, para lo cual se utiliza la marca de tiempo del servidor regional que ha aceptado la escritura.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre la compatibilidad con la arquitectura multimaestro para las cuentas de API de MongoDB de Azure Cosmos DB. A continuación, eche un vistazo a los siguientes recursos:

* [Cómo habilitar la arquitectura multimaestro para cuentas de Azure Cosmos DB](enable-multi-master.md)

* [Descripción de resolución de conflictos en Azure Cosmos DB](multi-master-conflict-resolution.md)
