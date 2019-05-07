---
title: Crear una máquina virtual de Windows con el generador de imágenes de Azure (versión preliminar)
description: Crear una máquina virtual Windows con el generador de imágenes de Azure.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: jeconnoc
ms.openlocfilehash: 01109aa83c12bda9b1d21ec25784d663f8abf700
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159726"
---
# <a name="preview-create-a-windows-vm-with-azure-image-builder"></a>Vista previa: Crear una máquina virtual Windows con el generador de imágenes de Azure

En este artículo es mostrarle cómo puede crear una imagen personalizada de Windows mediante el generador de imágenes de máquina virtual de Azure. El ejemplo de este artículo utiliza tres [personalizadores](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#properties-customize) para personalizar la imagen:
- PowerShell (ScriptUri) - descargar y ejecutar un [script de PowerShell](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1).
- Reinicio de Windows - reinicia la máquina virtual.
- PowerShell (inline) - ejecutar un comando específico. En este ejemplo, crea un directorio en la máquina virtual mediante `mkdir c:\\buildActions`.
- Archivo - copiar un archivo de GitHub en la máquina virtual. Este ejemplo copia [index.md](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) a `c:\buildArtifacts\index.html` en la máquina virtual.

Usaremos una plantilla .json de ejemplo para configurar la imagen. El archivo .json que usamos aquí es: [helloImageTemplateWin.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 


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

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Usaremos algunas partes de información varias veces, por lo que vamos a crear algunas variables para almacenar esa información.

```azurecli-interactive
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myWinImgBuilderRG
# Region location 
location=WestUS2
# Name for the image 
imageName=myWinBuilderImage
# Run output name
runOutputName=aibWindows
# name of the image to be created
imageName=aibWinImage
```

Cree una variable para el identificador de suscripción. Puede obtener esta mediante `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

Cree el grupo de recursos.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

Conceder permiso de Image Builder para crear recursos en ese grupo de recursos. El `--assignee` valor es el identificador de registro de aplicación para el servicio de generador de imágenes. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="download-the-json-example"></a>Descargue el ejemplo de JSON

Descargue el archivo .json de ejemplo y configúrelo con las variables que creó.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json

```

## <a name="create-the-image"></a>Crear la imagen

Enviar la configuración de la imagen para el servicio de generador de imágenes de máquina virtual

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

Iniciar la generación de imagen.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

Espere hasta que finalice la compilación. Esto puede tardar unos 15 minutos.

## <a name="create-the-vm"></a>Creación de la máquina virtual

Crear la máquina virtual con la imagen que ha creado. Reemplace *<password>* con su propia contraseña para la `aibuser` en la máquina virtual.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgWinVm00 \
  --admin-username aibuser \
  --admin-password <password> \
  --image $imageName \
  --location $location
```

## <a name="verify-the-customization"></a>Comprobar la personalización

Crear una conexión de escritorio remoto a la máquina virtual con el nombre de usuario y contraseña que estableció cuando creó la máquina virtual. Dentro de la máquina virtual, abra un símbolo del sistema y escriba:

```console
dir c:\
```

Debería ver estas dos directorios creados durante la personalización de la imagen:
- buildActions
- buildArtifacts

## <a name="clean-up"></a>Limpieza

Cuando haya terminado, elimine los recursos.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
az group delete -n $imageResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los componentes del archivo .json que se usa en este artículo, consulte [la imagen de referencia de plantilla generador](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

