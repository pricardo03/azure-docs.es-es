---
title: Implementación de contenedores con Helm en Kubernetes en Azure
description: Uso de la herramienta de empaquetado de Helm para implementar contenedores en un clúster de Kubernetes en AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 531e6d9368b2bf91c48fd41b1e9330879b0df49a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101842"
---
# <a name="use-helm-with-azure-kubernetes-service-aks"></a>Uso de Helm con Azure Kubernetes Service (AKS)

[Helm][helm] es una herramienta de empaquetado de código abierto que le ayuda a instalar y administrar el ciclo de vida de las aplicaciones de Kubernetes. Helm, que funciona de forma similar a los administradores de paquetes de Linux como *APT* y *Yum*, se utiliza para administrar los gráficos de Kubernetes, que son paquetes de recursos de Kubernetes preconfigurados.

Este documento trata la configuración y uso de Helm en un clúster de Kubernetes en AKS.

## <a name="before-you-begin"></a>Antes de empezar

En los pasos que se detallan en este documento se asume que se ha creado un clúster de AKS y que ha establecido una conexión kubectl con dicho clúster. Si necesita estos elementos, vea la [Guía de inicio rápido de AKS][aks-quickstart].

## <a name="install-helm-cli"></a>Instalación de la CLI de Helm

La CLI de Helm es un cliente que se ejecuta en su sistema de desarrollo, y que permite iniciar, detener y administrar aplicaciones con Helm.

Si usa Azure CloudShell, la CLI de Helm ya estará instalada. Para instalar la CLI de Helm en un equipo Mac, use `brew`. Para conocer otras opciones de instalación, vea [Installing Helm][helm-install-options] (Instalación de Helm).

```console
brew install kubernetes-helm
```

Salida:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="create-service-account"></a>Creación de la cuenta de servicio

Antes de configurar Helm en un clúster habilitado para RBAC, necesita una cuenta de servicio y el enlace de rol para el servicio Tiller. Para obtener más información sobre cómo proteger Helm/Tiller en un clúster habilitado para RBAC, consulte [Tiller, Namespaces, and RBAC][tiller-rbac] (Tiller, espacios de nombres y RBAC). Nota: Si el clúster no está habilitado para RBAC, omita este paso.

Cree un archivo denominado `helm-rbac.yaml` y cópielo en el siguiente código YAML.

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
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

Cree la cuenta de servicio y el enlace de rol con el comando `kubectl create`.

```
kubectl create -f helm-rbac.yaml
```

Al usar un clúster habilitado para RBAC, tiene opciones en el nivel de acceso que tiene Tiller en el clúster. Consulte [Helm: role-based access controls][helm-rbac] (Helm: controles de acceso basados en roles) para obtener más información sobre las opciones de configuración.

## <a name="configure-helm"></a>Configuración de Helm

Ahora instale Tiller utilizando el comando [helm init][helm-init]. Si el clúster no está habilitado para RBAC, quite el argumento `--service-account` y el valor.

```
helm init --service-account tiller
```

## <a name="find-helm-charts"></a>Búsqueda de gráficos de Helm

Los gráficos de Helm se usan para implementar aplicaciones en un clúster de Kubernetes. Para buscar los gráficos de Helm creados previamente, use el comando [helm search][helm-search].

```azurecli-interactive
helm search
```

El resulta debe ser similar al siguiente, pero con más gráficos.

```
NAME                            VERSION DESCRIPTION
stable/acs-engine-autoscaler    2.0.0   Scales worker nodes within agent pools
stable/artifactory              6.1.0   Universal Repository Manager supporting all maj...
stable/aws-cluster-autoscaler   0.3.1   Scales worker nodes within autoscaling groups.
stable/buildkite                0.2.0   Agent for Buildkite
stable/centrifugo               2.0.0   Centrifugo is a real-time messaging server.
stable/chaoskube                0.5.0   Chaoskube periodically kills random pods in you...
stable/chronograf               0.3.0   Open-source web application written in Go and R...
stable/cluster-autoscaler       0.2.0   Scales worker nodes within autoscaling groups.
stable/cockroachdb              0.5.0   CockroachDB is a scalable, survivable, strongly...
stable/concourse                0.7.0   Concourse is a simple and scalable CI system.
stable/consul                   0.4.1   Highly available and distributed service discov...
stable/coredns                  0.5.0   CoreDNS is a DNS server that chains middleware ...
stable/coscale                  0.2.0   CoScale Agent
stable/dask-distributed         2.0.0   Distributed computation in Python
stable/datadog                  0.8.0   DataDog Agent
...
```

Para actualizar la lista de gráficos, use el comando [helm repo update][helm-repo-update].

```azurecli-interactive
helm repo update
```

Salida:

```
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Ejecución de gráficos de Helm

Para implementar Wordpress mediante un gráfico de Helm, use el comando [helm install][helm-install].

```azurecli-interactive
helm install stable/wordpress
```

El resultado es similar al siguiente, pero incluye información adicional como instrucciones sobre cómo usar la implementación de Kubernetes.

```
NAME:   bilging-ibex
LAST DEPLOYED: Tue Jun  5 14:31:49 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                     READY  STATUS   RESTARTS  AGE
bilging-ibex-mariadb-7557b5474-dmdxn     0/1    Pending  0         1s
bilging-ibex-wordpress-7494c545fb-tskhz  0/1    Pending  0         1s

==> v1/Secret
NAME                    TYPE    DATA  AGE
bilging-ibex-mariadb    Opaque  2     1s
bilging-ibex-wordpress  Opaque  2     1s

==> v1/ConfigMap
NAME                        DATA  AGE
bilging-ibex-mariadb        1     1s
bilging-ibex-mariadb-tests  1     1s

==> v1/PersistentVolumeClaim
NAME                    STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
bilging-ibex-mariadb    Pending  default  1s
bilging-ibex-wordpress  Pending  default  1s

==> v1/Service
NAME                    TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)                     AGE
bilging-ibex-mariadb    ClusterIP     10.0.76.164   <none>       3306/TCP                    1s
bilging-ibex-wordpress  LoadBalancer  10.0.215.250  <pending>    80:30934/TCP,443:31134/TCP  1s

==> v1beta1/Deployment
NAME                    DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
bilging-ibex-mariadb    1        1        1           0          1s
bilging-ibex-wordpress  1        1        1           0          1s
...
```

## <a name="list-helm-releases"></a>Lista de versiones de Helm

Para ver una lista de las versiones instaladas en el clúster, use el comando [helm list][helm-list].

```azurecli-interactive
helm list
```

Salida:

```
NAME            REVISION    UPDATED                     STATUS      CHART           NAMESPACE
bilging-ibex    1           Tue Jun  5 14:31:49 2018    DEPLOYED    wordpress-1.0.9 default
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo administrar gráficos de Kubernetes, vea la documentación de Helm.

> [!div class="nextstepaction"]
> [Documentación de Helm][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/helm/#helm-install
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
