---
title: Replicar datos en Azure Database for MySQL.
description: En este artículo se describe la replicación de datos internos para Azure Database for MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/31/2018
ms.openlocfilehash: ca748dff67be2a37ca61f34602f207265cc77aaa
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53544233"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replicar datos en Azure Database for MySQL

Data-in Replication permite sincronizar los datos de un servidor de MySQL que se ejecuta de forma local, en máquinas virtuales o servicios de base de datos hospedados por otros proveedores de nube en el servicio de Azure Database for MySQL. Data-in Replication se basa en la replicación según la posición del archivo de registro binario (binlog), que es nativa de MySQL. Para obtener más información acerca de la replicación de binlog, consulte la [Introducción a la replicación de binlog de MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Cuándo utilizar Data-in Replication
Los escenarios principales para considerar el uso de Data-in Replication son:

- **Sincronización de datos híbridos:** con Replicación de datos de entrada, se pueden mantener los datos sincronizados entre los servidores locales y Azure Database for MySQL. Esta sincronización resulta útil para crear aplicaciones híbridas. Este método resulta atractivo cuando se tiene un servidor de base de datos local existente, pero quiere mover los datos a una región más cercana de los usuarios finales.
- **Sincronización de multi-nube:** para soluciones de nube complejas, use Replicación de datos de entrada para sincronizar datos entre Azure Database for MySQL y distintos proveedores de nube, incluidas las máquinas virtuales y los servicios de base de datos hospedados en dichas nubes.

## <a name="limitations-and-considerations"></a>Limitaciones y consideraciones

### <a name="data-not-replicated"></a>Datos no replicados
La [*base de datos del sistema mysql* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) del servidor maestro no se replica. No se replican los cambios en las cuentas y permisos en el servidor maestro. Si crea una cuenta en el servidor maestro y esta cuenta debe tener acceso al servidor de réplica, cree manualmente la misma cuenta en el servidor de réplica. Para reconocer qué tablas se encuentran en la base de datos del sistema, vea el [manual de MySQL](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Requisitos
- La versión del servidor maestro debe ser al menos la versión 5.6 de MySQL. 
- Las versiones del servidor maestro y de réplica deben ser las mismas. Por ejemplo, ambas deben ser MySQL versión 5.6 o ambas deben ser MySQL versión 5.7.
- Cada tabla debe tener una clave principal.
- El servidor maestro debe usar el motor InnoDB de MySQL.
- El usuario debe tener permisos para configurar el registro binario y crear nuevos usuarios en el servidor maestro.

### <a name="other"></a>Otros
- Replicación de datos de entrada solo se admite en los planes de tarifa De uso general y Optimizada para memoria de los planes de tarifa
- No se admiten identificadores de transacción global (GTID).

## <a name="next-steps"></a>Pasos siguientes
- Información acerca de cómo [configurar la replicación de datos internos](howto-data-in-replication.md)
