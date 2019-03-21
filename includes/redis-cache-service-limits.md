---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 77019068279261a36f8ba40247e5f5694ca77892
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554073"
---
| Recurso | Límite |
| --- | --- |
| Tamaño de memoria caché |530 GB |
| Bases de datos |64 |
| Número máximo de clientes conectado |40.000 |
| Caché de Azure para las réplicas de Redis, para lograr alta disponibilidad |1 |
| Particiones en una caché premium con agrupación en clústeres |10 |

Los tamaños y límite de Azure Cache for Redis son diferentes para cada plan de tarifa. Para ver los planes de tarifa y sus tamaños asociados, consulte [caché de Azure para Redis precios](https://azure.microsoft.com/pricing/details/cache/).

Para información sobre los límites de configuración de Azure Cache for Redis, consulte [Configuración predeterminada del servidor Redis](../articles/azure-cache-for-redis/cache-configure.md#default-redis-server-configuration).

Dado que Microsoft realiza la configuración y la administración de instancias de Azure Cache for Redis, no se admiten todos los comandos de Redis en Azure Cache for Redis. Para más información, consulte [Comandos de Redis que no se admiten en Azure Cache for Redis](../articles/azure-cache-for-redis/cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis).

