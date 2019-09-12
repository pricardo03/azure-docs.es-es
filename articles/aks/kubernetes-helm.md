---
title: Implementación de contenedores con Helm en Kubernetes en Azure
description: Aprenda a usar la herramienta de empaquetado de Helm para implementar contenedores en un clúster de Azure Kubernetes Service (AKS)
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 05/23/2019
ms.author: zarhoads
ms.openlocfilehash: bc74ac660c5bba0624416d0a1724d959a4c385a7
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70305274"
---
# <a name="install-applications-with-helm-in-azure-kubernetes-service-aks"></a>Instalación de aplicaciones con Helm en Azure Kubernetes Service (AKS)

[Helm][helm] es una herramienta de empaquetado de código abierto que ayuda a instalar y administrar el ciclo de vida de las aplicaciones de Kubernetes. Helm, que funciona de forma similar a los administradores de paquetes de Linux como *APT* y *Yum*, se utiliza para administrar los gráficos de Kubernetes, que son paquetes de recursos de Kubernetes preconfigurados.

En este artículo se muestra cómo configurar y usar Helm en un clúster de Kubernetes en AKS.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

También necesita tener instalada la CLI de Helm, que es el cliente que se ejecuta en el sistema de desarrollo. Permite iniciar, detener y administrar las aplicaciones con Helm. Si usa Azure Cloud Shell, la CLI de Helm ya está instalada. Para obtener las instrucciones de instalación en su plataforma local, consulte [Instalación de Helm][helm-install].

> [!IMPORTANT]
> Helm está diseñada para ejecutarse en nodos de Linux. Si tiene nodos de Windows Server en el clúster, debe asegurarse de que los pods de Helm solo se programan para ejecutarse en nodos de Linux. También deberá asegurarse de que todos los gráficos de Helm que instale están programados también para ejecutarse en los nodos correctos. Los comandos de este artículo utilizan [selectores de nodo][k8s-node-selector] para asegurarse de que los pods están programados para los nodos correctos, pero no todos los gráficos de Helm pueden exponer un selector de nodo. También puede considerar otras opciones en el clúster, como los valores [taints][taints].

## <a name="create-a-service-account"></a>Creación de una cuenta de servicio

Antes de implementar Helm en un clúster de AKS habilitado para RBAC, necesita una cuenta de servicio y el enlace de rol para el servicio Tiller. Para más información sobre cómo proteger Helm o Tiller en un clúster habilitado para RBAC, consulte [Tiller, Namespaces, and RBAC][tiller-rbac] (Tiller, espacios de nombres y RBAC). Si el clúster de AKS no está habilitado para RBAC, omita este paso.

Cree un archivo denominado `helm-rbac.yaml` y cópielo en el siguiente código YAML:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Cree la cuenta de servicio y el enlace de rol con el comando `kubectl apply`:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="secure-tiller-and-helm"></a>Protección de Tiller y Helm

El cliente de Helm y el servicio Tiller se autentican y comunican entre sí mediante TLS/SSL. Este método de autenticación ayuda a proteger el clúster de Kubernetes y a determinar qué servicios se pueden implementar. Para mejorar la seguridad, puede generar sus propios certificados firmados. Cada usuario de Helm recibiría su propio certificado de cliente, y Tiller se inicializaría en el clúster de Kubernetes con los certificados aplicados. Para más información, consulte [Uso de TLS/SSL entre Helm y Tiller][helm-ssl].

Con un clúster de Kubernetes habilitado para RBAC, puede controlar el nivel de acceso de Tiller al clúster. Puede definir el espacio de nombres de Kubernetes en el que Tiller está implementado y restringir los espacios de nombres en los que Tiller puede implementar los recursos. Este enfoque le permite crear instancias de Tiller en diferentes espacios de nombres y establecer los límites de implementación, así como reducir el ámbito de los usuarios del cliente de Helm a determinados espacios de nombres. Para más información, consulte los [controles de acceso basado en rol de Helm][helm-rbac].

## <a name="configure-helm"></a>Configuración de Helm

Para implementar un servicio básico de Tiller en un clúster de AKS, use el comando [helm init][helm-init]. Si el clúster no está habilitado para RBAC, quite el argumento `--service-account` y el valor. Los siguientes ejemplos también establecen el [Historial-Max][helm-history-max] en 200.

Si ha configurado SSL/TLS para Tiller y Helm, omita este paso de inicialización básica y, en su lugar, proporcione los parámetros `--tiller-tls-` necesarios, tal como se muestra en el ejemplo siguiente.

```console
helm init --history-max 200 --service-account tiller --node-selectors "beta.kubernetes.io/os=linux"
```

Si ha configurado TLS/SSL entre Helm y Tiller, proporcione los parámetros `--tiller-tls-*` y los nombres de sus propios certificados, tal como se muestra en el ejemplo siguiente:

```console
helm init \
    --tiller-tls \
    --tiller-tls-cert tiller.cert.pem \
    --tiller-tls-key tiller.key.pem \
    --tiller-tls-verify \
    --tls-ca-cert ca.cert.pem \
    --history-max 200 \
    --service-account tiller \
    --node-selectors "beta.kubernetes.io/os=linux"
```

## <a name="find-helm-charts"></a>Búsqueda de gráficos de Helm

Los gráficos de Helm se usan para implementar aplicaciones en un clúster de Kubernetes. Para buscar los gráficos de Helm creados previamente, use el comando [helm search][helm-search]:

```console
helm search
```

En la siguiente salida de ejemplo condensada se muestran algunos de los gráficos de Helm disponibles para usar:

```
$ helm search

NAME                           CHART VERSION    APP VERSION  DESCRIPTION
stable/aerospike               0.1.7            v3.14.1.2    A Helm chart for Aerospike in Kubernetes
stable/anchore-engine          0.1.7            0.1.10       Anchore container analysis and policy evaluatio...
stable/apm-server              0.1.0            6.2.4        The server receives data from the Elastic APM a...
stable/ark                     1.0.1            0.8.2        A Helm chart for ark
stable/artifactory             7.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/artifactory-ha          0.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/auditbeat               0.1.0            6.2.4        A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler  0.3.3                         Scales worker nodes within autoscaling groups.
stable/bitcoind                0.1.3            0.15.1       Bitcoin is an innovative payment network and a ...
stable/buildkite               0.2.3            3            Agent for Buildkite
stable/burrow                  0.4.4            0.17.1       Burrow is a permissionable smart contract machine
stable/centrifugo              2.0.1            1.7.3        Centrifugo is a real-time messaging server.
stable/cerebro                 0.1.0            0.7.3        A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager            v0.3.3           v0.3.1       A Helm chart for cert-manager
stable/chaoskube               0.7.0            0.8.0        Chaoskube periodically kills random pods in you...
stable/chartmuseum             1.5.0            0.7.0        Helm Chart Repository with support for Amazon S...
stable/chronograf              0.4.5            1.3          Open-source web application written in Go and R...
stable/cluster-autoscaler      0.6.4            1.2.2        Scales worker nodes within autoscaling groups.
stable/cockroachdb             1.1.1            2.0.0        CockroachDB is a scalable, survivable, strongly...
stable/concourse               1.10.1           3.14.1       Concourse is a simple and scalable CI system.
stable/consul                  3.2.0            1.0.0        Highly available and distributed service discov...
stable/coredns                 0.9.0            1.0.6        CoreDNS is a DNS server that chains plugins and...
stable/coscale                 0.2.1            3.9.1        CoScale Agent
stable/dask                    1.0.4            0.17.4       Distributed computation in Python with task sch...
stable/dask-distributed        2.0.2                         DEPRECATED: Distributed computation in Python
stable/datadog                 0.18.0           6.3.0        DataDog Agent
...
```

Para actualizar la lista de gráficos, use el comando [helm repo update][helm-repo-update]. El ejemplo siguiente muestra una actualización de repositorio correcta:

```console
$ helm repo update

Hold tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete.
```

## <a name="run-helm-charts"></a>Ejecución de gráficos de Helm

Para instalar los gráficos con Helm, use el comando [helm install][helm-install] y especifique el nombre del gráfico que se instalará. Para ver cómo funciona la instalación de un gráfico de Helm, vamos a instalar una implementación básica de nginx mediante un gráfico de Helm. Si ha configurado SSL/TLS, agregue el parámetro `--tls` para usar el certificado de cliente de Helm.

```console
helm install stable/nginx-ingress \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

En la siguiente salida de ejemplo condensada se muestra el estado de implementación de los recursos de Kubernetes creados mediante el gráfico de Helm:

```
$ helm install stable/nginx-ingress --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

NAME:   flailing-alpaca
LAST DEPLOYED: Thu May 23 12:55:21 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                      DATA  AGE
flailing-alpaca-nginx-ingress-controller  1     0s

==> v1/Pod(related)
NAME                                                            READY  STATUS             RESTARTS  AGE
flailing-alpaca-nginx-ingress-controller-56666dfd9f-bq4cl       0/1    ContainerCreating  0         0s
flailing-alpaca-nginx-ingress-default-backend-66bc89dc44-m87bp  0/1    ContainerCreating  0         0s

==> v1/Service
NAME                                           TYPE          CLUSTER-IP  EXTERNAL-IP  PORT(S)                     AGE
flailing-alpaca-nginx-ingress-controller       LoadBalancer  10.0.109.7  <pending>    80:31219/TCP,443:32421/TCP  0s
flailing-alpaca-nginx-ingress-default-backend  ClusterIP     10.0.44.97  <none>       80/TCP                      0s
...
```

Se tarda entre un minuto y dos en que la dirección *EXTERNAL-IP* del servicio nginx-ingress-controller se llene y se le permita acceder a este servicio con un explorador web.

## <a name="list-helm-releases"></a>Lista de versiones de Helm

Para ver una lista de las versiones instaladas en el clúster, use el comando [helm list][helm-list]. En el ejemplo siguiente se muestra la versión de nginx-ingress implementada en el paso anterior. Si ha configurado SSL/TLS, agregue el parámetro `--tls` para usar el certificado de cliente de Helm.

```console
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                 APP VERSION   NAMESPACE
flailing-alpaca   1         Thu May 23 12:55:21 2019    DEPLOYED    nginx-ingress-1.6.13    0.24.1      default
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Al implementar un gráfico de Helm, se crean algunos recursos de Kubernetes. Estos recursos incluyen pods, implementaciones y servicios. Para limpiar estos recursos, use el comando `helm delete` y especifique el nombre de la versión, tal como hemos visto en el comando `helm list` anterior. En el ejemplo siguiente se elimina la versión denominada *flailing-alpaca*:

```console
$ helm delete flailing-alpaca

release "flailing-alpaca" deleted
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo administrar las implementaciones de aplicaciones de Kubernetes con Helm, consulte la documentación de Helm.

> [!div class="nextstepaction"]
> [Documentación de Helm][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-history-max]: https://helm.sh/docs/using_helm/#initialize-helm-and-install-tiller
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac
[helm-ssl]: https://docs.helm.sh/using_helm/#using-ssl-between-helm-and-tiller

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
[taints]: operator-best-practices-advanced-scheduler.md
