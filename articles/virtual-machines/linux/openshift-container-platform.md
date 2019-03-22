---
title: Implementación de OpenShift Container Platform en Azure | Microsoft Docs
description: Implemente OpenShift Container Platform en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/04/2018
ms.author: haroldw
ms.openlocfilehash: 1d869d822cdeb0051836a5fc5f01eb69c523f9e3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57995545"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Implementación de OpenShift Container Platform en Azure

Puede usar uno de los diversos métodos para implementar OpenShift Container Platform en Azure:

- Puede implementar manualmente los componentes necesarios de la infraestructura de Azure y, a continuación, seguir la [documentación de OpenShift Container Platform](https://docs.openshift.com/container-platform).
- También puede usar una plantilla existente de [Resource Manager](https://github.com/Microsoft/openshift-container-platform/) que simplifica la implementación del clúster de OpenShift Container Platform.
- Otra opción consiste en usar la [oferta de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

En todos los casos, se requiere una suscripción a Red Hat. Durante la implementación, la instancia de Red Hat Enterprise Linux está registrada en la suscripción de Red Hat y asociada al identificador de grupo que contiene los derechos para OpenShift Container Platform.
Asegúrese de que tiene un nombre de usuario, una contraseña y un identificador de grupo válidos para Red Hat Subscription Manager (RHSM). Puede usar una clave de activación, identificador de organización e identificador de grupo. Puede comprobar esta información iniciando sesión en https://access.redhat.com.

## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>Implementación de la plantilla de Resource Manager para OpenShift Container Platform

Para realizar la implementación mediante la plantilla de Resource Manager, se usa un archivo de parámetros para proporcionar los parámetros de entrada. Para personalizar aún más la implementación, puede bifurcar el repositorio de GitHub y cambiar los elementos correspondientes.

Algunas opciones de personalización comunes son, entre otras:

- Tamaño de máquina virtual de bastión (variable en azuredeploy.json)
- Convenciones de nomenclatura (variables en azuredeploy.json)
- Especificaciones del clúster de OpenShift, modificadas mediante el archivo de hosts (deployOpenShift.sh)

### <a name="configure-the-parameters-file"></a>Configuración del archivo de parámetros

La [plantilla de OpenShift Container Platform](https://github.com/Microsoft/openshift-container-platform) tiene varias ramas disponibles para las diferentes versiones de OpenShift Container Platform.  Según sus necesidades, puede implementar directamente desde el repositorio o puede bifurcar el repositorio y realizar cambios personalizados en las plantillas o los scripts antes de implementar.

Use el valor `appId` de la entidad de servicio que creó anteriormente para el parámetro `aadClientId`.

En el ejemplo siguiente se muestra un archivo de parámetros llamado azuredeploy.parameters.json con todas las entradas necesarias.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_D4s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_D4s_v3"
        },
        "cnsVmSize": {
            "value": "Standard_E4s_v3"
        },
        "osImageType": {
            "value": "defaultgallery"
        },
        "marketplaceOsImage": {
            "value": {
                "publisher": "RedHat",
                "offer": "RHEL",
                "sku": "7-RAW",
                "version": "latest"
            }
        },
        "storageKind": {
            "value": "managed"
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
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "false"
        },
        "enableCNS": {
            "value": "false"
        },
        "rhsmUsernameOrOrgId": {
            "value": "{RHSM Username}"
        },
        "rhsmPasswordOrActivationKey": {
            "value": "{RHSM Password}"
        },
        "rhsmPoolId": {
            "value": "{Pool ID}"
        },
        "rhsmBrokerPoolId": {
            "value": "{Pool ID}"
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
        "enableAzure": {
            "value": "true"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "masterClusterDnsType": {
            "value": "default"
        },
        "masterClusterDns": {
            "value": "console.contoso.com"
        },
        "routingSubDomainType": {
            "value": "nipio"
        },
        "routingSubDomain": {
            "value": "routing.contoso.com"
        },
        "virtualNetworkNewOrExisting": {
            "value": "new"
        },
        "virtualNetworkName": {
            "value": "openshiftvnet"
        },
        "addressPrefixes": {
            "value": "10.0.0.0/14"
        },
        "masterSubnetName": {
            "value": "mastersubnet"
        },
        "masterSubnetPrefix": {
            "value": "10.1.0.0/16"
        },
        "infraSubnetName": {
            "value": "infrasubnet"
        },
        "infraSubnetPrefix": {
            "value": "10.2.0.0/16"
        },
        "nodeSubnetName": {
            "value": "nodesubnet"
        },
        "nodeSubnetPrefix": {
            "value": "10.3.0.0/16"
        },
        "existingMasterSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/mastersubnet"
        },
        "existingInfraSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/masterinfrasubnet"
        },
        "existingCnsSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/cnssubnet"
        },
        "existingNodeSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/nodesubnet"
        },
        "masterClusterType": {
            "value": "public"
        },
        "masterPrivateClusterIp": {
            "value": "10.1.0.200"
        },
        "routerClusterType": {
            "value": "public"
        },
        "routerPrivateClusterIp": {
            "value": "10.2.0.201"
        },
        "routingCertType": {
            "value": "selfsigned"
        },
        "masterCertType": {
            "value": "selfsigned"
        },
        "proxySettings": {
            "value": "none"
        },
        "httpProxyEntry": {
            "value": "none"
        },
        "httpsProxyEntry": {
            "value": "none"
        },
        "noProxyEntry": {
            "value": "none"
        }
    }
}
```

Reemplace los parámetros por su información específica.

Diferentes versiones pueden tener distintos parámetros; por ello, compruebe los parámetros necesarios para la rama que utilice.

### <a name="deploy-using-azure-cli"></a>Implementación con la CLI de Azure

> [!NOTE] 
> El comando siguiente requiere la CLI de Azure 2.0.8 o una versión posterior. Puede comprobar la versión de la CLI con el comando `az --version`. Para actualizar la versión de la CLI, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

En el ejemplo siguiente se implementa el clúster de OpenShift y todos los recursos relacionados en un grupo de recursos llamado openshiftrg, con el nombre de implementación myOpenShiftCluster. Se hace referencia a la plantilla directamente desde el repositorio de GitHub y se usa un archivo de parámetros local denominado azuredeploy.parameters.json.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

La implementación tarda al menos 30 minutos en completarse, según el número total de nodos implementados y las opciones configuradas. El nombre de dominio completo DNS del host de tipo bastión y la dirección URL de la consola de OpenShift se imprimen en el terminal cuando concluye la implementación.

```json
{
  "Bastion DNS FQDN": "bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com",
  "OpenShift Console URL": "http://openshiftlb.eastus.cloudapp.azure.com/console"
}
```

Si no desea que la línea de comandos espere hasta que finalice la implementación, agregue `--no-wait` como una de las opciones para la implementación del grupo. La salida de la implementación se puede recuperar en Azure Portal, en la sección de implementación del grupo de recursos.
 
## <a name="deploy-using-the-openshift-container-platform-azure-marketplace-offer"></a>Implementación con la oferta de Azure Marketplace de OpenShift Container Platform

La manera más sencilla de implementar OpenShift Container Platform en Azure es usar la [oferta de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Se trata de la opción más sencilla, pero también tiene funcionalidades de personalización limitadas. La oferta de Marketplace incluye las siguientes opciones de configuración:

- **Nodos maestros**: tres (3) nodos maestros con tipo de instancia configurable.
- **Nodos de infraestructura**: tres (3) nodos de infraestructura con tipo de instancia configurable.
- **Nodos**: el número de nodos es configurable (entre 2 y 9), así como el tipo de instancia.
- **Tipo de disco**: se usan discos administrados.
- **Redes**: compatibilidad con una red nueva o existente, así como un intervalo CIDR personalizado.
- **CNS**: se puede habilitar CNS.
- **Métricas**: se pueden habilitar métricas.
- **Registro**: se puede habilitar el registro.
- **Proveedor de soluciones en la nube de Azure**: Se puede habilitar.

## <a name="connect-to-the-openshift-cluster"></a>Conexión con el clúster de OpenShift

Cuando finalice la implementación, puede recuperar la conexión de la sección de salida de la implementación. Conéctese a la consola de OpenShift con el explorador con `OpenShift Console URL`. Como alternativa, puede acceder mediante SSH al host de tipo bastión. El siguiente es un ejemplo en el que el nombre de usuario administrador es clusteradmin y el nombre de dominio completo de DNS de la dirección IP pública del host de tipo bastión es bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [az group delete](/cli/azure/group) para quitar el grupo de recursos, el clúster de OpenShift y todos los recursos relacionados.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Pasos siguientes

- [Tareas posteriores a la implementación](./openshift-post-deployment.md)
- [Solución de problemas de implementación de OpenShift en Azure](./openshift-troubleshooting.md)
- [Introducción a OpenShift Container Platform](https://docs.openshift.com/container-platform)

### <a name="documentation-contributors"></a>Colaboradores de documentación

Gracias a Vincent Power (vincepower) y Alfred Sin (asinn826) por sus aportaciones para mantener actualizada esta documentación.
