---
title: Solución de problemas de Redis Cache | Microsoft Docs
description: Más información sobre cómo resolver problemas de pérdida de datos con Redis Cache
services: cache
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 10/17/2019
ms.author: yegu
ms.openlocfilehash: 507a8aef36e4ea5db0b4fdecb54876158398d96a
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795248"
---
# <a name="troubleshoot-data-loss"></a>Solución de problemas de pérdidas de datos

En esta sección se describe cómo diagnosticar pérdidas de datos reales o percibidas que se pueden producir en Redis Cache.

- [Pérdida parcial de claves](#partial-loss-of-keys)
- [Pérdida principal o completa de claves](#major-or-complete-loss-of-keys)

> [!NOTE]
> Algunos de los pasos de solución de problemas de esta guía incluyen instrucciones para ejecutar comandos de Redis y supervisar diversas métricas de rendimiento. Para más información e instrucciones, consulte los artículos de la sección [Información adicional](#additional-information) .
>

## <a name="partial-loss-of-keys"></a>Pérdida parcial de claves

Redis no elimina aleatoriamente las claves una vez que se han almacenado en la memoria. Aunque quitará las claves en respuesta a las directivas de expiración o expulsión, así como a los comandos de eliminación de claves explícita. Además, es posible que las claves que se hayan escrito en el nodo maestro en una instancia Premium o Estándar de Redis Cache no estén disponibles inmediatamente en una réplica. Los datos se replican desde el maestro a la réplica de forma asincrónica y sin bloqueo.

Si observa que las claves han desaparecido de la caché, puede comprobar lo siguiente para ver cuál puede ser la causa:

| Causa | DESCRIPCIÓN |
|---|---|
| [Expiración de la clave](#key-expiration) | Las claves se quitan debido a los tiempos de expiración establecidos |
| [Expulsión de la clave](#key-eviction) | Las claves se quitan en la presión de memoria |
| [Eliminación de la clave](#key-deletion) | Las claves se quitan mediante comandos de eliminación explícita |
| [Replicación asincrónica](#async-replication) | Las claves no están disponibles en una réplica debido a retrasos de replicación de datos |

### <a name="key-expiration"></a>Expiración de la clave

Redis quitará automáticamente una clave a la que se le asigna un tiempo de expiración y ese período ha pasado. Puede encontrar más detalles sobre la expiración de las claves de Redis en la documentación del comando [EXPIRE](http://redis.io/commands/expire). Los valores de tiempo de expiración también se pueden establecer a través de [SET](http://redis.io/commands/set), [SETEX](https://redis.io/commands/setex), [GETSET](https://redis.io/commands/getset)y otros comandos \*STORE.

Puede usar el comando [INFO](http://redis.io/commands/info) para obtener estadísticas sobre cuántas claves han expirado. La sección *Estadísticas* muestra el número total de claves expiradas. La sección *Espacio de claves* proporciona información adicional sobre el número de claves con tiempos de expiración y el valor de tiempo de expiración promedio.

```
# Stats

expired_keys:46583

# Keyspace

db0:keys=3450,expires=2,avg_ttl=91861015336
```

Además, puede consultar las métricas de diagnóstico de la memoria caché para ver si hay una correlación entre el momento en que falta la clave y un pico en las claves expiradas. Consulte el [Anexo](https://gist.github.com/JonCole/4a249477142be839b904f7426ccccf82#appendix) para obtener información sobre el uso de notificaciones de Keyspace o MONITOR para depurar este tipo de problemas.

### <a name="key-eviction"></a>Expulsión de la clave

Redis requiere espacio en memoria para almacenar datos. Purgará las claves para liberar memoria disponible cuando sea necesario. Cuando los valores **used_memory** o **used_memory_rss** del comando [INFO](http://redis.io/commands/info) se aproximen al valor **maxmemory** configurado, Redis comenzará a expulsar las claves de la memoria en función de la [directiva de caché](http://redis.io/topics/lru-cache).

Puede supervisar el número de claves expulsadas con el comando [INFO](http://redis.io/commands/info).

```
# Stats

evicted_keys:13224
```

Además, puede consultar las métricas de diagnóstico de la memoria caché para ver si hay una correlación entre el momento en que falta la clave y un pico en las claves expulsadas. Consulte el [Anexo](https://gist.github.com/JonCole/4a249477142be839b904f7426ccccf82#appendix) para obtener información sobre el uso de notificaciones de Keyspace o MONITOR para depurar este tipo de problemas.

### <a name="key-deletion"></a>Eliminación de la clave

Los clientes de Redis pueden emitir el comando [DEL](http://redis.io/commands/del) o [HDEL](http://redis.io/commands/hdel) para quitar claves de Redis explícitamente. Puede realizar un seguimiento del número de operaciones de eliminación mediante el comando [INFO](http://redis.io/commands/info). Si se ha llamado a los comandos DEL o HDEL, aparecerán en la sección *Commandstats* (Estadísticas de comandos).

```
# Commandstats

cmdstat_del:calls=2,usec=90,usec_per_call=45.00

cmdstat_hdel:calls=1,usec=47,usec_per_call=47.00
```

### <a name="async-replication"></a>Replicación asincrónica

Cualquier instancia de Redis Cache en el nivel Estándar o Premium se configura con un nodo maestro y al menos una réplica. Los datos se copian desde el maestro a una réplica de forma asincrónica mediante un proceso en segundo plano. En el sitio web [redis.io](http://redis.io/topics/replication) se describe cómo funciona la replicación de datos de Redis en general. En los escenarios en los que los clientes escriben en Redis con frecuencia, se puede producir una pérdida parcial de datos debido al hecho de que se garantiza que esta replicación será instantánea. Por ejemplo, si el maestro deja de estar disponible _después_ de que un cliente escriba una clave en él pero _antes_ de que el proceso en segundo plano tenga ocasión de enviar esta clave a la réplica, la clave se perderá cuando la réplica tome el control como nuevo maestro.

## <a name="major-or-complete-loss-of-keys"></a>Pérdida principal o completa de claves

Si observa que la mayoría de las claves ha desaparecido de la caché, puede comprobar lo siguiente para ver cuál puede ser la causa:

| Causa | DESCRIPCIÓN |
|---|---|
| [Vaciado de la clave](#key-flushing) | Las claves se han purgado manualmente |
| [Selección incorrecta de la base de datos](#incorrect-database-selection) | Redis está configurado para usar una base de datos no predeterminada |
| [Error de instancia de Redis](#redis-instance-failure) | Las claves se quitan mediante comandos de eliminación explícita |

### <a name="key-flushing"></a>Vaciado de la clave

Los clientes pueden llamar al comando [FLUSHDB](http://redis.io/commands/flushdb) para quitar todas las claves de una **sola** base de datos o [FLUSHALL](http://redis.io/commands/flushall) para quitar todas las claves de **todas** las bases de datos de una caché en Redis. Puede averiguar si las claves se han vaciado mediante el comando [INFO](http://redis.io/commands/info). En la sección *Commandstats* (Estadísticas de comandos) se mostrará si se ha llamado a alguno de los comandos FLUSH.

```
# Commandstats

cmdstat_flushall:calls=2,usec=112,usec_per_call=56.00

cmdstat_flushdb:calls=1,usec=110,usec_per_call=52.00
```

### <a name="incorrect-database-selection"></a>Selección incorrecta de la base de datos

Redis Cache utiliza la base de datos **db0** de manera predeterminada. Si cambia a otra base de datos (por ejemplo, db1) e intenta leer las claves de ella, Redis no las encontrará allí porque cada base de datos es una unidad independiente lógicamente y contiene un conjunto de datos diferente. Use el comando [SELECT](http://redis.io/commands/select) para usar otras bases de datos disponibles y buscar claves en cada una de ellas.

### <a name="redis-instance-failure"></a>Error de instancia de Redis

Redis es un almacén de datos en memoria. Los datos se mantienen en las máquinas físicas o virtuales que hospedan Redis. Una instancia de Redis Cache en el nivel Básico se ejecuta en una sola máquina virtual (VM). Si esa máquina virtual deja de estar disponible, se pierden todos los datos que ha almacenado en la caché. Las memorias caché de los niveles Estándar y Premium ofrecen una resistencia mucho mayor frente a la pérdida de datos mediante el uso de dos máquinas virtuales en una configuración replicada. Cuando se produce un error en el nodo maestro de dicha caché, el nodo de réplica se encargará de servir los datos automáticamente. Estas máquinas virtuales se encuentran en dominios de error y de actualización independientes para minimizar la posibilidad de que ambas no estén disponibles al mismo tiempo. Aunque, en caso de que se produzca una interrupción importante del centro de datos, las máquinas virtuales pueden dejar de estar disponibles a la vez. Los datos se perderán en estos casos excepcionales.

Debería plantearse la posibilidad de utilizar [Permanencia de datos de Redis](http://redis.io/topics/persistence) y [replicación geográfica](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-geo-replication) para mejorar la protección de sus datos contra estos fallos de infraestructura.

## <a name="additional-information"></a>Información adicional

- [Solución de problemas del lado servidor de Redis Cache](cache-troubleshoot-server.md)
- [¿Qué oferta y tamaño de Azure Cache for Redis debo utilizar?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [Supervisión de Azure Cache for Redis](cache-how-to-monitor.md)
- [¿Cómo puedo ejecutar comandos de Redis?](cache-faq.md#how-can-i-run-redis-commands)
