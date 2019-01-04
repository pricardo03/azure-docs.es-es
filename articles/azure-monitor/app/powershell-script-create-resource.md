---
title: Script de PowerShell para crear un recurso de Application Insights | Microsoft Docs
description: Automatice la creación de recursos de Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/19/2016
ms.author: mbullwin
ms.openlocfilehash: 3fe6a89073da731332a91ece40a6ea6f667d150a
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "54005168"
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a>Script de PowerShell para crear un recurso de Application Insights


Cuando desee supervisar una nueva aplicación —o una nueva versión de una aplicación— con [Application Insights para Visual Studio](https://azure.microsoft.com/services/application-insights/), configure un recurso nuevo en Microsoft Azure. Este recurso es donde se analizan y muestran los datos de telemetría procedentes de su aplicación. 

Puede automatizar la creación de un nuevo recurso mediante PowerShell.

Por ejemplo, si está desarrollando una aplicación de dispositivo móvil, es probable que, en cualquier momento, haya varias versiones publicadas de la aplicación en uso por los clientes. No querrá obtener los resultados de telemetría mezclados de las diferentes versiones. Por tanto, hará que el proceso de compilación cree un nuevo recurso para cada compilación.

> [!NOTE]
> Si desea crear un conjunto de recursos todos al mismo tiempo, considere la posibilidad de [crear los recursos mediante una plantilla de Azure](powershell.md).
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a>Script para crear un recurso en Application Insights
Consulte las especificaciones de cmdlet pertinentes:

* [New-AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*Script de PowerShell*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Connect-AzureRmAccount / Connect-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

# Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Qué hacer con el valor iKey
Cada recurso se identifica por su clave de instrumentación (iKey). El valor iKey es un resultado del script de creación de recursos. El script de compilación debe proporcionar el valor iKey al SDK de Application Insights insertado en la aplicación.

Hay dos maneras de hacer que el valor iKey esté disponible para el SDK:

* En [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* O bien en el [código de inicialización](../../azure-monitor/app/api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Otras referencias
* [Crear Application Insights y recursos de pruebas web a partir de plantillas](powershell.md)
* [Configurar la supervisión de diagnósticos de Azure con PowerShell](powershell-azure-diagnostics.md) 
* [Establecimiento de alertas mediante PowerShell](powershell-alerts.md)

