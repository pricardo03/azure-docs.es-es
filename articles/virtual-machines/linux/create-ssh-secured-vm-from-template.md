---
title: Creación de una máquina virtual Linux en Azure a partir de una plantilla | Microsoft Docs
description: En este artículo se describe cómo usar la CLI de Azure para crear una máquina virtual Linux a partir de una plantilla de Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/30/2018
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc2b4dde9796336112d6c8a68d16d0b3006b3fee
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936421"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Procedimiento para crear una máquina virtual Linux con plantillas de Azure Resource Manager
En este artículo se muestra cómo implementar rápidamente una máquina virtual Linux con plantillas de Azure Resource Manager y la CLI de Azure. 


## <a name="templates-overview"></a>Introducción a las plantillas
Las plantillas de Azure Resource Manager son archivos JSON que definen la infraestructura y la configuración de la solución de Azure. Mediante una plantilla, puede implementar la solución repetidamente a lo largo del ciclo de vida y tener la seguridad de que los recursos se implementan de forma coherente. Para obtener más información sobre el formato de la plantilla y cómo se crea, consulte [Creación de la primera plantilla de Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md). Para ver la sintaxis JSON para los tipos de recursos, consulte [Define resources in Azure Resource Manager templates](/azure/templates/) (Definición de recursos en plantillas de Azure Resource Manager).


## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Se debe crear un grupo de recursos antes de una máquina virtual. En el ejemplo siguiente, se crea un grupo de recursos denominado "*myResourceGroup*" en la ubicación *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>de una máquina virtual
En el ejemplo siguiente se crea una máquina virtual a partir de [esta plantilla de Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) con [az group deployment create](/cli/azure/group/deployment#az_group_deployment_create). Se permite solo la autenticación de SSH. Cuando se le solicite, proporcione el valor de su propia clave pública SSH, por ejemplo, el contenido de *~/.ssh/id_rsa.pub*. Si necesita crear un par de claves SSH, consulte el artículo sobre cómo [crear y usar un par de claves SSH pública y privada para máquinas virtuales Linux en Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

En el ejemplo anterior, especificó una plantilla almacenada en GitHub. También puede descargar o crear una plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.


## <a name="connect-to-virtual-machine"></a>Conexión a la máquina virtual
Para acceder mediante SSH a la máquina virtual, obtenga la dirección IP pública con [az vm show](/cli/azure/vm#az-vm-show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

Después, puede acceder mediante SSH a la máquina virtual de la forma habitual. Proporcione su propia dirección IP pública obtenida con el comando anterior:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Pasos siguientes
En este ejemplo, creó una máquina virtual Linux básica. Para obtener más plantillas de Resource Manager que incluyen marcos de aplicaciones o crear entornos más complejos, examine la [Galería de plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/).