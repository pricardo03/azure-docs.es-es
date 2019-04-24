---
title: Personalizar CoreDNS para Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo personalizar CoreDNS adición de subdominios o ampliar los puntos de conexión DNS personalizados con Azure Kubernetes Service (AKS)
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 03/15/2019
ms.author: jnoller
ms.openlocfilehash: 9186c5ff7c6fbc68487a1ccff0fc1d2d1478df79
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466461"
---
# <a name="customize-coredns-with-azure-kubernetes-service"></a>Personalizar CoreDNS con Azure Kubernetes Service

Azure Kubernetes Service (AKS) se usa el [CoreDNS] [ coredns] proyecto para la administración de DNS del clúster y la resolución con todos los *1.12.x* y clústeres de más alto. Anteriormente, se utiliza el proyecto de kube-dns. Este proyecto de kube-dns está en desuso. Para obtener más información sobre la personalización de CoreDNS y Kubernetes, consulte el [documentación oficial de canal de subida][corednsk8s].

Como AKS es un servicio administrado, no se puede modificar la configuración principal para CoreDNS (un *archivo imagen de memoria*). En su lugar, use un Kubernetes *ConfigMap* para invalidar la configuración predeterminada. Para ver el valor predeterminado de AKS CoreDNS ConfigMaps, use el `kubectl get configmaps coredns -o yaml` comando.

Este artículo muestra cómo usar ConfigMaps para las opciones de personalización básica de CoreDNS en AKS.

> [!NOTE]
> `kube-dns` ofrece diferentes [opciones de personalización] [ kubednsblog] a través de un mapa de la configuración de Kubernetes. Es CoreDNS **no** compatible con versiones anteriores con kube-dns. Deben actualizarse las personalizaciones que usó anteriormente para su uso con CoreDNS.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, consulte la Guía de inicio rápido AKS [mediante la CLI de Azure] [aks-quickstart-cli] o [mediante Azure portal] [portal de inicio rápido de aks].

## <a name="change-the-dns-ttl"></a>Cambiar el TTL de DNS

Un escenario que desea configurar en CoreDNS consiste en aumentar o disminuir el tiempo de configuración de vida (TTL) para almacenar en caché del nombre DNS. En este ejemplo, vamos a modificar el valor TTL. De forma predeterminada, este valor es 30 segundos. Para obtener más información acerca de las opciones de caché DNS, consulte el [oficial de CoreDNS][dnscache].

En el ejemplo siguiente ConfigMap, tenga en cuenta la `name` valor. De forma predeterminada, CoreDNS no admite este tipo de personalización que modifique el archivo de imagen de memoria propia. Usa AKS el *coredns-custom* ConfigMap para incorporar sus propias configuraciones y se carga después el archivo de imagen de memoria principal.

El ejemplo siguiente se indica a CoreDNS que para todos los dominios (se muestra en el `.` en `.:53`), en el puerto 53 (el puerto predeterminado DNS), establezca el TTL de caché a 15 (`cache 15`). Cree un archivo denominado `coredns-custom.json` y pegue el siguiente ejemplo de configuración:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom # this is the name of the configmap you can overwrite with your changes
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    .:53 {
        cache 15  # this is our new cache value
    }
```

Cree la ConfigMap mediante el [kubectl aplicar configmap] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl apply configmap coredns-custom.json
```

Para comprobar las personalizaciones se han aplicado, use el [kubectl get configmaps] [ kubectl-get] y especifique su *coredns-custom* ConfigMap:

```
kubectl get configmaps coredns-custom -o yaml
```

Forzar ahora CoreDNS para volver a cargar el ConfigMap. El [kubectl eliminar pod] [ kubectl delete] comando no es destructiva y no provoca tiempo de inactividad. El `kube-dns` pods se eliminan y el programador de Kubernetes, a continuación, vuelve a crear. Estos nuevos pods contienen el cambio en el valor TTL.

```console
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="rewrite-dns"></a>Vuelva a escribir DNS

Un escenario que tiene consiste en realizar reescrituras de nombre DNS en la marcha. En el ejemplo siguiente, reemplace `<domain to be written>` con su propio nombre de dominio completo. Cree un archivo denominado `coredns-custom.json` y pegue el siguiente ejemplo de configuración:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    <domain to be rewritten>.com:53 {
        errors
        cache 30
        rewrite name substring <domain to be rewritten>.com default.svc.cluster.local
        proxy .  /etc/resolv.conf # you can redirect this to a specific DNS server such as 10.0.0.10
    }
```

Como se muestra en el ejemplo anterior, cree la ConfigMap utilizando el [kubectl aplicar configmap] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML. A continuación, forzar CoreDNS para volver a cargar la ConfigMap utilizando el [kubectl eliminar pod] [ kubectl delete] para el programador de Kubernetes volver a crearlas:

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="custom-proxy-server"></a>Servidor proxy personalizado

Si tiene que especificar un servidor proxy para el tráfico de red, puede crear ConfigMap para personalizar el DNS. En el ejemplo siguiente, actualice el `proxy` nombre y dirección con los valores para su propio entorno. Cree un archivo denominado `coredns-custom.json` y pegue el siguiente ejemplo de configuración:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    .:53 {
        proxy foo.com 1.1.1.1
    }
```

Como se muestra en los ejemplos anteriores, cree la ConfigMap utilizando el [kubectl aplicar configmap] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML. A continuación, forzar CoreDNS para volver a cargar la ConfigMap utilizando el [kubectl eliminar pod] [ kubectl delete] para el programador de Kubernetes volver a crearlas:

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="use-custom-domains"></a>Usar dominios personalizados

Es posible que desee configurar dominios personalizados que solo se pueden resolver internamente. Por ejemplo, puede resolver el dominio personalizado *puglife.local*, lo que no es un dominio de nivel superior válido. Sin un dominio personalizado ConfigMap, el clúster de AKS no puede resolver la dirección.

En el ejemplo siguiente, actualice la dirección IP y dominio personalizado para dirigir el tráfico a con los valores para su propio entorno. Cree un archivo denominado `coredns-custom.json` y pegue el siguiente ejemplo de configuración:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  puglife.server: |
    puglife.local:53 {
        errors
        cache 30
        proxy . 192.11.0.1  # this is my test/dev DNS server
    }
```

Como se muestra en los ejemplos anteriores, cree la ConfigMap utilizando el [kubectl aplicar configmap] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML. A continuación, forzar CoreDNS para volver a cargar la ConfigMap utilizando el [kubectl eliminar pod] [ kubectl delete] para el programador de Kubernetes volver a crearlas:

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="stub-domains"></a>Dominios de código auxiliar

CoreDNS también puede utilizarse para configurar dominios de código auxiliar. En el ejemplo siguiente, actualizar los dominios personalizados y las direcciones IP con los valores para su propio entorno. Cree un archivo denominado `coredns-custom.json` y pegue el siguiente ejemplo de configuración:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
    abc.com:53 {
        errors
        cache 30
        proxy . 1.2.3.4
    }
    my.cluster.local:53 {
        errors
        cache 30
        proxy . 2.3.4.5
    }
```

Como se muestra en los ejemplos anteriores, cree la ConfigMap utilizando el [kubectl aplicar configmap] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML. A continuación, forzar CoreDNS para volver a cargar la ConfigMap utilizando el [kubectl eliminar pod] [ kubectl delete] para el programador de Kubernetes volver a crearlas:

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se mostraron algunos escenarios de ejemplo para la personalización de CoreDNS. Para obtener información sobre el proyecto CoreDNS, consulte [la página de proyecto ascendente CoreDNS][coredns].

Para más información acerca de conceptos de red principales, consulte [red conceptos relacionados con aplicaciones en AKS][concepts-network].

<!-- LINKS - external -->
[kubednsblog]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[coredns]: https://coredns.io/
[corednsk8s]: https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns
[dnscache]: https://coredns.io/plugins/cache/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete

<!-- LINKS - external -->
[concepts-network]: concepts-network.md