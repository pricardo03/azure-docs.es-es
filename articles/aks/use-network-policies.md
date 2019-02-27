---
title: Pods seguros con directivas de redes en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo proteger el tráfico que fluye dentro y fuera de los pods mediante directivas de red de Kubernetes en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/12/2019
ms.author: iainfou
ms.openlocfilehash: 250c4fc6e51bacc68c965394b9fd430b1b75a52c
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447181"
---
# <a name="secure-traffic-between-pods-using-network-policies-in-azure-kubernetes-service-aks"></a>Protección del tráfico entre pods mediante directivas de red en Azure Kubernetes Service (AKS)

Al ejecutar aplicaciones modernas basadas en microservicios en Kubernetes, con frecuenta querrá controlar qué componentes pueden comunicarse entre sí. El principio de privilegios mínimos debe aplicarse a cómo el tráfico puede fluir entre pods de un clúster de AKS. Por ejemplo, es probable que desee bloquear el tráfico directamente a las aplicaciones de back-end. En Kubernetes, la característica *Directiva de red* permite definir las reglas de tráfico de entrada y salida entre pods de un clúster.

En este artículo se muestra cómo usar las directivas de red para controlar el flujo de tráfico entre pods en AKS.

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición a condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="before-you-begin"></a>Antes de empezar

Es preciso que esté instalada y configurada la versión 2.0.56 de la CLI de Azure u otra versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

Para crear un clúster de AKS con la directiva de red, habilite primero una marca de característica en la suscripción. Para registrar la marca de característica *EnableNetworkPolicy*, use el comando [az feature register][az-feature-register] tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az feature register --name EnableNetworkPolicy --namespace Microsoft.ContainerService
```

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EnableNetworkPolicy')].{Name:name,State:properties.state}"
```

Cuando todo esté listo, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-network-policy"></a>Información general sobre la directiva de red

De forma predeterminada, todos los pods de un clúster de AKS pueden enviar y recibir tráfico sin limitaciones. Para mejorar la seguridad, puede definir reglas que controlen el flujo de tráfico. Por ejemplo, las aplicaciones de back-end solo se suelen exponer en los servicios de front-end o los componentes de base de datos solo son accesibles para las capas de aplicación que se conectan a ellos.

Las directivas de red son recursos de Kubernetes que permiten controlar el flujo de tráfico entre pods. Puede permitir o denegar el tráfico según la configuración como etiquetas asignadas, espacio de nombres o puerto de tráfico. Las directivas de red se definen como manifiestos de YAML y se pueden incluir como parte de uno más amplio que también crea una implementación o un servicio.

Para ver las directivas de red en acción, vamos a crear y luego expandir una directiva que define el flujo de tráfico de la siguiente forma:

* Deniegue todo el tráfico al pod.
* Permita el tráfico en función de las etiquetas de pod.
* Permita el tráfico según el espacio de nombres.

## <a name="create-an-aks-cluster-and-enable-network-policy"></a>Creación de un clúster de AKS y habilitación de la directiva de red

Solo se puede habilitar la directiva de red cuando se crea el clúster. No se puede habilitar la directiva de red en un clúster de AKS existente. 

Para usar la directiva de red con un clúster de AKS, debe usar el [complemento de CNI de Azure][azure-cni] y definir su propia red virtual y sus propias subredes. Para obtener más información sobre cómo planear los rangos de subred requeridos, consulte la sección sobre cómo [configurar redes avanzadas][use-advanced-networking].

En el ejemplo siguiente se invoca el script:

* Crea una red virtual y una subred.
* Crea una entidad de servicio de Azure Active Directory (AD) para usarse con el clúster de AKS.
* Asigna permisos de *colaborador* para la entidad de servicio de clúster de AKS en la red virtual.
* Crea un clúster de AKS en la red virtual definida y habilita la directiva de red.

Proporcione su propia variable segura *SP_PASSWORD*. Si lo desea, reemplace las variables *RESOURCE_GROUP_NAME* y *CLUSTER_NAME*:

```azurecli-interactive
SP_PASSWORD=mySecurePassword
RESOURCE_GROUP_NAME=myResourceGroup-NP
CLUSTER_NAME=myAKSCluster
LOCATION=canadaeast

# Create a resource group
az group create --name $RESOURCE_GROUP_NAME --location $LOCATION

# Create a virtual network and subnet
az network vnet create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name myVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16

# Create a service principal and read in the application ID
SP_ID=$(az ad sp create-for-rbac --password $SP_PASSWORD --skip-assignment --query [appId] -o tsv)

# Wait 15 seconds to make sure that service principal has propagated
echo "Waiting for service principal to propagate..."
sleep 15

# Get the virtual network resource ID
VNET_ID=$(az network vnet show --resource-group $RESOURCE_GROUP_NAME --name myVnet --query id -o tsv)

# Assign the service principal Contributor permissions to the virtual network resource
az role assignment create --assignee $SP_ID --scope $VNET_ID --role Contributor

# Get the virtual network subnet resource ID
SUBNET_ID=$(az network vnet subnet show --resource-group $RESOURCE_GROUP_NAME --vnet-name myVnet --name myAKSSubnet --query id -o tsv)

# Create the AKS cluster and specify the virtual network and service principal information
# Enable network policy using the `--network-policy` parameter
az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --kubernetes-version 1.12.4 \
    --generate-ssh-keys \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal $SP_ID \
    --client-secret $SP_PASSWORD \
    --network-policy calico
```

La operación de creación del clúster tarda unos minutos. Al terminar, configure `kubectl` para conectarse a su clúster de Kubernetes con el comando [az aks get-credentials][az-aks-get-credentials]. Con este comando se descargan las credenciales y se configura la CLI de Kubernetes para usarlas:

```azurecli-interactive
az aks get-credentials --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME
```

## <a name="deny-all-inbound-traffic-to-a-pod"></a>Denegación de todo el tráfico entrante a un pod

Antes de definir reglas para permitir el tráfico de red específico, primero cree una directiva de red para denegar todo el tráfico. Esta directiva proporciona un punto de partida para incluir en la lista blanca solo el tráfico deseado. También puede ver claramente que el tráfico se descarta cuando se aplica la directiva de red.

En lo que respecta a nuestro entorno de aplicación de ejemplo y a nuestras reglas de tráfico, vamos a crear un espacio de nombres denominado *development* para ejecutar nuestros pods de ejemplo:

```console
kubectl create namespace development
kubectl label namespace/development purpose=development
```

Ahora cree un pod de back-end de ejemplo que ejecute NGINX. Este pod de back-end puede usarse para simular una aplicación web de back-end de ejemplo. Cree este pod en el espacio de nombres *development* y abra el puerto *80* para atender el tráfico web. Etiquete el pod con *app=webapp,role=backend* para que podamos utilizarlo como destino con una directiva de red en la sección siguiente:

```console
kubectl run backend --image=nginx --labels app=webapp,role=backend --namespace development --expose --port 80 --generator=run-pod/v1
```

Para probar que puede llegar correctamente a la página web de NGINX predeterminada, cree otro pod y asocie a una sesión de terminal:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Cuando se encuentre en el símbolo del sistema del shell, use `wget` para confirmar que puede acceder a la página web de NGINX predeterminada:

```console
wget -qO- http://backend
```

La siguiente salida de ejemplo muestra que se devuelve la página web de NGINX predeterminada:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Salga de la sesión de terminal asociada. El pod de prueba se elimina automáticamente:

```console
exit
```

### <a name="create-and-apply-a-network-policy"></a>Creación y aplicación de una directiva de red

Ahora que ha confirmado que acceder a la página web de NGINX básica en el pod de back-end de ejemplo, cree una directiva de red para denegar todo el tráfico. Cree un archivo denominado `backend-policy.yaml` y pegue el siguiente manifiesto de YAML. Este manifiesto usa un elemento *podSelector* para asociar la directiva a los pods que tienen la etiqueta *app:webapp,role:backend*, como el pod de NGINX de ejemplo. No hay reglas definidas en *ingress*, por lo que se deniega todo el tráfico entrante en el pod:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress: []
```

Aplique la directiva de red mediante el comando [kubectl apply][kubectl-apply] y especifique el nombre del manifiesto de YAML:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-network-policy"></a>Prueba de la directiva de red

Comprobemos puede acceder a la página web de NGINX en el pod de back-end de nuevo. Cree otro pod de prueba y asocie una sesión de terminal:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Cuando se encuentre en el símbolo del sistema del shell, use `wget` para ver si puede acceder a la página web de NGINX predeterminada. Esta vez, establezca un valor de tiempo de expiración de *2* segundos. Ahora, la directiva de red bloquea todo el tráfico entrante, por lo que no se puede cargar la página, tal como se muestra en el ejemplo siguiente:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Salga de la sesión de terminal asociada. El pod de prueba se elimina automáticamente:

```console
exit
```

## <a name="allow-inbound-traffic-based-on-a-pod-label"></a>Tráfico entrante permitido en función de una etiqueta de pod

En la sección anterior, se ha programado un pod de NGINX de back-end y se creó una directiva de red para denegar todo el tráfico. Ahora vamos a crear un pod de front-end y actualizar la directiva de red para permitir el tráfico desde los pods de front-end.

Actualice la directiva de red para permitir el tráfico desde los pods con las etiquetas *app:webapp,role:frontend* y en cualquier espacio de nombres. Edite el anterior archivo *back-end policy.yaml* y agregue unas reglas de entrada *matchLabels* para que el manifiesto sea similar al ejemplo siguiente:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector: {}
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

> [!NOTE]
> Esta directiva de red usa un elemento *namespaceSelector* y un elemento *podSelector* para la regla de entrada. La sintaxis YAML es importante para que las reglas de entrada sean o no aditivas. En este ejemplo, ambos elementos deben coincidir para que se aplique la regla de entrada. Puede que las versiones de Kubernetes anteriores a *1.12* no puedan interpretar correctamente estos elementos y restrinjan el tráfico de red según lo esperado. Para más información, consulte [Behavior of to and from selectors][policy-rules] (Comportamiento de los selectores to and from).

Aplique la política de red actualizada mediante el comando [kubectl apply][kubectl-apply] y especifique el nombre del manifiesto de YAML:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

Ahora, programe un pod que se etiquete como *app=webapp,role=frontend* y asocie una sesión de terminal:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Cuando se encuentre en el símbolo del sistema del shell, use `wget` para ver si puede acceder a la página web de NGINX predeterminada:

```console
wget -qO- http://backend
```

Como la regla de entrada permite el tráfico con los pods que tengan las etiquetas *app: webapp,role: frontend*, se permite el tráfico desde el pod de front-end. La siguiente salida de ejemplo muestra que se devuelve la página web de NGINX predeterminada:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Salga de la sesión de terminal asociada. El pod se elimina automáticamente:

```console
exit
```

### <a name="test-a-pod-without-a-matching-label"></a>Prueba de un pod sin una etiqueta coincidente

La directiva de red permite el tráfico desde los pods con la etiqueta *app: webapp,role: frontend*, pero debe denegar todo el tráfico. Vamos a probar que otro pod sin esas etiquetas no tenga acceso al pod de NGINX de back-end. Cree otro pod de prueba y asocie una sesión de terminal:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Cuando se encuentre en el símbolo del sistema del shell, use `wget` para ver si puede acceder a la página web de NGINX predeterminada. La directiva de red bloquea el tráfico entrante, por lo que no se puede cargar la página, tal como se muestra en el ejemplo siguiente:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Salga de la sesión de terminal asociada. El pod de prueba se elimina automáticamente:

```console
exit
```

## <a name="allow-traffic-only-from-within-a-defined-namespace"></a>Tráfico permitido solo desde dentro de un espacio de nombres definido

En los ejemplos anteriores, ha creado una directiva de red que deniega todo el tráfico y, después, actualiza la directiva para permitir el tráfico desde los pods con una etiqueta específica. Otra de las cosas que se suele necesitar es limitar el tráfico a solo dentro de un espacio de nombres especificado. Si los ejemplos anteriores fueran para el tráfico en un espacio de nombres *development*, puede que desee crear una directiva de red que impida que el tráfico de otro espacio de nombres, como *production*, llegue a los pods.

En primer lugar, cree un espacio de nombres para simular un espacio de nombres production:

```console
kubectl create namespace production
kubectl label namespace/production purpose=production
```

Programe un pod de prueba en el espacio de nombres *production* que está etiquetado como *app=webapp,role=frontend*. Asocie una sesión de terminal:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Cuando se encuentre en el símbolo del sistema del shell, use `wget` para confirmar que puede acceder a la página web de NGINX predeterminada:

```console
wget -qO- http://backend.development
```

Como las etiquetas del pod coinciden con lo que actualmente está permitido en la directiva de red, se permite el tráfico. La directiva de red no analiza los espacios de nombres, solo las etiquetas de pod. La siguiente salida de ejemplo muestra que se devuelve la página web de NGINX predeterminada:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Salga de la sesión de terminal asociada. El pod de prueba se elimina automáticamente:

```console
exit
```

### <a name="update-the-network-policy"></a>Actualización de la directiva de red

Ahora vamos a actualizar la sección *namespaceSelector* de la regla de entrada para permitir únicamente el tráfico desde el espacio de nombres *development*. Edite el archivo de manifiesto *backend-policy.yaml*, como se muestra en el ejemplo siguiente:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          purpose: development
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

En ejemplos más complejos, podría definir varias reglas de entrada, como utilizar un elemento *namespaceSelector* y, a continuación, un elemento *podSelector*.

Aplique la política de red actualizada mediante el comando [kubectl apply][kubectl-apply] y especifique el nombre del manifiesto de YAML:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-updated-network-policy"></a>Prueba de la directiva de red actualizada

Ahora, programe otro pod en el espacio de nombres *production* y asocie una sesión de terminal:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Una vez en el símbolo del sistema del shell, use `wget` para ver que ahora la directiva de red deniega el tráfico:

```console
$ wget -qO- --timeout=2 http://backend.development

wget: download timed out
```

Salga del pod de prueba:

```console
exit
```

Con el tráfico denegado desde el espacio de nombres *production*, programe ahora de nuevo un pod de prueba en el espacio de nombres *development* y asocie una sesión de terminal:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Una vez en el símbolo del sistema del shell, use `wget` para ver que la directiva de red permite el tráfico:

```console
wget -qO- http://backend
```

Como el pod está programado en el espacio de nombres para que coincide con lo permitido en la política de red, el tráfico está permitido. La siguiente salida de ejemplo muestra que se devuelve la página web de NGINX predeterminada:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Salga de la sesión de terminal asociada. El pod de prueba se elimina automáticamente:

```console
exit
```

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, creamos dos espacios de nombres y aplicamos una directiva de red. Para limpiar estos recursos, use el comando [kubectl delete][kubectl-delete] y especifique los nombres de recursos como sigue:

```console
kubectl delete namespace production
kubectl delete namespace development
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los recursos red, consulte [Conceptos de redes de aplicaciones en Azure Kubernetes Service (AKS)][concepts-network].

Si quiere saber más detalles sobre el uso de directivas, consulte [Directivas de red de Kubernetes][kubernetes-network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[policy-rules]: https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[use-advanced-networking]: configure-advanced-networking.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[concepts-network]: concepts-network.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
