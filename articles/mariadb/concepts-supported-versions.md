---
title: Versiones admitidas de Azure Database for MariaDB
description: Se describen las versiones admitidas de Azure Database for MariaDB.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: aa83b9955839aaa03aa2ebf46c9e464dc75c8d3a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977509"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Versiones de servidores de Azure Database for MariaDB admitidas
Azure Database for MariaDB se ha desarrollado a partir del [servidor MariaDB](https://downloads.mariadb.org/) de origen abierto, mediante el motor InnoDB. Azure Database for MariaDB actualmente admite las siguientes versiones:

## <a name="mariadb-version-10217"></a>MariaDB versión 10.2.17
Consulte la [documentación de MariaDB](https://downloads.mariadb.org/mariadb/10.2.17/) para obtener más información sobre las mejoras y correcciones de MariaDB 10.2.17.

> [!NOTE]
> En el servicio, se usa una puerta de enlace para redirigir las conexiones a las instancias de servidor. Una vez establecida la conexión, el cliente de MySQL muestra la versión de MariaDB establecida en la puerta de enlace, no la versión real que se ejecuta en la instancia del servidor MariaDB. Para determinar la versión de la instancia del servidor MariaDB, use el comando `SELECT VERSION();` en el símbolo del sistema de MySQL.

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones
El servicio administra automáticamente la aplicación de revisiones para las actualizaciones de versión secundaria.

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre las cuotas y limitaciones aplicables a recursos específicos en función de su **nivel de servicio**, consulte [Niveles de servicio](./concepts-pricing-tiers.md).