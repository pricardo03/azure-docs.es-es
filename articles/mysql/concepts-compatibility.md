---
title: Compatibilidad de los controladores y las herramientas de administración de Azure Database for MySQL
description: En este artículo se describen las herramientas de administración y los controladores de MySQL compatibles con Azure Database for MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: 05f48145973777052590f8d10e1a2ce1fd22ec7a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60525397"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>Herramientas de administración y controladores de MySQL compatibles con Azure Database for MySQL
En este artículo se describen las herramientas de administración y los controladores compatibles con Azure Database for MySQL.

## <a name="mysql-drivers"></a>Controladores de MySQL
Azure Database for MySQL usa la edición comunitaria más popular del mundo de la base de datos MySQL. Por lo tanto, es compatible con una amplia variedad de controladores y lenguajes de programación. El objetivo es la compatibilidad con las tres versiones más recientes de los controladores MySQL, y continúan los esfuerzos con los autores de la comunidad de código abierto para mejorar constantemente la funcionalidad y la facilidad de uso de los controladores de MySQL. En la tabla siguiente se proporciona una lista de controladores que se han probado y son compatibles con Azure Database for MySQL 5.6 y 5.7:

| **Controlador** | **Vínculos** | **Versiones compatibles** | **Versiones incompatibles** | **Notas** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | Para la conexión PHP 7.0 con SSL MySQLi, agregue MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT en la cadena de conexión. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO: establezca la opción ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` en false.|
| .NET | [MySqlConnector en GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Paquete de instalación de NuGet](https://www.nuget.org/packages/MySqlConnector/) | 0.27 y posterior | 0.26.5 y anterior | |
| Conector MySQL/NET | [Conector MySQL/NET](https://github.com/mysql/mysql-connector-net) | 8.0, 7.0, 6.10 |  | Un error de codificación puede provocar errores de conexión en algunos sistemas Windows que no sean UTF8. |
| Nodejs |  [MySQLjs en GitHub](https://github.com/mysqljs/mysql/) <br> Paquete de instalación de NPM:<br> Ejecute `npm install mysql` en NPM | 2.15 | 2.14.1 y anterior | |
| GO | https://github.com/go-sql-driver/mysql/releases | 1.3, 1.4 | 1.2 y anterior | Use `allowNativePasswords=true` en la cadena de conexión para la versión 1.3. La versión 1.4 contiene una corrección y `allowNativePasswords=true` ya no es necesario. |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 y anterior | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | 1.5.5 y anterior | |

## <a name="management-tools"></a>Herramientas de administración
La ventaja de compatibilidad se amplía también a las herramientas de administración de base de datos. Sus herramientas actuales deben continuar funcionando con Azure Database for MySQL, siempre y cuando la manipulación de la base de datos tenga lugar dentro de los límites establecidos por los permisos de usuario. En la tabla siguiente se indican las tres herramientas comunes de administración de base de datos que se han probado y son compatibles con Azure Database for MySQL 5.6 y 5.7:

|                                     | **MySQL Workbench 6.x y superiores** | **Navicat 12** | **PHPMyAdmin 4.x y superiores** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Crear, actualizar, leer, escribir, eliminar | X | X | X |
| Conexión SSL | X | X | X |
| Completado automático de consultas SQL | X | X |  |
| Importación y exportación de datos | X | X | X |
| Exportación en varios formatos | X | X | X |
| Copia de seguridad y restauración |  | X |  |
| Muestra de parámetros de servidor | X | X | X |
| Muestra de conexiones de cliente | X | X | X |

## <a name="next-steps"></a>Pasos siguientes

- [Troubleshoot connection issues to Azure Database for MySQL](howto-troubleshoot-common-connection-issues.md) (Solución de problemas de conexión a Azure Database for MySQL)