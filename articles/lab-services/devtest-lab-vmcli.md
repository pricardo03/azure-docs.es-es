---
title: Creación y administración de máquinas virtuales en DevTest Labs con la CLI de Azure | Microsoft Docs
description: Aprenda a usar Azure DevTest Labs para crear y administrar máquinas virtuales con la CLI de Azure
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: 48a30ef86cdb10b540ffe1231294542ccff87255
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795943"
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a>Creación y administración de máquinas virtuales con DevTest Labs mediante la CLI de Azure
Este inicio rápido le ayudará a crear, iniciar, conectarse, actualizar y limpiar una máquina de desarrollo en su laboratorio. 

Antes de empezar:

* Si no se ha creado un laboratorio, encontrará instrucciones [aquí](devtest-lab-create-lab.md).

* [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). Para empezar, ejecute az login para crear una conexión con Azure. 

## <a name="create-and-verify-the-virtual-machine"></a>Creación y comprobación de la máquina virtual 
Antes de ejecutar los comandos relacionados de DevTest Labs, establecer el contexto de Azure adecuado mediante la `az account set` comando:

```azurecli
az account set --subscription 11111111-1111-1111-1111-111111111111
```

El comando para crear una máquina virtual es: `az lab vm create`. El grupo de recursos para el laboratorio, el nombre de laboratorio y el nombre de máquina virtual son todo lo necesario. El resto de los argumentos cambian según el tipo de máquina virtual.

El siguiente comando crea una imagen basada en Windows desde Azure Marketplace. El nombre de la imagen es la misma que verían al crear una máquina virtual mediante el portal de Azure. 

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "Visual Studio Community 2017 on Windows Server 2016 (x64)" --image-type gallery --size 'Standard_D2s_v3’ --admin-username 'AdminUser' --admin-password 'Password1!'
```

El siguiente comando crea una máquina virtual basada en una imagen personalizada disponible en el laboratorio:

```azurecli
az lab vm create --resource-group DtlResourceGroup --lab-name MyLab --name 'MyTestVm' --image "My Custom Image" --image-type custom --size 'Standard_D2s_v3' --admin-username 'AdminUser' --admin-password 'Password1!'
```

El **tipo de imagen** argumento ha cambiado de **galería** a **personalizado**. El nombre de la imagen coincida con lo que se ve si fuera a crear la máquina virtual en Azure portal.

El siguiente comando crea una máquina virtual desde una imagen de marketplace con ssh autenticación:

```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```

También puede crear máquinas virtuales basadas en las fórmulas estableciendo el **tipo de imagen** parámetro **fórmula**. Si tiene que elegir una red virtual específica para la máquina virtual, use el **nombre de red virtual** y **subred** parámetros. Para obtener más información, consulte [crear az lab vm](/cli/azure/lab/vm#az-lab-vm-create).

## <a name="verify-that-the-vm-is-available"></a>Compruebe que la máquina virtual esté disponible.
Use el `az lab vm show` comando para comprobar que la máquina virtual está disponible antes de iniciar y conectarse a él. 

```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-to-the-virtual-machine"></a>Inicio y conexión a la máquina virtual
El siguiente comando de ejemplo inicia una máquina virtual:

```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```

Conectarse a una máquina virtual: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) o [escritorio remoto](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a>Actualización de la máquina virtual
El comando de ejemplo siguiente aplica a los artefactos a una máquina virtual:

```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

Muestre los artefactos disponibles en el laboratorio.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a>Detención y eliminación de la máquina virtual    
El siguiente comando de ejemplo detiene una máquina virtual.

```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Elimine una máquina virtual.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes
Consulte el siguiente contenido: [Documentación de la CLI de Azure para Azure DevTest Labs](/cli/azure/lab?view=azure-cli-latest). 