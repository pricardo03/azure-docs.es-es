---
title: Crear una máquina virtual Linux con Azure Image Builder (versión preliminar)
description: Cree una máquina virtual Linux con Azure Image Builder.
author: cynthn
ms.author: cynthn
ms.date: 05/02/2019
ms.topic: article
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: 854645af95d780053d94668921e41ac189bbbfb7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65159516"
---
# <a name="preview-create-a-linux-vm-with-azure-image-builder"></a>Vista previa: Crear una máquina virtual Linux con Azure Image Builder

En este artículo se muestra cómo puede crear una imagen personalizada de Linux mediante Azure Image Builder y la CLI de Azure. En el ejemplo de este artículo se usan tres [personalizadores](image-builder-json.md#properties-customize) distintos para personalizar la imagen:

- Shell (ScriptUri): descarga y ejecuta un [script de shell](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/customizeScript.sh).
- Shell (alineado): ejecuta comandos específicos. En este ejemplo, los comandos alineados incluyen la creación de un directorio y la actualización del sistema operativo.
- Archivo: copia un [archivo de GitHub](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) en un directorio de la máquina virtual.

Se usará una plantilla .json de ejemplo para configurar la imagen. El archivo .json que se usa aquí es: [helloImageTemplateLinux.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json). 

> [!IMPORTANT]
> Azure Image Builder se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-features"></a>Registrar las características
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
imageResourceGroup=myImageBuilerRGLinux
# Datacenter location - we are using West US 2 in this example
location=WestUS2
# Name for the image - we are using myBuilderImage in this example
imageName=myBuilderImage
# Run output name
runOutputName=aibLinux
```

Cree una variable para el id. de suscripción. Puede obtenerlo mediante `az account show | grep id`.

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

## <a name="download-the-json-example"></a>Descargar el ejemplo .json

Descargue el archivo .json de ejemplo y configúrelo con las variables que ha creado.

```azurecli-interactive
curl https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Linux_Managed_Image/helloImageTemplateLinux.json -o helloImageTemplateLinux.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateLinux.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateLinux.json
sed -i -e "s/<region>/$location/g" helloImageTemplateLinux.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateLinux.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateLinux.json
```

## <a name="create-the-image"></a>Crear la imagen
Envío de la configuración de la imagen al servicio de generador de imágenes de máquina virtual

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateLinux.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01
```

Inicie la generación de imágenes.

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateLinux01 \
     --action Run 
```

Espere hasta que se complete la compilación. Puede tardar unos 15 minutos.


## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree la máquina virtual con la imagen que ha compilado.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name myVM \
  --admin-username azureuser \
  --image $imageName \
  --location $location \
  --generate-ssh-keys
```

Obtenga la dirección IP de la salida de la creación de la máquina virtual y úsela para conectarse mediante SSH a la máquina virtual.

```azurecli-interactive
ssh azureuser@<pubIp>
```

Debería ver que la imagen se ha personalizado con un mensaje del día en cuanto se establece la conexión SSH.

```console

*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

Escriba `exit` cuando haya terminado para cerrar la conexión SSH.

## <a name="check-the-source"></a>Comprobación del origen

En la plantilla de generador de imágenes, en "Propiedades", verá la imagen de origen, el script de personalización que ejecuta y dónde se distribuye.

```azurecli-interactive
cat helloImageTemplateLinux.json
```

Para obtener más información sobre este archivo .json, consulte [Referencia de la plantilla de generador de imágenes](image-builder-json.md).

## <a name="clean-up"></a>Limpieza

Cuando haya terminado, elimine los recursos.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateLinux01

az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los componentes del archivo .json que se usan en este artículo, consulte [Referencia de la plantilla de generador de imágenes](image-builder-json.md).