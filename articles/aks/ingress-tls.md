---
title: Creación de una entrada HTTPS con un clúster de Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo instalar y configurar un controlador de entrada NGINX que usa Let’s Encrypt para la generación automática de certificados SSL en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 4679b800126f75596dcb78b46c65c6ac2b616729
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364632"
---
# <a name="create-an-https-ingress-controller-on-azure-kubernetes-service-aks"></a>Creación de un controlador de entrada HTTPS en Azure Kubernetes Service (AKS)

Un controlador de entrada es un software que proporciona el proxy inverso, el enrutamiento del tráfico configurable y la terminación de TLS para los servicios de Kubernetes. Los recursos de entrada de Kubernetes se usan para configurar las reglas de entrada y las rutas de los distintos servicios de Kubernetes. Mediante reglas de entrada y un controlador de entrada, se puede usar una sola dirección IP para enrutar el tráfico a varios servicios en un clúster de Kubernetes.

En este artículo se muestra cómo implementar el [controlador de entrada NGINX][nginx-ingress] en un clúster de Azure Kubernetes Service (AKS). El proyecto [cert-manager][cert-manager] se usa para generar y configurar automáticamente certificados [Let's Encrypt][lets-encrypt]. Por último, en el clúster de AKS se ejecutan dos aplicaciones, a las que se puede acceder con una sola dirección IP.

También puede:

- [Crear un controlador de entrada básico con conectividad de red externa][aks-ingress-basic]
- [Habilitar el complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Crear un controlador de entrada que usa una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada con una dirección IP pública estática y configurar Let's Encrypt para generar certificados TLS de forma automática][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se usa Helm para instalar el controlador de entrada NGINX, cert-manager y una aplicación web de ejemplo. Debe tener Helm inicializado dentro del clúster de AKS y usar una cuenta de servicio para Tiller. Asegúrese de que está usando la versión más reciente de Helm. Para obtener instrucciones de actualización, vea la [documentación de instalación de Helm][helm-install]. Para más información sobre cómo configurar y usar Helm, consulte el artículo sobre la [instalación de aplicaciones con Helm en Azure Kubernetes Service (AKS)][use-helm].

En este artículo también se requiere que ejecute la versión 2.0.41 de la CLI de Azure o una versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Crear un controlador de entrada

Para crear el controlador de entrada, use `Helm` para instalar *nginx-ingress*. Para obtener redundancia adicional, se implementan dos réplicas de los controladores de entrada NGINX con el parámetro `--set controller.replicaCount`. Para sacar el máximo provecho de las réplicas en ejecución del controlador de entrada, asegúrese de que hay más de un nodo en el clúster de AKS.

> [!TIP]
> En el ejemplo siguiente se instala el controlador de entrada en el espacio de nombres `kube-system`. Si quiere, puede especificar otro espacio de nombres para su propio entorno. Si su clúster de AKS no tiene RBAC habilitado, agregue `--set rbac.create=false` a los comandos.

```console
helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2
```

Durante la instalación se crea una dirección IP pública de Azure para el controlador de entrada. Esta dirección IP pública es estática durante el período de vida del controlador de entrada. Si elimina el controlador de entrada, se pierde la asignación de dirección IP pública. Si después crea un controlador de entrada adicional, se asigna una dirección IP pública nueva. Si quiere conservar el uso de la dirección IP pública, en su lugar puede [crear un controlador de entrada con una dirección IP pública estática][aks-ingress-static-tls].

Para obtener la dirección IP pública, use el comando `kubectl get service`. La asignación de la dirección IP al servicio puede tardar hasta un minuto.

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Todavía no se creó ninguna regla de entrada. Si navega a la dirección IP pública, se muestra la página 404 predeterminada del controlador de entrada NGINX.

## <a name="configure-a-dns-name"></a>Configuración de un nombre DNS

Para que los certificados HTTPS funcionen correctamente, configure un FQDN para la dirección IP del controlador de entrada. Actualice el siguiente script con la dirección IP del controlador de entrada y un nombre único que quisiera usar para el FQDN:

```console
#!/bin/bash

# Public IP address of your ingress controller
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with DNS name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

El controlador de entrada ahora es accesible mediante el nombre de dominio completo.

## <a name="install-cert-manager"></a>Instalar cert-manager

El controlador de entrada NGINX es compatible con la terminación de TLS. Hay varias maneras de recuperar y configurar certificados para HTTPS. En este artículo se muestra cómo utilizar [cert manager][cert-manager], que proporciona la generación automática de certificados de [Lets Encrypt] [ lets-encrypt] y la funcionalidad de administración.

> [!NOTE]
> En este artículo se usa el entorno `staging` para Let's Encrypt. En las implementaciones de producción, use `letsencrypt-prod` y `https://acme-v02.api.letsencrypt.org/directory` en las definiciones de recursos y al instalar el gráfico de Helm.

Para instalar el controlador de cert-manager en un clúster habilitado para RBAC, use el comando `helm install` siguiente:

```console
helm install stable/cert-manager \
    --namespace kube-system \
    --set ingressShim.defaultIssuerName=letsencrypt-staging \
    --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Si el clúster no está habilitado para RBAC, use el comando siguiente:

```console
helm install stable/cert-manager \
    --namespace kube-system \
    --set ingressShim.defaultIssuerName=letsencrypt-staging \
    --set ingressShim.defaultIssuerKind=ClusterIssuer \
    --set rbac.create=false \
    --set serviceAccount.create=false
```

Para obtener más información sobre cert-manager, consulte el [proyecto de cert manager][cert-manager].

## <a name="create-a-ca-cluster-issuer"></a>Creación de un emisor de clúster de entidad de certificación

Para poder emitir certificados, cert-manager requiere un recurso [Issuer] [ cert-manager-issuer] o [ClusterIssuer] [ cert-manager-cluster-issuer]. La funcionalidad de los recursos de Kubernetes es idéntica, pero `Issuer` funciona en un espacio de nombres único, mientras que `ClusterIssuer` funciona en todos los espacios de nombres. Para más información, consulte la documentación de [cert-manager issuer][cert-manager-issuer].

Cree un emisor de clúster, como `cluster-issuer.yaml`, con el manifiesto de ejemplo siguiente. Actualice la dirección de correo electrónico con una dirección válida de su organización:

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    http01: {}
```

Para crear el emisor, use el comando `kubectl apply -f cluster-issuer.yaml`.

```
$ kubectl apply -f cluster-issuer.yaml

clusterissuer.certmanager.k8s.io/letsencrypt-staging created
```

## <a name="create-a-certificate-object"></a>Creación de un objeto de certificado

A continuación, se debe crear un recurso de certificado. El recurso de certificado define el certificado X.509 deseado. Para más información, consulte los [certificados de cert-manager certificates][cert-manager-certificates].

Cree el recurso de certificado, como `certificates.yaml`, con el manifiesto de ejemplo siguiente. Actualice *dnsNames* y los *dominios* al nombre DNS que creó en un paso anterior. Si usa un controlador de entrada solo para uso interno, especifique el nombre DNS interno para el servicio.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Para crear el recurso de certificado, use el comando `kubectl apply -f certificates.yaml`.

```
$ kubectl apply -f certificates.yaml

certificate.certmanager.k8s.io/tls-secret created
```

## <a name="run-demo-applications"></a>Ejecución de aplicaciones de demostración

Se deben haber configurado una solución de administración de certificados y un controlador de entrada. Ahora vamos a ejecutar dos aplicaciones de demostración en el clúster de AKS. En este ejemplo, Helm se usa para implementar dos instancias de una aplicación "Hola mundo" sencilla.

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

Ambas aplicaciones se ejecutan ahora en el clúster de Kubernetes, sin embargo, están configuradas con un servicio de tipo `ClusterIP`. Por lo tanto, no se puede acceder a ellas desde Internet. Para que estén disponibles de manera pública, cree un recurso de entrada de Kubernetes. El recurso de entrada configura las reglas de enrutamiento del tráfico a una de las dos aplicaciones.

En el ejemplo siguiente, el tráfico a la dirección `https://demo-aks-ingress.eastus.cloudapp.azure.com/` se enruta al servicio denominado `aks-helloworld`. El tráfico a la dirección `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` se enruta al servicio `ingress-demo`. Actualice los *hosts* y el *host* al nombre DNS que creó en un paso anterior.

Cree un archivo denominado `hello-world-ingress.yaml` y cópielo en el ejemplo siguiente de YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
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

## <a name="test-the-ingress-configuration"></a>Prueba de la configuración de entrada

Abra un explorador web en el FQDN del controlador de entrada de Kubernetes, como *https://demo-aks-ingress.eastus.cloudapp.azure.com*.

Como estos ejemplos usan `letsencrypt-staging`, el explorador no confía en el certificado SSL emitido. Acepte el mensaje de advertencia para continuar a la aplicación. La información de certificado muestra que Let’s Encrypt emite este certificado *Fake LE Intermediate X1*. Este certificado falso indica que `cert-manager` procesó correctamente la solicitud y recibió un certificado del proveedor:

![Certificado de almacenamiento provisional de Let's Encrypt](media/ingress/staging-certificate.png)

Cuando modifica Let's Encrypt para usar `prod` en lugar de `staging`, se usa un certificado de confianza que Let's Encrypt emite, tal como se muestra en el ejemplo siguiente:

![Certificado de Let's Encrypt](media/ingress/certificate.png)

La aplicación de demostración se muestra en el explorador web:

![Primer ejemplo de aplicación](media/ingress/app-one.png)

A continuación, agregue la ruta de acceso */hello-world-two* al FQDN, como *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two*. Se muestra la segunda aplicación de demostración con el título personalizado:

![Segundo ejemplo de aplicación](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, se usa Helm para instalar los componentes de entrada, los certificados y las aplicaciones de ejemplo. Al implementar un gráfico de Helm, se crean algunos recursos de Kubernetes. Estos recursos incluyen pods, implementaciones y servicios. Para limpiarlos, quite primero los recursos de certificado:

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Ahora, despliegue una lista de las versiones de Helm con el comando `helm list`. Busque los gráficos denominados *nginx-ingress*, *cert-manager*, y *aks-helloworld*, tal y como se muestra en la salida del ejemplo siguiente:

```
$ helm list

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
billowing-kitten        1           Tue Oct 16 17:24:05 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
loitering-waterbuffalo  1           Tue Oct 16 17:26:16 2018    DEPLOYED    cert-manager-v0.3.4     v0.3.2      kube-system
flabby-deer             1           Tue Oct 16 17:27:06 2018    DEPLOYED    aks-helloworld-0.1.0                default
linting-echidna         1           Tue Oct 16 17:27:02 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Elimine las versiones con el comando `helm delete`. En el ejemplo siguiente se elimina la implementación de entrada NGINX, así como el administrador de certificados y las dos aplicaciones hola mundo de AKS de ejemplo.

```
$ helm delete billowing-kitten loitering-waterbuffalo flabby-deer linting-echidna

release "billowing-kitten" deleted
release "loitering-waterbuffalo" deleted
release "flabby-deer" deleted
release "linting-echidna" deleted
```

A continuación, elimine el repositorio de Helm para la aplicación hola mundo de AKS:

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
- [cert-manager][cert-manager]

También puede:

- [Crear un controlador de entrada básico con conectividad de red externa][aks-ingress-basic]
- [Habilitar el complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Crear un controlador de entrada que usa una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada con una dirección IP pública estática y configurar Let's Encrypt para generar certificados TLS de forma automática][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
