---
title: Personalización de CoreDNS para Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo personalizar CoreDNS para agregar subdominios o ampliar puntos de conexión DNS personalizados con Azure Kubernetes Service (AKS).
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 03/15/2019
ms.author: jenoller
ms.openlocfilehash: 247665f58dd064565f0e9aebc9859e97ce0ab0c0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836973"
---
# <a name="customize-coredns-with-azure-kubernetes-service"></a>Personalización de CoreDNS con Azure Kubernetes Service

Azure Kubernetes Service (AKS) usa el proyecto [CoreDNS][coredns] para la administración y la resolución DNS del clúster con todos los clústeres *1.12.x* y versiones superiores. Anteriormente, usaba el proyecto kube-dns. Actualmente, este proyecto kube-dns está en desuso. Para obtener más información sobre la personalización de CoreDNS y Kubernetes, consulte la [documentación oficial del canal de subida][corednsk8s].

Dado que AKS es un servicio administrado, no puede modificar la configuración principal para CoreDNS (un archivo *CoreFile*). En vez de eso, use un archivo *ConfigMap* de Kubernetes para invalidar la configuración predeterminada. Para ver los archivos ConfigMap de CoreDNS predeterminados de AKS, use el comando `kubectl get configmaps coredns -o yaml`.

En este artículo se muestra cómo usar los archivos ConfigMap para las opciones de personalización básica de CoreDNS en AKS.

> [!NOTE]
> `kube-dns` ofrecía [opciones de personalización][kubednsblog] diferentes a través de una asignación de configuración de Kubernetes. CoreDNS **no** es compatible con versiones anteriores de kube-dns. Todas las personalizaciones que usó anteriormente deben actualizarse para su uso con CoreDNS.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

## <a name="what-is-supportedunsupported"></a>Elementos admitidos y no admitidos

Se admiten todos los complementos de CoreDNS integrados. No se admiten los complementos de terceros o complementarios. 

## <a name="rewrite-dns"></a>Reescritura de DNS

Un escenario que debe considerar es la reescritura de nombres DNS sobre la marcha. En el ejemplo siguiente, reemplace `<domain to be written>` por su propio nombre de dominio completo. Cree un archivo denominado `corednsms.yaml` y pegue la siguiente configuración de ejemplo:

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

Cree el archivo ConfigMap mediante el comando [kubectl apply configmap][kubectl-apply] y especifique el nombre de su manifiesto de YAML:

```console
kubectl apply -f corednsms.yaml
```

Para comprobar que las personalizaciones se han aplicado, use el comando [kubectl get configmaps][kubectl-get] y especifique su archivo ConfigMap *coredns-custom*:

```
kubectl get configmaps --namespace=kube-system coredns-custom -o yaml
```

Ahora, fuerce a que CoreDNS vuelva a cargar el archivo ConfigMap. El comando [kubectl delete pod][kubectl delete] no es destructivo y no provoca tiempo de inactividad. Se eliminan los pods `kube-dns` y el Programador de Kubernetes vuelve a crearlos. Estos nuevos pods contienen el cambio en el valor TTL.

```console
kubectl delete pod --namespace kube-system -l k8s-app=kube-dns
```

> [!Note]
> El comando anterior es correcto. Mientras cambiamos `coredns`, la implementación tiene el nombre **kube-dns**.

## <a name="custom-proxy-server"></a>Servidor proxy personalizado

Si tiene que especificar un servidor proxy para el tráfico de red, puede crear un archivo ConfigMap para personalizar el DNS. En el ejemplo siguiente, actualice el nombre y la dirección del `proxy` con los valores de su propio entorno. Cree un archivo denominado `corednsms.yaml` y pegue la siguiente configuración de ejemplo:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    <domain to be rewritten>.com:53 {
        proxy foo.com 1.1.1.1
    }
```

Como en los ejemplos anteriores, cree el archivo ConfigMap mediante el comando [kubectl apply configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] para que Kubernetes Scheduler lo recree: Uso de dominios personalizados

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="use-custom-domains"></a>Es posible que le interese configurar dominios personalizados que solo se puedan resolver internamente.

Por ejemplo, tal vez quiera resolver el dominio personalizado *puglife.local*, que no es un dominio de nivel superior válido. Sin un archivo ConfigMap de dominio personalizado, el clúster de AKS no puede resolver la dirección. En el ejemplo siguiente, actualice con los valores de su propio entorno el dominio personalizado y la dirección IP a la que se va a dirigir el tráfico.

Cree un archivo denominado `corednsms.yaml` y pegue la siguiente configuración de ejemplo: Como en los ejemplos anteriores, cree el archivo ConfigMap mediante el comando [kubectl apply configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] para que Kubernetes Scheduler lo recree:

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

Dominios de código auxiliar También se puede usar CoreDNS para configurar dominios de código auxiliar.

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="stub-domains"></a>En el ejemplo siguiente, actualice con los valores de su propio entorno los dominios personalizados y las direcciones IP.

Cree un archivo denominado `corednsms.yaml` y pegue la siguiente configuración de ejemplo: Como en los ejemplos anteriores, cree el archivo ConfigMap mediante el comando [kubectl apply configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] para que Kubernetes Scheduler lo recree: Complemento de hosts

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
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

Dado que se admiten todos los complementos integrados, también se puede personalizar el complemento de [Hosts][coredns hosts] de CoreDNS: Pasos siguientes

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="hosts-plugin"></a>En este artículo se han mostrado algunos escenarios de ejemplo para la personalización de CoreDNS.

Para obtener información sobre el proyecto CoreDNS, consulte la [página de proyecto del canal de subida de CoreDNS][coredns].

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom # this is the name of the configmap you can overwrite with your changes
  namespace: kube-system
data:
    test.override: |
          hosts example.hosts example.org { # example.hosts must be a file
              10.0.0.1 example.org
              fallthrough
          }
```

## <a name="next-steps"></a>Para obtener más información sobre conceptos básicos de red, consulte [Conceptos de redes de aplicaciones en Azure Kubernetes Service (AKS)][concepts-network].

This article showed some example scenarios for CoreDNS customization. For information on the CoreDNS project, see <bpt id="p1">[</bpt>the CoreDNS upstream project page<ept id="p1">][coredns]</ept>.

To learn more about core network concepts, see <bpt id="p1">[</bpt>Network concepts for applications in AKS<ept id="p1">][concepts-network]</ept>.

<!-- LINKS - external -->
[kubednsblog]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[coredns]: https://coredns.io/
[corednsk8s]: https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns
[dnscache]: https://coredns.io/plugins/cache/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[coredns hosts]: https://coredns.io/plugins/hosts/

<!-- LINKS - internal -->
[concepts-network]: concepts-network.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
