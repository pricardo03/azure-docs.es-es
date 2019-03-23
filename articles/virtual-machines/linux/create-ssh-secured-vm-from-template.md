---
title: Creación de una máquina virtual Linux en Azure a partir de una plantilla | Microsoft Docs
description: En este artículo se describe cómo usar la CLI de Azure para crear una máquina virtual Linux a partir de una plantilla de Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 334f69390e4506c6db76c1814f8ec8f1e4417ee9
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372342"
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Procedimiento para crear una máquina virtual Linux con plantillas de Azure Resource Manager

Obtenga información sobre cómo crear una máquina virtual (VM) de Linux mediante una plantilla de Azure Resource Manager y la CLI de Azure desde Azure Cloud shell. Para crear una máquina virtual de Windows, consulte [crear una máquina virtual de Windows desde una plantilla de Resource Manager](../windows/ps-template.md).

## <a name="templates-overview"></a>Introducción a las plantillas

Las plantillas de Azure Resource Manager son archivos JSON que definen la infraestructura y la configuración de la solución de Azure. Mediante una plantilla, puede implementar la solución repetidamente a lo largo del ciclo de vida y tener la seguridad de que los recursos se implementan de forma coherente. Para obtener más información sobre el formato de la plantilla y cómo se crea, consulte [inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md). Para ver la sintaxis JSON para los tipos de recursos, consulte [Define resources in Azure Resource Manager templates](/azure/templates/microsoft.compute/allversions) (Definición de recursos en plantillas de Azure Resource Manager).

## <a name="create-a-virtual-machine"></a>de una máquina virtual

Normalmente, la creación de una máquina virtual de Azure incluye dos pasos:

1. Cree un grupo de recursos. Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Se debe crear un grupo de recursos antes de una máquina virtual.
1. Cree una máquina virtual.

En el ejemplo siguiente se crea una máquina virtual desde un [plantilla de inicio rápido de Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Autenticación de SSH solo se permite para esta implementación. Cuando se le solicite, proporcione el valor de su propia clave pública SSH, por ejemplo, el contenido de *~/.ssh/id_rsa.pub*. Si necesita crear un par de claves SSH, consulte el artículo sobre cómo [crear y usar un par de claves SSH pública y privada para máquinas virtuales Linux en Azure](mac-create-ssh-keys.md). Esta es una copia de la plantilla:

[!code-json[create-linux-vm](~/quickstart-templates/101-vm-sshkey/azuredeploy.json)]

Para ejecutar el script CLI, seleccione **Pruébelo** para abrir Azure Cloud shell. Para pegar el script, haga clic en el shell y, a continuación, seleccione **pegar**:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
echo "Enter the project name (used for generating resource names):" &&
read projectName &&
echo "Enter the administrator username:" &&
read username &&
echo "Enter the SSH public key:" &&
read key &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json --parameters projectName=$projectName adminUsername=$username adminPublicKey='$key' &&
az vm show --resource-group $resourceGroupName --name "$projectName-vm" --show-details --query publicIps --output tsv
```

El último comando de CLI de Azure muestra la dirección IP pública de la máquina virtual recién creada. Necesita la dirección IP pública para conectarse a la máquina virtual. Consulte la sección siguiente de este artículo.

En el ejemplo anterior, especificó una plantilla almacenada en GitHub. También puede descargar o crear una plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

Estos son algunos recursos adicionales:

- Para obtener información sobre cómo desarrollar plantillas de Resource Manager, consulte [documentación de Azure Resource Manager](/azure/azure-resource-manager/).
- Para ver los esquemas de la máquina virtual de Azure, consulte [referencia de plantilla de Azure](/azure/templates/microsoft.compute/allversions).
- Para ver más ejemplos de plantillas de máquina virtual, consulte [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

## <a name="connect-to-virtual-machine"></a>Conexión a la máquina virtual

Después, puede acceder mediante SSH a la máquina virtual de la forma habitual. Proporcione su propia dirección IP pública obtenida con el comando anterior:

```bash
ssh <adminUsername>@<ipAddress>
```

## <a name="next-steps"></a>Pasos siguientes

En este ejemplo, creó una máquina virtual Linux básica. Para obtener más plantillas de Resource Manager que incluyen marcos de aplicaciones o creación entornos más complejos, examinar el [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

Para más información sobre cómo crear plantillas, vea las propiedades y la sintaxis de JSON para los tipos de recursos que ha implementado:

- [Microsoft.Network/networkSecurityGroups](/azure/templates/microsoft.network/networksecuritygroups)
- [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
