---
title: Creación de una entrada HTTPS con un clúster de Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo instalar y configurar un controlador de entrada NGINX que usa Let’s Encrypt para la generación automática de certificados TLS en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: mlearned
ms.openlocfilehash: 30f25ad9152bc722b54a834ef0ed037ac1666014
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615285"
---
# <a name="create-an-https-ingress-controller-on-azure-kubernetes-service-aks"></a>Creación de un controlador de entrada HTTPS en Azure Kubernetes Service (AKS)

Un controlador de entrada es un software que proporciona el proxy inverso, el enrutamiento del tráfico configurable y la terminación de TLS para los servicios de Kubernetes. Los recursos de entrada de Kubernetes se usan para configurar las reglas de entrada y las rutas de los distintos servicios de Kubernetes. Mediante reglas de entrada y un controlador de entrada, se puede usar una sola dirección IP para enrutar el tráfico a varios servicios en un clúster de Kubernetes.

En este artículo se muestra cómo implementar el [controlador de entrada NGINX][nginx-ingress] in an Azure Kubernetes Service (AKS) cluster. The [cert-manager][cert-manager]. El proyecto se usa para generar y configurar automáticamente certificados de [Let's Encrypt][lets-encrypt]. Por último, en el clúster de AKS se ejecutan dos aplicaciones, a las que se puede acceder con una sola dirección IP. También puede:

[Creación de un controlador de entrada básico con conectividad de red externa][aks-ingress-basic]

- [Habilitación del complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Creación de un controlador de entrada que use una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada que usa sus propios certificados TLS][aks-ingress-own-tls]
- [Crear un controlador de entrada que usa Let's Encrypt para generar automáticamente certificados TLS con una dirección IP pública estática][aks-ingress-static-tls]
- Antes de empezar

## <a name="before-you-begin"></a>En este artículo se supone que ya tiene un clúster de AKS.

Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal]. En este artículo se usa Helm para instalar el controlador de entrada NGINX, cert-manager y una aplicación web de ejemplo.

Debe tener Helm inicializado dentro del clúster de AKS y usar una cuenta de servicio para Tiller. Asegúrese de que está usando la versión más reciente de Helm. Para obtener instrucciones de actualización, vea la [documentación de instalación de Helm][helm-install]. For more information on configuring and using Helm, see [Install applications with Helm in Azure Kubernetes Service (AKS)][use-helm]. En este artículo también se requiere que ejecute la versión 2.0.64 de la CLI de Azure o una versión posterior.

Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install]. Crear un controlador de entrada

## <a name="create-an-ingress-controller"></a>Para crear el controlador de entrada, use `Helm` para instalar *nginx-ingress*.

Para obtener redundancia adicional, se implementan dos réplicas de los controladores de entrada NGINX con el parámetro `--set controller.replicaCount`. Para sacar el máximo provecho de las réplicas en ejecución del controlador de entrada, asegúrese de que hay más de un nodo en el clúster de AKS. El controlador de entrada también debe programarse en un nodo de Linux.

Los nodos de Windows Server (actualmente en versión preliminar en AKS) no deben ejecutar el controlador de entrada. Un selector de nodos se especifica mediante el parámetro `--set nodeSelector` para indicar al programador de Kubernetes que ejecute el controlador de entrada NGINX en un nodo basado en Linux. En el siguiente ejemplo se crea un espacio de nombres de Kubernetes para los recursos de entrada denominado *ingress-basic*.

> [!TIP]
> Especifique un espacio de nombres para su propio entorno según sea necesario. Si su clúster de AKS no tiene RBAC habilitado, agregue `--set rbac.create=false` a los comandos de Helm. Si quiere habilitar la [conservación de direcciones IP de origen del cliente][client-source-ip] para las solicitudes a los contenedores de su clúster, agregue `--set controller.service.externalTrafficPolicy=Local` al comando de instalación de Helm.

> [!TIP]
> La dirección IP de origen del cliente se almacena en el encabezado de la solicitud en *X-Forwarded-For*. Al usar un controlador de entrada con la conservación de direcciones IP de origen del cliente habilitada, el paso a través de SSL no funcionará. Durante la instalación se crea una dirección IP pública de Azure para el controlador de entrada.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Esta dirección IP pública es estática durante el período de vida del controlador de entrada. Si elimina el controlador de entrada, se pierde la asignación de dirección IP pública. Si después crea un controlador de entrada adicional, se asigna una dirección IP pública nueva. Si quiere conservar el uso de la dirección IP pública, en su lugar puede [crear un controlador de entrada con una dirección IP pública estática][aks-ingress-static-tls]. Para obtener la dirección IP pública, use el comando `kubectl get service`.

La asignación de la dirección IP al servicio puede tardar hasta un minuto. Todavía no se creó ninguna regla de entrada.

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
billowing-kitten-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
billowing-kitten-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Si navega a la dirección IP pública, se muestra la página 404 predeterminada del controlador de entrada NGINX. Configuración de un nombre DNS

## <a name="configure-a-dns-name"></a>Para que los certificados HTTPS funcionen correctamente, configure un FQDN para la dirección IP del controlador de entrada.

Actualice el siguiente script con la dirección IP del controlador de entrada y un nombre único que quisiera usar para el FQDN: El controlador de entrada ahora es accesible mediante el nombre de dominio completo.

```azurecli-interactive
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

Instalar cert-manager

## <a name="install-cert-manager"></a>El controlador de entrada NGINX es compatible con la terminación de TLS.

Hay varias maneras de recuperar y configurar certificados para HTTPS. En este artículo se muestra cómo usar [cert manager][cert-manager], which provides automatic [Lets Encrypt][lets-encrypt], que proporciona la generación de certificados y la funcionalidad de administración. En este artículo se usa el entorno `staging` para Let's Encrypt.

> [!NOTE]
> En las implementaciones de producción, use `letsencrypt-prod` y `https://acme-v02.api.letsencrypt.org/directory` en las definiciones de recursos y al instalar el gráfico de Helm. Para instalar el controlador de cert-manager en un clúster habilitado para RBAC, use el comando `helm install` siguiente:

Para obtener más información sobre la configuración cert-manager, consulte el [proyecto cert-manager][cert-manager].

```console
# Install the CustomResourceDefinition resources separately
kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.8/deploy/manifests/00-crds.yaml

# Create the namespace for cert-manager
kubectl create namespace cert-manager

# Label the cert-manager namespace to disable resource validation
kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  --name cert-manager \
  --namespace cert-manager \
  --version v0.8.0 \
  jetstack/cert-manager
```

Creación de un emisor de clúster de entidad de certificación

## <a name="create-a-ca-cluster-issuer"></a>Para poder emitir certificados, cert-manager requiere un recurso [Issuer][cert-manager-issuer] or [ClusterIssuer][cert-manager-cluster-issuer].

La funcionalidad de los recursos de Kubernetes es idéntica, pero `Issuer` funciona en un espacio de nombres único, mientras que `ClusterIssuer` funciona en todos los espacios de nombres. Para obtener más información, consulte la documentación de [cert-manager issuer][cert-manager-issuer]. Cree un emisor de clúster, como `cluster-issuer.yaml`, con el manifiesto de ejemplo siguiente.

Actualice la dirección de correo electrónico con una dirección válida de su organización: Para crear el emisor, use el comando `kubectl apply -f cluster-issuer.yaml`.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
  namespace: ingress-basic
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    http01: {}
```

Ejecución de aplicaciones de demostración

```
$ kubectl apply -f cluster-issuer.yaml

clusterissuer.certmanager.k8s.io/letsencrypt-staging created
```

## <a name="run-demo-applications"></a>Se deben haber configurado una solución de administración de certificados y un controlador de entrada.

Ahora vamos a ejecutar dos aplicaciones de demostración en el clúster de AKS. En este ejemplo, Helm se usa para implementar dos instancias de una aplicación "Hola mundo" sencilla. Antes de instalar los gráficos de Helm de ejemplo, agregue el repositorio de ejemplos de Azure al entorno de Helm como se indica a continuación:

Cree la primera aplicación de demostración desde un gráfico de Helm con el comando siguiente:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

A continuación, instale una segunda instancia de la aplicación de demostración.

```console
helm install azure-samples/aks-helloworld --namespace ingress-basic
```

Para esta segunda instancia, se especifica un nuevo título de manera que las dos aplicaciones se distingan visualmente. También se especifica un nombre de servicio único: Creación de una ruta de entrada

```console
helm install azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Ambas aplicaciones se ejecutan ahora en el clúster de Kubernetes, sin embargo, están configuradas con un servicio de tipo `ClusterIP`.

Por lo tanto, no se puede acceder a ellas desde Internet. Para que estén disponibles de manera pública, cree un recurso de entrada de Kubernetes. El recurso de entrada configura las reglas de enrutamiento del tráfico a una de las dos aplicaciones. En el ejemplo siguiente, el tráfico a la dirección `https://demo-aks-ingress.eastus.cloudapp.azure.com/` se enruta al servicio denominado `aks-helloworld`.

El tráfico a la dirección `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` se enruta al servicio `ingress-demo`. Actualice los *hosts* y el *host* al nombre DNS que creó en un paso anterior. Cree un archivo denominado `hello-world-ingress.yaml` y cópielo en el ejemplo siguiente de YAML.

Cree el recurso de entrada con el comando `kubectl apply -f hello-world-ingress.yaml`.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

Creación de un objeto de certificado

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="create-a-certificate-object"></a>A continuación, se debe crear un recurso de certificado.

El recurso de certificado define el certificado X.509 deseado. Para obtener más información, consulte los [certificados de cert-manager][cert-manager-certificates]. Es probable que el Administrador de certificados haya creado automáticamente un objeto de certificado mediante la corrección de compatibilidad (shim) de entrada, que se implementa automáticamente con el Administrador de certificados desde v0.2.2.

Para obtener más información, consulte la [documentación sobre corrección de compatibilidad (shim) de entrada][ingress-shim]. Para comprobar que el certificado se creó correctamente, use el comando `kubectl describe certificate tls-secret --namespace ingress-basic`.

Si se ha emitido el certificado, verá un resultado similar al siguiente:

Si necesita crear un recurso de certificado adicional, puede hacerlo con el siguiente ejemplo de manifiesto.
```
Type    Reason          Age   From          Message
----    ------          ----  ----          -------
  Normal  CreateOrder     11m   cert-manager  Created new ACME order, attempting validation...
  Normal  DomainVerified  10m   cert-manager  Domain "demo-aks-ingress.eastus.cloudapp.azure.com" verified with "http-01" validation
  Normal  IssueCert       10m   cert-manager  Issuing certificate...
  Normal  CertObtained    10m   cert-manager  Obtained certificate from ACME server
  Normal  CertIssued      10m   cert-manager  Certificate issued successfully
```

Actualice *dnsNames* y los *dominios* al nombre DNS que creó en un paso anterior. Si usa un controlador de entrada solo para uso interno, especifique el nombre DNS interno para el servicio. Para crear el recurso de certificado, use el comando `kubectl apply -f certificates.yaml`.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
  namespace: ingress-basic
spec:
  secretName: tls-secret-staging
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

Prueba de la configuración de entrada

```
$ kubectl apply -f certificates.yaml

certificate.certmanager.k8s.io/tls-secret-staging created
```

## <a name="test-the-ingress-configuration"></a>Abra un explorador web en el FQDN del controlador de entrada de Kubernetes, como *https://demo-aks-ingress.eastus.cloudapp.azure.com* .

Como estos ejemplos usan `letsencrypt-staging`, el explorador no confía en el certificado SSL emitido.

Acepte el mensaje de advertencia para continuar a la aplicación. La información de certificado muestra que Let’s Encrypt emite este certificado *Fake LE Intermediate X1*. Este certificado falso indica que `cert-manager` procesó correctamente la solicitud y recibió un certificado del proveedor: Certificado de almacenamiento provisional de Let's Encrypt

![Cuando modifica Let's Encrypt para usar `prod` en lugar de `staging`, se usa un certificado de confianza que Let's Encrypt emite, tal como se muestra en el ejemplo siguiente:](media/ingress/staging-certificate.png)

Certificado de Let's Encrypt

![La aplicación de demostración se muestra en el explorador web:](media/ingress/certificate.png)

Primer ejemplo de aplicación

![A continuación, agregue la ruta de acceso */hello-world-two* al FQDN, como *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two* .](media/ingress/app-one.png)

Se muestra la segunda aplicación de demostración con el título personalizado: Segundo ejemplo de aplicación

![Limpieza de recursos](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>En este artículo, se usa Helm para instalar los componentes de entrada, los certificados y las aplicaciones de ejemplo.

Al implementar un gráfico de Helm, se crean algunos recursos de Kubernetes. Estos recursos incluyen pods, implementaciones y servicios. Para limpiar estos recursos, puede eliminar el espacio de nombres de ejemplo completo o los recursos individuales. Eliminación del espacio de nombres de ejemplo y de todos los recursos

### <a name="delete-the-sample-namespace-and-all-resources"></a>Para eliminar el espacio de nombres de ejemplo completo, use el comando `kubectl delete` y especifique el nombre del espacio de nombres.

Todos los recursos del espacio de nombres se eliminan. A continuación, elimine el repositorio de Helm para la aplicación Hola mundo de AKS:

```console
kubectl delete namespace ingress-basic
```

Eliminación de recursos individualmente

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Como alternativa, un enfoque más pormenorizado consiste en eliminar los recursos individuales creados.

Primero, quite los recursos de certificado: Ahora, despliegue una lista de las versiones de Helm con el comando `helm list`.

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Busque los gráficos denominados *nginx-ingress*, *cert-manager* y *aks-helloworld*, tal y como se muestra en la salida del ejemplo siguiente: Elimine las versiones con el comando `helm delete`.

```
$ helm list

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
billowing-kitten        1           Wed Mar  6 19:37:43 2019    DEPLOYED    nginx-ingress-1.3.1     0.22.0      kube-system
loitering-waterbuffalo  1           Wed Mar  6 20:25:01 2019    DEPLOYED    cert-manager-v0.6.6     v0.6.2      kube-system
flabby-deer             1           Wed Mar  6 20:27:54 2019    DEPLOYED    aks-helloworld-0.1.0                default
linting-echidna         1           Wed Mar  6 20:27:59 2019    DEPLOYED    aks-helloworld-0.1.0                default
```

En el ejemplo siguiente se elimina la implementación de entrada NGINX, así como el administrador de certificados y las dos aplicaciones Hola mundo de AKS de ejemplo. A continuación, elimine el repositorio de Helm para la aplicación Hola mundo de AKS:

```
$ helm delete billowing-kitten loitering-waterbuffalo flabby-deer linting-echidna

release "billowing-kitten" deleted
release "loitering-waterbuffalo" deleted
release "flabby-deer" deleted
release "linting-echidna" deleted
```

Elimine el propio espacio de nombres.

```console
helm repo remove azure-samples
```

Use el comando `kubectl delete` y especifique el nombre del espacio de nombres: Finalmente, elimine la ruta de entrada que dirige el tráfico a las aplicaciones de ejemplo:

```console
kubectl delete namespace ingress-basic
```

Pasos siguientes

```console
kubectl delete -f hello-world-ingress.yaml
```

## <a name="next-steps"></a>En este artículo se incluyen algunos componentes externos a AKS.

Para más información sobre estos componentes, consulte las siguientes páginas del proyecto: [CLI de Helm][helm-cli]

- [Controlador de entrada NGINX][nginx-ingress]
- [cert-manager][cert-manager]
- También puede:

[Creación de un controlador de entrada básico con conectividad de red externa][aks-ingress-basic]

- [Habilitación del complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Creación de un controlador de entrada que use una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada que usa sus propios certificados TLS][aks-ingress-own-tls]
- [Crear un controlador de entrada que usa Let's Encrypt para generar automáticamente certificados TLS con una dirección IP pública estática][aks-ingress-static-tls]
- <bpt id="p1">[</bpt>Create an ingress controller that uses Let's Encrypt to automatically generate TLS certificates with a static public IP address<ept id="p1">][aks-ingress-static-tls]</ept>

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[ingress-shim]: https://docs.cert-manager.io/en/latest/tasks/issuing-certificates/ingress-shim.html
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
[aks-ingress-own-tls]: ingress-own-tls.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[client-source-ip]: concepts-network.md#ingress-controllers
[install-azure-cli]: /cli/azure/install-azure-cli