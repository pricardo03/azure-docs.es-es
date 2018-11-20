---
title: Guía de inicio rápido para el uso de Machine Learning Services (con R) en Azure SQL Database (versión preliminar) | Microsoft Docs
description: En este tema se muestra cómo usar Machine Learning Services en Azure SQL Database y cómo ejecutar scripts de R para ofrecer análisis avanzados a escala, y la capacidad de llevar los cálculos y el procesamiento a donde residen los datos, sin necesidad de extraer los datos a través de la red.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.reviewer: ''
manager: cgronlun
ms.date: 11/07/2018
ms.openlocfilehash: 382ac23ea4c8e0ec54314bb754c00a8e6e43e9f6
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300972"
---
# <a name="quickstart-use-machine-learning-services-with-r-in-azure-sql-database-preview"></a>Guía de inicio rápido: Uso de Machine Learning Services (con R) en Azure SQL Database (versión preliminar)

En este artículo se explica cómo puede utilizar la versión preliminar pública de Machine Learning Services (con R) en Azure SQL Database. Le guía por los conceptos básicos para mover datos entre la base de datos SQL y R. También explica cómo encapsular código R bien formado en el procedimiento almacenado [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) para crear, entrenar y utilizar modelos de aprendizaje automático en una base de datos SQL.

El aprendizaje automático en SQL Database se utiliza para ejecutar funciones y código R y el código está totalmente disponible para los datos relacionales como procedimientos almacenados, como script T-SQL que contiene instrucciones de R, o como código R que contiene T-SQL. Utilice la eficacia de los paquetes empresariales de R para ofrecer análisis avanzados a escala, y la capacidad de llevar los cálculos y el procesamiento a donde residen los datos, sin necesidad de extraer los datos a través de la red.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-up-for-the-preview"></a>Suscríbase a la vista previa

De forma predeterminada, la versión preliminar pública de Machine Learning Services (con R) en SQL Database no está habilitada. Envíe un correo electrónico a Microsoft en [sqldbml@microsoft.com](mailto:sqldbml@microsoft.com) para registrarse en la versión preliminar pública.

Cuando ya esté inscrito en el programa, Microsoft lo incorporará a la versión preliminar pública y migrará la base de datos existente o creará una nueva base de datos en un servicio habilitado para R.

Machine Learning Services (con R) en SQL Database actualmente solo está disponible en el modelo de compra basado en núcleos virtuales de los niveles de servicio **Propósito general** y **Crítico para la empresa** para bases de datos únicas y en grupo. En esta versión preliminar pública inicial, no se admiten ni el nivel de servicio **Hiperescala** ni **Instancia administrada**. No se debe usar Machine Learning Services con R para cargas de trabajo de producción durante la versión preliminar pública.

Cuando Machine Learning Services (con R) se ha habilitado para la instancia de SQL Database, regrese a esta página para aprender a ejecutar scripts de R en el contexto de un procedimiento almacenado.

Actualmente, R es el único lenguaje que se admite. En este momento no hay ninguna compatibilidad con Python.

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar el código de ejemplo en estos ejercicios, primero debe tener una instancia de SQL Database con Machine Learning Services (con R) habilitado. Durante la versión preliminar pública, Microsoft le incorporará y habilitará el aprendizaje automático para la base de datos nueva o existente, tal como se ha descrito anteriormente.

Puede conectarse a SQL Database y ejecutar los scripts de R con cualquier herramienta de administración o de consulta de bases de datos, siempre y cuando pueda conectarse a una instancia de SQL Database y ejecutar una consulta T-SQL o un procedimiento almacenado. En esta guía de inicio rápido se usa [SQL Server Management Studio](sql-database-connect-query-ssms.md).

Para el ejercicio de [agregar un paquete](#add-package), también deberá instalar [R](https://www.r-project.org/) y [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) en el equipo local.

Esta guía de inicio rápido también exige configurar una regla de firewall de nivel de servidor. Para consultar una guía de inicio rápido en donde se muestra cómo hacerlo, vea [Create server-level firewall rule](sql-database-get-started-portal-firewall.md) (Crear regla de firewall de nivel de servidor).

## <a name="different-from-sql-server"></a>Diferente de SQL Server

La funcionalidad de Machine Learning Services (con R) en Azure SQL Database es similar a [SQL Server Machine Learning Services](https://review.docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). Sin embargo, hay algunas diferencias:

- Solo R. Actualmente no hay compatibilidad para Python.
- No es necesario configurar `external scripts enabled` mediante `sp_configure`.
- No es necesario dar permiso de ejecución de scripts a los usuarios.
- Los paquetes deben instalarse mediante **sqlmlutils**.
- No hay ningún gobierno de recursos externos independiente. Los recursos de R representan un determinado porcentaje de los recursos de SQL, dependiendo del nivel.

## <a name="verify-r-exists"></a>Comprobación de la existencia de R

Puede confirmar que Machine Learning Services (con R) está habilitado para la base de datos SQL. Siga estos pasos:

1. Abra SQL Server Management Studio y conéctese a la base de datos SQL.

1. Ejecute el código siguiente. 

    ```sql
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(31 + 11)';
    GO
    ```
    Si todo es correcto, verá un mensaje de resultados como este.

    ```text
    STDOUT message(s) from external script: 
    42
    ```

1. Si recibe algún error, es posible que la versión preliminar pública de Machine Learning Services (con R) no esté habilitada para la base de datos SQL. Consulte cómo registrarse para participar en la versión preliminar pública anterior.

## <a name="basic-r-interaction"></a>Interacción de R básica

Hay dos maneras de ejecutar código R en SQL Database:

+ Agregue un script de R como argumento del procedimiento almacenado del sistema, [sp_execute_external_script](https://docs.microsoft.com/sql//relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
+ Desde un [cliente R remoto](https://review.docs.microsoft.com/sql/advanced-analytics/r/set-up-a-data-science-client), conéctese a la base de datos SQL y ejecute el código utilizando SQL Database como contexto de cálculo.

El siguiente ejercicio se centra en el primer modelo de interacción: cómo pasar el código de R a un procedimiento almacenado.

1. Ejecute un script sencillo para ver cómo se puede ejecutar un script de R en la base de datos SQL.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R',
    @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c, d)'
    ```

2. Suponiendo que todo está configurado correctamente, se calcula el resultado correcto y la función de R `print` devuelve el resultado a la ventana de **mensajes**.

    **Resultados**

    ```text
    STDOUT message(s) from external script: 
    0.5 2
    ```

    Mientras que obtener mensajes **stdout** es útil cuando se prueba el código, más a menudo es necesario que devuelva los resultados en formato tabular, para que se puedan utilizar en una aplicación o escribirlo en una tabla. Consulte la sección de entradas y salidas más abajo para más información.

Recuerde, todo lo que hay dentro del argumento `@script` debe ser un código de R válido.

## <a name="inputs-and-outputs"></a>Entradas y salidas

De forma predeterminada, [sp_execute_external_script](https://review.docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) acepta un único conjunto de datos de entrada, que normalmente se proporciona en forma de una consulta SQL válida. Otros tipos de entrada pueden pasarse como variables SQL.

El procedimiento almacenado devuelve una sola trama de datos de R como salida, pero también puede generar valores escalares y modelos como variables. Por ejemplo, puede generar un modelo entrenado como una variable binaria y pasarlo a una instrucción INSERT de T-SQL, para escribir ese modelo en una tabla. También puede generar trazados (en formato binario) o valores escalares (valores individuales, como fecha y hora, el tiempo transcurrido para entrenar el modelo, etc.).

Por ahora, veamos solo las variables de entrada y salida predeterminadas de sp_execute_external_script: `InputDataSet` y `OutputDataSet`.

1. Cree una pequeña tabla de datos de prueba mediante la ejecución de la siguiente instrucción T-SQL:

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    INSERT INTO RTestData VALUES (1);
    INSERT INTO RTestData VALUES (10);
    INSERT INTO RTestData VALUES (100);
    GO
    ```

    Cuando se ha creado la tabla, utilice la instrucción siguiente para consultar la tabla:
  
    ```sql
    SELECT * FROM RTestData
    ```

    **Resultados**

    ![Contenido de la tabla RTestData](./media/sql-database-connect-query-r/select-rtestdata.png)

2. Puede obtener los datos de la tabla como entrada al script de R. Ejecute la instrucción siguiente. Obtiene los datos de la tabla, realiza un recorrido de ida y vuelta mediante el entorno en tiempo de ejecución de R, y devuelve los valores con el nombre de columna *NewColName*.

    Los datos devueltos por la consulta se pasan al entorno en tiempo de ejecución de R, que devuelve los datos a SQL Database como una trama de datos. La cláusula WITH RESULT SETS define el esquema de la tabla de datos devueltos para SQL Database.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    **Resultados**

    ![Salida del script de R que devuelve datos de una tabla](./media/sql-database-connect-query-r/r-output-rtestdata.png)

3. Vamos a cambiar el nombre de las variables de entrada o salida. El script anterior usaba los nombres de las variables de entrada y salida predeterminados, _InputDataSet_ y _OutputDataSet_. Para definir los datos de entrada asociados con _InputDatSet_, utilice la variable *@input_data_1*.

    En este script, los nombres de las variables de salida y entrada para el procedimiento almacenado se han cambiado a *SQL_out* y *SQL_in*:

    ```sql
    EXECUTE sp_execute_external_script
      @language = N'R'
      , @script = N' SQL_out <- SQL_in;'
      , @input_data_1 = N' SELECT 12 as Col;'
      , @input_data_1_name  = N'SQL_in'
      , @output_data_1_name =  N'SQL_out'
      WITH RESULT SETS (([NewColName] INT NOT NULL));
    ```

    Tenga en cuenta que R distingue entre mayúsculas y minúsculas, por lo que las variables de entrada y salida en `@input_data_1_name` y `@output_data_1_name` tiene que coincidir en mayúsculas y minúsculas con las del código R en `@script`. 

    Además, el orden de los parámetros es importante. Debe especificar primero los parámetros requeridos *@input_data_1* y *@output_data_1* para poder utilizar los parámetros opcionales *@input_data_1_name* y *@output_data_1_name*.

    Solo se puede pasar un conjunto de datos de entrada como parámetro, y solo se puede devolver un conjunto de datos. Sin embargo, puede llamar a otros conjuntos de datos desde el interior del código R y puede devolver salidas de otros tipos además del conjunto de datos. También puede agregar la palabra clave OUTPUT a cualquier parámetro para que se devuelva con los resultados. 

    La instrucción `WITH RESULT SETS` define el esquema de los datos que se usan en SQL Database. Necesita proporcionar tipos de datos compatibles con SQL para cada columna que se obtenga de R. Puede utilizar la definición de esquema para proporcionar nuevos nombres de columna también, ya que no necesita utilizar los nombres de columna de la trama de datos de R.

4. También puede generar valores mediante el script de R y dejar la cadena de consulta de entrada en _@input_data_1_ en blanco.

    ```sql
    EXECUTE sp_execute_external_script
        @language = N'R'
        , @script = N' mytextvariable <- c("hello", " ", "world");
            OutputDataSet <- as.data.frame(mytextvariable);'
        , @input_data_1 = N''
    WITH RESULT SETS (([Col1] CHAR(20) NOT NULL));
    ```

    **Resultados**

    ![Resultados de la consulta con @script como entrada](./media/sql-database-connect-query-r/r-data-generated-output.png)

## <a name="check-r-version"></a>Comprobación de la versión de R

Si desea ver qué versión de R está instalada en la base de datos SQL, haga lo siguiente:

1. Ejecute el script siguiente en la base de datos SQL.

    ```SQL
    EXECUTE sp_execute_external_script
    @language =N'R',
    @script=N'print(version)';
    GO
    ```

2. La función `print` de R devuelve la versión en la ventana de **mensajes**. En el siguiente ejemplo de salida, puede ver que SQL Database en este caso tiene instalada la versión 3.4.4 de R.

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

Microsoft proporciona una serie de paquetes de R preinstalados con Machine Learning Services en la base de datos SQL. Para ver una lista de los paquetes R que están instalados, incluida la versión, las dependencias, la licencia y la información de la ruta de acceso de la biblioteca, siga los pasos que se indican a continuación. Para agregar paquetes adicionales, consulte la sección [Adición de un paquete](#add-package).

1. Ejecute el script siguiente en la base de datos SQL.

    ```SQL
    EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
    OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
    WITH result sets((Package NVARCHAR(255), Version NVARCHAR(100), Depends NVARCHAR(4000)
        , License NVARCHAR(1000), LibPath NVARCHAR(2000)));
    ```

2. La salida es de `installed.packages()` en R y se devuelve como un conjunto de resultados.

    **Resultados**

    ![Paquetes instalados de R](./media/sql-database-connect-query-r/r-installed-packages.png)


## <a name="create-a-predictive-model"></a>Creación de un modelo predictivo

Puede entrenar un modelo con R y guardarlo en una tabla de la base de datos SQL. En este ejercicio, entrenará un modelo de regresión simple que predice la distancia de frenado de un coche basado en la velocidad. Deberá usar el conjunto de datos `cars` incluido con R, porque es pequeño y fácil de entender.

1. En primer lugar, cree una tabla para guardar los datos de aprendizaje.

    ```sql
    CREATE TABLE dbo.CarSpeed (speed INT NOT NULL, distance INT NOT NULL)
    GO
    INSERT INTO dbo.CarSpeed (speed, distance)
    EXEC sp_execute_external_script
    @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
    GO
    ```

    Muchos conjuntos de datos, pequeños y grandes, se incluyen con el entorno en tiempo de ejecución de R. Para obtener una lista de los conjuntos de datos instalados con R, escriba `library(help="datasets")` desde el símbolo del sistema de R.

2. Cree un modelo de regresión. Los datos de velocidad del coche contienen dos columnas, ambas numéricas, `dist` y `speed`. Hay múltiples observaciones de algunas velocidades. A partir de estos datos, se creará un modelo de regresión lineal que describe alguna relación entre la velocidad del coche y la distancia necesaria para detenerlo.

    Los requisitos de un modelo lineal son sencillos:

    - Defina una fórmula que describa la relación entre la variable dependiente `speed` y la variable independiente `distance`.

    - Proporcione los datos de entrada para utilizarlos en el entrenamiento del modelo.

    > [!TIP]
    > Si necesita un actualizador de los modelos lineales, le recomendamos este tutorial, que describe el proceso de ajuste de un modelo mediante rxLinMod: [Ajuste de modelos lineales](https://docs.microsoft.com/r-server/r/how-to-revoscaler-linear-model).

    Para crear el modelo, se define la fórmula dentro del código R y se pasan los datos como un parámetro de entrada.

    ```sql
    DROP PROCEDURE IF EXISTS generate_linear_model;
    GO
    CREATE PROCEDURE generate_linear_model
    AS
    BEGIN
        EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
            trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));'
        , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
        , @input_data_1_name = N'CarsData'
        , @output_data_1_name = N'trained_model'
        WITH RESULT SETS ((model VARBINARY(max)));
    END;
    GO
    ```

    El primer argumento para rxLinMod es el parámetro *formula*, que define la distancia como dependiente de la velocidad. Los datos de entrada se almacenan en la variable `CarsData`, que se rellena con la consulta SQL. Si no asigna un nombre específico a los datos de entrada, el nombre predeterminado de la variable será _InputDataSet_.

2. A continuación, cree una tabla en la que almacene el modelo para que pueda volver a entrenarlo o utilizarlo para la predicción. La salida de un paquete de R que crea un modelo suele ser un **objeto binario**. Por lo tanto, la tabla debe proporcionar una columna del tipo **VARBINARY(max)**.

    ```sql
    CREATE TABLE dbo.stopping_distance_models (
        model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(max) NOT NULL
    );
    ```
3. Para guardar el modelo, ejecute la siguiente instrucción de Transact-SQL para llamar al procedimiento almacenado, generar el modelo y guardarlo en una tabla.

    ```sql
    INSERT INTO dbo.stopping_distance_models (model)
    EXEC generate_linear_model;
    ```

    Tenga en cuenta que si ejecuta este código por segunda vez, obtendrá este error:

    ```text
    Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object dbo.stopping_distance_models
    ```

    Una opción para evitar este error consiste en actualizar el nombre de cada nuevo modelo. Por ejemplo, podría cambiar el nombre a algo más descriptivo e incluir el tipo de modelo, el día en que lo creó, etc.

    ```sql
    UPDATE dbo.stopping_distance_models
    SET model_name = 'rxLinMod ' + FORMAT(GETDATE(), 'yyyy.MM.HH.mm', 'en-gb')
    WHERE model_name = 'default model'
    ```

4. Generalmente, la salida de R desde el procedimiento almacenado [sp_execute_external_script](https://review.docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) se limita a una única trama de datos.

    Sin embargo, puede devolver salidas de otros tipos, como valores escalares, además de la trama de datos.

    Por ejemplo, supongamos que quiere entrenar un modelo pero que inmediatamente ve una tabla de coeficientes del modelo. Puede crear la tabla de coeficientes como el conjunto de resultados principal y generar el modelo entrenado en una variable SQL. Puede reutilizar el modelo inmediatamente llamando a la variable, o puede guardar el modelo en una tabla, tal como se muestra aquí.

    ```sql
    DECLARE @model VARBINARY(max), @modelname VARCHAR(30)
    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
            speedmodel <- rxLinMod(distance ~ speed, CarsData)
            modelbin <- serialize(speedmodel, NULL)
            OutputDataSet <- data.frame(coefficients(speedmodel));'
        , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
        , @input_data_1_name = N'CarsData'
        , @params = N'@modelbin varbinary(max) OUTPUT'
        , @modelbin = @model OUTPUT
        WITH RESULT SETS (([Coefficient] FLOAT NOT NULL));

    -- Save the generated model
    INSERT INTO dbo.stopping_distance_models(model_name, model)
    VALUES ('latest model', @model)
    ```

    **Resultados**

    ![Modelo entrenado con salida adicional](./media/sql-database-connect-query-r/r-train-model-with-additional-output.png)

## <a name="predict"></a>Predicción

Utilice el modelo que creó en la sección anterior para puntuar las predicciones con respecto a los nuevos datos. Para realizar la _puntuación_ con datos nuevos, obtenga uno de los modelos entrenados de la tabla y después llame a un nuevo conjunto de datos en los que basar las predicciones. Puntuación es un término que a veces se utiliza en la ciencia de datos para referirse a la generación de predicciones, probabilidades u otros valores basados en nuevos datos introducidos en un modelo entrenado.

1. En primer lugar, cree una tabla con los nuevos datos de velocidad. ¿Ha observado que los datos de entrenamiento originales se detienen a una velocidad de 25 millas por hora? Esto se debe a que los datos originales se basaron en un experimento de 1920. Podría preguntarse, ¿cuánto tiempo tardaría un automóvil de la década de 1920 en detenerse, asumiendo que podría ir tan rápido como 60 mph o incluso 100 mph? Para responder a esta pregunta, debe proporcionar algunos valores de velocidad nuevos.

    ```sql
    CREATE TABLE dbo.NewCarSpeed(speed INT NOT NULL,
        distance INT NULL
    )
    GO
    INSERT dbo.NewCarSpeed(speed)
    VALUES (40), (50), (60), (70), (80), (90), (100)
    ```

    En este ejemplo, debido a que el modelo se basa en el algoritmo **rxLinMod** proporcionado como parte del paquete **RevoScaleR**, se llama a la función [rxPredict](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxpredict), en lugar de a la función R `predict` genérica.

    ```sql
    DECLARE @speedmodel varbinary(max) = 
        (SELECT model FROM dbo.stopping_distance_models WHERE model_name = 'latest model');

    EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'
                current_model <- unserialize(as.raw(speedmodel));
                new <- data.frame(NewCarData);
                predicted.distance <- rxPredict(current_model, new);
                str(predicted.distance);
                OutputDataSet <- cbind(new, ceiling(predicted.distance));
                '
        , @input_data_1 = N'SELECT speed FROM [dbo].[NewCarSpeed]'
        , @input_data_1_name = N'NewCarData'
        , @params = N'@speedmodel varbinary(max)'
        , @speedmodel = @speedmodel
    WITH RESULT SETS ((new_speed INT, predicted_distance INT));
    ```

    El script anterior ejecuta los pasos siguientes:

    + Utilice una instrucción SELECT para obtener un modelo único de la tabla y pasarlo como parámetro de entrada.

    + Después de recuperar el modelo de la tabla, llame a la función `unserialize` en el modelo.

        > [!TIP] 
        > Compruebe también las nuevas [funciones de serialización](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxserializemodel) proporcionadas por RevoScaleR, que admiten la puntuación en tiempo real.
    + Aplique la función `rxPredict` con los argumentos apropiados al modelo y proporcione los nuevos datos de entrada.

    + En el ejemplo, la función `str` se agrega durante la fase de prueba, para comprobar el esquema de los datos que se devuelven desde R. Puede eliminar la instrucción más tarde.

    + Los nombres de columna utilizados en el script de R no necesariamente se pasan a la salida del procedimiento almacenado. Aquí hemos usado la instrucción WITH RESULTS para definir algunos nombres de columna nuevos.

    **Resultados**

    ![Resultado establecido para predecir la distancia de frenado](./media/sql-database-connect-query-r/r-predict-stopping-distance-resultset.png)

    También es posible utilizar la instrucción [PREDICT en Transact-SQL](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql) para generar un valor de predicción o puntuación basada en un modelo almacenado.

<a name="add-package"></a>

## <a name="add-a-package"></a>Adición de un paquete

Si necesita usar un paquete que no esté ya instalado en la base de datos SQL, puede instalarlo mediante [sqlmlutils](https://github.com/Microsoft/sqlmlutils). Siga los pasos siguientes para instalar el paquete.

1. Descargue el último archivo zip **sqlmlutils** de [github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist) a su equipo local. No es necesario descomprimir el archivo.

1. Si no tiene R instalado, descárguelo de [www.r-project.org](https://www.r-project.org/) e instálelo en su equipo local. R está disponible para Windows, MacOS y Linux. En este ejemplo, se usa Windows.

1. Primero, instale el paquete **RODBCext**, que es un requisito previo para **sqlmlutils**. **RODBCext** también instala la dependencia del paquete **RODBC**. Abra un **símbolo del sistema** y ejecute el comando siguiente:

    ```
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    ```

    Si recibe el error **'R' no se reconoce como un comando interno o externo, programa o archivo por lotes ejecutable**, es probable que la ruta a R.exe no esté incluida en su variable de entorno **PATH** en Windows. Puede agregar el directorio a la variable de entorno o vaya al directorio en el símbolo del sistema (por ejemplo `cd C:\Program Files\R\R-3.5.1\bin`).

1. Utilice el comando **R CMD INSTALL** para instalar **sqlmlutils**. Especifique la ruta de acceso al directorio en el que ha descargado el archivo zip y el nombre del archivo zip. Por ejemplo: 

    ```
    R CMD INSTALL C:\Users\youruser\Downloads\sqlmlutils_0.5.0.zip
    ```

    La salida que se obtiene debe ser similar a la siguiente:

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/youruser/Documents/R/win-library/3.5'
    package 'sqlmlutils' successfully unpacked and MD5 sums checked
    ```

1. En este ejemplo usará RStudio Desktop como entorno de desarrollo integrado. Puede usar otro entorno de desarrollo integrado si lo prefiere. Descargue e instale RStudio Desktop desde [www.rstudio.com/products/rstudio/download/](https://www.rstudio.com/products/rstudio/download/), si aún no tiene RStudio instalado.

1. Abra **RStudio** y cree un nuevo archivo **Script de R**. Use el siguiente código R para instalar un paquete mediante sqlmlutils. En el siguiente ejemplo, instalará el paquete [glue](https://cran.r-project.org/web/packages/glue/), que puede formatear e interpolar una cadena.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!NOTE]
    > El **ámbito** puede ser **público** o **privado**. El ámbito público es útil para que el administrador de la base de datos instale paquetes que todos los usuarios puedan usar. El ámbito privado hace que el paquete solo esté disponible para el usuario que lo instala. Si no se especifica el ámbito, el ámbito predeterminado es **PRIVADO**.

1. Ahora, compruebe que el paquete **glue** se ha instalado.

    ```R
    r<-sql_installed.packages(connectionString = connection, fields=c("Package", "LibPath", "Attributes", "Scope"))
    View(r)
    ```

    **Resultados**

    ![Contenido de la tabla RTestData](./media/sql-database-connect-query-r/r-verify-package-install.png)


1. Una vez instalado el paquete, puede usarlo en su script de R a través de **sp_execute_external_script**. Abra **SQL Server Management Studio** y conéctese a la base de datos SQL. Ejecute el siguiente script:

    ```sql
    EXEC sp_execute_external_script @language = N'R'
    , @script = N'
    library(glue)

    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
        ''my age next year is {age + 1}, '',
        ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    print(text)
    ';
    ```

    Verá el siguiente resultado en la pestaña de mensajes.

    **Resultados**

    ```text
    STDOUT message(s) from external script:
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

1. Si desea quitar el paquete, ejecute el siguiente script de R en **RStudio** en el equipo local.

    ```R
    library(sqlmlutils) 
    connection <- connectionInfo(server= "yourserver.database.windows.net", 
        database = "yourdatabase", uid = "yoursqluser", pwd = "yoursqlpassword")
    sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC") 
    ```

> [!NOTE]
> Otra forma de instalar paquetes R en la base de datos SQL es cargar el paquete de R desde el flujo de bytes con [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Machine Learning Services, consulte los siguientes artículos sobre SQL Server Machine Learning Services. Aunque estos artículos son para SQL Server, la mayor parte de la información también se aplica a Machine Learning Services (con R) en Azure SQL Database.

- [SQL Server Machine Learning Services](https://review.docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)
- [Tutorial: Información de los análisis en bases de datos con R en SQL Server](https://review.docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers)
- [Tutorial completo de ciencia de datos para R y SQL Server](https://review.docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough)
- [Tutorial: Uso de funciones RevoScaleR R con datos de SQL Server](https://review.docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages)
