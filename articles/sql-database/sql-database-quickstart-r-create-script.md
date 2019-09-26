---
title: Crear y ejecutar scripts de R sencillos
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Ejecute scripts de R sencillos en Azure SQL Database Machine Learning Services (versión preliminar).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: a47e7a81ba486056841bdc0fe65cfd10f1b2c412
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123186"
---
# <a name="create-and-run-simple-r-scripts-in-azure-sql-database-machine-learning-services-preview"></a>Crear y ejecutar scripts de R sencillos en Azure SQL Database Machine Learning Services (versión preliminar)

En esta guía de inicio rápido, creará y ejecutará un conjunto de scripts de R sencillos mediante la versión preliminar pública de [Machine Learning Services (con R) en Azure SQL Database](sql-database-machine-learning-services-overview.md). Aprenderá a encapsular un script de R bien formado en el procedimiento almacenado [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) y a ejecutar ese script en una base de datos SQL.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/) antes de empezar.

- Para ejecutar el código de ejemplo en estos ejercicios, primero debe tener una instancia de Azure SQL Database con Machine Learning Services (con R) habilitada. Durante la versión preliminar pública, Microsoft le incorporará y habilitará el aprendizaje automático para la base de datos nueva o existente. Siga los pasos que se indican en [Suscríbase a la versión preliminar](sql-database-machine-learning-services-overview.md#signup).

- Asegúrese de que tiene instalada la versión más reciente de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). Puede ejecutar los scripts de R con herramientas de consulta o de otro tipo de administración de base de datos, pero en este tutorial usará SSMS.

- En esta guía de inicio rápido, también es necesario configurar una regla de firewall de nivel de servidor. Para obtener información acerca de cómo puede realizar estos pasos, consulte [Crear una regla de firewall de nivel de servidor](sql-database-server-level-firewall-rule.md).

## <a name="run-a-simple-script"></a>Ejecución de un script sencillo

Para ejecutar un script de R, deberá pasarlo como un argumento al procedimiento almacenado del sistema, [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

En los pasos siguientes, deberá ejecutar este script de R de ejemplo en la base de datos SQL:

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. Abra **SQL Server Management Studio** y conéctese a la base de datos SQL.

   Si necesita ayuda para conectarse, consulte [Inicio rápido: Uso de SQL Server Management Studio para conectarse a una base de datos de Azure SQL Database y realizar consultas en ella](sql-database-connect-query-ssms.md).

1. Pase el script de R completo al procedimiento almacenado [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

   El script se pasa mediante el argumento `@script`. Todo lo que haya en el argumento `@script` debe ser un código de R válido.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

   Si recibe algún error, es posible que la versión preliminar pública de Machine Learning Services (con R) no esté habilitada para la base de datos SQL. Consulte los [requisitos previos](#prerequisites) anteriores.

   > [!NOTE]
   > Si es un administrador, puede ejecutar código externo automáticamente. Puede conceder permiso a otros usuarios mediante el comando:
   <br>**GRANT EXECUTE ANY EXTERNAL SCRIPT TO** *\<username\>* .

2. Asimismo, se calcula el resultado correcto y la función de R `print` devuelve ese resultado a la ventana de **mensajes**.

   Este debe tener un aspecto similar al siguiente.

    **Resultados**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Ejecutar un script Hello world

Un script de ejemplo típico es aquel que solo envía la cadena "Hello world". Ejecute el siguiente comando.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Las entradas a este procedimiento almacenado incluyen:

| | |
|-|-|
| @language | define la extensión de lenguaje que se va a llamar, en este caso, R. |
| @script | define los comandos que se pasan al runtime de R. Su script de R completo debe estar incluido en este argumento, como texto Unicode. También puede agregar el texto a una variable de tipo **nvarchar** y, luego, llamar a la variable. |
| @input_data_1 | los datos que devuelve la consulta se pasan al runtime de R, que a su vez devuelve los datos a SQL Server como una trama de datos. |
|La cláusula WITH RESULT SETS | define el esquema de la tabla de datos devuelta para SQL Server, agregando "Hello world" como nombre de columna e **int** para el tipo de datos. |

El comando da como resultado el siguiente texto:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Uso de las entradas y salidas

De forma predeterminada, [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) acepta como entrada un único conjunto de datos que, normalmente, se proporciona en forma de una consulta SQL válida. A continuación, se devuelve un solo marco de datos de R como salida.

Por ahora, usaremos las variables de entrada y salida predeterminadas de [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql): **InputDataset** y **OutputDataset**.

1. Cree una pequeña tabla de datos de prueba.

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

1. Use la instrucción `SELECT` para consultar la tabla.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Resultados**

    ![Contenido de la tabla RTestData](./media/sql-database-quickstart-r-create-script/select-rtestdata.png)

1. Ejecute el siguiente script de R. Recupera los datos de la tabla mediante la instrucción `SELECT`, los pasa a través del runtime de R y los devuelve como un marco de datos. La cláusula `WITH RESULT SETS` define el esquema de la tabla de datos devuelta para SQL Database y agrega el nombre de columna *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Resultados**

    ![Salida del script de R que devuelve datos de una tabla](./media/sql-database-quickstart-r-create-script/r-output-rtestdata.png)

1. A continuación, cambiaremos el nombre de las variables de entrada y salida. Los nombres predeterminados de las variables de entrada y salida son **InputDataSet** y **OutputDataSet**. Este script cambia esos nombres a **SQL_in** y **SQL_out**:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Tenga en cuenta que R distingue mayúsculas de minúsculas. Las variables de entrada y salida utilizadas en el script de R (**SQL_out**, **SQL_in**) deben coincidir con los valores definidos con `@input_data_1_name` y `@output_data_1_name`, incluido el caso.

   > [!TIP]
   > Solo se puede pasar un conjunto de datos de entrada como parámetro, y solo se puede devolver un conjunto de datos. Sin embargo, puede llamar a otros conjuntos de datos desde el interior del código R y puede devolver salidas de otros tipos además del conjunto de datos. También puede agregar la palabra clave OUTPUT a cualquier parámetro para que se devuelva con los resultados.

1. También puede generar valores usando simplemente el script de R sin datos de entrada (`@input_data_1` está establecido en blanco).

   En el siguiente script se muestra el texto "hello" y "world".

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Resultados**

    ![Resultados de la consulta con @script como entrada](./media/sql-database-quickstart-r-create-script/r-data-generated-output.png)

## <a name="check-r-version"></a>Comprobación de la versión de R

Si quiere ver qué versión de R está instalada en la base de datos SQL, ejecute el siguiente script.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

La función `print` de R devuelve la versión en la ventana de **mensajes**. En el siguiente ejemplo de salida, puede ver que SQL Database en este caso tiene instalada la versión 3.4.4 de R.

**Resultados**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>Enumeración de paquetes de R

Microsoft proporciona una serie de paquetes de R preinstalados con Machine Learning Services en la base de datos SQL.

Para ver una lista de los paquetes R que están instalados, incluida la versión, las dependencias, la licencia y la información de la ruta de acceso de la biblioteca, ejecute el siguiente script.

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

La salida es de `installed.packages()` en R y se devuelve como un conjunto de resultados.

**Resultados**

![Paquetes instalados de R](./media/sql-database-quickstart-r-create-script/r-installed-packages.png)

## <a name="next-steps"></a>Pasos siguientes

Para crear un modelo de Machine Learning mediante R en SQL Database, siga esta guía de inicio rápido:

> [!div class="nextstepaction"]
> [Crear y entrenar un modelo predictivo en R con Machine Learning Services (versión preliminar) de Azure SQL Database](sql-database-quickstart-r-train-score-model.md)

Para más información acerca de Azure SQL Database Machine Learning Services con R (versión preliminar), consulte los siguientes artículos.

- [Azure SQL Database Machine Learning Services con R (versión preliminar)](sql-database-machine-learning-services-overview.md)
- [Escribir funciones de R avanzadas en Azure SQL Database Machine Learning Services (versión preliminar)](sql-database-machine-learning-services-functions.md)
- [Trabajar con datos SQL y R en Machine Learning Services (versión preliminar) de Azure SQL Database](sql-database-machine-learning-services-data-issues.md)
