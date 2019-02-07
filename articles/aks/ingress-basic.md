---
title: Creación de un controlador de entrada básico en Azure Kubernetes Service (AKS)
description: Aprenda a instalar y configurar un controlador de entrada NGINX básico en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 99f97f7d796fecf1ac77cb3752a9ba7019edfbbc
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55729872"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Creación de un controlador de entrada en Azure Kubernetes Service (AKS)

Un controlador de entrada es un software que proporciona el proxy inverso, el enrutamiento del tráfico configurable y la terminación de TLS para los servicios de Kubernetes. Los recursos de entrada de Kubernetes se usan para configurar las reglas de entrada y las rutas de los distintos servicios de Kubernetes. Mediante reglas de entrada y un controlador de entrada, se puede usar una sola dirección IP para enrutar el tráfico a varios servicios en un clúster de Kubernetes.

En este artículo se muestra cómo implementar el [controlador de entrada NGINX][nginx-ingress] en un clúster de Azure Kubernetes Service (AKS). Se ejecutan dos aplicaciones en el clúster de AKS, a las que se puede acceder con una sola dirección IP.

También puede:

- [Habilitar el complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Crear un controlador de entrada que usa una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada que usa sus propios certificados TLS][aks-ingress-own-tls]
- Crear un controlador de entrada que use Let's Encrypt para generar certificados TLS de forma automática [con una dirección IP pública dinámica][aks-ingress-tls] o [con una dirección IP pública estática][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se usa Helm para instalar el controlador de entrada NGINX, cert-manager y una aplicación web de ejemplo. Debe tener Helm inicializado dentro del clúster de AKS y usar una cuenta de servicio para Tiller. Para más información sobre cómo configurar y usar Helm, consulte el artículo sobre la [instalación de aplicaciones con Helm en Azure Kubernetes Service (AKS)][use-helm].

En este artículo también se requiere que ejecute la versión 2.0.41 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Crear un controlador de entrada

Para crear el controlador de entrada, use `Helm` para instalar *nginx-ingress*. Para obtener redundancia adicional, se implementan dos réplicas de los controladores de entrada NGINX con el parámetro `--set controller.replicaCount`. Para sacar el máximo provecho de las réplicas en ejecución del controlador de entrada, asegúrese de que hay más de un nodo en el clúster de AKS.

> [!TIP]
> En el ejemplo siguiente se instala el controlador de entrada en el espacio de nombres `kube-system`. Si quiere, puede especificar otro espacio de nombres para su propio entorno. Si su clúster de AKS no tiene RBAC habilitado, agregue `--set rbac.create=false` a los comandos.

```console
helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2
```

Cuando se crea el servicio del equilibrador de carga de Kubernetes para el controlador de entrada NGINX, se asigna la dirección IP pública dinámica, como se muestra en la salida del ejemplo siguiente:

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                         TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
masked-otter-nginx-ingress-controller        LoadBalancer   10.0.92.99    40.117.74.8   80:31077/TCP,443:32592/TCP   7m
masked-otter-nginx-ingress-default-backend   ClusterIP      10.0.46.106   <none>        80/TCP                       7m
```

No se han creado reglas de entrada aún, por lo que aparece la página 404 predeterminada del controlador de entrada NGINX si navega a la dirección IP interna. Las reglas de entrada se configuran en los pasos siguientes.

## <a name="run-demo-applications"></a>Ejecución de aplicaciones de demostración

Para ver el controlador de entrada en acción, vamos a ejecutar dos aplicaciones de demostración en el clúster de AKS. En este ejemplo, Helm se usa para implementar dos instancias de una aplicación "Hola mundo" sencilla.

Antes de instalar los gráficos de Helm de ejemplo, agregue el repositorio de ejemplos de Azure al entorno de Helm como se indica a continuación:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Cree la primera aplicación de demostración desde un gráfico de Helm con el comando siguiente:

```console
helm install azure-samples/aks-helloworld
```

A continuación, instale una segunda instancia de la aplicación de demostración. Para esta segunda instancia, se especifica un nuevo título de manera que las dos aplicaciones se distingan visualmente. También se especifica un nombre de servicio único:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Creación de una ruta de entrada

Ambas aplicaciones ahora se ejecutan en el clúster de Kubernetes. Para enrutar el tráfico a cada aplicación, cree un recurso de entrada de Kubernetes. El recurso de entrada configura las reglas de enrutamiento del tráfico a una de las dos aplicaciones.

En el ejemplo siguiente, el tráfico a la dirección `http://40.117.74.8/` se enruta al servicio denominado `aks-helloworld`. El tráfico a la dirección `http://40.117.74.8/hello-world-two` se enruta al servicio `ingress-demo`.

Cree un archivo denominado `hello-world-ingress.yaml` y cópielo en el ejemplo siguiente de YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Cree el recurso de entrada con el comando `kubectl apply -f hello-world-ingress.yaml`.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Prueba del controlador de entrada

Para probar las rutas para el controlador de entrada, vaya a las dos aplicaciones. Abra un explorador web en la dirección IP de su controlador de entrada NGINX, como *http://40.117.74.8*. La primera aplicación de demostración aparece en el explorador web, como se muestra en el ejemplo siguiente:

![Primera aplicación ejecutándose detrás del controlador de entrada](media/ingress-basic/app-one.png)

A continuación, agregue la ruta de acceso */hello-world-two* a la dirección IP, como *http://40.117.74.8/hello-world-two*. Se muestra la segunda aplicación de demostración con el título personalizado:

![Segunda aplicación ejecutándose detrás del controlador de entrada](media/ingress-basic/app-two.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, se usa Helm para instalar los componentes de entrada y las aplicaciones de ejemplo. Al implementar un gráfico de Helm, se crean algunos recursos de Kubernetes. Estos recursos incluyen pods, implementaciones y servicios. Para borrar dichos recursos, primero muestre las versiones de Helm mediante el comando `helm list`. Busque los gráficos denominados *nginx-ingress* y *aks-helloworld*, tal y como se muestra en la salida del ejemplo siguiente:

```
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
gilded-duck         1           Tue Oct 16 16:52:25 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
righteous-numbat    1           Tue Oct 16 16:53:53 2018    DEPLOYED    aks-helloworld-0.1.0                default
looming-moth        1           Tue Oct 16 16:53:59 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Elimine las versiones con el comando `helm delete`. En el ejemplo siguiente se elimina la implementación de entrada NGINX y las dos aplicaciones Hola mundo de AKS de ejemplo.

```
$ helm delete gilded-duck righteous-numbat looming-moth

release "gilded-duck" deleted
release "righteous-numbat" deleted
release "looming-moth" deleted
```

A continuación, elimine el repositorio de Helm para la aplicación Hola mundo de AKS:

```console
helm repo remove azure-samples
```

Finalmente, elimine la ruta de entrada que dirige el tráfico a las aplicaciones de ejemplo:

```console
kubectl delete -f hello-world-ingress.yaml
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se incluyen algunos componentes externos a AKS. Para más información sobre estos componentes, consulte las siguientes páginas del proyecto:

- [CLI de Helm][helm-cli]
- [Controlador de entrada NGINX][nginx-ingress]

También puede:

- [Habilitar el complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Crear un controlador de entrada que usa una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada que usa sus propios certificados TLS][aks-ingress-own-tls]
- Crear un controlador de entrada que use Let's Encrypt para generar certificados TLS de forma automática [con una dirección IP pública dinámica][aks-ingress-tls] o [con una dirección IP pública estática][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
