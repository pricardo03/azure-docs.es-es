---
title: Restauración de una copia de seguridad a Instancia administrada de Azure SQL Database | Microsoft Docs
description: Restaure una copia de seguridad de datos a una instancia administrada de Azure SQL Database mediante SSMS.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: 40d07827cbd856fe3be3d797dde793b1a7f50207
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/20/2018
ms.locfileid: "53653245"
---
# <a name="quickstart-restore-a-database-to-a-managed-instance"></a>Inicio rápido: Restauración de una base de datos en una instancia administrada 

En este inicio rápido, usará SQL Server Management Studio (SSMS) para restaurar una base de datos (el archivo de copia de seguridad de Wide World Importers - Standard) de Azure Blob Storage en una [instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) de Azure SQL Database. 

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

> [!NOTE]
> * Para más información sobre la migración con Azure Database Migration Service (DMS), consulte [Migración de Instancia administrada mediante DMS](../dms/tutorial-sql-server-to-managed-instance.md). 
> * Para más información sobre los diversos métodos de migración, consulte [Migración de una instancia de SQL Server a Instancia administrada de Azure SQL Database](sql-database-managed-instance-migrate.md).

## <a name="prerequisites"></a>Requisitos previos

En esta guía de inicio rápido:
- Se usan los recursos del inicio rápido [Creación de una instancia administrada](sql-database-managed-instance-get-started.md).
- Es necesario que el equipo tenga instalada la versión más reciente de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms).
- Es necesario el uso de SSMS para conectarse a la instancia administrada. Consulte estos inicios rápidos sobre procedimientos de conexión:
  * [Conexión a una Instancia administrada de Azure SQL Database desde una máquina virtual de Azure](sql-database-managed-instance-configure-vm.md)
  * [Configuración de una conexión de punto a sitio a una instancia administrada de Azure SQL Database desde el entorno local](sql-database-managed-instance-configure-p2s.md).


> [!NOTE]
> Para más información sobre cómo realizar una copia de seguridad de una base de datos de SQL Server y restaurarla mediante Azure Blob Storage y una clave de [Firma de acceso compartido (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1), consulte [Copia de seguridad de SQL Server en una dirección URL](sql-database-managed-instance-get-started-restore.md).

## <a name="restore-the-database-from-a-backup-file"></a>Restauración de la base de datos desde un archivo de copia de seguridad

En SSMS, siga estos pasos para restaurar la base de datos Wide World Importers en la instancia administrada. El archivo de copia de seguridad de base de datos se almacena en una cuenta de almacenamiento de blobs de Azure configurada previamente.

1. Abra SMSS y conéctese a la instancia administrada.

2. En el menú izquierdo, haga clic con el botón derecho en la instancia administrada y seleccione **Nueva consulta** para abrir una nueva ventana de consulta.

3. Ejecute el siguiente script de SQL, que usa una cuenta de almacenamiento configurada previamente y la clave SAS para [crear una credencial](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql?view=sql-server-2017) en la instancia administrada.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases] 
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D' 
   ```

    ![crear credencial](./media/sql-database-managed-instance-get-started-restore/credential.png)

  
3. Para comprobar sus credenciales, ejecute el siguiente script, que usa una dirección URL de [contenedor](https://azure.microsoft.com/services/container-instances/) para obtener una lista de archivos de copia de seguridad.

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![lista de archivos](./media/sql-database-managed-instance-get-started-restore/file-list.png)

4. Ejecute el siguiente script para restaurar la base de datos Wide World Importers.

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![Restauración](./media/sql-database-managed-instance-get-started-restore/restore.png)

5. Ejecute el siguiente script para realizar un seguimiento del estado de la restauración.

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')
   ```

6. Cuando se completa la restauración, se puede ver en el Explorador de objetos. 

## <a name="next-steps"></a>Pasos siguientes

- Para solucionar problemas con una copia de seguridad en la dirección URL, consulte [Prácticas recomendadas y solución de problemas de Copia de seguridad en URL de SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- Para información general sobre las opciones de conexión de aplicaciones, consulte [Conexión de las aplicaciones a Instancia administrada](sql-database-managed-instance-connect-app.md).
- Para realizar consultas con sus herramientas o lenguajes favoritos, consulte [Inicios rápidos: Conexión y consulta de Azure SQL Database](sql-database-connect-query.md).
