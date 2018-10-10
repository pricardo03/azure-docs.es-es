---
title: Ventajas clave de la distribución global de Azure Cosmos DB
description: Información acerca de la arquitectura multimaestro de Azure Cosmos DB con las ventajas clave que ofrece la replicación geográfica, la arquitectura multimaestro y los casos de uso donde es útil.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 11d70a648bfc1882753688e5352835b04cee6b9f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968360"
---
# <a name="distribute-data-globally-with-azure-cosmos-db"></a>Distribución global de los datos con Azure Cosmos DB

Este artículo describe las ventajas clave de la distribución global de datos y algunos escenarios en tiempo real en los que la distribución de datos globales es necesaria.

## <a name="key-benefits"></a>Ventajas principales

Estas son las ventajas clave disponibles para las cuentas que aprovechan las funcionalidades de distribución de datos globales de Azure Cosmos DB:

* **Latencia de un solo dígito**: Azure Cosmos DB ofrece < 10 ms de latencia de lectura y escritura en el percentil 99, una latencia garantizada por [contratos de nivel de servicio con respaldo financiero](https://azure.microsoft.com/support/legal/sla/cosmos-db/).

* **"Cinco nueves" de disponibilidad**: Azure Cosmos DB ofrece del 99,999 % de disponibilidad de lectura y escritura, garantizada por [contratos de nivel de servicio con respaldo financiero](https://azure.microsoft.com/support/legal/sla/cosmos-db/).

* **Resolución de conflictos flexible**: para los clientes que aprovechan las capacidades de la arquitectura multimaestro, Azure Cosmos DB ofrece tres modos para controlar los conflictos que garantizan la coherencia e integridad de los datos globales.

* **Coherencia ajustable**: Azure Cosmos DB admite 5 [niveles de coherencia](consistency-levels.md) diferentes con la distribución global en mente, con compatibilidad para todos, pero con coherencia fuerte para las cuentas con funcionalidad de arquitectura multimaestro.

* **Tolerancia a errores implícita** : con datos replicados en varias regiones, las aplicaciones pueden disfrutar de alta tolerancia a errores frente a interrupciones regionales.

## <a name="use-cases"></a>Casos de uso

Aquí se presenta solo una pequeña muestra de los escenarios que pueden aprovechar las capacidades de distribución global de Azure Cosmos DB.

* **Las aplicaciones de medios sociales**: las aplicaciones de medios sociales requieren baja latencia, alta disponibilidad y escalabilidad para proporcionar una gran experiencia para los usuarios ubicados en todo el mundo.

* **IoT** : las implementaciones de Edge geográficamente distribuidas necesitan con frecuencia realizar el seguimiento de los datos de series temporales desde varias ubicaciones. Cada dispositivo se puede asignar a su región más cercana. Cuando los dispositivos se mueven a ubicaciones diferentes, se puede cambiar su asignación para que escriban en la región más cercana disponible.

* **Comercio electrónico** : el comercio electrónico requiere muy baja latencia, así como alta disponibilidad. La distribución geográfica de datos con lecturas y escrituras pone los datos más cercana de los usuarios, aumentando la capacidad de respuesta de las aplicaciones. Disponibilidad para lecturas y escrituras en varias regiones proporciona una mayor disponibilidad.

* **Detección de fraudes y anomalías** : con frecuencia, las aplicaciones que supervisan la actividad del usuario o de la cuenta están distribuidas globalmente y tienen que mantener el seguimiento de varios eventos a la vez para actualizar las puntuaciones que mantienen las métricas de riesgo alineadas.

* **Medición** : el recuento y la regulación del uso (como llamadas API, transacciones por segundo, minutos usados) se pueden implementar globalmente de forma fácil con la funcionalidad de arquitectura multimaestro de Azure Cosmos DB. La resolución de conflictos integrada garantiza la exactitud de los recuentos y la regulación en tiempo real.

## <a name="next-steps"></a>Pasos siguientes  

En este artículo, ha aprendido acerca de los beneficios clave y casos de uso de las funcionalidades de distribución de datos globales de Azure Cosmos DB. A continuación, eche un vistazo a los siguientes recursos:

* [Cómo Azure Cosmos DB permite la distribución global inmediata](distribute-data-globally-turnkey.md)

* [Cómo configurar la replicación global de bases de datos de Azure Cosmos DB](tutorial-global-distribution-sql-api.md)

* [Cómo habilitar la arquitectura multimaestro para cuentas de Azure Cosmos DB](enable-multi-master.md)

* [Cómo funcionan las conmutaciones por error manuales y automáticas en Azure Cosmos DB](regional-failover.md)

* [Descripción de resolución de conflictos en Azure Cosmos DB](multi-master-conflict-resolution.md)

* [Uso de la arquitectura multimaestro con bases de datos NoSQL de código abierto](multi-master-oss-nosql.md)
