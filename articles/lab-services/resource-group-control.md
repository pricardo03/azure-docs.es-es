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
ms.date: 03/07/2019
ms.author: spelluru
ms.openlocfilehash: 1001e6aec7ba2f6ce62eb267d218149296048bb9
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485890"
---
# <a name="specify-a-resource-group-for-lab-virtual-machines-in-azure-devtest-labs"></a>Especificación de un grupo de recursos para máquinas virtuales de laboratorio en Azure DevTest Labs

Como propietario de un laboratorio, puede configurar las máquinas virtuales del laboratorio para que se creen en un grupo de recursos específico. Esta característica sirve de ayuda en los siguientes escenarios:

- Hay menos grupos de recursos creados por los laboratorios en la suscripción.
- Tiene los laboratorios se ejecutan dentro de un conjunto fijo de grupos de recursos que configure.
- Se evitan las restricciones y las aprobaciones necesarias para crear grupos de recursos dentro de la suscripción de Azure.
- Consolidar todos los recursos de laboratorio dentro de un grupo de recursos único para simplificar el seguimiento de esos recursos y aplicar [directivas](../governance/policy/overview.md) para administrar recursos en el nivel de grupo de recursos.

Con esta característica, puede usar una secuencia de comandos para especificar un grupo de recursos nuevo o existente en su suscripción de Azure para el laboratorio máquinas virtuales. Actualmente, Azure DevTest Labs admite esta característica a través de una API.

## <a name="use-azure-portal"></a>Usar Azure Portal
Siga estos pasos para especificar un grupo de recursos para todas las máquinas virtuales creadas en el laboratorio. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **todos los servicios** en el menú de navegación izquierdo. 
3. Seleccione **DevTest Labs** en la lista.
4. En la lista de laboratorios, seleccione su **laboratorio**.  
5. Seleccione **configuración y directivas** en el **configuración** sección en el menú izquierdo. 
6. Seleccione **configuración de laboratorio** en el menú izquierdo. 
7. Seleccione **todas las máquinas virtuales en un grupo de recursos**. 
8. Seleccione un grupo de recursos existente en la lista desplegable lista (o) seleccione **crear nuevo**, escriba un **nombre** para el grupo de recursos y seleccione **Aceptar**. 

    ![Seleccione el grupo de recursos para todas las máquinas virtuales de laboratorio](./media/resource-group-control/select-resource-group.png)

## <a name="use-powershell"></a>Uso de PowerShell 
El ejemplo siguiente muestra cómo usar un script de PowerShell para crear todas las máquinas virtuales de laboratorio en un grupo de recursos.

```powershell
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

Invocar el script mediante el comando siguiente. ResourceGroup.ps1 es el archivo que contiene el script anterior:

```powershell
.\ResourceGroup.ps1 -subId <subscriptionID> -labRg <labRGNAme> -labName <LanName> -vmRg <RGName> 
```

## <a name="use-an-azure-resource-manager-template"></a>Usar una plantilla de Azure Resource Manager
Si usa una plantilla de Azure Resource Manager para crear un laboratorio, use el **vmCreationResourceGroupId** propiedad en la sección de propiedades de laboratorio de la plantilla, tal como se muestra en el ejemplo siguiente:

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
                "uniqueIdentifier": "000000000f-0000-0000-0000-00000000000000"
            },
            "dependsOn": []
        },
```


## <a name="api-to-configure-a-resource-group-for-lab-vms"></a>API para configurar un grupo de recursos para máquinas virtuales de laboratorio
Tiene las siguientes opciones como un propietario de laboratorio cuando se usa esta API:

- Elija la **grupo de recursos del laboratorio** para todas las máquinas virtuales.
- Elija un **grupo de recursos existente** que no sea el grupo de recursos del laboratorio para todas las máquinas virtuales.
- Escriba un **nuevo grupo de recursos** para todas las máquinas virtuales.
- Continuar usando el comportamiento existente, en el que se crea un grupo de recursos para cada máquina virtual en el laboratorio.
 
Esta configuración se aplica a nuevas máquinas virtuales creadas en el laboratorio. La VM en el laboratorio más antiguas que se crearon en sus propios grupos de recursos no se ven afectadas. Los entornos que se crean en su laboratorio continúan en sus propios grupos de recursos.

Cómo usar esta API:
- Versión de API de uso **2018_10_15_preview**.
- Si especifica un nuevo grupo de recursos, asegúrese de que tiene **permisos de escritura en grupos de recursos** en su suscripción. Si no tiene permisos de escritura, crear nuevas máquinas virtuales en el grupo de recursos especificado generará un error.
- Al usar la API, pase el **id. del grupo de recursos completo**. Por ejemplo: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>`. Asegúrese de que el grupo de recursos está en la misma suscripción que el laboratorio. 


## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes: 

- [Set policies for a lab](devtest-lab-get-started-with-lab-policies.md) (Definición de directivas para un laboratorio)
- [Preguntas más frecuentes](devtest-lab-faq.md)
