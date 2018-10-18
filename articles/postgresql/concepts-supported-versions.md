---
title: Versiones admitidas de Azure Database for PostgreSQL
description: Se describen las versiones admitidas de Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/07/2018
ms.openlocfilehash: f337679279f4a3eb6d01808f15a59fee4c8f3cd1
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630600"
---
# <a name="supported-postgresql-database-versions"></a>Versiones de base de datos de PostgreSQL admitidas
Microsoft pretende agregar la compatibilidad de las versiones n-2 del motor de PostgreSQL en el servicio Azure Database for PostgreSQL, donde n se corresponde con la versión principal lanzada actualmente y -2 con las dos versiones principales anteriores.

Azure Database for PostgreSQL actualmente admite las siguientes versiones:

## <a name="postgresql-version-104"></a>PostgreSQL versión 10.4
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-4.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-969"></a>PostgreSQL versión 9.6.9
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-9.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-9513"></a>PostgreSQL versión 9.5.13
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-13.html) para obtener información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones
Azure Database for PostgreSQL administra automáticamente la aplicación de revisiones para las actualizaciones de versión secundaria. Actualmente, no se admite la actualización de versión principal. Por ejemplo, no se admite la actualización de PostgreSQL 9.5 a PostgreSQL 9.6. Si quiere actualizar a la siguiente versión principal, realice un [volcado de memoria y restáurelo ](./howto-migrate-using-dump-and-restore.md) a un servidor que se haya creado con la nueva versión del motor.

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre la compatibilidad de las distintas extensiones de PostgreSQL, vea [Extensiones de PostgreSQL](concepts-extensions.md).
