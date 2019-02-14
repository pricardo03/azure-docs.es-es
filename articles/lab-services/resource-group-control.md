---
title: Especificación del grupo de recursos para VM en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo especificar un grupo de recursos para VM en un laboratorio en Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2019
ms.author: spelluru
ms.openlocfilehash: ddda9ef2b9bb716f7cdd33aa8fe9233f6c7d8e82
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55749007"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Especificación de un grupo de recursos para máquinas virtuales de laboratorio en Azure DevTest Labs
Como propietario de un laboratorio, puede configurar las máquinas virtuales del laboratorio para que se creen en un grupo de recursos específico. Use esta característica para no alcanzar los límites del grupo de recursos en su suscripción de Azure. Esta característica también le permite consolidar todos los recursos del laboratorio dentro de un único grupo de recursos. También simplifica el seguimiento de esos recursos y la aplicación de [directivas](../governance/policy/overview.md) para administrarlos a nivel del grupo de recursos.

Con esta característica, puede usar un script para especificar un grupo de recursos nuevo o existente dentro de su suscripción de Azure para todas las máquinas virtuales del laboratorio. Actualmente, DevTest Labs admite esta característica a través de una API. 

## <a name="api-to-configure-a-resource-group-for-labs-vms"></a>API para configurar un grupo de recursos para VM laboratorios
Ahora vamos a examinar las opciones que tiene como propietario de un laboratorio mientras usa esta API: 

- Puede elegir el **grupo de recursos del laboratorio** para todas las máquinas virtuales.
- Puede elegir un **grupo de recursos existente** distinto del grupo de recursos del laboratorio para todas las máquinas virtuales.
- Puede escribir un nombre del **nuevo grupo de recursos** para todas las máquinas virtuales.
- Puede continuar con el comportamiento existente, es decir, se crea un grupo de recursos para cada VM en el laboratorio.
 
Esta configuración se aplica a nuevas máquinas virtuales creadas en el laboratorio. Las VM antiguas en el laboratorio creadas en sus propios grupos de recursos seguirán sin verse afectadas. Sin embargo, puede migrar estas máquinas virtuales de sus grupos de recursos individuales al grupo de recursos común, de modo que todas las máquinas virtuales del laboratorio se encuentren en un grupo de recursos común. Para obtener más información, consulte [Traslado de los recursos a un nuevo grupo de recursos](../azure-resource-manager/resource-group-move-resources.md). Los entornos creados en su laboratorio se mantienen en sus propios grupos de recursos.

### <a name="how-to-use-this-api"></a>Cómo usar esta API:
- Use la versión **2018_10_15_preview** al usar esta API. 
- Si especifica un nuevo grupo de recursos, asegúrese de tener **permisos de escritura en grupos de recursos** dentro de su suscripción. Sin permisos de escritura, se genera un error al crear nuevas máquinas virtuales en el grupo de recursos especificado. 
- Al usar la API, pase el **id. del grupo de recursos completo**. Por ejemplo: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Asegúrese de que el grupo de recursos esté en la misma suscripción que el laboratorio. 

## <a name="use-powershell"></a>Uso de PowerShell 
El ejemplo siguiente se describe cómo crear todas las máquinas virtuales de laboratorio en un nuevo grupo de recursos mediante un script de PowerShell.

```PowerShell
[CmdletBinding()]
Param(
    $subId,
    $labRg,
    $labName,
    $vmRg
)

az login | out-null

az account set --subscription $subId | out-null

$rgId = "/subscriptions/"+$subId+"/resourceGroups/"+$vmRg

"Updating lab '$labName' with vm rg '$rgId'..."

az resource update -g $labRg -n $labName --resource-type "Microsoft.DevTestLab/labs" --api-version 2018-10-15-preview --set properties.vmCreationResourceGroupId=$rgId

"Done. New virtual machines will now be created in the resource group '$vmRg'."
```

Invoque el script mediante el comando siguiente (ResourceGroup.ps1 es el archivo que contiene el script anterior): 

```PowerShell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-azure-resource-manager-template"></a>Usar plantillas de Azure Resource Manager
Si usas la plantilla de Azure Resource Manager para crear un laboratorio, use la propiedad **vmCreationResourceGroupId** en la sección de propiedades del laboratorio de la plantilla de Resource Manager, como se muestra en el ejemplo siguiente:

```json
        {
            "type": "microsoft.devtestlab/labs",
            "name": "[parameters('lab_name')]",
            "apiVersion": "2018_10_15_preview",
            "location": "eastus",
            "tags": {},
            "scale": null,
            "properties": {
                "vmCreationResourceGroupId": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>",
                "labStorageType": "Premium",
                "premiumDataDisks": "Disabled",
                "provisioningState": "Succeeded",
                "uniqueIdentifier": "6e6f668f-992b-435c-bac3-d328b745cd25"
            },
            "dependsOn": []
        },
```


## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes: 

- [Set policies for a lab](devtest-lab-get-started-with-lab-policies.md) (Definición de directivas para un laboratorio)
- [Preguntas más frecuentes](devtest-lab-faq.md)
