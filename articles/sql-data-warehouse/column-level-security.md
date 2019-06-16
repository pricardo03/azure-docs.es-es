---
title: Seguridad de nivel de columna de Azure SQL Data Warehouse | Microsoft Docs
description: La seguridad de nivel de columna (CLS) permite a los clientes controlar el acceso a columnas de la tabla de la base de datos según el contexto de ejecución del usuario o su pertenencia a grupos. CLS simplifica el diseño y la codificación de la seguridad de la aplicación. CLS permite implementar restricciones de acceso a columnas.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/02/2019
ms.author: kavithaj
ms.reviewer: igorstan, carlrab
ms.openlocfilehash: aa91bd586e064239d0e05c754427947963c9ee3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61082807"
---
# <a name="column-level-security"></a>Seguridad de nivel de columna
La seguridad de nivel de columna (CLS) permite a los clientes controlar el acceso a columnas de la tabla de la base de datos según el contexto de ejecución del usuario o su pertenencia a grupos.

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]

CLS simplifica el diseño y la codificación de la seguridad de la aplicación. CLS le permite implementar restricciones de acceso a columnas para proteger la información confidencial. Por ejemplo, garantiza que determinados usuarios puedan acceder solo a ciertas columnas de una tabla pertenecientes a su departamento. La lógica de restricción de acceso está en el nivel de la base de datos, en lugar de encontrarse en otro nivel de la aplicación lejos de los datos. La base de datos aplica las restricciones de acceso cada vez que se intenta acceder a los datos desde cualquier nivel. Esta restricción hace que el sistema de seguridad resulte más sólido y confiable, ya que reduce el área expuesta del sistema de seguridad global. Además, CLS también elimina la necesidad de introducir vistas para filtrar las columnas a fin de imponer restricciones de acceso a los usuarios.

Puede implementar CLS con la instrucción de T-SQL [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql). Con este mecanismo, se admite tanto la autenticación de SQL como la de Azure Active Directory (AAD).

![cls](./media/column-level-security/cls.png)

## <a name="syntax"></a>Sintaxis

```sql
GRANT <permission> [ ,...n ] ON
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]
    TO <database_principal> [ ,...n ]
    [ WITH GRANT OPTION ]
    [ AS <database_principal> ]
<permission> ::=
    SELECT
  | UPDATE
<database_principal> ::=
      Database_user
    | Database_role
    | Database_user_mapped_to_Windows_User
    | Database_user_mapped_to_Windows_Group
```

## <a name="example"></a>Ejemplo
En el ejemplo siguiente, se muestra cómo impedir que "TestUser" obtenga acceso a la columna "SSN" de la tabla "Membership":

Cree la tabla "Membership" con la columna SSN destinada a almacenar números del seguro social:

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

Permita que "TestUser" obtenga acceso a todas las columnas, excepto a la columna SSN, que contiene información confidencial:

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

Las consultas ejecutadas como "TestUser" producirán un error si incluyen la columna SSN:

```sql
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>Casos de uso
A continuación, se indican algunos ejemplos de cómo se usa CLS actualmente:
- Una empresa de servicios financieros solo permite a los administradores de cuentas obtener acceso a los números del seguro social (SSN), números de teléfono y otra información de identificación personal (PII) del cliente.
- Un profesional sanitario solo permite a los médicos y a las enfermeras obtener acceso a los historiales médicos confidenciales, mientras que no permite ver estos datos a los miembros del departamento de facturación.
