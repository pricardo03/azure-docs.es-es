---
title: Restricciones de las características de Azure SQL Database | Microsoft Docs
description: Las restricciones de las características de Azure SQL Database mejoran la seguridad de base de datos mediante la restricción de las características de la base de datos que pueden utilizar los atacantes para acceder a la información contenida en dichas bases de datos.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: barmichal
ms.author: mibar
ms.reviewer: vanto
ms.date: 03/22/2019
ms.openlocfilehash: f2fd6cb73428c69fbb27cb93377f851a4e06221d
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70959134"
---
# <a name="azure-sql-database-feature-restrictions"></a>Restricciones de las características de Azure SQL Database

Un origen común de los ataques de SQL Server se lleva a cabo a través de aplicaciones web que tienen acceso a la base de datos en la que se usan diversas formas de ataques de inyección de código SQL para recopilar información sobre la base de datos.  Idealmente, el código de la aplicación se desarrolla de manera que no se permita la inyección de código SQL.  Sin embargo, en las bases de código de gran tamaño que incluyen código heredado y externo, no es posible estar seguro de que se han abordado todos los casos, por lo que las inyecciones de código SQL son un elemento de protección del que disponemos.  El objetivo de las restricciones de características es el de ayudar a impedir que algunas formas de inyección de código SQL filtren información sobre la base de datos, incluso cuando la inyección de código SQL es correcta.

## <a name="enabling-feature-restrictions"></a>Habilitación de restricciones de características

La habilitación de restricciones de características se realiza mediante el procedimiento almacenado `sp_add_feature_restriction` de la siguiente forma:

```sql
EXEC sp_add_feature_restriction <feature>, <object_class>, <object_name>
```

Se pueden restringir las siguientes características:

| Característica          | DESCRIPCIÓN |
|------------------|-------------|
| N'ErrorMessages' | Cuando se restringe, se enmascaran los datos de usuario dentro del mensaje de error. Consultar [Restricción de característica de mensajes de error](#error-messages-feature-restriction) |
| N'Waitfor'       | Cuando se restringe, se devolverá el comando inmediatamente sin ningún retraso. Consultar [Restricción de característica WAITFOR](#waitfor-feature-restriction) |

El valor de `object_class` puede ser `N'User'` o `N'Role'` para denotar si `object_name` es un nombre de usuario o un nombre de rol en la base de datos.

En el ejemplo siguiente se enmascararán todos los mensajes de error del usuario `MyUser`:

```sql
EXEC sp_add_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="disabling-feature-restrictions"></a>Deshabilitación de restricciones de características

La deshabilitación de restricciones de características se realiza mediante el procedimiento almacenado `sp_drop_feature_restriction` de la siguiente forma:

```sql
EXEC sp_drop_feature_restriction <feature>, <object_class>, <object_name>
```

En el siguiente ejemplo se deshabilita el enmascaramiento del mensaje de error para el usuario `MyUser`:

```sql
EXEC sp_drop_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="viewing-feature-restrictions"></a>Visualización de restricciones de características

La vista `sys.sql_feature_restrictions` presenta todas las restricciones de características definidas actualmente en la base de datos. Cuenta con las columnas siguientes:

| Nombre de la columna | Tipo de datos | DESCRIPCIÓN |
|-------------|-----------|-------------|
| class       | nvarchar(128) | Clase de objeto al que se aplica la restricción. |
| object      | nvarchar(256) | Nombre del objeto al que se aplica la restricción. |
| feature     | nvarchar(128) | Característica restringida. |

## <a name="feature-restrictions"></a>Restricciones de características

### <a name="error-messages-feature-restriction"></a>Restricción de característica de mensajes de error

Un método de ataque por inyección de código SQL frecuente es insertar código que provoca un error.  Mediante el examen del mensaje de error, un atacante puede obtener información sobre el sistema, lo que permite realizar ataques adicionales más orientados.  Este ataque puede ser especialmente útil cuando la aplicación no muestra los resultados de una consulta, pero muestra mensajes de error.

Considere la posibilidad de una aplicación web que tenga una solicitud con esta forma:

```html
http://www.contoso.com/employee.php?id=1
```

Que ejecuta la siguiente consulta de base de datos:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Si el valor que se pasa como parámetro `id` a la solicitud de aplicación web se copia para reemplazar $EmpId en la consulta de base de datos, un atacante podría realizar la solicitud siguiente:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(DB_NAME() AS INT)=0
```

Aparecerá el siguiente error que permitirá al atacante obtener el nombre de la base de datos:

```sql
Conversion failed when converting the nvarchar value 'HR_Data' to data type int.
```

Después de habilitar la restricción de características de mensajes de error para el usuario de la aplicación en la base de datos, se enmascara un mensaje de error devuelto de modo que no se pierde información interna sobre la base de datos:

```sql
Conversion failed when converting the ****** value '******' to data type ******.
```

De forma similar, el atacante podría realizar la solicitud siguiente:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(Salary AS TINYINT)=0
```

Aparecerá el siguiente error que permitirá al atacante conocer el salario del empleado:

```sql
Arithmetic overflow error for data type tinyint, value = 140000.
```

Mediante una restricción de características de mensajes de error, la base de datos devolverá:

```sql
Arithmetic overflow error for data type ******, value = ******.
```

### <a name="waitfor-feature-restriction"></a>Restricción de la característica WAITFOR

Una inyección de código SQL oculta se produce cuando una aplicación no proporciona al atacante los resultados del código SQL inyectado o un mensaje de error, pero el atacante puede deducir la información de la base de datos mediante la creación de una consulta condicional en la que las dos ramas condicionales tardan una cantidad de tiempo diferente en ejecutarse. Al comparar el tiempo de respuesta, el atacante puede saber qué rama se ha ejecutado y, por lo tanto, obtener información sobre el sistema. La variante más sencilla de este ataque es usar la instrucción `WAITFOR` para especificar el retraso.

Considere la posibilidad de una aplicación web que tenga una solicitud con esta forma:

```html
http://www.contoso.com/employee.php?id=1
```

Que ejecuta la siguiente consulta de base de datos:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Si el valor que se pasa como parámetro id a la solicitud de aplicación web se copia para reemplazar $EmpId en la consulta de base de datos, un atacante puede realizar la solicitud siguiente:

```html
http://www.contoso.com/employee.php?id=1; IF SYSTEM_USER='sa' WAITFOR DELAY '00:00:05'
```

Además, la consulta tardaría 5 segundos adicionales si se utiliza la cuenta de `sa`. Si la restricción de la característica `WAITFOR` está deshabilitada en la base de datos, la instrucción `WAITFOR` se ignorará y no se filtrará información cuando se produzca este ataque.