---
title: Introducción a trabajos de base de datos elástica | Microsoft Docs
description: Utilice los trabajos de Elastic Database para ejecutar scripts de T-SQL que abarcan varias bases de datos.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 6d794fb14b7f581c9e9b92dc581de97e0a236630
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59786479"
---
# <a name="getting-started-with-elastic-database-jobs"></a>Introducción a trabajos de Elastic Database

Trabajos de Elastic Database (versión preliminar) para Azure SQL Database permite ejecutar de forma confiable scripts de T-SQL que abarcan varias bases de datos al tiempo que realizan reintentos automáticos y ofrecen garantías de finalización futura. Para más información sobre la característica de trabajo de Elastic Database, vea [Trabajos elásticos](sql-database-elastic-jobs-overview.md).

Este artículo amplía el ejemplo que aparece en [Introducción a las herramientas de Elastic Database](sql-database-elastic-scale-get-started.md). Cuando termine, habrá aprendido a crear y administrar trabajos que administran un grupo de bases de datos relacionadas. No es necesario usar las herramientas de escalado elástico para aprovechar las ventajas de los trabajos elásticos.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

Descargue [Introducción al ejemplo de herramientas de Elastic Database](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Creación de un administrador de mapas de particiones con la aplicación de ejemplo

Aquí se crea un administrador del mapa de particiones junto con varias particiones, seguido de la inserción de datos en las particiones. Si ya dispone de particiones configuradas con datos particionados en ellas, puede omitir los pasos siguientes y pasar a la sección siguiente.

1. Cree y ejecute la aplicación de ejemplo **Introducción a las herramientas de Elastic Database**. Siga los pasos hasta el paso 7 de la sección [Descarga y ejecución de la aplicación de ejemplo](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Al final del paso 7, verá el siguiente símbolo del sistema:

   ![símbolo del sistema](./media/sql-database-elastic-query-getting-started/cmd-prompt.png)

2. En la ventana de comandos, escriba "1" y pulse **Entrar**. De esta forma, se creará el administrador de mapas de particiones y se agregarán dos particiones al servidor. A continuación, escriba "3" y pulse **Entrar**; repita esta acción cuatro veces. De esta forma, se insertan las filas de datos de ejemplo en sus particiones.
3. [Azure Portal](https://portal.azure.com) debería mostrar tres nuevas bases de datos:

   ![Confirmación de Visual Studio](./media/sql-database-elastic-query-getting-started/portal.png)

   En este punto, creamos una colección de bases de datos personalizada que refleja todas las bases de datos en el mapa de particiones. Así, podemos crear y ejecutar un trabajo que agrega una nueva tabla entre las particiones.

Aquí normalmente se crearía un destino del mapa de particiones, con el cmdlet **New-AzureSqlJobTarget** . Se debe establecer la base de datos de administrador de mapa de particiones como destino de la base de datos y luego especificar el mapa de particiones específico como destino. En lugar de eso, vamos a enumerar todas las bases de datos del servidor y a agregar las bases de datos, excepto la base de datos maestra, a la nueva colección personalizada.

## <a name="creates-a-custom-collection-and-add-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Crea una colección personalizada y agregar todas las bases de datos del servidor, excepto la maestra, al destino de la colección personalizada

   ```powershell
    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName
    $dbsinserver | %{
    $currentdb = $_.DatabaseName
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target."
        }

        else
        {
            throw $_
        }

    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
   }
   ```

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Crear un script T-SQL para su ejecución transversal en las bases de datos

   ```powershell
    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script
   ```

## <a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Creación del trabajo para ejecutar un script en todo el grupo personalizado de bases de datos

   ```powershell
    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="execute-the-job"></a>Ejecución del trabajo

Puede usar este script de PowerShell para ejecutar un trabajo existente:

Actualice la variable siguiente para que refleje el nombre del trabajo que se quiera ejecutar:

   ```powershell
    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="retrieve-the-state-of-a-single-job-execution"></a>Recuperación del estado de ejecución de un único trabajo

Use el mismo cmdlet **Get-AzureSqlJobExecution** con el parámetro **IncludeChildren** para ver el estado de ejecuciones de trabajos secundarios, es decir, el estado específico de cada ejecución en cada base de datos destino del trabajo.

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions
   ```

## <a name="view-the-state-across-multiple-job-executions"></a>Ver el estado de varias ejecuciones de trabajos

El cmdlet **Get-AzureSqlJobExecution** tiene varios parámetros opcionales que sirven para mostrar varias ejecuciones del trabajo, filtradas por los parámetros proporcionados. Aquí mostramos algunas de las posibles formas de usar Get-AzureSqlJobExecution:

Recuperar todas las ejecuciones de trabajos activos de nivel superior:

   ```powershell
    Get-AzureSqlJobExecution
   ```

Recuperar todas las ejecuciones de trabajos de nivel superior, incluidas las ejecuciones de trabajos inactivos:

   ```powershell
    Get-AzureSqlJobExecution -IncludeInactive
   ```

Recuperar todas las ejecuciones de trabajos secundarios de un identificador de ejecución de trabajos proporcionado, incluidas las ejecuciones de trabajos inactivos:

   ```powershell
    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren
   ```

Recuperar todas las ejecuciones de trabajos creadas con una combinación de programación y trabajo, incluidos los trabajos inactivos:

   ```powershell
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive
   ```

Recuperar todos los trabajos que se destinan a un mapa de particiones especificado, incluidos los trabajos inactivos:

   ```powershell
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Recuperar todos los trabajos que se destinan a una colección personalizada especificada, incluidos los trabajos inactivos:

   ```powershell
    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive
   ```

Recuperar la lista de ejecuciones de tareas de trabajos dentro de la ejecución de un trabajo específico:

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions
   ```

Recuperar los detalles de ejecución de tareas de trabajo:

Puede usar este script de PowerShell para ver los detalles de una ejecución de tareas de trabajo, lo que resulta especialmente útil al depurar errores de ejecución.

   ```powershell
    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution
   ```

## <a name="retrieve-failures-within-job-task-executions"></a>Recuperación de errores dentro de las ejecuciones de tareas de trabajo

El objeto JobTaskExecution incluye una propiedad para el ciclo de vida de la tarea y una propiedad de mensaje. Si no se realiza correctamente la ejecución de tareas de un trabajo, la propiedad Lifecycle da se establece en *Failed* y la propiedad Message se establece en el mensaje de excepción resultante y en su pila. Si un trabajo no se realiza correctamente, es importante ver los detalles de las tareas de trabajo que no se realizaron correctamente en un trabajo determinado.

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions)
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }
   ```

## <a name="waiting-for-a-job-execution-to-complete"></a>Esperar a que se complete la ejecución de un trabajo

Este script de PowerShell sirve para esperar a que una tarea de trabajo se complete:

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="create-a-custom-execution-policy"></a>Crear una directiva de ejecución personalizada

Los trabajos de Elastic Database admiten la creación de directivas de ejecución personalizadas que se pueden aplicar al iniciar trabajos.

Actualmente, las directivas de ejecución permiten definir:

* Nombre: Identificador de la directiva de ejecución.
* Tiempo de espera del trabajo: tiempo total antes de que Trabajos de Elastic Database cancele un trabajo.
* Intervalo de reintento inicial: intervalo de espera antes del primer reintento.
* Intervalo máximo de reintento: límite de intervalos de reintentos que se va a usar.
* Coeficiente de retroceso del intervalo de reintento: coeficiente que se usa para calcular el siguiente intervalo entre reintentos.  Se usa la siguiente fórmula: (Intervalo de reintento inicial) * Math.pow((Coeficiente de retroceso del intervalo), (Número de reintentos) - 2).
* Número máximo de intentos: número máximo de reintentos para llevar a cabo un trabajo.

La directiva de ejecución predeterminada usa los valores siguientes:

* Nombre: Directiva de ejecución predeterminada
* Tiempo de espera del trabajo: 1 semana
* Intervalo de reintento inicial:  100 milisegundos
* Intervalo máximo de reintento: 30 minutos
* Coeficiente de intervalo de reintento: 2
* Número máximo de intentos: 2 147 483 647

Crear la directiva de ejecución que quiera:

   ```powershell
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy
   ```

### <a name="update-a-custom-execution-policy"></a>Actualización de una directiva de ejecución personalizada

Actualizar la directiva de ejecución que se quiere actualizar:

   ```powershell
    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
   ```

## <a name="cancel-a-job"></a>Cancelación de un trabajo

Los trabajos de Elastic Database admiten solicitudes de cancelación de trabajos.  Si Trabajos de Elastic Database detecta una solicitud de cancelación para un trabajo que está ejecutándose en ese momento, intenta detener el trabajo.

Trabajos de Elastic Database puede realizar una cancelación de dos formas distintas:

1. Cancelar las tareas en ejecución actualmente: si se detecta una cancelación mientras se ejecuta una tarea, se intenta cancelar el aspecto de la tarea que se está ejecutando actualmente.  Por ejemplo:  si hay una consulta de larga ejecución en curso en el momento en que se intenta realizar una cancelación, se intentará cancelar la consulta.
2. Cancelación de reintentos de tareas: si el subproceso de control detecta una cancelación antes de iniciar una tarea para su ejecución, evitará iniciar la tarea y declarará cancelada la solicitud.

Si se solicita una cancelación de un trabajo para un trabajo primario, se respeta la solicitud de cancelación para el trabajo primario y para todos sus trabajos secundarios.

Para enviar una solicitud de cancelación, use el cmdlet **Stop-AzureSqlJobExecution** y establezca el parámetro **JobExecutionId**.

   ```powershell
    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId
   ```

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Eliminación de un trabajo por nombre y el historial de trabajos

Los trabajos de Elastic Database admiten la eliminación asincrónica de trabajos. Un trabajo se puede marcar para eliminación y el sistema elimina el trabajo y su historial de trabajos una vez completadas todas las ejecuciones de trabajos para ese trabajo. El sistema no cancela automáticamente las ejecuciones de trabajos activos.  

En su lugar, se debe invocar Stop-AzureSqlJobExecution para cancelar las ejecuciones de trabajos activos.

Para desencadenar la eliminación del trabajo, use el cmdlet **Remove-AzureSqlJob** y establezca el parámetro **JobName**.

   ```powershell
    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
   ```

## <a name="create-a-custom-database-target"></a>Creación de un destino de base de datos personalizada

En los trabajos de Elastic Database, se pueden definir destinos de base de datos personalizada que sirven para su ejecución directa o para su inclusión en un grupo de base de datos personalizada. Puesto que los **grupos elásticos** todavía no se admiten directamente a través de las API de PowerShell, cree solo un destino de base de datos personalizada y un destino de la colección de bases de datos personalizada que englobe todas las bases de datos del grupo.

Establecimiento de las siguientes variables para que reflejen la información de base de datos que se quiera:

   ```powershell
    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName
   ```

## <a name="create-a-custom-database-collection-target"></a>Creación de un destino de colección de bases de datos personalizada

Puede definirse un destino de colección de bases de datos personalizada para habilitar la ejecución transversal en varios destinos de base de datos definidos. Después de crear un grupo de bases de datos, las bases de datos se pueden asociar al destino de la colección personalizada.

Establecimiento de las siguientes variables para que reflejen la configuración de destino de la colección personalizada que se quiera:

   ```powershell
    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="add-databases-to-a-custom-database-collection-target"></a>Adición de bases de datos a un destino de colección de bases de datos personalizada

Los destinos de base de datos se pueden asociar a los destinos de colección de bases de datos personalizada para crear un grupo de bases de datos. Cada vez que se crea un trabajo que se destina a una colección de bases de datos personalizada, se expande para dirigirse a las bases de datos asociadas al grupo en el momento de ejecución.

Adición de la base de datos que se quiera a una colección personalizada específica:

   ```powershell
    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName
   ```

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Revisión de las bases de datos incluidas en un destino de colección de bases de datos personalizada

Use el cmdlet **Get-AzureSqlJobTarget** para recuperar las bases de datos secundarias en un destino de la colección de bases de datos personalizada.

   ```powershell
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets
   ```

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Creación de un trabajo para ejecutar un script transversalmente en un destino de colección de bases de datos personalizada

Use el cmdlet **New-AzureSqlJob** para crear un trabajo en un grupo de bases de datos definido por un destino de la colección de base de datos personalizada. Trabajos de Elastic Database expande el trabajo en varios trabajos secundarios, cada uno correspondiente a una base de datos asociada al destino de la colección de bases de datos personalizada, y garantiza que el script se ejecuta en cada una de las bases de datos. De nuevo, es importante que los scripts sean idempotentes para que sean resistentes a los reintentos.

   ```powershell
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job
   ```

## <a name="data-collection-across-databases"></a>Recopilación de datos de una base de datos a otra

**Trabajos de Elastic Database** es compatible con la ejecución de una consulta transversal en un grupo de bases de datos y envía los resultados a la tabla de la base de datos especificada. La tabla se puede consultar a posteriori para ver los resultados de la consulta de cada base de datos. Esto ofrece un mecanismo asincrónico para ejecutar una consulta transversalmente en varias bases de datos. Los casos de error, como que una de las bases de datos no esté disponible temporalmente, se controlan automáticamente a través de reintentos.

Se crea automáticamente la tabla de destino especificada, si todavía no existe ninguna que coincida con el esquema del conjunto de resultados devuelto. Si la ejecución de un script devuelve varios conjuntos de resultados, Trabajos de Elastic Database solo envía el primero a la tabla de destino proporcionada.

El siguiente script de PowerShell sirve para ejecutar un script que recopile sus resultados en una tabla especificada. Este script presupone que se creó un script T-SQL que genera un único conjunto de resultados y se creó un destino de la colección de bases de datos personalizada.

Establecimiento de las siguientes opciones para que reflejen el script, las credenciales y el destino de ejecución que se quieran:

   ```powershell
    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
   ```

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Creación e inicio de un trabajo en escenarios de recolección de datos

   ```powershell
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution
   ```

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Creación de una programación para la ejecución de trabajos con un desencadenador de trabajo

El siguiente script de PowerShell sirve para crear una programación recurrente. Este script usa un intervalo de un minuto, pero New-AzureSqlJobSchedule también admite los parámetros -DayInterval, -HourInterval, -MonthInterval y -WeekInterval. Se pueden crear programaciones que se ejecutan una sola vez pasando -OneTime.

Creación de una programación:

   ```powershell
    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime
    Write-Output $schedule
   ```

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Creación de un desencadenador de trabajo para que un trabajo se ejecute según una programación de tiempo

Se puede definir un desencadenador de trabajo para que un trabajo se ejecute según una programación de tiempo. El siguiente script de PowerShell sirve para crear un desencadenador de trabajo.

Establecimiento de las siguientes variables para que se correspondan con el trabajo y la programación que se quiera:

   ```powershell
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
    Write-Output $jobTrigger
   ```

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Eliminación de una asociación programada para detener la ejecución de un trabajo en la programación

Para suspender la ejecución de un trabajo recurrente a través de un desencadenador de trabajo, se puede quitar el desencadenador de trabajo.
Quite un desencadenador de trabajo para detener un trabajo que se ejecute según una programación con el cmdlet **Remove-AzureSqlJobTrigger** .

   ```powershell
    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName
   ```

## <a name="import-elastic-database-query-results-to-excel"></a>Importación de los resultados de la consulta de base de datos elástica a Excel

 Puede importar los resultados de una consulta a un archivo Excel.

1. Inicie Excel 2013.
2. Diríjase a la barra de herramientas **Datos** .
3. Haga clic en **Desde otros orígenes** y luego en **Desde SQL Server**.

   ![Importación de Excel desde otros orígenes](./media/sql-database-elastic-query-getting-started/exel-sources.png)

4. En el **Asistente de conexión de datos** , escriba el nombre del servidor y las credenciales de inicio de sesión. A continuación, haga clic en **Siguiente**.
5. En el cuadro de diálogo **Seleccione la base de datos que contiene la información que desea**, seleccione la base de datos **ElasticDBQuery**.
6. Seleccione la tabla **Customers** de la vista de lista y haga clic en **Siguiente**. Haga clic en **Finalizar**.
7. En el formulario **Importar datos**, en **Seleccione cómo desea ver estos datos en el libro**, seleccione **Tabla** y haga clic en **Aceptar**.

Todas las filas de la tabla **Clientes** , almacenadas en distintas particiones, completan la hoja de Excel.

## <a name="next-steps"></a>Pasos siguientes

Ahora puede usar las funciones de datos de Excel. Use la cadena de conexión con el nombre de servidor, el nombre de base de datos y las credenciales para conectar su BI y las herramientas de integración de datos a la base de datos de consulta elástica. Asegúrese de que SQL Server se admite como origen de datos para la herramienta. Consulte la base de datos de consulta elástica y las tablas externas como cualquier otra base de datos SQL Server y las tablas de SQL Server que quiera conectar con la herramienta.

### <a name="cost"></a>Coste

No hay ningún cargo adicional por usar la característica de consulta de Elastic Database. En cambio, en este momento la característica solo está disponible en bases de datos de nivel Premium o Crítico para la empresa y en grupos elásticos como punto de conexión, pero las particiones pueden pertenecer a cualquier nivel de servicio.

Para obtener información sobre los precios, consulte [Detalles de precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->