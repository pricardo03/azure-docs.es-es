---
title: Restricciones de características de base de datos de SQL Azure | Microsoft Docs
description: Restricciones de características de base de datos de SQL Azure mejora la seguridad de base de datos mediante la restricción de las características de la base de datos que puede ser por los atacantes obtener acceso a información en ellos.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: ac7a074e78def504a10b4daa07971f919f414a88
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66259458"
---
# <a name="azure-sql-database-feature-restrictions"></a>Restricciones de características de base de datos de SQL Azure

Un origen común de los ataques de SQL Server es a través de aplicaciones web que tienen acceso a la base de datos donde se usan diversas formas de ataques de inyección de SQL para recopilar información acerca de la base de datos.  Idealmente, código de la aplicación se desarrolla por lo que no se permite para la inyección de SQL.  Sin embargo, en grandes-bases de código que incluyen código heredado y externo, uno nunca puede ser seguro de que se han abordado todos los casos, por lo que las inyecciones SQL son un hecho que tenemos para protegerse.  El objetivo de restricciones de características es impedir que algunas formas de inyección de código SQL de la pérdida de información acerca de la base de datos, incluso cuando la inyección SQL es correcta.

## <a name="enabling-feature-restrictions"></a>Habilitación de restricciones de características

Habilitación de restricciones de características se realiza mediante el `sp_add_feature_restriction` procedimiento almacenado como sigue:

```sql
EXEC sp_add_feature_restriction <feature>, <object_class>, <object_name>
```

Se pueden restringidas las siguientes características:

| Característica          | DESCRIPCIÓN |
|------------------|-------------|
| 'N'ErrorMessages | Cuando restringido, se enmascare los datos de usuario en el mensaje de error. Consulte [mensajes de Error de restricción de características](#error-messages-feature-restriction) |
| N'Waitfor'       | Cuando restringido, el comando devolverá inmediatamente sin ningún retraso. Consulte [WAITFOR característica restricción](#waitfor-feature-restriction) |

El valor de `object_class` puede ser `N'User'` o `N'Role'` para indicar si `object_name` es un nombre de usuario o un nombre de rol en la base de datos.

En el ejemplo siguiente, se hará que todos los mensajes de error de usuario `MyUser` se enmascare:

```sql
EXEC sp_add_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="disabling-feature-restrictions"></a>Deshabilitar restricciones de características

Deshabilitar restricciones de características se realiza mediante el `sp_drop_feature_restriction` procedimiento almacenado como sigue:

```sql
EXEC sp_drop_feature_restriction <feature>, <object_class>, <object_name>
```

En el ejemplo siguiente se deshabilita el enmascaramiento de mensaje de error para el usuario `MyUser`:

```sql
EXEC sp_drop_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="viewing-feature-restrictions"></a>Restricciones de características de visualización

El `sys.sql_feature_restrictions` vista presenta todas las restricciones de característica definidas actualmente en la base de datos. Tiene las siguientes columnas:

| Nombre de la columna | Tipo de datos | DESCRIPCIÓN |
|-------------|-----------|-------------|
| class       | nvarchar(128) | Clase de objeto al que se aplica la restricción |
| objeto      | nvarchar(256) | Nombre del objeto al que se aplica la restricción |
| Característica     | nvarchar(128) | Característica que está restringido |

## <a name="feature-restrictions"></a>Restricciones de características

### <a name="error-messages-feature-restriction"></a>Restricción de características de los mensajes de error

Es un método común de ataque de inyección de SQL insertar el código que se produce un error.  Examinando el mensaje de error, un atacante puede obtener información sobre el sistema, lo que permite ataques más orientados adicionales.  Este ataque puede ser especialmente útil cuando la aplicación no muestra los resultados de una consulta, pero mostrar mensajes de error.

Considere la posibilidad de una aplicación web que tiene una solicitud en forma de:

```html
http://www.contoso.com/employee.php?id=1
```

Que ejecuta la consulta de base de datos siguientes:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Si el valor se pasa como el `id` parámetro a la solicitud de aplicación web se copia para reemplazar $EmpId en la consulta de base de datos, un atacante podría realizar la solicitud siguiente:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(DB_NAME() AS INT)=0
```

Y se devolverá el siguiente error, lo que permite al atacante obtenga información sobre el nombre de la base de datos:

```sql
Conversion failed when converting the nvarchar value 'HR_Data' to data type int.
```

Después de habilitar los mensajes de error de características restricción para el usuario de la aplicación en la base de datos, el mensaje de error devuelto es enmascarado de modo que no se pierde ninguna información interna sobre la base de datos:

```sql
Conversion failed when converting the ****** value '******' to data type ******.
```

De forma similar, el atacante podría realizar la solicitud siguiente:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(Salary AS TINYINT)=0
```

Y se devolverá el siguiente error, lo que permite al atacante obtenga información sobre el salario del empleado:

```sql
Arithmetic overflow error for data type tinyint, value = 140000.
```

Mediante una restricción de características de los mensajes de error, la base de datos devolvería:

```sql
Arithmetic overflow error for data type ******, value = ******.
```

### <a name="waitfor-feature-restriction"></a>Restricción de la característica WAITFOR

Una inyección de SQL oculta es cuando una aplicación no proporciona un atacante con los resultados de la consulta SQL insertado o con un mensaje de error, pero el atacante puede deducir la información de la base de datos mediante la creación de una consulta condicional en el que las dos ramas condicionales tomar una cantidad de tiempo de ejecución diferente. Al comparar el tiempo de respuesta, el atacante puede saber qué rama se ha ejecutado y, por tanto, obtener información sobre el sistema. Usa la variante de este ataque más sencilla la `WAITFOR` instrucción para insertar el retraso.

Considere la posibilidad de una aplicación web que tiene una solicitud en forma de:

```html
http://www.contoso.com/employee.php?id=1
```

que ejecuta la consulta de base de datos siguientes:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Si el valor pasado como el parámetro id a las solicitudes de aplicación web se copia para reemplazar $EmpId en la consulta de base de datos, un atacante puede realizar la solicitud siguiente:

```html
http://www.contoso.com/employee.php?id=1; IF SYSTEM_USER='sa' WAITFOR DELAY '00:00:05'
```

Y la consulta tendría un 5 segundos adicionales si el `sa` se estaba usando la cuenta. Si `WAITFOR` restricción de la característica está deshabilitada en la base de datos, el `WAITFOR` instrucción se omitirá y no la información se pierde con este ataque.