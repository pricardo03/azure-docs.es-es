---
title: Implementación de OKD en Azure | Microsoft Docs
description: Implementación de OKD en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: e525bf0a5aa9bda7fdbbcefc4cb5b683c7fabc3b
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426233"
---
# <a name="deploy-okd-in-azure"></a>Implementación de OKD en Azure

Existen dos maneras de implementar OKD (antes OpenShift Origin) en Azure:

- Se pueden implementar manualmente todos los componentes de la infraestructura de Azure necesarios y, después, seguir la [documentación](https://docs.okd.io/3.10/welcome/index.html) de OKD.
- También se puede usar una [plantilla de Resource Manager](https://github.com/Microsoft/openshift-origin) existente, lo que simplifica la implementación del clúster de OKD.

## <a name="deploy-by-using-the-okd-template"></a>Implementación mediante la plantilla de OKD

Use el valor `appId` de la entidad de servicio que creó anteriormente para el parámetro `aadClientId`.

En el ejemplo siguiente se crea un archivo de parámetros llamado azuredeploy.parameters.json con todas las entradas necesarias.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

### <a name="deploy-by-using-azure-cli"></a>Implementación con la CLI de Azure


> [!NOTE] 
> El comando siguiente requiere la CLI de Azure 2.0.8 o una versión posterior. Puede comprobar la versión de la CLI con el comando `az --version`. Para actualizar la versión de la CLI, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

En el ejemplo siguiente se implementa el clúster de OKD y todos los recursos relacionados en un grupo de recursos llamado myResourceGroup con una implementación llamada myOpenShiftCluster. Se hace referencia a la plantilla directamente desde el repositorio de GitHub mediante el uso de un archivo de parámetros local denominado azuredeploy.parameters.json.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

La implementación tarda al menos 25 minutos en completarse, según el número total de nodos implementados. La dirección URL de la consola de OKS y el nombre DNS del maestro de OpenShift se imprime en el terminal cuando concluye la implementación.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-okd-cluster"></a>Conexión al clúster de OKD

Cuando concluya la implementación, conéctese a la consola de OKD con el explorador mediante `OpenShift Console Uri`. Como alternativa, puede conectarse al maestro de OKD con el siguiente comando:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [az group delete](/cli/azure/group#az_group_delete) para quitar el grupo de recursos, el clúster de OpenShift y todos los recursos relacionados.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

- [Tareas posteriores a la implementación](./openshift-post-deployment.md)
- [Solución de problemas de implementación de OpenShift](./openshift-troubleshooting.md)
- [Introducción a OKD](https://docs.okd.io/latest/getting_started/index.html)
