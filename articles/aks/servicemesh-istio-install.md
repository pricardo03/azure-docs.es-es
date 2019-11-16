---
title: Instalación de Istio en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo instalar y usar Istio para crear una malla de servicio en un clúster de Azure Kubernetes Service (AKS).
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: 245ac3b1fd88b8d2430e9ddefef3562efd16e6d1
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73885378"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Instalación y uso de Istio en Azure Kubernetes Service (AKS)

[Istio][istio-github] es una malla de servicio de código abierto que proporciona un conjunto clave de funcionalidades en los microservicios de un clúster de Kubernetes. Estas características incluyen la administración del tráfico, la seguridad e identidad de servicio, el cumplimiento de directivas y la observación. Para más información sobre Istio, vea la documentación del sitio oficial [¿Qué es Istio?][istio-docs-concepts].

En este artículo se explica cómo instalar Istio. El binario de cliente `istioctl` de Istio se instala en el equipo cliente y, a continuación, los componentes de Istio se instalan en un clúster de Kubernetes en AKS.

> [!NOTE]
> Estas instrucciones hacen referencia a la versión `1.3.2` de Istio y utilizan al menos la versión `2.14.2` de Helm.
>
> El equipo de Istio ha probado las versiones de Istio `1.3.x` y las ha comparado con las versiones de Kubernetes `1.13`, `1.14`, `1.15`. Encontrará otras versiones de Istio en [GitHub: Istio > Releases][istio-github-releases] (GitHub: Istio > Versiones), información sobre cada una de las versiones en [Istio News][istio-release-notes] (Novedades de Istio) y las versiones compatibles de Kubernetes en [Istio General FAQ][istio-faq] (Istio: Preguntas frecuentes generales).

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Descargar e instalar el binario de cliente istioctl de Istio
> * Instalar Istio en AKS
> * Validar la instalación de Istio
> * Acceder a los complementos
> * Desinstalar Istio de AKS

## <a name="before-you-begin"></a>Antes de empezar

En los pasos detallados en este artículo se da por hecho que ha creado un clúster de AKS (Kubernetes `1.13` y versiones posteriores, con RBAC habilitado) y que ha establecido una conexión `kubectl` con el clúster. Si necesita ayuda con cualquiera de estos elementos, consulte el [inicio rápido de AKS][aks-quickstart].

Necesitará [Helm][helm] para seguir estas instrucciones e instalar Istio. Es recomendable que tenga la versión estable más reciente correctamente instalada y configurada en el clúster. Si necesita ayuda con la instalación de Helm, consulte las [instrucciones de instalación de Helm para AKS][helm-install]. Todos los pods de Istio también deben programarse para ejecutarse en nodos de Linux.

Asegúrese de leer la documentación de [rendimiento y escalabilidad de Istio](https://istio.io/docs/concepts/performance-and-scalability/) para comprender los requisitos de recursos adicionales para ejecutar Istio en el clúster de AKS. Los requisitos de memoria y núcleos varían en función de la carga de trabajo concreta. Elija el número de nodos y el tamaño de la máquina virtual adecuados para la instalación.

En este artículo, las instrucciones de instalación de Istio se dividen en varios pasos discretos. El resultado final presenta la misma estructura que las [instrucciones][istio-install-helm] de instalación oficiales de Istio.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/istio/install-client-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [MacOS - download and install client binary](includes/servicemesh/istio/install-client-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/istio/install-client-binary-windows.md)]

::: zone-end

## <a name="add-the-istio-helm-chart-repository"></a>Incorporación del repositorio de gráficos de Helm de Istio

Agregue el repositorio de gráficos de Helm para la versión de Istio. Asegúrese de ejecutar `helm repo update` para actualizar la información local del repositorio de gráficos.

```azurecli
helm repo add istio.io https://storage.googleapis.com/istio-release/releases/$ISTIO_VERSION/charts/
helm repo update
```

## <a name="install-the-istio-crds-on-aks"></a>Instalar los CRD de Istio en AKS

Istio usa [Definiciones de recursos personalizadas (CRD)][kubernetes-crd] para administrar su configuración del entorno en tiempo de ejecución. Primero debemos instalar los CRD de Istio, ya que los componentes de Istio dependen de ellos. Use Helm y el gráfico `istio-init` para instalar los CRD de Istio en el espacio de nombres `istio-system` en su clúster de AKS:

```azurecli
helm install istio.io/istio-init --name istio-init --namespace istio-system
```

Los [trabajos][kubernetes-jobs] se implementan como parte del `istio-init`gráfico de Helm para instalar los CRD. Estos trabajos deben tardar menos de 20 segundos en finalizar, según el entorno del clúster. Puede comprobar que los trabajos se hayan completado correctamente de la siguiente manera:

```azurecli
kubectl get jobs -n istio-system
```

En la siguiente salida de ejemplo se muestran los trabajos completados con éxito.

```console
NAME                      COMPLETIONS   DURATION   AGE
istio-init-crd-10-1.3.2   1/1           14s        14s
istio-init-crd-11-1.3.2   1/1           12s        14s
istio-init-crd-12-1.3.2   1/1           14s        14s
```

Ahora que hemos confirmado la finalización correcta de los trabajos, debemos comprobar que tenemos la cantidad correcta de CRD de Istio instalados. Puede comprobar que los 23 CRD de Istio se han instalado ejecutando el siguiente comando. El comando debe devolver el número `23`.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - check CRD count](includes/servicemesh/istio/install-check-crd-count-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - check CRD count](includes/servicemesh/istio/install-check-crd-count-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - check CRD count](includes/servicemesh/istio/install-check-crd-count-powershell.md)]

::: zone-end

Si ha llegado a este punto, eso significa que ha instalado correctamente los CRD de Istio. Ahora vaya a la siguiente sección para [instalar los componentes de Istio en AKS](#install-the-istio-components-on-aks).

## <a name="install-the-istio-components-on-aks"></a>Instalar los componentes de Istio en AKS

Instalaremos [Grafana][grafana] y [Kiali][kiali] como parte de nuestra instalación de Istio. Grafana proporciona paneles de análisis y supervisión, y Kiali proporciona un panel de observación de la malla de servicio. En la configuración, cada uno de estos componentes requiere credenciales que deben proporcionarse como un [secreto][kubernetes-secrets].

Antes de que podamos instalar los componentes de Istio, debemos crear los secretos para Grafana y Kiali. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - create secrets for Grafana and Kiali](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash check for CRDs](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell check for CRDs](includes/servicemesh/istio/install-create-secrets-powershell.md)]

::: zone-end

### <a name="install-istio-components"></a>Instalar componentes de Istio

Ahora que hemos creado correctamente los secretos de Grafana y Kiali en nuestro clúster de AKS, es hora de instalar los componentes de Istio. Use Helm y el gráfico `istio` para instalar los componentes de Istio en el espacio de nombres `istio-system` en su clúster de AKS. 

> [!NOTE]
> **Opciones de instalación**
> 
> Vamos a usar las siguientes opciones como parte de nuestra instalación:
> - `global.controlPlaneSecurityEnabled=true`: TLS mutuo habilitado para el plano de control.
> - `global.mtls.enabled=true`: requiere que todas las comunicaciones entre servicios tengan mTLS
> - `grafana.enabled=true`: permite habilitar la implementación de Grafana para realizar análisis y paneles de supervisión.
> - `grafana.security.enabled=true`: permite habilitar la autenticación de Grafana.
> - `tracing.enabled=true`: permite habilitar la implementación de Jaeger para realizar el seguimiento.
> - `kiali.enabled=true`: permite habilitar la implementación de Kiali para un panel de observación de la malla de servicios.
>
> **Selectores de nodos**
>
> Actualmente, Istio debe programarse para su ejecución en nodos de Linux. Si tiene nodos de Windows Server en el clúster, debe asegurarse de que los pods de Istio solo se programan para ejecutarse en nodos de Linux. Usaremos [selectores de nodo][kubernetes-node-selectors] para garantizar que los pods se programan en los nodos correctos.

> [!CAUTION]
> Las características [SDS (servicio de detección de secretos)][istio-feature-sds] y [CNI][istio-feature-cni] de Istio se encuentran actualmente en fase [alfa][istio-feature-stages], por lo que debe considerarse su habilitación. Además, la función de Kubernetes [Service Account Token Volume Projection][kubernetes-feature-sa-projected-volume] (un requisito de SDS) no está habilitada en las versiones actuales de AKS.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/istio/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/istio/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - install Istio components](includes/servicemesh/istio/install-components-powershell.md)]

::: zone-end

El gráfico de Helm `istio` implementa un gran número de objetos. Puede ver la lista de la salida del comando `helm install` anterior. La implementación de los componentes de Istio debe tardar menos de dos minutos en finalizar, según el entorno del clúster.

En este punto, ya tendrá implementado Istio en su clúster de AKS. Para asegurarse de que ha implementado correctamente Istio, vaya a la siguiente sección para [validar la instalación de Istio](#validate-the-istio-installation).

## <a name="validate-the-istio-installation"></a>Validar la instalación de Istio

En primer lugar, confirme que se han creado los servicios esperados. Use el comando [kubectl get svc][kubectl-get] para ver los servicios en ejecución. Consulte el espacio de nombres `istio-system` donde se instalaron los componentes de Istio mediante el gráfico de Helm `istio`:

```console
kubectl get svc --namespace istio-system --output wide
```

La salida del ejemplo siguiente muestra los servicios que ahora deben estar en ejecución:

- `istio-*` servicios
- Servicios de seguimiento de complementos `jaeger-*`, `tracing` y `zipkin`
- Servicio de métricas de complementos `prometheus`
- Servicio de panel de supervisión y análisis de complementos `grafana`
- Servicio de panel de malla de servicio de complementos `kiali`

Si `istio-ingressgateway` muestra una dirección IP externa de `<pending>`, espere unos minutos hasta que las redes de Azure hayan asignado una dirección IP.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                                      AGE   SELECTOR
grafana                  ClusterIP      10.0.164.244   <none>           3000/TCP                                                                                                                                     53s   app=grafana
istio-citadel            ClusterIP      10.0.49.16     <none>           8060/TCP,15014/TCP                                                                                                                           53s   istio=citadel
istio-galley             ClusterIP      10.0.175.173   <none>           443/TCP,15014/TCP,9901/TCP                                                                                                                   53s   istio=galley
istio-ingressgateway     LoadBalancer   10.0.226.151   20.188.221.111   15020:31128/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:30817/TCP,15030:30436/TCP,15031:32485/TCP,15032:30980/TCP,15443:30124/TCP   53s   app=istio-ingressgateway,istio=ingressgateway,release=istio
istio-pilot              ClusterIP      10.0.102.158   <none>           15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       53s   istio=pilot
istio-policy             ClusterIP      10.0.234.53    <none>           9091/TCP,15004/TCP,15014/TCP                                                                                                                 53s   istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.216.8     <none>           443/TCP,15014/TCP                                                                                                                            53s   istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.154.215   <none>           9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       53s   istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                                   52s   app=jaeger
jaeger-collector         ClusterIP      10.0.26.109    <none>           14267/TCP,14268/TCP                                                                                                                          52s   app=jaeger
jaeger-query             ClusterIP      10.0.70.55     <none>           16686/TCP                                                                                                                                    52s   app=jaeger
kiali                    ClusterIP      10.0.36.206    <none>           20001/TCP                                                                                                                                    53s   app=kiali
prometheus               ClusterIP      10.0.236.99    <none>           9090/TCP                                                                                                                                     53s   app=prometheus
tracing                  ClusterIP      10.0.83.152    <none>           80/TCP                                                                                                                                       52s   app=jaeger
zipkin                   ClusterIP      10.0.25.86     <none>           9411/TCP                                                                                                                                     52s   app=jaeger
```

A continuación, confirme que se han creado los pods necesarios. Use el comando [kubectl get pods][kubectl-get] y vuelva a consultar el espacio de nombres `istio-system`:

```console
kubectl get pods --namespace istio-system
```

La salida de ejemplo siguiente muestra los pods que se ejecutan:

- los pods `istio-*`
- el pod de métricas de complementos `prometheus-*`
- pod del panel de supervisión y análisis de complementos `grafana-*`
- el pod del panel de malla de servicio de complementos `kiali`

```console
NAME                                     READY   STATUS      RESTARTS   AGE
grafana-7c48555456-msl7b                 1/1     Running     0          88s
istio-citadel-566fc66db7-m8wgl           1/1     Running     0          87s
istio-galley-5746db8d56-pl5gg            1/1     Running     0          88s
istio-ingressgateway-6c94f7c9bf-f5lt5    1/1     Running     0          88s
istio-init-crd-10-1.3.2-xw9g2            0/1     Completed   0          92m
istio-init-crd-11-1.3.2-54rz8            0/1     Completed   0          92m
istio-init-crd-12-1.3.2-789qj            0/1     Completed   0          92m
istio-pilot-6748968b6d-rvdfx             2/2     Running     0          87s
istio-policy-7576bbbcf7-2stft            2/2     Running     0          87s
istio-sidecar-injector-76d79d494-7jk9n   1/1     Running     0          87s
istio-telemetry-74b7bf676d-tfrcl         2/2     Running     0          88s
istio-tracing-655d9588bc-d2htg           1/1     Running     0          86s
kiali-65d55bcfb8-tqrfk                   1/1     Running     0          88s
prometheus-846f9849bd-br8kp              1/1     Running     0          87s
```

Debe haber tres pods `istio-init-crd-*` con un estado `Completed`. Estos pods se encargaron de ejecutar los trabajos que crearon los CRD en un paso anterior. El resto de pods deben mostrar un estado de `Running`. Si los pods no tienen estos estados, espere un minuto o dos hasta que los tengan. Si algún pod notifica un problema, use el comando [kubectl describe pod][kubectl-describe] para revisar su salida y su estado.

## <a name="accessing-the-add-ons"></a>Acceso a complementos

En los pasos de instalación anteriores, Istio instaló una serie de complementos que ofrecen funcionalidad adicional. Las aplicaciones web de los complementos **no** se exponen de forma pública a través de una dirección IP externa. 

Para acceder a las interfaces de usuario de los complementos, use el comando `istioctl dashboard`. Este comando utiliza [kubectl port-forward][kubectl-port-forward] y un puerto aleatorio para crear una conexión segura entre la máquina cliente y el pod correspondiente en el clúster de AKS. A continuación, abre automáticamente la aplicación web del complemento en el explorador predeterminado.

Asimismo, agregamos una capa adicional de seguridad para Grafana y Kiali especificando sus respectivas credenciales anteriormente en este artículo.

### <a name="grafana"></a>Grafana

Los paneles de análisis y supervisión para Istio los proporciona [Grafana][grafana]. Recuerde usar las credenciales que creó a través del secreto de Grafana cuando se le solicite. Para abrir el panel de Grafana de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard grafana
```

### <a name="prometheus"></a>Prometheus

Las métricas para Istio las proporciona [Prometheus][prometheus]. Para abrir el panel de Prometheus de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard prometheus
```

### <a name="jaeger"></a>Jaeger

El seguimiento de Istio lo proporciona [Jaeger][jaeger]. Para abrir el panel de Jaeger de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard jaeger
```

### <a name="kiali"></a>Kiali

[Kiali][kiali] proporciona un panel de observación de malla de servicio. Recuerde usar las credenciales que creó a través del secreto de Kiali cuando se le solicite. Para abrir el panel de Kiali de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard kiali
```

### <a name="envoy"></a>Envoy

Hay disponible una interfaz simple a los proxies de [Envoy][envoy]. Proporciona información de configuración y métricas de un proxy de Envoy que se ejecuta en un pod especificado. Para abrir la interfaz de Envoy de forma segura, ejecute el siguiente comando:

```console
istioctl dashboard envoy <pod-name>.<namespace>
```

## <a name="uninstall-istio-from-aks"></a>Desinstalar Istio de AKS

> [!WARNING]
> Eliminar Istio de un sistema en ejecución puede provocar problemas de tráfico entre sus servicios. Asegúrese de haber hecho los aprovisionamientos necesarios para que su sistema funcione correctamente sin Istio antes de continuar.

### <a name="remove-istio-components-and-namespace"></a>Quitar los componentes y el espacio de nombres de Istio

Para quitar Istio de su clúster de AKS, use los siguientes comandos. Los comandos `helm delete` quitarán los gráficos `istio` y `istio-init`, y el comando `kubectl delete namespace` quitará el espacio de nombres `istio-system`.

```azurecli
helm delete --purge istio
helm delete --purge istio-init
kubectl delete namespace istio-system
```

### <a name="remove-istio-crds-and-secrets"></a>Eliminación de los CRD y los secretos de Istio

Los comandos anteriores eliminan todos los componentes y el espacio de nombres de Istio, pero aún quedan los CRD y los secretos de Istio. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-powershell.md)]

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

En la siguiente documentación se describe cómo usar Istio para proporcionar enrutamiento inteligente para implantar un lanzamiento controlado:

> [!div class="nextstepaction"]
> [Escenario de enrutamiento inteligente con Istio en AKS][istio-scenario-routing]

Para explorar más opciones de instalación y configuración para Istio, consulte los siguientes artículos oficiales de Istio:

- [Istio: guía de instalación de Helm][istio-install-helm]
- [Istio: opciones de instalación de Helm][istio-install-helm-options]

También puede seguir otros escenarios mediante:

- [Ejemplo de la aplicación BookInfo de Istio][istio-bookinfo-example]

Para aprender a supervisar la aplicación de AKS con Application Insights e Istio, consulte la siguiente documentación de Azure Monitor:

- [Supervisión de aplicaciones sin instrumentación para aplicaciones hospedadas de Kubernetes][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-faq]: https://istio.io/faq/general/
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/news/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-helm]: https://istio.io/docs/setup/install/helm/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/

[istio-feature-stages]: https://istio.io/about/feature-stages/
[istio-feature-sds]: https://istio.io/docs/tasks/security/auth-sds/
[istio-feature-cni]: https://istio.io/docs/setup/additional-setup/cni/

[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-feature-sa-projected-volume]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection
[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-jobs]: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-node-selectors]: https://docs.microsoft.com/azure/aks/concepts-clusters-workloads#node-selectors
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[envoy]: https://www.envoyproxy.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./servicemesh-istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
