---
title: Enrutamiento inteligente y versiones de valor controlado con Istio en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo usar Istio para proporcionar enrutamiento inteligente e implementar versiones de valor controlado en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 12/3/2018
ms.author: pabouwer
ms.openlocfilehash: 0a4e5e7e310a9949ee59291c2032eafda46955a9
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "52893341"
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

En los pasos detallados en este artículo se da por hecho que ha creado un clúster de AKS (Kubernetes 1.10 y versiones posteriores, con RBAC habilitado) y que ha establecido una conexión `kubectl` con el clúster. Istio también debe estar instalado en el clúster.

Si necesita ayuda con cualquiera de estos elementos, vea el [inicio rápido de AKS][aks-quickstart] y la [guía de instalación de Istio en AKS][istio-install].

## <a name="about-this-application-scenario"></a>Acerca de este escenario de aplicación

La aplicación de votación de AKS de ejemplo proporciona dos opciones de voto (gatos o perros) a los usuarios. Hay un componente de almacenamiento que conserva el número de votos de cada opción. Además, hay un componente de análisis que proporciona detalles sobre los votos emitidos para cada opción.

En este artículo, primero debe implementar la versión *1.0* de la aplicación de votación y la versión *1.0* del componente de análisis. El componente de análisis proporciona recuentos simples del número de votos. La aplicación de votación y el componente de análisis interactúan con la versión *1.0* del componente de almacenamiento, que está respaldada por Redis.

Actualice el componente de análisis a la versión *1.1*, que proporciona recuentos, totales y porcentajes.

Un subconjunto de usuarios prueban la versión *2.0* de la aplicación mediante una versión de valor controlado. Esta nueva versión usa un componente de almacenamiento respaldado por una base de datos MySQL.

Una vez que esté seguro de que la versión *2.0* funciona según lo previsto en el subconjunto de usuarios, lance la versión *2.0* a todos los usuarios.

## <a name="deploy-the-application"></a>Implementación de la aplicación

Vamos a empezar con la implementación de la aplicación en el clúster de Azure Kubernetes Service (AKS). En el diagrama siguiente se muestra lo que se ejecuta al final de esta sección, la versión *1.0* de todos los componentes con solicitudes de entrada atendidas mediante la puerta de enlace de entrada de Istio:

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-01.png)

Los artefactos necesarios para avanzar en este artículo están disponibles en el repositorio [Azure-Samples/aks-voting-app][github-azure-sample] de GitHub. Puede descargar los artefactos o clonar el repositorio como sigue:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

Cambie a la siguiente carpeta en el repositorio clonado o descargado y ejecute todos los pasos subsiguientes desde esta carpeta:

```console
cd scenarios/intelligent-routing-with-istio
```

Primero, cree un espacio de nombres en el clúster de AKS para la aplicación de votación de AKS de ejemplo denominada *votación* como se indica a continuación:

```console
kubectl create namespace voting
```

Etiquete el espacio de nombres con `istio-injection=enabled`. Esta etiqueta indica a Istio que inyecte automáticamente los servidores proxy de Istio como patrones sidecar en todos los pods de este espacio de nombres.

```console
kubectl label namespace voting istio-injection=enabled
```

Ahora vamos a crear los componentes de la aplicación de votación de AKS. Cree estos componentes en el espacio de nombres de *votación* creado en un paso anterior.

```console
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

La siguiente salida de ejemplo muestra que los recursos se crearon correctamente:

```
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

```console
kubectl get pods -n voting
```

En la salida de ejemplo siguiente se muestra que hay tres instancias del pod *voting-app* y una única instancia de los pods *voting-analytics* y *voting-storage*. Cada pod tiene dos contenedores. Uno de estos contenedores es el componente y el otro es *istio-proxy*:

```
NAME                                    READY     STATUS    RESTARTS   AGE
voting-analytics-1-0-669f99dcc8-lzh7k   2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-bdmld         2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-gcrng         2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-strzc         2/2       Running   0          1m
voting-storage-1-0-7954799d96-5fv9r     2/2       Running   0          1m
```

Para obtener información sobre el pod, use el [pod kubectl describe][kubectl-describe]. Reemplace el nombre del pod con el nombre de un pod en su propio clúster de AKS de la salida anterior:

```console
kubectl describe pod voting-app-1-0-6c65c4bdd4-bdmld --namespace voting
```

Istio ha inyectado automáticamente el contenedor *istio-proxy* para administrar el tráfico de red hacia y desde los componentes, como se muestra en la siguiente salida de ejemplo:

```
[...]
Containers:
  voting-app:
    Image:         mcr.microsoft.com/aks/samples/voting/app:1.0
    ...
  istio-proxy:
    Image:         docker.io/istio/proxyv2:1.0.4
[...]
```

No puede conectarse a la aplicación de votación hasta que se cree la [puerta de enlace][istio-reference-gateway] y el [servicio virtual][istio-reference-virtualservice] de Istio. Los recursos de Istio enrutan el tráfico desde la puerta de enlace de entrada de Istio predeterminada hasta nuestra aplicación.

> [!NOTE]
> Una *puerta de enlace* es un componente del perímetro de la malla de servicio que recibe el tráfico HTTP y TCP entrante o saliente.
>
> Un *servicio virtual* define un conjunto de reglas de enrutamiento para uno o varios servicios de destino.

Use el binario de cliente `istioctl` para implementar la puerta de enlace y el servicio virtual yaml. Igual que con el comando `kubectl apply`, recuerde especificar el espacio de nombres en el que se implementan estos recursos.

```console
istioctl create -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

Obtenga la dirección IP de la puerta de enlace de entrada de Istio con el uso del comando siguiente:

```console
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

En la siguiente salida de ejemplo se muestra la dirección IP de la puerta de enlace de entrada:

```
52.187.250.239
```

Abra un explorador y pegue la dirección IP. Se muestra la aplicación de votación de AKS de ejemplo.

![La aplicación de votación de AKS se ejecuta en nuestro clúster de AKS con Istio habilitado.](media/istio/deploy-app-01.png)

La información de la parte inferior de la pantalla muestra que la aplicación usa la versión *1.0* de *voting-app* y la versión *1.0* (Redis) como opción de almacenamiento.

## <a name="update-the-application"></a>Actualizar la aplicación

Vamos a implementar una versión nueva del componente de análisis. Esta nueva versión *1.1* muestra los totales y porcentajes además del recuento de cada categoría.

En el diagrama siguiente se muestra lo que se ejecuta al final de esta sección, solo la versión *1.1* de nuestro componente *voting-analytics* tiene tráfico enrutado desde el componente *voting-app*. Aunque la versión *1.0* de nuestro componente *voting-analytics* continúa ejecutándose y el servicio *voting-analytics* hace referencia a ella, los servidores proxy de Istio deniegan el tráfico hacia y desde dicha versión.

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-02.png)

Vamos a implementar la versión *1.1* del componente *voting-analytics*. Cree este componente en el espacio de nombres *voting*:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

Vuelva a abrir la aplicación de votación de AKS de ejemplo en un explorador con el uso de la dirección IP de la puerta de enlace de entrada de Istio obtenida en el paso anterior.

El explorador alterna entre las dos vistas que se muestran a continuación. Puesto que utiliza un [servicio][kubernetes-service] de Kubernetes para el componente *voting-analytics* con un único selector de etiquetas (`app: voting-analytics`), Kubernetes usa el comportamiento predeterminado de round robin entre los pods que coinciden con dicho selector. En este caso, se trata de las versiones *1.0* y *1.1* de los pods *voting-analytics*.

![La versión 1.0 del componente de análisis que se ejecuta en la aplicación de votación de AKS.](media/istio/deploy-app-01.png)

![La versión 1.1 del componente de análisis que se ejecuta en la aplicación de votación de AKS.](media/istio/update-app-01.png)

Puede visualizar el cambio entre las dos versiones del componente *voting-analytics* como sigue. Recuerde usar la dirección IP de su propia puerta de enlace de entrada de Istio.

```console
INGRESS_IP=52.187.250.239
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
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

Ahora se va a bloquear el tráfico solo a la versión *1.1* del componente *voting-analytics* y a la versión *1.0* del componente *voting-storage*. A continuación, defina las reglas de enrutamiento de todos los demás componentes.

> * Un *servicio virtual* define un conjunto de reglas de enrutamiento para uno o varios servicios de destino.
> * Una *regla de destino* define las directivas de tráfico y las directivas específicas de la versión.
> * Una *directiva* define qué métodos de autenticación pueden aceptarse en las cargas de trabajo.

Use el binario de cliente `istioctl` para reemplazar la definición de servicio virtual en el componente *voting-app* y agregue [reglas de destino][istio-reference-destinationrule] y [servicios virtuales][istio-reference-virtualservice] para los demás componentes.

Agregue también una [directiva][istio-reference-policy] al espacio de nombres *voting* para asegurarse de que todas las comunicaciones entre los servicios están protegidas con certificados de cliente y Mutual TLS.

Como hay una definición de servicio virtual existente para el componente *voting-app* que se reemplaza, use el comando `istioctl replace` como sigue:

```console
istioctl replace -f istio/step-2a-update-voting-app-virtualservice.yaml --namespace voting
```

En la siguiente salida de ejemplo se muestra que el servicio virtual de Istio se ha actualizado correctamente:

```
Updated config virtual-service/voting/voting-app to revision 141902
```

A continuación, use el comando `istioctl create` para agregar la nueva directiva y también las nuevas reglas de destino y los servicios virtuales para todos los demás componentes.

* La directiva tiene `peers.mtls.mode` establecido en `STRICT` para garantizar la aplicación del certificado Mutual TLS entre los servicios dentro del espacio de nombres *voting*.
* También establece `trafficPolicy.tls.mode` en `ISTIO_MUTUAL` en todas nuestras reglas de destino. Istio ofrece servicios con identidades seguras y protege las comunicaciones entre los servicios con certificados cliente y Mutual TLS que Istio administra de forma transparente.

```console
istioctl create -f istio/step-2b-add-routing-for-all-components.yaml --namespace voting
```

En la siguiente salida de ejemplo se muestra que la nueva directiva, las reglas de destino y los servicios virtuales se han creado correctamente:

```
Created config policy/voting/default to revision 142118
Created config destination-rule/voting/voting-app at revision 142119
Created config destination-rule/voting/voting-analytics at revision 142120
Created config virtual-service/voting/voting-analytics at revision 142121
Created config destination-rule/voting/voting-storage at revision 142122
Created config virtual-service/voting/voting-storage at revision 142123
```

Si vuelve a abrir la aplicación de votación de AKS en un explorador, solo la nueva versión *1.1* del componente *voting-analytics* se usa en el componente *voting-app*.

![La versión 1.1 del componente de análisis que se ejecuta en la aplicación de votación de AKS.](media/istio/update-app-01.png)

Puede visualizar con más facilidad que ahora solo se nos redirige a la versión *1.1* del componente *voting-analytics* como sigue. Recuerde usar la dirección IP de la puerta de enlace de entrada de Istio.

Puede visualizar que ahora solo se le redirige a la versión *1.1* del componente *voting-analytics* como sigue. Recuerde usar la dirección IP de su propia puerta de enlace de entrada de Istio:

```azurecli-interactive
INGRESS_IP=52.187.250.239
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

La salida de ejemplo siguiente muestra la parte pertinente del sitio web devuelto:

```
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Confirme que Istio usa los certificados Mutual TLS para proteger las comunicaciones entre cada uno de nuestros servicios. Los siguientes comandos comprueban la configuración TLS de cada uno de los servicios *voting-app*:

```console
istioctl authn tls-check voting-app.voting.svc.cluster.local
istioctl authn tls-check voting-analytics.voting.svc.cluster.local
istioctl authn tls-check voting-storage.voting.svc.cluster.local
```

La salida de ejemplo siguiente muestra que el certificado Mutual TLS se ha aplicado en cada servicio mediante la directiva y las reglas de destino:

```
HOST:PORT                                    STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-app.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-app/voting

HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting

HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
```

## <a name="roll-out-a-canary-release-of-the-application"></a>Lanzamiento de una versión de valor controlado de la aplicación

Vamos a implementar una versión nueva *2.0* de los componentes *voting-app*, *voting-analytics* y *voting-storage*. El nuevo componente *voting-storage* usa MySQL en lugar de Redis, y los componentes *voting-app* y *voting-analytics* se actualizan para permitirles usar este nuevo componente *voting-storage*.

El componente *voting-app* ahora admite la funcionalidad de marca de características. Esta marca de características permite probar la funcionalidad de la versión de valor controlado de Istio para un subconjunto de usuarios.

En el diagrama siguiente se muestra lo que se ejecuta al final de esta sección.

* La versión *1.0* del componente *voting-app*, la versión *1.1* del componente *voting-analytics* y la versión *1.0* del componente *voting-storage* pueden comunicarse entre sí.
* La versión *2.0* del componente *voting-app*, la versión *2.0* del componente *voting-analytics* y la versión *2.0* del componente *voting-storage* pueden comunicarse entre sí.
* La versión *2.0* del componente *voting-app* solo es accesible para los usuarios que tengan un conjunto específico de marcas de características. Este cambio se administra con una marca de características mediante una cookie.

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-03.png)

En primer lugar, actualice las reglas de destino y los servicios virtuales de Istio destinados a estos nuevos componentes. Estas actualizaciones garantizan que el tráfico no se enrute incorrectamente a los nuevos componentes y que los usuarios no tengan un acceso inesperado:

```console
istioctl replace -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

En la siguiente salida de ejemplo se muestra que las reglas de destino y los servicios virtuales se han actualizado correctamente:

```
Updated config destination-rule/voting/voting-app to revision 150930
Updated config virtual-service/voting/voting-app to revision 150931
Updated config destination-rule/voting/voting-analytics to revision 150937
Updated config virtual-service/voting/voting-analytics to revision 150939
Updated config destination-rule/voting/voting-storage to revision 150940
Updated config virtual-service/voting/voting-storage to revision 150941
```

A continuación, vamos a agregar los objetos de Kubernetes para los componentes de la nueva versión *2.0*. Actualice también el servicio *voting-storage* para incluir el puerto *3306* para MySQL:

```console
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

En la siguiente salida de ejemplo se muestra que los objetos de Kubernetes se han actualizado o creado correctamente:

```
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Espere hasta que todos los pods de la versión *2.0* estén en ejecución. Use el comando [kubectl get pods][kubectl-get] para ver todos los pods en el espacio de nombres *voting*:

```azurecli-interactive
kubectl get pods --namespace voting
```

Ahora debe tener la capacidad de alternar entre la versión *1.0* y la versión *2.0* (de valor controlado) de la aplicación de votación. El botón de alternancia de la marca de características en la parte inferior de la pantalla establece una cookie. El servicio virtual *voting-app* usa esta cookie para enrutar a los usuarios a la nueva versión *2.0*.

![Versión 1.0 de la aplicación de votación de AKS: la marca de características NO ESTÁ establecida.](media/istio/canary-release-01.png)

![Versión 2.0 de la aplicación de votación de AKS: la marca de características ESTÁ establecida.](media/istio/canary-release-02.png)

Los recuentos de votos son diferentes entre las versiones de la aplicación. Esta diferencia resalta que se están usando dos back-ends de almacenamiento distintos.

## <a name="finalize-the-rollout"></a>Finalización del lanzamiento

Una vez que haya probado correctamente la versión de valor controlado, actualice el servicio virtual *voting-app* para enrutar todo el tráfico a la versión *2.0* del componente *voting-app*. A continuación, todos los usuarios verán la versión *2.0* de la aplicación, con independencia de que la marca de características esté establecida o no:

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-04.png)

Actualice todas las reglas de destino para quitar las versiones de los componentes que ya no desea que estén activas. A continuación, actualice todos los servicios virtuales para que dejen de hacer referencia a dichas versiones.

Como ya no hay tráfico a ninguna de las versiones anteriores de los componentes, ahora puede eliminar con seguridad todas las implementaciones de dichos componentes.

![Enrutamiento y componentes de la aplicación de votación de AKS.](media/istio/components-and-routing-05.png)

Ya ha lanzado correctamente una versión nueva de la aplicación de votación de AKS.

## <a name="next-steps"></a>Pasos siguientes

Puede explorar escenarios adicionales con el [ejemplo de la aplicación Bookinfo en Istio][istio-bookinfo-example].

<!-- LINKS - external -->
[github-azure-sample]: https://github.com/Azure-Samples/aks-voting-app
[istio]: https://istio.io
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-requirements-pods-and-services]: https://istio.io/docs/setup/kubernetes/spec-requirements/
[istio-reference-gateway]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#Gateway
[istio-reference-policy]: https://istio.io/docs/reference/config/istio.authentication.v1alpha1/#Policy
[istio-reference-virtualservice]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#VirtualService
[istio-reference-destinationrule]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#DestinationRule
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-install]: ./istio-install.md
