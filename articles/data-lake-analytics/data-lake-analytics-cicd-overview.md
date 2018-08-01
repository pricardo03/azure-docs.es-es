---
title: Configuración de canalizaciones de CI/CD para Azure Data Lake Analytics | Microsoft Docs
description: Aprenda a configurar una integración continua y una implementación continua para Azure Data Lake Analytics.
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
ms.openlocfilehash: c069bc2a6147a021ea9bdf37e2926d5c8f33281c
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39145016"
---
# <a name="how-to-set-up-cicd-pipeline-for-azure-data-lake-analytics"></a>Configuración de canalizaciones de CI/CD para Azure Data Lake Analytics

En este documento, aprenderá a configurar canalizaciones de CI/CD para trabajos de U-SQL y bases de datos de U-SQL.

## <a name="cicd-for-u-sql-job"></a>CI/CD para trabajos de U-SQL

Las Herramientas de Azure Data Lake para Visual Studio proporcionan un tipo de proyecto de U-SQL que le ayuda a organizar los scripts de U-SQL. El uso del proyecto de U-SQL para administrar código de U-SQL facilita los escenarios de CI/CD.

## <a name="build-u-sql-project"></a>Creación de un proyecto de U-SQL

El proyecto de U-SQL se puede compilar con MSBuild. Para ello, es preciso usar los parámetros correspondientes. Siga los pasos que se indican a continuación para configurar el proceso de compilación de proyectos de U-SQL.

### <a name="project-migration"></a>Migración del proyecto

Antes de configurar la tarea de compilación en el proyecto de U-SQL, asegúrese de usar la versión más reciente del mismo. Abra el archivo de proyecto de U-SQL en el editor y compruebe si tiene los siguientes elementos de importación:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

En caso negativo, tiene dos opciones para migrar el proyecto:

- Opción 1: Cambie el elemento de importación antiguo por el de arriba.
- Opción 2: Abra el proyecto antiguo en cualquier versión de Herramientas de Azure Data Lake para Visual Studio posterior a la 2.3.3000.0. La plantilla del proyecto antiguo se actualizará automáticamente a la versión más reciente. Los proyectos nuevos creados con una versión posterior a la 2.3.3000.0 usan la plantilla nueva directamente.

### <a name="get-nuget-package"></a>Obtención de un paquete NuGet

MSBuild no proporciona compatibilidad integrada con el tipo de proyecto de U-SQL. Para agregar esta capacidad, es preciso agregar una referencia de la solución al [paquete Nuget Microsoft.Azure.DataLake.USQL.SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) que agregue el servicio de lenguaje requerido.

Para agregar dicha referencia, puede hacer clic con el botón derecho en la solución en el Explorador de soluciones y elegir **Administrar paquetes NuGet**. O bien puede agregar un archivo llamado `packages.config` a la carpeta de la solución y agregarle el contenido siguiente.

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180620" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>Administración de referencias de la base de datos de U-SQL

Si los scripts U-SQL del proyecto de U-SQL tienen instrucciones de consulta para los objetos de base de datos de U-SQL, como por ejemplo, consultar una tabla de U-SQL o hacer referencia a un ensamblado, antes de crear el proyecto de U-SQL es preciso hacer referencia al proyecto de base de datos de U-SQL correspondiente que incluye la definición de dichos objetos.

[Más información acerca del proyecto de base de datos de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)

>[!NOTE]
>El proyecto de base de datos de U-SQL está actualmente en versión preliminar pública. Si tiene la instrucción DROP en el proyecto, se produce un error en la compilación. Pronto se permitirá la instrucción DROP.
>

### <a name="build-u-sql-project-with-msbuild-command-line"></a>Compilación de un proyecto de U-SQL con la línea de comandos de MSBuild

Después de migrar el proyecto y obtener el paquete NuGet, puede llamar a la línea de comandos de MSBuild estándar con los argumentos adicionales siguientes para compilar el proyecto de U-SQL:

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder;/p:EnableDeployment=true
``` 

Estos son la definición de los argumentos y los valores:

* USQLSDKPath = <paquete Nuget de U-SQL> \build\runtime: este parámetro hace referencia a la ruta de instalación del paquete NuGet para el servicio de lenguaje de U-SQL.
* USQLTargetType=Merge or SyntaxCheck:
    * Merge: el modo Merge compila los archivos de código subyacente, como los archivos . cs, py y .r, y alinea la biblioteca de código definida por el usuario resultante (por ejemplo, un archivo binario dll, código de Python o código de R) en el script U-SQL.
    * SyntaxCheck: el modo SyntaxCheck en primer lugar combina los archivos de código subyacente en el script U-SQL y luego compila el script U-SQL para validar el código.
* DataRoot =<DataRoot path>: DataRoot solo se necesita para el modo SyntaxCheck. Al compilar el script con el modo SyntaxCheck, MSBuild comprueba las referencias que hay en el script a objetos de base de datos. Antes de hacer la compilación asegúrese de configurar un entorno local coincidente que contenga los objetos a los que se hace referencia desde la base de datos de U-SQL en la carpeta DataRoot del equipo en que se realiza compilación. Para administrar estas dependencias de la base de datos, también se puede [hacer referencia a un proyecto de base de datos de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project). Tenga en cuenta que MSBuild solo comprueba la referencia de los objetos de base de datos, no los archivos.
* EnableDeployment = true o false: EnableDeployment indica si se permite implementar bases de datos de U-SQL de referencia durante el proceso de compilación. Si hacer referencia al proyecto de base de datos de U-SQL y consume los objetos de base de datos en el script U-SQL, establezca este parámetro en true.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Integración continua con un servicio de Visual Studio Team Services

Además de la línea de comandos, los clientes también pueden utilizar la tarea de compilación de Visual Studio o de MSBuild para compilar proyectos de U-SQL en un servicio de Visual Studio Team Services. Para configurar la tarea de compilación, asegúrese de que:

1.  Agrega la tarea de restauración de NuGet para obtener la solución a la que hace referencia el paquete NuGet incluyendo `Azure.DataLake.USQL.SDK`, con el fin de que MSBuild pueda encontrar el lenguaje U-SQL de destino. Establezca **Opciones avanzadas > Directorio de destino** como `$(Build.SourcesDirectory)/packages` si desea usar el ejemplo de argumentos de MSBuild directamente en el paso 2.

    ![Tarea de MSBuild de CI/CD del conjunto de Data Lake para el proyecto de U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

    ![Tarea de Nuget de CI/CD del conjunto de Data Lake para el proyecto de U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  Establece los argumentos de MSBuild. Puede establecer los argumentos en la tarea de compilación de Visual Studio o de MSBuild como de muestra a continuación, o bien puede definir variables para estos argumentos en la definición de la compilación de VSTS.

    ```
    /p:USQLSDKPath=/p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory) /p:EnableDeployment=true
    ```

    ![Variables de MSBuild de CI/CD del conjunto de Data Lake para el proyecto de U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

### <a name="u-sql-project-build-output"></a>Salida de compilación del proyecto de U-SQL

Después de ejecutar la compilación, se compilan todos los scripts del proyecto de U-SQL y la salida se comprime en un archivo zip llamado `USQLProjectName.usqlpack`. La estructura de carpetas del proyecto se mantendrán en la salida de la compilación comprimida.

>[!NOTE]
>
>El archivo de código subyacente de cada script U-SQL se combinarán como instrucción insertada en la salida de la compilación del script.
>

## <a name="test-u-sql-script"></a>Prueba del script U-SQL

Azure Data Lake proporciona proyectos de prueba para el script U-SQL y para UDO/UDAG/UDF de C#:
* [Aprenda a agregar casos de prueba para un script de U-SQL y el código de C# extendido](data-lake-analytics-cicd-test.md#test-u-sql-scripts)
* [Aprenda a ejecutar estos casos de prueba en un servicio de Visual Studio Team Services](data-lake-analytics-cicd-test.md#run-test-cases-in-visual-studio-team-service)

## <a name="u-sql-job-deployment"></a>Implementación de un trabajo de U-SQL

Después de comprobar el código a través del proceso de compilación y prueba, los trabajos de U-SQL se pueden enviar directamente desde el servicio de Visual Studio Team Services a través de una **tarea de Azure PowerShell**. También se puede implementar el script en Azure Data Lake Store o Azure Blob Storage y [ejecutar los trabajos programados a través de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

### <a name="submit-u-sql-jobs-through-visual-studio-team-service"></a>Envío de trabajos de U-SQL a través de un servicio de Visual Studio Team Services

La salida de la compilación del proyecto de U-SQL es un archivo ZIP llamado **USQLProjectName.usqlpack** que incluye todos los scripts U-SQL del proyecto. Puede usar la [tarea de Azure PowerShell del servicio de Visual Studio Team Services](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) con el siguiente script de ejemplo de PowerShell para enviar trabajos de U-SQL directamente desde la canalización de la versión o de la compilación del servicio de Visual Studio Team Services.

```powershell
<#
    This script can be used to submit U-SQL Jobs with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and submit all scripts one-by-one.

    Note: the code behind file for each U-SQL script will be merged into the built U-SQL script in build output.
          
    Example :
        USQLJobSubmission.ps1 -ADLAAccountName "myadlaaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\" -DegreeOfParallelism 2
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLAAccountName, # ADLA account name to submit U-SQL jobs
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DegreeOfParallelism = 1
)

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

## Get U-SQL scripts in U-SQL project build output(.usqlpack file)
Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    $USQLFiles = Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue

    return $USQLFiles
}

## Submit U-SQL scripts to ADLA account one-by-one
Function SubmitAnalyticsJob()
{
    $usqlFiles = GetUsqlFiles

    Write-Output "$($usqlFiles.Count) jobs to be submitted..."

    # Submit each usql script and wait for completion before moving ahead.
    foreach ($usqlFile in $usqlFiles)
    {
        $scriptName = "[Release].[$([System.IO.Path]::GetFileNameWithoutExtension($usqlFile.fullname))]"

        Write-Output "Submitting job for '{$usqlFile}'"

        $jobToSubmit = Submit-AzureRmDataLakeAnalyticsJob -Account $ADLAAccountName -Name $scriptName -ScriptPath $usqlFile -DegreeOfParallelism $DegreeOfParallelism
        
        LogJobInformation $jobToSubmit
        
        Write-Output "Waiting for job to complete. Job ID:'{$($jobToSubmit.JobId)}', Name: '$($jobToSubmit.Name)' "
        $jobResult = Wait-AzureRmDataLakeAnalyticsJob -Account $ADLAAccountName -JobId $jobToSubmit.JobId  
        LogJobInformation $jobResult
    }
}

Function LogJobInformation($jobInfo)
{
    Write-Output "************************************************************************"
    Write-Output ([string]::Format("Job Id: {0}", $(DefaultIfNull $jobInfo.JobId)))
    Write-Output ([string]::Format("Job Name: {0}", $(DefaultIfNull $jobInfo.Name)))
    Write-Output ([string]::Format("Job State: {0}", $(DefaultIfNull $jobInfo.State)))
    Write-Output ([string]::Format("Job Started at: {0}", $(DefaultIfNull  $jobInfo.StartTime)))
    Write-Output ([string]::Format("Job Ended at: {0}", $(DefaultIfNull  $jobInfo.EndTime)))
    Write-Output ([string]::Format("Job Result: {0}", $(DefaultIfNull $jobInfo.Result)))
    Write-Output "************************************************************************"
}

Function DefaultIfNull($item)
{
    if ($item -ne $null)
    {
        return $item
    }
    return ""
}

Function Main()
{
    Write-Output ([string]::Format("ADLA account: {0}", $ADLAAccountName))
    Write-Output ([string]::Format("Root folde for usqlpack: {0}", $ArtifactsRoot))
    Write-Output ([string]::Format("AU count: {0}", $DegreeOfParallelism))

    Write-Output "Starting USQL script deployment..."
    
    SubmitAnalyticsJob

    Write-Output "Finished deployment..."
}

Main
```

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>Implementación de trabajos de U-SQL a través de Azure Data Factory

Además de enviar trabajos de U-SQL directamente desde un servicio de Visual Studio Team Services, también puede cargar los scripts compilados en Azure Data Lake Store o Azure Blob Storage y [ejecutar los trabajos programados a través de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

Use la [tarea de Azure PowerShell del servicio de Visual Studio Team Services](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) con el siguiente script de PowerShell de ejemplo para cargar los scripts U-SQL en la cuenta de Azure Data Lake Store.

```powershell
<#
    This script can be used to upload U-SQL files to ADLS with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and upload all scripts to ADLS one-by-one.
          
    Example :
        FileUpload.ps1 -ADLSName "myadlsaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\"
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLSName, # ADLS account name to upload U-SQL scripts
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DesitinationFolder = "USQLScriptSource" # Desitination folder in ADLS
)

Function UploadResources()
{
    Write-Host "************************************************************************"
    Write-Host "Uploading files to $ADLSName"
    Write-Host "***********************************************************************"

    $usqlScripts = GetUsqlFiles

    $files = @(get-childitem $usqlScripts -recurse)
    foreach($file in $files)
    {
        Write-Host "Uploading file: $($file.Name)"
        Import-AzureRmDataLakeStoreItem -AccountName $ADLSName -Path $file.FullName -Destination "/$(Join-Path $DesitinationFolder $file)" -Force
    }
}

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    return Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue
}

UploadResources
```

## <a name="cicd-for-u-sql-database"></a>CI/CD para la base de datos de U-SQL

Herramientas de Azure Data Lake para Visual Studio proporciona la plantilla del proyecto de base de datos de U-SQL que permite a los desarrolladores desarrollar, administrar e implementar las bases de datos de U-SQL de forma rápida y sencilla. [Más información acerca del proyecto de base de datos de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="build-u-sql-database-project"></a>Compilación de un proyecto de base de datos de U-SQL

### <a name="get-nuget-package"></a>Obtención de un paquete NuGet

MSBuild no proporciona compatibilidad integrada con el tipo de proyecto de base de datos de U-SQL. Para agregar esta capacidad, es preciso agregar una referencia de la solución al [paquete Nuget Microsoft.Azure.DataLake.USQL.SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) que agregue el servicio de lenguaje requerido.

Para agregar dicha referencia, puede hacer clic con el botón derecho en la solución en el Explorador de soluciones y elegir **Administrar paquetes NuGet** para la solución y, después, buscar el paquete NuGet e instalarlo. O bien puede agregar un archivo llamado "packages.config" a la carpeta de la solución y agregarle el contenido siguiente.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-database-project-with-msbuild-command-line"></a>Compilación de un proyecto de base de datos de U-SQL con la línea de comandos de MSBuild

Puede llamar a la línea de comandos de MSBuild estándar y pasar la referencia del paquete NuGet del SDK de U-SQL como argumento adicional, como se hace a continuación, para compilar el proyecto de base de datos de U-SQL:

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

El argumento `USQLSDKPath=<U-SQL Nuget package>\build\runtime` hace referencia a la ruta de instalación del paquete NuGet para el servicio de lenguaje de U-SQL.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Integración continua con un servicio de Visual Studio Team Services

Además de la línea de comandos, los clientes también pueden utilizar la tarea de **compilación de Visual Studio** o de **MSBuild** para compilar proyectos de base de datos de U-SQL en un servicio de Visual Studio Team Services. Para configurar la tarea de compilación, asegúrese de que:

1.  Agrega la tarea de restauración de NuGet para obtener la solución a la que hace referencia el paquete NuGet incluyendo `Azure.DataLake.USQL.SDK`, con el fin de que MSBuild pueda encontrar el lenguaje U-SQL de destino. Establezca **Opciones avanzadas > Directorio de destino** como `$(Build.SourcesDirectory)/packages` si desea usar el ejemplo de argumentos de MSBuild directamente en el paso 2.

    ![Tarea de MSBuild de CI/CD del conjunto de Data Lake para el proyecto de U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

    ![Tarea de Nuget de CI/CD del conjunto de Data Lake para el proyecto de U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  Establece los argumentos de MSBuild. Puede establecer los argumentos en la tarea de compilación de Visual Studio o de MSBuild como de muestra a continuación, o bien puede definir variables para estos argumentos en la definición de la compilación de VSTS.

    ```
    /p:USQLSDKPath=/p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime
    ```

    ![Variables de MSBuild de CI/CD del conjunto de Data Lake para el proyecto de base de datos de U-SQL](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

### <a name="u-sql-database-project-build-output"></a>Salida de compilación de un proyecto de base de datos de U-SQL

La salida de compilación del proyecto de base de datos de U-SQL es un paquete de implementación de la base de datos de U-SQL, denominado con el sufijo `.usqldbpack`. El paquete `.usqldbpack` es un archivo ZIP que incluye todas las instrucciones DDL en un único script U-SQL en la carpeta DDL y todos los archivos DLL y archivos adicionales para los ensamblados en la carpeta Temp.

## <a name="test-table-valued-function-and-stored-procedure"></a>Prueba de la función con valores de tabla y del procedimiento almacenado

En este momento, no se admite la adición directa de casos de prueba a los procedimientos almacenados y las funciones con valores de tabla. Como alternativa, puede crear un proyecto de U-SQL que tenga scripts U-SQL que llamen a dichas funciones y escribir casos de prueba para ellos. Siga estos pasos para configurar casos de prueba para las funciones con valores de tabla y para los procedimientos almacenados definidos en el proyecto de base de datos de U-SQL:

1.  Cree un proyecto de U-SQL para realizar las pruebas y escriba scripts U-SQL que llamen a las funciones con valores de tabla y a los procedimientos almacenados.
2.  Agregue referencia de base de datos a este proyecto de U-SQL. Para obtener la definición de la función con valores de tabla y del procedimiento almacenado, es preciso hacer referencia al proyecto de base de datos que contiene la instrucción de DDL. [Más información acerca de la referencia de base de datos](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).
3.  Agregue casos de prueba de los scripts U-SQL que llamen a procedimientos almacenados y a funciones con valores de tabla. [Aprenda a agregar casos de prueba para un script U-SQL](data-lake-analytics-cicd-test.md#test-u-sql-scripts).

## <a name="deploy-u-sql-database-through-visual-studio-team-service"></a>Implementación de una base de datos de U-SQL a través de un servicio de Visual Studio Team Services

`PackageDeploymentTool.exe` proporciona interfaces de programación y de línea de comandos que ayudan a implementar el paquete de implementación de bases de datos de U-SQL (.usqldbpack). El SDK se incluye en el [paquete NuGet del SDK de U-SQL](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), que se encuentra en build/runtime/PackageDeploymentTool.exe. Mediante el uso de `PackageDeploymentTool.exe` puede implementar bases de datos de U-SQL tanto en Azure Data Lake Analytics como en una cuenta local.

>[!NOTE]
>
>Pronto llegarán la compatibilidad con la línea de comandos de PowerShell y la compatibilidad con la tarea de versión del servicio de Visual Studio Team Services para la implementación de base de datos de U-SQL.
>

Siga estos pasos para configurar la tarea de implementación de base de datos el servicio de Visual Studio Team Services:

1. Agregue una tarea de Script de PowerShell a la canalización de compilación o de versión, y ejecute el siguiente script de PowerShell. Esta tarea ayuda a obtener las dependencias del SDK de Azure para `PackageDeploymentTool.exe` y `PackageDeploymentTool.exe`. Puede establecer los parámetros - AzureSDK y - DBDeploymentTool para cargar las dependencias y la herramienta de implementación a algunas carpetas específicas. Pase la ruta de acceso -AzureSDK a `PackageDeploymentTool.exe` como parámetro - AzureSDKPath en el paso 2. 

    ```powershell
    <#
        This script is used for getting dependencies and SDKs for U-SQL database deployment.
        PowerShell command line support for deploying U-SQL database package(.usqldbpack file) will come soon.
        
        Example :
            GetUSQLDBDeploymentSDK.ps1 -AzureSDK "AzureSDKFolderPath" -DBDeploymentTool "DBDeploymentToolFolderPath"
    #>

    param (
        [string]$AzureSDK = "AzureSDK", # Folder to cache Azure SDK dependencies
        [string]$DBDeploymentTool = "DBDeploymentTool", # Folder to cache U-SQL dabatase deployment tool
        [string]$workingfolder = "" # Folder to execute these command lines
    )

    if ([string]::IsNullOrEmpty($workingfolder))
    {
        $scriptpath = $MyInvocation.MyCommand.Path
        $workingfolder = Split-Path $scriptpath
    }
    cd $workingfolder

    echo "workingfolder=$workingfolder, outputfolder=$outputfolder"
    echo "Downloading required packages..."

    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Analytics/3.2.3-preview -outf Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Store/2.3.3-preview -outf Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.3 -outf Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime/2.3.11 -outf Microsoft.Rest.ClientRuntime.2.3.11.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure/3.3.7 -outf Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure.Authentication/2.3.3 -outf Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip
    iwr https://www.nuget.org/api/v2/package/Newtonsoft.Json/6.0.8 -outf Newtonsoft.Json.6.0.8.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.DataLake.USQL.SDK/ -outf USQLSDK.zip

    echo "Extracting packages..."

    Expand-Archive Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview -Force
    Expand-Archive Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Store.2.3.3-preview -Force
    Expand-Archive Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip -DestinationPath Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.2.3.11.zip -DestinationPath Microsoft.Rest.ClientRuntime.2.3.11 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.3.3.7 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3 -Force
    Expand-Archive Newtonsoft.Json.6.0.8.zip -DestinationPath Newtonsoft.Json.6.0.8 -Force
    Expand-Archive USQLSDK.zip -DestinationPath USQLSDK -Force

    echo "Copy required DLLs to output folder..."

    mkdir $AzureSDK -Force
    mkdir $DBDeploymentTool -Force
    copy Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.Azure.Management.DataLake.Store.2.3.3-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.2.3.11\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3\lib\net452\*.dll $AzureSDK
    copy Newtonsoft.Json.6.0.8\lib\net45\*.dll $AzureSDK
    copy USQLSDK\build\runtime\*.* $DBDeploymentTool
    ```

2. Agregue una **tarea de línea de comandos** a la canalización de compilación o de versión, y rellene el script que llama a `PackageDeploymentTool.exe`. `PackageDeploymentTool.exe` se encuentra en la carpeta $DBDeploymentTool definida. Este es el script de ejemplo: 

    * Implementación local de una base de datos de U-SQL

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Use el modo de autenticación interactiva para implementar una base de datos de U-SQL en la cuenta de Azure Data Lake Analytics:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Use la autenticación interactiva para implementar una base de datos de U-SQL en la cuenta de Azure Data Lake Analytics:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete>
        ```

    * Use la autenticación de certFile para implementar una base de datos de U-SQL en la cuenta de Azure Data Lake Analytics:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

**Descripción del parámetro de PackageDeploymentTool.exe:**

**Parámetros comunes:**

|Parámetro|DESCRIPCIÓN|Valor predeterminado|Obligatorio|
|---------|-----------|-------------|--------|
|Paquete|Ruta de acceso del paquete de implementación de la base de datos de U-SQL que se va a implementar|null|true|
|Base de datos|Nombre de base de datos que se va a implementar o crear|maestro|false|
|LogFile|Ruta de acceso del archivo de registro, de forma predeterminada es la salida estándar (consola)|null|false|
|LogLevel|Nivel de registro: Detallado, Normal, Advertencia y Error|LogLevel.Normal|false|

**Parámetro para la implementación local:**

|Parámetro|DESCRIPCIÓN|Valor predeterminado|Obligatorio|
|---------|-----------|-------------|--------|
|DataRoot|Ruta de acceso de la carpeta raíz de datos local|null|true|

**Parámetro para la implementación de Azure Data Lake Analytics:**

|Parámetro|DESCRIPCIÓN|Valor predeterminado|Obligatorio|
|---------|-----------|-------------|--------|
|Cuenta|Especifica la cuenta de Azure Data Lake Analytics en la que se realiza la implementación, por número de cuenta|null|true|
|ResourceGroup|Nombre del grupo de recursos de Azure para la cuenta de Azure Data Lake Analytics|null|true|
|SubscriptionId|Identificador de la suscripción de Azure para la cuenta de Azure Data Lake Analytics|null|true|
|Inquilino|Nombre del inquilino (nombre de dominio del directorio de AAD, se puede encontrar en la página de administración de la suscripción de Azure Portal)|null|true|
|AzureSDKPath|Ruta de acceso para buscar los ensamblados dependientes en el SDK de Azure|null|true|
|Interactive|Use el modo interactivo para la autenticación, o no|false|false|
|ClientId|Identificador de aplicación de AAD para la autenticación interactiva, se requiere en la autenticación no interactiva|null|se requiere para la autenticación no interactiva|
|Secrete|Secreto y contraseña de ninguna autenticación interactiva, solo se debe usar en un entorno de confianza y seguro|null|necesario para la autenticación no interactiva o use SecreteFile|
|SecreteFile|El archivo guarda el secreto o la contraseña de la autenticación no interactiva, asegúrese de que solo pueda leerlo el usuario actual|null|necesario para la autenticación no interactiva o use Secrete|
|CertFile|El archivo guarda la certificación X.509 para la autenticación no interactiva; el valor predeterminado es usar la autenticación Secrete del cliente|null|false|
|JobPrefix|Prefijo del trabajo DDL de U-SQL para la implementación de la base de datos|Deploy_ + DateTime.Now|false|

## <a name="next-steps"></a>Pasos siguientes

- [Prueba de código de Azure Data Lake Analytics](data-lake-analytics-cicd-test.md)
- [Ejecución local de scripts U-SQL](data-lake-analytics-data-lake-tools-local-run.md)
- [Uso de un proyecto de base de datos de U-SQL para desarrollar bases de datos de U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)
