---
title: Procedimientos recomendados de infraestructura como código de Azure Service Fabric | Microsoft Docs
description: Procedimientos recomendados para la administración de Service Fabric como infraestructura como código.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: jeanpaul.connock
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 23c851008988af06927d387daaa5a6df980dab96
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2019
ms.locfileid: "54914760"
---
# <a name="infrastructure-as-code"></a>Infraestructura como código

En un escenario de producción, cree clústeres de Azure Service Fabric con plantillas de Resource Manager. Las plantillas de Resource Manager proporcionan un mayor control de las propiedades de los recursos y garantizan que disponga de un modelo coherente de recursos.

Las plantillas de ejemplo de Resource Manager están disponibles para Windows y Linux en los [ejemplos de Azure de GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Estas plantillas se pueden usar como punto de partida para crear la plantilla de clúster. Descargue las plantillas `azuredeploy.json` y `azuredeploy.parameters.json` y modifíquelas para satisfacer sus requisitos personalizados.

Para implementar las plantillas `azuredeploy.json` y `azuredeploy.parameters.json` que descargó anteriormente, use los siguientes comandos de la CLI de Azure:

```azurecli
ResourceGroupName="sfclustergroup"
Location="westus"

az group create --name $ResourceGroupName --location $Location 
az group deployment create --name $ResourceGroupName  --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
```

Creación de un recurso con PowerShell

```powershell
$ResourceGroupName="sfclustergroup"
$Location="westus"
$Template="azuredeploy.json"
$Parameters="azuredeploy.parameters.json"

New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
New-AzureRmResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
```

## <a name="azure-service-fabric-resources"></a>Recursos de Azure Service Fabric

Puede implementar aplicaciones y servicios en el clúster de Service Fabric mediante Azure Resource Manager. Consulte [Administración de aplicaciones y servicios como recursos de Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-arm-resource). Los siguientes son recursos específicos de la aplicación Service Fabric de procedimientos recomendados que incluir en los recursos de la plantilla de Resource Manager.

```json
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

Para implementar la aplicación con Azure Resource Manager, primero debe [crear un archivo sfpkg](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg) (paquete de aplicación de Service Fabric). El siguiente script de Python es un ejemplo de cómo crear un sfpkg:

```python
# Create SFPKG that needs to be uploaded to Azure Storage Blob Container
microservices_sfpkg = zipfile.ZipFile(self.microservices_app_package_name, 'w', zipfile.ZIP_DEFLATED)
package_length = len(self.microservices_app_package_path)

for root, dirs, files in os.walk(self.microservices_app_package_path):
    root_folder = root[package_length:]
    for file in files:
        microservices_sfpkg.write(os.path.join(root, file), os.path.join(root_folder, file))

microservices_sfpkg.close()
```

## <a name="next-steps"></a>Pasos siguientes

* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster independiente con Windows Server](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Crear un clúster Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)