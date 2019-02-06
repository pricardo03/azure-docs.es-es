---
title: 'App Service en Azure Stack: actualización de dominios de error | Microsoft Docs'
description: Redistribución de Azure App Service en Azure Stack entre dominios de error
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: anwestg
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: 249e4b466e9be567111aaa22b40ca3e5dadb6cac
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246388"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Redistribución de Azure App Service en Azure Stack entre dominios de error

*Se aplica a: Sistemas integrados de Azure Stack*

Con la actualización 1802, Azure Stack ahora admite la distribución de cargas de trabajo entre dominios de error, una característica que es crítica para la alta disponibilidad.

>[!IMPORTANT]
>Para poder beneficiarse de la compatibilidad con dominios de error, debe haber actualizado el sistema integrado en Azure Stack a la versión 1802. Este documento solo se aplica a implementaciones de proveedor de recursos de App Service finalizadas antes de la actualización 1802. Si ha implementado App Service en Azure Stack después de aplicar la actualización 1802 a Azure Stack, el proveedor de recursos ya estará distribuido entre dominios de error.

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Reequilibrio de un proveedor de recursos de App Service entre dominios de error

Para redistribuir los conjuntos de escalado implementados para el proveedor de recursos de App Service, debe realizar los pasos de este artículo para cada conjunto de escalado. De forma predeterminada, los nombres de los conjuntos de escalado son:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

>[!NOTE]
> Si no ha implementado ninguna instancia en algunos de los conjuntos de escalado a nivel de trabajo, no necesita reequilibrarlos. Los conjuntos de escalado se reequilibrarán correctamente al escalarlos horizontalmente en el futuro.

Para escalar horizontalmente los conjuntos de escalado, siga estos pasos:

1. Inicie sesión en el Portal de administración de Azure Stack.
1. Seleccione **Todos los servicios**.
2. En la categoría **COMPUTE**, seleccione **Conjuntos de escalado de máquinas virtuales**. Los conjuntos de escalado implementados como parte de la implementación de App Service se enumerarán con la información del recuento de instancias. La captura de pantalla siguiente muestra un ejemplo de conjunto de escalado.

      ![Azure App Service Scale Sets enumerados en la experiencia de usuario con Virtual Machine Scale Sets][1]

1. Escale cada conjunto horizontalmente. Por ejemplo, si tiene tres instancias existentes en el conjunto de escalado, debe escalarlas horizontalmente a seis, para que las tres instancias nuevas se implementen en los dominios de error. En el siguiente ejemplo de PowerShell se muestra el escalamiento horizontal del conjunto de escalado.

   ```powershell
   Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

   # Get current scale set
   $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

   # Set and update the capacity of your scale set
   $vmss.sku.capacity = 6
   Update-AzureRmVmss -ResourceGroupName AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
   ```

   >[!NOTE]
   >Este paso puede tardar algunas horas en finalizar, según el tipo de rol y del número de instancias.

1. En **Roles de administración de App Service**, supervise el estado de las nuevas instancias de rol. Para comprobar el estado de una instancia de rol individual, seleccione el tipo de rol en la lista.

    ![Azure App Service en roles de Azure Stack][2]

1. Cuando el estado de las nuevas instancias de rol sea **Listo**, vuelva a **Conjunto de escalado de máquinas virtuales** y **elimine** las instancias de rol antiguas.

1. Repita estos pasos para **cada** conjunto de escalado de máquinas virtuales.

## <a name="next-steps"></a>Pasos siguientes

También puede probar otros [servicios de Plataforma como servicio (PaaS)](azure-stack-tools-paas-services.md).

* [Proveedor de recursos de SQL Server](azure-stack-sql-resource-provider-deploy.md)
* [Proveedor de recursos de MySQL](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
