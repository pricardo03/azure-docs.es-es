---
title: Crear una nueva versión de imagen de una versión de imagen existente con el generador de imágenes de Azure (versión preliminar)
description: Crear una nueva versión de imagen de una versión de imagen existente con el generador de imágenes de Azure.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: jeconnoc
ms.openlocfilehash: f1bce4c2e5c7ae9dc7ec5917fbc5ac115eecdffa
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160101"
---
# <a name="preview-create-a-new-image-version-from-an-existing-image-version-using-azure-image-builder"></a>Vista previa: Crear una nueva versión de imagen de una versión de imagen existente con el generador de imágenes de Azure

Este artículo muestra cómo tomar una versión de imagen existente en un [Galería de imágenes compartidas](shared-image-galleries.md), actualizarlo y publicarlo como una nueva versión de imagen a la galería.

Usaremos una plantilla .json de ejemplo para configurar la imagen. El archivo .json que usamos aquí es: [helloImageTemplateforSIGfromWinSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Win_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromWinSIG.json). 

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

Si ha usado [crear una imagen y distribuir a una galería de imágenes compartidas](image-builder-gallery.md) para crear la Galería de imágenes compartidas, ya ha creado las variables que necesitamos. Si no es así, configure algunas variables que se usará para este ejemplo.

La versión preliminar, el generador de imágenes solo admitirá la creación de imágenes personalizadas en el mismo grupo de recursos que la imagen administrada de origen. Actualice el nombre del grupo de recursos en este ejemplo sea el mismo grupo de recursos que la imagen administrada de origen.

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
username="user name for the VM"
vmpassword="password for the VM"
```

Cree una variable para el identificador de suscripción. Puede obtener esta mediante `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Subscription ID>
```

Obtenga la versión de la imagen que desea actualizar.

```azurecli-interactive
sigDefImgVersionId=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'id' -o json | grep 0. | tr -d '"' | tr -d '[:space:]')
```


Si ya tiene su propia galería de imágenes compartidas y no siguió el ejemplo anterior, deberá asignar permisos para Image Builder tener acceso al grupo de recursos, por lo que puede tener acceso a la galería.


```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup
```


## <a name="modify-helloimage-example"></a>Modificar ejemplo helloImage
Puede revisar el ejemplo que se van a usar, abra el archivo .json aquí: [helloImageTemplateforSIGfromSIG.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json) junto con el [referencia de plantillas de Image Builder](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 


Descargue el ejemplo de JSON y configúrelo con las variables. 

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Win_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromWinSIG.json -o helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s%<sigDefImgVersionId>%$sigDefImgVersionId%g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIGfromWinSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIGfromWinSIG.json
```

## <a name="create-the-image"></a>Crear la imagen

Enviar la configuración de la imagen para el servicio de generador de imágenes de máquina virtual.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIGfromWinSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n imageTemplateforSIGfromWinSIG01
```

Iniciar la generación de imagen.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n imageTemplateforSIGfromWinSIG01 \
     --action Run 
```

Espere hasta que se ha creado la imagen y la replicación antes de continuar con el paso siguiente.


## <a name="create-the-vm"></a>Creación de la máquina virtual

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgWinVm002 \
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

Ahora debería ver dos directorios:
- `buildActions` que se creó en la primera versión de imagen.
- `buildActions2` que se creó como parte de la actualización de la primera versión de imagen para crear la segunda versión de imagen.


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los componentes del archivo .json que se usa en este artículo, consulte [la imagen de referencia de plantilla generador](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).