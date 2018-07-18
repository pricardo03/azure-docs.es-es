---
title: Configuración de la entrada con un clúster de Azure Kubernetes Service (AKS)
description: Instale y configure un controlador de entrada NGINX en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/25/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 9bee80ebbaf0568706428d673d584819b1daa143
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096995"
---
# <a name="https-ingress-on-azure-kubernetes-service-aks"></a>Entrada HTTPS en Azure Kubernetes Service (AKS)

Un controlador de entrada es un software que proporciona el proxy inverso, el enrutamiento del tráfico configurable y la terminación de TLS para los servicios de Kubernetes. Los recursos de entrada de Kubernetes se usan para configurar las reglas de entrada y las rutas de los distintos servicios de Kubernetes. Mediante reglas de entrada y un controlador de entrada, se puede enrutar el tráfico a varios servicios de Kubernetes de un clúster de Kubernetes con una sola dirección externa.

Este documento le guiará con la implementación de ejemplo del [controlador de entrada NGINX][nginx-ingress] en un clúster de Azure Kubernetes Service (AKS). Además, con el proyecto [cert-manager][cert-manager] se generan y se configuran automáticamente certificados [Let's Encrypt][lets-encrypt]. Por último, en el clúster de AKS se ejecutan varias aplicaciones, a las cuales se puede acceder con una sola dirección.

## <a name="install-an-ingress-controller"></a>Instalación de un controlador de entrada

Instale el controlador de entrada NGINX con Helm. Consulte la [documentación][nginx-ingress] del controlador de entrada NGINX para información de implementación detallada.

En este ejemplo se instala el controlador en el espacio de nombres `kube-system`, el cual se puede modificar a uno de su elección. Si su clúster de AKS no tiene RBAC habilitado, agregue `--set rbac.create=false` al comando. Para obtener más información, consulte el [gráfico de nginx-ingress][nginx-ingress].

```bash
helm install stable/nginx-ingress --namespace kube-system
```

Durante la instalación se crea una dirección IP pública de Azure para el controlador de entrada. Obtenga la dirección IP pública con el comando de servicio get kubectl. La asignación de la dirección IP al servicio puede tardar un tiempo.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Dado que no se han creado reglas de entrada, si llega hasta la dirección IP pública, se le dirigirá a la página 404 predeterminada de los controladores de entrada NGINX.

![Back-end predeterminado de NGINX](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Configuración del nombre DNS

Dado que se usan certificados HTTPS, debe configurar un nombre de dominio completo para la dirección IP de los controladores de entrada. En este ejemplo, se crea un nombre de dominio completo de Azure con la CLI de Azure. Actualice el script con la dirección IP del controlador de entrada y el nombre de dominio completo que desee.

```bash
#!/bin/bash

# Public IP address
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with dns name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

El controlador de entrada ahora debería ser accesible mediante el nombre de dominio completo.

## <a name="install-cert-manager"></a>Instalar cert-manager

El controlador de entrada NGINX es compatible con la terminación de TLS. Aunque hay varias maneras de recuperar y configurar certificados para HTTPS, en este documento se muestra el uso de [cert-manager][cert-manager], que proporciona una funcionalidad de generación y administración automáticas de certificados [Let's Encrypt][lets-encrypt].

Para instalar el controlador de cert-manager, use el siguiente comando de instalación de Helm.

```bash
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Si el clúster no tiene RBAC habilitado, use este comando.

```bash
helm install stable/cert-manager \
  --set ingressShim.defaultIssuerName=letsencrypt-prod \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

Para obtener más información sobre cert-manager, consulte el [proyecto de cert manager][cert-manager].

## <a name="create-ca-cluster-issuer"></a>Crear el emisor de clúster de entidad de certificación

Para poder emitir certificados, cert-manager requiere un recurso [Issuer] [ cert-manager-issuer] o [ClusterIssuer] [ cert-manager-cluster-issuer]. La funcionalidad de los recursos es idéntica, sin embargo `Issuer` funciona en un único espacio de nombres mientras que `ClusterIssuer` funciona en todos los espacios de nombres. Para más información, consulte la documentación de [cert-manager issuer][cert-manager-issuer].

Cree un recurso de emisor del clúster con el siguiente manifiesto. Actualice la dirección de correo electrónico con una dirección válida de su organización.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-prod
    http01: {}
```

## <a name="create-certificate-object"></a>Crear un objeto de certificado

A continuación, se debe crear un recurso de certificado. El recurso de certificado define el certificado X.509 deseado. Para más información, vea los [certificados de cert-manager certificates][cert-manager-certificates].

Cree el recurso de certificado con el siguiente manifiesto.

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
    name: letsencrypt-prod
    kind: ClusterIssuer
```

## <a name="run-application"></a>Ejecución de la aplicación

Hasta este momento se han configurado un controlador de entrada y una solución de administración de certificados. Ahora ejecute algunas aplicaciones en el clúster de AKS.

En este ejemplo, se ejecutan varias instancias de una sencilla aplicación Hola mundo con Helm.

Antes de ejecutar la aplicación, agregue el repositorio de ejemplos de Helm de Azure al sistema de desarrollo.

```bash
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Ejecute el gráfico de Hola mundo de AKS con el siguiente comando:

```bash
helm install azure-samples/aks-helloworld
```

A continuación, instale una segunda instancia de la aplicación Hola mundo.

Para esta segunda instancia, especifique un nuevo título de manera que las dos aplicaciones se distingan visualmente. También debe especificar un nombre de servicio único. Estas configuraciones se pueden ver en el siguiente comando.

```bash
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Creación de la ruta de entrada

Ambas aplicaciones se ejecutan ahora en el clúster de Kubernetes, sin embargo, se han configurado con un servicio de tipo `ClusterIP`. Por lo tanto, no se puede acceder a ellas desde internet. Para que estén disponibles, cree un recurso de entrada de Kubernetes. El recurso de entrada configura las reglas de enrutamiento del tráfico a una de las dos aplicaciones.

Cree un archivo denominado `hello-world-ingress.yaml` y cópielo en el siguiente código YAML.

Tenga en cuenta que el tráfico a la dirección `https://demo-aks-ingress.eastus.cloudapp.azure.com/` se enruta al servicio denominado `aks-helloworld`. El tráfico a la dirección `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` se enruta al servicio `ingress-demo`.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-prod
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

Cree el recurso de entrada con el comando `kubectl apply`.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Prueba de la configuración de entrada

Busque el nombre de dominio completo del controlador de entrada de Kubernetes para ver la aplicación Hola mundo.

![Primer ejemplo de aplicación](media/ingress/app-one.png)

Ahora busque el nombre de dominio completo del controlador de entrada con la ruta de acceso `/hello-world-two`, verá la aplicación Hola mundo con el título personalizado.

![Segundo ejemplo de aplicación](media/ingress/app-two.png)

Observe que la conexión está cifrada y que se utiliza un certificado emitido por Let's Encrypt.

![Certificado de Let's Encrypt](media/ingress/certificate.png)

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el software que se muestra en este documento.

- [CLI de Helm][helm-cli]
- [Controlador de entrada NGINX][nginx-ingress]
- [cert-manager][cert-manager]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
