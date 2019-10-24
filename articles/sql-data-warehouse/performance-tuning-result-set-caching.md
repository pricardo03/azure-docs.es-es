---
title: Ajuste del rendimiento con la copia en caché del conjunto de resultados | Microsoft Docs
description: Introducción a las características
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 10/10/2019
ms.author: xiaoyul
ms.reviewer: nidejaco;
ms.openlocfilehash: f6323501fc0078677c4c0e2cd0e43a15583df29b
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513985"
---
# <a name="performance-tuning-with-result-set-caching"></a>Ajuste del rendimiento con la copia en caché del conjunto de resultados  
Cuando se habilita la copia en caché del conjunto de resultados, Azure SQL Data Warehouse copia automáticamente en caché los resultados de la consulta realizada en la base de datos de usuario para un uso repetido.  Esto permite que las ejecuciones posteriores de la consulta obtengan los resultados directamente de la memoria caché persistente, por lo que no es necesario volver a realizar el proceso.   La copia en caché del conjunto de resultados mejora el rendimiento de las consultas y reduce la utilización de recursos de proceso.  Además, las consultas que usan conjuntos de resultados en la memoria caché no usan ningún espacio de simultaneidad y, por lo tanto, no cuentan para los límites de simultaneidad existentes. Por seguridad, los usuarios solo pueden acceder a los resultados en la memoria caché si tienen los mismos permisos de acceso a los datos que los usuarios que crearon estos resultados.  

## <a name="key-commands"></a>Comandos clave
[Activación o desactivación (ON/OFF) de la copia en caché del conjunto de resultados de una base de datos de usuario](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azure-sqldw-latest)

[Activación o desactivación (ON/OFF) de la copia en caché del conjunto de resultados de una sesión](https://docs.microsoft.com/sql/t-sql/statements/set-result-set-caching-transact-sql?view=azure-sqldw-latest)

[Comprobación del tamaño del conjunto de resultados copiado en caché](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql?view=azure-sqldw-latest)  

[Limpieza de la memoria caché](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql?view=azure-sqldw-latest)

## <a name="whats-not-cached"></a>No se copia en caché  

Una vez activada la copia en caché del conjunto de resultados de una base de datos, se copian en caché los resultados de todas las consultas hasta que la memoria caché está llena, excepto en el caso de las siguientes consultas:
- Consultas con funciones no deterministas como DateTime.Now()
- Consultas con funciones definidas por el usuario
- Consultas que devuelven datos con un tamaño de fila superior a 64 kB

Las consultas con conjuntos de resultados de gran tamaño (por ejemplo, más de 1 millón de filas) pueden presentar un rendimiento más lento durante la primera ejecución, cuando se crea la memoria caché de resultados.

La copia en caché del conjunto de resultados no es compatible con la seguridad de nivel de fila.  

## <a name="when-cached-results-are-used"></a>Uso de los resultados en la memoria caché

El conjunto de resultados en caché se reutiliza en una consulta si se cumplen todos los requisitos siguientes:
- El usuario que ejecuta la consulta tiene acceso a todas las tablas a las que se hace referencia en la consulta.
- Hay una coincidencia exacta entre la nueva consulta y la consulta anterior que generó la caché del conjunto de resultados.
- No hay cambios en los datos o en el esquema de las tablas a partir de las cuales se generó el conjunto de resultados en caché.

Ejecute este comando para comprobar si una consulta se ejecutó con un acierto o un error de caché de resultados. Si hay un acierto de caché, result_cache_hit devolverá 1.

```sql
SELECT request_id, command, result_cache_hit FROM sys.pdw_exec_requests 
WHERE request_id = <'Your_Query_Request_ID'>
```

## <a name="manage-cached-results"></a>Administración de los resultados en caché 

El tamaño máximo de la memoria caché del conjunto de resultados es de 1 TB por base de datos.  Los resultados en caché se invalidan automáticamente cuando cambian los datos de la consulta subyacente.  

La expulsión de la caché se administra automáticamente mediante Azure SQL Data Warehouse según la siguiente programación: 
- Cada 48 horas si el conjunto de resultados no se ha usado o se ha invalidado. 
- Cuando la memoria caché del conjunto de resultados se aproxima al tamaño máximo.

Los usuarios pueden vaciar manualmente toda la memoria caché del conjunto de resultados mediante una de estas opciones: 
- Desactivación de la característica de copia en caché del conjunto de resultados de la base de datos 
- Ejecución de DBCC DROPRESULTSETCACHE mientras está conectado a la base de datos

Al pausar una base de datos, no se vaciará el conjunto de resultados en caché.  

## <a name="next-steps"></a>Pasos siguientes
Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo de SQL Data Warehouse](sql-data-warehouse-overview-develop.md). 
