---
title: Implementar un clúster de Azure Service Fabric en zonas de disponibilidad | Microsoft Docs
description: Obtenga información sobre cómo crear un clúster de Azure Service Fabric a través de las zonas de disponibilidad.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: b664c3d655ab45c89a65a0aea31622f57ddc8d9e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "65077461"
---
# <a name="deploy-an-azure-service-fabric-cluster-across-availability-zones"></a>Implementar un clúster de Azure Service Fabric en zonas de disponibilidad
Las zonas de disponibilidad en Azure es una oferta que protege sus aplicaciones y datos de los errores del centro de datos de alta disponibilidad. Una zona de disponibilidad es una ubicación física única equipada con alimentación independiente, refrigeración y redes dentro de una región de Azure.

Service Fabric admite los clústeres que se distribuyen entre las zonas de disponibilidad mediante la implementación de tipos de nodos que están anclados a zonas específicas. Esto garantizará la alta disponibilidad de sus aplicaciones. Las zonas de disponibilidad de Azure solo están disponibles en regiones seleccionadas. Para obtener más información, consulte [Introducción a las zonas de disponibilidad de Azure](https://docs.microsoft.com/azure/availability-zones/az-overview).

Plantillas de ejemplo están disponibles: [Service Fabric entre la plantilla de zona de disponibilidad](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="recommended-topology-for-primary-node-type-of-azure-service-fabric-clusters-spanning-across-availability-zones"></a>Recomienda la topología para el tipo de nodo principal de clústeres de Azure Service Fabric que abarca las zonas de disponibilidad
Un clúster de Service Fabric que se distribuyen entre las zonas de disponibilidad garantiza la alta disponibilidad del estado del clúster. Para abarcar las zonas de un clúster de Service Fabric, debe crear un tipo de nodo principal en cada zona de disponibilidad admitidos por la región. Esto distribuirá los nodos raíz uniformemente en cada uno de los tipos de nodo principal.

La topología recomendada para el tipo de nodo principal requiere que los recursos que se describen a continuación:

* Establece el nivel de confiabilidad del clúster a Platinum.
* Tres tipos de nodo marcado como principales.
    * Cada tipo de nodo debe asignarse a su propio conjunto de escalado de máquina virtual ubicado en distintas zonas.
    * Cada conjunto de escalado de máquina virtual debe tener al menos cinco nodos (durabilidad Silver).
* Un único recurso de IP pública mediante la SKU estándar.
* Un único recurso de equilibrador de carga mediante la SKU estándar.
* Un NSG que se hace referencia a la subred en la que implementa los conjuntos de escalado de máquinas virtuales.

>[!NOTE]
> El conjunto de escalado de máquinas virtuales propiedad de grupo de selección de ubicación solo debe establecerse en true, ya que Service Fabric no admite un conjunto de escalado de máquina virtual única que abarca las zonas.

 ![Arquitectura de la zona de disponibilidad de Azure Service Fabric][sf-architecture]

## <a name="networking-requirements"></a>Requisitos de red
### <a name="public-ip-and-load-balancer-resource"></a>Dirección IP pública y el recurso de equilibrador de carga
Para habilitar las zonas de propiedad en una escala de máquina virtual establecida recurso, el equilibrador de carga y el recurso IP que se hace referencia a ese conjunto de escalado de máquina virtual dos deben utilizar un *estándar* SKU. Creación de un recurso IP sin la propiedad de SKU o un equilibrador de carga, se creará una SKU básica, que no admite zonas de disponibilidad. Un equilibrador de carga de SKU estándar bloqueará todo el tráfico desde el exterior de forma predeterminada; para permitir el tráfico fuera de, debe implementarse un NSG a la subred.

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat('LB','-', parameters('clusterName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
        "name": "Standard"
    }
}
```

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName')]", 
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', parameters('subnet0Name')))]"
    ],
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
        {
            "name": "[parameters('subnet0Name')]",
            "properties": {
                "addressPrefix": "[parameters('subnet0Prefix')]",
                "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', parameters('subnet0Name')))]"
              }
            }
          }
        ]
    },
    "sku": {
        "name": "Standard"
    }
}
```

>[!NOTE]
> No es posible hacer un cambio en contexto de SKU en los recursos de equilibrador IP y de carga públicos. Si va a migrar desde recursos existentes que tienen una SKU básica, consulte la sección de migración de este artículo.

### <a name="virtual-machine-scale-set-nat-rules"></a>Las reglas NAT del conjunto de escalado de máquina virtual
El equilibrador de carga reglas NAT de entrada deben coincidir con los grupos de NAT desde el conjunto de escalado de máquinas virtuales. Cada conjunto de escalado de máquina virtual debe tener un único grupo NAT entrante.

```json
{
"inboundNatPools": [
    {
        "name": "LoadBalancerBEAddressNatPool0",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "50999",
            "frontendPortRangeStart": "50000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool1",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "51999",
            "frontendPortRangeStart": "51000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool2",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "52999",
            "frontendPortRangeStart": "52000",
            "protocol": "tcp"
        }
    }
    ]
}
```

### <a name="standard-sku-load-balancer-outbound-rules"></a>Reglas de salida estándares de equilibrador de carga de SKU
Estándar de equilibrador de carga y dirección IP pública estándar presentan capacidades nuevas y comportamientos diferentes a la conectividad saliente cuando se usan las SKU básica. Si quiere conectividad saliente al trabajar con las SKU de nivel Estándar, debe definirlas con las direcciones IP públicas estándar o con la instancia pública de Load Balancer estándar. Para obtener más información, consulte [las conexiones salientes](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#snatexhaust) y [Azure Load Balancer estándar](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

>[!NOTE]
> La plantilla estándar hace referencia a un NSG que permite todo el tráfico saliente de forma predeterminada. El tráfico entrante se limita a los puertos necesarios para las operaciones de administración de Service Fabric. Las reglas de NSG pueden modificarse para satisfacer sus necesidades.

### <a name="enabling-zones-on-a-virtual-machine-scale-set"></a>Habilitación de las zonas en una escala de máquina virtual establecido
Para habilitar una zona, en una escala de máquina virtual establecido que debe incluir los tres valores siguientes en el recurso de conjunto de escalado de máquina virtual.

* El primer valor es el **zonas** propiedad, que especifica qué zona de disponibilidad que se va a implementar el conjunto de escalado de máquinas virtuales.
* El segundo valor es la propiedad "singlePlacementGroup", que se debe establecer en true.
* El tercer valor es la propiedad "faultDomainOverride" en la extensión de conjunto de escalado de máquina virtual de Service Fabric. El valor de esta propiedad debe incluir la región y la zona en la que se colocará este conjunto de escalado de máquinas virtuales. Ejemplo: "faultDomainOverride": "eastus/az1" conjunto de escalado de máquinas virtuales de todos los recursos se deben colocar en la misma región porque los clústeres de Azure Service Fabric no es necesario entre la compatibilidad con regiones.

```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "zones": ["1"],
    "properties": {
        "singlePlacementGroup": "true",
    },
    "virtualMachineProfile": {
    "extensionProfile": {
    "extensions": [
    {
    "name": "[concat(parameters('vmNodeType1Name'),'_ServiceFabricNode')]",
    "properties": {
        "type": "ServiceFabricNode",
        "autoUpgradeMinorVersion": false,
        "publisher": "Microsoft.Azure.ServiceFabric.Test",
        "settings": {
            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
            "nodeTypeRef": "[parameters('vmNodeType1Name')]",
            "dataPath": "D:\\\\SvcFab",
            "durabilityLevel": "Silver",
            "certificate": {
                "thumbprint": "[parameters('certificateThumbprint')]",
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
            "systemLogUploadSettings": {
                "Enabled": true
            },
            "faultDomainOverride": "eastus/az1"
        },
        "typeHandlerVersion": "1.0"
    }
}
```

### <a name="enabling-multiple-primary-node-types-in-the-service-fabric-cluster-resource"></a>Habilitación de varios tipos de nodo principal en el recurso de clúster de Service Fabric
Para configurar uno o varios tipos de nodo como principal en un recurso de clúster, establezca la propiedad "isPrimary" en "true". Al implementar un clúster de Service Fabric en zonas de disponibilidad, debe tener tres tipos de nodo en distintas zonas.

```json
{
    "reliabilityLevel": "Platinum",
    "nodeTypes": [
    {
        "name": "[parameters('vmNodeType0Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt0applicationEndPort')]",
            "startPort": "[parameters('nt0applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt0fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt0ephemeralEndPort')]",
            "startPort": "[parameters('nt0ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt0fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType1Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt1applicationEndPort')]",
            "startPort": "[parameters('nt1applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt1fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt1ephemeralEndPort')]",
            "startPort": "[parameters('nt1ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt1fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt1InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType2Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt2applicationEndPort')]",
            "startPort": "[parameters('nt2applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt2ephemeralEndPort')]",
            "startPort": "[parameters('nt2ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt2InstanceCount')]"
    }
    ],
}
```

## <a name="migrate-to-using-availability-zones-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Migrar al uso de las zonas de disponibilidad de un clúster mediante un equilibrador de carga de SKU básica y una dirección IP de SKU básica
Para migrar un clúster, lo que estaba usando un equilibrador de carga y de IP con una SKU básica, primero debe crear un recurso de Load Balancer e IP completamente nuevo mediante la SKU estándar. No es posible actualizar estos recursos in situ.

El equilibrador de carga y la IP nuevo se deben hacer referencia en los nuevos tipos de nodo zona de disponibilidad entre que le gustaría utilizar. En el ejemplo anterior, el escalado de máquinas virtuales nuevas tres recursos de conjunto se agregaron en las zonas 1, 2 y 3. Estos escalado de máquinas virtuales se establece la referencia de la IP y el equilibrador de carga recién creado y están marcados como tipos de nodo principal en el recurso de clúster de Service Fabric.

Para empezar, deberá agregar los nuevos recursos a la plantilla de Resource Manager existente. Estos recursos incluyen:
* Un recurso de IP pública mediante la SKU estándar.
* Un recurso de equilibrador de carga mediante la SKU estándar.
* Un NSG que se hace referencia a la subred en la que implementa los conjuntos de escalado de máquinas virtuales.
* Tres tipos de nodo marcan como principales.
    * Cada tipo de nodo debe asignarse a su propio conjunto de escalado de máquina virtual ubicado en distintas zonas.
    * Cada conjunto de escalado de máquina virtual debe tener al menos cinco nodos (durabilidad Silver).

Puede encontrar un ejemplo de estos recursos en el [plantilla de ejemplo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/10-VM-Ubuntu-2-NodeType-Secure).

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $Template `
    -TemplateParameterFile $Parameters
```

Una vez que han terminado de implementar los recursos, puede empezar a deshabilitar los nodos en el tipo de nodo principal del clúster original. Como los nodos están deshabilitados, los servicios del sistema se migrará al nuevo tipo de nodo principal que había instalado en el paso anterior.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Write-Host "Connected to cluster"

$nodeNames = @("_nt0_0", "_nt0_1", "_nt0_2", "_nt0_3", "_nt0_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames) {
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Una vez que todos los nodos están deshabilitados, los servicios del sistema se ejecutará en el tipo de nodo principal, que se reparte entre zonas. A continuación, puede quitar los nodos deshabilitados del clúster. Una vez que se quitan los nodos, puede quitar la dirección IP original, el equilibrador de carga, y los recursos del conjunto de escalado de máquina virtual.

```powershell
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}

$scaleSetName="nt0"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force

$lbname="LB-cluster-nt0"
$oldPublicIpName="LBIP-cluster-0"
$newPublicIpName="LBIP-cluster-1"

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force
```

A continuación, debe quitar las referencias a estos recursos de la plantilla de Resource Manager que había implementado.

El último paso implica actualizar el nombre DNS y dirección IP pública.

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

```

[sf-architecture]: ./media/service-fabric-cross-availability-zones/sf-cross-az-topology.png
