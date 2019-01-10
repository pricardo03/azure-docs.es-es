---
title: Replicar datos en Azure Database for MariaDB.
description: En este artículo se describe la replicación de datos internos para Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0a1ead1580f6764fec7d1d18daa38bf093f242f2
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53547616"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>Replicar datos en Azure Database for MariaDB

La Replicación de datos de entrada permite sincronizar los datos de un servidor de MariaDB que se ejecuta de forma local, en máquinas virtuales o servicios de base de datos hospedados por otros proveedores de nube en el servicio de Azure Database for MariaDB. La Replicación de datos de entrada se basa en la replicación según la posición del archivo de registro binario (binlog), que es nativa de MariaDB. Para obtener más información sobre la replicación de binlog, consulte la [introducción a la replicación de binlog](https://mariadb.com/kb/en/library/replication-overview/).

## <a name="when-to-use-data-in-replication"></a>Cuándo utilizar Data-in Replication
Los escenarios principales para considerar el uso de Data-in Replication son:

- **Sincronización de datos híbridos:** con Replicación de datos de entrada, se pueden mantener los datos sincronizados entre los servidores locales y Azure Database for MariaDB. Esta sincronización resulta útil para crear aplicaciones híbridas. Este método resulta atractivo cuando se tiene un servidor de base de datos local existente, pero quiere mover los datos a una región más cercana de los usuarios finales.
- **Sincronización de multi-nube:** para soluciones de nube complejas, use Replicación de datos de entrada para sincronizar datos entre Azure Database for MariaDB y distintos proveedores de nube, incluidas las máquinas virtuales y los servicios de base de datos hospedados en dichas nubes.

## <a name="limitations-and-considerations"></a>Limitaciones y consideraciones

### <a name="data-not-replicated"></a>Datos no replicados
La [*base de datos del sistema mysql* ](https://mariadb.com/kb/en/library/the-mysql-database-tables/) del servidor maestro no se replica. No se replican los cambios en las cuentas y permisos en el servidor maestro. Si crea una cuenta en el servidor maestro y esta cuenta debe tener acceso al servidor de réplica, cree manualmente la misma cuenta en el servidor de réplica. Para reconocer qué tablas se encuentran en la base de datos del sistema, vea la [documentación de MariaDB](https://mariadb.com/kb/en/library/the-mysql-database-tables/).

### <a name="requirements"></a>Requisitos
- La versión del servidor maestro debe ser al menos la versión 10.2 de MariaDB.
- Las versiones del servidor maestro y de réplica deben ser las mismas. Por ejemplo, ambas deben ser la versión 10.2 de MariaDB.
- Cada tabla debe tener una clave principal.
- El servidor maestro debe usar el motor InnoDB.
- El usuario debe tener permisos para configurar el registro binario y crear nuevos usuarios en el servidor maestro.

### <a name="other"></a>Otros
- La Replicación de datos de entrada solo se admite en los planes de tarifa De uso general y Optimizada para memoria.
- No se admiten identificadores de transacción global (GTID).

## <a name="next-steps"></a>Pasos siguientes
- Información sobre cómo [configurar la replicación de datos internos](howto-data-in-replication.md).
