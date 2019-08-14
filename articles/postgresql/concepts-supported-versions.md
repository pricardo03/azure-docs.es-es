---
title: 'Versiones admitidas de Azure Database for PostgreSQL: servidor único'
description: 'Aquí se describen las versiones admitidas de Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/06/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: b77fd082be43b8cbdedf7cbe5875a8931eb0474a
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68837895"
---
# <a name="supported-postgresql-database-versions"></a>Versiones de base de datos de PostgreSQL admitidas
Microsoft tiene la intención de admitir versiones n-2 del motor de PostgreSQL en el servicio Azure Database for PostgreSQL: servidor único. Estas versiones serían la versión principal actual en Azure (n) y las dos versiones principales anteriores (-2).

Azure Database for PostgreSQL actualmente admite las siguientes versiones principales:

## <a name="postgresql-version-11"></a>PostgreSQL, versión 11
La versión secundaria actual es 11.4. Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-4.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-10"></a>PostgreSQL, versión 10
La versión secundaria actual es 10.9. Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-9.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-96"></a>PostgreSQL, versión 9.6
La versión secundaria actual es 9.6.14. Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-14.html) para obtener más información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="postgresql-version-95"></a>PostgreSQL, versión 9.5
La versión secundaria actual es 9.5.18. Consulte la [documentación de PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-18.html) para obtener información sobre las mejoras y correcciones de esta versión secundaria.

## <a name="managing-upgrades"></a>Administración de actualizaciones
Azure Database for PostgreSQL administra automáticamente las actualizaciones de versiones secundarias. 

No se admite la actualización automática de la versión principal. Por ejemplo, no hay una actualización automática de PostgreSQL 95 a PostgreSQL 9.6. Para actualizar a la siguiente versión principal, cree un [volcado de base de datos y restáurelo](./howto-migrate-using-dump-and-restore.md) a un servidor que se haya creado con la nueva versión del motor.

### <a name="version-syntax"></a>Sintaxis de versión
Antes de la versión 10 de PostgreSQL, la [directiva de versiones de PostgreSQL](https://www.postgresql.org/support/versioning/) consideraba que una actualización de la _versión principal_ suponía un aumento en el primer _o_ segundo número. Por ejemplo, de 9.5 a 9.6 se consideraba una actualización de la versión _principal_. A partir de la versión 10, solo se considera una actualización de la versión principal un cambio en el primer número. Por ejemplo, de 10.0 a 10.1 es una actualización de versión _secundaria_. De la versión 10 a la 11 se consideraría una actualización de versión _principal_.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las extensiones de PostgreSQL admitidas,consulte el [documento de extensiones](concepts-extensions.md).
