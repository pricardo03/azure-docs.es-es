---
title: Pasos de optimización posteriores a la migración cuando se usa la API de Azure Cosmos DB para MongoDB
description: Este documento proporciona las técnicas de optimización posteriores a la migración de MongoDB a la APi de Azure Cosmos DB para Mongodb.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: roaror
ms.openlocfilehash: c0c761fef481a1fdaa027f1329e9a4e72d62985a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014314"
---
# <a name="post-migration-optimization-steps-when-using-azure-cosmos-dbs-api-for-mongodb"></a>Pasos de optimización posteriores a la migración cuando se usa la API de Azure Cosmos DB para MongoDB 

Después de migrar los datos almacenados en la base de datos de MongoDB a Azure Cosmos DB API para MongoDB, puede conectarse a Azure Cosmos DB y administrar los datos. Esta guía proporciona los pasos que debe considerar después de la migración. Consulte la [MongoDB migrar a la API de Azure Cosmos DB para MongoDB tutorial](../dms/tutorial-mongodb-cosmos-db.md) para conocer los pasos de migración.

En esta guía, aprenderá a:
- [Conectar la aplicación](#connect-account)
- [Optimizar la directiva de indexación](#indexing)
- [Configurar la distribución global de API de Azure Cosmos DB para MongoDB](#distribute-data)
- [Definir el nivel de coherencia](#consistency)

> [!NOTE]
> El paso posterior a la migración sólo es obligatorio en el nivel de aplicación está cambiando la cadena de conexión en la aplicación para que apunte a la nueva cuenta de Azure Cosmos DB. Todos los demás pasos de migración se recomiendan las optimizaciones.
>

## <a id="connect-account"></a>Conectar la aplicación 

1. En un nuevo inicio de sesión de ventana en la [portal de Azure](https://www.portal.azure.com/)
2. Desde el [portal de Azure](https://www.portal.azure.com/), en el panel izquierdo abierto el **todos los recursos** menú y buscar la cuenta de Azure Cosmos DB a la que se han migrado los datos.
3. Abra el **cadena de conexión** hoja. El panel derecho contiene toda la información que necesita para conectarse correctamente a la cuenta.
4. Use la información de conexión en la configuración de la aplicación (o en otros lugares pertinentes) para reflejar la API de Azure Cosmos DB para la conexión de MongoDB en la aplicación. 
![Connection-String](./media/mongodb-post-migration/connection-string.png)

Para obtener más información, consulte el [conectar una aplicación de MongoDB a Azure Cosmos DB](connect-mongodb-account.md) página.

## <a id="indexing"></a>Optimizar la directiva de indexación

Todos los campos de datos se indexan automáticamente, de forma predeterminada, durante la migración de datos a Azure Cosmos DB. En muchos casos, esta directiva de indexación predeterminada es aceptable. En general, quitando los índices optimiza las solicitudes de escritura y tener el valor predeterminado (es decir, la indexación automática) de directiva de indexación optimiza las solicitudes de lectura.

Para obtener más información sobre la indexación, consulte [indexación de datos en la API de Azure Cosmos DB para MongoDB](mongodb-indexing.md) , así como la [indexación en Azure Cosmos DB](index-overview.md) artículos.

## <a id="distribute-data"></a>Distribuya globalmente sus datos

Azure Cosmos DB está disponible en todos los [regiones de Azure](https://azure.microsoft.com/regions/#services) en todo el mundo. Después de seleccionar el nivel de coherencia predeterminado para la cuenta de Azure Cosmos DB, puede asociar una o varias regiones de Azure (dependiendo de sus necesidades de distribución global). Para alta disponibilidad y continuidad del negocio, se recomienda siempre que se ejecuta en al menos 2 regiones. Puede revisar las sugerencias para [optimizar el costo de las implementaciones de varias regiones de Azure Cosmos DB](optimize-cost-regions.md).

Distribuya globalmente sus datos, vea [distribuir datos globalmente en Azure Cosmos DB API para MongoDB](tutorial-global-distribution-mongodb.md). 

## <a id="consistency"></a>Definir el nivel de coherencia
Azure Cosmos DB ofrece 5 bien definidos [niveles de coherencia](consistency-levels.md). Para obtener información acerca de la asignación entre los niveles de coherencia de MongoDB y Azure Cosmos DB, leer [niveles de coherencia y la API de Azure Cosmos DB](consistency-levels-across-apis.md). El nivel de coherencia predeterminado es el nivel de coherencia de sesión. Cambiar el nivel de coherencia es opcional y puede optimizarlo para la aplicación. Para cambiar el nivel de coherencia mediante Azure portal:

1. Vaya a la **coherencia predeterminada** la hoja de configuración.
2. Seleccione su [nivel de coherencia](consistency-levels.md)

La mayoría de los usuarios dejan su nivel de coherencia en la configuración predeterminada de la coherencia de sesión. Sin embargo, hay [equilibrios de disponibilidad y rendimiento de distintos niveles de coherencia](consistency-levels-tradeoffs.md). 

## <a name="next-steps"></a>Pasos siguientes

* [Conectar una aplicación de MongoDB a Azure Cosmos DB](connect-mongodb-account.md)
* [Conectarse a la cuenta de Azure Cosmos DB mediante Studio 3T](mongodb-mongochef.md)
* [Lee de cómo distribuir globalmente con API de Azure Cosmos DB para MongoDB](mongodb-readpreference.md)
* [Hacer que caduquen datos con la API de Azure Cosmos DB para MongoDB](mongodb-time-to-live.md)
* [Niveles de coherencia en Azure Cosmos DB](consistency-levels.md)
* [Indexación en Azure Cosmos DB](index-overview.md)
* [Unidades de solicitud en Azure Cosmos DB](request-units.md)





