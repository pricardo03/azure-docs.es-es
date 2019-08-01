---
title: 'Versiones admitidas de Azure Database for PostgreSQL: servidor único'
description: 'Aquí se describen las versiones admitidas de Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/26/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: fae8dfb2e2cc532f4aed17f83f13bbee31d29113
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68551361"
---
# <a name="supported-postgresql-database-versions"></a>Versiones de base de datos de PostgreSQL admitidas
Microsoft tiene la intención de admitir versiones n-2 del motor de PostgreSQL en el servicio Azure Database for PostgreSQL: servidor único. Estas versiones serían la versión principal actual en Azure (n) y las dos versiones principales anteriores (-2).

Azure Database for PostgreSQL actualmente admite las siguientes versiones:

## <a name="postgresql-version-112"></a>PostgreSQL, versión 11.2
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-2.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-107"></a>PostgreSQL, versión 10.7
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-7.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-9612"></a>PostgreSQL, versión 9.6.12
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-9516"></a>PostgreSQL, versión 9.5.16
Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html) para obtener información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones
Azure Database for PostgreSQL administra automáticamente la aplicación de revisiones de versiones secundarias. Actualmente, no se admite la actualización de versión principal. Por ejemplo, no se admite la actualización de PostgreSQL 9.5 a PostgreSQL 9.6. Si quiere actualizar a la siguiente versión principal, cree un [volcado de base de datos y restáurelo ](./howto-migrate-using-dump-and-restore.md) a un servidor que se haya creado con la nueva versión del motor.

> Tenga en cuenta que antes de la versión 10 de PostgreSQL, la [directiva de versiones de PostgreSQL](https://www.postgresql.org/support/versioning/) consideraba que una actualización de la _versión principal_ suponía un aumento en el primer _o_ segundo número (por ejemplo, de 9.5 a 9.6 se consideró una actualización de la _principal_).
> A partir de la versión 10, solo un cambio en el primer número se considera una actualización de la versión principal (por ejemplo, de 10.0 a 10.1 es una actualización _menor_ de la versión, y 10 a 11 es una actualización de la versión _principal_).

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre la compatibilidad de las distintas extensiones de PostgreSQL, vea [Extensiones de PostgreSQL](concepts-extensions.md).
