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
ms.date: 11/12/2018
ms.openlocfilehash: 04e47e15206e22e7965121c6c277e4032ea04ddf
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621061"
---
# <a name="supported-postgresql-database-versions"></a>Versiones de base de datos de PostgreSQL admitidas
Microsoft tiene la intención de admitir versiones n-2 del motor de PostgreSQL en el servicio Azure Database for PostgreSQL. Estas versiones serían la versión principal actual en Azure (n) y las dos versiones principales anteriores (-2).

Azure Database for PostgreSQL actualmente admite las siguientes versiones:

## <a name="postgresql-version-105"></a>PostgreSQL versión 10.5
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-5.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-9610"></a>PostgreSQL versión 9.6.10
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-10.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-9514"></a>PostgreSQL versión 9.5.14
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-14.html) para obtener información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones
Azure Database for PostgreSQL administra automáticamente la aplicación de revisiones de versiones secundarias. Actualmente, no se admite la actualización de versión principal. Por ejemplo, no se admite la actualización de PostgreSQL 9.5 a PostgreSQL 9.6. Si quiere actualizar a la siguiente versión principal, realice un [volcado y restáurelo ](./howto-migrate-using-dump-and-restore.md) a un servidor que se haya creado con la nueva versión del motor.

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre la compatibilidad de las distintas extensiones de PostgreSQL, vea [Extensiones de PostgreSQL](concepts-extensions.md).
