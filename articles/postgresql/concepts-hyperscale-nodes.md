---
title: 'Nodos en Azure Database for PostgreSQL: Hiperescala (Citus) (versión preliminar)'
description: Los dos tipos de nodo en un grupo de servidores.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: c6b948ed63f43f1597103d123be5ed39f42bd276
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077281"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Nodos en Azure Database for PostgreSQL: Hiperescala (Citus) (versión preliminar)

El tipo de hospedaje Hiperescala (Citus) (versión preliminar) permite a los servidores de Azure Database for PostgreSQL (denominados "nodos") coordinar entre sí en una arquitectura de "nada compartido". Los nodos de un grupo de servidores almacenan colectivamente más datos y usan más núcleos de CPU de lo que sería posible en un único servidor. La arquitectura también permite escalar la base de datos al agregar más nodos al grupo de servidores.

## <a name="coordinator-and-workers"></a>Coordinación y trabajos

Cada grupo de servidores tiene un nodo de coordinación y varios trabajos. Las aplicaciones envían sus consultas al nodo de coordinación, que las retransmite a los trabajos pertinentes y acumula los resultados. Las aplicaciones no son capaces de conectarse directamente a los trabajos.

Para cada consulta, el coordinador la enruta a un único nodo de trabajo o la paraleliza en varias según si los datos necesarios se encuentren en un único nodo o en varios. El coordinador decide qué hacer tras consultar las tablas de metadatos. Estas tablas hacen un seguimiento de los nombres DNS y del estado de los nodos de trabajo, y de la distribución de datos entre nodos.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre cómo los nodos almacenan [datos distribuidos](concepts-hyperscale-distributed-data.md)
