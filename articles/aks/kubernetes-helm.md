---
title: Implementación de contenedores con Helm en Kubernetes en Azure
description: Uso de la herramienta de empaquetado de Helm para implementar contenedores en un clúster de Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: dd2deba25615373765dd3492d03c1ba547c8ba8c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055141"
---
# <a name="install-applications-with-helm-in-azure-kubernetes-service-aks"></a>Instalación de aplicaciones con Helm en Azure Kubernetes Service (AKS)

[Helm][helm] es una herramienta de empaquetado de código abierto que le ayuda a instalar y administrar el ciclo de vida de las aplicaciones de Kubernetes. Helm, que funciona de forma similar a los administradores de paquetes de Linux como *APT* y *Yum*, se utiliza para administrar los gráficos de Kubernetes, que son paquetes de recursos de Kubernetes preconfigurados.

En este artículo se muestra cómo configurar y usar Helm en un clúster de Kubernetes en AKS.

## <a name="before-you-begin"></a>Antes de empezar

En los pasos que se detallan en este documento se da por hecho que ha creado un clúster de AKS y que ha establecido una conexión `kubectl` con dicho clúster. Si necesita estos elementos, vea la [Guía de inicio rápido de AKS][aks-quickstart].

## <a name="install-helm-cli"></a>Instalación de la CLI de Helm

La CLI de Helm es un cliente que se ejecuta en su sistema de desarrollo, y que permite iniciar, detener y administrar aplicaciones con Helm.

Si usa Azure Cloud Shell, la CLI de Helm ya está instalada. Para instalar la CLI de Helm en un equipo Mac, use `brew`. Para conocer otras opciones de instalación, vea [Installing Helm][helm-install-options] (Instalación de Helm).

```console
brew install kubernetes-helm
```

Salida:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.9.1.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.9.1.high_sierra.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.9.1: 50 files, 66.2MB
```

## <a name="create-a-service-account"></a>Creación de una cuenta de servicio

Antes de implementar Helm en un clúster habilitado para RBAC, necesita una cuenta de servicio y el enlace de rol para el servicio Tiller. Para obtener más información sobre cómo proteger Helm/Tiller en un clúster habilitado para RBAC, consulte [Tiller, Namespaces, and RBAC][tiller-rbac] (Tiller, espacios de nombres y RBAC). Si el clúster no está habilitado para RBAC, omita este paso.

Cree un archivo denominado `helm-rbac.yaml` y cópielo en el siguiente código YAML:

```yaml
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

Cree la cuenta de servicio y el enlace de rol con el comando `kubectl create`:

```console
kubectl create -f helm-rbac.yaml
```

## <a name="secure-tiller-and-helm"></a>Protección de Tiller y Helm

El cliente de Helm y el servicio Tiller se autentican y comunican entre sí mediante TLS/SSL. Este método de autenticación ayuda a proteger el clúster de Kubernetes y a determinar qué servicios se pueden implementar. Para mejorar la seguridad, puede generar sus propios certificados firmados. Cada usuario de Helm recibiría su propio certificado de cliente, y Tiller se inicializaría en el clúster de Kubernetes con los certificados aplicados. Para más información, consulte [Using TLS/SSL between Helm and Tiller][helm-ssl] (Uso de TLS/SSL entre Helm y Tiller).

Con un clúster de Kubernetes habilitado para RBAC, puede controlar el nivel de acceso de Tiller al clúster. Puede definir el espacio de nombres de Kubernetes en el que Tiller está implementado y restringir los espacios de nombres en los que Tiller puede implementar los recursos. Este enfoque le permite crear instancias de Tiller en diferentes espacios de nombres y establecer los límites de implementación, así como reducir el ámbito de los usuarios del cliente de Helm a determinados espacios de nombres. Para más información, consulte los [controles de acceso basado en rol de Helm][helm-rbac].

## <a name="configure-helm"></a>Configuración de Helm

Para implementar un servicio básico de Tiller en un clúster de AKS, use el comando [helm init][helm-init]. Si el clúster no está habilitado para RBAC, quite el argumento `--service-account` y el valor. Si ha configurado SSL/TLS para Tiller y Helm, omita este paso de inicialización básica y, en su lugar, proporcione los parámetros `--tiller-tls-` necesarios, tal como se muestra en el ejemplo siguiente.

```console
helm init --service-account tiller
```

Si ha configurado TLS/SSL entre Helm y Tiller, proporcione los parámetros `--tiller-tls-` y los nombres de sus propios certificados, tal como se muestra en el ejemplo siguiente:

```console
helm init \
    --tiller-tls \
    --tiller-tls-cert tiller.cert.pem \
    --tiller-tls-key tiller.key.pem \
    --tiller-tls-verify \
    --tls-ca-cert ca.cert.pem \
    --service-account tiller
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
stable/acs-engine-autoscaler   2.2.0            2.1.1        Scales worker nodes within agent pools
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

Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Ejecución de gráficos de Helm

Para instalar los gráficos con Helm, use el comando [helm install][helm-install] y especifique el nombre del gráfico que se instalará. Para ver este comando en acción, vamos a instalar una implementación básica de Wordpress mediante un gráfico de Helm. Si ha configurado SSL/TLS, agregue el parámetro `--tls` para usar el certificado de cliente de Helm.

```console
helm install stable/wordpress
```

En la siguiente salida de ejemplo condensada se muestra el estado de implementación de los recursos de Kubernetes creados mediante el gráfico de Helm:

```
$ helm install stable/wordpress

NAME:   wishful-mastiff
LAST DEPLOYED: Thu Jul 12 15:53:56 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1beta1/Deployment
NAME                       DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
wishful-mastiff-wordpress  1        1        1           0          1s

==> v1beta1/StatefulSet
NAME                     DESIRED  CURRENT  AGE
wishful-mastiff-mariadb  1        1        1s

==> v1/Pod(related)
NAME                                        READY  STATUS   RESTARTS  AGE
wishful-mastiff-wordpress-6f96f8fdf9-q84sz  0/1    Pending  0         1s
wishful-mastiff-mariadb-0                   0/1    Pending  0         1s

==> v1/Secret
NAME                       TYPE    DATA  AGE
wishful-mastiff-mariadb    Opaque  2     2s
wishful-mastiff-wordpress  Opaque  2     2s

==> v1/ConfigMap
NAME                           DATA  AGE
wishful-mastiff-mariadb        1     2s
wishful-mastiff-mariadb-tests  1     2s

==> v1/PersistentVolumeClaim
NAME                       STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
wishful-mastiff-wordpress  Pending  default  2s

==> v1/Service
NAME                       TYPE          CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
wishful-mastiff-mariadb    ClusterIP     10.1.116.54  <none>       3306/TCP                    2s
wishful-mastiff-wordpress  LoadBalancer  10.1.217.64  <pending>    80:31751/TCP,443:31264/TCP  2s
...
```

La dirección de *IP externa* del servicio Wordpress tarda un minuto o dos en rellenarse y le permite acceder a este servicio con un explorador web.

## <a name="list-helm-releases"></a>Lista de versiones de Helm

Para ver una lista de las versiones instaladas en el clúster, use el comando [helm list][helm-list]. En el ejemplo siguiente se muestra la versión de Wordpress implementada en el paso anterior. Si ha configurado SSL/TLS, agregue el parámetro `--tls` para usar el certificado de cliente de Helm.

```console
$ helm list

NAME             REVISION    UPDATED                     STATUS      CHART              NAMESPACE
wishful-mastiff  1           Thu Jul 12 15:53:56 2018    DEPLOYED    wordpress-2.1.3  default
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo administrar las implementaciones de aplicaciones de Kubernetes con Helm, consulte la documentación de Helm.

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
[helm-ssl]: https://docs.helm.sh/using_helm/#using-ssl-between-helm-and-tiller

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
