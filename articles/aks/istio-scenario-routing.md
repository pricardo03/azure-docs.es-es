---
title: Enrutamiento inteligente y versiones de valor controlado con Istio en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo usar Istio para proporcionar enrutamiento inteligente e implementar versiones de valor controlado en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 04/19/2019
ms.author: pabouwer
ms.openlocfilehash: bd660a2b6ffb96478c3170cc7013ff22518b758f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64702204"
---
# <a name="use-intelligent-routing-and-canary-releases-with-istio-in-azure-kubernetes-service-aks"></a>Uso del enrutamiento inteligente y de las versiones de valor controlado con Istio en Azure Kubernetes Service (AKS)

[Istio][istio-github] es una malla de servicio de código abierto que proporciona un conjunto clave de funcionalidades en los microservicios de un clúster de Kubernetes. Estas características incluyen la administración del tráfico, la seguridad e identidad de servicio, el cumplimiento de directivas y la observación. Para más información sobre Istio, vea la documentación del sitio oficial [What is Istio?][istio-docs-concepts] (¿Qué es Istio?).

En este artículo se explica cómo usar la funcionalidad de administración de tráfico de Istio. Se usa una aplicación de votación de AKS de ejemplo para explorar el enrutamiento inteligente y las versiones de valor controlado.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Implementación de la aplicación
> * Actualizar la aplicación
> * Lanzamiento de una versión de valor controlado de la aplicación
> * Finalización del lanzamiento

## <a name="before-you-begin"></a>Antes de empezar

> [!NOTE]
> Este escenario se ha probado con la versión de Istio `1.1.3`.

Los pasos detallados en este artículo se suponen que ha creado un clúster de AKS (Kubernetes `1.11` y versiones posteriores, con RBAC habilitado) y que ha establecido un `kubectl` conexión con el clúster. También necesitará Istio instalado en el clúster.

Si necesita ayuda con cualquiera de estos elementos, vea el [inicio rápido AKS] [ aks-quickstart] y [instalar Istio en AKS] [ istio-install] orientación.

## <a name="about-this-application-scenario"></a>Acerca de este escenario de aplicación

La aplicación de votación de AKS de ejemplo proporciona dos opciones de voto (**gatos** o **perros**) a los usuarios. Hay un componente de almacenamiento que conserva el número de votos de cada opción. Además, hay un componente de análisis que proporciona detalles sobre los votos emitidos para cada opción.

En este escenario de aplicación, primero debe implementar la versión `1.0` de la aplicación de votación y la versión `1.0` del componente de análisis. El componente de análisis proporciona recuentos simples del número de votos. La aplicación de votación y el componente de análisis de interactuar con la versión `1.0` del componente de almacenamiento, que está respaldado por Redis.

Actualizar el componente de análisis a la versión `1.1`, que proporciona recuentos y ahora los totales y porcentajes.

Un subconjunto de la versión de prueba de los usuarios `2.0` de la aplicación a través de un lanzamiento controlado. Esta nueva versión usa un componente de almacenamiento respaldado por una base de datos MySQL.

Una vez que esté seguro de que la versión `2.0` funciona según lo previsto en el subconjunto de usuarios, desplegar versión `2.0` a todos los usuarios.

## <a name="deploy-the-application"></a>Implementación de la aplicación

Vamos a empezar con la implementación de la aplicación en el clúster de Azure Kubernetes Service (AKS). El siguiente diagrama muestra lo que se ejecuta al final de esta sección, versión `1.0` de todos los componentes con las solicitudes entrantes que atiende a través de la puerta de enlace de entrada Istio:

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-01.png)

Los artefactos necesarios para avanzar en este artículo están disponibles en el repositorio [Azure-Samples/aks-voting-app][github-azure-sample] de GitHub. Puede descargar los artefactos o clonar el repositorio como sigue:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

Cambie a la siguiente carpeta en el repositorio clonado o descargado y ejecute todos los pasos subsiguientes desde esta carpeta:

```console
cd scenarios/intelligent-routing-with-istio
```

Primero, cree un espacio de nombres en el clúster de AKS para la aplicación de votación de AKS de ejemplo denominada `voting` como sigue:

```azurecli
kubectl create namespace voting
```

Etiquete el espacio de nombres con `istio-injection=enabled`. Esta etiqueta indica a Istio que inyecte automáticamente los servidores proxy de Istio como patrones sidecar en todos los pods de este espacio de nombres.

```azurecli
kubectl label namespace voting istio-injection=enabled
```

Ahora vamos a crear los componentes de la aplicación de votación de AKS. Creación de estos componentes en el `voting` espacio de nombres creado en el paso anterior.

```azurecli
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

La salida del ejemplo siguiente muestra los recursos que se va a crear:

```console
deployment.apps/voting-storage-1-0 created
service/voting-storage created
deployment.apps/voting-analytics-1-0 created
service/voting-analytics created
deployment.apps/voting-app-1-0 created
service/voting-app created
```

> [!NOTE]
> Istio tiene algunos requisitos específicos en relación a los pods y los servicios. Para más información, vea la documentación sobre [requisitos para pods y servicios de Istio][istio-requirements-pods-and-services].

Para ver los pods creados, use el comando [kubectl get pods][kubectl-get] como sigue:

```azurecli
kubectl get pods -n voting
```

La salida del ejemplo siguiente se muestra que hay tres instancias de la `voting-app` pod y una única instancia de ambos el `voting-analytics` y `voting-storage` pods. Cada pod tiene dos contenedores. Uno de estos contenedores es el componente y el otro es el `istio-proxy`:

```console
NAME                                    READY     STATUS    RESTARTS   AGE
voting-analytics-1-0-57c7fccb44-ng7dl   2/2       Running   0          39s
voting-app-1-0-956756fd-d5w7z           2/2       Running   0          39s
voting-app-1-0-956756fd-f6h69           2/2       Running   0          39s
voting-app-1-0-956756fd-wsxvt           2/2       Running   0          39s
voting-storage-1-0-5d8fcc89c4-2jhms     2/2       Running   0          39s
```

Para obtener información sobre el pod, use [kubectl describe pod][kubectl-describe]. Reemplace el nombre del pod con el nombre de un pod en su propio clúster de AKS de la salida anterior:

```azurecli
kubectl describe pod voting-app-1-0-956756fd-d5w7z --namespace voting
```

El `istio-proxy` contenedor automáticamente ha sido insertado por Istio para administrar el tráfico de red hacia y desde los componentes, como se muestra en la salida del ejemplo siguiente:

```
[...]
Containers:
  voting-app:
    Image:         mcr.microsoft.com/aks/samples/voting/app:1.0
    ...
  istio-proxy:
    Image:         docker.io/istio/proxyv2:1.1.3
[...]
```

No puede conectarse a la aplicación de votación hasta que se cree la [puerta de enlace][istio-reference-gateway] y el [servicio virtual][istio-reference-virtualservice] de Istio. Los recursos de Istio enrutan el tráfico desde la puerta de enlace de entrada de Istio predeterminada hasta nuestra aplicación.

> [!NOTE]
> Una **puerta de enlace** es un componente del perímetro de la malla de servicio que recibe el tráfico HTTP y TCP entrante o saliente.
> 
> Un **servicio virtual** define un conjunto de reglas de enrutamiento para uno o varios servicios de destino.

Use el `kubectl apply` comando para implementar la puerta de enlace y el servicio Virtual yaml. No olvide especificar estos recursos se implementan en el espacio de nombres.

```azurecli
kubectl apply -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

La salida del ejemplo siguiente muestra la nueva puerta de enlace y servicio Virtual que se va a crear:

```console
virtualservice.networking.istio.io/voting-app created
gateway.networking.istio.io/voting-app-gateway created
```

Obtenga la dirección IP de la puerta de enlace de entrada de Istio con el comando siguiente:

```azurecli
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

En la siguiente salida de ejemplo se muestra la dirección IP de la puerta de enlace de entrada:

```
20.188.211.19
```

Abra un explorador y pegue la dirección IP. Se muestra la aplicación de votación de AKS de ejemplo.

![La aplicación de votación de AKS se ejecuta en nuestro clúster de AKS con Istio habilitado.](media/istio/deploy-app-01.png)

La información en la parte inferior de la pantalla se muestra que la aplicación utiliza la versión `1.0` de `voting-app` y versión `1.0` de `voting-storage` (Redis).

## <a name="update-the-application"></a>Actualizar la aplicación

Vamos a implementar una versión nueva del componente de análisis. Esta nueva versión `1.1` muestra los totales y porcentajes además el recuento para cada categoría.

El siguiente diagrama muestra lo que se ejecutará al final de esta sección, solo la versión `1.1` de nuestro `voting-analytics` componente tiene el enrutamiento de tráfico desde el `voting-app` componente. Aunque versión `1.0` de nuestro `voting-analytics` componente continúa ejecutándose y se hace referencia mediante el `voting-analytics` servicio, los proxies Istio deniegan el tráfico hacia y desde él.

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-02.png)

Vamos a implementar la versión `1.1` de la `voting-analytics` componente. Crear este componente en el `voting` espacio de nombres:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

La salida del ejemplo siguiente muestra los recursos que se va a crear:

```console
deployment.apps/voting-analytics-1-1 created
```

Vuelva a abrir la aplicación de votación de AKS de ejemplo en un explorador con el uso de la dirección IP de la puerta de enlace de entrada de Istio obtenida en el paso anterior.

El explorador alterna entre las dos vistas que se muestran a continuación. Puesto que utiliza un Kubernetes [servicio] [ kubernetes-service] para el `voting-analytics` componente con sólo un selector de una sola etiqueta (`app: voting-analytics`), Kubernetes utiliza el comportamiento predeterminado de round robin entre la Pods que coinciden con ese selector. En este caso, es tanto versión `1.0` y `1.1` de su `voting-analytics` pods.

![La versión 1.0 del componente de análisis que se ejecuta en la aplicación de votación de AKS.](media/istio/deploy-app-01.png)

![La versión 1.1 del componente de análisis que se ejecuta en la aplicación de votación de AKS.](media/istio/update-app-01.png)

Puede visualizar el cambio entre las dos versiones de la `voting-analytics` componente como se indica a continuación. Recuerde usar la dirección IP de su propia puerta de enlace de entrada de Istio.

Bash 

```bash
INGRESS_IP=20.188.211.19
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

PowerShell

```powershell
$INGRESS_IP="20.188.211.19"
(1..5) |% { (Invoke-WebRequest -Uri $INGRESS_IP).Content.Split("`n") | Select-String -Pattern "results" }
```

La salida de ejemplo siguiente muestra la parte pertinente del sitio web devuelto a medida que el sitio cambia entre versiones:

```
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

### <a name="lock-down-traffic-to-version-11-of-the-application"></a>Bloqueo del tráfico a la versión 1.1 de la aplicación

Ahora vamos a bloquear el tráfico únicamente a la versión `1.1` de la `voting-analytics` componente y a la versión `1.0` de la `voting-storage` componente. A continuación, defina las reglas de enrutamiento de todos los demás componentes.

> * Un **servicio virtual** define un conjunto de reglas de enrutamiento para uno o varios servicios de destino.
> * Una **regla de destino** define las directivas de tráfico y las directivas específicas de la versión.
> * Una **directiva** define qué métodos de autenticación pueden aceptarse en las cargas de trabajo.

Use la `kubectl apply` comando para reemplazar la definición de servicio Virtual en su `voting-app` y agregue [las reglas de destino] [ istio-reference-destinationrule] y [servicios virtuales] [ istio-reference-virtualservice] para los demás componentes. Agregará un [directiva] [ istio-reference-policy] a la `voting` espacio de nombres para asegurarse de que todos se comunican entre los servicios se protege mediante certificados de cliente y de TLS mutua.

* La directiva tiene `peers.mtls.mode` establecido en `STRICT` para garantizar que se apliquen TLS mutua entre los servicios en la `voting` espacio de nombres.
* También establecemos la `trafficPolicy.tls.mode` a `ISTIO_MUTUAL` en todas las reglas de destino. Istio ofrece servicios con identidades seguras y protege las comunicaciones entre los servicios con certificados cliente y Mutual TLS que Istio administra de forma transparente.

```azurecli
kubectl apply -f istio/step-2-update-and-add-routing-for-all-components.yaml --namespace voting
```

La salida del ejemplo siguiente muestra la nueva directiva, las reglas de destino y los servicios Virtual que se va a actualizar/crear:

```console
virtualservice.networking.istio.io/voting-app configured
policy.authentication.istio.io/default created
destinationrule.networking.istio.io/voting-app created
destinationrule.networking.istio.io/voting-analytics created
virtualservice.networking.istio.io/voting-analytics created
destinationrule.networking.istio.io/voting-storage created
virtualservice.networking.istio.io/voting-storage created
```

Si abre la aplicación Voting AKS en un explorador nuevo, solo la nueva versión `1.1` de la `voting-analytics` componente lo usa el `voting-app` componente.

![La versión 1.1 del componente de análisis que se ejecuta en la aplicación de votación de AKS.](media/istio/update-app-01.png)

Puede ver que le es ahora sólo enrutará a la versión `1.1` de su `voting-analytics` componente como se indica a continuación. Recuerde usar la dirección IP de su propia puerta de enlace de entrada de Istio:

Bash 

```bash
INGRESS_IP=20.188.211.19
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

PowerShell

```powershell
$INGRESS_IP="20.188.211.19"
(1..5) |% { (Invoke-WebRequest -Uri $INGRESS_IP).Content.Split("`n") | Select-String -Pattern "results" }
```

La salida de ejemplo siguiente muestra la parte pertinente del sitio web devuelto:

```
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Confirmemos ahora que Istio utiliza TLS mutua para proteger las comunicaciones entre cada uno de nuestros servicios. Para esto, usaremos el [comprobación de autenticación tls] [ istioctl-authn-tls-check] comando el `istioctl` cliente binario, que tiene la forma siguiente.

```console
istioctl authn tls-check <pod-name[.namespace]> [<service>]
```

Este conjunto de comandos proporcionan información sobre el acceso a los servicios especificados, de todos los pods que se encuentran en un espacio de nombres y coinciden con un conjunto de etiquetas:

Bash

```bash
# mTLS configuration between each of the istio ingress pods and the voting-app service
kubectl get pod -n istio-system -l app=istio-ingressgateway | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.istio-system voting-app.voting.svc.cluster.local

# mTLS configuration between each of the voting-app pods and the voting-analytics service
kubectl get pod -n voting -l app=voting-app | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-analytics.voting.svc.cluster.local

# mTLS configuration between each of the voting-app pods and the voting-storage service
kubectl get pod -n voting -l app=voting-app | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-storage.voting.svc.cluster.local

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
kubectl get pod -n voting -l app=voting-analytics,version=1.1 | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-storage.voting.svc.cluster.local
```

PowerShell

```powershell
# mTLS configuration between each of the istio ingress pods and the voting-app service
(kubectl get pod -n istio-system -l app=istio-ingressgateway | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".istio-system") voting-app.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-app pods and the voting-analytics service
(kubectl get pod -n voting -l app=voting-app | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-analytics.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-app pods and the voting-storage service
(kubectl get pod -n voting -l app=voting-app | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-storage.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
(kubectl get pod -n voting -l app=voting-analytics,version=1.1 | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-storage.voting.svc.cluster.local } }
```

Esta salida del ejemplo siguiente se muestra que se aplica TLS mutua para cada una de nuestras consultas anteriores. La salida también muestra la directiva y las reglas de destino que exige el TLS mutua:

```console
# mTLS configuration between istio ingress pods and the voting-app service
HOST:PORT                                    STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-app.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-app/voting

# mTLS configuration between each of the voting-app pods and the voting-analytics service
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting

# mTLS configuration between each of the voting-app pods and the voting-storage service
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
```

## <a name="roll-out-a-canary-release-of-the-application"></a>Lanzamiento de una versión de valor controlado de la aplicación

Ahora vamos a implementar una nueva versión `2.0` de la `voting-app`, `voting-analytics`, y `voting-storage` componentes. El nuevo `voting-storage` componente usan MySQL en lugar de Redis y el `voting-app` y `voting-analytics` componentes se actualizan para que puedan usar este nuevo `voting-storage` componente.

El `voting-app` componente ahora admite la funcionalidad de marca de característica. Esta marca de características permite probar la funcionalidad de la versión de valor controlado de Istio para un subconjunto de usuarios.

El siguiente diagrama muestra lo que debe ejecutar al final de esta sección.

* Versión `1.0` de la `voting-app` componente, versión `1.1` de la `voting-analytics` componente y la versión `1.0` de la `voting-storage` componente son capaces de comunicarse entre sí.
* Versión `2.0` de la `voting-app` componente, versión `2.0` de la `voting-analytics` componente y la versión `2.0` de la `voting-storage` componente son capaces de comunicarse entre sí.
* Versión `2.0` de la `voting-app` componente solo son accesibles a los usuarios que tienen un conjunto de marca de característica específica. Este cambio se administra con una marca de características mediante una cookie.

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-03.png)

En primer lugar, actualice las reglas de destino y los servicios virtuales de Istio destinados a estos nuevos componentes. Estas actualizaciones garantizan que el tráfico no se enrute incorrectamente a los nuevos componentes y que los usuarios no tengan un acceso inesperado:

```azurecli
kubectl apply -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

La salida del ejemplo siguiente muestra las reglas de destino y los servicios Virtual que se está actualizando:

```console
destinationrule.networking.istio.io/voting-app configured
virtualservice.networking.istio.io/voting-app configured
destinationrule.networking.istio.io/voting-analytics configured
virtualservice.networking.istio.io/voting-analytics configured
destinationrule.networking.istio.io/voting-storage configured
virtualservice.networking.istio.io/voting-storage configured
```

A continuación, vamos a agregar los objetos de Kubernetes para la nueva versión `2.0` componentes. También actualiza el `voting-storage` servicio para incluir el `3306` puerto MySQL:

```azurecli
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

En la siguiente salida de ejemplo se muestra que los objetos de Kubernetes se han actualizado o creado correctamente:

```console
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Espere hasta que todas las versiones de la `2.0` se ejecutan los pods. Use la [kubectl get pods] [ kubectl-get] comando para ver todos los pods en la `voting` espacio de nombres:

```azurecli
kubectl get pods --namespace voting
```

Ahora podrá alternar entre la versión `1.0` y versión `2.0` (controlado) de la aplicación de votación. El botón de alternancia de la marca de características en la parte inferior de la pantalla establece una cookie. Esta cookie se usa por la `voting-app` servicio Virtual para dirigir usuarios a la nueva versión `2.0`.

![Versión 1.0 de la aplicación de votación de AKS: la marca de características NO ESTÁ establecida.](media/istio/canary-release-01.png)

![Versión 2.0 de la aplicación de votación de AKS: la marca de características ESTÁ establecida.](media/istio/canary-release-02.png)

Los recuentos de votos son diferentes entre las versiones de la aplicación. Esta diferencia resalta que se están usando dos back-ends de almacenamiento distintos.

## <a name="finalize-the-rollout"></a>Finalización del lanzamiento

Una vez que ha probado correctamente el lanzamiento controlado, actualice el `voting-app` el servicio Virtual para enrutar todo el tráfico a la versión `2.0` de la `voting-app` componente. Todos los usuarios, a continuación, verán versión `2.0` de la aplicación, independientemente de si se establece la marca de característica o no:

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-04.png)

Actualice todas las reglas de destino para quitar las versiones de los componentes que ya no desea que estén activas. A continuación, actualice todos los servicios virtuales para que dejen de hacer referencia a dichas versiones.

Como ya no hay tráfico a ninguna de las versiones anteriores de los componentes, ahora puede eliminar con seguridad todas las implementaciones de dichos componentes.

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-05.png)

Ya ha lanzado correctamente una versión nueva de la aplicación de votación de AKS.

## <a name="clean-up"></a>Limpieza 

Puede quitar la aplicación de votación de AKS se usa en este escenario de su clúster de AKS eliminando el `voting` espacio de nombres como sigue:

```azurecli
kubectl delete namespace voting
```

La salida del ejemplo siguiente se muestra que se han quitado todos los componentes de la aplicación de votación de AKS desde el clúster de AKS.

```console
namespace "voting" deleted
```

## <a name="next-steps"></a>Pasos siguientes

Puede explorar escenarios adicionales con el [ejemplo de la aplicación Bookinfo en Istio][istio-bookinfo-example].

<!-- LINKS - external -->
[github-azure-sample]: https://github.com/Azure-Samples/aks-voting-app
[istio-github]: https://github.com/istio/istio

[istio]: https://istio.io
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-requirements-pods-and-services]: https://istio.io/docs/setup/kubernetes/prepare/requirements/
[istio-reference-gateway]: https://istio.io/docs/reference/config/networking/v1alpha3/gateway/
[istio-reference-policy]: https://istio.io/docs/reference/config/istio.authentication.v1alpha1/#Policy
[istio-reference-virtualservice]: https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/
[istio-reference-destinationrule]: https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[istioctl-authn-tls-check]: https://istio.io/docs/reference/commands/istioctl/#istioctl-authn-tls-check

[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-install]: ./istio-install.md
