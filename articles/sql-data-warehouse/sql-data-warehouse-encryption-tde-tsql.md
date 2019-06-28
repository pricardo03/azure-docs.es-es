---
title: Cifrado de datos transparente en SQL Data Warehouse (T-SQL)| Microsoft Docs
description: Cifrado de datos transparente (TDE) en SQL Data Warehouse (T-SQL)
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/30/2019
ms.author: kavithaj
ms.reviewer: rortloff
ms.openlocfilehash: 1d62bc5cb080f2167f13bd58e9a8a334b8ae5163
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65146066"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Introducción al cifrado de datos transparente (TDE)
> [!div class="op_single_selector"]
> * [Información general sobre seguridad](sql-data-warehouse-overview-manage-security.md)
> * [Autenticación](sql-data-warehouse-authentication.md)
> * [Cifrado (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Cifrado (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Permisos necesarios
Para habilitar el Cifrado de datos transparente (TDE), debe ser un administrador o un miembro del rol dbmanager.

## <a name="enabling-encryption"></a>Habilitar el cifrado
Para habilitar TDE para una instancia de SQL Data Warehouse, siga estos pasos:

1. Conéctese a la base de datos *maestra* en el servidor que hospeda la base de datos mediante un inicio de sesión que es un administrador o un miembro del rol **dbmanager** en la base de datos maestra.
2. Ejecute la siguiente instrucción para cifrar la base de datos.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Deshabilitar el cifrado
Para deshabilitar TDE para una instancia de SQL Data Warehouse, siga estos pasos:

1. Conéctese a la base de datos *maestra* mediante un inicio de sesión que es un administrador o un miembro del rol **dbmanager** en la base de datos maestra.
2. Ejecute la siguiente instrucción para cifrar la base de datos.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Se debe reanudar una instancia de SQL Data Warehouse en pausa antes de realizar cambios en la configuración de TDE.
> 
> 

## <a name="verifying-encryption"></a>Comprobación del cifrado
Para comprobar el estado del cifrado para un SQL Data Warehouse, siga estos pasos:

1. Conéctese a la base de datos *maestra* o de instancia mediante un inicio de sesión que es un administrador o un miembro del rol **dbmanager** en la base de datos maestra.
2. Ejecute la siguiente instrucción para cifrar la base de datos.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Un resultado de ```1``` indica una base de datos cifrada, ```0``` indica una base de datos no cifrada.

## <a name="encryption-dmvs"></a>DMV de cifrado
* [sys.databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
