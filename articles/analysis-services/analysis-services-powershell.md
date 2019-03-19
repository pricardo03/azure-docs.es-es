---
title: Administración de Azure Analysis Services con PowerShell | Microsoft Docs
description: Administración de Azure Analysis Services con PowerShell.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 177d74a54e4ab4de698cbb63091656cc8b584e2b
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010691"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Administración de Azure Analysis Services con PowerShell

En este artículo se describen los cmdlets de PowerShell que se usan para realizar tareas de administración de bases de datos y del servidor de Azure Analysis Services. 

Las tareas de administración del servidor, como crear o eliminar un servidor, suspender o reanudar operaciones del servidor o cambiar el nivel de servicio, usan cmdlets de Azure Resource Manager (recurso) y de Analysis Services (servidor). Otras tareas de administración de bases de datos, como la incorporación y eliminación de miembros de roles, el procesamiento o la creación de particiones, usan los cmdlets incluidos en el mismo módulo de SqlServer que SQL Server Analysis Services.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>Permisos

Para la mayoría de las tareas de PowerShell, es necesario disponer de privilegios de administración en el servidor de Analysis Services que esté administrando. Las tareas programadas de PowerShell son operaciones desatendidas. La cuenta o entidad de servicio que ejecuta Scheduler debe tener privilegios de administrador en el servidor de Analysis Services. 

Para las operaciones del servidor mediante cmdlets de PowerShell de Azure, su cuenta o la cuenta que ejecuta scheduler también debe pertenecer al rol de propietario del recurso en [Control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Operaciones de administración de recursos 

Módulo - [Az.AnalysisServices](/powershell/module/az.analysisservices)

|Cmdlet|DESCRIPCIÓN| 
|------------|-----------------| 
|[Get-AzAnalysisServicesServer](/powershell/module/az.analysisservices/get-azanalysisservicesserver)|Obtiene los detalles de una instancia del servidor.|  
|[New-AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver)|Crea una instancia de servidor.|   
|[New-AzAnalysisServicesFirewallConfig](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallconfig)|Crea una configuración de firewall de Analysis Services.|   
|[New-AzAnalysisServicesFirewallRule](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallrule)|Crea una regla de firewall de Analysis Services.|   
|[Remove-AzAnalysisServicesServer](/powershell/module/az.analysisservices/remove-azanalysisservicesserver)|Quita una instancia de servidor.|  
|[Resume-AzAnalysisServicesServer](/powershell/module/az.analysisservices/resume-azanalysisservicesserver)|Reanuda una instancia de servidor.|  
|[Suspend-AzAnalysisServicesServer](/powershell/module/az.analysisservices/suspend-azanalysisservicesserver)|Suspende una instancia de servidor.| 
|[Set-AzAnalysisServicesServer](/powershell/module/az.analysisservices/set-azanalysisservicesserver)|Modifica una instancia de servidor.|   
|[Test-AzAnalysisServicesServer](/powershell/module/az.analysisservices/test-azanalysisservicesserver)|Comprueba la existencia de una instancia del servidor.| 

## <a name="server-management-operations"></a>Operaciones de administración de servidor

Módulo: [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|DESCRIPCIÓN| 
|------------|-----------------| 
|[Add-AzAnalysisServicesAccount](/powershell/module/az.analysisservices/add-AzAnalysisServicesaccount)|Agrega una cuenta autenticada para utilizarla con las solicitudes de cmdlet de servidor de Azure Analysis Services.| 
|[Export-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/export-AzAnalysisServicesinstancelog)|Exporta un registro de una instancia de servidor de Analysis Services en la sesión iniciada actualmente entorno tal como se especifica en el comando Add-AzAnalysisServicesAccount|  
|[Restart-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|Reinicia una instancia de servidor de Analysis Services en el entorno actualmente conectado; especificado en el comando Add-AzAnalysisServicesAccount.|  
|[Sync-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|Sincroniza una base de datos especificada en la instancia especificada del servidor de Analysis Services a todas las instancias de escalado horizontal de consultas en la sesión iniciada actualmente entorno tal como se especifica en el comando Add-AzAnalysisServicesAccount|  

## <a name="database-operations"></a>Operaciones de la base de datos

Las operaciones de la base de datos de Azure Analysis Services utilizan el mismo [módulo SqlServer](https://www.powershellgallery.com/packages/SqlServer) que SQL Server Analysis Services. Pero no todos los cmdlets son compatibles con Azure Analysis Services. Para más información, consulte [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell).

El módulo SqlServer proporciona cmdlets de administración de base de datos específicos de la tarea, así como el cmdlet de uso general Invoke-ASCmd que acepta una consulta o script de Tabular Model Scripting Language (TMSL). Los siguientes cmdlets del módulo SqlServer son compatibles con Azure Analysis Services.

  
|Cmdlet|DESCRIPCIÓN|
|------------|-----------------| 
|[Add-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|Agrega un miembro a un rol de base de datos.| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|Realiza una copia de seguridad de una base de datos de Analysis Services.|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|Quita un miembro de un rol de base de datos.|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|Ejecuta un script de TMSL.|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|Procesa una base de datos.|  
|[Invoke-ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|Procesa una partición.| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|Procesa una tabla.|  
|[Merge-Partition](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|Combina una partición.|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|Restaura una base de datos de Analysis Services.| 
  

## <a name="related-information"></a>Información relacionada

* [Descarga del módulo de PowerShell de SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Descarga de SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [Módulo de SqlServer en Galería de PowerShell](https://www.powershellgallery.com/packages/SqlServer)    
* [Programación de modelo tabular para el nivel de compatibilidad 1200 y superior](https://msdn.microsoft.com/library/mt712541.aspx)
