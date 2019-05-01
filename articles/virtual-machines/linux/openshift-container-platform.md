---
title: Implementación de OpenShift Container Platform en Azure | Microsoft Docs
description: Implemente OpenShift Container Platform en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/18/2019
ms.author: haroldw
ms.openlocfilehash: 296bc42313ef80425004d3c9b43c6792cbaf97f4
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718242"
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Implementación de OpenShift Container Platform en Azure

Puede usar uno de los diversos métodos para implementar OpenShift Container Platform en Azure:

- Puede implementar manualmente los componentes necesarios de la infraestructura de Azure y, a continuación, seguir la [documentación de OpenShift Container Platform](https://docs.openshift.com/container-platform).
- También puede usar una plantilla existente de [Resource Manager](https://github.com/Microsoft/openshift-container-platform/) que simplifica la implementación del clúster de OpenShift Container Platform.
- Otra opción consiste en usar la [oferta de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

En todos los casos, se requiere una suscripción a Red Hat. Durante la implementación, la instancia de Red Hat Enterprise Linux está registrada en la suscripción de Red Hat y asociada al identificador de grupo que contiene los derechos para OpenShift Container Platform.
Asegúrese de que tiene un nombre de usuario, una contraseña y un identificador de grupo válidos para Red Hat Subscription Manager (RHSM). Puede usar una clave de activación, identificador de organización e identificador de grupo. Puede comprobar esta información iniciando sesión en https://access.redhat.com.


## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>Implementación de la plantilla de Resource Manager para OpenShift Container Platform

### <a name="private-clusters"></a>Clústeres privada

Implementación de clústeres de OpenShift privados requiere más de no tener una dirección IP pública asociada al equilibrador de carga principal (consola web) o a la infraestructura (enrutador) de equilibrador de carga.  Un clúster privado generalmente usa un servidor DNS personalizado (no Azure DNS predeterminado), un nombre de dominio personalizado (por ejemplo, contoso.com) y predefinidas de las redes virtuales.  Para los clústeres privados, deberá configurar la red virtual con todas las subredes adecuadas y la configuración del servidor DNS de antemano.  A continuación, usar **existingMasterSubnetReference**, **existingInfraSubnetReference**, **existingCnsSubnetReference**, y  **existingNodeSubnetReference** para especificar la subred existente para su uso por el clúster.

Si se selecciona master privada (**masterClusterType**= privada), debe especificarse para una dirección IP privada estática **masterPrivateClusterIp**.  Esta dirección IP se asignará para el front-end del equilibrador de carga principal.  La dirección IP debe estar dentro del CIDR para la subred de maestra y no está en uso.  **masterClusterDnsType** debe establecerse en "custom" y el patrón de nombre DNS debe proporcionarse para **masterClusterDns**.  El nombre DNS debe asignar a la IP privada estática y se usará para acceder a la consola en los nodos maestros.

Si se selecciona el enrutador privado (**routerClusterType**= privada), debe especificarse para una dirección IP privada estática **routerPrivateClusterIp**.  Esta dirección IP se asignará a la parte frontal de la infraestructura equilibrador de carga.  La dirección IP debe estar dentro del CIDR para la infraestructura subred y no esté en uso.  **routingSubDomainType** debe establecerse en "custom" y el nombre DNS con caracteres comodín para el enrutamiento se debe proporcionar para **routingSubDomain**.  

Si se seleccionan patrones privadas y enrutador privado, también debe especificarse el nombre de dominio personalizado para **domainName**

Tras una implementación correcta, el nodo bastión es el único nodo con una dirección IP pública que puede acceder mediante ssh en.  Incluso si los nodos principales están configurados para acceso público, no se exponen para ssh acceso.

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
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "_artifactsLocation": {
            "value": "https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master"
        },
        "location": {
            "value": "eastus"
        },
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
            "value": "changeme"
        },
        "openshiftClusterPrefix": {
            "value": "changeme"
        },
        "minorVersion": {
            "value": "69"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 3
        },
        "nodeInstanceCount": {
            "value": 3
        },
        "cnsInstanceCount": {
            "value": 3
        },
        "osDiskSize": {
            "value": 64
        },
        "dataDiskSize": {
            "value": 64
        },
        "cnsGlusterDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "changeme"
        },
        "enableMetrics": {
            "value": "false"
        },
        "enableLogging": {
            "value": "false"
        },
        "enableCNS": {
            "value": "false"
        },
        "rhsmUsernameOrOrgId": {
            "value": "changeme"
        },
        "rhsmPoolId": {
            "value": "changeme"
        },
        "rhsmBrokerPoolId": {
            "value": "changeme"
        },
        "sshPublicKey": {
            "value": "GEN-SSH-PUB-KEY"
        },
        "keyVaultSubscriptionId": {
            "value": "255a325e-8276-4ada-af8f-33af5658eb34"
        },
        "keyVaultResourceGroup": {
            "value": "changeme"
        },
        "keyVaultName": {
            "value": "changeme"
        },
        "enableAzure": {
            "value": "true"
        },
        "aadClientId": {
            "value": "changeme"
        },
        "domainName": {
            "value": "contoso.com"
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
            "value": "apps.contoso.com"
        },
        "virtualNetworkNewOrExisting": {
            "value": "new"
        },
        "virtualNetworkName": {
            "value": "changeme"
        },
        "addressPrefixes": {
            "value": "10.0.0.0/14"
        },
        "masterSubnetName": {
            "value": "changeme"
        },
        "masterSubnetPrefix": {
            "value": "10.1.0.0/16"
        },
        "infraSubnetName": {
            "value": "changeme"
        },
        "infraSubnetPrefix": {
            "value": "10.2.0.0/16"
        },
        "nodeSubnetName": {
            "value": "changeme"
        },
        "nodeSubnetPrefix": {
            "value": "10.3.0.0/16"
        },
        "existingMasterSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/mastersubnet"
        },
        "existingInfraSubnetReference": {
            "value": "/subscriptions/abc686f6-963b-4e64-bff4-99dc369ab1cd/resourceGroups/vnetresourcegroup/providers/Microsoft.Network/virtualNetworks/openshiftvnet/subnets/infrasubnet"
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
            "value": "10.2.0.200"
        },
        "routingCertType": {
            "value": "selfsigned"
        },
        "masterCertType": {
            "value": "selfsigned"
        }
    }
}
```

Reemplace los parámetros por su información específica.

Diferentes versiones pueden tener distintos parámetros; por ello, compruebe los parámetros necesarios para la rama que utilice.

### <a name="azuredeployparametersjson-file-explained"></a>azuredeploy. Explicado del archivo Parameters.JSON

| Propiedad | DESCRIPCIÓN | Opciones válidas | Valor predeterminado |
|----------|-------------|---------------|---------------|
| `_artifactsLocation`  | Dirección URL para los artefactos (json, scripts, etcetera.) |  |  https:\//raw.githubusercontent.com/Microsoft/openshift-container-platform/master  |
| `location` | Región de Azure para implementar los recursos |  |  |
| `masterVmSize` | Tamaño de la máquina virtual maestra. Seleccione uno de los tamaños permitidos de máquina virtual aparece en el archivo azuredeploy.json |  | Standard_E2s_v3 |
| `infraVmSize` | Tamaño de la infraestructura máquina virtual. Seleccione uno de los tamaños permitidos de máquina virtual aparece en el archivo azuredeploy.json |  | Standard_D4s_v3 |
| `nodeVmSize` | Tamaño de la máquina virtual del nodo de aplicación. Seleccione uno de los tamaños permitidos de máquina virtual aparece en el archivo azuredeploy.json |  | Standard_D4s_v3 |
| `cnsVmSize` | Tamaño de la máquina virtual del nodo de almacenamiento nativo (CNS) de contenedor. Seleccione uno de los tamaños permitidos de máquina virtual aparece en el archivo azuredeploy.json |  | Standard_E4s_v3 |
| `osImageType` | La imagen RHEL que se usará. defaultgallery: Petición; Marketplace: imagen de terceros | defaultgallery <br> marketplace | defaultgallery |
| `marketplaceOsImage` | Si `osImageType` es marketplace, a continuación, especifique los valores adecuados para 'publisher', 'ofrecen', 'sku', 'version' de la oferta de marketplace. Este parámetro es un tipo de objeto |  |  |
| `storageKind` | El tipo de almacenamiento que se usará  | administrado<br> no administrado | administrado |
| `openshiftClusterPrefix` | Prefijo que se usa para configurar los nombres de host para todos los nodos del clúster.  Entre 1 y 20 caracteres |  | mycluster |
| `minoVersion` | La versión secundaria de OpenShift Container Platform 3.11 para implementar |  | 69 |
| `masterInstanceCount` | Número de nodos maestros para implementar | 1, 3, 5 | 3 |
| `infraInstanceCount` | Número de infraestructura para implementar los nodos | 1, 2, 3 | 3 |
| `nodeInstanceCount` | Número de nodos que se va a implementar | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30 | 2 |
| `cnsInstanceCount` | Número de nodos de CNS para implementar | 3, 4 | 3 |
| `osDiskSize` | Tamaño del disco de sistema operativo para la máquina virtual (en GB) | 64, 128, 256, 512, 1024, 2048 | 64 |
| `dataDiskSize` | Tamaño del disco de datos para adjuntar a los nodos para el volumen de Docker (en GB) | 32, 64, 128, 256, 512, 1024, 2048 | 64 |
| `cnsGlusterDiskSize` | Tamaño del disco de datos para adjuntar a los nodos de CNS para su uso por glusterfs (en GB | 32, 64, 128, 256, 512, 1024, 2048 | 128 |
| `adminUsername` | Nombre de usuario de administrador para el inicio de sesión del sistema operativo (VM) y de usuario inicial de OpenShift |  | ocpadmin |
| `enableMetrics` | Habilitar las métricas. Las métricas requieren más recursos, así que seleccione un tamaño adecuado para la infraestructura de máquina virtual | true <br> false | false |
| `enableLogging` | Habilitar el registro. elasticsearch pod requiere 8 GB de RAM, así que seleccione un tamaño adecuado para la infraestructura de máquina virtual | true <br> false | false |
| `enableCNS` | Habilitar el almacenamiento nativo de contenedor | true <br> false | false |
| `rhsmUsernameOrOrgId` | Id. de nombre de usuario de administrador de suscripciones Hat u organización rojo |  |  |
| `rhsmPoolId` | Red Hat Subscription Manager identificador de grupo que contiene los derechos de OpenShift para nodos de proceso |  |  |
| `rhsmBrokerPoolId` | Red Hat Subscription Manager identificador de grupo que contiene los derechos de OpenShift para maestros y de infra nodos. Si no tiene los identificadores de otro grupo, escriba el mismo Id. de grupo como 'rhsmPoolId' |  |
| `sshPublicKey` | Copie la clave pública SSH aquí |  |  |
| `keyVaultSubscriptionId` | El identificador de suscripción de la suscripción que contiene el almacén de claves |  |  |
| `keyVaultResourceGroup` | El nombre del grupo de recursos que contiene el almacén de claves |  |  |
| `keyVaultName` | El nombre del que creó el almacén de claves |  |  |
| `enableAzure` | Habilitar a proveedor de nube de Azure | true <br> false | true |
| `aadClientId` | Azure Active Directory Id. de cliente también se conoce como identificador de la aplicación para la entidad de servicio |  |  |
| `domainName` | Nombre del nombre de dominio personalizado para usar (si corresponde). Establecido en "none" Si no implementar clúster totalmente privado |  | None |
| `masterClusterDnsType` | Tipo de dominio para la consola web de OpenShift. 'default' usará la etiqueta DNS del maestro de infraestructura IP pública. 'custom' permite definir su propio nombre | default <br> personalizado | default |
| `masterClusterDns` | El nombre DNS personalizado para tener acceso a la consola de OpenShift web si ha seleccionado "personalizada" para `masterClusterDnsType` |  | console.contoso.com |
| `routingSubDomainType` | Si establece en 'nipio', `routingSubDomain` usará nip.io.  Use 'custom' Si tiene su propio dominio que desea utilizar para el enrutamiento | nipio <br> personalizado | nipio |
| `routingSubDomain` | El nombre DNS de carácter comodín que desea usar para el enrutamiento si ha seleccionado "personalizada" para `routingSubDomainType` |  | apps.contoso.com |
| `virtualNetworkNewOrExisting` | Seleccione si desea usar una red Virtual existente o crear una nueva red Virtual | Existente <br> new | new |
| `virtualNetworkResourceGroupName` | Nombre del grupo de recursos para la nueva red Virtual si ha seleccionado 'new' para `virtualNetworkNewOrExisting` |  | resourceGroup().name |
| `virtualNetworkName` | El nombre de la nueva red Virtual para crear si ha seleccionado 'new' para `virtualNetworkNewOrExisting` |  | openshiftvnet |
| `addressPrefixes` | Prefijo de dirección de la nueva red virtual |  | 10.0.0.0/14 |
| `masterSubnetName` | El nombre de la subred principal |  | mastersubnet |
| `masterSubnetPrefix` | Debe ser un subconjunto de lo addressPrefix CIDR utilizado para la subred maestra: |  | 10.1.0.0/16 |
| `infraSubnetName` | El nombre de la infraestructura subred |  | infrasubnet |
| `infraSubnetPrefix` | CIDR utilizado para la infraestructura debe ser un subconjunto de lo addressPrefix subred: |  | 10.2.0.0/16 |
| `nodeSubnetName` | El nombre de la subred de nodo |  | nodesubnet |
| `nodeSubnetPrefix` | Debe ser un subconjunto de lo addressPrefix CIDR utilizado para la subred de nodo: |  | 10.3.0.0/16 |
| `existingMasterSubnetReference` | Referencia completa a la subred existente para los nodos principales. No es necesario si crear nueva red virtual / subred |  |  |
| `existingInfraSubnetReference` | Referencia completa a una subred existente para infraestructura nodos. No es necesario si crear nueva red virtual / subred |  |  |
| `existingCnsSubnetReference` | Referencia completa a la subred existente para los nodos de CNS. No es necesario si crear nueva red virtual / subred |  |  |
| `existingNodeSubnetReference` | Referencia completa a la subred existente para los nodos de proceso. No es necesario si crear nueva red virtual / subred |  |  |
| `masterClusterType` | Especifique si el clúster usa nodos maestros públicos o privados. Si se elige privada, los nodos principales no exponerse a Internet a través de una dirección IP pública. En su lugar, usará la dirección IP privada que especificó en el `masterPrivateClusterIp` | público <br> privado | público |
| `masterPrivateClusterIp` | Si se seleccionan los nodos maestros privados, a continuación, una dirección IP privada debe especificarse para su uso por el equilibrador de carga interno para los nodos principales. Esta dirección IP estática debe estar dentro del bloque CIDR para la subred de maestra y ya no está en uso. Si se seleccionan los nodos maestros públicos, este valor no se utilizará, aunque todavía se debe especificar |  | 10.1.0.200 |
| `routerClusterType` | Especifique si el clúster usa privada o pública de infraestructura nodos. Si se elige privada, la infraestructura, los nodos no se exponen a Internet a través de una dirección IP pública. En su lugar, usará la dirección IP privada que especificó en el `routerPrivateClusterIp` | público <br> privado | público |
| `routerPrivateClusterIp` | Si es privada infra se seleccionan los nodos y, después, se debe especificar una dirección IP privada para usar el equilibrador de carga interno para infraestructura nodos. Esta dirección IP estática debe estar dentro del bloque CIDR para la subred de maestra y ya no está en uso. Si es público infra se seleccionan los nodos, este valor no se usarán pero todavía se debe especificar |  | 10.2.0.200 |
| `routingCertType` | Usar certificado personalizado para un dominio de enrutamiento o el certificado autofirmado predeterminado: siga las instrucciones de **certificados personalizados** sección | selfsigned <br> personalizado | selfsigned |
| `masterCertType` | Usar certificado personalizado para el dominio principal o el certificado autofirmado predeterminado: siga las instrucciones de **certificados personalizados** sección | selfsigned <br> personalizado | selfsigned |

<br>

### <a name="deploy-using-azure-cli"></a>Implementación con la CLI de Azure

> [!NOTE] 
> El comando siguiente requiere la CLI de Azure 2.0.8 o una versión posterior. Puede comprobar la versión de la CLI con el comando `az --version`. Para actualizar la versión de la CLI, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latesti).

En el ejemplo siguiente se implementa el clúster de OpenShift y todos los recursos relacionados en un grupo de recursos llamado openshiftrg, con el nombre de implementación myOpenShiftCluster. Se hace referencia a la plantilla directamente desde el repositorio de GitHub y se usa un archivo de parámetros local denominado azuredeploy.parameters.json.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

La implementación tarda al menos 60 minutos en completarse, según el número total de nodos implementados y las opciones configuradas. El nombre de dominio completo DNS del host de tipo bastión y la dirección URL de la consola de OpenShift se imprimen en el terminal cuando concluye la implementación.

```json
{
  "Bastion DNS FQDN": "bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com",
  "OpenShift Console URL": "http://openshiftlb.eastus.cloudapp.azure.com/console"
}
```

Si no desea que la línea de comandos espere hasta que finalice la implementación, agregue `--no-wait` como una de las opciones para la implementación del grupo. La salida de la implementación se puede recuperar en Azure Portal, en la sección de implementación del grupo de recursos.

## <a name="connect-to-the-openshift-cluster"></a>Conexión con el clúster de OpenShift

Cuando finalice la implementación, puede recuperar la conexión de la sección de salida de la implementación. Conectarse a la consola de OpenShift con el explorador mediante la **dirección URL de la consola de OpenShift**. También puede acceder mediante SSH al host bastión. El siguiente es un ejemplo en el que el nombre de usuario administrador es clusteradmin y el nombre de dominio completo de DNS de la dirección IP pública del host de tipo bastión es bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com:

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
