---
title: Creación de una máquina virtual Windows con Azure Image Builder (versión preliminar)
description: Cree una máquina virtual Windows con Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-windows
manager: jeconnoc
ms.openlocfilehash: 01109aa83c12bda9b1d21ec25784d663f8abf700
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65159726"
---
# <a name="preview-create-a-windows-vm-with-azure-image-builder"></a>Vista previa: Creación de una máquina virtual Windows con Azure Image Builder

En este artículo se muestra cómo puede crear una imagen personalizada de Windows mediante el generador de imágenes de máquina virtual de Azure. En el ejemplo de este artículo se usan tres [personalizadores](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#properties-customize) distintos para personalizar la imagen:
- PowerShell (ScriptUri): para descargar y ejecutar un [script de PowerShell](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1).
- Reinicio de Windows: reinicia la máquina virtual.
- PowerShell (insertado) ejecuta un comando específico. En este ejemplo, se crea un directorio en la máquina virtual mediante `mkdir c:\\buildActions`.
- Archivo: para copiar un archivo de GitHub en la máquina virtual. En este ejemplo se copia [index.md](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) en `c:\buildArtifacts\index.html` en la máquina virtual.

Se usará una plantilla .json de ejemplo para configurar la imagen. El archivo .json que se usa aquí es: [helloImageTemplateWin.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 


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
```

Si no se muestran como registradas, ejecute lo siguiente:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages

az provider register -n Microsoft.Storage
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Se usarán algunos fragmentos de información de forma repetida, por lo que se crearán diversas variables para almacenar esa información.

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

Cree una variable para el identificador de la suscripción. Puede obtenerlo mediante `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

Cree el grupo de recursos.

```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

Conceda a Image Builder permiso para crear recursos en ese grupo de recursos. El valor `--assignee` es el identificador de registro de aplicación para el servicio Image Builder. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="download-the-json-example"></a>Descarga del ejemplo .json

Descargue el archivo .json de ejemplo y configúrelo con las variables que ha creado.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json

```

## <a name="create-the-image"></a>Crear la imagen

Envío de la configuración de la imagen al servicio de generador de imágenes de máquina virtual

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

Inicie la generación de imágenes.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

Espere hasta que se complete la compilación. Esta operación puede tardar unos 15 minutos.

## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree la máquina virtual con la imagen que ha compilado. Reemplace *<password>* con su propia contraseña para `aibuser` en la máquina virtual.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgWinVm00 \
  --admin-username aibuser \
  --admin-password <password> \
  --image $imageName \
  --location $location
```

## <a name="verify-the-customization"></a>Comprobación de la personalización

Cree una conexión de Escritorio remoto a la máquina virtual con el nombre de usuario y la contraseña que ha establecido al crear la máquina virtual. Dentro de la máquina virtual, abra un símbolo del sistema y escriba lo siguiente:

```console
dir c:\
```

Debería ver estos dos directorios creados durante la personalización de la imagen:
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

Para obtener más información sobre los componentes del archivo .json que se usan en este artículo, vea [Referencia de la plantilla de generador de imágenes](../linux/image-builder-json.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

