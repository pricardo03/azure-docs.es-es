---
title: Uso de Azure Image Builder con una galería de imágenes para máquinas virtuales Windows (versión preliminar)
description: Cree imágenes de Windows con Azure Image Builder y la galería de imágenes compartidas.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-widows
manager: jeconnoc
ms.openlocfilehash: 2453d37720bcf48b95b428cf78c6186de40b31aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65160116"
---
# <a name="preview-create-a-windows-image-and-distribute-it-to-a-shared-image-gallery"></a>Vista previa: Creación de una imagen de Windows y distribución en una galería de imágenes compartidas 

En este artículo se muestra cómo puede usar Azure Image Builder para crear una versión de imagen en una [galería de imágenes compartidas](shared-image-galleries.md) y después distribuirla globalmente.

Se usará una plantilla .json para configurar la imagen. El archivo .json que se usa aquí es: [helloImageTemplateforWinSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/1_Creating_a_Custom_Win_Shared_Image_Gallery_Image/helloImageTemplateforWinSIG.json). 

Para distribuir la imagen en una galería de imágenes compartidas, en la plantilla se usa [sharedImage](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#distribute-sharedimage) como valor de la sección `distribute` de la plantilla.

> [!IMPORTANT]
> Azure Image Builder se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-features"></a>Registro de las características
Para usar Azure Image Builder durante la versión preliminar, tendrá que registrar la nueva característica.

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

Si no se muestran como registradas, ejecute lo siguiente:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Compute
```

## <a name="set-variables-and-permissions"></a>Establecimiento de variables y permisos 

Se usarán algunos fragmentos de información de forma repetida, por lo que se crearán diversas variables para almacenar esa información. Reemplace los valores de las variables, como `username` y `vmpassword`, por información propia.

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

Cree una variable para el identificador de la suscripción. Puede obtenerlo mediante `az account show | grep id`.

```azurecli-interactive
subscriptionID="Subscription ID"
```

Cree el grupo de recursos.

```azurecli-interactive
az group create -n $sigResourceGroup -l $location
```


Conceda a Azure Image Builder permiso para crear recursos en ese grupo de recursos. El valor `--assignee` es el identificador de registro de aplicación para el servicio Image Builder. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```





## <a name="create-an-image-definition-and-gallery"></a>Creación de una definición de imagen y una galería

Cree una galería de imágenes. 

```azurecli-interactive
az sig create \
    -g $sigResourceGroup \
    --gallery-name $sigName
```

Cree la definición de una imagen.

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


## <a name="download-and-configure-the-json"></a>Descarga y configuración del archivo .json

Descargue la plantilla .json y configúrela con las variables.

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

## <a name="create-the-image-version"></a>Creación de la versión de la imagen

En la sección siguiente se creará la versión de la imagen en la galería. 

Envíe la configuración de la imagen al servicio Azure Image Builder.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

Inicie la generación de imágenes.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforWinSIG01 \
     --action Run 
```

La creación de la imagen y su replicación en las dos regiones puede llevar un tiempo. Espere a que termine esta parte antes de pasar a la creación de una máquina virtual.


## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree una máquina virtual a partir la versión de la imagen creada por Azure Image Builder.

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm001 \
  --admin-username $username \
  --admin-password $vmpassword \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --location $location
```


## <a name="verify-the-customization"></a>Comprobación de la personalización
Cree una conexión de Escritorio remoto a la máquina virtual con el nombre de usuario y la contraseña que ha establecido al crear la máquina virtual. Dentro de la máquina virtual, abra un símbolo del sistema y escriba lo siguiente:

```console
dir c:\
```

Debería ver un directorio con el nombre `buildActions`, que se ha creado durante la personalización de la imagen.


## <a name="clean-up-resources"></a>Limpieza de recursos
Si ahora quiere volver a personalizar la versión de la imagen para crear una nueva de la misma imagen, **omita este paso** y vaya a [Uso de Azure Image Builder para crear otra versión de la imagen](image-builder-gallery-update-image-version.md).


Esta acción eliminará la imagen que se ha creado, junto con todos los demás archivos de recursos. Asegúrese de que haya terminado con esta implementación antes de eliminar los recursos.

Al eliminar los recursos de la galería de imágenes, tendrá que eliminar todas las versiones de la imagen antes de poder eliminar la definición de la imagen que se ha usado para crearlas. Para eliminar una galería, primero tiene que haber eliminado todas las definiciones de imagen de la galería.

Elimine la plantilla de Azure Image Builder.

```azurecli-interactive
az resource delete \
    --resource-group $sigResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforWinSIG01
```

Obtenga la versión de la imagen creada por el generador de imágenes, que siempre empieza por `0.`, y después elimínela.

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

Elimine la galería.

```azurecli-interactive
az sig delete -r $sigName -g $sigResourceGroup
```

Elimine el grupo de recursos.

```azurecli-interactive
az group delete -n $sigResourceGroup -y
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo actualizar la versión de la imagen que ha creado, vea [Uso de Azure Image Builder para crear otra versión de la imagen](image-builder-gallery-update-image-version.md).