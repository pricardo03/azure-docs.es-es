---
title: Azure Database for MariaDB controladores y compatibilidad de las herramientas de administración
description: En este artículo se describe los controladores de MariaDB y herramientas de administración que son compatibles con Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: 7a3d9a5f87a565625052fc54e3ecccc99fd928a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386830"
---
# <a name="mariadb-drivers-and-management-tools-compatible-with-azure-database-for-mariadb"></a>MariaDB controladores y herramientas de administración compatibles con Azure Database for MariaDB

En este artículo se describe los controladores y herramientas de administración que son compatibles con Azure Database for MariaDB.

## <a name="mariadb-drivers"></a>Controladores de MariaDB

Azure Database for MariaDB usa la edición community de servidor MariaDB. Por lo tanto, es compatible con una amplia variedad de controladores y lenguajes de programación. API y el protocolo de MariaDB son compatibles con las que usan MySQL. Esto significa que los conectores que funcionan con MySQL deben funcionar también con MariaDB.

El objetivo es compatible con las tres versiones más recientes MariaDB controladores y con esfuerzos con los autores de la Comunidad de código abierto para mejorar constantemente la funcionalidad y facilidad de uso de los controladores de MariaDB. En la tabla siguiente se proporciona una lista de controladores que se han probado y que resultaron para ser compatibles con Azure Database for MariaDB 10.2:

**Controlador** | **Vínculos** | **Versiones compatibles** | **Versiones incompatibles** | **Notas**
---|---|---|---|---
PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | Para la conexión PHP 7.0 con SSL MySQLi, agregue MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT en la cadena de conexión. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO: establezca la opción ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` en false.
.NET | [MySqlConnector en GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Paquete de instalación de NuGet](https://www.nuget.org/packages/MySqlConnector/) | 0.27 y posterior | 0.26.5 y anterior |
Conector MySQL/NET | [Conector MySQL/NET](https://github.com/mysql/mysql-connector-net) | 8.0, 7.0, 6.10 |  | Un error de codificación puede provocar errores de conexión en algunos sistemas Windows que no sean UTF8.
Node.js |  [MySQLjs en GitHub](https://github.com/mysqljs/mysql/) <br> Paquete de instalación de NPM:<br> Ejecute `npm install mysql` en NPM | 2.15 | 2.14.1 y anterior
GO | https://github.com/go-sql-driver/mysql/releases | 1.3, 1.4 | 1.2 y anterior | Use `allowNativePasswords=true` en la cadena de conexión para la versión 1.3. La versión 1.4 contiene una corrección y `allowNativePasswords=true` ya no es necesario.
Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 y anterior |
Java | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | 1.5.5 y anterior |

## <a name="management-tools"></a>Herramientas de administración

La ventaja de compatibilidad se amplía también a las herramientas de administración de base de datos. Las herramientas existentes deben continuar funcionando con Azure Database for MariaDB, siempre y cuando la manipulación de la base de datos funciona dentro de los confines de permisos de usuario. Tres herramientas comunes de administración de base de datos que han sido probadas y que resultaron para ser compatibles con Azure Database for MariaDB 10.2 se muestran en la tabla siguiente:

| | **MySQL Workbench 6.x y superiores** | **Navicat 12** | **PHPMyAdmin 4.x y superiores**
---|---|---|---
Crear, actualizar, leer, escribir, eliminar | X | X | X
Conexión SSL | X | X | X
Completado automático de consultas SQL | X | X |
Importación y exportación de datos | X | X | X
Exportación en varios formatos | X | X | X
Copia de seguridad y restauración |  | X |
Muestra de parámetros de servidor | X | X | X
Muestra de conexiones de cliente | X | X | X

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de conexión a Azure Database for MariaDB](howto-troubleshoot-common-connection-issues.md)