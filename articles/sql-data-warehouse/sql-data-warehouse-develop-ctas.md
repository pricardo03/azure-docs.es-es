---
title: Instrucción CREATE TABLE AS SELECT (CTAS) en Azure SQL Data Warehouse | Microsoft Docs
description: Explicación y ejemplos de con la instrucción CREATE TABLE AS SELECT (CTAS) en Azure SQL Data Warehouse para desarrollar soluciones.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/26/2019
ms.author: mlee3gsd
ms.reviewer: jrasnick
ms.custom: seoapril2019
ms.openlocfilehash: ea95a13277927b485bb9da3b75b84cce4337bf88
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280441"
---
# <a name="create-table-as-select-ctas-in-azure-sql-data-warehouse"></a>CREATE TABLE AS SELECT (CTAS) en Azure SQL Data Warehouse

Explicación y ejemplos de codificación con la instrucción T-SQL CREATE TABLE AS SELECT (CTAS) en Azure SQL Data Warehouse para desarrollar soluciones.

## <a name="create-table-as-select-ctas"></a>CREATE TABLE AS SELECT (CTAS)

La instrucción [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) o CTAS es una de las características más importantes de T-SQL disponibles. CTAS es una operación paralela que crea una nueva tabla basada en la salida de una instrucción SELECT. CTAS es la forma más sencilla y rápida para crear e insertar datos en una tabla con un solo comando.

## <a name="selectinto-vs-ctas"></a>SELECT..INTO frente a CTAS

CTAS es una versión muy cargada de la [seleccione... EN](/sql/t-sql/queries/select-into-clause-transact-sql) instrucción.

Aquí tiene un ejemplo de una instrucción SELECT..INTO sencilla:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

SELECCIONE... EN que no permite cambiar el método de distribución o el tipo de índice como parte de la operación. `[dbo].[FactInternetSales_new]` se crea utilizando el tipo de distribución predeterminado de ROUND_ROBIN y la estructura de tabla predeterminada como CLUSTERED COLUMNSTORE INDEX.

Uso de CTAS es posible especificar ambos tipo de la distribución de los datos de tabla, así como la estructura de tabla. Para convertir el ejemplo anterior en CTAS:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
   ,CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [!NOTE]
> Si solo intenta cambiar el índice en la operación `CTAS` y la tabla de origen presenta una distribución hash, la operación `CTAS` ofrece mejores resultados si mantiene el mismo tipo de datos y de columna de distribución. Esto evitará el movimiento cruzado de los datos de distribución durante la operación que es más eficaz.

## <a name="using-ctas-to-copy-a-table"></a>Uso de CTAS para copiar una tabla

Quizás uno de los más comunes usos del `CTAS` es crear una copia de una tabla con el fin de cambiar el DDL. Si, por ejemplo, se creó inicialmente la tabla como `ROUND_ROBIN` y ahora desea cambiarla a una tabla distribuida en una columna, `CTAS` es cómo cambiaría la columna de distribución. `CTAS` También puede utilizarse para cambiar los tipos de particiones, la indexación o columna.

Supongamos que ha creado esta tabla con el tipo de distribución predeterminado de `ROUND_ROBIN` al no especificar una columna de distribución en el `CREATE TABLE`.

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

Ahora desea crear una copia nueva de esta tabla con un índice de almacén de columnas en clúster, a fin de poder aprovechar el rendimiento de las tablas de almacén de columnas en clúster. También desea distribuir esta tabla en ProductKey, ya que prevé combinaciones en esta columna y desea evitar el movimiento de datos durante las mismas. Por último, también desea agregar la creación de particiones en OrderDateKey para que se puede eliminar rápidamente datos antiguos quitando particiones antiguas. Aquí es la instrucción CTAS que copiaría la tabla antigua en una tabla nueva.

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
> Intente pensar: "primero CTAS". Resolver un problema mediante `CTAS` suele ser un buen enfoque, incluso si está escribiendo más datos como resultado.

## <a name="ansi-join-replacement-for-update-statements"></a>Sustitución de una combinación ANSI en instrucciones update

Es posible que tener una actualización compleja que combina más de dos tablas mediante la sintaxis de combinación ANSI para realizar la actualización o eliminación.

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

La consulta original podría haber tenido un aspecto similar a este ejemplo:

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

SQL Data Warehouse no admite combinaciones ANSI en la `FROM` cláusula de una `UPDATE` instrucción, por lo que no puede usar el ejemplo anterior sin modificación alguna.

Puede usar una combinación de un `CTAS` y una combinación implícita para reemplazar el ejemplo anterior:

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

A veces, el mejor enfoque para eliminar datos es usar `CTAS`, especialmente para `DELETE` las instrucciones que usan ANSI la sintaxis de combinación. Esto es porque SQL Data Warehouse no admite combinaciones ANSI en la `FROM` cláusula de una `DELETE` instrucción. En lugar de eliminar los datos, seleccione los datos que desea conservar.

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

Las instrucciones merge se pueden sustituir, al menos en parte, mediante el uso de CTAS. Puede combinar la INSERCIÓN y la actualización en una sola instrucción. Deben estar restringidos a los registros eliminados desde la `SELECT` instrucción a omitir en los resultados.

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

Instintivamente, puede pensar que debería migrar este código a CTAS y sería correcto. Sin embargo, hay un problema oculto.

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

Observe que la columna "resultado" traslada los valores de tipo y nulabilidad de los datos de la expresión. Que transporta los datos al día el tipo puede provocar sutiles variaciones en los valores si no tiene cuidado.

Pruebe este ejemplo:

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

Esto es importante para las migraciones de datos. Aunque la segunda consulta es posiblemente más precisa, hay un problema. Los datos serán diferentes en comparación con el sistema de origen y eso conduce a preguntas sobre la integridad de la migración. Éste es uno de esos casos raros en los que la respuesta "incorrecta" es realmente la adecuada.

El motivo que se vea una discrepancia entre los dos resultados como es debido a la conversión de tipos implícita. En el primer ejemplo, la tabla define la definición de columna. Cuando se inserta la fila, se produce una conversión de tipos implícita. En el segundo ejemplo, no hay ninguna conversión de tipos implícita como la expresión define el tipo de datos de la columna. Observe también que la columna del segundo ejemplo se ha definido como una columna que acepta valores NULL mientras que en el primer ejemplo no. Cuando se creó la tabla en el primer ejemplo, se definió explícitamente la nulabilidad de la columna. En el segundo ejemplo, se ha dejado en la expresión y de forma predeterminada, se daría lugar a una definición NULL.

Para resolver estos problemas, debe establecer explícitamente la conversión de tipos y la nulabilidad en la parte SELECT de la instrucción CTAS. No se puede establecer estas propiedades en la parte de la tabla de crear.

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
* La segunda parte de ISNULL es una constante, 0

> [!NOTE]
> Para que la nulabilidad se establezca correctamente, es fundamental usar ISNULL y no COLAESCE. COALESCE no es una función determinista y, por tanto, el resultado de la expresión siempre será NULLable. ISNULL es diferente. Es determinista. Por lo tanto, cuando la segunda parte de la función ISNULL es una constante o un literal, el valor resultante será NOT NULL.

Asegurar la integridad de los cálculos también es importante para la conmutación de particiones de tabla. Imagine que tiene esta tabla se definen como una tabla de hechos:

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

Sin embargo, el campo de cantidad es una expresión calculada; no es parte del origen de datos.

Para crear el conjunto de datos con particiones, es posible que desea utilizar el código siguiente:

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

La consulta se ejecutaría perfectamente. El problema viene cuando se intenta realizar el cambio de partición. Las definiciones de tabla no coinciden. Para hacer que coincida con las definiciones de tabla, se debe modificar para agregar la CTAS un `ISNULL` función para conservar el atributo de nulabilidad de la columna.

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

Puede ver que la coherencia de los tipos y mantenimiento de las propiedades de nulabilidad en una instrucción CTAS es una buena práctica de mejor ingeniería. Ayuda a mantener la integridad de los cálculos y también garantiza que la modificación de particiones es posible.

Hacer referencia a la [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) documentación. CTAS es una de las instrucciones más importantes en Azure SQL Data Warehouse. Asegúrese de que la comprende perfectamente.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más sugerencias sobre desarrollo, vea la [información general sobre desarrollo](sql-data-warehouse-overview-develop.md).

