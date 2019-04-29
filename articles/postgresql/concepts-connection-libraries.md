---
title: Bibliotecas de conexiones de Azure Database for PostgreSQL
description: En este artículo se describen varias bibliotecas y controladores que los desarrolladores pueden usar al codificar aplicaciones para conectarse a Azure Database for PostgreSQL y realizar ahí consultas.
services: postgresql
author: WenJason
ms.author: v-jay
manager: digimobile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
origin.date: 02/28/2018
ms.date: 12/03/2018
ms.openlocfilehash: 0e762a2d7cf82e2957fb276fcea0a20553f719e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60559779"
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Bibliotecas de conexiones de Azure Database for PostgreSQL
En este artículo se enumeran las bibliotecas y los controladores que pueden usar los desarrolladores para desarrollar aplicaciones con el fin de conectarse a Azure Database for PostgreSQL y realizar ahí consultas.

## <a name="client-interfaces"></a>Interfaces de cliente
La mayoría de las bibliotecas de cliente de lenguajes para conectarse al servidor PostgreSQL son proyectos externos y se distribuyen de manera independiente. Las bibliotecas indicadas son compatibles con las plataformas Windows, Linux y Mac para la conexión con Azure Database for PostgreSQL. Se muestran varios ejemplos de inicio rápido en la sección Pasos siguientes.

| **Lenguaje** | **Interfaz de cliente** | **Información adicional** | **Descargar** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | Compatible con API 2.0 de BD | [Descargar](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Extensión de base de datos | [Instalación](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Paquete de Pg npm](https://www.npmjs.com/package/pg) | Cliente sin bloqueo de JavaScript puro | [Instalación](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Controlador JDBC tipo 4 | [Descargar](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Pg gem](https://deveiate.org/code/pg/) | Interfaz Ruby | [Descargar](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Go | [Package pq](https://godoc.org/github.com/lib/pq) | Controlador de Postgres de Go puro | [Instalación](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](https://www.npgsql.org/) | Proveedor de datos de ADO.NET | [Descargar](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | Controlador ODBC | [Descargar](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Interfaz de idioma C principal | Se incluye |
| C++ | [libpqxx](http://pqxx.org/) | Interfaz de C++ de nuevo estilo | [Descargar](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Pasos siguientes
Lea estos tutoriales rápidos sobre cómo conectarse a Azure Database for PostgreSQL y realizar consultas ahí mediante el lenguaje de su elección:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
