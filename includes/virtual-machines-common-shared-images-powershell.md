---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 12/10/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 91889971e1ab8a9ea8341f6bc57735d973ea0e89
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60188345"
---
## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. 

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede ir a [https://shell.azure.com/powershell](https://shell.azure.com/powershell) para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.


## <a name="preview-register-the-feature"></a>Vista previa: Registrar la característica

La galería de imágenes compartidas está en versión preliminar, pero debe registrar la característica antes de poder usarla. Para registrar la característica de galería de imágenes compartidas:

```azurepowershell-interactive
Register-AzProviderFeature `
   -FeatureName GalleryPreview `
   -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="get-the-managed-image"></a>Obtener la imagen administrada

Puede ver una lista de imágenes disponibles en un grupo de recursos con [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Una vez que sepa el nombre de la imagen y en qué grupo de recursos está, puede usar `Get-AzImage` nuevamente para obtener el objeto de imagen y almacenarlo en una variable para usarlo más adelante. En este ejemplo se obtiene una imagen denominada *myImage* desde el grupo de recursos "myResourceGroup" y se asigna a la variable *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Creación de una galería de imágenes 

Una galería de imágenes es el recurso principal que se usa para habilitar el uso compartido de imágenes. Los nombres de las galerías deben ser únicos dentro de su suscripción. Cree una galería de imágenes con [New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). En el ejemplo siguiente se crea una galería denominada *myGallery* en el grupo de recursos *myGalleryRG*.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   
## <a name="create-an-image-definition"></a>Creación de la definición de una imagen 

Cree la definición de la imagen de la galería con [New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). En este ejemplo, la imagen de la galería se denomina *myGalleryImage*.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```
### <a name="using-publisher-offer-and-sku"></a>Uso de publicador, oferta y SKU 
Para clientes que planean sobre la implementación de imágenes compartidas, **en una próxima versión**, podrá usar su personal definido **-Publisher**, **-ofrecen** y **- Sku** valores para buscar y especificar una definición de la imagen y después crear una máquina virtual con la versión más reciente de la imagen de la coincidencia de definición de la imagen. Por ejemplo, estas son las tres definiciones de imágenes y sus valores:

|Definición de imágenes|Publicador|Oferta|SKU|
|---|---|---|---|
|myImage1|myPublisher|myOffer|mySku|
|myImage2|myPublisher|standardOffer|mySku|
|myImage3|Prueba|standardOffer|testSku|

Los tres tienen conjuntos de valores únicos. Puede tener versiones de imágenes que comparten uno o dos valores, pero no los tres. **En una próxima versión**, podrá combinar estos valores con el fin de solicitar la versión más reciente de una imagen específica. **Esto no funciona en la versión actual**, pero estará disponible en el futuro. Cuando publique, debe usarse con la siguiente sintaxis para establecer la imagen de origen como *myImage1* de la tabla anterior.

```powershell
$vmConfig = Set-AzVMSourceImage `
   -VM $vmConfig `
   -PublisherName myPublisher `
   -Offer myOffer `
   -Skus mySku 
```

Esto es similar a cómo se pueden especificar actualmente uso publicador, oferta y SKU de [imágenes de Azure Marketplace](../articles/virtual-machines/windows/cli-ps-findimage.md) para obtener la versión más reciente de una imagen de Marketplace. Con esto en mente, cada definición de imagen debe tener un conjunto único de estos valores.  

## <a name="create-an-image-version"></a>Creación de la versión de una imagen

Cree una versión de imagen a partir de una imagen mediante [New-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). En este ejemplo, la versión de imagen es *1.0.0* y se replica en los centros de datos *Centro-oeste de EE. UU.* y *Centro-sur de EE. UU.*


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

Puede tardar un rato en replicar la imagen en todas las regiones de destino, por lo que creamos un trabajo para que pueda hacer un seguimiento del progreso. Para ver el progreso del trabajo, escriba `$job.State`.

```azurepowershell-interactive
$job.State
```

