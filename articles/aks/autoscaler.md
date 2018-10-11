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
ms.openlocfilehash: 3bac6534f43d62e6eb9381b8513025ba9117ed04
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857013"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Escalador automático en Azure Kubernetes Service (AKS): Versión preliminar

Azure Kubernetes Service (AKS) proporciona una solución flexible para la implementación de un clúster de Kubernetes administrado en Azure. A medida que aumentan las demandas de recursos, el escalador automático de clúster permite que el clúster crezca para satisfacer la demanda en base a las restricciones que el usuario establezca. Para ello, el escalador automático de clúster escala los nodos de agente según los pods pendientes. Examina el clúster periódicamente para comprobar si hay pods pendientes o nodos vacíos y aumenta el tamaño si es posible. De forma predeterminada, el escalador automático de clúster examina en busca de pods pendientes cada 10 segundos, y si un nodo no se necesita durante más de 10 minutos lo quita. Cuando se usa con el [escalador automático de pod horizontal](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) (HPA), el HPA actualiza las réplicas de pod y los recursos según la demanda. Si tras este escalado de pod no hay suficientes nodos o hay nodos innecesarios, la entidad de certificación responderá y programará los pods en el nuevo conjunto de nodos.

En este artículo se describe cómo implementar el escalador automático del clúster en los nodos de agente. Sin embargo, puesto que el escalador automático del clúster se implementa en el espacio de nombres del sistema kube, no reducirá verticalmente el nodo que ejecuta ese pod.

> [!IMPORTANT]
> La integración del escalador automático de clúster de Azure Kubernetes Service (AKS) está actualmente en **versión preliminar**. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.
>

## <a name="prerequisites"></a>Requisitos previos

En este documento se supone que tiene un clúster de AKS habilitado para RBAC. Si necesita un clúster de AKS, consulte la [guía de inicio rápido de Azure Kubernetes Service (AKS)][aks-quick-start].

 Para usar el escalador automático del clúster, el clúster tiene que usar Kubernetes v1.10.X o superior y tiene que estar habilitado para RBAC. Para actualizar el clúster, consulte el artículo sobre [actualizar un clúster AKS][aks-upgrade].

## <a name="gather-information"></a>Recopilación de información

Para generar los permisos para que el escalador automático del clúster se ejecute en el clúster, ejecute este script de Bash:

```sh
#! /bin/bash
ID=`az account show --query id -o json`
SUBSCRIPTION_ID=`echo $ID | tr -d '"' `

TENANT=`az account show --query tenantId -o json`
TENANT_ID=`echo $TENANT | tr -d '"' | base64`

read -p "What's your cluster name? " cluster_name
read -p "Resource group name? " resource_group

CLUSTER_NAME=`echo $cluster_name | base64`
RESOURCE_GROUP=`echo $resource_group | base64`

PERMISSIONS=`az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$SUBSCRIPTION_ID" -o json`
CLIENT_ID=`echo $PERMISSIONS | sed -e 's/^.*"appId"[ ]*:[ ]*"//' -e 's/".*//' | base64`
CLIENT_SECRET=`echo $PERMISSIONS | sed -e 's/^.*"password"[ ]*:[ ]*"//' -e 's/".*//' | base64`

SUBSCRIPTION_ID=`echo $ID | tr -d '"' | base64 `

NODE_RESOURCE_GROUP=`az aks show --name $cluster_name  --resource-group $resource_group -o tsv --query 'nodeResourceGroup' | base64`

echo "---
apiVersion: v1
kind: Secret
metadata:
    name: cluster-autoscaler-azure
    namespace: kube-system
data:
    ClientID: $CLIENT_ID
    ClientSecret: $CLIENT_SECRET
    ResourceGroup: $RESOURCE_GROUP
    SubscriptionID: $SUBSCRIPTION_ID
    TenantID: $TENANT_ID
    VMType: QUtTCg==
    ClusterName: $CLUSTER_NAME
    NodeResourceGroup: $NODE_RESOURCE_GROUP
---"
```

Después de seguir los pasos del script, el script generará los detalles en forma de un secreto, como:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>$
  ResourceGroup: <base64-encoded-resource-group>  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

A continuación, obtenga el nombre de su grupo de nodos con el siguiente comando. 

```console
$ kubectl get nodes --show-labels
```

Salida:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

A continuación, extraiga el valor de la etiqueta **agentpool**. El nombre predeterminado para el grupo de nodos de un clúster es "nodepool1".

Ahora, con el secreto y el grupo de nodos puede crear un gráfico de implementación.

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
      - image: gcr.io/google-containers/cluster-autoscaler:v1.2.2
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
kubectl create -f aks-cluster-autoscaler.yaml
```

Para comprobar si se está ejecutando el escalador automático de clúster, use el comando siguiente y compruebe la lista de pods. Debe haber un pod precedido por un "escalador automático del clúster" en ejecución. Si ve esto, significa que se ha implementado el escalador automático del clúster.

```console
kubectl -n kube-system get pods
```

Para ver el estado del escalador automático de clúster, ejecute

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

## <a name="interpreting-the-cluster-autoscaler-status"></a>Interpretación del estado del escalador automático del clúster

```console
$ kubectl -n kube-system describe configmap cluster-autoscaler-status
Name:         cluster-autoscaler-status
Namespace:    kube-system
Labels:       <none>
Annotations:  cluster-autoscaler.kubernetes.io/last-updated=2018-07-25 22:59:22.661669494 +0000 UTC

Data
====
status:
----
Cluster-autoscaler status at 2018-07-25 22:59:22.661669494 +0000 UTC:
Cluster-wide:
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 registered=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC

NodeGroups:
  Name:        nodepool1
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0 cloudProviderTarget=1 (minSize=1, maxSize=5))
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 cloudProviderTarget=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC


Events:  <none>
```

El estado del escalador automático del clúster le permite ver el estado del escalador automático de clúster en dos niveles diferentes: en todo el clúster y dentro de cada grupo de nodos. Puesto que AKS actualmente solo admite un grupo de nodos, estas métricas son las mismas.

* El mantenimiento indica el mantenimiento general de los nodos. Si el escalador automático del clúster tiene problemas para crear o quitar nodos del clúster, este estado cambia a "Unhealthy" (Incorrecto). También hay un desglose del estado de los distintos nodos:
    * "Ready" (Listo) significa que un nodo está preparado para contener pods programados.
    * "Unready" (No listo) significa un nodo que se desglosa una vez que se inicia.
    * "NotStarted" significa que un nodo no se inició totalmente aún.
    * "LongNotStarted" significa que un nodo no se pudo iniciar en un plazo razonable.
    * "Registered" (Registrado) significa que un nodo se registró en el grupo
    * "Unregistered" (No registrado) significa que existe un nodo en el lado del proveedor del clúster pero no se pudo registrar en Kubernetes.
  
* ScaleUp permite comprobar cuándo el clúster determina que se debe producir un escalado vertical en el clúster.
    * Una transición es cuando cambia el número de nodos del clúster o cambia el estado de un nodo.
    * El número de nodos preparados es el número de nodos disponibles y preparados en el clúster. 
    * cloudProviderTarget es el número de nodos que el escalador automático del clúster ha determinado que necesita el clúster para controlar su carga de trabajo.

* ScaleDown le permite comprobar si hay candidatos para la reducción vertical. 
    * Un candidato a reducción vertical es un nodo que el escalador automático del clúster ha determinado que se puede quitar sin que afecte a la capacidad del clúster para controlar su carga de trabajo. 
    * Los tiempos proporcionados muestran la última vez que se buscaron en el clúster candidatos de reducción vertical y la hora de su última transición.

Por último, en Events (Eventos), puede ver los eventos de escalado o reducción vertical, correctos o incorrectos, y sus tiempos, que el escalador automático del clúster ha llevado a cabo.

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
