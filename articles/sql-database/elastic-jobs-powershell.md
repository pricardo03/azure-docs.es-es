---
title: Creación de un agente de trabajos elásticos de Azure SQL Database mediante PowerShell | Microsoft Docs
description: Aprenda a crear un agente de trabajos elásticos mediante PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: johnpaulkee
ms.author: joke
ms.reviwer: sstein
manager: craigg
ms.date: 03/13/2019
ms.openlocfilehash: eb5066185f9301450a68276dd4b2ce2123231b34
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666800"
---
# <a name="create-an-elastic-job-agent-using-powershell"></a>Creación de un agente de trabajos elásticos mediante PowerShell

Los [trabajos elásticos](sql-database-job-automation-overview.md#elastic-database-jobs) habilitan la ejecución de uno o más scripts de Transact-SQL (T-SQL) en paralelo en varias bases de datos.

En este tutorial aprenderá los pasos necesarios para ejecutar una consulta en varias bases de datos:

> [!div class="checklist"]
> * Creación de un agente de trabajos elásticos
> * Creación de credenciales de trabajo para que los trabajos puedan ejecutar scripts en sus destinos
> * Definición de los destinos (servidores, grupos elásticos, bases de datos, mapas de particiones) en los que desea ejecutar el trabajo
> * Creación de credenciales de ámbito de base de datos en las bases de datos de destino para que el agente pueda conectar y ejecutar trabajos
> * Creación de un trabajo
> * Incorporación de pasos de trabajo a un trabajo
> * Inicio de la ejecución de un trabajo
> * Supervisión de un trabajo

## <a name="prerequisites"></a>Requisitos previos

La versión actualizada de los trabajos de Elastic Database tiene un nuevo conjunto de cmdlets de PowerShell para su utilización durante la migración. Estos nuevos cmdlets transfieren todas las credenciales de trabajos, destinos (incluidas bases de datos, servidores y colecciones personalizadas), desencadenadores de trabajos, programaciones de trabajos, contenido de trabajos y trabajos existentes a un nuevo agente de trabajos elásticos.

### <a name="install-the-latest-elastic-jobs-cmdlets"></a>Instalación de los últimos cmdlets de trabajos elásticos

Si no tiene una suscripción de Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

Instale el módulo **Az.Sql** 1.1.1-preview para obtener los cmdlets de trabajos elásticos más recientes. Ejecute los comandos siguientes en PowerShell con acceso administrativo.

```powershell
# Installs the latest PackageManagement powershell package which PowershellGet v1.6.5 is dependent on
Find-Package PackageManagement -RequiredVersion 1.1.7.2 | Install-Package -Force

# Installs the latest PowershellGet module which adds the -AllowPrerelease flag to Install-Module
Find-Package PowerShellGet -RequiredVersion 1.6.5 | Install-Package -Force

# Restart your powershell session with administrative access

# Places Az.Sql preview cmdlets side by side with existing Az.Sql version
Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease

# Import the Az.Sql module
Import-Module Az.Sql -RequiredVersion 1.1.1

# Confirm if module successfully imported - if the imported version is 1.1.1, then continue
Get-Module Az.Sql
```

- Además del módulo **Az.Sql** 1.1.1-preview, este tutorial también requiere el módulo *sqlserver* de PowerShell. Para más información, consulte [Instalación de un módulo de SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module).


## <a name="create-required-resources"></a>Creación de los recursos necesarios

La creación de un agente de trabajos elásticos requiere una base de datos (S0 o superior) para usarla como [base de datos de trabajos](sql-database-job-automation-overview.md#job-database). 

*El script siguiente crea un nuevo grupo de recursos, servidor y base de datos para usarla como la base de datos de trabajos. El siguiente script también crea un segundo servidor con 2 bases de datos en blanco para ejecutar trabajos.*

Los trabajos elásticos no tienen ningún requisito de nomenclatura específico, por lo que puede usar las convenciones de nomenclatura que desee, siempre y cuando cumplan con los [requisitos de Azure](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

```powershell
# Sign in to your Azure account
Connect-AzAccount

# Create a resource group
Write-Output "Creating a resource group..."
$ResourceGroupName = Read-Host "Please enter a resource group name"
$Location = Read-Host "Please enter an Azure Region"
$Rg = New-AzResourceGroup -Name $ResourceGroupName -Location $Location
$Rg

# Create a server
Write-Output "Creating a server..."
$AgentServerName = Read-Host "Please enter an agent server name"
$AgentServerName = $AgentServerName + "-" + [guid]::NewGuid()
$AdminLogin = Read-Host "Please enter the server admin name"
$AdminPassword = Read-Host "Please enter the server admin password"
$AdminPasswordSecure = ConvertTo-SecureString -String $AdminPassword -AsPlainText -Force
$AdminCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AdminLogin, $AdminPasswordSecure
$AgentServer = New-AzSqlServer -ResourceGroupName $ResourceGroupName -Location $Location -ServerName $AgentServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($AdminCred)

# Set server firewall rules to allow all Azure IPs
Write-Output "Creating a server firewall rule..."
$AgentServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$AgentServer

# Create the job database
Write-Output "Creating a blank SQL database to be used as the Job Database..."
$JobDatabaseName = "JobDatabase"
$JobDatabase = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $AgentServerName -DatabaseName $JobDatabaseName -RequestedServiceObjectiveName "S0"
$JobDatabase
```

```powershell
# Create a target server and some sample databases - uses the same admin credential as the agent server just for simplicity
Write-Output "Creating target server..."
$TargetServerName = Read-Host "Please enter a target server name"
$TargetServerName = $TargetServerName + "-" + [guid]::NewGuid()
$TargetServer = New-AzSqlServer -ResourceGroupName $ResourceGroupName -Location $Location -ServerName $TargetServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($AdminCred)

# Set target server firewall rules to allow all Azure IPs
$TargetServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$TargetServer | New-AzSqlServerFirewallRule -StartIpAddress 0.0.0.0 -EndIpAddress 255.255.255.255 -FirewallRuleName AllowAll
$TargetServer

# Create some sample databases to execute jobs against...
$Db1 = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $TargetServerName -DatabaseName "TargetDb1"
$Db1
$Db2 = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $TargetServerName -DatabaseName "TargetDb2"
$Db2
```

## <a name="enable-the-elastic-jobs-preview-for-your-subscription"></a>Habilitación de la versión preliminar de trabajos elásticos para la suscripción

Para usar trabajos elásticos, registre la característica en su suscripción de Azure mediante la ejecución del siguiente comando. Ejecute este comando una vez para la suscripción en la que tiene pensado aprovisionar el agente de trabajos elásticos. No es necesario registrar las suscripciones que solo contengan bases de datos que son destinos de trabajos.

```powershell
Register-AzProviderFeature -FeatureName sqldb-JobAccounts -ProviderNamespace Microsoft.Sql
```

## <a name="create-the-elastic-job-agent"></a>Creación del agente de trabajos elásticos

Un agente de trabajos elásticos es un recurso de Azure para crear, ejecutar y administrar trabajos. El agente ejecuta los trabajos según una programación o como un trabajo único.

El cmdlet **New-AzSqlElasticJobAgent** requiere que exista ya una base de datos de Azure SQL, por lo que los parámetros *ResourceGroupName*, *ServerName*, y *DatabaseName* deben todos apuntar a recursos ya existentes.

```powershell
Write-Output "Creating job agent..."
$AgentName = Read-Host "Please enter a name for your new Elastic Job agent"
$JobAgent = $JobDatabase | New-AzSqlElasticJobAgent -Name $AgentName
$JobAgent
```

## <a name="create-job-credentials-so-that-jobs-can-execute-scripts-on-its-targets"></a>Creación de credenciales de trabajo para que los trabajos puedan ejecutar scripts en sus destinos

Los trabajos usan credenciales de ámbito de base de datos para conectarse a las bases de datos de destino que especifica el grupo de destino después de la ejecución. Estas credenciales de ámbito de base de datos también se usan para conectarse a la base de datos maestra para enumerar todas las bases de datos de un servidor o un grupo elástico, cuando cualquiera de estos se utiliza como el tipo de miembro del grupo de destino.

Las credenciales de ámbito de base de datos se deben crear en la base de datos del trabajo.  
Todas las bases de datos de destino deben tener un inicio de sesión con permisos suficientes para que el trabajo se complete correctamente.

![Credenciales de trabajos elásticos](media/elastic-jobs-overview/job-credentials.png)

Además de las credenciales de la imagen, observe la adición de los comandos *GRANT* en el siguiente script. Estos permisos son necesarios para el script que hemos elegido para este trabajo de ejemplo. Dado que en el ejemplo se crea una nueva tabla en las bases de datos de destino, cada base de datos de destino necesita los permisos adecuados para ejecutarse correctamente.

Para crear las credenciales de trabajos necesarias (en la base de datos de trabajo), ejecute el script siguiente:

```powershell
# In the master database (target server)
# - Create the master user login
# - Create the master user from master user login
# - Create the job user login
$Params = @{
  'Database' = 'master'
  'ServerInstance' =  $TargetServer.ServerName + '.database.windows.net'
  'Username' = $AdminLogin
  'Password' = $AdminPassword
  'OutputSqlErrors' = $true
  'Query' = "CREATE LOGIN masteruser WITH PASSWORD='password!123'"
}
Invoke-SqlCmd @Params
$Params.Query = "CREATE USER masteruser FROM LOGIN masteruser"
Invoke-SqlCmd @Params
$Params.Query = "CREATE LOGIN jobuser WITH PASSWORD='password!123'"
Invoke-SqlCmd @Params

# For each of the target databases
# - Create the jobuser from jobuser login
# - Make sure they have the right permissions for successful script execution
$TargetDatabases = @( $Db1.DatabaseName, $Db2.DatabaseName )
$CreateJobUserScript =  "CREATE USER jobuser FROM LOGIN jobuser"
$GrantAlterSchemaScript = "GRANT ALTER ON SCHEMA::dbo TO jobuser"
$GrantCreateScript = "GRANT CREATE TABLE TO jobuser"

$TargetDatabases | % {
  $Params.Database = $_

  $Params.Query = $CreateJobUserScript
  Invoke-SqlCmd @Params

  $Params.Query = $GrantAlterSchemaScript
  Invoke-SqlCmd @Params

  $Params.Query = $GrantCreateScript
  Invoke-SqlCmd @Params
}

# Create job credential in Job database for master user
Write-Output "Creating job credentials..."
$LoginPasswordSecure = (ConvertTo-SecureString -String "password!123" -AsPlainText -Force)

$MasterCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "masteruser", $LoginPasswordSecure
$MasterCred = $JobAgent | New-AzSqlElasticJobCredential -Name "masteruser" -Credential $MasterCred

$JobCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "jobuser", $LoginPasswordSecure
$JobCred = $JobAgent | New-AzSqlElasticJobCredential -Name "jobuser" -Credential $JobCred
```

## <a name="define-the-target-databases-you-want-to-run-the-job-against"></a>Definición de las bases de datos de destino en las que desea ejecutar los trabajos

Un [grupo de destino](sql-database-job-automation-overview.md#target-group) define el conjunto de una o varias bases de datos en las que se ejecutará un paso de un trabajo. 

En el fragmento de código siguiente se crean dos grupos de destino: *ServerGroup* y *ServerGroupExcludingDb2*. *ServerGroup* tiene como destino todas las bases de datos que existen en el servidor en el momento de la ejecución y *ServerGroupExcludingDb2* tiene como destino todas las bases de datos del servidor, excepto *TargetDb2*:

```powershell
Write-Output "Creating test target groups..."
# Create ServerGroup target group
$ServerGroup = $JobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroup'
$ServerGroup | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -RefreshCredentialName $MasterCred.CredentialName

# Create ServerGroup with an exclusion of Db2
$ServerGroupExcludingDb2 = $JobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroupExcludingDb2'
$ServerGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -RefreshCredentialName $MasterCred.CredentialName
$ServerGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -Database $Db2.DatabaseName -Exclude
```

## <a name="create-a-job"></a>Creación de un trabajo

```powershell
Write-Output "Creating a new job"
$JobName = "Job1"
$Job = $JobAgent | New-AzSqlElasticJob -Name $JobName -RunOnce
$Job
```

## <a name="create-a-job-step"></a>Creación de un paso de trabajo

Este ejemplo define dos pasos de trabajo para el trabajo que se va a ejecutar. El primer paso de trabajo (*step1*) crea una nueva tabla (*Step1Table*) en cada base de datos del grupo de destino *ServerGroup*. El segundo paso de trabajo (*step2*) crea una nueva tabla (*Step2Table*) en cada base de datos excepto en *TargetDb2*, ya que el grupo de destino [definido anteriormente](#define-the-target-databases-you-want-to-run-the-job-against) especificó que se excluyera.

```powershell
Write-Output "Creating job steps"
$SqlText1 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step1Table')) CREATE TABLE [dbo].[Step1Table]([TestId] [int] NOT NULL);"
$SqlText2 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step2Table')) CREATE TABLE [dbo].[Step2Table]([TestId] [int] NOT NULL);"

$Job | Add-AzSqlElasticJobStep -Name "step1" -TargetGroupName $ServerGroup.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText1
$Job | Add-AzSqlElasticJobStep -Name "step2" -TargetGroupName $ServerGroupExcludingDb2.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText2
```


## <a name="run-the-job"></a>Ejecución del trabajo

Para iniciar el trabajo inmediatamente, ejecute el comando siguiente:

```powershell
Write-Output "Start a new execution of the job..."
$JobExecution = $Job | Start-AzSqlElasticJob
$JobExecution
```

Después de una correcta finalización debería ver dos nuevas tablas en TargetDb1 y solo una en TargetDb2:


   ![comprobación de las tablas nuevas en SSMS](media/elastic-jobs-overview/job-execution-verification.png)




## <a name="monitor-status-of-job-executions"></a>Supervisión del estado de las ejecuciones de trabajos

Los siguientes fragmentos de código permiten obtener la información sobre las ejecuciones:

```powershell
# Get the latest 10 executions run
$JobAgent | Get-AzSqlElasticJobExecution -Count 10

# Get the job step execution details
$JobExecution | Get-AzSqlElasticJobStepExecution

# Get the job target execution details
$JobExecution | Get-AzSqlElasticJobTargetExecution -Count 2
```

## <a name="schedule-the-job-to-run-later"></a>Programación de un trabajo para que se ejecute más tarde

Para programar un trabajo para que se ejecute en un momento determinado, ejecute el siguiente comando:

```powershell
# Run every hour starting from now
$Job | Set-AzSqlElasticJob -IntervalType Hour -IntervalCount 1 -StartTime (Get-Date) -Enable
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Elimine los recursos de Azure que ha creado en este tutorial eliminando el grupo de recursos.

> [!TIP]
> Si piensa seguir trabajando con estos trabajos, no limpie los recursos creados en este artículo. Si no va a continuar, use los siguientes pasos para eliminar todos los recursos creados en este artículo.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $ResourceGroupName
```


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ejecutó un script de Transact-SQL en un conjunto de bases de datos.  Ha aprendido a realizar las siguientes tareas:

> [!div class="checklist"]
> * Creación de un agente de trabajos elásticos
> * Creación de credenciales de trabajo para que los trabajos puedan ejecutar scripts en sus destinos
> * Definición de los destinos (servidores, grupos elásticos, bases de datos, mapas de particiones) en los que desea ejecutar el trabajo
> * Creación de credenciales de ámbito de base de datos en las bases de datos de destino para que el agente pueda conectar y ejecutar trabajos
> * Creación de un trabajo
> * Adición de un paso de trabajo al trabajo
> * Inicio de la ejecución del trabajo
> * Supervisión del trabajo

> [!div class="nextstepaction"]
>[Administración de trabajos elásticos mediante Transact-SQL](elastic-jobs-tsql.md)
