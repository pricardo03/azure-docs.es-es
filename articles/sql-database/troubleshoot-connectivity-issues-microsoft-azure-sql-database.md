---
title: Solución de problemas de conectividad de Microsoft Azure SQL Database | Microsoft Docs
description: Se describe cómo solucionar problemas de conectividad en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
ms.openlocfilehash: 9de6d85e1fc54d60f999cfa18665067b3998a432
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390674"
---
# <a name="troubleshooting-connectivity-issues-with-microsoft-azure-sql-database"></a>Solución de problemas de conectividad de Microsoft Azure SQL Database

Cuando la conexión a Azure SQL Database no se logra establecer, se reciben mensajes de error. Estos problemas de conexión pueden deberse a la reconfiguración de Azure SQL Database, la configuración del firewall, el tiempo de espera de la conexión o a información de inicio de sesión incorrecta. Además, si se alcanza el límite máximo en algunos recursos de Azure SQL Database, no se puede conectar a Azure SQL Database.

## <a name="error-40613-database--x--on-server--y--is-not-currently-available"></a>Error 40613: La base de datos <x> del servidor <y> no está disponible actualmente

**Detalles del error**

``40613: Database <DBname> on server < server name > is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of '< Tracing ID >'.``

Para resolver este problema:

1. Compruebe el [Panel de servicios de Microsoft Azure](https://status.azure.com/status) para ver interrupciones conocidas. 
2. Si no hay interrupciones conocidas, vaya al [sitio web de soporte técnico de Microsoft Azure](http://azure.microsoft.com/support/options) para abrir un caso de soporte técnico.

Para más información, consulte [Solución del error "La base de datos del servidor no está disponible en este momento"](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-common-connection-issues#troubleshoot-transient-errors).

## <a name="a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server"></a>Se ha producido un error relacionado con la red o específico de la instancia al establecer una conexión en SQL Server.

El problema se produce si la aplicación no se puede conectar al servidor.

Para resolver este problema, pruebe los pasos (en orden) de la sección [Pasos para solucionar problemas de conexión comunes](#steps-to-fix-common-connection-issues).

## <a name="the-server-was-not-found-or-was-not-accessible-errors-26-40-10053"></a>No se detectó el servidor o no estaba accesible (errores 26, 40, 10053)

### <a name="error-26-error-locating-serverinstance-specified"></a>Error 26: Error al buscar el servidor/instancia especificados

**Detalles del error**

``System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.(provider: SQL Network Interfaces, error: 26 – Error Locating Server/Instance Specified)``

### <a name="error-40-could-not-open-a-connection-to-sql-server"></a>Error 40: No se pudo abrir una conexión a SQL Server

**Detalles del error**

``A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)``

### <a name="error-10053-a-transport-level-error-has-occurred-when-receiving-results-from-the-server"></a>Error 10053: Error en el nivel del transporte al recibir los resultados del servidor.

**Detalles del error**

``10053: A transport-level error has occurred when receiving results from the server. (Provider: TCP Provider, error: 0 - An established connection was aborted by the software in your host machine)``

Estos errores se producen porque la aplicación no se puede conectar al servidor.

Para resolver este problema, pruebe los pasos (en orden) de la sección [Pasos para solucionar problemas de conexión comunes](#steps-to-fix-common-connection-issues).

## <a name="cannot-connect-to-servername-due-to-firewall-issues"></a>No se puede conectar a <servername> por problemas del firewall

### <a name="error-40615-cannot-connect-to--servername-"></a>Error 40615: No se puede conectar a <nombredelservidor>

Para resolver este problema, [configure un firewall en SQL Database con Azure Portal](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

### <a name="error-5-cannot-connect-to--servername-"></a>Error 5: No se puede conectar a <nombredelservidor>

Para resolver este problema, asegúrese de que el puerto 1433 está abierto para las conexiones salientes en todos los firewalls entre el cliente e Internet.

Para más información, consulte [Configuración del Firewall de Windows para permitir el acceso a SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="unable-to-log-in-to-the-server-errors-18456-40531"></a>No se puede iniciar sesión en el servidor (errores 18456, 40531)

### <a name="login-failed-for-user--user-name-"></a>Error de inicio de sesión del usuario "<nombre de usuario>"

**Detalles del error**

``Login failed for user '<User name>'.This session has been assigned a tracing ID of '<Tracing ID>'. Provide this tracing ID to customer support when you need assistance. (Microsoft SQL Server, Error: 18456)``

Para resolver este problema, póngase en contacto con el administrador de servicio para que le proporcione un nombre de usuario y una contraseña de SQL Server válidos.

Normalmente, el administrador de servicio puede usar los siguientes pasos para agregar las credenciales de inicio de sesión:

1. Inicie sesión en el servidor mediante SQL Server Management Studio (SSMS).
2. Ejecute la siguiente consulta SQL para comprobar si el nombre de inicio de sesión está deshabilitado:

   ```
   SELECT name, is_disabled FROM sys.sql_logins
   ```

3. Si el nombre correspondiente está deshabilitado, habilítelo mediante la instrucción siguiente: 

   ```
   Alter login <User name> enable
   ```

4. Si el nombre de usuario de inicio de sesión SQL no existe, créelo con estos pasos:

   1. En SSMS, haga doble clic en **Seguridad** para expandirlo. 
   2. Haga clic con el botón derecho en **Inicios de sesión** y, a continuación, seleccione **Nuevo inicio de sesión**. 
   3. En el script generado con marcadores de posición, edite y ejecute la siguiente consulta SQL:
 
   ```
   CREATE LOGIN <SQL_login_name, sysname, login_name>
   WITH PASSWORD = ‘<password, sysname, Change_Password>’
   GO
   ```       
5. Haga doble clic en **Base de datos**. 
6. Seleccione la base de datos para la que desea conceder permiso al usuario.
7. Haga doble clic en **Seguridad**. 
8. Haga clic con el botón derecho en **Usuarios** y, a continuación, seleccione **Nuevo usuario**. 
9. En el script generado con marcadores de posición, edite y ejecute la siguiente consulta SQL: 

   ```
   CREATE USER <user_name, sysname, user_name>          
   FOR LOGIN <login_name, sysname, login_name>
   WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
   GO
   
   -- Add user to the database owner role

   EXEC sp_addrolemember N’db_owner’, N’<user_name, sysname, user_name>’
   GO
   ```
   
   > [!NOTE]
   > también puede utilizar `sp_addrolemember` para asignar usuarios específicos a roles de base de datos específicos.

Para más información, consulte [Administración de bases de datos e inicios de sesión en Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="connection-timeout-expired-errors"></a>Errores de tiempo de espera de conexión agotado

### <a name="systemdatasqlclientsqlexception-0x80131904-connection-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904): Tiempo de espera de conexión agotado

**Detalles del error**

``System.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed while attempting to consume the pre-login handshake acknowledgement. This could be because the pre-login handshake failed or the server was unable to respond back in time. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=3; handshake=29995;``

### <a name="systemdatasqlclientsqlexception-0x80131904-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904): Tiempo de espera agotado

**Detalles del error**

``System.Data.SqlClient.SqlException (0x80131904): Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding.``

### <a name="systemdataentitycoreentityexception-the-underlying-provider-failed-on-open"></a>System.Data.Entity.Core.EntityException: Error del proveedor subyacente en [Abierto]

**Detalles del error**

``System.Data.Entity.Core.EntityException: The underlying provider failed on Open. -> System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. -> System.ComponentModel.Win32Exception: The wait operation timed out``

### <a name="cannot-connect-to--server-name-"></a>No se puede conectar a <nombre del servidor>

**Detalles del error**

``Cannot connect to <server name>.ADDITIONAL INFORMATION:Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=231; handshake=983; [Login] initialization=0; authentication=0; [Post-Login] complete=13000; (Microsoft SQL Server, Error: -2) For help, click: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=-2&LinkId=20476 The wait operation timed out``

Estas excepciones pueden producirse por problemas de conexión o de consulta. Para confirmar que este error se debe a problemas de conectividad, consulte [Confirmación de si un error se debe a un problema de conectividad](#confirm-whether-an-error-is-caused-by-a-connectivity-issue).

Los tiempos de espera de conexión se producen porque la aplicación no se puede conectar al servidor. Para resolver este problema, pruebe los pasos (en orden) de la sección [Pasos para solucionar problemas de conexión comunes](#steps-to-fix-common-connection-issues).

## <a name="transient-errors-errors-40197-40545"></a>Errores transitorios (errores 40197, 40545)

### <a name="error-40197-the-service-has-encountered-an-error-processing-your-request-please-try-again-error-code--code-"></a>Error 40197: Error en el servicio al procesar la solicitud. Vuelva a intentarlo. Código de error <código>

Este problema se produce por un error transitorio detectado durante una reconfiguración o conmutación por error en el back-end.

Para resolver este problema, espere un poco y vuelva a intentarlo. No es necesario abrir un caso de soporte técnico a menos que el problema persista.

Como procedimiento recomendado, asegúrese de que la lógica de reintento esté implantada. Para más información sobre la lógica de reintento, consulte [Solución de problemas temporales y de errores de conexión en SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues).

## <a name="connection-terminated-because-of-a-system-defined-limit"></a>La conexión terminó porque se alcanzó un límite definido por el sistema

### <a name="error-10928-resource-id-d"></a>Error 10928: Id. de recurso: %d

**Detalles del error**

``10928: Resource ID: %d. The %s limit for the database is %d and has been reached. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. The Resource ID value in error message indicates the resource for which limit has been reached. For sessions, Resource ID = 2.``

Para resolver este problema, pruebe uno de los métodos siguientes:

* Compruebe si hay consultas de larga duración:

  > [!NOTE]
  > Se trata de un enfoque minimalista que puede que no resuelva el problema.

  1. Ejecute la siguiente consulta SQL para comprobar la vista [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) y ver las solicitudes de bloqueo:

             ```
             SELECT * FROM dm_exec_requests
             ```

  2. Determine el **búfer de entrada** para el bloqueador de encabezado.
  3. Optimice la consulta del bloqueador de encabezado.

    Para un procedimiento de solución de problemas detallado, consulte [¿Se ejecuta correctamente mi consulta en la nube?](http://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)

* Si la base de datos alcanza el límite constantemente a pesar del bloqueo de direccionamiento y las consultas de larga duración, podría actualizarse a una de las nuevas versiones preliminares (como la edición [Estándar o Premium](https://azure.microsoft.com/pricing/details/sql-database/)).

Para más información sobre las opciones de precios de SQL Database, consulte [Precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/).

Para más información sobre las vistas de administración dinámica, consulte [Vistas de administración dinámica del sistema](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views).

Para más información sobre este mensaje de error, consulte [Límites de recursos de SQL Database para un servidor de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server).

### <a name="error-10929-resource-id-1"></a>Error 10929: Identificador de recurso: 1

**Detalles del error**

``10929: Resource ID: 1. The %s minimum guarantee is %d, maximum limit is %d and the current usage for the database is %d. However, the server is currently too busy to support requests greater than %d for this database. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. Otherwise, please try again later.``

Para más información sobre este error, consulte [Mensajes de error para los programas de cliente de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-develop-error-messages).

### <a name="error-40501-the-service-is-currently-busy"></a>Error 40501: El servicio está ocupado actualmente

**Detalles del error**

``40501: The service is currently busy. Retry the request after 10 seconds. Incident ID: %ls. Code: %d.``

Se trata de un error de limitación del motor, que indica que se superan los límites de los recursos.

Para más información sobre los límites de recursos, consulte [Límites de los recursos del servidor de bases de datos](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server).

### <a name="error-40544-the-database-has-reached-its-size-quota"></a>Error 40544: La base de datos ha alcanzado su cuota de tamaño

**Detalles del error**

``40544: The database has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions. Incident ID: <ID>. Code: <code>.``

Este error se produce cuando la base de datos ha alcanzado su cuota de tamaño.

Los pasos siguientes pueden ayudarle a solucionar el problema o proporcionarle opciones adicionales:

1. Compruebe el tamaño actual de la base de datos mediante el panel en Azure Portal.

   > [!NOTE]
   > Para identificar qué tablas consumen más espacio y, por lo tanto, son posibles candidatos para la limpieza, ejecute la siguiente consulta SQL:

   ```
   SELECT o.name,
    a.SUM(p.row_count) AS 'Row Count',
    b.SUM(p.reserved_page_count) * 8.0 / 1024 AS 'Table Size (MB)'
   FROM sys.objects o
   JOIN sys.dm_db_partition_stats p on p.object_id = o.object_id
   GROUP BY o.name
   ORDER BY [Table Size (MB)] DESC
   ```

2. Si el tamaño actual no supera el tamaño máximo admitido para la versión, puede usar ALTER DATABASE para aumentar el valor de MAXSIZE. 
3. Si la base de datos ya supera el tamaño máximo admitido para la edición, pruebe uno o varios de los pasos siguientes:
   - Realice actividades normales de limpieza de la base de datos. Por ejemplo, limpie los datos no deseados con truncar o eliminar, o bien mueva los datos con SQL Server Integration Services (SSIS) o el programa de copia masiva (bcp).
   - Cree particiones o elimine datos, quite índices o consulte la documentación para obtener soluciones posibles.

   - Para el escalado de la base de datos, consulte [Escalar recursos de base de datos única](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-scale) y [Escalar recursos de grupos elásticos](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool-scale).

### <a name="error-40549-session-is-terminated-because-you-have-a-long-running-transaction"></a>Error 40549: La sesión terminó porque tiene una transacción de larga duración

**Detalles del error**

``40549: Session is terminated because you have a long-running transaction. Try shortening your transaction.``

Si se encuentra con frecuencia este mensaje de error, siga estos pasos para intentar resolver el problema: 

1. Compruebe la vista sys.dm_exec_requests para ver todas las sesiones abiertas que tengan un valor alto para la columna total_elapsed_time. Para realizar esta comprobación, ejecute el siguiente script de SQL:

   ```
   SELECT * FROM dm_exec_requests
   ```
2. Determine el búfer de entrada de la consulta de larga duración. 
3. Ajuste de la consulta.

Considere también el procesamiento por lotes de las consultas. Para más información sobre el procesamiento por lotes, consulte [Uso del procesamiento por lotes para mejorar el rendimiento de las aplicaciones de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance).

Para un procedimiento de solución de problemas detallado, consulte [¿Se ejecuta correctamente mi consulta en la nube?](http://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)

### <a name="error-40551-the-session-has-been-terminated-because-of-excessive-tempdb-usage"></a>Error 40551: La sesión ha terminado debido al uso excesivo de TEMPDB

**Detalles del error**

``40551: The session has been terminated because of excessive TEMPDB usage. Try modifying your query to reduce the temporary table space usage.``

Para resolver el problema, siga estos pasos:

1. Modifique las consultas para reducir el uso del espacio de la tabla temporal. 
2. Quite los objetos temporales cuando ya no se necesiten. 
3. Trunque las tablas o quite las que no se usen.

### <a name="error-40552-the-session-has-been-terminated-because-of-excessive-transaction-log-space-usage"></a>Error 40552: La sesión ha terminado debido al uso excesivo del espacio de registro de transacciones

**Detalles del error**

``40552: The session has been terminated because of excessive transaction log space usage. Try modifying fewer rows in a single transaction.``

Para resolver este problema, pruebe los métodos siguientes:

* El problema puede producirse debido a operaciones de inserción, actualización o eliminación. Intente reducir el número de filas que en las que se trabaja inmediatamente al implementar el procesamiento por lotes o al dividir las transacciones en varias más pequeñas.
* El problema puede producirse debido a operaciones de recompilación de índices. Para solucionar este problema, asegúrese de que el número de filas afectadas de la tabla * (tamaño medio del campo que se actualiza en bytes + 80) < 2 gigabytes (GB).

  > [!NOTE]
  > Para la recompilación del índice, el tamaño medio del campo que se actualiza debe sustituirse por el tamaño medio del índice.

### <a name="error-40553-the-session-has-been-terminated-because-of-excessive-memory-usage"></a>Error 40553: La sesión ha terminado debido al uso excesivo de la memoria

**Detalles del error**

``40553 : The session has been terminated because of excessive memory usage. Try modifying your query to process fewer rows.``

Para solucionar este problema, intente optimizar la consulta.

Para un procedimiento de solución de problemas detallado, consulte [¿Se ejecuta correctamente mi consulta en la nube?](http://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)


### <a name="cannot-open-database-master-requested-by-the-login-the-login-failed"></a>No se puede abrir la base de datos maestra solicitada por el inicio de sesión. Error de inicio de sesión.

Este problema se produce porque la cuenta no tiene permiso de acceso a la base de datos maestra. Sin embargo, SQL Server Management Studio (SSMS) intenta conectarse a la base de datos maestra.

Para resolver el problema, siga estos pasos:

1. En la pantalla de inicio de sesión de SSMS, seleccione **Opciones** y, luego, **Propiedades de la conexión**. 
2. En el campo **Conectar con base de datos**, escriba el nombre de la base de datos predeterminada del usuario como la base de datos de inicio de sesión predeterminada y seleccione **Conectar**.

   ![Propiedades de la conexión](media/troubleshoot-connectivity-issues-microsoft-azure-sql-database/cannot-open-database-master.png)

## <a name="confirm-whether-an-error-is-caused-by-a-connectivity-issue"></a>Confirmación de si un error se debe a un problema de conectividad

Para confirmar si un error se debe a un problema de conectividad, revise el seguimiento de la pila de los marcos que muestran llamadas para abrir una conexión como las siguientes (observe la referencia a la clase **SqlConnection**):

```
System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
 at System.Data.SqlClient.SqlConnection.Open()
 at AzureConnectionTest.Program.Main(String[] args)
ClientConnectionId:<Client connection ID>
```

Cuando se produce la excepción por problemas de consulta, observará una pila de llamadas similar a la siguiente (observe la referencia a la clase **SqlCommand**). En esta situación, [optimice las consultas ](http://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

```
  at System.Data.SqlClient.SqlCommand.ExecuteReader()
  at AzureConnectionTest.Program.Main(String[] args)
  ClientConnectionId:<Client ID>
```
Para instrucciones adicionales sobre cómo ajustar el rendimiento, consulte los siguientes recursos:

* [Mantenimiento de índices y estadísticas de Azure SQL](https://techcommunity.microsoft.com/t5/Azure-Database-Support-Blog/How-to-maintain-Azure-SQL-Indexes-and-Statistics/ba-p/368787)
* [Ajuste manual del rendimiento de consultas en Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
* [Supervisión del rendimiento de Azure SQL Database con vistas de administración dinámica](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
* [Funcionamiento del almacén de consultas de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)


## <a name="steps-to-fix-common-connection-issues"></a>Pasos para solucionar problemas de conexión comunes

1. Asegúrese de que TCP/IP esté habilitado como protocolo de cliente en el servidor de aplicaciones. Para más información, consulte [Configurar protocolos de cliente](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-client-protocols). En los servidores de aplicaciones en los que no tiene instaladas herramientas de SQL Server, compruebe que TCP/IP esté habilitado mediante la ejecución de **cliconfg.exe** (utilidad de red de cliente de SQL Server). 
2. Compruebe la cadena de conexión de la aplicación para asegurarse de que está configurada correctamente. Por ejemplo, asegúrese de que la cadena de conexión especifica el puerto correcto (1433) y el nombre completo del servidor.
Consulte [Obtención de información de conexión de SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms#get-sql-server-connection-information).
3. Intente aumentar el valor de tiempo de espera de la conexión. Se recomienda el uso de un tiempo de espera de conexión de al menos 30 segundos. 
4. Pruebe la conectividad entre el servidor de aplicaciones y la base de datos de Azure SQL mediante [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms), un archivo UDL, ping o Telnet. Para más información, consulte [Resolver errores de conexión a SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) y [Diagnóstico de problemas de conexión](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues#diagnostics).

   > [!NOTE]
   > Como paso de solución de problemas, también puede probar la conectividad en otro equipo cliente.

5. Como procedimiento recomendado, asegúrese de que la lógica de reintento esté implantada. Para más información sobre la lógica de reintento, consulte [Solución de problemas temporales y de conexión a SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues).

Si con los pasos anteriores no se resuelve el problema, intente recopilar más datos y luego póngase en contacto con el soporte técnico. Si la aplicación es un servicio en la nube, habilite el registro. Este paso devuelve una marca de tiempo UTC del error. Además, Azure SQL devuelve el identificador de seguimiento. [Los servicios de soporte técnico de Microsoft](http://azure.microsoft.com/support/options/) pueden usar esta información. 

Para más información sobre cómo habilitar el registro, consulte [Habilitar el registro de diagnósticos para las aplicaciones de Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/).

**Documentos relacionados**

* [Arquitectura de conectividad de Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture)<br>
* [Controles de acceso a la red para Azure SQL Database y Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-networkaccess-overview)
