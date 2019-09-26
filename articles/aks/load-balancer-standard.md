---
title: Uso de un equilibrador de carga de SKU estándar en Azure Kubernetes Service (AKS)
description: Aprenda a usar un equilibrador de carga con una SKU estándar para exponer los servicios con Azure Kubernetes Service (AKS).
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 09/05/2019
ms.author: zarhoads
ms.openlocfilehash: d2a0ff5db6707c4f765c71937a7d0f0749401959
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71172197"
---
# <a name="use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Uso de un equilibrador de carga de SKU estándar en Azure Kubernetes Service (AKS)

Para proporcionar acceso a las aplicaciones en Azure Kubernetes Service (AKS), puede crear y usar una instancia de Azure Load Balancer. Los equilibradores de carga que se ejecutan en AKS pueden usarse como un equilibrador de carga interno o externo. Un equilibrador de carga interno permite que un servicio de Kubernetes sea accesible solo para las aplicaciones que se ejecutan en la misma red virtual que el clúster de AKS. Un equilibrador de carga externo recibe una o más direcciones IP públicas de entrada y permite que un servicio de Kubernetes sea accesible externamente mediante las direcciones IP públicas.

Azure Load Balancer está disponible en dos SKU: *Básica* y *Estándar*. De forma predeterminada, la SKU *básica* se usa cuando un manifiesto de servicio se emplea para crear un equilibrador de carga en AKS. Con la SKU *estándar*, el equilibrador de carga proporciona características y funcionalidades adicionales, como un tamaño mayor de grupo back-end y Availability Zones. Es importante comprender las diferencias entre los equilibradores de carga *estándar* y *básicos* antes de decidir cuál usar. Una vez creado el clúster de AKS, no se puede cambiar la SKU del equilibrador de carga para ese clúster. Para más información sobre las SKU *básicas* y *estándar*, consulte [Comparación de las SKU de Load Balancer][azure-lb-comparison].

En este artículo se muestra cómo crear y usar una instancia de Azure Load Balancer con la SKU *estándar* con Azure Kubernetes Service (AKS).

Se presupone que tiene un conocimiento básico de los conceptos de Kubernetes y Azure Load Balancer. Para obtener más información, consulte [Conceptos básicos de Kubernetes de Azure Kubernetes Service (AKS)][kubernetes-concepts] y [¿Qué es Azure Load Balancer?][azure-lb].

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0.59 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][install-azure-cli].

## <a name="before-you-begin"></a>Antes de empezar

Si usa una subred o un grupo de recursos existentes, la entidad de servicio del clúster de AKS necesita permiso para administrar los recursos de red. En general, asigne el rol *Colaborador de red* a la entidad de servicio en los recursos delegados. Para más información sobre los permisos, consulte [Delegación del acceso de AKS a otros recursos de Azure][aks-sp].

Debe crear un clúster de AKS que establezca la SKU del equilibrador de carga en *estándar* y no *básica*, que es la predeterminada.

### <a name="install-aks-preview-cli-extension"></a>Instalación de la extensión aks-preview de la CLI

Para usar la SKU estándar del equilibrador de carga de Azure, necesita la extensión *aks-preview* de la CLI, versión 0.4.12 o superior. Instale la extensión de la CLI de Azure *aks-preview* con el comando [az extension add][az-extension-add] y, a continuación, busque las actualizaciones disponibles con el comando [az extension update][az-extension-update]:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="limitations"></a>Limitaciones

Las siguientes limitaciones se aplican al crear y administrar clústeres de AKS que admiten un equilibrador de carga con la SKU *estándar*:

* Se requiere al menos una dirección IP pública o un prefijo de dirección IP pública para permitir el tráfico de salida del clúster de AKS. Esta dirección IP o el prefijo de dirección IP pública son necesarios para mantener la conectividad entre el plano de control y los nodos de agente, así como para mantener la compatibilidad con versiones anteriores de AKS. Tiene las siguientes opciones para especificar direcciones IP o prefijos de dirección IP pública con un equilibrador de carga de SKU *estándar*:
    * Proporcione sus propias IP públicas.
    * Proporcione sus propios prefijos de dirección IP pública.
    * Especifique un número hasta 100 para permitir que el clúster de AKS cree esa cantidad de direcciones IP públicas de SKU *estándar* en el mismo grupo de recursos que se creó como el clúster de AKS, que normalmente se denomina con *MC_* al principio. AKS asigna la dirección IP pública al equilibrador de carga de SKU *estándar*. De forma predeterminada, se creará automáticamente una IP pública en el mismo grupo de recursos que el clúster de AKS, si no se especifica ninguna dirección IP pública, prefijo de dirección IP pública o número de direcciones IP. Asimismo, debe permitir las direcciones públicas y evitar la creación de cualquier directiva de Azure Policy que prohíba la creación de direcciones IP.
* Cuando se usa la SKU *estándar* de un equilibrador de carga, debe usar la versión de Kubernetes 1.13 o una superior.
* La definición de la SKU del equilibrador de carga solo puede realizarse cuando se crea un clúster de AKS. No se puede cambiar la SKU del equilibrador de carga una vez creado un clúster de AKS.
* Además, solo se puede usar un tipo de SKU de equilibrador de carga en un único clúster.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un grupo lógico en el que se implementan y se administran los recursos de Azure. Cuando se crea un grupo de recursos, se le pide que especifique una ubicación. Dicha ubicación es donde se almacenan los metadatos del grupo de recursos, así como el lugar en el que los recursos se ejecutan en Azure si no se especifica otra región al crear los recursos. Cree un grupo de recursos con el comando [az group create][az-group-create].

En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

En la siguiente salida de ejemplo se muestra que los recursos se crearon correctamente:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-aks-cluster"></a>Creación de un clúster de AKS
Para poder ejecutar un clúster de AKS que admita un equilibrador de carga con la SKU *estándar*, el clúster debe establecer el parámetro *load-balancer-sku* en *standard*. Este parámetro crea un equilibrador de carga con la SKU *estándar* cuando se crea el clúster. Al ejecutar un servicio *LoadBalancer* en el clúster, la configuración del equilibrador de carga de SKU *estándar* se actualiza con la configuración del servicio. Use el comando [az aks create][az-aks-create] para crear un clúster de AKS denominado *myAKSCluster*.

> [!NOTE]
> La propiedad *load-balancer-sku* solo puede usarse cuando se crea el clúster. No se puede cambiar la SKU del equilibrador de carga una vez creado un clúster de AKS. Además, solo se puede usar un tipo de SKU de equilibrador de carga en un único clúster.
> 
> Si quiere usar sus propias direcciones IP públicas, use los parámetros *load-balancer-Outbound-ips*o *load-balancer-outbound-ip-prefixes*. Ambos parámetros también se pueden usar al [actualizar el clúster.](#optional---provide-your-own-public-ips-or-prefixes-for-egress)

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys
```

Transcurridos unos minutos, el comando se completa y devuelve información en formato JSON sobre el clúster.

## <a name="connect-to-the-cluster"></a>Conexión al clúster

Para administrar un clúster de Kubernetes, usará [kubectl][kubectl], el cliente de línea de comandos de Kubernetes. Si usa Azure Cloud Shell, `kubectl` ya está instalado. Para instalar `kubectl` localmente, use el comando [az aks install-cli][az-aks-install-cli]:

```azurecli
az aks install-cli
```

Para configurar `kubectl` para conectarse a su clúster de Kubernetes, use el comando [az aks get-credentials][az-aks-get-credentials]. Con este comando se descargan las credenciales y se configura la CLI de Kubernetes para usarlas.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Para comprobar la conexión con el clúster, use el comando [kubectl get][kubectl-get] para que devuelva una lista de los nodos del clúster.

```azurecli-interactive
kubectl get nodes
```

La salida del ejemplo siguiente muestra el nodo único creado en los pasos anteriores. Asegúrese de que el estado del nodo es *Listo*:

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.13.10
```

## <a name="verify-your-cluster-uses-the-standard-sku"></a>Comprobación de que el clúster usa la SKU *estándar*

Use el comando [az aks show][az-aks-show] para mostrar la configuración del clúster.

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster

{
  "aadProfile": null,
  "addonProfiles": null,
   ...
   "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerSku": "standard",
    ...
```

Compruebe que la propiedad *loadBalancerSku* se muestra como *standard*.

## <a name="use-the-load-balancer"></a>Uso del equilibrador de carga

Para usar el equilibrador de carga en el clúster, cree un manifiesto de servicio con el tipo de servicio *LoadBalancer*. Para mostrar el equilibrador de carga en funcionamiento, cree otro manifiesto con una aplicación de ejemplo para ejecutar en el clúster. Esta aplicación de ejemplo se expone a través del equilibrador de carga y puede verse mediante un explorador.

Cree un manifiesto llamado `sample.yaml`, tal como se muestra en el ejemplo siguiente:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
```

El manifiesto anterior configura dos implementaciones: *azure-vote-front* y *azure-vote-back*. Para configurar la implementación *azure-vote-front* que se va a exponer con el equilibrador de carga, cree un manifiesto llamado `standard-lb.yaml`, tal como se muestra en el ejemplo siguiente:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

El servicio *azure-vote-front* usa el tipo *LoadBalancer* para configurar el equilibrador de carga en el clúster de AKS y conectarse a la implementación *azure-vote-front*.

Implemente la aplicación de ejemplo y el equilibrador de carga mediante [kubectl apply][kubectl-apply] y especifique el nombre de los manifiestos de YAML:

```console
kubectl apply -f sample.yaml
kubectl apply -f standard-lb.yaml
```

El equilibrador de carga de SKU *estándar* está configurado ahora para exponer la aplicación de ejemplo. Consulte los detalles de servicio de *azure-vote-front* mediante [kubectl get][kubectl-get] para ver la dirección IP pública del equilibrador de carga. La dirección IP pública del equilibrador de carga se muestra en la columna *EXTERNAL-IP*. Puede que tarde uno o dos minutos en cambiar de *\<pendiente\>* a una dirección IP interna real, tal como se muestra en el ejemplo siguiente:

```
$ kubectl get service azure-vote-front

NAME                TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
azure-vote-front    LoadBalancer   10.0.227.198   52.179.23.131   80:31201/TCP   16s
```

Vaya a la dirección IP pública en un explorador y compruebe que ve la aplicación de ejemplo. En el ejemplo anterior, la dirección IP pública es `52.179.23.131`.

![Imagen de la exploración hasta Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

> [!NOTE]
> También puede configurar el equilibrador de carga como interno y no exponer una dirección IP pública. Para configurar el equilibrador de carga como interno, agregue `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` como una anotación al servicio *LoadBalancer*. Puede ver un manifiesto de YAML de ejemplo, así como más detalles sobre un equilibrador de carga interno [aquí][internal-lb-yaml].

## <a name="optional---scale-the-number-of-managed-public-ips"></a>Opcional: escalar el número de direcciones IP públicas administradas

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

## <a name="optional---provide-your-own-public-ips-or-prefixes-for-egress"></a>Opcional: proporcione sus propios prefijos o direcciones IP públicas para la salida

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

```
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

```
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

## <a name="clean-up-the-standard-sku-load-balancer-configuration"></a>Limpieza de la configuración del equilibrador de carga de SKU estándar

Para quitar la configuración del equilibrador de carga y de la aplicación de ejemplo, use [kubectl delete][kubectl-delete]:

```console
kubectl delete -f sample.yaml
kubectl delete -f standard-lb.yaml
```

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
[az-network-public-ip-show]: /cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show
[az-network-public-ip-prefix-show]: /cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

