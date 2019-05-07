---
title: 'Nodos de Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)'
description: Los dos tipos de nodos en un grupo de servidores.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: c6b948ed63f43f1597103d123be5ed39f42bd276
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077281"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Nodos de Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)

El hiperescala (Citus) (versión preliminar) en el tipo de hospedaje permite base de datos de Azure para servidores de PostgreSQL (denominados nodos) para coordinar entre sí en una arquitectura de ««nada compartido». Los nodos de un grupo de servidores colectivamente almacenar más datos y usan más núcleos de CPU que sería posible en un único servidor. La arquitectura también permite escalar agregando más nodos al grupo de servidores de la base de datos.

## <a name="coordinator-and-workers"></a>Coordinador y los trabajadores

Cada grupo de servidores tiene un nodo coordinador y varios trabajos. Las aplicaciones envían sus consultas al nodo coordinador, que lo retransmite a los trabajos pertinentes y acumula los resultados. Las aplicaciones no son capaz de conectarse directamente a los trabajadores.

Para cada consulta, el coordinador lo enruta a un único nodo de trabajo o ejecuta en paralelo en las diferentes dependiendo de si los datos necesarios se encuentren en un nodo único o múltiple. El coordinador decide qué hacer consultando las tablas de metadatos. Estas tablas realizar un seguimiento de los nombres DNS y el mantenimiento de nodos de trabajo y la distribución de datos entre nodos.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre cómo almacenan nodos [datos distribuidos](concepts-hyperscale-distributed-data.md)
