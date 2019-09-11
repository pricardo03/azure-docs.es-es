---
title: Instalación de Hashicorp Consul en Azure Kubernetes Service (AKS)
description: Aprenda cómo instalar y usar Consul para crear una malla de servicio en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: dstrebel
ms.service: container-service
ms.topic: article
ms.date: 06/19/2019
ms.author: dastrebe
ms.openlocfilehash: 7acd2533079499091427c7e63741b9c587ee29e5
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70189081"
---
# <a name="install-and-use-consul-connect-in-azure-kubernetes-service-aks"></a>Instalación y uso de Consul Connect en Azure Kubernetes Service (AKS)

[Consul][consul-github] es una malla de servicio de código abierto que proporciona un conjunto clave de funcionalidades en los microservicios de un clúster de Kubernetes. Estas características incluyen la detección y segmentación de servicios, la comprobación de estado y la observación. Para más información sobre Consul, consulte la documentación oficial en [¿Qué es Consul?][consul-docs-concepts].

En este artículo se explica cómo instalar Consul. Los componentes de Consul se instalan en un clúster de Kubernetes en AKS.

> [!NOTE]
> Estas instrucciones hacen referencia a la versión de Consul `1.5`.
>
> El equipo de Hashicorp ha probado las versiones `1.5.x` de Consul con las versiones de Kubernetes `1.12`, `1.14`, `1.14`. Puede encontrar versiones de Consul adicionales en [GitHub: versiones de Consul][consul-github-releases] e información sobre cada una de las versiones en [Consul: notas de la versión][consul-release-notes].

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Instalar los componentes de Consul en AKS
> * Validar la instalación de Consul
> * Desinstalar Consul de AKS

## <a name="before-you-begin"></a>Antes de empezar

En los pasos detallados en este artículo se da por hecho que ha creado un clúster de AKS (Kubernetes `1.11` y versiones posteriores, con RBAC habilitado) y que ha establecido una conexión `kubectl` con el clúster. Si necesita ayuda con cualquiera de estos elementos, consulte el [inicio rápido de AKS][aks-quickstart].

Necesitará [Helm][helm] para seguir estas instrucciones e instalar Consul. Es recomendable que tenga la versión `2.12.2` o posterior instalada y configurada correctamente en su clúster. Si necesita ayuda con la instalación de Helm, consulte las [instrucciones de instalación de Helm para AKS][helm-install]. Todos los pods de Consul también deben programarse para ejecutarse en nodos de Linux.

En este artículo, las instrucciones de instalación de Consul se dividen en varios pasos discretos. El resultado final presenta la misma estructura que las [instrucciones][consul-install-k8] de instalación oficiales de Consul.

### <a name="install-the-consul-components-on-aks"></a>Instalar los componentes de Consul en AKS

En primer lugar, se clonará la versión oficial de Consul de HashiCorp en el repositorio de GitHub de Kubernetes.

```bash
git clone https://github.com/hashicorp/consul-helm.git
cd consul-helm
```

A continuación, se debe crear un archivo de valores de Helm personalizado para la instalación de Consul. Cree el siguiente archivo de valores personalizado antes de instalar Consul.

```bash
vim consul-values.yaml
```

Luego, copie y pegue los valores siguientes en el archivo consul-values.yaml.

```yaml
# Sets datacenter name and version Consul to use
global:
  datacenter: dc1
  image: "consul:1.5.2"

# Enables proxies to be injected into pods
connectInject:
  enabled: true

# Enables UI on ClusterIP
ui:
  service:
    type: "ClusterIP"

# Enables GRCP that is required for connectInject
client:
  enabled: true
  grpc: true

# Sets replicase to 3 for HA
server:
  replicas: 3
  bootstrapExpect: 1
  disruptionBudget:
    enabled: true
    maxUnavailable: 1

# Syncs Kubernetes service discovery with Consul
syncCatalog:
  enabled: true
```

Ahora que tenemos el archivo de valores personalizado, se puede instalar Consul en el clúster de AKS.

Bash

```bash
helm install -f consul-values.yaml --name consul --namespace consul .
```
El gráfico de Helm `Consul` implementa una serie de objetos. Puede ver la lista de la salida del comando `helm install` anterior. La implementación de los componentes de Consul puede tardar entre 4 y 5 minutos, según el entorno del clúster.

> [!NOTE]
> Todos los pods de Consul deben programarse para ejecutarse en nodos de Linux. Si tiene grupos de nodos de Windows Server además de grupos de nodos de Linux en su clúster, compruebe que todos los pods de Consul se hayan programado para ejecutarse en nodos de Linux.

En este punto, ya tendrá implementado Consul en su clúster de AKS. Para asegurarse de que ha implementado correctamente Consul, vaya a la siguiente sección para validar su instalación.

## <a name="validate-the-consul-installation"></a>Validar la instalación de Consul

En primer lugar, confirme que se han creado los servicios esperados. Use el comando [kubectl get svc][kubectl-get] para ver los servicios en ejecución. Consulte el espacio de nombres `consul` donde se instalaron los componentes de Consul mediante el gráfico de Helm `consul`:

```console
kubectl get svc --namespace consul
```

La salida del ejemplo siguiente muestra los servicios que ahora deben estar en ejecución:

```console
NAME                                 TYPE           CLUSTER-IP     EXTERNAL-IP             PORT(S)                                                                   AGE
consul                               ExternalName   <none>         consul.service.consul   <none>                                                                    38m
consul-consul-connect-injector-svc   ClusterIP      10.0.89.113    <none>                  443/TCP                                                                   40m
consul-consul-dns                    ClusterIP      10.0.166.82    <none>                  53/TCP,53/UDP                                                             40m
consul-consul-server                 ClusterIP      None           <none>                  8500/TCP,8301/TCP,8301/UDP,8302/TCP,8302/UDP,8300/TCP,8600/TCP,8600/UDP   40m
consul-consul-ui                     ClusterIP      10.0.117.164   <none>                  80/TCP                                                                    40m
```

A continuación, confirme que se han creado los pods necesarios. Use el comando [kubectl get pods][kubectl-get] y vuelva a consultar el espacio de nombres `consul`:

```console
kubectl get pods --namespace consul
```

La salida de ejemplo siguiente muestra los pods que se ejecutan:

```console
NAME                                                              READY   STATUS    RESTARTS   AGE
consul-consul-7cc9v                                               1/1     Running   0          37m
consul-consul-7klg7                                               1/1     Running   0          37m
consul-consul-connect-injector-webhook-deployment-57f88df8hgmfs   1/1     Running   0          37m
consul-consul-lq8qb                                               1/1     Running   0          37m
consul-consul-server-0                                            1/1     Running   0          37m
consul-consul-server-1                                            1/1     Running   0          36m
consul-consul-server-2                                            1/1     Running   0          36m
consul-consul-sync-catalog-7cf7d5bfff-jjbjv                       1/1     Running   2          37m
```

 Todos los pods deben mostrar un estado de `Running`. Si los pods no tienen estos estados, espere un minuto o dos hasta que los tengan. Si algún pod notifica un problema, use el comando [kubectl describe pod][kubectl-describe] para revisar su salida y su estado.

## <a name="accessing-the-consul-ui"></a>Acceso a la interfaz de usuario de Consul

La interfaz de usuario de Consul se instaló en una operación anterior que proporciona la configuración de Consul basada en la interfaz de usuario. La interfaz de usuario de Consul no se expone públicamente a través de una dirección IP externa. Para acceder a las interfaces de usuario de los complementos, use el comando [kubectl port-forward][kubectl-port-forward]. Este comando crea una conexión segura el equipo cliente y el pod relevante en el clúster de AKS.

```bash
kubectl port-forward -n consul svc/consul-consul-ui 8080:80
```
Ahora puede abrir un explorador y hacer que apunte a `http://localhost:8080/ui` para abrir la interfaz de usuario de Consul. Al abrir la interfaz de usuario, verá lo siguiente:

![Interfaz de usuario de Consul](./media/consul/consul-ui.png)

## <a name="uninstall-consul-from-aks"></a>Desinstalar Consul de AKS

> [!WARNING]
> Eliminar Consul de un sistema en ejecución puede provocar problemas relacionados con el tráfico entre sus servicios. Asegúrese de que su sistema esté bien aprovisionado de forma que pueda funcionar correctamente sin Consul antes de continuar.

### <a name="remove-consul-components-and-namespace"></a>Quitar los componentes y el espacio de nombres de Consul

Para quitar Consul del clúster de AKS, use los siguientes comandos. Los comandos `helm delete` quitarán el gráfico `consul`, y el comando `kubectl delete ns` quitará el espacio de nombres `consul`.

```bash
helm delete --purge consul
kubectl delete ns consul
```

## <a name="next-steps"></a>Pasos siguientes

Para explorar más opciones de instalación y configuración de Consul, consulte los siguientes artículos oficiales:

- [Consul: guía de instalación de Helm][consul-install-k8]
- [Consul: opciones de instalación de Helm][consul-install-helm-options]

Puede consultar también escenarios adicionales con la [aplicación de ejemplo de Consul][consul-app-example].

<!-- LINKS - external -->
[Hashicorp]: https://hashicorp.com
[cosul-github]: https://github.com/hashicorp/consul
[helm]: https://helm.sh

[consul-docs-concepts]: https://www.consul.io/docs/platform/k8s/index.html
[consul-github]: https://github.com/hashicorp/consul
[consul-github-releases]: https://github.com/hashicorp/consul/releases
[consul-release-notes]: https://github.com/hashicorp/consul/blob/master/CHANGELOG.md
[consul-install-download]: https://www.consul.io/downloads.html
[consul-install-k8]: https://www.consul.io/docs/platform/k8s/run.html
[consul-install-helm-options]: https://www.consul.io/docs/platform/k8s/helm.html#configuration-values-
[consul-app-example]: https://github.com/hashicorp/demo-consul-101/tree/master/k8s
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[consul-scenario-mtls]: ./consul-mtls.md
[helm-install]: ./kubernetes-helm.md
