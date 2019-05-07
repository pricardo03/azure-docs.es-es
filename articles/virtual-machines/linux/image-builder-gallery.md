---
title: Utilice el generador de imágenes de Azure con una galería de imágenes para las máquinas virtuales de Linux (versión preliminar)
description: Crear imágenes de Linux con el generador de imágenes de Azure y la Galería de imágenes compartidas.
author: cynthn
ms.author: cynthn
ms.date: 04/20/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: e9a8a30d9f5f170073c0ad671a248703b1078864
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159501"
---
# <a name="preview-create-a-linux-image-and-distribute-it-to-a-shared-image-gallery"></a>Vista previa: Crear una imagen de Linux y distribuirla a una galería de imágenes compartidas 

Este artículo muestra cómo puede usar el generador de imágenes de Azure para crear una versión de la imagen en un [Galería de imágenes compartidas](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/shared-image-galleries), a continuación, distribuir la imagen global.


Usaremos una plantilla .json de ejemplo para configurar la imagen. El archivo .json que usamos aquí es: [helloImageTemplateforSIG.json](https://github.com/danielsollondon/azvmimagebuilder/blob/master/quickquickstarts/1_Creating_a_Custom_Linux_Shared_Image_Gallery_Image/helloImageTemplateforSIG.json). 

Para distribuir la imagen en una galería de imágenes compartidas, se usa la plantilla [sharedImage](image-builder-json.md#distribute-sharedimage) como el valor de la `distribute` sección de la plantilla.

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
```

Si no dicen registrado, ejecute lo siguiente:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```

## <a name="set-variables-and-permissions"></a>Establecimiento de variables y permisos 

Usaremos algunas partes de información varias veces, por lo que vamos a crear algunas variables para almacenar esa información.

La versión preliminar, el generador de imágenes solo admitirá la creación de imágenes personalizadas en el mismo grupo de recursos que la imagen administrada de origen. Actualice el nombre del grupo de recursos en este ejemplo sea el mismo grupo de recursos que la imagen administrada de origen.

```azurecli-interactive
# Resource group name - we are using ibLinuxGalleryRG in this example
sigResourceGroup=ibLinuxGalleryRG
# Datacenter location - we are using West US 2 in this example
location=westus2
# Additional region to replicate the image to - we are using East US in this example
additionalregion=eastus
# name of the shared image gallery - in this example we are using myGallery
sigName=myIbGallery
# name of the image definition to be created - in this example we are using myImageDef
imageDefName=myIbImageDef
# image distribution metadata reference name
runOutputName=aibLinuxSIG
```

Cree una variable para el identificador de suscripción. Puede obtener esta mediante `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Subscription ID>
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
   --publisher myIbPublisher \
   --offer myOffer \
   --sku 18.04-LTS \
   --os-type Linux
```


## <a name="download-and-configure-the-json"></a>Descargar y configurar el .json

Descargue la plantilla .json y configúrelo con las variables.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Linux_Shared_Image_Gallery_Image/helloImageTemplateforSIG.json -o helloImageTemplateforSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIG.json
```

## <a name="create-the-image-version"></a>Crear la versión de imagen

La siguiente sección creará la versión de la imagen en la galería. 

Enviar la configuración de la imagen para el servicio de generador de imágenes de Azure.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIG01
```

Iniciar la generación de imagen.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforSIG01 \
     --action Run 
```

Creación de la imagen y replicarla en ambas regiones pueden tardar un rato. Espere a que termine esta parte antes de pasar a la creación de una máquina virtual.


## <a name="create-the-vm"></a>Creación de la máquina virtual

Crear una máquina virtual desde la versión de la imagen que se creó el generador de imágenes de Azure.

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name myAibGalleryVM \
  --admin-username aibuser \
  --location $location \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --generate-ssh-keys
```

SSH en la máquina virtual.

```azurecli-interactive
ssh aibuser@<publicIpAddress>
```

Debería ver la imagen se ha personalizado con un *mensaje del día* tan pronto como se establece la conexión SSH.

```console
*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea ahora, intente volver a personalizar la versión de la imagen para crear una nueva versión de la misma imagen, omita los pasos y vaya a [usar generador de imágenes de Azure para crear otra versión de la imagen](image-builder-gallery-update-image-version.md).


Esta acción eliminará la imagen que se creó, junto con todos los demás archivos de recursos. Asegúrese de que haya terminado con esta implementación antes de eliminar los recursos.

Al eliminar los recursos de la Galería de imágenes, debe eliminar todas las versiones de la imagen antes de poder eliminar la definición de la imagen para su creación. Para eliminar una galería, primero deberá eliminaron todas las definiciones de la imagen en la galería.

Eliminar la plantilla generador de imágenes.

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIG01
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

Obtenga más información sobre [galerías de imágenes de Azure Shared](shared-image-galleries.md).