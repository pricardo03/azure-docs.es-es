---
title: Crear una imagen de máquina Virtual y usar una identidad administrada asignada por el usuario para obtener acceso a archivos en Azure Storage (versión preliminar)
description: Crear imagen de máquina virtual mediante el generador de imágenes de Azure, que puede tener acceso a archivos almacenados en Azure Storage con la identidad administrada asignada por el usuario.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: d10ee8c1af85de5eb79cd4a4af6882c7a8f084f1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159561"
---
# <a name="create-an-image-and-use-a-user-assigned-managed-identity-to-access-files-in-azure-storage"></a>Crear una imagen y usar una identidad administrada asignada por el usuario acceso a los archivos en el almacenamiento de Azure 

Azure admite el generador de imágenes mediante secuencias de comandos, o copiar archivos desde varias ubicaciones, como GitHub y Azure storage etcetera. Para utilizar estos métodos, debe haber estado externamente puede tener acceso al generador de imágenes de Azure, pero puede proteger mediante Tokens de SAS de Azure Storage BLOB.

En este artículo se muestra cómo crear una imagen personalizada mediante el almacén de imágenes de máquina virtual de Azure que usará el servicio de un [identidad administrada asignada por el usuario](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview) para tener acceso a los archivos de almacenamiento de Azure de la personalización de la imagen, sin tener que volver a realizar los archivos accesibles públicamente o configuración de tokens de SAS.

En el ejemplo siguiente, creará dos grupos de recursos, se utilizará para la imagen personalizada de uno y otro va a hospedar una cuenta de almacenamiento de Azure, que contiene un archivo de script. Esto simula un escenario real, donde puede tener los artefactos de compilación o archivos de imagen en diferentes cuentas de almacenamiento, fuera de Image Builder. Se creará una identidad asignada por el usuario, a continuación, conceda a ese permisos de lectura en el archivo de script, pero no establecerá cualquier acceso público a ese archivo. A continuación, usará el personalizador del Shell para descargar y ejecutar ese script desde la cuenta de almacenamiento.


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
# Image resource group name 
imageResourceGroup=aibmdimsi
# storage resource group
strResourceGroup=aibmdimsistor
# Location 
location=WestUS2
# name of the image to be created
imageName=aibCustLinuxImgMsi01
# image distribution metadata reference name
runOutputName=u1804ManImgMsiro
```

Cree una variable para el identificador de suscripción. Puede obtener esta mediante `az account show | grep id`.

```azurecli-interactive
subscriptionID=<Your subscription ID>
```

Cree los grupos de recursos para la imagen y el almacenamiento de la secuencia de comandos.

```azurecli-interactive
# create resource group for image template
az group create -n $imageResourceGroup -l $location
# create resource group for the script storage
az group create -n $strResourceGroup -l $location
```


Crear el almacenamiento y copie el script de ejemplo de GitHub.

```azurecli-interactive
# script storage account
scriptStorageAcc=aibstorscript$(date +'%s')

# script container
scriptStorageAccContainer=scriptscont$(date +'%s')

# script url
scriptUrl=https://$scriptStorageAcc.blob.core.windows.net/$scriptStorageAccContainer/customizeScript.sh

# create storage account and blob in resource group
az storage account create -n $scriptStorageAcc -g $strResourceGroup -l $location --sku Standard_LRS

az storage container create -n $scriptStorageAccContainer --fail-on-exist --account-name $scriptStorageAcc

# copy in an example script from the GitHub repo 
az storage blob copy start \
    --destination-blob customizeScript.sh \
    --destination-container $scriptStorageAccContainer \
    --account-name $scriptStorageAcc \
    --source-uri https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh
```



Conceder permiso de Image Builder para crear recursos en el grupo de recursos de imagen. El `--assignee` valor es el identificador de registro de aplicación para el servicio de generador de imágenes. 

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```


## <a name="create-user-assigned-managed-identity"></a>Crear la identidad administrada asignada por el usuario

Crear la identidad y asignar permisos para la cuenta de almacenamiento de la secuencia de comandos. Para obtener más información, consulte [asignada por el usuario administra identidades](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm#user-assigned-managed-identity).

```azurecli-interactive
# Create the user assigned identity 
identityName=aibBuiUserId$(date +'%s')
az identity create -g $imageResourceGroup -n $identityName
# assign the identity permissions to the storage account, so it can read the script blob
imgBuilderCliId=$(az identity show -g $imageResourceGroup -n $identityName | grep "clientId" | cut -c16- | tr -d '",')
az role assignment create \
    --assignee $imgBuilderCliId \
    --role "Storage Blob Data Reader" \
    --scope /subscriptions/$subscriptionID/resourceGroups/$strResourceGroup/providers/Microsoft.Storage/storageAccounts/$scriptStorageAcc/blobServices/default/containers/$scriptStorageAccContainer 
# create the user identity URI
imgBuilderId=/subscriptions/$subscriptionID/resourcegroups/$imageResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/$identityName
```


## <a name="modify-the-example"></a>Modifique el ejemplo

Descargue el archivo .json de ejemplo y configúrelo con las variables que creó.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/7_Creating_Custom_Image_using_MSI_to_Access_Storage/helloImageTemplateMsi.json -o helloImageTemplateMsi.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateMsi.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateMsi.json
sed -i -e "s/<region>/$location/g" helloImageTemplateMsi.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateMsi.json
sed -i -e "s%<scriptUrl>%$scriptUrl%g" helloImageTemplateMsi.json
sed -i -e "s%<imgBuilderId>%$imgBuilderId%g" helloImageTemplateMsi.json
sed -i -e "s%<runOutputName>%$runOutputName%g" helloImageTemplateMsi.json
```

## <a name="create-the-image"></a>Crear la imagen

Enviar la configuración de la imagen para el servicio de generador de imágenes de Azure.

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateMsi.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateMsi01
```

Iniciar la generación de imagen.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateMsi01 \
     --action Run 
```

Espere a que finalice la compilación. Esto puede tardar unos 15 minutos.

## <a name="create-a-vm"></a>Crear una VM

Crear una máquina virtual desde la imagen. 

```bash
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgVm00 \
  --admin-username aibuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

Una vez creada la máquina virtual, inicie una sesión de SSH con la máquina virtual.

```azurecli-interactive
ssh aibuser@<publicIp>
```

Debería ver que la imagen se ha personalizado con un mensaje del día en cuanto se establece la conexión SSH.

```console

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

## <a name="clean-up"></a>Limpieza

Cuando haya terminado, puede eliminar los recursos si ya no son necesarios.

```azurecli-interactive
az identity delete --ids $imgBuilderId
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateMsi01
az group delete -n $imageResourceGroup
az group delete -n $strResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Si tiene algún problema para trabajar con el generador de imágenes de Azure, consulte [Troubleshooting](https://github.com/danielsollondon/azvmimagebuilder/blob/master/troubleshootingaib.md?toc=%2fazure%2fvirtual-machines%context%2ftoc.json).