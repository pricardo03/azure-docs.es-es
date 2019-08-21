---
title: Uso de un controlador de entrada de Traefik personalizado y configuración de HTTPS
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 08/13/2019
ms.topic: conceptual
description: Aprenda a configurar Azure Dev Spaces para usar un controlador de entrada de Traefik personalizado y configurar HTTPS con ese controlador de entrada.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s
ms.openlocfilehash: 50908bde65b69cb475391cd30bca758dd571f114
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69037300"
---
# <a name="use-a-custom-traefik-ingress-controller-and-configure-https"></a>Uso de un controlador de entrada de Traefik personalizado y configuración de HTTPS

En este artículo se muestra cómo configurar Azure Dev Spaces para usar un controlador de entrada de Traefik personalizado. En este artículo también se muestra cómo configurar el controlador de entrada personalizado para usar HTTPS.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. En caso de no tener ninguna, puede crear una [cuenta gratuita][azure-account-create].
* [La CLI de Azure instalada][az-cli].
* [Clúster de Azure Kubernetes Service (AKS) con Azure Dev Spaces habilitado][qs-cli].
* [kubectl][kubectl] instalado.
* [Helm 2.13 o superior instalado][helm-installed].
* [Un dominio personalizado][custom-domain] con una [zona DNS][dns-zone] en el mismo grupo de recursos que el clúster de AKS.

## <a name="configure-a-custom-traefik-ingress-controller"></a>Configuración de un controlador de entrada de Traefik personalizado

Conecte el clúster con [kubectl][kubectl], el cliente de la línea de comandos de Kubernetes. Para configurar `kubectl` para conectarse a su clúster de Kubernetes, use el comando [az aks get-credentials][az-aks-get-credentials]. Con este comando se descargan las credenciales y se configura la CLI de Kubernetes para usarlas.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKS
```

Para comprobar la conexión con el clúster, use el comando [kubectl get][kubectl-get] para que devuelva una lista de los nodos del clúster.

```console
$ kubectl get nodes
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
```

Cree un espacio de nombres Kubernetes para el controlador de entrada de Traefik e instálelo mediante `helm`.

```console
kubectl create ns traefik
helm init --wait
helm install stable/traefik --name traefik --namespace traefik --set kubernetes.ingressClass=traefik --set kubernetes.ingressEndpoint.useDefaultPublishedService=true
```

Obtenga la dirección IP del servicio del controlador de entrada de Traefik mediante [kubectl get][kubectl-get].

```console
kubectl get svc -n traefik --watch
```

La salida de ejemplo muestra las direcciones IP de todos los servicios en el espacio de nombres de *Traefik*.

```console
NAME      TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
traefik   LoadBalancer   10.0.205.78   <pending>     80:32484/TCP,443:30620/TCP   20s
...
traefik   LoadBalancer   10.0.205.78   MY_EXTERNAL_IP   80:32484/TCP,443:30620/TCP   60s
```

Agregue un registro *A* a la zona DNS con la dirección IP externa del servicio de Traefik mediante [az network dns record-set a add-record][az-network-dns-record-set-a-add-record].

```console
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name *.traefik \
    --ipv4-address MY_EXTERNAL_IP
```

En el ejemplo anterior se agrega un registro *A* a la zona DNS *MY_CUSTOM_DOMAIN*.

En este artículo, puede usar la [aplicación de ejemplo Bike Sharing de Azure Dev Spaces](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) para demostrar el uso de Azure Dev Spaces. Clone la aplicación desde GitHub y vaya a su directorio:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/charts
```

Abra [values.yaml][values-yaml] y reemplace todas las instancias de *<REPLACE_ME_WITH_HOST_SUFFIX>* por *traefik.MY_CUSTOM_DOMAIN* mediante el dominio para *MY_CUSTOM_DOMAIN*. Reemplace también *kubernetes.io/ingress.class: traefik-azds  # Dev Spaces-specific* por *kubernetes.io/ingress.class: traefik  # Custom Ingress*. A continuación se muestra un ejemplo de un archivo `values.yaml` actualizado:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: traefik  # Custom Ingress
    hosts:
      - dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: traefik  # Custom Ingress
    hosts:
      - dev.gateway.traefik.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
```

Guarde los cambios y cierre el archivo.

Implemente la aplicación de ejemplo mediante `helm install`.

```console
helm install -n bikesharing . --dep-up --namespace dev --atomic
```

En el ejemplo anterior se implementa la aplicación de ejemplo en el espacio de nombres *dev*.

Seleccione el espacio *dev* con la aplicación de ejemplo mediante `azds space select` y muestre las direcciones URL para acceder a la aplicación de ejemplo mediante `azds list-uris`.

```console
azds space select -n dev
azds list-uris
```

La siguiente salida muestra las direcciones URL de ejemplo de `azds list-uris`.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/  Available
http://dev.gateway.traefik.MY_CUSTOM_DOMAIN/         Available
```

Navegue hasta el servicio *bikesharingweb* abriendo la dirección URL pública con el comando `azds list-uris`. En el ejemplo anterior, la dirección URL pública para el servicio *bikesharingweb* es `http://dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/`.

Use el comando `azds space select` para crear un espacio secundario en *dev* y mostrar las direcciones URL para acceder al espacio de desarrollo secundario.

```console
azds space select -n dev/azureuser1 -y
azds list-uris
```

La salida siguiente muestra las direcciones URL de ejemplo de `azds list-uris` para tener acceso a la aplicación de ejemplo en el espacio de desarrollo secundario *azureuser1*.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/  Available
http://azureuser1.s.dev.gateway.traefik.MY_CUSTOM_DOMAIN/         Available
```

Navegue hasta el servicio *bikesharingweb* en el espacio de desarrollo secundario *azureuser1* abriendo la dirección URL pública con el comando `azds list-uris`. En el ejemplo anterior, la dirección URL pública para el servicio *bikesharingweb* en el espacio de desarrollo secundario *azureuser1* es `http://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/`.

## <a name="configure-the-traefik-ingress-controller-to-use-https"></a>Configuración del controlador de entrada de Traefik para usar HTTPS

Cree un archivo `dev-spaces/samples/BikeSharingApp/traefik-values.yaml` similar al ejemplo siguiente. Actualice el valor de *email* con su propio correo electrónico, que se usa para generar el certificado con Let's Encrypt.

```yaml
fullnameOverride: traefik
replicas: 1
cpuLimit: 400m
memoryRequest: 200Mi
memoryLimit: 500Mi
externalTrafficPolicy: Local
kubernetes:
  ingressClass: traefik
  ingressEndpoint:
    useDefaultPublishedService: true
dashboard:
  enabled: false
debug:
  enabled: false
accessLogs:
  enabled: true
  fields:
    defaultMode: keep
    headers:
      defaultMode: keep
      names:
        Authorization: redact
acme:
  enabled: true
  email: "someone@example.com"
  staging: false
  challengeType: tls-alpn-01
ssl:
  enabled: true
  enforced: true
  permanentRedirect: true
  tlsMinVersion: VersionTLS12
  cipherSuites:
    - TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
    - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    - TLS_RSA_WITH_AES_128_GCM_SHA256
    - TLS_RSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
```

Actualice el servicio de *Traefik* con `helm repo update` e incluya el archivo *traefik-values.yaml* que ha creado.

```console
cd ..
helm upgrade traefik stable/traefik --namespace traefik --values traefik-values.yaml
```

El comando anterior ejecuta una nueva versión del servicio de Traefik con los valores de *traefik-values.yaml* y quita el servicio anterior. El servicio de Traefik también crea un certificado TLS mediante Let's Encrypt y comienza a redirigir el tráfico web para usar HTTPS.

> [!NOTE]
> La nueva versión del servicio de Traefik puede tardar unos minutos en iniciarse. Puede comprobar el progreso mediante `kubectl get pods --namespace traefik --watch`.

Navegue hasta la aplicación de ejemplo en el espacio secundario *dev/azureuser1* y observe que se le redirige para usar HTTPS. Observe también que la página se carga, pero el explorador muestra algunos errores. Al abrir la consola del explorador, se muestra el error relacionado con una página HTTPS que intenta cargar recursos HTTP. Por ejemplo:

```console
Mixed Content: The page at 'https://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/devsignin' was loaded over HTTPS, but requested an insecure resource 'http://azureuser1.s.dev.gateway.traefik.MY_CUSTOM_DOMAIN/api/user/allUsers'. This request has been blocked; the content must be served over HTTPS.
```

Para corregir este error, actualice [BikeSharingWeb/azds.yaml][azds-yaml] para usar *traefik* para *kubernetes.io/ingress.class* y el dominio personalizado para *$(hostSuffix)* . Por ejemplo:

```yaml
...
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik
      hosts:
      # This expands to [space.s.][rootSpace.]bikesharingweb.<random suffix>.<region>.azds.io
      - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.traefik.MY_CUSTOM_DOMAIN
...
```

Actualice [BikeSharingWeb/package.json][package-json] con una dependencia para el paquete *url*.

```json
{
...
    "react-responsive": "^6.0.1",
    "universal-cookie": "^3.0.7",
    "url": "0.11.0"
  },
...
```

Actualice el método *getApiHostAsync* en [BikeSharingWeb/pages/helpers.js][helpers-js] para usar HTTPS:

```javascript
...
    getApiHostAsync: async function() {
        const apiRequest = await fetch('/api/host');
        const data = await apiRequest.json();
        
        var urlapi = require('url');
        var url = urlapi.parse(data.apiHost);

        console.log('apiHost: ' + "https://"+url.host);
        return "https://"+url.host;
    },
...
```

Navegue hasta el directorio `BikeSharingWeb` y use `azds up` para ejecutar el servicio de BikeSharingWeb actualizado.

```console
cd BikeSharingWeb/
azds up
```

Navegue hasta la aplicación de ejemplo en el espacio secundario *dev/azureuser1* y observe que se le redirige para usar HTTPS sin ningún error.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de la forma en que Azure Dev Spaces le ayuda a desarrollar aplicaciones más complejas en varios contenedores y cómo puede simplificar el desarrollo en colaboración mediante el uso de distintas versiones o bifurcaciones del código en distintos espacios.

> [!div class="nextstepaction"]
> [Desarrollo en equipo en Azure Dev Spaces][team-development-qs]


[az-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-network-dns-record-set-a-add-record]: /cli/azure/network/dns/record-set/a?view=azure-cli-latest#az-network-dns-record-set-a-add-record
[custom-domain]: ../../app-service/manage-custom-dns-buy-domain.md#buy-the-domain
[dns-zone]: ../../dns/dns-getstarted-cli.md
[qs-cli]: ../quickstart-cli.md
[team-development-qs]: ../quickstart-team-development.md

[azds-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/azds.yaml
[azure-account-create]: https://azure.microsoft.com/free
[helm-installed]: https://github.com/helm/helm/blob/master/docs/install.md
[helpers-js]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/pages/helpers.js#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[package-json]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/package.json
[values-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml