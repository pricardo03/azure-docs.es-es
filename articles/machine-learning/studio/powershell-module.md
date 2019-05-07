---
title: Módulos de PowerShell para Machine Learning Studio
titleSuffix: Azure Machine Learning Studio
description: Use PowerShell para crear y administrar áreas de trabajo de Azure Machine Learning Studio, experimentos, servicios web y mucho más.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.date: 04/25/2019
ms.openlocfilehash: bee42f8a9582908963c0eef95a2fd04742cd425e
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205660"
---
# <a name="powershell-modules-for-azure-machine-learning-studio"></a>Módulos de PowerShell para Azure Machine Learning Studio

Con los módulos de PowerShell, puede administrar mediante programación los recursos de Studio, como áreas de trabajo, conjuntos de datos y servicios web.

Puede interactuar con recursos de Studio mediante tres módulos de PowerShell:

* [Azure PowerShell Az](#az-rm), lanzado en 2018, que incluye toda la funcionalidad de AzureRM, aunque con distintos nombres de cmdlets
* [AzureRM](#az-rm) publicado en 2016, se sustituye por PowerShell Az
* [PowerShell clásico para Azure Machine Learning](#classic), lanzado en 2016

Aunque estos módulos de PowerShell tienen algunas similitudes, cada uno está diseñado para escenarios concretos. En este artículo se describen las diferencias entre los módulos de PowerShell y se ofrece orientación para decidir cuál elegir.  

Consulte la [tabla de compatibilidad](#support-table) siguiente para comprobar qué recursos son compatibles con cada módulo. 

## <a name="az-rm"></a> Azure PowerShell Az y AzureRM

Az ahora es el módulo deseado de PowerShell para interactuar con Azure e incluye toda la funcionalidad anterior de AzureRM. AzureRM seguirá recibiendo las correcciones de errores, pero no recibirá ninguna característica o cmdlets nuevos.  Tanto Az como AzureRM administran soluciones implementadas con el modelo de implementación de **Azure Resource Manager**. Estos recursos incluyen áreas de trabajo de Studio y servicios web "Nueva" de Studio. 

PowerShell clásico se puede instalar junto con Az o AzureRM para cubrir ambos tipos de recursos "nuevo" y "clásico". Sin embargo, no se recomienda tener Az y AzureRM instalados al mismo tiempo. Para decidir entre Az y AzureRM, Microsoft recomienda Az para todas las implementaciones futuras.  Obtener más información acerca de Az frente a AzureRM y la ruta de migración en [Introducción a Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

Para empezar a trabajar con Az, siga las [instrucciones de instalación de Azure Az](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="classic"></a> PowerShell clásico

El [módulo PowerShell clásico](https://aka.ms/amlps) para Studio permite administrar los recursos implementados con el **modelo de implementación clásica**. Estos recursos incluyen Studio usuario activos, los servicios web "clásico" y extremos de servicio web "clásico".

Sin embargo, Microsoft recomienda que utilice el modelo de implementación de Resource Manager para todos los recursos futuros para simplificar la implementación y administración de recursos. Si desea obtener más información sobre los modelos de implementación, vea el artículo [Implementación mediante Azure Resource Manager frente a la implementación clásica](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

Para empezar a utilizar PowerShell clásico, descargue el [paquete de versión](https://github.com/hning86/azuremlps/releases) de GitHub y siga las [instrucciones de instalación](https://github.com/hning86/azuremlps/blob/master/README.md). Estas instrucciones explican cómo desbloquear el archivo DLL descargado y descomprimido y, después, importarlo en el entorno de PowerShell.

PowerShell clásico se puede instalar junto con Az o AzureRM para cubrir ambos tipos de recursos "nuevo" y "clásico".

## <a name="support-table"></a> Tabla de compatibilidad con PowerShell


| | **Az** |  **PowerShell clásico** |
| --- | --- | --- |
| Crear o eliminar las áreas de trabajo | [Plantillas de Resource Manager](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) |  |
| Administrar planes de compromiso de área de trabajo | [New-AzMlCommitmentPlan](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlcommitmentplan) | |
| Administrar usuarios del área de trabajo |  | [Add-AmlWorkspaceUsers](https://github.com/hning86/azuremlps#add-amlworkspaceusers)|
| Administración de servicios web | [New-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlwebservice) <br>(servicios web "new")|| [New-AmlWebService](https://github.com/hning86/azuremlps#manage-classic-web-service) <br>(servicios web "clásico") |
| Administrar puntos de conexión del servicio web o claves |  [Get-AzMlWebServiceKey](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservicekey)|  [Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#manage-classic-web-servcie-endpoint)|
| Administrar conjuntos de datos y entrenar modelos de usuario| | [Get-AmlDataset](https://github.com/hning86/azuremlps#manage-user-assets-dataset-trained-model-transform) |
| Administrar experimentos de usuario |  | [Start-AmlExperiment](https://github.com/hning86/azuremlps#manage-experiment) |
| Administrar módulos personalizados | | [New-AmlCustomModule](https://github.com/hning86/azuremlps#manage-custom-module) |


## <a name="next-steps"></a>Pasos siguientes
Consulte la documentación completa de estas módulo de PowerShell:
* [PowerShell clásico](https://aka.ms/amlps)
* [Azure PowerShell Az](https://docs.microsoft.com/powershell/module/az.machinelearning/#machine_learning)
