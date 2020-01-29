---
title: Copia de una base de datos
description: Cree una copia transaccionalmente coherente de una base de datos de Azure SQL existente en el mismo servidor o en un servidor diferente.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sashan
ms.reviewer: carlrab
ms.date: 11/14/2019
ms.openlocfilehash: e1df345fb9a89972ad1857a937c22d6e10ad1fba
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76289414"
---
# <a name="copy-a-transactionally-consistent-copy-of-an-azure-sql-database"></a>Creación de una copia transaccionalmente coherente de una base de datos de Azure SQL

Azure SQL Database proporciona varios métodos para crear una copia transaccionalmente coherente de una base de datos de Azure SQL existente ([base de datos única](sql-database-single-database.md)) en el mismo servidor o en un servidor diferente. Puede copiar una instancia de SQL Database mediante Azure Portal, PowerShell o T-SQL.

## <a name="overview"></a>Información general

Una copia de la base de datos es una instantánea de la base de datos de origen en el momento de la solicitud de copia. Puede seleccionar el mismo servidor u otro distinto, así como optar por conservar su nivel de servicio y tamaño de proceso, o un tamaño de proceso diferente dentro del mismo nivel de servicio (edición). Cuando se complete la copia, esta se convierte en una base de datos independiente y completamente funcional. Llegado a este punto, puede actualizar a cualquier edición o cambiar a una edición anterior. Los inicios de sesión, usuarios y permisos pueden administrarse de forma independiente. La copia se crea mediante la tecnología de replicación geográfica y, una vez completada la propagación, el vínculo de replicación geográfica finaliza automáticamente. Todos los requisitos para usar la replicación geográfica se aplican a la operación de copia de la base de datos. Consulte [Información general de la replicación geográfica activa](sql-database-active-geo-replication.md) para obtener más información.

> [!NOTE]
> Las [copias de seguridad de base de datos automatizadas](sql-database-automated-backups.md) se usan cuando se crea una copia de la base de datos.

## <a name="logins-in-the-database-copy"></a>Inicios de sesión en la copia de la base de datos

Al copiar una base de datos en el mismo servidor de SQL Database, los mismos inicios de sesión se pueden usar en ambas bases de datos. La entidad de seguridad que usa para copiar la base de datos se convierte en el propietario de la base de datos en la nueva base de datos. Todos los usuarios de base de datos, sus permisos y sus identificadores de seguridad (SID) se copian en la copia de la base de datos.  

Al copiar una base de datos en un servidor de SQL Database diferente, la entidad de seguridad del nuevo servidor se convierte en el propietario de la nueva base de datos. Si usa [usuarios de base de datos contenidos](sql-database-manage-logins.md) para el acceso a datos, asegúrese de que tanto las bases de datos principales como las secundarias tengan siempre las mismas credenciales de usuario, de tal forma que, una vez completada la copia, pueda obtener acceso inmediato a ellas con las mismas credenciales.

Si usa [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), puede eliminar completamente la necesidad de administrar las credenciales en la copia. Pero al copiar la base de datos a un nuevo servidor, puede que el acceso basado en inicios de sesión no funcione debido a que esas cuentas de inicio de sesión no se encuentran en el nuevo servidor. Consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md) para obtener información sobre cómo administrar inicios de sesión al copiar una base de datos a un servidor de SQL Database diferente.

Cuando la copia se realiza correctamente y antes de que se reasignen otros usuarios, solo el inicio de sesión que inició la copia, el propietario de la base de datos, puede iniciar sesión en la nueva base de datos. Para resolver los inicios de sesión una vez completada la operación de copia, consulte [Resolución de inicios de sesión](#resolve-logins).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Copia de base de datos mediante Azure Portal

Para copiar una base de datos mediante Azure Portal, abra la página de la base de datos y haga clic en **Copiar**.

   ![Copia de base de datos](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Copia de base de datos mediante PowerShell

Para copiar una base de datos, use los ejemplos siguientes.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

En el caso de PowerShell, use el cmdlet [New-AzSqlDatabaseCopy](/powershell/module/az.sql/new-azsqldatabasecopy).

> [!IMPORTANT]
> El módulo de Azure Resource Manager (RM) para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. El módulo de AzureRM continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo.  Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos. Para obtener más información sobre la compatibilidad, vea [Presentación del nuevo módulo Az de Azure PowerShell](/powershell/azure/new-azureps-module-az).

```powershell
New-AzSqlDatabaseCopy -ResourceGroupName "<resourceGroup>" -ServerName $sourceserver -DatabaseName "<databaseName>" `
    -CopyResourceGroupName "myResourceGroup" -CopyServerName $targetserver -CopyDatabaseName "CopyOfMySampleDatabase"
```

La copia de la base de datos es una operación asincrónica, pero la base de datos de destino se crea inmediatamente después de aceptar la solicitud. Si tiene que cancelar la operación de copia mientras todavía está en curso, quite la base de datos de destino mediante el cmdlet [Remove-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase).

# <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azure-cli
az sql db copy --dest-name "CopyOfMySampleDatabase" --dest-resource-group "myResourceGroup" --dest-server $targetserver `
    --name "<databaseName>" --resource-group "<resourceGroup>" --server $sourceserver
```

La copia de la base de datos es una operación asincrónica, pero la base de datos de destino se crea inmediatamente después de aceptar la solicitud. Si tiene que cancelar la operación de copia mientras todavía está en curso, coloque la base de datos de destino mediante el comando [az sql db delete](/cli/azure/sql/db#az-sql-db-delete).

* * *

Para obtener un script de ejemplo completo, consulte [Copia de una base de datos en un nuevo servidor](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="rbac-roles-to-manage-database-copy"></a>Roles RBAC para administrar la copia de la base de datos

Para crear una copia de la base de datos, debe tener los siguientes roles:

- Propietario de la suscripción o
- Rol de colaborador de SQL Server o
- Rol personalizado en las bases de datos de origen y de destino con el siguiente permiso:

   Microsoft.Sql/servers/databases/read  Microsoft.Sql/servers/databases/write

Para cancelar una copia de la base de datos, debe tener los siguientes roles:

- Propietario de la suscripción o
- Rol de colaborador de SQL Server o
- Rol personalizado en las bases de datos de origen y de destino con el siguiente permiso:

   Microsoft.Sql/servers/databases/read  Microsoft.Sql/servers/databases/write

Para administrar la copia de la base de datos mediante Azure Portal, también necesitará los siguientes permisos:

   Microsoft.Resources/subscriptions/resources/read Microsoft.Resources/subscriptions/resources/write Microsoft.Resources/deployments/read Microsoft.Resources/deployments/write Microsoft.Resources/deployments/operationstatuses/read

Si quiere ver las operaciones en implementaciones en el grupo de recursos en el portal, las operaciones en varios proveedores de recursos, incluidas las operaciones de SQL, necesitará estos roles RBAC adicionales:

   Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read

## <a name="copy-a-database-by-using-transact-sql"></a>Copia de una base de datos mediante Transact-SQL

Inicie sesión en la base de datos maestra mediante el inicio de sesión de entidad de seguridad de nivel de servidor o el inicio de sesión que creó la base de datos que quiere copiar. Para que la copia de la base de datos sea correcta, los inicios de sesión que no son de la entidad de seguridad de nivel de servidor deben ser miembros del rol dbmanager. Para obtener más información sobre los inicios de sesión y conectarse al servidor, consulte [Administración de inicios de sesión](sql-database-manage-logins.md).

Inicie la copia de la base de datos de origen con la instrucción [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx). Con la ejecución de esta instrucción se inicia el proceso de copia de la base de datos. Dado que copiar una base de datos es un proceso asincrónico, se devuelve la instrucción CREATE DATABASE antes de que la base de datos complete la copia.

### <a name="copy-a-sql-database-to-the-same-server"></a>Copiar una base de datos SQL en el mismo servidor

Inicie sesión en la base de datos maestra mediante el inicio de sesión de entidad de seguridad de nivel de servidor o el inicio de sesión que creó la base de datos que quiere copiar. Para que la copia de la base de datos sea correcta, los inicios de sesión que no son de la entidad de seguridad de nivel de servidor deben ser miembros del rol dbmanager.

Este comando copia Base de datos1 en una base de datos nueva denominada Base de datos2 del mismo servidor. Según el tamaño de su base de datos, la operación de copia puede tardar algún tiempo en completarse.

   ```sql
   -- execute on the master database to start copying
   CREATE DATABASE Database2 AS COPY OF Database1;
   ```

### <a name="copy-a-sql-database-to-a-different-server"></a>Copiar una base de datos SQL en un servidor diferente

Inicie sesión en la base de datos maestra del servidor de destino, el servidor de SQL Database donde se creará la nueva base de datos. Use un inicio de sesión que tenga el mismo nombre y contraseña que el propietario de la base de datos de la base de datos de origen en el servidor de SQL Database de origen. El inicio de sesión en el servidor de destino también debe ser miembro del rol dbmanager o ser el inicio de sesión de entidad de seguridad de nivel de servidor.

Este comando copia Base de datos1 del servidor1 en una nueva base de datos denominada Base de datos2 del servidor2. Según el tamaño de su base de datos, la operación de copia puede tardar algún tiempo en completarse.

```sql
-- Execute on the master database of the target server (server2) to start copying from Server1 to Server2
CREATE DATABASE Database2 AS COPY OF server1.Database1;
```

> [!IMPORTANT]
> Los firewalls de ambos servidores deben estar configurados para permitir la conexión de entrada desde la dirección IP del cliente que emite el comando T-SQL COPY.

### <a name="copy-a-sql-database-to-a-different-subscription"></a>Copiar una base de datos SQL en una suscripción diferente

Puede usar los pasos descritos en la sección anterior para copiar la base de datos en un servidor de base de datos SQL en una suscripción diferente. Asegúrese de que usa un inicio de sesión que tenga el mismo nombre y la misma contraseña que el propietario de la base de datos de origen y que, asimismo, es miembro del rol dbmanager o es el inicio de sesión de la entidad de seguridad en el nivel de servidor. 

> [!NOTE]
> [Azure Portal](https://portal.azure.com) no permite copiar en otra suscripción, ya que Portal llama a la API de ARM y usa los certificados de suscripción para acceder a los dos servidores que intervienen en la replicación geográfica.  

### <a name="monitor-the-progress-of-the-copying-operation"></a>Supervisión del progreso de la operación de copia

Supervise el proceso de copia consultando las vistas sys.databases y sys.dm_database_copies. Mientras que la copia esté en curso, la columna **state_desc** de la vista sys.databases para la nueva base de datos se establece en **COPYING**.

* Si se produce un error en el proceso de copia, la columna **state_desc** de la vista sys.databases para la nueva base de datos se establece en **SUSPECT**. Ejecute la instrucción DROP en la nueva base de datos e inténtelo de nuevo más tarde.
* Si el proceso de copia es correcto, la columna **state_desc** de la vista sys.databases para la nueva base de datos se establece en **ONLINE**. Se completa la copia y la nueva base de datos es una base de datos normal, que se puede modificar independientemente de la base de datos de origen.

> [!NOTE]
> Si decide cancelar la copia mientras está en curso, ejecute la instrucción [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) en la nueva base de datos. Como alternativa, al ejecutar la instrucción DROP DATABASE en la base de datos de origen también se cancelará el proceso de copia.

> [!IMPORTANT]
> Si necesita crear una copia con un SLO bastante más pequeño que el origen, es posible que la base de datos de destino no tenga suficientes recursos para completar el proceso de inicialización y que se produzca un error en la operación de copia. En este escenario, use una solicitud de restauración geográfica para crear una copia en un servidor o una región diferentes. Para más información, consulte [Recuperación de una base de datos de Azure SQL mediante copias de seguridad de base de datos](sql-database-recovery-using-backups.md#geo-restore).

## <a name="resolve-logins"></a>Resolución de inicios de sesión

Después de que la nueva base de datos esté en línea en el servidor de destino, use la instrucción [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) para volver a asignar los usuarios de la nueva base de datos a inicios de sesión en el servidor de destino. Para resolver los usuarios huérfanos, consulte [Solucionar problemas de usuarios huérfanos (SQL Server)](https://msdn.microsoft.com/library/ms175475.aspx). Consulte también [Administración de la seguridad de una base de datos de Azure SQL después de la recuperación ante desastres](sql-database-geo-replication-security-config.md).

Todos los usuarios de la nueva base de datos mantienen los permisos que tenían en la base de datos de origen. El usuario que inició la copia de la base de datos se convierte en el propietario de la base de datos de la nueva base de datos y se le asigna un nuevo identificador de seguridad (SID). Cuando la copia se realiza correctamente y antes de que se reasignen otros usuarios, solo el inicio de sesión que inició la copia, el propietario de la base de datos, puede iniciar sesión en la nueva base de datos.

Para información sobre cómo administrar usuarios e inicios de sesión al copiar una base de datos a un servidor de SQL Database diferente, consulte [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md).

## <a name="database-copy-errors"></a>Errores de copia de base de datos

Pueden encontrarse los siguientes errores al copiar una base de datos en Azure SQL Database. Para más información, vea [Copiar una base de datos de Azure SQL](sql-database-copy.md).

| Código de error | severity | Descripción |
| ---:| ---:|:--- |
| 40635 |16 |El cliente con la dirección IP '%.&#x2a;ls' está deshabilitado temporalmente. |
| 40637 |16 |Crear copia de base de datos está deshabilitado actualmente. |
| 40561 |16 |Error al copiar la base de datos. No existe la base de datos de origen o de destino. |
| 40562 |16 |Error al copiar la base de datos. La base de datos de origen se ha quitado. |
| 40563 |16 |Error al copiar la base de datos. La base de datos de destino se ha quitado. |
| 40564 |16 |No se pudo copiar la base de datos debido a un error interno. Quite la base de datos de destino e inténtelo de nuevo. |
| 40565 |16 |Error al copiar la base de datos. No se permite más de una copia de base de datos simultánea para el mismo origen. Quite la base de datos de destino y vuelva a intentarlo más adelante. |
| 40566 |16 |No se pudo copiar la base de datos debido a un error interno. Quite la base de datos de destino e inténtelo de nuevo. |
| 40567 |16 |No se pudo copiar la base de datos debido a un error interno. Quite la base de datos de destino e inténtelo de nuevo. |
| 40568 |16 |Error al copiar la base de datos. La base de datos de origen ha dejado de estar disponible. Quite la base de datos de destino e inténtelo de nuevo. |
| 40569 |16 |Error al copiar la base de datos. La base de datos de destino ha dejado de estar disponible. Quite la base de datos de destino e inténtelo de nuevo. |
| 40570 |16 |No se pudo copiar la base de datos debido a un error interno. Quite la base de datos de destino y vuelva a intentarlo más adelante. |
| 40571 |16 |No se pudo copiar la base de datos debido a un error interno. Quite la base de datos de destino y vuelva a intentarlo más adelante. |

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los inicios de sesión, consulte [Administración de inicios de sesión](sql-database-manage-logins.md) y [Administración de la seguridad de Azure SQL Database después de la recuperación ante desastres](sql-database-geo-replication-security-config.md).
- Para exportar una base de datos, consulte [Exportación de una base de datos a un archivo BACPAC](sql-database-export.md).
