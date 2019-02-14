---
title: 'Hospedaje de alta densidad con escalado por aplicación: Azure App Service | Microsoft Docs'
description: Hospedaje de alta densidad en Azure App Service
author: btardif
manager: erikre
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/22/2018
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: 08d6d0c31e1cff799e952c50bae3446e41477aba
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104576"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>Hospedaje de alta densidad en Azure App Service con escalado por aplicación

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De forma predeterminada, para escalar las aplicaciones de App Service, se escala [plan de App Service](overview-hosting-plans.md) en el que se ejecutan. Cuando se ejecutan varias aplicaciones en el mismo plan de App Service, cada instancia escalada horizontalmente ejecuta todas las aplicaciones del plan.

Puede habilitar el *escalado por aplicación* en el nivel del plan de App Service. Escala una aplicación de forma independiente del plan de App Service que lo hospeda. De esta manera, se puede escalar un plan de App Service a 10 instancias, pero se puede configurar una aplicación para usar solo cinco.

> [!NOTE]
> El escalado por aplicación solo está disponible para los planes de tarifa **Estándar**, **Premium**, **Premium V2** y **Aislado**.
>

## <a name="per-app-scaling-using-powershell"></a>Escalado por aplicación mediante PowerShell

Cree un plan con escalado por aplicación; para ello, pase el parámetro ```-PerSiteScaling $true``` al cmdlet ```New-AzAppServicePlan```.

```powershell
New-AzAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Habilite el escalado por aplicación con un plan de App Service existente; para ello, pase el parámetro `-PerSiteScaling $true` al cmdlet ```Set-AzAppServicePlan```.

```powershell
# Enable per-app scaling for the App Service Plan using the "PerSiteScaling" parameter.
Set-AzAppServicePlan -ResourceGroupName $ResourceGroup `
   -Name $AppServicePlan -PerSiteScaling $true
```

En el nivel de aplicación, configure el número de instancias que puede usar la aplicación en el plan de App Service.

En el ejemplo siguiente, la aplicación está limitada a dos instancias, independientemente de la cantidad de estas a las que se escale horizontalmente el plan de App Service subyacente.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` es diferente de `$newapp.MaxNumberOfWorkers`. En el escalado por aplicación se usa `$newapp.SiteConfig.NumberOfWorkers` para determinar las características de escalado de la aplicación.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Escalado por aplicación mediante Azure Resource Manager

La siguiente plantilla de Azure Resource Manager crea:

- Un plan de App Service que se escala horizontalmente a 10 instancias.
- Una aplicación que está configurada para escalarse hasta un máximo de 5 instancias.

El plan de App Service es establecer la propiedad **PerSiteScaling** en true `"perSiteScaling": true`. La aplicación configura el **número de trabajadores** que se va a usar en 5 `"properties": { "numberOfWorkers": "5" }`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Configuración recomendada para el hospedaje de alta densidad
El escalado por aplicación es una característica que está habilitada tanto en las regiones de Azure globales como en los entornos de [App Service Environment](environment/app-service-app-service-environment-intro.md). Sin embargo, la estrategia recomendada es usar entornos de App Service para aprovechar sus características avanzadas y los grupos de mayor capacidad.  

Siga estos pasos para configurar el hospedaje de alta densidad para las aplicaciones:

1. Configure el entorno del App Service y elija un grupo de trabajo dedicado al escenario de hospedaje de alta densidad.
2. Cree un único plan de App Service y escálelo para que utilice toda la capacidad disponible en el grupo de trabajo.
3. Establezca la marca `PerSiteScaling` en true en el plan de App Service.
4. Las nuevas aplicaciones se crean y se asignan al plan de App Service con la propiedad **numberOfWorkers** establecida en **1**. El uso de esta configuración produciría la máxima densidad posible en este grupo de trabajo.
5. El número de trabajadores se puede configurar por separado por cada aplicación para conceder recursos adicionales según sea necesario. Por ejemplo: 
    - En una aplicación de uso elevado, puede establecer **numberOfWorkers** en **3** para que haya una mayor capacidad de procesamiento para esa aplicación. 
    - En las aplicaciones de poco uso se establecerá **numberOfWorkers** en **1**.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción detallada sobre los planes de Azure App Service](overview-hosting-plans.md)
- [Introducción al entorno de App Service](environment/app-service-app-service-environment-intro.md)
