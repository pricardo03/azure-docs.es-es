---
title: Instrucción CREATE TABLE AS SELECT (CTAS) en Azure SQL Data Warehouse | Microsoft Docs
description: Sugerencias para la codificación con la instrucción CREATE TABLE AS SELECT (CTAS) en Azure SQL Data Warehouse para el desarrollo de soluciones.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/26/2019
ms.author: mlee3gsd
ms.reviewer: igorstan
ms.openlocfilehash: f791f460efec1b84533379e74add003619dbac6f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521574"
---
# <a name="using-create-table-as-select-ctas-in-azure-sql-data-warehouse"></a>Uso de la instrucción CREATE TABLE AS SELECT (CTAS) en Azure SQL Data Warehouse
Sugerencias para la codificación con la instrucción CREATE TABLE AS SELECT (CTAS) de T-SQL en Azure SQL Data Warehouse para el desarrollo de soluciones.

## <a name="what-is-create-table-as-select-ctas"></a>¿Qué es la instrucción CREATE TABLE AS SELECT (CTAS)?

La instrucción [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) o CTAS es una de las características más importantes de T-SQL disponibles. Se trata de una operación paralela que crea una nueva tabla basada en la salida de una instrucción SELECT. CTAS es la forma más sencilla y rápida para crear e insertar datos en una tabla con un solo comando. 

## <a name="selectinto-vs-ctas"></a>SELECT..INTO frente a CTAS
Puede considerarse CTAS como una versión muy cargada de la instrucción [SELECT...INTO](/sql/t-sql/queries/select-into-clause-transact-sql).

Aquí tiene un ejemplo de una instrucción SELECT..INTO sencilla:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

SELECT..INTO, sin embargo, no permite cambiar el método de distribución o el tipo de índice como parte de la operación. `[dbo].[FactInternetSales_new]` se crea utilizando el tipo de distribución predeterminada como ROUND_ROBIN y la estructura de tabla predeterminada como CLUSTERED COLUMNSTORE INDEX.

Uso de CTAS, son capaces de especificar la distribución de los datos de tabla, así como el tipo de estructura de tabla.
Para convertir el ejemplo anterior en CTAS:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,   CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

 

> [!NOTE]
> Si solo intenta cambiar el índice en la operación `CTAS` y la tabla de origen presenta una distribución hash, la operación `CTAS` ofrece mejores resultados si mantiene el mismo tipo de datos y de columna de distribución. Esto evitará el movimiento cruzado de los datos de distribución durante la operación que es más eficaz.
> 
> 

## <a name="using-ctas-to-copy-a-table"></a>Uso de CTAS para copiar una tabla
Quizás uno de los usos más comunes de `CTAS` es el de crear una copia de una tabla, con el fin de poder cambiar el DDL. Si, por ejemplo, se creó inicialmente la tabla como `ROUND_ROBIN` y ahora desea cambiarla a una tabla distribuida en una columna, `CTAS` es cómo cambiaría la columna de distribución. `CTAS` también se puede usar para cambiar las particiones, el indexado o los tipos de columnas.

Supongamos que creó esta tabla con el tipo de distribución predeterminado, `ROUND_ROBIN` distribuido, ya que no se especificó ninguna columna de distribución en `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Ahora desea crear una copia nueva de esta tabla con un índice de almacén de columnas en clúster, a fin de poder aprovechar el rendimiento de las tablas de almacén de columnas en clúster. También desea distribuir esta tabla en ProductKey, ya que prevé combinaciones en esta columna y desea evitar el movimiento de datos durante las mismas. Por último, también desea agregar la creación de particiones en OrderDateKey, con el fin de eliminar rápidamente datos antiguos mediante la anulación de particiones anteriores. Aquí es la instrucción CTAS que copiaría la tabla antigua en una tabla nueva.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Finalmente, puede cambiar el nombre de las tablas y ponerle a la tabla nueva el nombre de la antigua, para luego anular esta última.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

## <a name="using-ctas-to-work-around-unsupported-features"></a>Uso de CTAS para solucionar características no admitidas
CTAS también se puede utilizar para solucionar algunas de las características no admitidas que se indican a continuación. Este método a menudo puede resultar para ser una situación ventajosa ya no solo el código será compatible pero con frecuencia se ejecutará más rápido en SQL Data Warehouse. Este rendimiento es el resultado de su diseño totalmente paralelizado. Algunas situaciones que se pueden solucionar con CTAS incluyen:

* ANSI JOINS en UPDATE
* ANSI JOINS en DELETE
* Instrucción MERGE

> [!NOTE]
> Intente pensar: "primero CTAS". Si cree que puede resolver un problema mediante `CTAS` , este puede ser el mejor enfoque, incluso si como consecuencia escribe más datos.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>Sustitución de una combinación ANSI en instrucciones update
Puede encontrarse con que tiene una actualización compleja que combina más de dos tablas con una sintaxis de combinación ANSI para realizar una operación UPDATE o DELETE.

Imagine que hubiera que actualizar esta tabla:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(    [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,    [CalendarYear]                    SMALLINT        NOT NULL
,    [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

La consulta original podría haber tenido un aspecto similar al siguiente:

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT    [EnglishProductCategoryName]
        ,        [CalendarYear]
        ,        SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Dado que SQL Data Warehouse no admite combinaciones ANSI en la cláusula `FROM` de una instrucción `UPDATE`, no puede copiar este código sin cambiarlo ligeramente.

Puede usar una mezcla de un `CTAS` y una combinación implícita para reemplazar este código:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,        ISNULL(CAST([CalendarYear] AS SMALLINT),0)                         AS [CalendarYear]
,        ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                        AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,        [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>Sustitución de una combinación ANSI en instrucciones delete
A veces el mejor enfoque para eliminar los datos es el uso de `CTAS`. En lugar de eliminar los datos, seleccione los datos que desea conservar. Esto es especialmente cierto para `DELETE` sintaxis de combinación de instrucciones que usan ANSI, ya que SQL Data Warehouse no admite combinaciones ANSI en la `FROM` cláusula de una `DELETE` instrucción.

A continuación se muestra un ejemplo de una instrucción DELETE convertida:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Sustitución de instrucciones MERGE
Las instrucciones merge se pueden sustituir, al menos en parte, mediante el uso de CTAS. Puede consolidar INSERT y UPDATE en una única instrucción. Deben estar restringidos a los registros eliminados desde la `SELECT` instrucción a omitir en los resultados.

El ejemplo siguiente es para una operación UPSERT:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>Recomendación de CTAS: establezca explícitamente el tipo de datos y la nulabilidad de salida
Al migrar código, podría encontrarse que ejecutando este tipo de patrón de codificación:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instintivamente, puede pensar que debería migrar este código a CTAS y sería correcto. Sin embargo, hay un problema oculto aquí.

El siguiente código NO produce el mismo resultado:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Observe que la columna "resultado" traslada los valores de tipo y nulabilidad de los datos de la expresión. Esto puede provocar sutiles variaciones en los valores si no tiene cuidado.

Intente lo siguiente como ejemplo:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

El valor almacenado para el resultado es diferente. Como el valor almacenado en la columna de resultados se usa en otras expresiones, el error se vuelve incluso más significativo.

![Resultados de CTAS](media/sql-data-warehouse-develop-ctas/ctas-results.png)

Esto es especialmente importante para las migraciones de datos. Aunque se puede decir que la segunda consulta es más precisa, hay un problema. Los datos serán diferentes en comparación con el sistema de origen y eso conduce a preguntas sobre la integridad de la migración. Éste es uno de esos casos raros en los que la respuesta "incorrecta" es realmente la adecuada.

El motivo de que veamos esta disparidad entre los dos resultados se debe a la conversión implícita de los tipos. En el primer ejemplo, la tabla define la definición de columna. Cuando se inserta la fila, se produce una conversión de tipos implícita. En el segundo ejemplo, no hay ninguna conversión de tipos implícita como la expresión define el tipo de datos de la columna. Observe también que la columna del segundo ejemplo se ha definido como una columna que acepta valores NULL mientras que en el primer ejemplo no. Cuando se creó la tabla en el primer ejemplo, se definió explícitamente la nulabilidad de la columna. En el segundo ejemplo, se ha dejado en la expresión y de forma predeterminada, se daría lugar a una definición NULL.  

Para resolver estos problemas, debe establecer explícitamente la conversión de tipos y la nulabilidad en la parte SELECT de la instrucción CTAS. No se pueden establecer estas propiedades en la parte create table.

En el ejemplo siguiente se muestra cómo corregir el código:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Tenga en cuenta lo siguiente:

* Se podría haber usado CAST o CONVERT.
* ISNULL se usa para forzar la nulabilidad no COALESCE.
* ISNULL es la función más externa.
* La segunda parte de ISNULL es una constante, es decir, 0.

> [!NOTE]
> Para que la nulabilidad se establezca correctamente, es fundamental usar ISNULL y no COLAESCE. COALESCE no es una función determinista y, por tanto, el resultado de la expresión siempre será NULLable. ISNULL es diferente. Es determinista. Por lo tanto, cuando la segunda parte de la función ISNULL es una constante o un literal, el valor resultante será NOT NULL.
> 
> 

Esta sugerencia no solo es útil para garantizar la integridad de los cálculos. También es importante para la modificación de particiones de tabla. Imagine que tiene esta tabla definida como hecho:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Sin embargo, el campo de valor es una expresión calculada que no forma parte de los datos de origen.

Para crear el conjunto de datos con particiones, es posible que desee hacer lo siguiente:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

La consulta se ejecutaría perfectamente. El problema se produce al intentar realizar el cambio de partición. Las definiciones de tabla no coinciden. Para hacer que coincida con las definiciones de tabla, se debe modificar para agregar la CTAS un `ISNULL` función para conservar el atributo de nulabilidad de la columna.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Por lo tanto, puede ver que la coherencia de los tipos y el mantenimiento de las propiedades de nulabilidad en CTAS es una buena práctica de ingeniería. Ayuda a mantener la integridad de los cálculos y también garantiza que la modificación de particiones es posible.

Hacer referencia a la [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) documentación. Es una de las instrucciones más importantes de SQL Data Warehouse. Asegúrese de que la comprende perfectamente.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).

