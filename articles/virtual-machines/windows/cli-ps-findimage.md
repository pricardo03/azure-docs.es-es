---
title: Selección de imágenes de máquina virtual Windows en Azure | Microsoft Docs
description: Use Azure PowerShell para determinar el publicador, la oferta, la SKU y la versión para imágenes de VM de Marketplace.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/08/2018
ms.author: danlep
ms.openlocfilehash: 5934e955d2a18d111c625670bced134df37ef045
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409601"
---
# <a name="find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Buscar imágenes de VM de Windows en Azure Marketplace con Azure PowerShell

En este artículo se describe cómo usar Azure PowerShell para buscar imágenes de VM en Azure Marketplace. A continuación, puede especificar una imagen de Marketplace cuando se crea una máquina virtual mediante programación con PowerShell, plantillas de Resource Manager u otras herramientas.

También puede examinar las imágenes y ofertas disponibles mediante el escaparate de [Azure Marketplace](https://azuremarketplace.microsoft.com/), [Azure Portal](https://portal.azure.com) o la [CLI de Azure](../linux/cli-ps-findimage.md). 

Asegúrese de que ha instalado y configurado el [módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps) más reciente.

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Tabla de imágenes de Windows más usadas
| Publicador | Oferta | SKU |
|:--- |:--- |:--- |:--- |
| Microsoft Windows Server |Windows Server |2016-Datacenter |
| Microsoft Windows Server |Windows Server |2016-Datacenter-Server-Core |
| Microsoft Windows Server |Windows Server |2016-Datacenter-with-Containers |
| Microsoft Windows Server |Windows Server |Centro de datos de 2012-R2 |
| Microsoft Windows Server |Windows Server |Centro de datos de 2012 |
| Microsoft Windows Server |Windows Server |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2017-WS2016 |Enterprise |
| MicrosoftRServer |RServer-WS2016 |Enterprise |

## <a name="navigate-the-images"></a>Navegación por las imágenes

Una manera de encontrar una imagen en una ubicación es mediante la ejecución de los cmdlets [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) y [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) en orden:

1. Listado de los publicadores de imágenes.
2. Para un publicador determinado, enumeración de sus ofertas.
3. Para una oferta determinada, enumeración de sus SKU.

Luego, para una SKU seleccionada, ejecute [Get-AzureRMVMImage](/powershell/module/azurerm.compute/get-azurermvmimage) para mostrar las versiones que se van a implementar.

1. Muestre una lista de los publicadores:

    ```powershell
    $locName="<Azure location, such as West US>"
    Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
    ```

2. Rellene el nombre del publicador elegido y haga una lista de las ofertas:

    ```powershell
    $pubName="<publisher>"
    Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
    ```

3. Rellene el nombre de la oferta elegida y haga una lista de las SKU:

    ```powershell
    $offerName="<offer>"
    Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
    ```
    
4. Rellene el nombre de la SKU elegido y obtenga la versión de imagen:

    ```powershell
    $skuName="<SKU>"
    Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
    ```
    
En la salida del comando `Get-AzureRMVMImage`, puede seleccionar una imagen de la versión para implementar una máquina virtual nueva.

En el ejemplo siguiente se muestra la secuencia completa de comandos y sus salidas:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Salida parcial:

```
PublisherName
-------------
...
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Para el publicador de *MicrosoftWindowsServer*:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Salida:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

Para la oferta de *WindowsServer*:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Salida:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
```

Luego, para la SKU de *2016-Datacenter*:

```powershell
$skuName="2016-Datacenter"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Ahora puede combinar el publicador, la oferta, la SKU y la versión que se seleccionó en un URN (valores separados por :). Pase este URN con el parámetro `--image` al crear una máquina virtual con el cmdlet [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Puede reemplazar de forma opcional el número de versión en el URN con "más actualizado" para obtener la versión más actualizada de la imagen.

Si implementa una máquina virtual con una plantilla de Resource Manager, establecerá los parámetros de imagen individualmente en las propiedades `imageReference`. Consulte la [referencia de plantilla](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Vista de las propiedades del plan

Para ver la información del plan de compra de una imagen, ejecute el cmdlet `Get-AzureRMVMImage`. Si la propiedad `PurchasePlan` de la salida no es `null`, la imagen tienen términos que debe aceptar antes de la implementación mediante programación.  

Por ejemplo, la imagen *Windows Server 2016 Datacenter* no tiene términos adicionales, porque la información de `PurchasePlan` es `null`:

```powershell
$version = "2016.127.20170406"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

Salida:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/
                   Versions/2016.127.20170406
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2016-Datacenter
Version          : 2016.127.20170406
FilterExpression :
Name             : 2016.127.20170406
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

En el ejemplo siguiente se muestra un comando similar para la imagen *Data Science Virtual Machine - Windows 2016* que tiene las propiedades `PurchasePlan` siguientes: `name`, `product` y `publisher`. Algunas imágenes también tienen una propiedad `promotion code`. Para implementar esta imagen, consulte las secciones siguientes para aceptar los términos y habilitar la implementación mediante programación.

```powershell
Get-AzureRMVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Salida:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMIma
                   ge/Offers/windows-data-science-vm/Skus/windows2016/Versions/0.2.02
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 0.2.02
FilterExpression :
Name             : 0.2.02
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>Aceptación de los términos

Para ver los términos de licencia, use el cmdlet [Get-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/get-azurermmarketplaceterms) y pase los parámetros del plan de compra. La salida proporciona un vínculo a los términos de la imagen de Marketplace y muestra si anteriormente aceptó los términos. Asegúrese de que todas las letras sean minúsculas en los valores de parámetros.

```powershell
Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Salida:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 2/23/2018 7:43:00 PM
```

Use el cmdlet [Set-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms) para aceptar o rechazar los términos. Solo debe aceptar los términos una vez por suscripción para la imagen. Asegúrese de que todas las letras sean minúsculas en los valores de parámetros. 

```powershell
$agreementTerms=Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzureRmMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept
```

Salida:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : XXXXXXK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBXXXXXX
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>Implementación mediante los parámetros del plan de compra

Después de aceptar los términos de una imagen, puede implementar una máquina virtual en esa suscripción. Como se muestra en el fragmento de código siguiente, use el cmdlet [Set-AzureRmVMPlan](/powershell/module/azurerm.compute/set-azurermvmplan) para establecer la información de plan de Marketplace para el objeto de la máquina virtual. Para que un script completo cree la configuración de red para la máquina virtual y complete la implementación, consulte los [ejemplos de script de PowerShell](powershell-samples.md).

```powershell
...

$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information

$publisherName = "microsoft-ads"

$productName = "windows-data-science-vm"

$planName = "windows2016"

$vmConfig = Set-AzureRmVMPlan -VM $vmConfig -Publisher $publisherName -Product $productName -Name $planName

$cred=Get-Credential

$vmConfig = Set-AzureRmVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image

$offerName = "windows-data-science-vm"

$skuName = "windows2016"

$version = "0.2.02"

$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version $version
...
```
Luego, la configuración de la VM se pasará junto con los objetos de configuración de red al cmdlet `New-AzureRmVM`.

## <a name="next-steps"></a>Pasos siguientes
Para crear una máquina virtual rápidamente con el cmdlet `New-AzureRmVM` mediante información de imagen básica, consulte [Creación de una máquina virtual Windows con PowerShell](quick-create-powershell.md).

Consulte un ejemplo de script de PowerShell para [crear una máquina virtual completamente configurada](../scripts/virtual-machines-windows-powershell-sample-create-vm.md).


