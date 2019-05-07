---
title: Utilice el generador de imágenes de Azure con una galería de imágenes para las máquinas virtuales de Windows (versión preliminar)
description: Creación de imágenes de Windows con Azure Image Builder y Galería de imágenes compartidas.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-widows
manager: jeconnoc
ms.openlocfilehash: 2453d37720bcf48b95b428cf78c6186de40b31aa
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160116"
---
# <a name="preview-create-a-windows-image-and-distribute-it-to-a-shared-image-gallery"></a>Vista previa: Crear una imagen de Windows y distribuirla a una galería de imágenes compartidas 

En este artículo es mostrarle cómo puede usar el generador de imágenes de Azure para crear una versión de la imagen en un [Galería de imágenes compartidas](shared-image-galleries.md), a continuación, distribuir la imagen global.

Usaremos una plantilla JSON para configurar la imagen. El archivo .json que usamos aquí es: [helloImageTemplateforWinSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json). 

Para distribuir la imagen en una galería de imágenes compartidas, se usa la plantilla [sharedImage](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#distribute-sharedimage) como el valor de la `distribute` sección de la plantilla.

> [!IMPORTANT]
> Generador de imágenes de Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-features"></a>Registrar las características
Para usar el generador de imágenes de Azure durante la versión preliminar, deberá registrar la nueva característica.

```azurecli-interactive
az feature register --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview
```

Compruebe el estado del registro de la característica.

```azurecli-interactive
az feature show --namespace Microsoft.VirtualMachineImages --name VirtualMachineTemplatePreview | grep state
```

Compruebe el registro.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState
az provider show -n Microsoft.Storage | grep registrationState
az provider show -n Microsoft.Compute | grep registrationState
```

Si no dicen registrado, ejecute lo siguiente:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Compute
```

## <a name="set-variables-and-permissions"></a>Establecimiento de variables y permisos 

Usaremos algunas partes de información varias veces, por lo que vamos a crear algunas variables para almacenar esa información. Reemplace los valores de las variables, como `username` y `vmpassword`, por su propia información.

```azurecli-interactive
# Resource group name - we are using ibsigRG in this example
sigResourceGroup=myIBWinRG
# Datacenter location - we are using West US 2 in this example
location=westus
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=my22stSIG
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=winSvrimages
# image distribution metadata reference name
runOutputName=w2019SigRo
# User name and password for the VM
username="azureuser"
vmpassword="passwordfortheVM"
```

Cree una variable para el identificador de suscripción. Puede obtener esta mediante `az account show | grep id`.

```azurecli-interactive
subscriptionID="Subscription ID"
```

Cree el grupo de recursos.

```azurecli-interactive
az group create -n $sigResourceGroup -l $location
```


Conceder permiso de Azure Image Builder para crear recursos en ese grupo de recursos. El `--assignee` valor es el identificador de registro de aplicación para el servicio de generador de imágenes. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```





## <a name="create-an-image-definition-and-gallery"></a>Creación de una definición de la imagen y la Galería

Crear una galería de imágenes. 

```azurecli-interactive
az sig create \
    -g $sigResourceGroup \
    --gallery-name $sigName
```

Crear una definición de la imagen.

```azurecli-interactive
az sig image-definition create \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --publisher corpIT \
   --offer myOffer \
   --sku 2019 \
   --os-type Windows
```


## <a name="download-and-configure-the-json"></a>Descargar y configurar el .json

Descargue la plantilla .json y configúrelo con las variables.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json -o helloImageTemplateforWinSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforWinSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforWinSIG.json
```

## <a name="create-the-image-version"></a>Crear la versión de imagen

La siguiente sección creará la versión de la imagen en la galería. 

Enviar la configuración de la imagen para el servicio de generador de imágenes de Azure.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

Iniciar la generación de imagen.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforWinSIG01 \
     --action Run 
```

Creación de la imagen y replicarla en ambas regiones pueden tardar un rato. Espere a que termine esta parte antes de pasar a la creación de una máquina virtual.


## <a name="create-the-vm"></a>Creación de la máquina virtual

Crear una máquina virtual desde la versión de la imagen que se creó el generador de imágenes de Azure.

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm001 \
  --admin-username $username \
  --admin-password $vmpassword \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --location $location
```


## <a name="verify-the-customization"></a>Comprobar la personalización
Crear una conexión de escritorio remoto a la máquina virtual con el nombre de usuario y contraseña que estableció cuando creó la máquina virtual. Dentro de la máquina virtual, abra un símbolo del sistema y escriba:

```console
dir c:\
```

Debería ver un directorio denominado `buildActions` que se creó durante la personalización de la imagen.


## <a name="clean-up-resources"></a>Limpieza de recursos
Si desea volver a personalizar la versión de la imagen para crear una nueva versión de la misma imagen, ahora, pruebe **omitir este paso** y vaya a [usar generador de imágenes de Azure para crear otra versión de la imagen](image-builder-gallery-update-image-version.md).


Esta acción eliminará la imagen que se creó, junto con todos los demás archivos de recursos. Asegúrese de que haya terminado con esta implementación antes de eliminar los recursos.

Al eliminar los recursos de la Galería de imágenes, debe eliminar todas las versiones de la imagen antes de poder eliminar la definición de la imagen para su creación. Para eliminar una galería, primero deberá eliminaron todas las definiciones de la imagen en la galería.

Eliminar la plantilla generador de imágenes.

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

Obtener la versión de imagen creada por el generador de imágenes, siempre se inicia con `0.`y, a continuación, elimine la versión de imagen

```azurecli-interactive
sigDefImgVersion=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'name' -o json | grep 0. | tr -d '"')
az sig image-version delete \
   -g $sigResourceGroup \
   --gallery-image-version $sigDefImgVersion \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```   


Elimine la definición de la imagen.

```azurecli-interactive
az sig image-definition delete \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID
```

Eliminar la galería.

```azurecli-interactive
az sig delete -r $sigName -g $sigResourceGroup
```

Elimine el grupo de recursos.

```azurecli-interactive
az group delete -n $sigResourceGroup -y
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo actualizar la versión de la imagen que creó, consulte [usar generador de imágenes de Azure para crear otra versión de la imagen](image-builder-gallery-update-image-version.md).