---
title: Pods seguros con directivas de redes en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo proteger el tráfico que fluye dentro y fuera de los pods mediante el uso de directivas de red de Kubernetes en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: a0512806ec797f43fc54d8a28a7cbadf86faf1d9
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230013"
---
# <a name="secure-traffic-between-pods-using-network-policies-in-azure-kubernetes-service-aks"></a>Protección del tráfico entre pods mediante directivas de red en Azure Kubernetes Service (AKS)

Al ejecutar aplicaciones modernas basadas en microservicios en Kubernetes, con frecuenta querrá controlar qué componentes pueden comunicarse entre sí. El principio de privilegio mínimo se debe aplicar a cómo el tráfico puede fluir entre pods en un clúster de Azure Kubernetes Service (AKS). Supongamos que es posible que desean bloquear el tráfico directamente a las aplicaciones de back-end. El *directiva de red* característica en Kubernetes le permite definir las reglas de tráfico de entrada y salida entre pods en un clúster.

Este artículo muestra cómo instalar el motor de directiva de red y crear directivas de red de Kubernetes para controlar el flujo del tráfico entre pods en AKS. Solo se debe usar la directiva de red para los nodos basados en Linux y los pods en AKS.

## <a name="before-you-begin"></a>Antes de empezar

Necesita la CLI de Azure versión 2.0.61 o posterior instalado y configurado. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

> [!TIP]
> Si usa la característica de directiva de red durante la versión preliminar, se recomienda [crear un nuevo clúster](#create-an-aks-cluster-and-enable-network-policy).
> 
> Si desea seguir usando los clústeres de prueba existente que usa la directiva de red durante la versión preliminar, actualizar el clúster a un nuevas versiones de Kubernetes en la versión de GA más reciente y, a continuación, implemente el siguiente manifiesto YAML para corregir el bloqueo server métricas y Kubernetes panel. Esta solución solo es necesario para los clústeres que usan el motor de directiva de red Calico.
>
> Como práctica recomendada de seguridad, [revisar el contenido de este manifiesto YAML] [ calico-aks-cleanup] para comprender lo que se implementa en el clúster de AKS.
>
> `kubectl delete -f https://raw.githubusercontent.com/Azure/aks-engine/master/docs/topics/calico-3.3.1-cleanup-after-upgrade.yaml`

## <a name="overview-of-network-policy"></a>Información general sobre la directiva de red

Todos los pods en un clúster de AKS pueden enviar y recibir tráfico sin limitaciones, de forma predeterminada. Para mejorar la seguridad, puede definir reglas que controlen el flujo de tráfico. Aplicaciones de back-end a menudo solo se exponen a los servicios front-end necesarios, por ejemplo. O bien, los componentes de base de datos solo son accesibles para los niveles de aplicación que se conectan a ellos.

Directiva de red es una especificación de Kubernetes que define las directivas de acceso para la comunicación entre Pods. Con las directivas de red, definir un conjunto ordenado de reglas para enviar y recibir tráfico y aplicarlos a una colección de pods que coinciden con uno o varios selectores de etiqueta.

Estas reglas de directiva de red se definen como los manifiestos de YAML. Las directivas de redes pueden incluirse como parte de un manifiesto más amplio que también crea una implementación o un servicio.

### <a name="network-policy-options-in-aks"></a>Opciones de directivas de redes en AKS

Azure proporciona dos maneras de implementar la directiva de red. Elija una opción de directiva de red al crear un clúster de AKS. La opción de directiva no se puede cambiar después de crear el clúster:

* Implementación de Azure, denominada *las directivas de red de Azure*.
* *Las directivas de red calico*, una red de código abierto y una solución de seguridad de red fundada por [Tigera][tigera].

Ambas implementaciones utilizan Linux *IPTables* para aplicar las directivas especificadas. Las directivas se traducen en conjuntos de pares IP permitidos y no permitidos. Estos pares, a continuación, se programan como las reglas de filtro de IPTable.

Directiva de red sólo funciona con la opción Azure CNI (avanzado). Implementación es diferente para las dos opciones:

* *Directivas de red de Azure* -la Azure CNI configura un puente en el host de máquina virtual de red entre nodos. Las reglas de filtrado se aplican cuando los paquetes que se pasan a través del puente.
* *Las directivas de red calico* -la Azure CNI establece las rutas de kernel local para el tráfico entre nodos. Las directivas se aplican en la interfaz de red del pod.

### <a name="differences-between-azure-and-calico-policies-and-their-capabilities"></a>Diferencias entre las directivas de Azure y Calico y sus capacidades

| Funcionalidad                               | Azure                      | Calico                      |
|------------------------------------------|----------------------------|-----------------------------|
| Plataformas compatibles                      | Linux                      | Linux                       |
| Admite las opciones de red             | Azure CNI                  | Azure CNI                   |
| Compatibilidad con la especificación de Kubernetes | Todos los tipos de directiva admitidos |  Todos los tipos de directiva admitidos |
| Características adicionales                      | None                       | Extender el modelo de directiva que consta de la directiva de red Global, conjunto de red Global y el punto de conexión de Host. Para obtener más información sobre el uso de la `calicoctl` CLI para administrar estos extendidos de las características, consulte [referencia de usuario calicoctl][calicoctl]. |
| Soporte técnico                                  | Compatible con el soporte técnico de Azure y el equipo de ingeniería | Soporte de la Comunidad calico. Para obtener más información sobre soporte técnico de pago adicional, consulte [opciones de soporte técnico de proyecto Calico][calico-support]. |
| Registro                                  | Las reglas de agregado o eliminado en IPTables se registran en cada host en */var/log/azure-npm.log* | Para obtener más información, consulte [registros de componente Calico][calico-logs] |

## <a name="create-an-aks-cluster-and-enable-network-policy"></a>Creación de un clúster de AKS y habilitación de la directiva de red

Para ver las directivas de red en acción, vamos a crear y, a continuación, expanda una directiva que define el flujo de tráfico:

* Deniegue todo el tráfico al pod.
* Permita el tráfico en función de las etiquetas de pod.
* Permita el tráfico según el espacio de nombres.

En primer lugar, vamos a crear un clúster de AKS que admite la directiva de red. Solo se puede habilitar la característica de directiva de red cuando se crea el clúster. No se puede habilitar la directiva de red en un clúster de AKS existente.

Para usar la directiva de red con un clúster de AKS, debe usar el [Azure CNI complemento] [ azure-cni] y definir su propia red virtual y subredes. Para obtener más información sobre cómo planear los rangos de subred requeridos, consulte la sección sobre cómo [configurar redes avanzadas][use-advanced-networking].

En el ejemplo siguiente se invoca el script:

* Crea una red virtual y una subred.
* Crea un Azure Active Directory (Azure AD) de la entidad de servicio para su uso con el clúster de AKS.
* Asigna permisos de *colaborador* para la entidad de servicio de clúster de AKS en la red virtual.
* Crea un clúster de AKS en la red virtual definida y habilita la directiva de red.
    * El *azure* se usa la opción de directiva de red. Para usar Calico como la opción de directiva de red en su lugar, utilice el `--network-policy calico` parámetro.

Proporcione su propia variable segura *SP_PASSWORD*. Puede reemplazar el *nombre_grupo_recursos* y *CLUSTER_NAME* variables:

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
# Enable network policy by using the `--network-policy` parameter
az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --generate-ssh-keys \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal $SP_ID \
    --client-secret $SP_PASSWORD \
    --network-policy azure
```

La operación de creación del clúster tarda unos minutos. Cuando el clúster esté listo, configure `kubectl` para conectarse al clúster de Kubernetes mediante la [az aks get-credentials] [ az-aks-get-credentials] comando. Con este comando se descargan las credenciales y se configura la CLI de Kubernetes para usarlas:

```azurecli-interactive
az aks get-credentials --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME
```

## <a name="deny-all-inbound-traffic-to-a-pod"></a>Denegación de todo el tráfico entrante a un pod

Antes de definir reglas para permitir el tráfico de red específico, primero cree una directiva de red para denegar todo el tráfico. Esta directiva proporciona un punto de partida para comenzar a la lista blanca solo el tráfico deseado. También puede ver claramente que el tráfico se descarta cuando se aplica la directiva de red.

Para el entorno de aplicación de ejemplo y las reglas de tráfico, vamos a crear un espacio de nombres denominado *desarrollo* para ejecutar los pods de ejemplo:

```console
kubectl create namespace development
kubectl label namespace/development purpose=development
```

Crear un pod de back-end de ejemplo que se ejecuta NGINX. Este pod de back-end puede usarse para simular una aplicación basada en web de back-end de ejemplo. Cree este pod en el espacio de nombres *development* y abra el puerto *80* para atender el tráfico web. Etiquete el pod con *app=webapp,role=backend* para que podamos utilizarlo como destino con una directiva de red en la sección siguiente:

```console
kubectl run backend --image=nginx --labels app=webapp,role=backend --namespace development --expose --port 80 --generator=run-pod/v1
```

Cree otro pod y adjuntar una sesión de terminal para probar que pueda comunicarse correctamente con la página Web NGINX de forma predeterminada:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

En el símbolo del shell, use `wget` para confirmar que puede tener acceso a la página Web NGINX de forma predeterminada:

```console
wget -qO- http://backend
```

El siguiente ejemplo muestra que devuelve la página Web NGINX de forma predeterminada:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Salga de la sesión de terminal asociada. Automáticamente se elimina el pod de prueba.

```console
exit
```

### <a name="create-and-apply-a-network-policy"></a>Creación y aplicación de una directiva de red

Ahora que ha confirmado que puede usar la página Web NGINX básica en el pod de back-end de ejemplo, cree una directiva de red para denegar todo el tráfico. Cree un archivo denominado `backend-policy.yaml` y pegue el siguiente manifiesto de YAML. Este manifiesto se usa un *podSelector* para asociar la directiva a los pods que tienen el *app:webapp, rol: back-end* etiqueta, como su pod NGINX de ejemplo. No hay reglas definidas en *ingress*, por lo que se deniega todo el tráfico entrante en el pod:

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

Aplicar la directiva de red mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-network-policy"></a>Prueba de la directiva de red


Vamos a ver si puede usar la página Web NGINX en el pod de back-end nuevo. Cree otro pod de prueba y asocie una sesión de terminal:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

En el símbolo del shell, use `wget` para ver si puede tener acceso a la página Web NGINX de forma predeterminada. Esta vez, establezca un valor de tiempo de expiración de *2* segundos. Ahora la directiva de red bloquea todo el tráfico entrante, por lo que no se puede cargar la página, tal como se muestra en el ejemplo siguiente:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Salga de la sesión de terminal asociada. Automáticamente se elimina el pod de prueba.

```console
exit
```

## <a name="allow-inbound-traffic-based-on-a-pod-label"></a>Tráfico entrante permitido en función de una etiqueta de pod

En la sección anterior, se ha programado un pod NGINX de back-end y se creó una directiva de red para denegar todo el tráfico. Vamos a crear un pod de front-end y actualizar la directiva de red para permitir el tráfico desde los pods de front-end.

Actualice la directiva de red para permitir el tráfico desde los pods con las etiquetas *app:webapp,role:frontend* y en cualquier espacio de nombres. Editar anterior *back-end policy.yaml* y agréguele *matchLabels* de reglas de entrada para que el manifiesto es similar al ejemplo siguiente:

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
> Esta directiva de red usa un elemento *namespaceSelector* y un elemento *podSelector* para la regla de entrada. La sintaxis YAML es importante para las reglas de entrada se suma. En este ejemplo, ambos elementos deben coincidir para que se aplique la regla de entrada. Las versiones anteriores a Kubernetes *1.12* no puede interpretar correctamente estos elementos y restringir el tráfico de red según lo esperado. Para obtener más información acerca de este comportamiento, consulte [comportamiento de a y desde los selectores][policy-rules].

Aplicar la directiva de red actualizado mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

Programar un pod que se etiqueta como *app = webapp, rol = front-end* y adjuntar una sesión de terminal:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

En el símbolo del shell, use `wget` para ver si puede tener acceso a la página Web NGINX de forma predeterminada:

```console
wget -qO- http://backend
```

Porque la regla de entrada permite el tráfico con los pods que tengan las etiquetas *app: aplicación Web, rol: front-end*, se permite el tráfico desde el front-end pod. La salida del ejemplo siguiente muestra la página de Web NGINX predeterminado devuelve:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Salga de la sesión de terminal asociada. Automáticamente se elimina el pod.

```console
exit
```

### <a name="test-a-pod-without-a-matching-label"></a>Prueba de un pod sin una etiqueta coincidente

La directiva de red permite el tráfico desde los pods con la etiqueta *app: webapp,role: frontend*, pero debe denegar todo el tráfico. Vamos a probar para ver si otra vaina sin esas etiquetas puede tener acceso el pod NGINX de back-end. Cree otro pod de prueba y asocie una sesión de terminal:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

En el símbolo del shell, use `wget` para ver si puede tener acceso a la página Web NGINX de forma predeterminada. La directiva de red bloquea el tráfico entrante, por lo que no se puede cargar la página, tal como se muestra en el ejemplo siguiente:

```console
$ wget -qO- --timeout=2 http://backend

wget: download timed out
```

Salga de la sesión de terminal asociada. Automáticamente se elimina el pod de prueba.

```console
exit
```

## <a name="allow-traffic-only-from-within-a-defined-namespace"></a>Tráfico permitido solo desde dentro de un espacio de nombres definido

En los ejemplos anteriores, ha creado una directiva de red que deniega todo el tráfico y, a continuación, actualiza la directiva para permitir el tráfico desde los pods con una etiqueta específica. Otra necesidad común consiste en limitar el tráfico a solo dentro de un espacio de nombres especificado. Si fueron de los ejemplos anteriores para el tráfico en un *desarrollo* espacio de nombres, cree una directiva de red que impide que el tráfico de otro espacio de nombres, como *producción*, lleguen los pods.

En primer lugar, cree un espacio de nombres para simular un espacio de nombres production:

```console
kubectl create namespace production
kubectl label namespace/production purpose=production
```

Programe un pod de prueba en el espacio de nombres *production* que está etiquetado como *app=webapp,role=frontend*. Asocie una sesión de terminal:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

En el símbolo del shell, use `wget` para confirmar que puede tener acceso a la página Web NGINX de forma predeterminada:

```console
wget -qO- http://backend.development
```

Dado que las etiquetas de lo pod coincide con lo que actualmente está permitido en la directiva de red, se permite el tráfico. La directiva de red no analiza los espacios de nombres, solo las etiquetas de pod. La salida del ejemplo siguiente muestra la página de Web NGINX predeterminado devuelve:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Salga de la sesión de terminal asociada. Automáticamente se elimina el pod de prueba.

```console
exit
```

### <a name="update-the-network-policy"></a>Actualización de la directiva de red

Vamos a actualizar la regla de entrada *namespaceSelector* sección para permitir únicamente el tráfico desde el *desarrollo* espacio de nombres. Edite el archivo de manifiesto *backend-policy.yaml*, como se muestra en el ejemplo siguiente:

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

En los ejemplos más complejos, podría definir varias reglas de entrada, como un *namespaceSelector* y, a continuación, un *podSelector*.

Aplicar la directiva de red actualizado mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```azurecli-interactive
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-updated-network-policy"></a>Prueba de la directiva de red actualizada

Programar otra pod en el *producción* espacio de nombres y adjuntar una sesión de terminal:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

En el símbolo del shell, use `wget` para ver que la directiva de red ahora deniega el tráfico:

```console
$ wget -qO- --timeout=2 http://backend.development

wget: download timed out
```

Salga del pod de prueba:

```console
exit
```

Con el tráfico a impedir el *producción* espacio de nombres, programación de las copias de un pod de prueba en el *desarrollo* espacio de nombres y adjuntar una sesión de terminal:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

En el símbolo del shell, use `wget` para ver que la directiva de red permite el tráfico:

```console
wget -qO- http://backend
```

El tráfico se permite porque el pod se programa en el espacio de nombres que coincide con lo que está permitido en la directiva de red. La salida de ejemplo siguiente muestra la página de Web NGINX predeterminado devuelve:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Salga de la sesión de terminal asociada. Automáticamente se elimina el pod de prueba.

```console
exit
```

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, hemos creado dos espacios de nombres y aplica una directiva de red. Para limpiar estos recursos, use el [kubectl eliminar] [ kubectl-delete] comando y especifique los nombres de recursos:

```console
kubectl delete namespace production
kubectl delete namespace development
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los recursos de red, consulte [conceptos relacionados con aplicaciones de Azure Kubernetes Service (AKS) de red][concepts-network].

Para más información acerca de las directivas, consulte [las directivas de red de Kubernetes][kubernetes-network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[policy-rules]: https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors
[aks-github]: https://github.com/azure/aks/issues
[tigera]: https://www.tigera.io/
[calicoctl]: https://docs.projectcalico.org/v3.6/reference/calicoctl/
[calico-support]: https://www.projectcalico.org/support
[calico-logs]: https://docs.projectcalico.org/v3.6/maintenance/component-logs
[calico-aks-cleanup]: https://github.com/Azure/aks-engine/blob/master/docs/topics/calico-3.3.1-cleanup-after-upgrade.yaml

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[use-advanced-networking]: configure-advanced-networking.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[concepts-network]: concepts-network.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
