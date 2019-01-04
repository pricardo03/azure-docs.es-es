---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 71d19b5d922616944c176cdda98470607c515af9
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111854"
---
| Recurso | Límite |
| --- | --- |
| Tamaño de memoria caché |530 GB |
| Bases de datos |64 |
| Número máximo de clientes conectados |40.000 |
| Réplicas de Azure Cache for Redis (para alta disponibilidad) |1 |
| Particiones en una caché premium con agrupación en clústeres |10 |

Los tamaños y límite de Azure Cache for Redis son diferentes para cada plan de tarifa. Para ver los planes de tarifa y sus tamaños asociados, consulte los [precios de Azure Cache for Redis](https://azure.microsoft.com/pricing/details/cache/).

Para información sobre los límites de configuración de Azure Cache for Redis, consulte [Configuración predeterminada del servidor Redis](../articles/azure-cache-for-redis/cache-configure.md#default-redis-server-configuration).

Dado que Microsoft realiza la configuración y la administración de instancias de Azure Cache for Redis, no se admiten todos los comandos de Redis en Azure Cache for Redis. Para más información, consulte [Comandos de Redis que no se admiten en Azure Cache for Redis](../articles/azure-cache-for-redis/cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis).

