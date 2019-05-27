---
title: Solución de problemas de Azure SQL Data Warehouse | Microsoft Docs
description: Cómo solucionar los problemas de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: supportability
ms.date: 03/27/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.openlocfilehash: ddae39b7a2c76b5bb3f7caff1c0d5a8165c9d93a
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873825"
---
# <a name="troubleshooting-connectivity-issues"></a>Solución de problemas de conectividad

En este artículo se enumera las técnicas de solución de problemas comunes en torno a la conexión a SQL Data Warehouse.
- [Comprobar disponibilidad del servicio](./sql-data-warehouse-troubleshoot-connectivity.md#check-service-availability)
- [Compruebe si la operación de pausa o escalado](./sql-data-warehouse-troubleshoot-connectivity.md#check-for-paused-or-scaling-operation)
- [Compruebe la configuración del firewall](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-firewall-settings)
- [Compruebe la configuración de punto de conexión de red virtual o servicio](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-vnetservice-endpoint-settings)
- [Busque los controladores más recientes](./sql-data-warehouse-troubleshoot-connectivity.md#check-for-the-latest-drivers)
- [Compruebe la cadena de conexión](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-connection-string)
- [Problemas de conexión intermitentes](./sql-data-warehouse-troubleshoot-connectivity.md#intermittent-connection-issues)
- [Mensajes comunes de error](./sql-data-warehouse-troubleshoot-connectivity.md#common-error-messages)

## <a name="check-service-availability"></a>Comprobar disponibilidad del servicio

Compruebe si el servicio está disponible. En el portal de Azure, vaya a SQL data warehouse que está intentando conectarse. En el panel izquierdo de la tabla de contenido, haga clic en **diagnosticar y resolver problemas**.

![Seleccione el estado de los recursos](./media/sql-data-warehouse-troubleshoot-connectivity/diagnostics-link.png)

Aquí se mostrará el estado de SQL data warehouse. Si el servicio no se muestra como **disponible**, consulte más pasos.

![Servicio está disponible](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health.png)

Si muestra el estado de los recursos que el almacenamiento de datos está en pausa o escalado, siga las instrucciones para reanudar el almacenamiento de datos.

![Servicio pausado](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health-pausing.png) información adicional sobre el estado de los recursos puede encontrarse aquí.

## <a name="check-for-paused-or-scaling-operation"></a>Compruebe si la operación de pausa o escalado

Compruebe el portal para ver si su SQL data warehouse está en pausa o escalado.

![El servicio en pausa](./media/sql-data-warehouse-troubleshoot-connectivity/overview-paused.png)

Si ve el servicio está en pausa o escalado, compruebe que no está durante la programación de mantenimiento. En el portal para SQL data warehouse *Introducción*, verá que la programación de mantenimiento seleccionados.

![Información general sobre programación de mantenimiento](./media/sql-data-warehouse-troubleshoot-connectivity/overview-maintance-schedule.png)

En caso contrario, compruebe con el Administrador de TI para comprobar que este mantenimiento no es un evento programado. Para reanudar el almacenamiento de datos SQL, siga los pasos descritos [aquí](https://docs.microsoft.com/azure/sql-data-warehouse/pause-and-resume-compute-portal#resume-compute).

## <a name="check-your-firewall-settings"></a>Compruebe la configuración del firewall

SQL Data Warehouse se comunica a través del puerto 1433.   Si intenta conectarse desde una red corporativa, es posible que el firewall de la red no permita el tráfico saliente a través del puerto 1433. En ese caso, no se puede conectar al servidor de Azure SQL Database, a menos que el departamento de TI abra el puerto 1433. Puede encontrar información adicional sobre las configuraciones de firewall [aquí](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc.json#manage-ip-firewall-rules-using-the-azure-portal).

## <a name="check-your-vnetservice-endpoint-settings"></a>Compruebe la configuración de punto de conexión de red virtual o servicio

Si recibe errores 40914 y 40615, consulte [descripción del error y la resolución aquí](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#errors-40914-and-40615).

## <a name="check-for-the-latest-drivers"></a>Busque los controladores más recientes

### <a name="software"></a>Software

Compruebe para asegurarse de que usa las herramientas más recientes para conectarse a SQL data warehouse:

* SSMS
* Azure Data Studio 
* SQL Server Data Tools (Visual Studio)

### <a name="drivers"></a>Controladores

Compruebe para asegurarse de que usa las últimas versiones de controlador.  Con una versión anterior de los controladores podría provocar comportamientos inesperados como los controladores más antiguos pueden no admitir las nuevas características.

* [ODBC](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server)
* [JDBC](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)
* [OLE DB](https://docs.microsoft.com/sql/connect/oledb/download-oledb-driver-for-sql-server)
* [PHP](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server)

## <a name="check-your-connection-string"></a>Compruebe la cadena de conexión

Compruebe para asegurarse de que las cadenas de conexión se ha establecido correctamente.  A continuación se muestran algunos ejemplos.  También puede encontrar información adicional en torno a [aquí las cadenas de conexión](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connection-strings).

Cadena de conexión ADO.NET

```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

Cadena de conexión de ODBC

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

Cadena de conexión de PHP

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

Cadena de conexión JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="intermittent-connection-issues"></a>Problemas de conexión intermitentes

Compruebe si tiene mucha carga en el servidor con un gran número de solicitudes en cola. Es posible que deba escalar verticalmente el almacenamiento de datos para obtener recursos adicionales.

## <a name="common-error-messages"></a>Mensajes comunes de error

Errores 40914 y 40615, consulte el [descripción del error y la resolución aquí](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#errors-40914-and-40615).

## <a name="still-having-connectivity-issues"></a>¿Sigue teniendo problemas de conectividad?
Crear un [incidencia de soporte técnico](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) por lo que el equipo de ingeniería puede ayudarle.
