---
title: 'Inicio rápido: Creación de un clasificador de carga de trabajo - T-SQL | Microsoft Docs'
description: Uso de T-SQL para crear un clasificador de carga de trabajo con importancia alta
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: workload management
ms.date: 05/01/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 2074ea1028f6a5e8a0251c6d0c9519361db6ba8d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150384"
---
# <a name="quickstart-create-a-workload-classifier-using-t-sql"></a>Inicio rápido: Creación un clasificador de carga de trabajo mediante T-SQL

En este inicio rápido creará un clasificador de carga de trabajo rápidamente con importancia alta para el director general de su organización. Este clasificador de carga de trabajo permitirá que las consultas del director general tengan prioridad sobre otras de menor importancia en la cola.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

> [!NOTE]
> La creación de una instancia de SQL Data Warehouse puede dar lugar a un nuevo servicio facturable.  Para más información, consulte [Precios de Azure SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>
>

## <a name="prerequisites"></a>Requisitos previos

En este inicio rápido se da por supuesto que ya tiene una instancia de SQL Data Warehouse y que tiene permisos CONTROL DATABASE. Si tiene que crear una, consulte [Guía de inicio rápido: Creación de una instancia de Azure SQL Data Warehouse en Azure Portal, y realización de consultas en ella](create-data-warehouse-portal.md) para crear un almacenamiento de datos denominado **mySampleDataWarehouse**.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-login-for-theceo"></a>Creación del inicio de sesión para TheCEO

Cree un inicio de sesión de autenticación de SQL Server mediante la base de datos `master` con [CREAR INICIO DE SESIÓN](/sql/t-sql/statements/create-login-transact-sql) para "TheCEO".

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'TheCEO')
BEGIN
CREATE LOGIN [TheCEO] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Crear usuario

[Cree un usuario](/sql/t-sql/statements/create-user-transact-sql?view=azure-sqldw-latest), "TheCEO", en mySampleDataWarehouse

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'THECEO')
BEGIN
CREATE USER [TheCEO] FOR LOGIN [TheCEO]
END
;
```

## <a name="create-a-workload-classifier"></a>Creación del clasificador de carga de trabajo

Cree un [clasificador de carga de trabajo](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) para "TheCEO" con importancia alta.

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO];
CREATE WORKLOAD CLASSIFIER [wgcTheCEO]
WITH (WORKLOAD_GROUP = 'xlargerc'
      ,MEMBERNAME = 'TheCEO'
      ,IMPORTANCE = HIGH);
```

## <a name="view-existing-classifiers"></a>Visualización de los clasificadores existentes

```sql
SELECT * FROM sys.workload_management_workload_classifiers
```

## <a name="clean-up-resources"></a>Limpieza de recursos

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO]
DROP USER [TheCEO]
;
```

Se le está cobrando por unidades de almacenamiento de datos y por los datos almacenados en el almacenamiento de datos. Estos recursos de proceso y de almacenamiento se facturan por separado.

- Si desea conservar los datos de almacenamiento, puede pausar el proceso cuando no está utilizando el almacenamiento de datos. Si se hace una pausa en el proceso, solo se carga el almacenamiento de datos. Cuando esté listo para trabajar con los datos, reanude el proceso.
- Si desea quitar cobros futuros, puede eliminar el almacenamiento de datos.

Siga estos pasos para limpiar los recursos.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y seleccione el almacenamiento de datos.

    ![Limpieza de recursos](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Para pausar el proceso, seleccione el botón **Pausar**. Cuando el almacenamiento de datos se haya puesto en pausa, verá un botón **Iniciar**.  Para reanudar el proceso, seleccione **Iniciar**.

3. Para quitar el almacenamiento de datos para que no se le cobre por proceso o almacenamiento, seleccione **Eliminar**.

4. Para eliminar el servidor SQL que creó, seleccione **mynewserver-20180430.database.windows.net** en la imagen anterior y seleccione **Eliminar**.  Debe tener cuidado con este procedimiento, ya que la eliminación del servidor elimina también todas las bases de datos asignadas al servidor.

5. Para quitar el grupo de recursos, seleccione **myResourceGroup** y **Eliminar grupo de recursos**.

## <a name="next-steps"></a>Pasos siguientes

Ahora ha creado un clasificador de carga de trabajo. Ejecute algunas consultas como TheCEO para ver cómo responden. Consulte [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) para ver las consultas y la importancia asignada.

Para más información sobre la administración de cargas de trabajo de SQL Data Warehouse, consulte los artículos sobre la [Importancia de la carga de trabajo de SQL Data Warehouse](sql-data-warehouse-workload-importance.md) y la [Clasificación de la carga de trabajo de SQL Data Warehouse](sql-data-warehouse-workload-classification.md).
