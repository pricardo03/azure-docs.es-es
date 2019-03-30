---
title: Actualización de la configuración de un clúster de Azure Service Fabric | Microsoft Docs
description: Obtenga información acerca de cómo actualizar la configuración que ejecuta un clúster de Service Fabric en Azure con una plantilla de Resource Manager.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 818136f24eb063e2bd7217d5441bda19bf141317
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666603"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Actualización de la configuración de un clúster de Azure 

En este documento se explica cómo personalizar las diversas opciones de configuración para el clúster de Service Fabric. Para clústeres hospedados en Azure, puede personalizar la configuración en [Azure Portal](https://portal.azure.com) o mediante una plantilla de Azure Resource Manager.

> [!NOTE]
> No todas las configuraciones están disponibles en el portal, y es un [recomendado para personalizarlo mediante una plantilla de Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code); Portal es para Service Fabric Dev\Test del escenario solo.
> 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Personalización de la configuración de clústeres mediante plantillas de Resource Manager
Los clústeres de Azure pueden configurarse a través de la plantilla JSON de Resource Manager. Para obtener más información acerca de las distintas opciones de configuración, vea [Configuration settings for clusters](service-fabric-cluster-fabric-settings.md) (Opciones de configuración para clústeres). Como ejemplo, en los pasos siguientes se muestra cómo agregar un nuevo valor *MaxDiskQuotaInMB* a la sección *Diagnostics* con Azure Resource Explorer.

1. Vaya a https://resources.azure.com.
2. Para ir a la suscripción, expanda **subscriptions** -> **\<Su suscripción>** -> **resourceGroups** -> **\<Su grupo de recursos>** -> **providers** -> **Microsoft.ServiceFabric** -> **clusters** -> **\<El nombre del clúster>**
3. En la esquina superior derecha, seleccione **Read/Write** (Lectura y escritura).
4. Seleccione **Edit** (Editar), actualice el elemento JSON `fabricSettings` y agregue un nuevo elemento:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

También puede personalizar la configuración del clúster de una de las siguientes maneras con Azure Resource Manager:

- Use [Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template) para exportar y actualizar la plantilla de Resource Manager.
- Use [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell) para exportar y actualizar la plantilla de Resource Manager.
- Use la [CLI de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli) para exportar y actualizar la plantilla de Resource Manager.
- Use los comandos [Set-AzureRmServiceFabricSetting](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/Set-AzureRmServiceFabricSetting) y [Remove-AzureRmServiceFabricSetting](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/Remove-AzureRmServiceFabricSetting) de Azure RM PowerShell para modificar la configuración directamente.
- Use el comando [az sf cluster setting](https://docs.microsoft.com/cli/azure/sf/cluster/setting) de la CLI de Azure para modificar la configuración directamente.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre la [configuración de un clúster de Service Fabric](service-fabric-cluster-fabric-settings.md).
* Obtenga información sobre cómo [escalar o reducir horizontalmente el clúster](service-fabric-cluster-scale-up-down.md).
