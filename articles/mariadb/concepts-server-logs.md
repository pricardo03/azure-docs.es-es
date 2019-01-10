---
title: Registros de servidor en Azure Database for MariaDB
description: Se describen los registros disponibles en Azure Database for MariaDB y los parámetros disponibles para habilitar diferentes niveles de registro.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a26f61eb199d8f370e1a9dd010932dc868b74ae4
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53545185"
---
# <a name="server-logs-in-azure-database-for-mariadb"></a>Registros de servidor en Azure Database for MariaDB
En Azure Database for MariaDB, el registro de consultas lentas está disponible para los usuarios. No se admite el acceso al registro de transacciones. El registro de consultas lentas puede utilizarse para identificar cuellos de botella que afectan al rendimiento a fin de solucionar el problema.

Para más información acerca del registro de consultas lentas, consulte la [documentación de MariaDB](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-server-logs"></a>Acceso a registros de servidor
Puede enumerar y descargar los registros de servidor de Azure Database for MariaDB mediante Azure Portal y la CLI de Azure.

En Azure Portal, seleccione el servidor de Azure Database for MariaDB. En el encabezado **Supervisión**, seleccione la página **Registros de servidor**.

<!-- For more information on Azure CLI, see [Configure and access server logs using Azure CLI](howto-configure-server-logs-in-cli.md).-->

## <a name="log-retention"></a>Retención de registros
Los registros están disponibles hasta siete días después de su creación. Si el tamaño total de los registros disponibles supera los 7 GB, se eliminan los archivos más antiguos hasta que haya espacio disponible.

Los registros se rotan cada 24 horas o 7 GB, lo que ocurra primero.

## <a name="configure-logging"></a>registro
De forma predeterminada, el registro de consultas lentas está deshabilitado. Para habilitarlo, establezca low_query_log en ON.

Otros parámetros que se pueden ajustar son los siguientes:

- **long_query_time**: si una consulta tarda más que long_query_time (en segundos), esa consulta se registra. El valor predeterminado es 10 segundos.
- **log_slow_admin_statements**: si ON incluye instrucciones administrativas como ALTER_TABLE y ANALYZE_TABLE en las instrucciones escritas en slow_query_log.
- **log_queries_not_using_indexes**: determina si las consultas que no utilizan índices se registran en slow_query_log.
- **log_throttle_queries_not_using_indexes**: este parámetro limita el número de consultas que no son de índice que se pueden escribir en el registro de consultas lentas. Este parámetro surte efecto cuando log_queries_not_using_indexes está configurado en ON.

Consulte la [documentación del registro de consultas lentas](https://mariadb.com/kb/en/library/slow-query-log-overview/) de MariaDB para obtener una descripción completa de los parámetros de registro de consultas lentas.

## <a name="next-steps"></a>Pasos siguientes
- [Configuración y acceso a los registros de servidor en Azure Portal](howto-configure-server-logs-portal.md).
