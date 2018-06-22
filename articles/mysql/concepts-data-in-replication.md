---
title: Replicar datos en Azure Database for MySQL.
description: En este artículo se describe la replicación de datos internos para Azure Database for MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: 14ed3ef57da28b6929115cf3e5746653d199b140
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263755"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replicar datos en Azure Database for MySQL

La característica Data-in Replication permite sincronizar los datos de un servidor de MySQL que se ejecuta de forma local, en máquinas virtuales o servicios de base de datos hospedados por otros proveedores de nube en el servicio de Azure Database for MySQL. Data-in Replication se basa en la replicación según la posición del archivo de registro binario (binlog) nativa para MySQL. Para obtener más información acerca de la replicación de binlog, consulte la [Introducción a la replicación de binlog de MySQL](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Cuándo utilizar Data-in Replication
Los escenarios principales para considerar el uso de Data-in Replication son:

- **Sincronización de datos híbrida:** con Data-in Replication, se pueden mantener los datos sincronizados entre los servidores locales y Azure Database for MySQL. Esta sincronización resulta útil para crear aplicaciones híbridas. Este método resulta atractivo cuando se tiene un servidor de base de datos local existente, pero quiere mover los datos a una región más cercana de los usuarios finales.
- **Sincronización de multi nube:** para soluciones de nube complejas, use Data-in Replication para sincronizar datos entre Azure Database for MySQL y distintos proveedores de nube, incluidas las máquinas virtuales y servicios de base de datos hospedados en dichas nubes.

## <a name="limitations-and-considerations"></a>Limitaciones y consideraciones

### <a name="data-not-replicated"></a>Datos no replicados
La [*base de datos del sistema mysql* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) del servidor principal no se replica. No se replican los cambios en las cuentas y permisos en el servidor principal. Si crea una cuenta en el servidor principal y esta cuenta debe tener acceso al servidor Réplica, a continuación, cree manualmente la misma cuenta en el lado del servidor Réplica. Para reconocer qué tablas se encuentran en la base de datos del sistema, vea el [manual de MySQL](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Requisitos
- La versión del servidor principal debe ser al menos la versión 5.6 de MySQL. 
- La versión del servidor principal y Réplica deben ser la misma. Por ejemplo, ambas deben ser MySQL versión 5.6 o ambas deben ser MySQL versión 5.7.
- Cada tabla debe tener una clave principal.
- El servidor principal debe usar el motor InnoDB de MySQL.
- El usuario debe tener permisos para configurar el registro binario y crear nuevos usuarios en el servidor principal.

### <a name="other"></a>Otros
- No se admiten identificadores de transacción global (GTID).

## <a name="next-steps"></a>Pasos siguientes
- Información acerca de cómo [configurar la replicación de datos internos](howto-data-in-replication.md)
