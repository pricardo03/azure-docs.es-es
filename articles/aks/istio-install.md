---
title: Instalación de Istio en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo instalar y usar Istio para crear una malla de servicio en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 04/19/2019
ms.author: pabouwer
ms.openlocfilehash: 9d973cb2ac210e912d93941a2f81889557379f43
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625977"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Instalación y uso de Istio en Azure Kubernetes Service (AKS)

Documentación is an open-source service mesh that provides a key set of functionality across the microservices in a Kubernetes cluster. These features include traffic management, service identity and security, policy enforcement, and observability. For more information about Istio, see the official [What is Istio?][istio-docs-concepts] de [Istio][istio-github].

En este artículo se explica cómo instalar Istio. El binario de cliente `istioctl` de Istio se instala en el equipo cliente y, a continuación, los componentes de Istio se instalan en un clúster de Kubernetes en AKS.

> [!NOTE]
> Estas instrucciones son aplicables a la versión `1.1.3` de Istio.
>
> El equipo de Istio ha probado las versiones de Istio `1.1.x` y las ha comparado con las versiones de Kubernetes `1.11`, `1.12`, `1.13`. Puede encontrar versiones adicionales de Istio en [GitHub: versiones de Istio][istio-github-releases] and information about each of the releases at [Istio - Release Notes][istio-release-notes].

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Descargar Istio
> * Instalar el binario de cliente istioctl de Istio
> * Instalar los CRD de Istio en AKS
> * Instalar los componentes de Istio en AKS
> * Validar la instalación de Istio.
> * Acceso a complementos
> * Desinstalar Istio de AKS

## <a name="before-you-begin"></a>Antes de empezar

En los pasos detallados en este artículo se da por hecho que ha creado un clúster de AKS (Kubernetes `1.11` y versiones posteriores, con RBAC habilitado) y que ha establecido una conexión `kubectl` con el clúster. Si necesita ayuda con cualquiera de estos elementos, consulte el [inicio rápido de AKS][aks-quickstart].

Necesitará [Helm][helm] para seguir estas instrucciones e instalar Istio. Es recomendable que tenga la versión `2.12.2` o posterior instalada y configurada correctamente en su clúster. Si necesita ayuda con la instalación de Helm, consulte las [instrucciones de instalación de Helm para AKS][helm-install]. Todos los pods de Istio también deben programarse para ejecutarse en nodos de Linux.

En este artículo, las instrucciones de instalación de Istio se dividen en varios pasos discretos. El resultado final presenta la misma estructura que las [instrucciones][istio-install-helm] de instalación oficiales de Istio.

## <a name="download-istio"></a>Descargar Istio

En primer lugar, descargue y extraiga la versión más reciente de Istio. Los pasos varían ligeramente para un shell con Bash en MacOS, Linux o un subsistema de Windows para Linux y un shell de PowerShell. Elija uno de los siguientes pasos de instalación para su entorno preferido.

* [Bash en MacOS, Linux o un subsistema de Windows para Linux](#bash)
* [PowerShell](#powershell)

### <a name="bash"></a>Bash

En MacOS, utilice `curl` para descargar la versión más reciente de Istio y, a continuación, extraiga `tar` como sigue:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.1.3

# MacOS
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

En Linux o en el subsistema de Windows para Linux, use `curl` para descargar la versión más reciente de Istio y, a continuación, extraiga `tar` como sigue:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.1.3

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

A continuación, vaya a la sección [Instalar el binario de cliente istioctl de Istio](#install-the-istio-istioctl-client-binary).

### <a name="powershell"></a>PowerShell

En PowerShell, use `Invoke-WebRequest` para descargar la versión más reciente de Istio y, a continuación, extraiga `Expand-Archive` tal como se indica:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.1.3"

# Windows
# Use TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

A continuación, vaya a la sección [Instalar el binario de cliente istioctl de Istio](#install-the-istio-istioctl-client-binary).

## <a name="install-the-istio-istioctl-client-binary"></a>Instalar el binario de cliente istioctl de Istio

> [!IMPORTANT]
> Asegúrese de ejecutar los pasos en esta sección, desde la carpeta de nivel superior de la versión de Istio que descargó y extrajo.

El binario de cliente `istioctl` se ejecuta en su equipo cliente y le permite interactuar con la malla de servicio de Istio. Los pasos de instalación varían ligeramente entre los distintos sistemas operativos cliente. Elija uno de los siguientes pasos de instalación para su entorno preferido:

* [macOS](#macos)
* [Linux o Subsistema de Windows para Linux](#linux-or-windows-subsystem-for-linux)
* [Windows](#windows)

### <a name="macos"></a>MacOS

Para instalar el binario de cliente `istioctl` de Istio en un shell basado en Bash en MacOS, use los siguientes comandos. Estos comandos copian el binario de cliente `istioctl` en la ubicación del programa de usuario estándar en su `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Si desea completar la línea de comandos del binario de cliente `istioctl` de Istio, realice la siguiente configuración:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Ahora vaya a la siguiente sección para [instalar los CRD de Istio en AKS](#install-the-istio-crds-on-aks).

### <a name="linux-or-windows-subsystem-for-linux"></a>Linux o Subsistema de Windows para Linux

Use los comandos siguientes para instalar el binario de cliente `istioctl` de Istio en un shell basado en Bash en Linux o en el [Subsistema de Windows para Linux][install-wsl]. Estos comandos copian el binario de cliente `istioctl` en la ubicación del programa de usuario estándar en su `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

Si desea completar la línea de comandos del binario de cliente `istioctl` de Istio, realice la siguiente configuración:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Ahora vaya a la siguiente sección para [instalar los CRD de Istio en AKS](#install-the-istio-crds-on-aks).

### <a name="windows"></a>Windows

Para instalar el binario de cliente `istioctl` de Istio en un shell basado en **PowerShell** en Windows, use los siguientes comandos. Estos comandos copian los archivos binarios del cliente `istioctl` en una carpeta de Istio y, luego, permiten que estén disponible de inmediato (en el shell actual) y de forma permanente (entre reinicios del shell) mediante `PATH`. No es necesario tener privilegios elevados (administrador) para ejecutar estos comandos y no hace falta reiniciar el shell.

```powershell
# Copy istioctl.exe to C:\Istio
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:\Istio"
Copy-Item -Path .\bin\istioctl.exe -Destination "C:\Istio\"

# Add C:\Istio to PATH. 
# Make the new PATH permanently available for the current User, and also immediately available in the current shell.
$PATH = [environment]::GetEnvironmentVariable("PATH", "User") + "; C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $PATH, "User") 
[environment]::SetEnvironmentVariable("PATH", $PATH)
```

Ahora vaya a la siguiente sección para [instalar los CRD de Istio en AKS](#install-the-istio-crds-on-aks).

## <a name="install-the-istio-crds-on-aks"></a>Instalar los CRD de Istio en AKS

> [!IMPORTANT]
> Asegúrese de ejecutar los pasos en esta sección, desde la carpeta de nivel superior de la versión de Istio que descargó y extrajo.

Istio usa [Definiciones de recursos personalizadas (CRD)][kubernetes-crd] para administrar su configuración del entorno en tiempo de ejecución. Primero debemos instalar los CRD de Istio, ya que los componentes de Istio dependen de ellos. Use Helm y el gráfico `istio-init` para instalar los CRD de Istio en el espacio de nombres `istio-system` en su clúster de AKS:

```azurecli
helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system
```

Los [trabajos][kubernetes-jobs] se implementan como parte del `istio-init`gráfico de Helm para instalar los CRD. Estos trabajos deben tardar entre 1 y 2 minutos en completarse, dependiendo de su entorno de clúster. Puede comprobar que los trabajos se hayan completado correctamente de la siguiente manera:

```azurecli
kubectl get jobs -n istio-system
```

En la siguiente salida de ejemplo se muestran los trabajos completados con éxito.

```console
NAME                COMPLETIONS   DURATION   AGE
istio-init-crd-10   1/1           16s        18s
istio-init-crd-11   1/1           15s        18s
```

Ahora que hemos confirmado la finalización correcta de los trabajos, debemos comprobar que tenemos la cantidad correcta de CRD de Istio instalados. Puede comprobar que los 53 CRD de Istio se hayan instalado ejecutando el comando adecuado para su entorno. El comando debe devolver el número `53`.

Bash

```bash
kubectl get crds | grep 'istio.io' | wc -l
```

PowerShell

```powershell
(kubectl get crds | Select-String -Pattern 'istio.io').Count
```

Si ha llegado a este punto, eso significa que ha instalado correctamente los CRD de Istio. Ahora vaya a la siguiente sección para [instalar los componentes de Istio en AKS](#install-the-istio-components-on-aks).

## <a name="install-the-istio-components-on-aks"></a>Instalar los componentes de Istio en AKS

> [!IMPORTANT]
> Asegúrese de ejecutar los pasos en esta sección, desde la carpeta de nivel superior de la versión de Istio que descargó y extrajo.

Instalaremos [Grafana][grafana] and [Kiali][kiali] como parte de nuestra instalación de Istio. Grafana proporciona paneles de análisis y supervisión, y Kiali proporciona un panel de observación de la malla de servicio. En la configuración, cada uno de estos componentes requiere credenciales que deben proporcionarse como un [Secreto][kubernetes-secrets].

Antes de que podamos instalar los componentes de Istio, debemos crear los secretos para Grafana y Kiali. Cree estos secretos ejecutando los comandos adecuados para su entorno.

### <a name="add-grafana-secret"></a>Agregar el secreto de Grafana

Reemplace el token `REPLACE_WITH_YOUR_SECURE_PASSWORD` con su contraseña y ejecute los siguientes comandos:

#### <a name="macos-linux"></a>MacOS, Linux

```bash
GRAFANA_USERNAME=$(echo -n "grafana" | base64)
GRAFANA_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE
EOF
```

#### <a name="windows"></a>Windows

```powershell
$GRAFANA_USERNAME=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("grafana"))
$GRAFANA_PASSPHRASE=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("REPLACE_WITH_YOUR_SECURE_PASSWORD"))

"apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE" | kubectl apply -f -
```

### <a name="add-kiali-secret"></a>Agregar el secreto de Kiali

Reemplace el token `REPLACE_WITH_YOUR_SECURE_PASSWORD` con su contraseña y ejecute los siguientes comandos:

#### <a name="macos-linux"></a>MacOS, Linux

```bash
KIALI_USERNAME=$(echo -n "kiali" | base64)
KIALI_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE
EOF
```

#### <a name="windows"></a>Windows

```powershell
$KIALI_USERNAME=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("kiali"))
$KIALI_PASSPHRASE=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("REPLACE_WITH_YOUR_SECURE_PASSWORD"))

"apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE" | kubectl apply -f -
```

### <a name="install-istio-components"></a>Instalar componentes de Istio

Ahora que hemos creado correctamente los secretos de Grafana y Kiali en nuestro clúster de AKS, es hora de instalar los componentes de Istio. Use Helm y el gráfico `istio` para instalar los componentes de Istio en el espacio de nombres `istio-system` en su clúster de AKS. Use los comandos adecuados para su entorno.

> [!NOTE]
> Vamos a usar las siguientes opciones como parte de nuestra instalación:
> - `global.controlPlaneSecurityEnabled=true`: TLS mutuo habilitado para el plano de control.
> - `mixer.adapters.useAdapterCRDs=false`: permite eliminar las inspecciones en los CRD del adaptador del mezclador, ya que están en desuso y esto mejorará el rendimiento.
> - `grafana.enabled=true`: permite habilitar la implementación de Grafana para realizar análisis y paneles de supervisión.
> - `grafana.security.enabled=true`: permite habilitar la autenticación de Grafana.
> - `tracing.enabled=true`: permite habilitar la implementación de Jaeger para realizar el seguimiento.
> - `kiali.enabled=true`: permite habilitar la implementación de Kiali para un panel de observación de la malla de servicios.

Bash

```bash
helm install install/kubernetes/helm/istio --name istio --namespace istio-system \
  --set global.controlPlaneSecurityEnabled=true \
  --set mixer.adapters.useAdapterCRDs=false \
  --set grafana.enabled=true --set grafana.security.enabled=true \
  --set tracing.enabled=true \
  --set kiali.enabled=true
```

PowerShell

```powershell
helm install install/kubernetes/helm/istio --name istio --namespace istio-system `
  --set global.controlPlaneSecurityEnabled=true `
  --set mixer.adapters.useAdapterCRDs=false `
  --set grafana.enabled=true --set grafana.security.enabled=true `
  --set tracing.enabled=true `
  --set kiali.enabled=true
```

El gráfico de Helm `istio` implementa un gran número de objetos. Puede ver la lista de la salida del comando `helm install` anterior. La implementación de los componentes de Istio puede tardar entre 4 y 5 minutos, según el entorno de su clúster.

> [!NOTE]
> Todos los pods de Istio también deben programarse para ejecutarse en nodos de Linux. Si tiene grupos de nodos de Windows Server además de grupos de nodos de Linux en su clúster, compruebe que todos los pods de Istio hayan sido programados para ejecutarse en nodos de Linux.

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
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                                                                                                                                      AGE       SELECTOR
grafana                  ClusterIP      10.0.81.182    <none>          3000/TCP                                                                                                                                     119s      app=grafana
istio-citadel            ClusterIP      10.0.96.33     <none>          8060/TCP,15014/TCP                                                                                                                           119s      istio=citadel
istio-galley             ClusterIP      10.0.237.158   <none>          443/TCP,15014/TCP,9901/TCP                                                                                                                   119s      istio=galley
istio-ingressgateway     LoadBalancer   10.0.154.12    20.188.211.19   15020:30603/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:31198/TCP,15030:30610/TCP,15031:30937/TCP,15032:31344/TCP,15443:31499/TCP   119s      app=istio-ingressgateway,istio=ingressgateway,release=istio
istio-pilot              ClusterIP      10.0.178.56    <none>          15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       119s      istio=pilot
istio-policy             ClusterIP      10.0.116.118   <none>          9091/TCP,15004/TCP,15014/TCP                                                                                                                 119s      istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.31.160    <none>          443/TCP                                                                                                                                      119s      istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.187.246   <none>          9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       119s      istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>          5775/UDP,6831/UDP,6832/UDP                                                                                                                   119s      app=jaeger
jaeger-collector         ClusterIP      10.0.116.63    <none>          14267/TCP,14268/TCP                                                                                                                          119s      app=jaeger
jaeger-query             ClusterIP      10.0.22.108    <none>          16686/TCP                                                                                                                                    119s      app=jaeger
kiali                    ClusterIP      10.0.142.50    <none>          20001/TCP                                                                                                                                    119s      app=kiali
prometheus               ClusterIP      10.0.138.134   <none>          9090/TCP                                                                                                                                     119s      app=prometheus
tracing                  ClusterIP      10.0.165.210   <none>          80/TCP                                                                                                                                       118s      app=jaeger
zipkin                   ClusterIP      10.0.126.211   <none>          9411/TCP                                                                                                                                     118s      app=jaeger
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
NAME                                     READY     STATUS      RESTARTS   AGE
grafana-88779954d-nzpm7                  1/1       Running     0          6m26s
istio-citadel-7f699dc8c8-n7q8g           1/1       Running     0          6m26s
istio-galley-649bc8cd97-wfjzm            1/1       Running     0          6m26s
istio-ingressgateway-65dfbd566-42wkn     1/1       Running     0          6m26s
istio-init-crd-10-tmtw5                  0/1       Completed   0          20m38s
istio-init-crd-11-ql25l                  0/1       Completed   0          20m38s
istio-pilot-958dd8cc4-4ckf9              2/2       Running     0          6m26s
istio-policy-86b4b7cf9-zf7v7             2/2       Running     4          6m26s
istio-sidecar-injector-d48786c5c-pmrj9   1/1       Running     0          6m26s
istio-telemetry-7f6996fdcc-84w94         2/2       Running     3          6m26s
istio-tracing-79db5954f-h7hmz            1/1       Running     0          6m26s
kiali-5c4cdbb869-s28dv                   1/1       Running     0          6m26s
prometheus-67599bf55b-pgxd8              1/1       Running     0          6m26s
```

Debe haber dos pods `istio-init-crd-*` con un estado `Completed`. Estos pods se encargaron de ejecutar los trabajos que crearon los CRD en un paso anterior. El resto de pods deben mostrar un estado de `Running`. Si los pods no tienen estos estados, espere un minuto o dos hasta que los tengan. Si algún pod notifica un problema, use el comando [kubectl describe pod][kubectl-describe] para revisar su salida y su estado.

## <a name="accessing-the-add-ons"></a>Acceso a complementos

Se instalaron una serie de complementos en Istio en los pasos de instalación anteriores que ofrecen una funcionalidad adicional. Las interfaces de usuario de los complementos no se exponen de forma pública a través de una dirección IP externa. Para acceder a las interfaces de usuario de los complementos, use el comando [kubectl port-forward][kubectl-port-forward]. Este comando crea una conexión segura el equipo cliente y el pod relevante en el clúster de AKS.

Asimismo, agregamos una capa adicional de seguridad para Grafana y Kiali especificando sus respectivas credenciales anteriormente en este artículo.

### <a name="grafana"></a>Grafana

Los paneles de análisis y supervisión para Istio los proporciona [Grafana][grafana]. Reenvíe el puerto local `3000` del equipo cliente al puerto `3000` del pod que ejecuta Grafana en el clúster de AKS:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

La salida del ejemplo siguiente muestra el reenvío de puerto configurado para Grafana:

```console
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

Ahora puede acceder a Grafana en la siguiente dirección URL en el equipo cliente: [http://localhost:3000](http://localhost:3000). Recuerde usar las credenciales que creó a través del secreto de Grafana cuando se le solicite.

### <a name="prometheus"></a>Prometheus

Las métricas para Istio las proporciona [Prometheus][prometheus]. Reenvíe el puerto local `9090` del equipo cliente al puerto `9090` del pod que ejecuta Prometheus en el clúster de AKS:

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

El seguimiento de Istio lo proporciona [Jaeger][jaeger]. Reenvíe el puerto local `16686` del equipo cliente al puerto `16686` del pod que ejecuta Jaeger en el clúster de AKS:

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

[Kiali][kiali] proporciona un panel de observación de malla de servicio. Reenvíe el puerto local `20001` del equipo cliente al puerto `20001` del pod que ejecuta Kiali en el clúster de AKS:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

La salida del ejemplo siguiente muestra el reenvío de puerto configurado para Kiali:

```console
Forwarding from 127.0.0.1:20001 -> 20001
Forwarding from [::1]:20001 -> 20001
```

Ahora puede acceder al panel de observación de malla de servicio de Kiali en la siguiente dirección URL en el equipo cliente: [http://localhost:20001/kiali/console/](http://localhost:20001/kiali/console/). Recuerde usar las credenciales que creó a través del secreto de Kiali cuando se le solicite.

## <a name="uninstall-istio-from-aks"></a>Desinstalar Istio de AKS

> [!WARNING]
> Eliminar Istio de un sistema en ejecución puede provocar problemas de tráfico entre sus servicios. Asegúrese de haber hecho los aprovisionamientos necesarios para que su sistema funcione correctamente sin Istio antes de continuar.

### <a name="remove-istio-components-and-namespace"></a>Quitar los componentes y el espacio de nombres de Istio

Para quitar Istio de su clúster de AKS, use los siguientes comandos. Los comandos `helm delete` quitarán los gráficos `istio` y `istio-init`, y el comando `kubectl delete ns` quitará el espacio de nombres `istio-system`.

```azurecli
helm delete --purge istio
helm delete --purge istio-init
kubectl delete ns istio-system
```

### <a name="remove-istio-crds"></a>Quitar los CRD de Istio

Los comandos anteriores eliminan todos los componentes y el espacio de nombres de Istio, pero aún quedan los CRD de Istio. Para eliminar los CRD, puede usar uno de los siguientes métodos.

Método 1: este comando supone que está ejecutando este paso desde la carpeta de nivel superior de la versión descargada y extraída de Istio que usó para instalar Istio.

```azure-cli
kubectl delete -f install/kubernetes/helm/istio-init/files
```

Método 2: use uno de estos comandos si ya no tiene acceso a la versión descargada y extraída de Istio que usó para instalar Istio. Este comando tardará un poco más: espere unos minutos a que se complete.

Bash
```bash
kubectl get crds -o name | grep 'istio.io' | xargs -n1 kubectl delete
```

PowerShell
```powershell
kubectl get crds -o name | Select-String -Pattern 'istio.io' |% { kubectl delete $_ }
```

## <a name="next-steps"></a>Pasos siguientes

En la siguiente documentación se describe cómo usar Istio para proporcionar enrutamiento inteligente para implantar un lanzamiento controlado:

> [!div class="nextstepaction"]
> [Escenario de enrutamiento inteligente con Istio en AKS][istio-scenario-routing]

Para explorar más opciones de instalación y configuración para Istio, consulte los siguientes artículos oficiales de Istio:

- [Istio: guía de instalación de Helm][istio-install-helm]
- [Istio: opciones de instalación de Helm][istio-install-helm-options]

Puede consultar otros escenarios con el [ejemplo de la aplicación Bookinfo en Istio][istio-bookinfo-example].

Para aprender a supervisar la aplicación de AKS con Application Insights e Istio, consulte la siguiente documentación de Azure Monitor:
- [Supervisión de aplicaciones sin instrumentación para aplicaciones hospedadas de Kubernetes][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/about/notes/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-helm]: https://istio.io/docs/setup/kubernetes/install/helm/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-jobs]: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
