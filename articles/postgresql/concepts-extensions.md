---
title: Uso de extensiones de PostgreSQL en Azure Database for PostgreSQL
description: Describe la capacidad de ampliar la funcionalidad de su base de datos mediante extensiones en Azure Database for PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/12/2018
ms.openlocfilehash: d6d5a8500435a540f091a082e7dc0e0d6d455716
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540850"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>Extensiones de PostgreSQL en Azure Database for PostgreSQL
PostgreSQL ofrece la capacidad de ampliar la funcionalidad de su base de datos mediante extensiones. Las extensiones permiten agrupar varios objetos SQL relacionados en un solo paquete que se puede cargar o quitar de la base de datos con un solo comando. Después de cargarse en la base de datos, las extensiones pueden funcionar de la misma forma que las características integradas. Para obtener más información sobre las extensiones de PostgreSQL, vea  [Empaquetar objetos relacionados en una extensión](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>¿Cómo se utilizan las extensiones de PostgreSQL?
Para poder usar las extensiones de PostgreSQL, es preciso que estén instaladas en su base de datos. Para instalar una extensión específica, ejecute el comando  [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html)  desde la herramienta psql para cargar los objetos empaquetados en la base de datos.

Azure Database for PostgreSQL actualmente admite un subconjunto de extensiones de claves, como se enumeran a continuación. Aparte de las que se indican, no se admiten otras extensiones. No puede crear su propia extensión con el servicio Azure Database for PostgreSQL.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Extensiones admitidas por Azure Database for PostgreSQL
En las tablas siguientes se enumeran las extensiones estándar de PostgreSQL que Azure Database for PostgreSQL admite actualmente. Esta información también está disponible al ejecutar `SELECT * FROM pg_available_extensions;`.

### <a name="data-types-extensions"></a>Extensiones de tipos de datos

> [!div class="mx-tableFixed"]
| **Extensión** | **Descripción** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Proporciona un tipo de datos para las contraseñas de cifrado automático. |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Proporciona un tipo de cadena de caracteres que no distingue entre mayúsculas y minúsculas. |
| [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | Proporciona un tipo de datos para los cubos multidimensionales. |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Proporciona un tipo de datos para almacenar conjuntos de pares clave/valor. |
| [isn](https://www.postgresql.org/docs/9.6/static/isn.html) | Proporciona tipos de datos para los estándares internacionales de numeración de productos. |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Proporciona un tipo de datos para las estructuras de árbol jerárquicas. |

### <a name="functions-extensions"></a>Extensiones de funciones

> [!div class="mx-tableFixed"]
| **Extensión** | **Descripción** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Proporciona un medio para calcular distancias de círculo máximo en la superficie de la Tierra. |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Proporciona varias funciones para determinar las similitudes y la distancia entre las cadenas. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Proporciona funciones y operadores para manipular matrices de enteros sin valores nulos. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Proporciona funciones de cifrado. |
| [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Administra las tablas con particiones por hora o identificador. |
| [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Proporciona funciones y operadores para determinar la similitud del texto alfanumérico basado en la coincidencia de trigrama. |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Proporciona funciones que manipulan la totalidad del contenido de las tablas, incluidas tablas de referencias cruzadas. |
| [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Genera identificadores únicos universales (UUID). |

### <a name="full-text-search-extensions"></a>Extensiones de búsqueda de texto completo

> [!div class="mx-tableFixed"]
| **Extensión** | **Descripción** |
|---|---|
| [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Proporciona una plantilla de diccionario de búsqueda de texto para números enteros. |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Un diccionario de búsqueda de texto que quita los acentos (signos diacríticos) de lexemas. |

### <a name="index-types-extensions"></a>Extensiones de tipos de índices

> [!div class="mx-tableFixed"]
| **Extensión** | **Descripción** |
|---|---|
| [btree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Proporciona clases de operador GIN de ejemplo que implementan el comportamiento similar al del árbol B para determinados tipos de datos. |
| [btree\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Proporciona clases de operador de índice de GiST que implementan el árbol B. |

### <a name="language-extensions"></a>Extensiones de lenguaje

> [!div class="mx-tableFixed"]
| **Extensión** | **Descripción** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | Lenguaje de procedimientos que puede cargar PL/pgSQL. |
| [plv8](https://plv8.github.io/) | Una extensión del lenguaje JavaScript para PostgreSQL que puede usarse para almacenar procedimientos, desencadenadores, etc. |

### <a name="miscellaneous-extensions"></a>Extensiones variadas

> [!div class="mx-tableFixed"]
| **Extensión** | **Descripción** |
|---|---|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Proporciona un medio para examinar lo que sucede en la caché del búfer compartido en tiempo real. |
| [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Proporciona una manera de cargar datos de relación en la caché del búfer. |
| [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Proporciona un medio para realizar el seguimiento de las estadísticas de ejecución de todas las instrucciones SQL ejecutadas por un servidor. (Vea a continuación una nota sobre esta extensión). |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Proporciona un medio para mostrar información de bloqueo a nivel de fila. |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Proporciona un medio para mostrar estadísticas de nivel de tupla. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Se trata de un contenedor de datos externo utilizado para tener acceso a los datos almacenados en los servidores externos de PostgreSQL. |
| [hypopg](https://hypopg.readthedocs.io/en/latest/) | Proporciona un medio de creación de índices hipotéticos que no consume CPU ni disco. |
| [dblink](https://www.postgresql.org/docs/current/dblink.html) | Módulo que admite conexiones a otras bases de datos de PostgreSQL desde una sesión de base de datos. |


### <a name="postgis-extensions"></a>Extensiones de PostGIS

> [!div class="mx-tableFixed"]
| **Extensión** | **Descripción** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal | Objetos espaciales y geográficos para PostgreSQL. |
| address\_standardizer, address\_standardizer\_data\_us | Se utilizan para analizar una dirección en los elementos que la componen. Se utilizan para admitir el paso de normalización de la dirección de geocodificación. |
| [pgrouting](https://pgrouting.org/) | Extiende la base de datos geoespacial de PostGIS/PostgreSQL para proporcionar la funcionalidad de enrutamiento geoespacial. |


### <a name="using-pgstatstatements"></a>Usar pg_stat_statements
La extensión [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) está cargada previamente en cada servidor de Azure Database for PostgreSQL para proporcionarle un medio de seguimiento de las estadísticas de ejecución de las instrucciones SQL.
La configuración `pg_stat_statements.track`, que controla las instrucciones que la extensión cuenta, se establece de manera predeterminada en `top`, lo que significa que se realiza el seguimiento de todas las instrucciones que los clientes emiten directamente. Los otros dos niveles de seguimiento son `none` y `all`. Esta configuración es configurable como un parámetro de servidor a través de [Azure Portal](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) o la [CLI de Azure](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli).

Hay un equilibrio entre la información de ejecución de consulta que pg_stat_statements proporciona y el impacto en el rendimiento del servidor al registrar cada instrucción SQL. Si no está usando activamente la extensión pg_stat_statements, le recomendamos que establezca `pg_stat_statements.track` en `none`. Tenga en cuenta que algunos servicios de supervisión de terceros pueden basarse en pg_stat_statements para entregar información de rendimiento de consultas, por lo que debe confirmar si este es el caso para usted o no.


## <a name="next-steps"></a>Pasos siguientes
Si no ve una extensión que le gustaría utilizar, háganoslo saber. Vote por las solicitudes existentes o cree nuevos comentarios y solicitudes en nuestro [foro de comentarios de clientes](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
