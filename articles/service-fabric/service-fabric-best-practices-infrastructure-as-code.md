---
title: Procedimientos recomendados de infraestructura como código de Azure Service Fabric | Microsoft Docs
description: Procedimientos recomendados para la administración de Service Fabric como infraestructura como código.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 2dfe1493c6611fb69a417895aaa1028ad5881b9c
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237432"
---
# <a name="infrastructure-as-code"></a>Infraestructura como código

En un escenario de producción, cree clústeres de Azure Service Fabric con plantillas de Resource Manager. Las plantillas de Resource Manager proporcionan un mayor control de las propiedades de los recursos y garantizan que disponga de un modelo coherente de recursos.

Las plantillas de ejemplo de Resource Manager están disponibles para Windows y Linux en los [ejemplos de Azure de GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Estas plantillas se pueden usar como punto de partida para crear la plantilla de clúster. Descargue las plantillas `azuredeploy.json` y `azuredeploy.parameters.json` y modifíquelas para satisfacer sus requisitos personalizados.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

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

New-AzResourceGroup -Name $ResourceGroupName -Location $Location
New-AzResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
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

## <a name="azure-virtual-machine-operating-system-automatic-upgrade-configuration"></a>Configuración de actualización automática de máquina Virtual de Azure del sistema operativo 
Actualizar las máquinas virtuales es una operación iniciada por el usuario y se recomienda que utilice [actualización de máquina Virtual escala establecer automáticas del sistema operativo](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) para administración de revisiones de host; los clústeres de Azure Service Fabric Aplicación de orquestación de revisiones es una solución alternativa que está pensada para cuando hospedados fuera de Azure, aunque se puede usar POA en Azure, con una sobrecarga de hospedaje POA en que se va a un motivo habitual para preferir la actualización automática del sistema operativo de máquina Virtual de Azure a través de POA. Estas son las propiedades de plantilla de proceso escala de máquina Virtual establecido el Administrador de recursos para habilitar la actualización automática del sistema operativo:

```json
"upgradePolicy": {
   "mode": "Automatic",
   "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade": true,
        "disableAutomaticRollback": false
    }
},
```
Al usar las actualizaciones automáticas del sistema operativo con Service Fabric, la nueva imagen de sistema operativo se implanta un dominio de actualización en un momento para mantener una alta disponibilidad de los servicios que se ejecutan en Service Fabric. Para utilizar las actualizaciones automáticas del sistema operativo en Service Fabric, el clúster debe estar configurado para utilizar el nivel de durabilidad Silver o superior.

Asegúrese de que la siguiente clave del registro se establece en false para impedir que actualizaciones coordinadas de iniciar las máquinas host de windows: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU.

Estas son las propiedades de plantilla de proceso escala de máquina Virtual establecido el Administrador de recursos para establecer la clave del registro de Windows Update en false:
```json
"osProfile": {
        "computerNamePrefix": "{vmss-name}",
        "adminUsername": "{your-username}",
        "secrets": [],
        "windowsConfiguration": {
          "provisionVMAgent": true,
          "enableAutomaticUpdates": false
        }
      },
```

## <a name="azure-service-fabric-cluster-upgrade-configuration"></a>Configuración de actualización de Azure Service Fabric Cluster
El siguiente es la propiedad de la plantilla de Resource Manager para habilitar la actualización automática del clúster de Service Fabric:
```json
"upgradeMode": "Automatic",
```
Para actualizar manualmente el clúster, descargue la distribución de cab/deb a una máquina virtual de clúster y, a continuación, invocar el siguiente comando de PowerShell:
```powershell
Copy-ServiceFabricClusterPackage -Code -CodePackagePath <"local_VM_path_to_msi"> -CodePackagePathInImageStore ServiceFabric.msi -ImageStoreConnectionString "fabric:ImageStore"
Register-ServiceFabricClusterPackage -Code -CodePackagePath "ServiceFabric.msi"
Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <"msi_code_version">
```

## <a name="next-steps"></a>Pasos siguientes

* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster independiente con Windows Server](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Crear un clúster Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)
