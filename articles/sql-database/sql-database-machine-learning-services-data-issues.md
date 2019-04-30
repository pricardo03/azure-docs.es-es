---
title: Trabajar con tipos de datos SQL y R y objetos
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Obtenga información sobre cómo trabajar con tipos de datos y objetos de datos en R con Azure SQL Database mediante Machine Learning Services (versión preliminar), incluidos los problemas comunes que pueden surgir.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: 069a2a5b3b26bf517b57034f05ab7080ab392319
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60702527"
---
# <a name="work-with-r-and-sql-data-in-azure-sql-database-machine-learning-services-preview"></a>Trabajar con datos SQL y R en Machine Learning Services (versión preliminar) de Azure SQL Database

En este artículo se describe algunos de los problemas comunes que pueden producirse al mover datos entre R y SQL Database en [Machine Learning Services (con R) en Azure SQL Database](sql-database-machine-learning-services-overview.md). La experiencia que obtenga a través de este ejercicio proporciona esencial en segundo plano cuando se trabaja con datos en su propio script.

Problemas comunes que pueden surgir incluyen:

- A veces no coinciden los tipos de datos
- Pueden que se produzcan conversiones implícitas
- A veces se requieren las operaciones CAST y convert
- R y SQL usan objetos de datos diferentes

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/) antes de empezar.

- Para ejecutar el código de ejemplo en estos ejercicios, primero debe tener una instancia de Azure SQL Database con Machine Learning Services (con R) habilitada. Durante la versión preliminar pública, Microsoft le incorporará y habilitará el aprendizaje automático para la base de datos nueva o existente. Siga los pasos que se indican en [Suscríbase a la versión preliminar](sql-database-machine-learning-services-overview.md#signup).

- Asegúrese de que tiene instalada la versión más reciente de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Puede ejecutar los scripts de R con herramientas de consulta o de otro tipo de administración de base de datos, pero en este tutorial usará SSMS.

## <a name="working-with-a-data-frame"></a>Trabajar con una trama de datos

Cuando el script devuelve resultados de R para SQL, debe devolver los datos como un **data.frame**. Cualquier otro tipo de objeto que se genere en su secuencia de comandos: ya sea una lista, factor, vector o datos binarios - debe convertirse en una trama de datos si desea que se entregan como parte de los resultados del procedimiento almacenado. Afortunadamente, hay varias funciones de R que se pueden cambiar otros objetos en una trama de datos. Incluso puede serializar un modelo binario y devolverlo en una trama de datos, lo cual lo hará más adelante en este artículo.

En primer lugar, vamos a experimentar con algunos objetos básicos de R - vectores, matrices y listas - y vea cómo la conversión a una trama de datos cambia el resultado pasado a SQL.

Comparar estos dos scripts de "Hello World" en R. Las secuencias de comandos son casi idénticos, pero el primero devuelve una sola columna de tres valores, mientras que el segundo devuelve tres columnas con un solo valor de cada uno.

**Ejemplo 1**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
'
    , @input_data_1 = N'';
```

**Ejemplo 2**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
    , @input_data_1 = N'';
```

¿Por qué son los resultados tan diferentes?

Normalmente se puede encontrar la respuesta mediante el uso de R `str()` comando. Agregue la función `str(object_name)` en cualquier lugar en el script de R para que los datos del objeto especificado de R devuelve el esquema como un mensaje informativo. Puede ver los mensajes en el **mensajes** ficha en SSMS.

Para averiguar por qué Example 1 y 2 de ejemplo tienen los resultados son diferentes, inserte la línea `str(OutputDataSet)` al final de la `@script` definición de variable en cada instrucción, similar al siguiente:

**Ejemplo 1 con la función str agregado**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

**Ejemplo 2 con la función str agregado**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(c("hello"), " ", c("world"));
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

Ahora, revise el texto en **mensajes** para ver por qué el resultado es diferente.

**Resultados de Example 1**

```text
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Resultados de Example 2**

```text
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Como puede ver, un pequeño cambio en la sintaxis de R tenía un gran efecto en el esquema de los resultados. Para obtener detalles, las diferencias en los tipos de datos de R se explican con detalle en la *estructuras de datos* sección ["Advanced R" de Hadley Wickham](http://adv-r.had.co.nz).

Por ahora, tenga presente que deberá comprobar los resultados esperados cuando convirtamos objetos de R en tramas de datos.

> [!TIP]
> También puede usar las funciones de identidad de R, como `is.matrix`, `is.vector`para devolver información sobre la estructura de datos internos.

## <a name="implicit-conversion-of-data-objects"></a>Conversión implícita de objetos de datos

Cada objeto de datos de R tiene sus propias reglas sobre cómo controlar los valores cuando se combina con otros objetos de datos si los dos objetos de datos tienen el mismo número de dimensiones, o si cualquier objeto de datos contiene tipos de datos heterogéneos.

Por ejemplo, suponga que desea realizar la multiplicación de matrices mediante R. Desee multiplicar una matrix de una sola columna con los tres valores por una matriz con cuatro valores y esperan como resultado una matriz de 4 x 3.

En primer lugar, cree una tabla pequeña de datos de prueba.

```sql
CREATE TABLE RTestData (col1 INT NOT NULL)

INSERT INTO RTestData
VALUES (1);

INSERT INTO RTestData
VALUES (10);

INSERT INTO RTestData
VALUES (100);
GO
```

Ahora ejecute el siguiente script.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:15);
OutputDataSet <- as.data.frame(x %*% y);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col1] INT
            , [Col2] INT
            , [Col3] INT
            , Col4 INT
            ));
```

En segundo plano, la columna de tres valores se convierte en una matriz de una sola columna. Dado que este es simplemente un caso especial de una matriz de R, la matriz `y` se convierte implícitamente en una matriz de una sola columna para que los dos argumentos que se ajustan.

**Resultados**

|Col1|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1.500|

Sin embargo, tenga en cuenta lo que sucede cuando se cambia el tamaño de la matriz `y`.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:14);
OutputDataSet <- as.data.frame(y %*% x);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS(([Col1] INT));
```

Ahora R devuelve un valor único como resultado.

**Resultados**
    
|Col1|
|---|
|1542|

¿Por qué? En este caso, porque los dos argumentos se pueden tratar como vectores de la misma longitud, R devuelve el producto interior como una matriz.  Este es el comportamiento esperado según las reglas de álgebra lineal. Sin embargo, podría producir problemas si la aplicación auxiliar espera que el esquema de salida no cambie nunca!

## <a name="merge-or-multiply-columns-of-different-length"></a>Combinar o multiplicar columnas de longitud diferente

R proporciona gran flexibilidad para trabajar con vectores de diferentes tamaños y de combinar estas estructuras de tipo de columna en tramas de datos. Las listas de vectores pueden ser similar a una tabla, pero no siguen todas las reglas que rigen las tablas de base de datos.

Por ejemplo, el siguiente script define una matriz numérica de longitud 6 y lo almacena en la variable de R `df1`. La matriz numérica, a continuación, se combina con los enteros de la tabla RTestData (creado anteriormente) que contiene tres (3) valores, que se va a crear una nueva trama de datos, `df2`.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
df1 <- as.data.frame( array(1:6) );
df2 <- as.data.frame( c( InputDataSet , df1 ));
OutputDataSet <- df2;
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col2] INT NOT NULL
            , [Col3] INT NOT NULL
            ));
```

Para rellenar la trama de datos, R repite los elementos recuperados de RTestData tantas veces como sea necesario para que coincida con el número de elementos de la matriz `df1`.

**Resultados**
    
|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

Recuerde que una trama de datos solo es similar a una tabla, pero es realmente una lista de vectores.

## <a name="cast-or-convert-sql-data"></a>CAST o convert de datos de SQL

R y SQL no usan los mismos tipos de datos, por lo que al ejecutar una consulta en SQL para obtener datos y después lo trasladará al runtime de R, algún tipo de conversión implícita normalmente realiza. Otro conjunto de conversiones tiene lugar cuando se devuelven datos de R a SQL.

- SQL inserta los datos de la consulta en el proceso de R y lo convierte en una representación interna para la mayor eficacia.
- El runtime de R carga los datos en una variable data.frame y realiza sus propias operaciones en los datos.
- El motor de base de datos devuelve los datos a SQL con una conexión segura interna y presenta los datos en términos de tipos de datos SQL.
- Obtener los datos mediante la conexión a SQL mediante una biblioteca de cliente o de red que se puede emitir consultas SQL y controlar los conjuntos de datos tabulares. Esta aplicación cliente puede afectar a los datos de otras maneras.

Para ver cómo funciona esto, ejecute una consulta como esta el [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) almacenamiento de datos. Esta vista devuelve los datos empleados en crear previsiones de ventas.

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
    , CAST(ModelRegion AS VARCHAR(50)) AS ProductSeries
    , Amount
FROM [AdventureWorksDW].[dbo].[vTimeSeries]
WHERE [ModelRegion] = 'M200 Europe'
ORDER BY ReportingDate ASC
```

> [!NOTE]
> Puede usar cualquier versión de AdventureWorks, o crear una consulta diferente con una base de datos de su elección. El punto es intentar controlar algunos datos que contiene texto, fecha y hora y valores numéricos.

Ahora, pruebe a usar esta consulta como entrada para el procedimiento almacenado.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
str(InputDataSet);
OutputDataSet <- InputDataSet;
'
    , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Si se produce un error, probablemente deberá realizar algunas modificaciones en el texto de consulta. Por ejemplo, el predicado de cadena en la cláusula WHERE debe estar delimitado por dos conjuntos de comillas simples.

Después de obtener la consulta funciona, revise los resultados de la `str` función para ver cómo R trata los datos de entrada.

**Resultados**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- Se ha procesado la columna de fecha y hora con el tipo de datos R, **POSIXct**.
- La columna de texto "ProductSeries" se ha identificado como un **factor**, lo que significa que una variable de categoría. Los valores de cadena se tratan como factores de forma predeterminada. Si pasa una cadena a R, se convierte en un entero para uso interno y, a continuación, asignar a la cadena de salida.

## <a name="summary"></a>Resumen

Desde incluso estos ejemplos breves, puede ver la necesidad de comprobar los efectos de conversión de datos al pasar a SQL las consultas como entrada. Dado que no se admiten algunos tipos de datos SQL mediante R, considere la posibilidad de estas formas de evitar errores:

- Los datos de prueba de antemano y compruebe las columnas o valores en el esquema que podría ser un problema cuando se pasa al código de R.
- Especificar las columnas en el origen de datos de entrada individualmente, en lugar de usar `SELECT *`y saber cómo se tratará cada columna.
- Realice conversiones explícitas según sea necesario al preparar los datos de entrada, para evitar sorpresas.
- Evite pasar columnas de datos (por ejemplo, ROWGUID o GUID) que provocan errores y que no son útiles para el modelado.

Para obtener más información sobre los tipos de datos de R admitidos y no admitidos, vea [tipos de datos y las bibliotecas de R](/sql/advanced-analytics/r/r-libraries-and-data-types.md).
