---
title: Creación de un área de trabajo de Log Analytics mediante Azure PowerShell | Microsoft Docs
description: Aprenda a crear un área de trabajo de Log Analytics para habilitar soluciones de administración y recopilación de datos en sus entornos tanto locales como en la nube con Azure PowerShell.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptal
ms.date: 09/18/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 9abd46bf75e2a0113f44243d7c1695d96f9c1057
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220660"
---
# <a name="create-a-log-analytics-workspace-with-azure-powershell"></a>Creación de un área de trabajo de Log Analytics con Azure PowerShell

El módulo de Azure PowerShell se usa para crear y administrar recursos de Azure desde la línea de comandos de PowerShell o en scripts. En esta guía de inicio rápido se muestra cómo usar el módulo de Azure PowerShell para implementar un área de trabajo de Log Analytics en Azure, que constituye un entorno único con su propio repositorio de datos, orígenes de datos y soluciones.  Los pasos que se describen en este artículo son necesarios si se tiene intención de recopilar datos de los siguientes orígenes:

* Recursos de Azure de la suscripción  
* Equipos locales supervisados por System Center Operations Manager  
* Colecciones de dispositivos de System Center Configuration Manager  
* Datos de diagnóstico o registro de Azure Storage  
 
Para otros orígenes, como las máquinas virtuales de Azure y la máquinas virtuales Windows o Linux del entorno, consulte los temas siguientes:

* [Recopilación de datos de máquinas virtuales de Azure](log-analytics-quick-collect-azurevm.md)
* [Recopilación de datos de un equipo Linux híbrido](log-analytics-quick-collect-linux-computer.md)
* [Recopilación de datos de un equipo Windows híbrido](log-analytics-quick-collect-windows-computer.md)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar PowerShell localmente, para este tutorial se requiere la versión 5.7.0 del módulo de Azure PowerShell o cualquier versión posterior. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.

## <a name="create-a-workspace"></a>Crear un área de trabajo
Cree un área de trabajo con [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment). En el ejemplo siguiente se creará un área de trabajo denominada *TestWorkspace* en el grupo de recursos *Lab* en la ubicación *Este de EE. UU.* mediante una plantilla de Resource Manager desde la máquina local. La plantilla JSON está configurada para solicitar solo el nombre del área de trabajo y especifica un valor predeterminado para los restantes parámetros que es probable que se utilice como configuración estándar en su entorno. 

Los siguientes parámetros establecen un valor predeterminado:

* Ubicación: el valor predeterminado es Este de EE. UU.
* SKU: el valor predeterminado es el nuevo plan de tarifa por GB publicado en el modelo de precios de abril de 2018

>[!WARNING]
>Si se crea o configura un área de trabajo de Log Analytics en una suscripción que ha elegido el nuevo modelo de precios de abril de 2018, el único plan de tarifa válido de Log Analytics es **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Creación e implementación de una plantilla

1. Copie y pegue la siguiente sintaxis JSON en el archivo:

    ```json
    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. Edite la plantilla para adecuarla a sus requisitos.  Consulte la referencia [Plantilla Microsoft.OperationalInsights/workspaces](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) para saber qué propiedades y valores son compatibles. 
3. Guarde este archivo como **deploylaworkspacetemplate.json** en una carpeta local.   
4. Está listo para implementar esta plantilla. Use los siguientes comandos desde la carpeta que contiene la plantilla:

    ```powershell
        New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json
    ```

La implementación puede demorar unos minutos en completarse. Cuando termine, verá un mensaje similar al siguiente que incluye el resultado:

![Resultado de ejemplo cuando se completa la implementación](./media/log-analytics-template-workspace-configuration/template-output-01.png)

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene un área de trabajo disponible, puede configurar la recopilación de datos de telemetría de supervisión, realizar búsquedas en el registro para analizar dichos datos y agregar una solución de administración que proporcione datos adicionales e información de los análisis.  

* Para habilitar la recopilación de datos de recursos de Azure con Azure Diagnostics o Azure Storage, consulte [Recopilación de registros y métricas de Azure para servicios de Log Analytics](log-analytics-azure-storage.md).  
* [Agregue System Center Operations Manager como origen de datos](log-analytics-om-agents.md) para recopilar datos de agentes que informan a su grupo de administración de Operations Manager y almacenarlos en el repositorio del área de trabajo de Log Analytics.  
* Conecte [Configuration Manager](log-analytics-sccm.md) para importar equipos que son miembros de colecciones en la jerarquía.  
* Revise las [soluciones de administración](log-analytics-add-solutions.md) disponibles y cómo agregar o quitar una solución del área de trabajo.