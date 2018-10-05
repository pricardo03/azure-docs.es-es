---
title: Distribución de datos global con Azure Cosmos DB | Microsoft Docs
description: Obtenga más información sobre replicación geográfica, arquitectura multimaestro, conmutación por error y recuperación de datos a escala mundial con bases de datos globales de Azure Cosmos DB, un servicio de base de datos con varios modelos distribuido globalmente.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: 227c243d82665dc533e3bfa6a1fe3e9bb775a262
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408902"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Distribución de datos global con Azure Cosmos DB

Azure es ubicuo, se puede encontrar en más de cincuenta regiones geográficas y se halla en continua expansión. Dada su presencia en todo el mundo y la compatibilidad con la arquitectura multimaestro, una de las funcionalidades únicas que Azure ofrece a los desarrolladores es la posibilidad de compilar, implementar y administrar fácilmente aplicaciones distribuidas globalmente.

[Azure Cosmos DB](../cosmos-db/introduction.md) es un servicio de base de datos con varios modelos y distribución global de Microsoft para aplicaciones críticas. Azure Cosmos DB ofrece una distribución global inmediata, [escalado elástico de rendimiento y almacenamiento](../cosmos-db/partition-data.md) en todo el mundo, latencias de lecturas y escrituras inferiores a 10 milisegundos en el percentil 99, [modelos de coherencia bien definidos](consistency-levels.md) y alta disponibilidad garantizada, todo ello respaldado por [Acuerdos de Nivel de Servicio líderes del sector](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [indexa datos automáticamente todos sus datos](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) sin que haya que ocuparse de la administración de esquemas ni de índices.

## <a name="global-distribution-with-multi-master"></a>Distribución global con arquitectura multimaestro

Al ser un servicio en la nube, Azure Cosmos DB se ha diseñado cuidadosamente para admitir funcionalidad multiinquilino, distribución global y arquitectura multimaestro para modelos de datos de documentos, pares clave-valor, grafos y en columnas.

![Contenedor de Azure Cosmos DB particionado y distribuido en tres regiones](./media/distribute-data-globally/global-apps.png)

**Un solo contenedor de Azure Cosmos DB particionado y distribuido en varias regiones de Azure**

Como hemos aprendido al crear Azure Cosmos DB, la incorporación de distribución global no es algo que se pueda hacer en el último momento. No se puede "atornillar" sobre un sistema de base de datos de "único sitio". Las funcionalidades que ofrece una base de datos distribuida globalmente superan las de una recuperación ante desastres geográficos (Geo-DR) tradicional que ofrecen las bases de datos de "sitio único". Las bases de datos de sitio único que ofrecen la funcionalidad de Geo-DR son un estricto subconjunto de las bases de datos distribuidas globalmente.

Con la distribución global inmediata de Azure Cosmos DB, los desarrolladores no tienen que compilar su propia scaffolding (generación automática de interfaz de usuario) de replicación mediante el empleo del patrón Lambda en el registro de base de datos o la realización de "escrituras dobles" en varias regiones. Estos enfoques *no* se recomiendan, ya que es imposible garantizar su corrección y proporcionar Acuerdos de Nivel de Servicio sólidos.

## <a id="Next Steps"></a>Pasos siguientes

* [Cómo Azure Cosmos DB permite la distribución global inmediata](distribute-data-globally-turnkey.md)

* [Ventajas clave de la distribución global de Azure Cosmos DB](distribute-data-globally-benefits.md)

* [Cómo configurar la replicación global de bases de datos de Azure Cosmos DB](tutorial-global-distribution-sql-api.md)

* [Cómo habilitar la arquitectura multimaestro para cuentas de Azure Cosmos DB](enable-multi-master.md)

* [Cómo funcionan las conmutaciones por error manuales y automáticas en Azure Cosmos DB](regional-failover.md)

* [Descripción de resolución de conflictos en Azure Cosmos DB](multi-master-conflict-resolution.md)

* [Uso de la arquitectura multimaestro con las bases de datos NoSQL de código abierto](multi-master-oss-nosql.md)
