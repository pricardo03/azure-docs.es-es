---
title: 'Kubernetes en Azure: escalador automático de clúster'
description: Aprenda a usar el escalador automático de clúster con Azure Kubernetes Service (AKS) para escalar automáticamente el clúster con el fin de satisfacer la demanda.
services: container-service
author: sakthivetrivel
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/19/18
ms.author: sakthivetrivel
ms.custom: mvc
ms.openlocfilehash: 4f8df8e7004ca3cee832b6230dc153b21e2a6c18
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186720"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Escalador automático en Azure Kubernetes Service (AKS): Versión preliminar

Azure Kubernetes Service (AKS) proporciona una solución flexible para la implementación de un clúster de Kubernetes administrado en Azure. A medida que aumentan las demandas de recursos, el escalador automático de clúster permite que el clúster crezca para satisfacer la demanda en base a las restricciones que el usuario establezca. Para ello, el escalador automático de clúster escala los nodos de agente según los pods pendientes. Examina el clúster periódicamente para comprobar si hay pods pendientes o nodos vacíos y aumenta el tamaño si es posible. De forma predeterminada, el escalador automático de clúster examina en busca de pods pendientes cada 10 segundos, y si un nodo no se necesita durante más de 10 minutos lo quita. Cuando se usa con el escalador automático de pod horizontal (HPA), el HPA actualizará las réplicas de pod y los recursos según la demanda. Si tras este escalado de pod no hay suficientes nodos o hay nodos innecesarios después del escalado de pod, el escalador automático responderá y programará los pods en el nuevo conjunto de nodos.

> [!IMPORTANT]
> La integración del escalador automático de clúster de Azure Kubernetes Service (AKS) está actualmente en **versión preliminar**. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.
>

## <a name="prerequisites"></a>Requisitos previos

En este documento se supone que tiene un clúster de AKS habilitado para RBAC. Si necesita un clúster de AKS, consulte la [guía de inicio rápido de Azure Kubernetes Service (AKS)][aks-quick-start].

 Para usar el escalador automático del clúster, el clúster tiene que usar Kubernetes v1.10.X o superior y tiene que estar habilitado para RBAC. Para actualizar el clúster, consulte el artículo sobre [actualizar un clúster AKS][aks-upgrade].

## <a name="gather-information"></a>Recopilación de información

En la lista siguiente se muestra toda la información que tiene que proporcionar en la definición del escalador automático.

- *Identificador de suscripción*: el identificador correspondiente a la suscripción usada para este clúster
- *Nombre del grupo de recursos*: nombre del grupo de recursos al que pertenece el clúster 
- *Nombre del clúster*: nombre del clúster
- *Identificador de cliente*: identificador de aplicación concedido por un paso de generación de permiso
- *Secreto de cliente*: secreto de aplicación concedido por un paso de generación de permiso
- *Identificador de inquilino*: identificador del inquilino (propietario de la cuenta)
- *Grupo de recursos de nodo*: nombre del grupo de recursos que contiene la los nodos de agente en el clúster
- *Nombre del grupo de nodos*: nombre del grupo de nodos que desea escalar
- *Número mínimo de nodos*: número mínimo de nodos que van a existir en el clúster
- *Número máximo de nodos*: número máximo de nodos que van a existir en el clúster
- *Tipo de máquina virtual*: servicio que se usa para generar el clúster de Kubernetes

Obtenga el identificador de su suscripción con: 

``` azurecli
az account show --query id
```

Genere un conjunto de credenciales de Azure, ejecutando el comando siguiente:

```console
$ az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription-id>" --output json

"appId": <app-id>,
"displayName": <display-name>,
"name": <name>,
"password": <app-password>,
"tenant": <tenant-id>
```

El identificador de aplicación, la contraseña y el identificador de inquilino serán su clientID, clientSecret y tenantID en los pasos siguientes.

Obtenga en nombre de su grupo de nodos ejecutando el siguiente comando. 

```console
$ kubectl get nodes --show-labels
```

Salida:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

A continuación, extraiga el valor de la etiqueta **agentpool**. El nombre predeterminado para el grupo de nodos de un clúster es "nodepool1".

Para obtener el nombre del grupo de recursos de nodo, extraiga el valor de la etiqueta **kubernetes.azure.com<span></span>/cluster**. El nombre del grupo de recursos de nodo tiene generalmente el formato MC_ [grupo de recursos]\_[nombre-clúster] _ [ubicación].

El parámetro vmType hace referencia al servicio que se va a usar, que es en este caso AKS.

Ahora debe tener a mano la siguiente información:

- SubscriptionID
- ResourceGroup
- ClusterName
- ClientID
- ClientSecret
- TenantID
- NodeResourceGroup
- VMType

A continuación, codifique todos estos valores con base64. Por ejemplo, para codificar el valor VMType con base64:

```console
$ echo AKS | base64
QUtTCg==
```

## <a name="create-secret"></a>Creación de un secreto
Con estos datos, cree un secreto para la implementación con los valores que se encuentran en los pasos anteriores en el formato siguiente:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>
  ResourceGroup: <base64-encoded-resource-group>
  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

## <a name="create-a-deployment-chart"></a>Creación de un grupo de implementación

Cree un archivo denominado `aks-cluster-autoscaler.yaml` y copie en él el siguiente código YAML.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["events","endpoints"]
  verbs: ["create", "patch"]
- apiGroups: [""]
  resources: ["pods/eviction"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["pods/status"]
  verbs: ["update"]
- apiGroups: [""]
  resources: ["endpoints"]
  resourceNames: ["cluster-autoscaler"]
  verbs: ["get","update"]
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["watch","list","get","update"]
- apiGroups: [""]
  resources: ["pods","services","replicationcontrollers","persistentvolumeclaims","persistentvolumes"]
  verbs: ["watch","list","get"]
- apiGroups: ["extensions"]
  resources: ["replicasets","daemonsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["policy"]
  resources: ["poddisruptionbudgets"]
  verbs: ["watch","list"]
- apiGroups: ["apps"]
  resources: ["statefulsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["storage.k8s.io"]
  resources: ["storageclasses"]
  verbs: ["get", "list", "watch"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: Role
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["configmaps"]
  resourceNames: ["cluster-autoscaler-status"]
  verbs: ["delete","get","update"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: RoleBinding
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cluster-autoscaler
  template:
    metadata:
      labels:
        app: cluster-autoscaler
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
      - image: k8s.gcr.io/cluster-autoscaler:{{ ca_version }}
        imagePullPolicy: Always
        name: cluster-autoscaler
        resources:
          limits:
            cpu: 100m
            memory: 300Mi
          requests:
            cpu: 100m
            memory: 300Mi
        command:
        - ./cluster-autoscaler
        - --v=3
        - --logtostderr=true
        - --cloud-provider=azure
        - --skip-nodes-with-local-storage=false
        - --nodes=1:10:nodepool1
        env:
        - name: ARM_SUBSCRIPTION_ID
          valueFrom:
            secretKeyRef:
              key: SubscriptionID
              name: cluster-autoscaler-azure
        - name: ARM_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: ResourceGroup
              name: cluster-autoscaler-azure
        - name: ARM_TENANT_ID
          valueFrom:
            secretKeyRef:
              key: TenantID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_ID
          valueFrom:
            secretKeyRef:
              key: ClientID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_SECRET
          valueFrom:
            secretKeyRef:
              key: ClientSecret
              name: cluster-autoscaler-azure
        - name: ARM_VM_TYPE
          valueFrom:
            secretKeyRef:
              key: VMType
              name: cluster-autoscaler-azure
        - name: AZURE_CLUSTER_NAME
          valueFrom:
            secretKeyRef:
              key: ClusterName
              name: cluster-autoscaler-azure
        - name: AZURE_NODE_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: NodeResourceGroup
              name: cluster-autoscaler-azure
      restartPolicy: Always
```

Copie y pegue el secreto que creó en el paso anterior e insértelo al principio del archivo.

A continuación, para establecer el rango de nodos, rellene el argumento para `--nodes` en `command` en el formulario MIN:MAX:NODE_POOL_NAME. Por ejemplo: `--nodes=3:10:nodepool1` establece el número mínimo de nodos en 3, el número máximo de nodos en 10 y el nombre del grupo de nodos como nodepool1.

A continuación, rellene el campo de imagen en **containers** con la versión del escalador automático de clúster que desea usar. AKS requiere la versión v1.2.2 o una más reciente. En este ejemplo se utiliza v1.2.2.

## <a name="deployment"></a>Implementación

Implemente el escalador automático de clúster ejecutando

```console
kubectl create -f cluster-autoscaler-containerservice.yaml
```

Para comprobar si se está ejecutando el escalador automático de clúster, use el comando siguiente y compruebe la lista de pods. Si se está ejecutando un pod precedido de "cluster-autoscaler", quiere decir que el escalador automático de clúster se ha implementado.

```console
kubectl -n kube-system get pods
```

Para ver el estado del escalador automático de clúster, ejecute

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

## <a name="next-steps"></a>Pasos siguientes

Para usar el escalador automático del clúster con el escalador automático horizontal de pod, consulte [Escalado de infraestructura y aplicaciones de Kubernetes][aks-tutorial-scale].

Obtenga más información sobre la implementación y administración de AKS con los tutoriales de AKS.

> [!div class="nextstepaction"]
> [Tutorial de AKS][aks-tutorial-prepare-app]

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md

<!-- LINKS - external -->
[cluster-autoscale]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md
