---
title: Versiones admitidas de Azure Database for MariaDB
description: Se describen las versiones admitidas de Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 760cb39ea1f3980faba348c7aa1de68a66b20a8d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065721"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Versiones de servidores de Azure Database for MariaDB admitidas

Azure Database for MariaDB se ha desarrollado a partir del [servidor MariaDB](https://downloads.mariadb.org/) de origen abierto, mediante el motor InnoDB. Azure Database for MariaDB actualmente admite las siguientes versiones:

## <a name="mariadb-version-102"></a>MariaDB versión 10.2

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/library/mariadb-10223-release-notes/) para más información sobre las mejoras y correcciones de MariaDB 10.2.23.

## <a name="mariadb-version-103"></a>MariaDB versión 10.3

Consulte la [documentación de MariaDB](https://mariadb.com/kb/en/library/mariadb-10314-release-notes/) para más información sobre las mejoras y correcciones de MariaDB 10.3.14.

> [!NOTE]
> En el servicio, se usa una puerta de enlace para redirigir las conexiones a las instancias de servidor. Una vez establecida la conexión, el cliente de MySQL muestra la versión de MariaDB establecida en la puerta de enlace, no la versión real que se ejecuta en la instancia del servidor MariaDB. Para determinar la versión de la instancia del servidor MariaDB, use el comando `SELECT VERSION();` en el símbolo del sistema de MySQL.

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones

El servicio administra automáticamente la aplicación de revisiones para las actualizaciones de versión secundaria.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre las cuotas y las limitaciones aplicables a recursos específicos en función de su **nivel de servicio**, consulte [Niveles de servicio](./concepts-pricing-tiers.md).
