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
ms.date: 01/25/2019
ms.openlocfilehash: bd3a82f326cdf7f51e8842e45333ff2bd647c260
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58092759"
---
# <a name="powershell-modules-for-azure-machine-learning-studio"></a>Módulos de PowerShell para Azure Machine Learning Studio

Con los módulos de PowerShell, puede administrar mediante programación los recursos de Studio, como áreas de trabajo, conjuntos de datos y servicios web.

Puede interactuar con recursos de Studio mediante tres módulos de PowerShell:

* [Azure PowerShell Az](#az-rm), lanzado en 2018, que incluye toda la funcionalidad de AzureRM, aunque con distintos nombres de cmdlets
* [AzureRM](#az-rm), lanzado en 2016
* [PowerShell clásico para Azure Machine Learning](#classic), lanzado en 2016

Aunque estos módulos presentan algunas similitudes, cada uno de ellos está diseñado para escenarios determinados. En este artículo se describen las diferencias entre los módulos de PowerShell y se ofrece orientación para decidir cuál elegir.

## <a name="choosing-modules"></a> Elección de los módulos

Elegir entre los módulos de PowerShell disponibles depende del tipo de recursos que se administren.

Consulte la [tabla de compatibilidad](#support-table) siguiente para comprobar qué recursos son compatibles con cada módulo. Como PowerShell clásico se puede instalar en paralelo con Az o AzureRM, puede instalar dos módulos y cubrir todos los tipos de recursos (implementación clásica con Az o implementación clásica con AzureRM).

Sin embargo, no se recomienda tener Az y AzureRM instalados al mismo tiempo. Para decidir entre Az y AzureRM, Microsoft recomienda Az para todas las implementaciones futuras. Use AzureRM solo si existen circunstancias especiales en su entorno que lo requieran.

Para más información sobre las diferencias entre Az y AzureRM, así como la ruta de migración proporcionada, vea nuestra [introducción a Azure PowerShell Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

## <a name="az-rm"></a> Azure PowerShell Az y AzureRM

Tanto Az como AzureRM administran soluciones implementadas con el modelo de implementación de **Azure Resource Manager**. Estos recursos incluyen áreas de trabajo de Studio y nuevos servicios web de Studio. Para administrar los recursos implementados mediante el modelo de implementación clásica, debe usar el módulo PowerShell clásico. Si desea obtener más información sobre los modelos de implementación, vea el artículo [Implementación mediante Azure Resource Manager frente a la implementación clásica](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

Az ahora es el módulo deseado de PowerShell para interactuar con Azure e incluye toda la funcionalidad anterior de AzureRM. AzureRM seguirá recibiendo las correcciones de errores, pero no recibirá ninguna característica o cmdlets nuevos. Aunque hay una ruta de actualización de AzureRM, si tiene problemas con Az al trabajar con Studio, informe del problema y vuelva a usar AzureRM.

Para empezar a trabajar con Az, siga las [instrucciones de instalación de Azure Az](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="classic"></a> PowerShell clásico

El [módulo PowerShell clásico](https://aka.ms/amlps) para Studio permite administrar los recursos implementados con el **modelo de implementación clásica**. Estos recursos incluyen recursos de usuario, servicios web clásicos y puntos de conexión de servicios web clásicos de Studio.

Sin embargo, Microsoft recomienda usar el modelo de implementación de Resource Manager para todos los recursos nuevos a fin de simplificar la implementación y administración de recursos. Si desea obtener más información sobre los modelos de implementación, vea el artículo [Implementación mediante Azure Resource Manager frente a la implementación clásica](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

Para empezar a utilizar PowerShell clásico, descargue el [paquete de versión](https://github.com/hning86/azuremlps/releases) de GitHub y siga las [instrucciones de instalación](https://github.com/hning86/azuremlps/blob/master/README.md). Estas instrucciones explican cómo desbloquear el archivo DLL descargado y descomprimido y, después, importarlo en el entorno de PowerShell.

## <a name="support-table"></a> Tabla de compatibilidad con PowerShell

 **Áreas de trabajo de Studio** | **Az** |  **AzureRM** | **PowerShell clásico** |
| --- | --- | --- | --- |
| Crear o eliminar áreas de trabajo | [Plantillas de Resource Manager](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) | [Plantillas de Resource Manager](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) |  |
| Administrar usuarios del área de trabajo |  |  | [Add-AmlWorkspaceUsers](https://github.com/hning86/azuremlps#add-amlworkspaceusers)|
| Administrar planes de compromiso | [New-AzMlCommitmentPlan](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlcommitmentplan) | [New-AzureRmMlCommitmentPlan](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/new-azurermmlcommitmentplan) |
|||
| **Servicios web** | **Az** | **AzureRM** | **PowerShell clásico** |
| Administración de servicios web | [New-AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlwebservice) <br> (Nuevos servicios web) | [New-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/new-azurermmlwebservice) <br> (Nuevos servicios web) |[New-AmlWebService](https://github.com/hning86/azuremlps#manage-classic-web-service) <br> (servicios web clásicos) |
| Administrar puntos de conexión o claves |  [Get-AzMlWebServiceKeys](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservicekeys) <br> (Nuevos servicios web) | [Get-AzureRmMlWebServiceKeys](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservicekeys) <br> (Nuevos servicios web) | [Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#manage-classic-web-servcie-endpoint) <br> (servicios web clásicos) |
|||
| **Recursos de usuario** | **Az** | **AzureRM** | **PowerShell clásico** |
| Administrar conjuntos de datos o modelos entrenados |  |  | [Get-AmlDataset](https://github.com/hning86/azuremlps#manage-user-assets-dataset-trained-model-transform) |
| Administrar experimentos |  |  | [Start-AmlExperiment](https://github.com/hning86/azuremlps#manage-experiment) |
| Administrar módulos personalizados |  |  | [New-AmlCustomModule](https://github.com/hning86/azuremlps#manage-custom-module) |


## <a name="next-steps"></a>Pasos siguientes
Siga estos vínculos para consultar la documentación completa de los módulos de PowerShell:
* [AzureRM](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/#machine_learning)
* [PowerShell clásico](https://aka.ms/amlps)
* [Azure PowerShell Az](https://docs.microsoft.com/powershell/module/az.machinelearning/#machine_learning)
