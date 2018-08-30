---
title: Administración de Azure Analysis Services con PowerShell | Microsoft Docs
description: Administración de Azure Analysis Services con PowerShell.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 06/25/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6c648a9cb6b8d8dbfb60f1a5a6ebc386c57460b0
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887253"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Administración de Azure Analysis Services con PowerShell

En este artículo se describen los cmdlets de PowerShell que se usan para realizar tareas de administración de bases de datos y del servidor de Azure Analysis Services. 

Las tareas de administración del servidor como crear o eliminar un servidor, suspender o reanudar las operaciones del servidor o cambiar el nivel de servicio, usan cmdlets de Azure Resource Manager (recurso) y de Analysis Services (servidor). Las demás tareas para administrar las bases de datos, como agregar o quitar miembros de rol, procesar o crear particiones, usan los cmdlets que se incluyen en el mismo módulo de SqlServer como SQL Server Analysis Services.

## <a name="permissions"></a>Permisos
Para la mayoría de las tareas de PowerShell, es necesario disponer de privilegios de administración en el servidor de Analysis Services que esté administrando. Las tareas programadas de PowerShell son operaciones desatendidas. La cuenta o el principio de servicio que ejecuta Scheduler debe tener privilegios de administrador en el servidor de Analysis Services. 

Para las operaciones del servidor mediante cmdlets de AzureRm, su cuenta o la cuenta que ejecuta Scheduler también debe pertenecer al rol de propietario para el recurso en [Control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/overview.md). 

## <a name="resource-management-operations"></a>Operaciones de administración de recursos 
Módulo: [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices)

|Cmdlet|DESCRIPCIÓN| 
|------------|-----------------| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Obtiene los detalles de una instancia del servidor.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Crea una instancia de servidor.|   
|[New-AzureRmAnalysisServicesFirewallConfig](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallconfig)|Crea una configuración de firewall de Analysis Services.|   
|[New-AzureRmAnalysisServicesFirewallRule](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesfirewallrule)|Crea una regla de firewall de Analysis Services.|   
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Quita una instancia de servidor.|  
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Reanuda una instancia de servidor.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Suspende una instancia de servidor.| 
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Modifica una instancia de servidor.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Comprueba la existencia de una instancia del servidor.| 

## <a name="server-management-operations"></a>Operaciones de administración de servidor

Módulo: [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|DESCRIPCIÓN| 
|------------|-----------------| 
|[Add-AzureAnalysisServicesAccount](/powershell/module/azure.analysisservices/add-azureanalysisservicesaccount)|Agrega una cuenta autenticada para utilizarla con las solicitudes de cmdlet de servidor de Azure Analysis Services.| 
|[Export-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Exporta un registro a partir de una instancia de servidor de Analysis Services en el entorno actualmente conectado, tal como se especifica en el comando Add-AzureAnalysisServicesAccount|  
|[Restart-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Reinicia una instancia de servidor de Analysis Services en el entorno actualmente conectado; se especifica en el comando Add-AzureAnalysisServicesAccount.|  
|[Sync-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Sincroniza una base de datos especificada en la instancia especificada del servidor de Analysis Services para todas las instancias de escalado horizontal de consultas en el entorno actualmente conectado, tal como se especifica en el comando Add-AzureAnalysisServicesAccount|  

## <a name="database-operations"></a>Operaciones de la base de datos

Las operaciones de la base de datos de Azure Analysis Services utilizan el mismo [módulo SqlServer](https://www.powershellgallery.com/packages/SqlServer) que SQL Server Analysis Services. Pero no todos los cmdlets son compatibles con Azure Analysis Services. Para obtener más información, consulte [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell).

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
