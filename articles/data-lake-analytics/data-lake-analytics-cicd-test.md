---
title: Prueba de código de Azure Data Lake Analytics | Microsoft Docs
description: Aprenda a agregar casos de prueba para código de U-SQL y C# extendido para Azure Data Lake Analytics.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: fa5c113541452a93c25adc7c14bdaa6994434c71
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365867"
---
# <a name="test-your-azure-data-lake-analytics-code"></a>Prueba del código de Azure Data Lake Analytics

Azure Data Lake proporciona U-SQL como lenguaje, que combina SQL declarativo con C# imperativo para procesar datos a cualquier escala. En este documento, aprenderá a crear casos de prueba para código de U-SQL y C# extendido UDO (de operador definido por el usuario ).

## <a name="test-u-sql-scripts"></a>Prueba de scripts U-SQL

El script U-SQL se compila y optimiza como código ejecutable para ejecutarse en las máquinas en la nube o en la máquina local. El proceso de compilación y optimización trata el script de U-SQL completo como un todo. No es posible realizar la tradicional "prueba unitaria" para cada instrucción. Sin embargo, mediante el SDK de prueba y el SDK de ejecución local de U-SQL, puede realizar pruebas de nivel de script.

### <a name="create-test-cases-for-u-sql-script"></a>Creación de casos de prueba para el script U-SQL

Las Herramientas de Azure Data Lake para Visual Studio le permiten crear casos de prueba para el script U-SQL.

1.  Haga clic con el botón derecho en un script U-SQL en el Explorador de soluciones y después seleccione **Create Unit Test** (Crear prueba unitaria).
2.  Cree un proyecto de prueba o inserte el caso de prueba en un proyecto de prueba existente.

    ![Herramientas de Data Lake para Visual Studio: Creación de un proyecto de prueba de U-SQL](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project.png) 

    ![Herramientas de Data Lake para Visual Studio: Creación de una configuración de proyecto de prueba de U-SQL](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project-configure.png) 

### <a name="manage-the-test-data-source"></a>Administración del origen de datos de prueba

Al probar scripts U-SQL, necesita los archivos de entrada de prueba. Puede administrar estos datos de prueba si configura el **origen de datos de prueba** en las propiedades del proyecto de U-SQL. 

Cuando llama a la interfaz `Initialize()` en el SDK de prueba de U-SQL, se crea una carpeta raíz de datos local temporal en el directorio de trabajo del proyecto de prueba y se copian todos los archivos y subcarpetas (y los archivos de las subcarpetas) de la carpeta de origen de datos de prueba a la carpeta raíz de datos local temporal antes de ejecutar los casos de prueba para el script U-SQL. Puede agregar más carpetas de origen de datos de prueba si divide la ruta de la carpeta de datos de prueba con un punto y coma.

![Herramientas de Data Lake para Visual Studio: Configuración del origen de datos de prueba del proyecto](./media/data-lake-analytics-cicd-test/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-the-database-environment-for-testing"></a>Administración del entorno de base de datos para la prueba

Si, por ejemplo, los scripts U-SQL usan o consultan objetos de base de datos U-SQL que llaman a procedimientos almacenados, debe inicializar el entorno de base de datos antes de ejecutar los casos de prueba de U-SQL. La interfaz `Initialize()` en el SDK de prueba de U-SQL le ayudará a implementar todas las bases de datos a las que hace referencia el proyecto de U-SQL en la carpeta raíz de datos local temporal del directorio de trabajo del proyecto de prueba. 

Más información sobre [cómo administrar referencias de proyectos de base de datos de U-SQL para el proyecto de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).

### <a name="verify-test-results"></a>Comprobación de los resultados de la prueba

La interfaz `Run()` devuelve el resultado de la ejecución de un trabajo. 0 significa correcto y 1 significa error. También puede usar funciones de aserción de C# para comprobar los resultados. 

### <a name="run-test-cases-in-visual-studio"></a>Ejecución de casos de prueba en Visual Studio

Un proyecto de prueba del script de U-SQL se compila a partir de la plataforma de una prueba unitaria de C#. Después de compilar el proyecto, puede ejecutar todos los casos de prueba mediante **Explorador de pruebas > Lista de reproducción**. Como alternativa, haga clic con el botón derecho en el archivo .cs y, a continuación, seleccione **Ejecutar pruebas**.

## <a name="test-c-udos"></a>Prueba de los UDO de C#

### <a name="create-test-cases-for-c-udos"></a>Creación de casos de prueba para UDO de C#

Puede usar la plataforma de una prueba unitaria de C# para probar sus UDO (operadores definidos por el usuario) de C#. Al probar los UDO, debe preparar los objetos **IRowset** correspondientes como entradas.

Hay dos maneras de crear un objeto IRowset:

- Cargar datos desde un archivo para crear el objeto IRowset:

    ```csharp
    //Schema: "a:int, b:int"
    USqlColumn<int> col1 = new USqlColumn<int>("a");
    USqlColumn<int> col2 = new USqlColumn<int>("b");
    List<IColumn> columns = new List<IColumn> { col1, col2 };
    USqlSchema schema = new USqlSchema(columns);

    //Generate one row with default values
    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Get data from file
    IRowset rowset = UnitTestHelper.GetRowsetFromFile(@"processor.txt", schema, output.AsReadOnly(), discardAdditionalColumns: true, rowDelimiter: null, columnSeparator: '\t');
    ```

- Usar datos de una colección de datos para crear el objeto IRowset:

    ```csharp
    //Schema: "a:int, b:int"
    USqlSchema schema = new USqlSchema(
        new USqlColumn<int>("a"),
        new USqlColumn<int>("b")
    );

    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Generate Rowset with specified values
    List<object[]> values = new List<object[]>{
        new object[2] { 2, 3 },
        new object[2] { 10, 20 }
    };

    IEnumerable<IRow> rows = UnitTestHelper.CreateRowsFromValues(schema, values);
    IRowset rowset = UnitTestHelper.GetRowsetFromCollection(rows, output.AsReadOnly());
    ```

### <a name="verify-test-results"></a>Comprobación de los resultados de la prueba

Después de llamar a las funciones de UDO, puede comprobar los valores schema y Rowset con funciones de aserción de C# para comprobar el resultado. Puede usar código de ejemplo en un proyecto de ejemplo de prueba unitaria de UDO de C# de U-SQL mediante **Archivo > Nuevo > Proyecto** en Visual Studio.

### <a name="run-test-cases-in-visual-studio"></a>Ejecución de casos de prueba en Visual Studio

Después de compilar el proyecto de prueba, puede ejecutar todos los casos de prueba mediante **Explorador de pruebas > Lista de reproducción**, o bien hacer clic con el botón derecho en el archivo .cs y seleccionar **Ejecutar pruebas**.

## <a name="run-test-cases-in-visual-studio-team-service"></a>Ejecución de casos de prueba en Visual Studio Team Services

Tanto los **proyectos de prueba para el script de U-SQL** como los **proyectos de prueba de UDO de C#** heredan los proyectos de prueba unitaria de C#. La [tarea de prueba de Visual Studio](https://docs.microsoft.com/vsts/pipelines/test/getting-started-with-continuous-testing?view=vsts) en Visual Studio Team Services puede ejecutar estos casos de prueba. 

### <a name="run-u-sql-test-cases-in-visual-studio-team-service"></a>Ejecución de casos de prueba de U-SQL en Visual Studio Team Services

Para la prueba de U-SQL, asegúrese de cargar `CPPSDK` en su máquina de compilación y, después, pase la ruta de acceso de `CPPSDK` USqlScriptTestRunner (cppSdkFolderFullPath: \@"").

**¿Qué es CPPSDK?**

CPPSDK es un paquete que incluye Microsoft Visual C++ 14 y Windows SDK 10.0.10240.0. Este es el entorno que necesita el entorno de ejecución de U-SQL. Puede obtener este paquete en la carpeta de instalación de las Herramientas de Azure Data Lake para Visual Studio:

- Para Visual Studio 2015, se encuentra en `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK`
- Para Visual Studio 2017, se encuentra en `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\SDK\ScopeCppSDK`

**Preparación de CPPSDK en el agente de compilación de Visual Studio Team Services**

La forma habitual de preparar esta dependencia de CPPSDK en Visual Studio Team Service es la siguiente:

1.  Comprima la carpeta que incluye las bibliotecas de CPPSDK.
2.  Compruebe el archivo .zip en el sistema de control de código fuente. (El archivo .zip garantiza que compruebe todas las bibliotecas de la carpeta CPPSDK de modo que algunos archivos no se ignoren con ".gitignore").   
3.  Descomprima el archivo .zip en la canalización de compilación.
4.  Dirija `USqlScriptTestRunner` a esta carpeta descomprimida en la máquina de compilación.

### <a name="run-c-udo-test-cases-in-visual-studio-team-services"></a>Ejecución de casos de prueba de UDO de C# en Visual Studio Team Services

Para una prueba de UDO de C#, asegúrese de hacer referencia a los ensamblados siguientes, que son necesarios para los UDO. Si hace referencia a ellos mediante [el paquete de Nuget Microsoft.Azure.DataLake.USQL.Interfaces](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.Interfaces/), asegúrese de agregar una tarea de restauración de NuGet en la canalización de compilación.

* Microsoft.Analytics.Interfaces
* Microsoft.Analytics.Types
* Microsoft.Analytics.UnitTest

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de canalizaciones de CI/CD para Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Ejecución local de scripts U-SQL](data-lake-analytics-data-lake-tools-local-run.md)
- [Uso de un proyecto de base de datos de U-SQL para desarrollar bases de datos de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)

