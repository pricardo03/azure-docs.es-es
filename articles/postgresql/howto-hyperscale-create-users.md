---
title: Crear usuarios en Azure Database for PostgreSQL - Hiperescala (Citus)
description: En este artículo se describe cómo puede crear nuevas cuentas de usuario para interactuar con Azure Database for PostgreSQL - Hiperescala (Citus).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 7187135b29f0a9a790c032330c73bcb1ae27229b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511241"
---
# <a name="create-users-in-azure-database-for-postgresql---hyperscale-citus"></a>Crear usuarios en Azure Database for PostgreSQL - Hiperescala (Citus)

En este artículo se describe cómo puede crear usuarios dentro de un grupo de servidores Hiperescala (Citus). Para obtener información sobre los usuarios de la suscripción Azure y sus privilegios, visite el artículo [sobre el control de acceso basado en el rol (RBAC) de Azure](../role-based-access-control/built-in-roles.md) o revise [cómo personalizar los roles](../role-based-access-control/custom-roles.md).

## <a name="the-server-admin-account"></a>La cuenta de administrador del servidor

Un grupo de servidores de Hiperescala (Citus) recién creado incluye varios roles predefinidos:

* [Roles predeterminados de PostgreSQL](https://www.postgresql.org/docs/current/default-roles.html)
* *azure_pg_admin*
* *postgres*
* *citus*

El usuario administrador del servidor, *citus*, es miembro del rol *azure_pg_admin*.
Sin embargo, no forma parte del rol *postgres* (superusuario).  Dado que Hiperescala es un servicio PaaS administrado, solo Microsoft forma parte de la función de superusuario.

El motor de PostgreSQL usa privilegios para controlar el acceso a objetos de base de datos, como se describe en la [documentación de productos de PostgreSQL](https://www.postgresql.org/docs/current/static/sql-createrole.html).
En Azure Database for PostgreSQL, se le conceden estos privilegios al usuario administrador del servidor: LOGIN, NOSUPERUSER, INHERIT, CREATEDB, CREATEROLE y NOREPLICATION

## <a name="how-to-create-additional-users"></a>Cómo crear usuarios adicionales

La cuenta de administrador *citus* carece de permiso para crear usuarios adicionales. Para agregar un usuario, use Azure Portal en su lugar.

1. Vaya a la página de **Roles** del grupo Servidor de hiperescala y haga clic en **+ Agregar**:

   ![Página de roles](media/howto-hyperscale-create-users/1-role-page.png)

2. Introduzca el nombre y la contraseña del rol. Haga clic en **Save**(Guardar).

   ![Agregar rol](media/howto-hyperscale-create-users/2-add-user-fields.png)

El usuario se creará en el nodo coordinador del grupo de servidores y se propagará a todos los nodos de trabajo.

## <a name="how-to-delete-a-user-or-change-their-password"></a>Cómo eliminar un usuario o cambiar su contraseña

Vaya a la página **Roles** del grupo Servidor de Hiperescala y haga clic en los puntos suspensivos **...**  junto a un usuario. Los puntos suspensivos abrirán un menú para eliminar el usuario o restablecer su contraseña.

   ![Editar un rol](media/howto-hyperscale-create-users/edit-role.png)

El rol *citus* tiene privilegios y no se puede eliminar.

## <a name="how-to-modify-privileges-for-role"></a>Cómo modificar los privilegios para el rol

Las nuevas funciones se usan comúnmente para proporcionar acceso a la base de datos con privilegios restringidos. Para modificar los privilegios de usuario, use los comandos estándar de PostgreSQL con una herramienta como PgAdmin o psql. (Consulte [conectarse con psql](quickstart-create-hyperscale-portal.md#connect-to-the-database-using-psql) en el inicio rápido de la Hiperescala (Citus).)

Por ejemplo, para permitir que *db_user* lea *MyTable*, conceda el permiso:

```sql
GRANT SELECT ON mytable TO db_user;
```

Hiperescala (Citus) propaga las instrucciones GRANT de tabla única a través de todo el clúster y las aplica a todos los nodos de trabajo. Sin embargo, los GRANT que abarcan todo el sistema (por ejemplo, todas las tablas de un esquema) deben ejecutarse en cada nodo de fecha.  Use la función auxiliar *run_command_on_workers ()* :

```sql
-- applies to the coordinator node
GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;

-- make it apply to workers as well
SELECT run_command_on_workers(
  'GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;'
);
```

## <a name="next-steps"></a>Pasos siguientes

Abra el firewall para las direcciones IP de las máquinas de los nuevos usuarios para permitirles conectarse: [Creación y administración de las reglas de firewall en Hiperescala (Citus) con Azure Portal](howto-hyperscale-manage-firewall-using-portal.md).

Para más información sobre la gestión de cuentas de usuario de la base de datos, consulte la documentación del producto PostgreSQL:

* [Roles y privilegios de la base de datos](https://www.postgresql.org/docs/current/static/user-manag.html)
* [Sintaxis GRANT](https://www.postgresql.org/docs/current/static/sql-grant.html)
* [Privilegios](https://www.postgresql.org/docs/current/static/ddl-priv.html)
