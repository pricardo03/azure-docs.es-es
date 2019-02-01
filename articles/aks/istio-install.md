---
title: Instalación de Istio en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo instalar y usar Istio para crear una malla de servicio en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 12/3/2018
ms.author: pabouwer
ms.openlocfilehash: ecabb139b79494e8a62687a5634d9865d578c5c6
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104481"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Instalación y uso de Istio en Azure Kubernetes Service (AKS)

[Istio][istio-github] es una malla de servicio de código abierto que proporciona un conjunto clave de funcionalidades en los microservicios de un clúster de Kubernetes. Estas características incluyen la administración del tráfico, la seguridad e identidad de servicio, el cumplimiento de directivas y la observación. Para más información sobre Istio, vea la documentación del sitio oficial [What is Istio?][istio-docs-concepts] (¿Qué es Istio?).

En este artículo se explica cómo instalar Istio. El binario de cliente `istioctl` de Istio se instala en el equipo cliente y, a continuación, los componentes de Istio se instalan en un clúster de Kubernetes en AKS. Estas instrucciones son aplicables a la versión *1.0.4* de Istio. Puede encontrar versiones adicionales de Istio en [GitHub: versiones de Istio][istio-github-releases].

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Descargar Istio
> * Instalar el binario de cliente de Istio
> * Instalar los componentes de Kubernetes de Istio
> * Validación de la instalación

## <a name="before-you-begin"></a>Antes de empezar

En los pasos detallados en este artículo se da por hecho que ha creado un clúster de AKS (Kubernetes 1.10 y versiones posteriores, con RBAC habilitado) y que ha establecido una conexión `kubectl` con el clúster. Si necesita ayuda con cualquiera de estos elementos, vea el [inicio rápido de AKS][aks-quickstart].

Para instalar Istio, es necesario que [Helm][helm], versión *2.10.0* o alguna versión posterior estén instaladas y configuradas correctamente en el clúster. Si necesita ayuda con la instalación de Helm, consulte las [instrucciones de instalación de Helm para AKS][helm-install]. Si no tiene instalada al menos la versión *2.10.0* de Helm, actualícela o consulte la [guía de instalación de Istio con Helm][istio-install-helm] para conocer otras opciones de instalación.

En este artículo, las instrucciones de instalación de Istio se dividen en varios pasos discretos. Se describe cada uno de estos pasos para que pueda obtener información sobre cómo instalar Istio y cómo Istio funciona con Kubernetes. El resultado final presenta la misma estructura que las [instrucciones][istio-install-k8s-quickstart] de instalación oficiales de Istio.

## <a name="download-istio"></a>Descargar Istio

En primer lugar, descargue y extraiga la versión más reciente de Istio. Los pasos varían ligeramente para un shell con Bash en MacOS, Linux o un subsistema de Windows para Linux y un shell de PowerShell. Elija alguno de los siguientes pasos de instalación para su entorno preferido:

* [Bash en MacOS, Linux o un subsistema de Windows para Linux](#bash)
* [PowerShell](#powershell)

### <a name="bash"></a>Bash

En MacOS, utilice `curl` para descargar la versión más reciente de Istio y, a continuación, extraiga `tar` como sigue:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.0.4

# MacOS
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

En Linux o en el subsistema de Windows para Linux, use `curl` para descargar la versión más reciente de Istio y, a continuación, extraiga `tar` como sigue:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.0.4

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

### <a name="powershell"></a>PowerShell

En PowerShell, use [Invoke-WebRequest][Invoke-WebRequest] para descargar la versión más reciente de Istio y, a continuación, extraiga [Expand-Archive][Expand-Archive] como se indica a continuación:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.0.4"

# Windows
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

## <a name="install-the-istio-client-binary"></a>Instalar el binario de cliente de Istio

El binario de cliente `istioctl` se ejecuta en el equipo cliente y permite administrar las directivas y reglas de enrutamiento de Istio. Una vez más, los pasos de instalación varían ligeramente entre los distintos sistemas operativos cliente. Elija uno de los siguientes pasos de instalación para su entorno preferido.

> [!IMPORTANT]
> Ejecute todos los pasos restantes desde la carpeta de nivel superior de la versión de Istio que descargó y extrajo en la sección anterior.

### <a name="macos"></a>MacOS

Para instalar el binario de cliente `istioctl` de Istio en un shell basado en Bash en MacOS, use los siguientes comandos. Estos comandos copian el binario de cliente `istioctl` en la ubicación del programa de usuario estándar en su `PATH`.

```bash
cd istio-$ISTIO_VERSION
chmod +x ./bin/istioctl
sudo mv ./bin/istioctl /usr/local/bin/istioctl
```

Si desea completar la línea de comandos del binario de cliente `istioctl` de Istio, realice la siguiente configuración:

```bash
# Generate the bash completion file and source it in your current shell
istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Ahora vaya a la sección [Instalar los componentes de Kubernetes de Istio](#install-the-istio-kubernetes-components).

### <a name="linux-or-windows-subsystem-for-linux"></a>Linux o Subsistema de Windows para Linux

Use los comandos siguientes para instalar el binario de cliente `istioctl` de Istio en un shell basado en Bash en Linux o en el [Subsistema de Windows para Linux][install-wsl]. Estos comandos copian el binario de cliente `istioctl` en la ubicación del programa de usuario estándar en su `PATH`.

```bash
cd istio-$ISTIO_VERSION
chmod +x ./bin/istioctl
sudo mv ./bin/istioctl /usr/local/bin/istioctl
```

Si desea completar la línea de comandos del binario de cliente `istioctl` de Istio, realice la siguiente configuración:

```bash
# Generate the bash completion file and source it in your current shell
istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Ahora vaya a la sección [Instalar los componentes de Kubernetes de Istio](#install-the-istio-kubernetes-components).

### <a name="windows"></a> Windows

Para instalar el binario de cliente `istioctl` de Istio en un shell basado en PowerShell en Windows, use los siguientes comandos. Estos comandos copian el binario de cliente `istioctl` en una nueva ubicación del programa de usuario y hacen que esté disponible mediante su `PATH`.

```powershell
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:/Program Files/Istio"
mv ./bin/istioctl.exe "C:/Program Files/Istio/"
$PATH = [environment]::GetEnvironmentVariable("PATH", "User")
[environment]::SetEnvironmentVariable("PATH", $PATH + "; C:\Program Files\Istio\", "User")
```

## <a name="install-the-istio-kubernetes-components"></a>Instalar los componentes de Kubernetes de Istio

Para instalar los componentes de Istio en el clúster de AKS, use Helm. Instale los recursos de Istio en el espacio de nombres `istio-system` y habilite opciones adicionales de seguridad y supervisión como se indica a continuación:

```azurecli
helm install install/kubernetes/helm/istio --name istio --namespace istio-system \
  --set global.controlPlaneSecurityEnabled=true \
  --set grafana.enabled=true \
  --set tracing.enabled=true \
  --set kiali.enabled=true
```

El gráfico de Helm implementa un gran número de objetos. La implementación puede tardar de dos a tres minutos en completarse, en función del entorno del clúster.

## <a name="validate-the-installation"></a>Validación de la instalación

Para asegurarse de que tiene una implementación correcta de Istio, vamos a validar la instalación.

En primer lugar, confirme que se han creado los servicios esperados. Use el comando [kubectl get svc][kubectl-get] para ver los servicios en ejecución. Consulte el espacio de nombres *istio-system*, donde se instalaron los componentes de Istio y de complementos mediante el gráfico de Helm:

```console
kubectl get svc --namespace istio-system --output wide
```

La salida del ejemplo siguiente muestra los servicios que ahora deben estar en ejecución:

- Servicios *istio-**
- Servicios de seguimiento de complementos *jaeger-**, *tracing* y *zipkin*
- Servicio de métricas de complementos *prometheus*
- Servicio de panel de supervisión y análisis de complementos *grafana*
- Servicio de panel de malla de servicio de complementos *kiali*

Si `istio-ingressgateway` muestra una dirección IP externa de `<pending>`, espere unos minutos hasta que las redes de Azure hayan asignado una dirección IP.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                   AGE       SELECTOR
grafana                  ClusterIP      10.0.26.60     <none>           3000/TCP                                                                                                                  3m        app=grafana
istio-citadel            ClusterIP      10.0.88.87     <none>           8060/TCP,9093/TCP                                                                                                         3m        istio=citadel
istio-egressgateway      ClusterIP      10.0.115.115   <none>           80/TCP,443/TCP                                                                                                            3m        app=istio-egressgateway,istio=egressgateway
istio-galley             ClusterIP      10.0.104.183   <none>           443/TCP,9093/TCP                                                                                                          3m        istio=galley
istio-ingressgateway     LoadBalancer   10.0.12.216    52.187.250.239   80:31380/TCP,443:31390/TCP,31400:31400/TCP,15011:30469/TCP,8060:31999/TCP,853:31235/TCP,15030:32000/TCP,15031:30293/TCP   3m        app=istio-ingressgateway,istio=ingressgateway
istio-pilot              ClusterIP      10.0.38.134    <none>           15010/TCP,15011/TCP,8080/TCP,9093/TCP                                                                                     3m        istio=pilot
istio-policy             ClusterIP      10.0.253.81    <none>           9091/TCP,15004/TCP,9093/TCP                                                                                               3m        istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.181.186   <none>           443/TCP                                                                                                                   3m        istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.177.113   <none>           9091/TCP,15004/TCP,9093/TCP,42422/TCP                                                                                     3m        istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                3m        app=jaeger
jaeger-collector         ClusterIP      10.0.112.81    <none>           14267/TCP,14268/TCP                                                                                                       3m        app=jaeger
jaeger-query             ClusterIP      10.0.179.193   <none>           16686/TCP                                                                                                                 3m        app=jaeger
kiali                    ClusterIP      10.0.211.63    <none>           20001/TCP                                                                                                                 3m        app=kiali
prometheus               ClusterIP      10.0.70.113    <none>           9090/TCP                                                                                                                  3m        app=prometheus
tracing                  ClusterIP      10.0.139.121   <none>           80/TCP                                                                                                                    3m        app=jaeger
zipkin                   ClusterIP      10.0.60.155    <none>           9411/TCP                                                                                                                  3m        app=jaeger
```

A continuación, confirme que se han creado los pods necesarios. Use el comando [kubectl get pods][kubectl-get] y vuelva a consultar el espacio de nombres *istio-system*:

```console
kubectl get pods --namespace istio-system
```

La salida de ejemplo siguiente muestra los pods que se ejecutan:

- los pods de *istio-**
- el pod de métricas de complementos *prometheus-**
- el pod de panel de supervisión y análisis de complementos *grafana-**
- el pod de panel de malla de servicio de complementos *kiali*

```console
NAME                                     READY     STATUS      RESTARTS   AGE
grafana-59b787b9b-cr6d7                  1/1       Running     0          6m
istio-citadel-78df8c67d9-9lfpf           1/1       Running     0          6m
istio-egressgateway-6b96cd7f5-k848h      1/1       Running     0          6m
istio-galley-58f566cb66-8mhbv            1/1       Running     0          6m
istio-ingressgateway-6cbbf596f6-6jz8g    1/1       Running     0          6m
istio-pilot-8449d555fc-sl6kp             2/2       Running     0          6m
istio-policy-6b99d88bc5-55s52            2/2       Running     0          6m
istio-sidecar-injector-b88dfb954-8m86s   1/1       Running     0          6m
istio-telemetry-675cb4cb9d-8s7wd         2/2       Running     0          6m
istio-tracing-7596597bd7-sbnt9           1/1       Running     0          6m
kiali-5fbd6ffb-4qcxw                     1/1       Running     0          6m
prometheus-76db5fddd5-2tkxs              1/1       Running     0          6m
```

Todos los pods muestran el estado de `Running`. Si los pods no tienen estos estados, espere un minuto o dos hasta que los tengan. Si algún pod notifica un problema, use el comando [kubectl describe pod][kubectl-describe] para revisar su salida y su estado.

## <a name="accessing-the-add-ons"></a>Acceso a complementos

Se instalaron una serie de complementos en Istio en los pasos de instalación anteriores que ofrecen una funcionalidad adicional. Las interfaces de usuario de los complementos no se exponen de forma pública a través de una dirección IP externa. Para acceder a las interfaces de usuario de los complementos, use el comando [kubectl port-forward][kubectl-port-forward]. Este comando crea una conexión segura entre un puerto local en el equipo cliente y el pod relevante en el clúster de AKS.

### <a name="grafana"></a>Grafana

Los paneles de análisis y supervisión para Istio los proporciona [Grafana][grafana]. Reenvíe el puerto local *3000* del equipo cliente al puerto *3000* del pod que ejecuta Grafana en el clúster de AKS:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

La salida del ejemplo siguiente muestra el reenvío de puerto configurado para Grafana:

```console
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

Ahora puede acceder a Grafana en la siguiente dirección URL en el equipo cliente: [http://localhost:3000](http://localhost:3000).

### <a name="prometheus"></a>Prometheus

[Prometheus][prometheus] proporciona el explorador de expresiones para las métricas. Reenvíe el puerto local *9090* del equipo cliente al puerto *9090* del pod que ejecuta Prometheus en el clúster de AKS:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090
```

La salida del ejemplo siguiente muestra el reenvío de puerto configurado para Prometheus:

```console
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```

Ahora puede acceder al explorador de expresiones de Prometheus en la siguiente dirección URL en el equipo cliente: [http://localhost:9090](http://localhost:9090).

### <a name="jaeger"></a>Jaeger

La interfaz de usuario de seguimiento la proporciona [Jaeger][jaeger]. Reenvíe el puerto local *16686* del equipo cliente al puerto *16686* del pod que ejecuta Jaeger en el clúster de AKS:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686
```

La salida del ejemplo siguiente muestra el reenvío de puerto configurado para Jaeger:

```console
Forwarding from 127.0.0.1:16686 -> 16686
Forwarding from [::1]:16686 -> 16686
```

Ahora puede acceder a la interfaz de usuario de seguimiento de Jaeger en la siguiente dirección URL en el equipo cliente: [http://localhost:16686](http://localhost:16686).

### <a name="kiali"></a>Kiali

[Kiali][kiali] proporciona un panel de observación de malla de servicio. Reenvíe el puerto local *20001* del equipo cliente al puerto *20001* del pod que ejecuta Kiali en el clúster de AKS:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

La salida del ejemplo siguiente muestra el reenvío de puerto configurado para Kiali:

```console
Forwarding from 127.0.0.1:20001 -> 20001
Forwarding from [::1]:20001 -> 20001
```

Ahora puede acceder al panel de observación de malla de servicio de Kiali en la siguiente dirección URL en el equipo cliente: [http://localhost:20001](http://localhost:20001).

El nombre de usuario y la contraseña predeterminados del panel de Kiali es *username:admin/password:admin*. Estas credenciales se pueden configurar con los valores de Helm *kiali.dashboard.username* y *kiali.dashboard.passphrase*.

## <a name="next-steps"></a>Pasos siguientes

Para saber cómo puede usar Istio para proporcionar un enrutamiento inteligente entre varias versiones de la aplicación y lanzar una versión de valor controlado, consulte la siguiente documentación:

> [!div class="nextstepaction"]
> [Escenario de enrutamiento inteligente con Istio en AKS][istio-scenario-routing]

Para explorar más opciones de instalación y configuración para Istio, consulte los siguientes artículos oficiales de Istio:

- [Istio: guía de inicio rápido para la instalación de Kubernetes][istio-install-k8s-quickstart]
- [Istio: guía de instalación de Helm][istio-install-helm]
- [Istio: opciones de instalación de Helm][istio-install-helm-options]

Puede consultar otros escenarios con el [ejemplo de la aplicación Bookinfo en Istio][istio-bookinfo-example].

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-k8s-quickstart]: https://istio.io/docs/setup/kubernetes/quick-start/
[istio-install-helm]: https://istio.io/docs/setup/kubernetes/helm-install/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10
[kubernetes-crd]: https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
[Invoke-WebRequest]: /powershell/module/microsoft.powershell.utility/invoke-webrequest
[Expand-Archive]: /powershell/module/Microsoft.PowerShell.Archive/Expand-Archive
