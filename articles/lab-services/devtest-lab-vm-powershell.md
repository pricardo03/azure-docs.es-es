---
title: Crear una máquina virtual de DevTest Labs con Azure PowerShell | Microsoft Docs
description: Aprenda a usar Azure DevTest Labs para crear y administrar máquinas virtuales con Azure PowerShell.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: df6d7e59c6c569a56162a738924b4ffd05b47da6
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896620"
---
# <a name="create-a-virtual-machine-with-devtest-labs-using-azure-powershell"></a>Crear una máquina virtual con DevTest Labs mediante Azure PowerShell
Este artículo muestra cómo crear una máquina virtual en Azure DevTest Labs mediante Azure PowerShell. Puede usar scripts de PowerShell para automatizar la creación de máquinas virtuales en un laboratorio en Azure DevTest Labs. 

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar:

- [Creación de un laboratorio](devtest-lab-create-lab.md) si no desea usar un laboratorio existente para probar la secuencia de comandos o los comandos en este artículo. 
- [Instale Azure PowerShell](/powershell/azure/azurerm/other-install) o usar Azure Cloud Shell que se integra en el portal de Azure. 

## <a name="powershell-script"></a>Script de PowerShell
El script de ejemplo en esta sección utiliza la [Invoke-AzureRmResourceAction](/powershell/module/azurerm.resources/invoke-azurermresourceaction) cmdlet.  Este cmdlet toma el identificador de recurso del laboratorio, nombre de la acción a realizar (`createEnvironment`), y los parámetros necesarios realizan dicha acción. Los parámetros están en una tabla hash que contiene todas las propiedades de descripción de la máquina virtual. 

```powershell
[CmdletBinding()]

Param(
[Parameter(Mandatory = $false)]  $SubscriptionId,
[Parameter(Mandatory = $true)]   $LabResourceGroup,
[Parameter(Mandatory = $true)]   $LabName,
[Parameter(Mandatory = $true)]   $NewVmName,
[Parameter(Mandatory = $true)]   $UserName,
[Parameter(Mandatory = $true)]   $Password
)
 
pushd $PSScriptRoot

try {
    if ($SubscriptionId -eq $null) {
        $SubscriptionId = (Get-AzureRmContext).Subscription.SubscriptionId
    }

    $API_VERSION = '2016-05-15'
    $lab = Get-AzureRmResource -ResourceId "/subscriptions/$SubscriptionId/resourceGroups/$LabResourceGroup/providers/Microsoft.DevTestLab/labs/$LabName"

    if ($lab -eq $null) {
       throw "Unable to find lab $LabName resource group $LabResourceGroup in subscription $SubscriptionId."
    }

    #For this example, we are getting the first allowed subnet in the first virtual network
    #  for the lab.
    #If a specific virtual network is needed use | to find it. 
    #ie $virtualNetwork = @(Get-AzureRmResource -ResourceType  'Microsoft.DevTestLab/labs/virtualnetworks' -ResourceName $LabName -ResourceGroupName $lab.ResourceGroupName -ApiVersion $API_VERSION) | Where-Object Name -EQ "SpecificVNetName"

    $virtualNetwork = @(Get-AzureRmResource -ResourceType  'Microsoft.DevTestLab/labs/virtualnetworks' -ResourceName $LabName -ResourceGroupName $lab.ResourceGroupName -ApiVersion $API_VERSION)[0]

    $labSubnetName = $virtualNetwork.properties.allowedSubnets[0].labSubnetName

    #Prepare all the properties needed for the createEnvironment
    # call used to create the new VM.
    # The properties will be slightly different depending on the base of the vm
    # (a marketplace image, custom image or formula).
    # The setup of the virtual network to be used may also affect the properties.
    
    $parameters = @{
       "name"      = $NewVmName;
       "location"  = $lab.Location;
       "properties" = @{
          "labVirtualNetworkId"     = $virtualNetwork.ResourceId;
          "labSubnetName"           = $labSubnetName;
          "notes"                   = "Windows Server 2016 Datacenter";
          "osType"                  = "windows"
          "galleryImageReference"   = @{
             "offer"     = "WindowsServer";
             "publisher" = "MicrosoftWindowsServer";
             "sku"       = "2016-Datacenter";
             "osType"    = "Windows";
             "version"   = "latest"
          };
          "size"                    = "Standard_DS2_v2";
          "userName"                = $UserName;
          "password"                = $Password;
          "disallowPublicIpAddress" = $true;
       }
    }
    
    #The following line is the same as invoking
    # https://azure.github.io/projects/apis/#!/Labs/Labs_CreateEnvironment rest api

    Invoke-AzureRmResourceAction -ResourceId $lab.ResourceId -Action 'createEnvironment' -Parameters $parameters -ApiVersion $API_VERSION -Force -Verbose
}
finally {
   popd
}
```

Las propiedades de la máquina virtual en el script anterior nos permiten crear una máquina virtual con Windows Server 2016 DataCenter como el sistema operativo. Para cada tipo de máquina virtual, estas propiedades será ligeramente diferentes. El [definir la máquina virtual](#define-virtual-machine) sección muestra cómo determinar qué propiedades que se van a usar en esta secuencia de comandos.

El siguiente comando proporciona un ejemplo de ejecución del script que se guardan en un nombre de archivo: Create-LabVirtualMachine.ps1. 

```powershell
 PS> .\Create-LabVirtualMachine.ps1 -ResourceGroupName 'MyLabResourceGroup' -LabName 'MyLab' -userName 'AdminUser' -password 'Password1!' -VMName 'MyLabVM'
```

## <a name="define-virtual-machine"></a>Definición de máquina virtual
En esta sección se muestra cómo obtener las propiedades que son específicas de un tipo de máquina virtual que desea crear. 

### <a name="use-azure-portal"></a>Usar Azure Portal
Puede generar una plantilla de Azure Resource Manager al crear una máquina virtual en Azure portal. No es necesario completar el proceso de creación de la máquina virtual. Solo siga los pasos hasta que vea la plantilla. Esta es la mejor manera de obtener la descripción de JSON es necesaria si ya no dispone de un laboratorio de que máquina virtual creada. 

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Seleccione **todos los servicios** en el menú de navegación izquierdo.
3. Busque y seleccione **DevTest Labs** desde la lista de servicios. 
4. En el **DevTest Labs** , seleccione el laboratorio en la lista de laboratorios.
5. En la página principal para el laboratorio, seleccione **+ agregar** en la barra de herramientas. 
6. Seleccione un **imagen base** para la máquina virtual. 
7. Seleccione **opciones de automatización** en la parte inferior de la página anterior del **enviar** botón. 
8. Verá el **plantilla de Azure Resource Manager** para crear la máquina virtual. 
9. El segmento JSON en el **recursos** sección tiene la definición para el tipo de imagen que seleccionó anteriormente. 

    ```json
    {
      "apiVersion": "2018-10-15-preview",
      "type": "Microsoft.DevTestLab/labs/virtualmachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "labVirtualNetworkId": "[variables('labVirtualNetworkId')]",
        "notes": "Windows Server 2019 Datacenter",
        "galleryImageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2019-Datacenter",
          "osType": "Windows",
          "version": "latest"
        },
        "size": "[parameters('size')]",
        "userName": "[parameters('userName')]",
        "password": "[parameters('password')]",
        "isAuthenticationWithSshKey": false,
        "labSubnetName": "[variables('labSubnetName')]",
        "disallowPublicIpAddress": true,
        "storageType": "Standard",
        "allowClaim": false,
        "networkInterface": {
          "sharedPublicIpAddressConfiguration": {
            "inboundNatRules": [
              {
                "transportProtocol": "tcp",
                "backendPort": 3389
              }
            ]
          }
        }
      }
    }
    ```

En este ejemplo, consulte cómo obtener una definición de una imagen de Marketplace de Azure. Puede obtener una definición de una imagen personalizada, una fórmula o un entorno de la misma manera. Agregue los artefactos necesarios para la máquina virtual y establezca cualquier configuración avanzada que requiere. Después de proporcionar valores para los campos obligatorios y los campos opcionales, antes de seleccionar el **opciones de automatización** botón.

### <a name="use-azure-rest-api"></a>Use la API de REST de Azure
El siguiente procedimiento proporciona pasos para obtener las propiedades de una imagen mediante la API de REST: Estos pasos sólo funcionan para una máquina virtual existente en un laboratorio. 

1. Navegue hasta la [lista de máquinas virtuales -](/rest/api/dtl/virtualmachines/list) página, seleccione **Pruébelo** botón. 
2. Seleccione la **suscripción de Azure**.
3. Escriba el **grupo de recursos para el laboratorio**.
4. Escriba el **nombre del laboratorio**. 
5. Seleccione **Run** (Ejecutar).
6. Verá el **propiedades de la imagen** basándose en la que se creó la máquina virtual. 



## <a name="next-steps"></a>Pasos siguientes
Consulte el siguiente contenido: [Documentación de Azure PowerShell para Azure DevTest Labs](/powershell/module/az.devtestlabs/)