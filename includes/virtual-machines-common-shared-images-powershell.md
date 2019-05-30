---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: bae66078a1bcb1d80f0798b1d501598fa785fb80
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241193"
---
## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. 

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede ir a [https://shell.azure.com/powershell](https://shell.azure.com/powershell) para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.


## <a name="get-the-managed-image"></a>Obtener la imagen administrada

Puede ver una lista de imágenes disponibles en un grupo de recursos con [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Una vez que sepa el nombre de la imagen y en qué grupo de recursos está, puede usar `Get-AzImage` nuevamente para obtener el objeto de imagen y almacenarlo en una variable para usarlo más adelante. En este ejemplo se obtiene una imagen denominada *myImage* desde el grupo de recursos "myResourceGroup" y se asigna a la variable *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Creación de una galería de imágenes 

Una galería de imágenes es el recurso principal que se usa para habilitar el uso compartido de imágenes. Los caracteres permitidos para el nombre de la Galería son letras mayúsculas o minúsculas, números y puntos. El nombre de la Galería no puede contener guiones. Los nombres de las galerías deben ser únicos dentro de su suscripción. 

Cree una galería de imágenes con [New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). En el ejemplo siguiente se crea una galería denominada *myGallery* en el grupo de recursos *myGalleryRG*.

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

Las definiciones de la imagen creación una agrupación lógica de imágenes. Se utilizan para administrar la información sobre las versiones de la imagen que se crean dentro de ellos. Los nombres de definición de imagen pueden estar formados por períodos, dígitos, puntos, guiones y las letras mayúsculas o minúsculas. Para obtener más información acerca de los valores que se puede especificar para una definición de la imagen, consulte [definiciones de la imagen](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Crear la definición de la imagen mediante [New AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). En este ejemplo, la imagen de la galería se denomina *myGalleryImage*.

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


## <a name="create-an-image-version"></a>Creación de la versión de una imagen

Crear una versión de la imagen de una imagen administrada mediante [New AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). 

Los caracteres permitidos para la versión de una imagen son números y puntos. Los números deben estar dentro del rango de un entero de 32 bits. Formato: *MajorVersion*. *MinorVersion*. *Revisión*.

En este ejemplo, la versión de imagen es *1.0.0* y se replica en los centros de datos *Centro-oeste de EE. UU.* y *Centro-sur de EE. UU.* Al elegir las regiones de destino para la replicación, recuerde que también tiene que incluir el *origen* región como un destino para la replicación.


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

> [!NOTE]
> Deberá esperar a que finalice por completo que se compila y se replican para poder usar la misma imagen administrada para crear otra versión de la imagen la versión de imagen. 
>
> También puede almacenar la versión de la imagen en [almacenamiento con redundancia de zona](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) agregando `-StorageAccountType Standard_ZRS` al crear la versión de la imagen.
>


## <a name="share-the-gallery"></a>Compartir la Galería

Se recomienda que comparten acceso en el nivel de la Galería de imágenes. Usar una dirección de correo electrónico y la [Get AzADUser](/powershell/module/az.resources/get-azaduser) cmdlet para obtener el identificador de objeto para el usuario y, después, usar [New AzRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment) para concederles acceso a la galería. Reemplace el correo electrónico de ejemplo, alinne_montes@contoso.com en este ejemplo, con su propia información.

```azurepowershell-interactive
# Get the object ID for the user
$user = Get-AzADUser -StartsWith alinne_montes@contoso.com
# Grant access to the user for our gallery
New-AzRoleAssignment `
   -ObjectId $user.Id `
   -RoleDefinitionName Reader `
   -ResourceName $gallery.Name `
   -ResourceType Microsoft.Compute/galleries `
   -ResourceGroupName $resourceGroup.ResourceGroupName
```