---
title: Utilizar directivas de seguridad de pod en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo controlar el pod pacientes mediante PodSecurityPolicy en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/17/2019
ms.author: iainfou
ms.openlocfilehash: 7ce311ab9c554481f64c6c9be40e2018893a0966
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61027382"
---
# <a name="preview---secure-your-cluster-using-pod-security-policies-in-azure-kubernetes-service-aks"></a>Obtener una vista previa: proteger el clúster mediante las directivas de seguridad de pod en Azure Kubernetes Service (AKS)

Para mejorar la seguridad de su clúster de AKS, puede limitar lo que pueden ser pods programados. Los Pods que solicitan recursos no permite que no se pueden ejecutar en el clúster de AKS. Defina este acceso mediante las directivas de seguridad de pod. Este artículo muestra cómo usar las directivas de seguridad de pod para limitar la implementación de pods en AKS.

> [!IMPORTANT]
> Características de versión preliminar AKS son autoservicio y participación. Las versiones preliminares se proporcionan para recopilar comentarios y los errores de nuestra comunidad. Sin embargo, no se admiten por soporte técnico de Azure. Si crea un clúster, o agregar estas características para clústeres existentes, ese clúster no se admite hasta que la característica ya no está en versión preliminar y se aprueba para disponibilidad general (GA).
>
> Si tiene problemas con las características de vista previa, [abra una incidencia en el repositorio de GitHub de AKS] [ aks-github] con el nombre de la característica de vista previa en el título del error.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, vea la guía de inicio rápido AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

Necesita la CLI de Azure versión 2.0.61 o posterior instalado y configurado. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Instalación de la extensión aks-preview de la CLI

Clústeres AKS se actualizan para habilitar las directivas de seguridad de pod mediante la *-versión preliminar de aks* extensión de la CLI. Instalar el *-versión preliminar de aks* extensión de CLI de Azure mediante el [Agregar extensión az] [ az-extension-add] de comandos, tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Si ha instalado anteriormente el *-versión preliminar de aks* instalar haya disponible de extensión, actualizaciones utilizando el `az extension update --name aks-preview` comando.

### <a name="register-pod-security-policy-feature-provider"></a>Registrar proveedor de características de directiva de seguridad de pod

Para crear o actualizar un clúster AKS para usar las directivas de seguridad de pod, habilitar una marca de características en su suscripción. Para registrar el *PodSecurityPolicyPreview* marca de características, use la [register de la característica de az] [ az-feature-register] comando tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az feature register --name PodSecurityPolicyPreview --namespace Microsoft.ContainerService
```

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/PodSecurityPolicyPreview')].{Name:name,State:properties.state}"
```

Cuando todo esté listo, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="overview-of-pod-security-policies"></a>Información general de las directivas de seguridad de pod

En un clúster de Kubernetes, se usa un controlador de admisión para interceptar las solicitudes al servidor de API cuando es necesario crear un recurso. El controlador de admisión, a continuación, puede *validar* la solicitud del recurso con un conjunto de reglas, o *mutar* el recurso para cambiar los parámetros de implementación.

*PodSecurityPolicy* es un controlador de admisión que valida una especificación de pod cumple los requisitos definidos. Puede limitar el uso de contenedores con privilegios, el acceso a ciertos tipos de almacenamiento, o el usuario o grupo que puede ejecutar el contenedor como estos requisitos. Al intentar implementar un recurso donde las especificaciones de pod no cumplen los requisitos descritos en la directiva de seguridad de pod, se deniega la solicitud. Esta capacidad para controlar lo que pueden ser pods programados en el AKS clúster evita que algunos posibles vulnerabilidades de seguridad o las elevaciones de privilegios.

Cuando se habilita la directiva de seguridad de pod en un clúster de AKS, se aplican algunas directivas de forma predeterminada. Estas directivas predeterminadas proporcionan una experiencia de fábrica para definir qué pods que se puede programar. Sin embargo, los usuarios del clúster pueden encontrarse con problemas al implementar los pods hasta que defina sus propias directivas. El enfoque recomendado es:

* Creación de un clúster de AKS
* Definir sus propias directivas de seguridad de pod
* Habilitar la característica de directiva de seguridad de pod

Para mostrar cómo las directivas predeterminadas de limitan las implementaciones de pod, en este artículo, primero habilite la característica de directivas de seguridad de pod y después crear una directiva personalizada.

## <a name="enable-pod-security-policy-on-an-aks-cluster"></a>Habilitar directiva de seguridad de pod en un clúster de AKS

Puede habilitar o deshabilitar la directiva de seguridad de pod mediante la [actualizar az aks] [ az-aks-update] comando. El ejemplo siguiente se habilita la directiva de la seguridad de pod en el nombre del clúster *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*.

> [!NOTE]
> Para el uso del mundo real, no habilite la directiva de seguridad de pod hasta que haya definido sus propias directivas personalizadas. En este artículo, habilitar Directiva de seguridad de pod como el primer paso para ver cómo limitan el pod las directivas predeterminadas de las implementaciones.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-pod-security-policy
```

## <a name="default-aks-policies"></a>Directivas predeterminadas de AKS

Cuando se habilita la directiva de seguridad de pod, AKS crea dos directivas predeterminadas denominadas *con privilegios* y *restringido*. No editar o quitar estas directivas de forma predeterminada. En su lugar, cree sus propias directivas que definen la configuración que desee al control. Echemos un primer vistazo a lo que estas directivas predeterminadas son cómo afectan las implementaciones de pod.

Para ver las directivas disponibles, use la [kubectl get psp] [ kubectl-get] de comandos, tal como se muestra en el ejemplo siguiente. Como parte de la predeterminada *restringido* directiva, el usuario no tiene permiso *PRIV* usar para el usuario y de escalamiento de pod con privilegios *MustRunAsNonRoot*.

```console
$ kubectl get psp

NAME         PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged   true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
restricted   false          RunAsAny   MustRunAsNonRoot   MustRunAs   MustRunAs   false            configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

El *restringido* pod la directiva de seguridad se aplica a cualquier usuario autenticado en el clúster de AKS. Esta asignación se controla mediante ClusterRoles y ClusterRoleBindings. Use la [kubectl get clusterrolebindings] [ kubectl-get] comando y busque el *predeterminado: restringido:* enlace:

```console
kubectl get clusterrolebindings default:restricted -o yaml
```

Como se muestra en la siguiente salida reducida, la *psp: restringido* ClusterRole se asigna a cualquier *system: autenticado* a los usuarios. Esta capacidad proporciona un nivel básico de restricciones sin sus propias directivas que se está definidos.

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  [...]
  name: default:restricted
  [...]
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp:restricted
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:authenticated
```

Es importante entender cómo interactúan estas directivas predeterminadas con las solicitudes de usuario para programar los pods antes de empezar a crear directivas de seguridad de su pod. En las secciones siguientes, vamos a programar algunas pod para ver estas directivas de forma predeterminada en acción.

## <a name="create-a-test-user-in-an-aks-cluster"></a>Crear un usuario de prueba en un clúster de AKS

De forma predeterminada, cuando se usa el [az aks get-credentials] [ az-aks-get-credentials] comando, el *admin* las credenciales para el clúster de AKS y agregado a su `kubectl` config. El usuario administrador omite el cumplimiento de directivas de seguridad de pod. Si usa la integración de Azure Active Directory para los clústeres de AKS, podría iniciar sesión con las credenciales de un usuario sin derechos administrativos para ver el cumplimiento de directivas en acción. En este artículo, vamos a crear una cuenta de usuario de prueba en el clúster de AKS que puede usar.

Crear un espacio de nombres de ejemplo denominado *psp aks* para recursos de prueba mediante el [kubectl crear espacio de nombres] [ kubectl-create] comando. A continuación, cree una cuenta de servicio denominada *nonadmin usuario* utilizando el [kubectl crear serviceaccount] [ kubectl-create] comando:

```console
kubectl create namespace psp-aks
kubectl create serviceaccount --namespace psp-aks nonadmin-user
```

A continuación, cree un RoleBinding para el *nonadmin usuario* para realizar acciones básicas en el espacio de nombres mediante el [kubectl crear rolebinding] [ kubectl-create] comando:

```console
kubectl create rolebinding \
    --namespace psp-aks \
    psp-aks-editor \
    --clusterrole=edit \
    --serviceaccount=psp-aks:nonadmin-user
```

### <a name="create-alias-commands-for-admin-and-non-admin-user"></a>Crear comandos de alias de usuario de administrador y que no es administrador

Para resaltar la diferencia entre el usuario de administrador regulares cuando se usa `kubectl` y el usuario sin derechos administrativos que creó en los pasos anteriores, cree dos alias de línea de comandos:

* El **kubectl-admin** alias es para el usuario administrador regular y se centra en la *psp aks* espacio de nombres.
* El **kubectl nonadminuser** alias es para el *nonadmin usuario* creado en el paso anterior y se centra en la *psp aks* espacio de nombres.

Crear estos dos alias, como se muestra en los siguientes comandos:

```console
alias kubectl-admin='kubectl --namespace psp-aks'
alias kubectl-nonadminuser='kubectl --as=system:serviceaccount:psp-aks:nonadmin-user --namespace psp-aks'
```

## <a name="test-the-creation-of-a-privileged-pod"></a>Probar la creación de un pod con privilegios

Vamos a probar primero lo que sucede cuando se programa un pod con el contexto de seguridad `privileged: true`. Este contexto de seguridad eleva los privilegios del pod. En la sección anterior que mostraba el pod AKS de forma predeterminada en las directivas de seguridad, el *restringido* directiva debe denegar esta solicitud.

Cree un archivo denominado `nginx-privileged.yaml` y pegue el siguiente manifiesto YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: nginx:1.14.2
      securityContext:
        privileged: true
```

Crear el pod mediante la [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl-nonadminuser apply -f nginx-privileged.yaml
```

El pod no puede programarse, como se muestra en la salida del ejemplo siguiente:

```console
$ kubectl-nonadminuser apply -f nginx-privileged.yaml

Error from server (Forbidden): error when creating "nginx-privileged.yaml": pods "nginx-privileged" is forbidden: unable to validate against any pod security policy: [spec.containers[0].securityContext.privileged: Invalid value: true: Privileged containers are not allowed]
```

El pod no alcance la fase de la programación, por lo que no hay ningún recurso para eliminar antes de ir.

## <a name="test-creation-of-an-unprivileged-pod"></a>Creación de pruebas de un pod sin privilegios

En el ejemplo anterior, la especificación de pod solicitado escalación de privilegios. Esta solicitud se deniega de forma predeterminada el *restringido* pod de directiva de seguridad, por lo que no se puede programar el pod. Vamos a probar ahora ejecuta ese mismo pod NGINX sin la solicitud de elevación de privilegios.

Cree un archivo denominado `nginx-unprivileged.yaml` y pegue el siguiente manifiesto YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
```

Crear el pod mediante la [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

El programador de Kubernetes acepta la solicitud de pod. Sin embargo, si observa el estado de los pod mediante `kubectl get pods`, hay un error:

```console
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS                       RESTARTS   AGE
nginx-unprivileged   0/1     CreateContainerConfigError   0          26s
```

Use la [kubectl describe pod] [ kubectl-describe] comando para consultar los eventos para el pod. El siguiente ejemplo reducido muestra que la imagen y el contenedor requieren permisos de raíz, aunque no solicitan:

```console
$ kubectl-nonadminuser describe pod nginx-unprivileged

Name:               nginx-unprivileged
Namespace:          psp-aks
Priority:           0
PriorityClassName:  <none>
Node:               aks-agentpool-34777077-0/10.240.0.4
Start Time:         Thu, 28 Mar 2019 22:05:04 +0000
[...]
Events:
  Type     Reason     Age                     From                               Message
  ----     ------     ----                    ----                               -------
  Normal   Scheduled  7m14s                   default-scheduler                  Successfully assigned psp-aks/nginx-unprivileged to aks-agentpool-34777077-0
  Warning  Failed     5m2s (x12 over 7m13s)   kubelet, aks-agentpool-34777077-0  Error: container has runAsNonRoot and image will run as root
  Normal   Pulled     2m10s (x25 over 7m13s)  kubelet, aks-agentpool-34777077-0  Container image "nginx:1.14.2" already present on machine
```

Aunque no solicitamos cualquier acceso con privilegios, la imagen de contenedor de NGINX se necesita para crear un enlace de puerto *80*. Para enlazar puertos *1024* y versiones posteriores, el *raíz* usuario es necesario. Cuando el pod intenta iniciarse, el *restringido* pod la directiva de seguridad deniega esta solicitud.

En este ejemplo muestra que las directivas de seguridad de pod predeterminado creadas por AKS están en vigor y restringen las acciones que puede realizar un usuario. Es importante comprender el comportamiento de estas directivas de forma predeterminada, como no puede esperar un pod NGINX básico que se deniegan.

Antes de pasar al paso siguiente, elimine este pod de prueba mediante el [kubectl eliminar pod] [ kubectl-delete] comando:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="test-creation-of-a-pod-with-a-specific-user-context"></a>Creación de pruebas de un pod con un contexto de usuario específico

En el ejemplo anterior, la imagen de contenedor intentó automáticamente utilizar raíz para enlazar NGINX al puerto 80. El valor predeterminado ha denegado esta solicitud *restringido* pod de directiva de seguridad, por lo que no se puede iniciar el pod. Vamos a probar ahora ejecuta ese mismo pod NGINX con un contexto de usuario específico, como `runAsUser: 2000`.

Cree un archivo denominado `nginx-unprivileged-nonroot.yaml` y pegue el siguiente manifiesto YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged-nonroot
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx:1.14.2
      securityContext:
        runAsUser: 2000
```

Crear el pod mediante la [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl-nonadminuser apply -f nginx-unprivileged-nonroot.yaml
```

El programador de Kubernetes acepta la solicitud de pod. Sin embargo, si observa el estado de los pod mediante `kubectl get pods`, hay un error diferente que del ejemplo anterior:

```console
$ kubectl-nonadminuser get pods

NAME                         READY   STATUS              RESTARTS   AGE
nginx-unprivileged-nonroot   0/1     CrashLoopBackOff    1          3s
```

Use la [kubectl describe pod] [ kubectl-describe] comando para consultar los eventos para el pod. El siguiente ejemplo reducido muestra los eventos de pod:

```console
$ kubectl-nonadminuser describe pods nginx-unprivileged

Name:               nginx-unprivileged
Namespace:          psp-aks
Priority:           0
PriorityClassName:  <none>
Node:               aks-agentpool-34777077-0/10.240.0.4
Start Time:         Thu, 28 Mar 2019 22:05:04 +0000
[...]
Events:
  Type     Reason     Age                   From                               Message
  ----     ------     ----                  ----                               -------
  Normal   Scheduled  2m14s                 default-scheduler                  Successfully assigned psp-aks/nginx-unprivileged-nonroot to aks-agentpool-34777077-0
  Normal   Pulled     118s (x3 over 2m13s)  kubelet, aks-agentpool-34777077-0  Container image "nginx:1.14.2" already present on machine
  Normal   Created    118s (x3 over 2m13s)  kubelet, aks-agentpool-34777077-0  Created container
  Normal   Started    118s (x3 over 2m12s)  kubelet, aks-agentpool-34777077-0  Started container
  Warning  BackOff    105s (x5 over 2m11s)  kubelet, aks-agentpool-34777077-0  Back-off restarting failed container
```

Los eventos indican que se creó y se inicia el contenedor. No hay nada evidente de inmediato sobre por qué el pod es en un estado de error. Echemos un vistazo a los registros del pod mediante la [kubectl registros] [ kubectl-logs] comando:

```console
kubectl-nonadminuser logs nginx-unprivileged-nonroot --previous
```

La salida del registro siguiente en el ejemplo da una indicación que dentro de la configuración de NGINX, hay un error de permisos cuando intente iniciar el servicio. Este error se debe volver a por necesidad enlazar al puerto 80. Aunque la especificación de pod define una cuenta de usuario normal, esta cuenta de usuario no es suficiente en el nivel de sistema operativo para el servicio NGINX para iniciar y enlazar con el puerto restringido.

```console
$ kubectl-nonadminuser logs nginx-unprivileged-nonroot --previous

2019/03/28 22:38:29 [warn] 1#1: the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
nginx: [warn] the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
2019/03/28 22:38:29 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
```

Nuevamente, es importante comprender el comportamiento de las directivas de seguridad de pod de forma predeterminada. Este error era un poco más difícil realizar un seguimiento y de nuevo, no se puede esperar un pod NGINX básico que se deniegan.

Antes de pasar al paso siguiente, elimine este pod de prueba mediante el [kubectl eliminar pod] [ kubectl-delete] comando:

```console
kubectl-nonadminuser delete -f nginx-unprivileged-nonroot.yaml
```

## <a name="create-a-custom-pod-security-policy"></a>Crear una directiva de seguridad personalizado pod

Ahora que ha visto el comportamiento de las directivas de seguridad de pod de forma predeterminada, vamos a proporcionar una forma para el *nonadmin usuario* para programar correctamente los pods.

Vamos a crear una directiva para rechazar los pods que solicitan acceso con privilegios. Otras opciones, tales como *runAsUser* o permiten *volúmenes*, no se restringen explícitamente. Este tipo de directiva deniega una solicitud de acceso con privilegios, pero en caso contrario, permite que el clúster ejecute los pods solicitados.

Cree un archivo denominado `psp-deny-privileged.yaml` y pegue el siguiente manifiesto YAML:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: psp-deny-privileged
spec:
  privileged: false
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  runAsUser:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  volumes:
  - '*'
```

Crear la directiva con la [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl apply -f psp-deny-privileged.yaml
```

Para ver las directivas disponibles, use la [kubectl get psp] [ kubectl-get] de comandos, tal como se muestra en el ejemplo siguiente. Comparar el *psp-denegar-con privilegios* directiva con el valor predeterminado *restringido* directiva que se aplica en los ejemplos anteriores para crear un pod. Solo el uso de *PRIV* denegada por la directiva de extensión. No hay ninguna restricción en el usuario o grupo para el *psp-denegar-con privilegios* directiva.

```console
$ kubectl get psp

NAME                  PRIV    CAPS   SELINUX    RUNASUSER          FSGROUP     SUPGROUP    READONLYROOTFS   VOLUMES
privileged            true    *      RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
psp-deny-privileged   false          RunAsAny   RunAsAny           RunAsAny    RunAsAny    false            *
restricted            false          RunAsAny   MustRunAsNonRoot   MustRunAs   MustRunAs   false            configMap,emptyDir,projected,secret,downwardAPI,persistentVolumeClaim
```

## <a name="allow-user-account-to-use-the-custom-pod-security-policy"></a>Permitir cuenta de usuario usar la directiva de seguridad personalizado pod

En el paso anterior, ha creado una directiva de seguridad de pod para rechazar los pods que el acceso con privilegios de solicitud. Para permitir que la directiva que se usará, se crea un *rol* o un *ClusterRole*. A continuación, asocie una de estas funciones mediante un *RoleBinding* o *ClusterRoleBinding*.

En este ejemplo, cree un ClusterRole que le permite *usar* el *psp-denegar-con privilegios* directiva creada en el paso anterior. Cree un archivo denominado `psp-deny-privileged-clusterrole.yaml` y pegue el siguiente manifiesto YAML:

```yaml
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: psp-deny-privileged-clusterrole
rules:
- apiGroups:
  - extensions
  resources:
  - podsecuritypolicies
  resourceNames:
  - psp-deny-privileged
  verbs:
  - use
```

Cree la ClusterRole mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl apply -f psp-deny-privileged-clusterrole.yaml
```

Ahora cree un ClusterRoleBinding para usar el ClusterRole creado en el paso anterior. Cree un archivo denominado `psp-deny-privileged-clusterrolebinding.yaml` y pegue el siguiente manifiesto YAML:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: psp-deny-privileged-clusterrolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: psp-deny-privileged-clusterrole
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:serviceaccounts
```

Crear un ClusterRoleBinding mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl apply -f psp-deny-privileged-clusterrolebinding.yaml
```

> [!NOTE]
> En el primer paso de este artículo, se ha habilitado la característica de directiva de seguridad de pod en el clúster AKS. El método recomendado era habilitar solo la característica de directiva de seguridad de pod después de definir sus propias directivas. Se trata el escenario donde podría habilitar la característica de directiva de seguridad de pod. Se han definido una o varias directivas personalizadas y las cuentas de usuario se han asociado con esas directivas. Ahora puede hacerlo con seguridad de la directiva de seguridad de pod de características y minimizar los problemas causados por las directivas predeterminadas.

## <a name="test-the-creation-of-an-unprivileged-pod-again"></a>Probar la creación de un pod sin privilegios de nuevo

Con la directiva de seguridad de pod personalizado aplicado y un enlace para la cuenta de usuario para utilizar la directiva, vamos a intentar volver a crear un pod sin privilegios. Usar el mismo `nginx-privileged.yaml` manifiesto para crear el pod mediante la [kubectl aplicar] [ kubectl-apply] comando:

```console
kubectl-nonadminuser apply -f nginx-unprivileged.yaml
```

Se ha programado correctamente el pod. Al activar el estado de los pod mediante la [kubectl get pods] [ kubectl-get] comando, el pod es *ejecutando*:

```
$ kubectl-nonadminuser get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          7m14s
```

En este ejemplo se muestra cómo puede crear directivas de seguridad de pod personalizado para definir el acceso al clúster de AKS para distintos usuarios o grupos. Por lo tanto, el valor predeterminado las directivas AKS proporcionan controles estrictos sobre qué pod pueden ejecutar, crear sus propias directivas personalizadas, a continuación, definir correctamente las restricciones que necesita.

Eliminar el pod NGINX sin privilegios mediante la [kubectl eliminar] [ kubectl-delete] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl-nonadminuser delete -f nginx-unprivileged.yaml
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para deshabilitar la directiva de seguridad de pod, use el [actualizar az aks] [ az-aks-update] nuevo comando. El ejemplo siguiente se deshabilita la directiva de la seguridad de pod en el nombre del clúster *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --disable-pod-security-policy
```

A continuación, elimine el ClusterRole y ClusterRoleBinding:

```console
kubectl delete -f psp-deny-privileged-clusterrolebinding.yaml
kubectl delete -f psp-deny-privileged-clusterrole.yaml
```

Eliminar la directiva de red con [kubectl eliminar] [ kubectl-delete] comando y especifique el nombre de su manifiesto YAML:

```console
kubectl delete -f psp-deny-privileged.yaml
```

Por último, eliminar el *psp aks* espacio de nombres:

```console
kubectl delete namespace psp-aks
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se ha explicado cómo crear una directiva de seguridad de pod para evitar el uso de acceso con privilegios. Hay una gran cantidad de características que puede aplicar una directiva, como el tipo de volumen o el usuario RunAs. Para obtener más información sobre las opciones disponibles, consulte el [documentos de referencia de directiva de seguridad de pod de Kubernetes][kubernetes-policy-reference].

Para obtener más información acerca de cómo limitar el tráfico de red de pod, vea [proteger el tráfico entre pods mediante las directivas de redes en AKS][network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubernetes-policy-reference]: https://kubernetes.io/docs/concepts/policy/pod-security-policy/#policy-reference
[aks-github]: https://github.com/azure/aks/issues

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policies]: use-network-policies.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-extension-add]: /cli/azure/extension#az-extension-add
