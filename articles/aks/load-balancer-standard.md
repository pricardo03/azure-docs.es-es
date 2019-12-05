---
title: Uso de un equilibrador de carga de SKU estándar en Azure Kubernetes Service (AKS)
description: Aprenda a usar un equilibrador de carga con una SKU estándar para exponer los servicios con Azure Kubernetes Service (AKS).
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 09/27/2019
ms.author: zarhoads
ms.openlocfilehash: ef826239bc916b4ccf25785f92397286017d00f7
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74171399"
---
# <a name="use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Uso de un equilibrador de carga de SKU estándar en Azure Kubernetes Service (AKS)

Para proporcionar acceso a las aplicaciones a través de servicios Kubernetes de tipo `LoadBalancer` en Azure Kubernetes Service (AKS), puede usar un equilibrador de carga de Azure. Los equilibradores de carga que se ejecutan en AKS pueden usarse como un equilibrador de carga interno o externo. Un equilibrador de carga interno permite que un servicio de Kubernetes sea accesible solo para las aplicaciones que se ejecutan en la misma red virtual que el clúster de AKS. Un equilibrador de carga externo recibe una o más direcciones IP públicas de entrada y permite que un servicio de Kubernetes sea accesible externamente mediante las direcciones IP públicas.

Azure Load Balancer está disponible en dos SKU: *Básica* y *Estándar*. De forma predeterminada, la SKU *Estándar* se usa al crear un clúster de AKS. Con la SKU *Estándar*, el equilibrador de carga proporciona características y funcionalidades adicionales, como un tamaño mayor de grupo back-end y Availability Zones. Es importante comprender las diferencias entre los equilibradores de carga *estándar* y *básicos* antes de decidir cuál usar. Una vez creado el clúster de AKS, no se puede cambiar la SKU del equilibrador de carga para ese clúster. Para más información sobre las SKU *básicas* y *estándar*, consulte [Comparación de las SKU de Load Balancer][azure-lb-comparison].

Se presupone que tiene un conocimiento básico de los conceptos de Kubernetes y Azure Load Balancer. Para obtener más información, consulte [Conceptos básicos de Kubernetes de Azure Kubernetes Service (AKS)][kubernetes-concepts] y [¿Qué es Azure Load Balancer?][azure-lb].

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0.74 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][install-azure-cli].

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se da por supuesto que tiene un clúster de AKS con la SKU *Estándar* de Azure Load Balancer. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

Si usa una subred o un grupo de recursos existentes, la entidad de servicio del clúster de AKS también necesita permiso para administrar los recursos de red. En general, asigne el rol *Colaborador de red* a la entidad de servicio en los recursos delegados. Para más información sobre los permisos, consulte [Delegación del acceso de AKS a otros recursos de Azure][aks-sp].

### <a name="moving-from-a-basic-sku-load-balancer-to-standard-sku"></a>Traslado de un equilibrador de carga de la SKU básica a la SKU estándar

Si tiene un clúster existente con el equilibrador de carga de SKU básica, existen diferencias de comportamiento importantes que se deben tener en cuenta al migrar para usar un clúster con la SKU estándar.

Por ejemplo, la realización de implementaciones Blue-Green para migrar clústeres es una práctica común, dado que el tipo `load-balancer-sku` de un clúster solo se puede definir en el momento de la creación del clúster. Sin embargo, los equilibradores de carga de *SKU básica* usan direcciones IP de *SKU básica* que no son compatibles con los equilibradores de carga de *SKU estándar*, que requieren direcciones IP de *SKU estándar*. Al migrar clústeres para actualizar las SKU del equilibrador de carga, se necesitará una nueva dirección IP con una SKU de dirección IP compatible.

Para conocer más detalles sobre cómo migrar clústeres, consulte [nuestra documentación con consideraciones sobre la migración](acs-aks-migration.md) para ver una lista de temas importantes que se deben tener en cuenta al realizar una migración. Las siguientes limitaciones son también diferencias de comportamiento importantes que se deben tener en cuenta al usar equilibradores de carga de SKU estándar en AKS.

### <a name="limitations"></a>Limitaciones

Las siguientes limitaciones se aplican al crear y administrar clústeres de AKS que admiten un equilibrador de carga con la SKU *estándar*:

* Se requiere al menos una dirección IP pública o un prefijo de dirección IP pública para permitir el tráfico de salida del clúster de AKS. Esta dirección IP o el prefijo de dirección IP pública son necesarios para mantener la conectividad entre el plano de control y los nodos de agente, así como para mantener la compatibilidad con versiones anteriores de AKS. Tiene las siguientes opciones para especificar direcciones IP o prefijos de dirección IP pública con un equilibrador de carga de SKU *estándar*:
    * Proporcione sus propias IP públicas.
    * Proporcione sus propios prefijos de dirección IP pública.
    * Especifique un número hasta 100 para permitir que el clúster de AKS cree esa cantidad de direcciones IP públicas de SKU *estándar* en el mismo grupo de recursos que se creó como el clúster de AKS, que normalmente se denomina con *MC_* al principio. AKS asigna la dirección IP pública al equilibrador de carga de SKU *estándar*. De forma predeterminada, se creará automáticamente una IP pública en el mismo grupo de recursos que el clúster de AKS, si no se especifica ninguna dirección IP pública, prefijo de dirección IP pública o número de direcciones IP. Asimismo, debe permitir las direcciones públicas y evitar la creación de cualquier directiva de Azure Policy que prohíba la creación de direcciones IP.
* Si usa la SKU *estándar* de un equilibrador de carga, debe usar la versión de Kubernetes *1.13 o superior*.
* La definición de la SKU del equilibrador de carga solo puede realizarse cuando se crea un clúster de AKS. No se puede cambiar la SKU del equilibrador de carga una vez creado un clúster de AKS.
* Solo se puede usar un tipo de SKU de equilibrador de carga (básica o estándar) en un único clúster.
* Los equilibradores de carga de SKU *estándar* solo admiten direcciones IP de SKU *estándar*.

## <a name="configure-the-load-balancer-to-be-internal"></a>Configuración del equilibrador de carga para que sea interno

También puede configurar el equilibrador de carga como interno y no exponer una dirección IP pública. Para configurar el equilibrador de carga como interno, agregue `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` como una anotación al servicio *LoadBalancer*. Puede ver un manifiesto de YAML de ejemplo, así como más detalles sobre un equilibrador de carga interno [aquí][internal-lb-yaml].

## <a name="scale-the-number-of-managed-public-ips"></a>Escalado del número de direcciones IP públicas administradas

Cuando se usa un equilibrador de carga de SKU *estándar* con direcciones IP públicas de salida administradas, las cuales se crean de forma predeterminada, se puede escalar el número de direcciones IP públicas de salida administradas mediante el parámetro *load-balancer-managed-ip-count*.

Para actualizar un clúster existente, ejecute el siguiente comando. Este parámetro también se puede establecer en el momento de creación del clúster para tener varias direcciones IP públicas de salida administradas.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

En el ejemplo anterior se establece el número de direcciones IP públicas de salida administradas en *2* para el clúster de *myAKSCluster* en *myResourceGroup*. 

También puede usar el parámetro *load-balancer-managed-ip-count* para establecer el número inicial de direcciones IP públicas de salida administradas al crear el clúster anexando el parámetro `--load-balancer-managed-outbound-ip-count` y estableciéndolo en el valor deseado. El número predeterminado de direcciones IP públicas de salida administradas es 1.

## <a name="provide-your-own-public-ips-or-prefixes-for-egress"></a>Indicar prefijos o direcciones IP públicas propios para la salida

Cuando se usa un equilibrador de carga de SKU *estándar*, el clúster de AKS crea automáticamente una dirección IP pública en el mismo grupo de recursos creado para el clúster de AKS y asigna la dirección IP pública al equilibrador de carga de SKU *estándar*. Como alternativa, puede asignar su propia IP pública en el momento de la creación del clúster o puede actualizar las propiedades del equilibrador de carga de un clúster existente.

Al incorporar varios prefijos o direcciones IP, puede definir varios servicios de respaldo al definir la dirección IP detrás de un solo objeto de equilibrador de carga. El punto de conexión de salida de nodos específicos dependerá del servicio con el que estén asociados.

> [!IMPORTANT]
> Debe usar direcciones IP públicas de SKU *estándar* para salida con la SKU *estándar* del equilibrador de carga. Puede comprobar la SKU de las direcciones IP públicas mediante el comando [az network public-ip show][az-network-public-ip-show]:
>
> ```azurecli-interactive
> az network public-ip show --resource-group myResourceGroup --name myPublicIP --query sku.name -o tsv
> ```

Use el comando [az network public-ip show][az-network-public-ip-show] para enumerar los id. de las direcciones IP públicas.

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIP --query id -o tsv
```

El comando anterior muestra el id. de la dirección IP pública *myPublicIP* en el grupo de recursos *myResourceGroup*.

Use el comando *az aks update* con el parámetro *load-balancer-outbound-ips* para actualizar el clúster con las direcciones IP públicas.

En el ejemplo siguiente se usa el parámetro *load-balancer-outbound-ips* con los id. del comando anterior.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

También puede usar prefijos de dirección IP pública para la salida con el equilibrador de carga de SKU *estándar*. En el ejemplo siguiente se usa el comando [az network public-ip prefix show][az-network-public-ip-prefix-show] para enumerar los id. de los prefijos de dirección IP pública:

```azurecli-interactive
az network public-ip prefix show --resource-group myResourceGroup --name myPublicIPPrefix --query id -o tsv
```

El comando anterior muestra el id. del prefijo de dirección IP pública *myPublicIPPrefix* en el grupo de recursos *myResourceGroup*.

En el ejemplo siguiente se usa el parámetro *load-balancer-outbound-ip-prefixes* con los id. del comando anterior.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

> [!IMPORTANT]
> Las direcciones IP públicas y los prefijos de dirección IP pública deben estar en la misma región y formar parte de la misma suscripción que el clúster de AKS. 

### <a name="define-your-own-public-ip-or-prefixes-at-cluster-create-time"></a>Definición de sus propios prefijos o IP públicas en el momento de la creación del clúster

Es posible que quiera traer sus propias direcciones IP o prefijos IP para la salida en el momento de la creación del clúster para admitir escenarios como agregar puntos de conexión de salida a la lista de permitidos. Anexe los mismos parámetros mostrados anteriormente al paso de creación del clúster para definir sus propios prefijos IP y direcciones IP públicas al inicio del ciclo de vida de un clúster.

Use el comando *az aks create* con el parámetro *load-balancer-outbound-ips* para crear un clúster con las IP públicas en el inicio.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

Use el comando *az aks create* con el parámetro *load-balancer-outbound-ip-prefixes* para crear un clúster con los prefijos de direcciones IP públicas en el inicio.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

## <a name="show-the-outbound-rule-for-your-load-balancer"></a>Mostrar la regla de salida del equilibrador de carga

Para mostrar la regla de salida creada en el equilibrador de carga, use [az network lb outbound-rule list][az-network-lb-outbound-rule-list] y especifique el grupo de recursos del nodo del clúster de AKS:

```azurecli-interactive
NODE_RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
az network lb outbound-rule list --resource-group $NODE_RG --lb-name kubernetes -o table
```

Los comandos anteriores mostrarán la regla de salida del equilibrador de carga, por ejemplo:

```console
AllocatedOutboundPorts    EnableTcpReset    IdleTimeoutInMinutes    Name             Protocol    ProvisioningState    ResourceGroup
------------------------  ----------------  ----------------------  ---------------  ----------  -------------------  -------------
0                         True              30                      aksOutboundRule  All         Succeeded            MC_myResourceGroup_myAKSCluster_eastus  
```

En la salida del ejemplo, *AllocatedOutboundPorts* es 0. El valor de *AllocatedOutboundPorts* significa que la asignación de puertos SNAT se revierte a la asignación automática en función del tamaño del grupo de back-end. Consulte [Reglas de salida de Load Balancer][azure-lb-outbound-rules] y [Conexiones salientes en Azure][azure-lb-outbound-connections] para más detalles.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los servicios de Kubernetes en la [documentación de servicios de Kubernetes][kubernetes-services].

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-network-lb-outbound-rule-list]: /cli/azure/network/lb/outbound-rule?view=azure-cli-latest#az-network-lb-outbound-rule-list
[az-network-public-ip-show]: /cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show
[az-network-public-ip-prefix-show]: /cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[azure-lb-outbound-rules]: ../load-balancer/load-balancer-outbound-rules-overview.md#snatports
[azure-lb-outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#snat
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
