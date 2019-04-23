---
title: Controlar los recursos de clúster con RBAC y Azure AD en Azure Kubernetes Service
description: Obtenga información sobre cómo usar la pertenencia a grupos de Azure Active Directory para restringir el acceso a recursos de clúster mediante el control de acceso basado en roles (RBAC) en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/16/2019
ms.author: iainfou
ms.openlocfilehash: e974c47d1dfb04f66b622c64a7143d00de87c4cb
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014489"
---
# <a name="control-access-to-cluster-resources-using-role-based-access-control-and-azure-active-directory-identities-in-azure-kubernetes-service"></a>Controlar el acceso a recursos de clúster mediante el control de acceso basado en roles y las identidades de Azure Active Directory en Azure Kubernetes Service

Es posible configurar Azure Kubernetes Service (AKS) para que utilice Azure Active Directory (AD) para la autenticación de usuarios. En esta configuración, iniciar sesión en un clúster de AKS mediante un token de autenticación de Azure AD. También puede configurar Kubernetes control de acceso basado en roles (RBAC) para limitar el acceso a los recursos de clúster en función de un usuario identidad o pertenencia a grupo.

Este artículo muestra cómo usar la pertenencia a grupos de Azure AD para controlar el acceso a los espacios de nombres y los recursos con Kubernetes RBAC en un clúster de AKS del clúster. Los usuarios y grupos de ejemplo se crean en Azure AD y, a continuación, se crean Roles y RoleBindings en el clúster de AKS para conceder los permisos adecuados para crear y ver los recursos.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que tiene un clúster AKS existente habilitado con la integración de Azure AD. Si necesita un clúster de AKS, consulte [integrar Azure Active Directory con AKS][azure-ad-aks-cli].

Necesita la CLI de Azure versión 2.0.61 o posterior instalado y configurado. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][install-azure-cli].

## <a name="create-demo-groups-in-azure-ad"></a>Crear grupos de demostración de Azure AD

En este artículo, vamos a crear dos roles de usuario que se pueden usar para mostrar cómo controlan el acceso a recursos de clúster de Kubernetes RBAC y Azure AD. Se usan los siguientes roles de dos ejemplos:

* **Desarrollador de aplicaciones**
    * Un usuario denominado *aksdev* que forma parte de la *appdev* grupo.
* **Ingeniero de fiabilidad de sitio**
    * Un usuario denominado *akssre* que forma parte de la *opssre* grupo.

En entornos de producción, puede usar usuarios y grupos dentro de un inquilino de Azure AD existentes.

En primer lugar, obtenga el identificador de recurso de clúster de AKS mediante la [show de az aks] [ az-aks-show] comando. Asignar el identificador de recurso a una variable denominada *AKS_ID* por lo que puede hacer referencia a los comandos adicionales.

```azurecli-interactive
AKS_ID=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query id -o tsv)
```

Crear el primer grupo de ejemplo en Azure AD para los desarrolladores de aplicaciones mediante el [crear grupo de ad az] [ az-ad-group-create] comando. En el ejemplo siguiente se crea un grupo denominado *appdev*:

```azurecli-interactive
APPDEV_ID=$(az ad group create --display-name appdev --mail-nickname appdev --query objectId -o tsv)
```

Ahora, cree una asignación de roles de Azure para la *appdev* grupo empleando los [crear asignación de roles az] [ az-role-assignment-create] comando. Esta asignación permite que cualquier miembro del grupo de usar `kubectl` para interactuar con un clúster de AKS, concediéndoles el *rol de usuario de clúster de Azure Kubernetes Service*.

```azurecli-interactive
az role assignment create \
  --assignee $APPDEV_ID \
  --role "Azure Kubernetes Service Cluster User Role" \
  --scope $AKS_ID
```

> [!TIP]
> Si recibe un error como `Principal 35bfec9328bd4d8d9b54dea6dac57b82 does not exist in the directory a5443dcd-cd0e-494d-a387-3039b419f0d5.`, espere unos segundos para el identificador de objeto de grupo de Azure AD a propagarse a través del directorio, a continuación, pruebe el `az role assignment create` nuevo comando.

Crear un segundo grupo de ejemplo, en el SRE denominado *opssre*:

```azurecli-interactive
OPSSRE_ID=$(az ad group create --display-name opssre --mail-nickname opssre --query objectId -o tsv)
```

De nuevo, crear una asignación de rol de Azure para conceder a los miembros del grupo de la *rol de usuario de clúster de Azure Kubernetes Service*:

```azurecli-interactive
az role assignment create \
  --assignee $OPSSRE_ID \
  --role "Azure Kubernetes Service Cluster User Role" \
  --scope $AKS_ID
```

## <a name="create-demo-users-in-azure-ad"></a>Crear usuarios de demostración de Azure AD

Con dos grupos de ejemplo creados en Azure AD para nuestros desarrolladores de aplicaciones y SRE, ahora permite crear dos usuarios de ejemplo. Para probar la integración de RBAC al final del artículo, iniciar sesión en el clúster de AKS con estas cuentas.

Crear la primera cuenta de usuario en Azure AD mediante el [az ad usuario crear] [ az-ad-user-create] comando.

En el ejemplo siguiente se crea un usuario con el nombre para mostrar *AKS Dev* y el nombre principal de usuario (UPN) de `aksdev@contoso.com`. Actualizar el UPN para incluir un dominio comprobado para su inquilino de Azure AD (reemplace *contoso.com* con su propio dominio) y proporcionar su propia segura `--password` credencial:

```azurecli-interactive
AKSDEV_ID=$(az ad user create \
  --display-name "AKS Dev" \
  --user-principal-name aksdev@contoso.com \
  --password P@ssw0rd1 \
  --query objectId -o tsv)
```

Ahora agregue el usuario a la *appdev* grupo creado en la sección anterior mediante la [Agregar miembro del grupo az ad] [ az-ad-group-member-add] comando:

```azurecli-interactive
az ad group member add --group appdev --member-id $AKSDEV_ID
```

Cree una segunda cuenta de usuario. En el ejemplo siguiente se crea un usuario con el nombre para mostrar *AKS SRE* y el nombre principal de usuario (UPN) de `akssre@contoso.com`. De nuevo, actualice el UPN para incluir un dominio comprobado para su inquilino de Azure AD (reemplace *contoso.com* con su propio dominio) y proporcionar su propia segura `--password` credencial:

```azurecli-interactive
# Create a user for the SRE role
AKSSRE_ID=$(az ad user create \
  --display-name "AKS SRE" \
  --user-principal-name akssre@contoso.com \
  --password P@ssw0rd1 \
  --query objectId -o tsv)

# Add the user to the opssre Azure AD group
az ad group member add --group opssre --member-id $AKSSRE_ID
```

## <a name="create-the-aks-cluster-resources-for-app-devs"></a>Crear los recursos de clúster AKS para desarrolladores de aplicaciones

Ahora se crean los usuarios y grupos de Azure AD. Se crearon las asignaciones de roles de Azure para que los miembros del grupo para conectarse a un clúster de AKS como un usuario normal. Ahora, vamos a configurar el clúster de AKS para permitir el acceso a recursos específicos de estos grupos diferentes.

En primer lugar, obtenga el clúster de credenciales de administrador mediante el [az aks get-credentials] [ az-aks-get-credentials] comando. En una de las siguientes secciones, obtendrá el normal *usuario* las credenciales para ver la autenticación de Azure AD del clúster flujo en acción.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Crear un espacio de nombres en el clúster AKS con la [kubectl crear espacio de nombres] [ kubectl-create] comando. En el ejemplo siguiente se crea un espacio de nombres *dev*:

```console
kubectl create namespace dev
```

En Kubernetes, *Roles* definir los permisos para conceder, y *RoleBindings* aplicarlas a grupos o usuarios que desee. Estas asignaciones se pueden aplicar a un espacio de nombres especificado o a todo el clúster. Para más información, consulte [Uso de la autorización de RBAC][rbac-authorization].

En primer lugar, cree un rol para el *dev* espacio de nombres. Este rol concede permisos completos para el espacio de nombres. En entornos de producción, puede especificar los permisos más granulares para distintos usuarios o grupos.

Cree un archivo denominado `role-dev-namespace.yaml` y pegue el siguiente manifiesto YAML:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: dev-user-full-access
  namespace: dev
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["batch"]
  resources:
  - jobs
  - cronjobs
  verbs: ["*"]
```

Crear el rol mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de archivo de su manifiesto de YAML:

```console
kubectl apply -f role-dev-namespace.yaml
```

A continuación, obtenga el identificador de recurso para el *appdev* grupo empleando los [mostrar grupo de az ad] [ az-ad-group-show] comando. Este grupo se establece como el sujeto de un RoleBinding en el paso siguiente.

```azurecli-interactive
az ad group show --group appdev --query objectId -o tsv
```

Ahora, cree un RoleBinding para el *appdev* grupo para usar el rol creado anteriormente para el acceso de espacio de nombres. Cree un archivo denominado `rolebinding-dev-namespace.yaml` y pegue el siguiente manifiesto de YAML. En la última línea, reemplace *groupObjectId* con la salida del Id. de objeto de grupo del comando anterior:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: dev-user-access
  namespace: dev
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: dev-user-full-access
subjects:
- kind: Group
  namespace: dev
  name: groupObjectId
```

Cree la RoleBinding mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de archivo de su manifiesto de YAML:

```console
kubectl apply -f rolebinding-dev-namespace.yaml
```

## <a name="create-the-aks-cluster-resources-for-sres"></a>Crear los recursos de clúster AKS para SRE

Ahora, repita los pasos anteriores para crear un espacio de nombres, la función y RoleBinding para el SRE.

En primer lugar, cree un espacio de nombres *sre* utilizando el [kubectl crear espacio de nombres] [ kubectl-create] comando:

```console
kubectl create namespace sre
```

Cree un archivo denominado `role-sre-namespace.yaml` y pegue el siguiente manifiesto YAML:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: sre-user-full-access
  namespace: sre
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["batch"]
  resources:
  - jobs
  - cronjobs
  verbs: ["*"]
```

Crear el rol mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de archivo de su manifiesto de YAML:

```console
kubectl apply -f role-sre-namespace.yaml
```

Obtener el identificador de recurso para el *opssre* grupo empleando los [mostrar grupo de az ad] [ az-ad-group-show] comando:

```azurecli-interactive
az ad group show --group opssre --query objectId -o tsv
```

Crear un RoleBinding para el *opssre* grupo para usar el rol creado anteriormente para el acceso de espacio de nombres. Cree un archivo denominado `rolebinding-sre-namespace.yaml` y pegue el siguiente manifiesto de YAML. En la última línea, reemplace *groupObjectId* con la salida del Id. de objeto de grupo del comando anterior:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: sre-user-access
  namespace: sre
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: sre-user-full-access
subjects:
- kind: Group
  namespace: sre
  name: groupObjectId
```

Cree la RoleBinding mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de archivo de su manifiesto de YAML:

```console
kubectl apply -f rolebinding-sre-namespace.yaml
```

## <a name="interact-with-cluster-resources-using-azure-ad-identities"></a>Interactuar con recursos de clúster con identidades de Azure AD

Ahora, vamos a probar el trabajo de los permisos esperados al crear y administrar recursos en un clúster de AKS. En estos ejemplos, programar y ver los pods en el espacio de nombres asignados del usuario. A continuación, intente programar y ver los pods fuera del espacio de nombres asignado.

En primer lugar, restablecer el *kubeconfig* contexto mediante la [az aks get-credentials] [ az-aks-get-credentials] comando. En una sección anterior, para establecer el contexto con las credenciales de administrador de clúster. El usuario administrador omite el inicio de sesión de Azure AD en los mensajes. Sin el `--admin` se aplica el parámetro, el contexto de usuario que requiere que todas las solicitudes se autentiquen mediante Azure AD.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
```

Programación de un pod NGINX básico mediante el [kubectl ejecutar] [ kubectl-run] comando en el *dev* espacio de nombres:

```console
kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace dev
```

Como el inicio de sesión en el símbolo del sistema, escriba las credenciales de su propio `appdev@contoso.com` cuenta creada al principio del artículo. Una vez que se han iniciado correctamente, se almacena en caché el token de cuenta para el futuro `kubectl` comandos. NGINX es programar correctamente, tal como se muestra en la salida del ejemplo siguiente:

```console
$ kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace dev

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code B24ZD6FP8 to authenticate.

pod/nginx-dev created
```

Ahora utilice el [kubectl get pods] [ kubectl-get] comando para ver los pods en la *dev* espacio de nombres.

```console
kubectl get pods --namespace dev
```

Como se muestra en la salida del ejemplo siguiente, el pod NGINX está correctamente *ejecutando*:

```console
$ kubectl get pods --namespace dev

NAME        READY   STATUS    RESTARTS   AGE
nginx-dev   1/1     Running   0          4m
```

### <a name="create-and-view-cluster-resources-outside-of-the-assigned-namespace"></a>Crear y ver los recursos de clúster fuera del espacio de nombres asignado

Ahora pruebe ver los pods fuera de la *dev* espacio de nombres. Use la [kubectl get pods] [ kubectl-get] comando nuevo, esta vez para ver `--all-namespaces` como sigue:

```console
kubectl get pods --all-namespaces
```

Pertenencia a grupos del usuario no tiene un rol de Kubernetes que permite esta acción, como se muestra en la salida del ejemplo siguiente:

```console
$ kubectl get pods --all-namespaces

Error from server (Forbidden): pods is forbidden: User "aksdev@contoso.com" cannot list resource "pods" in API group "" at the cluster scope
```

En la misma manera, intenta programar un pod en el espacio de nombres diferente, como el *sre* espacio de nombres. Pertenencia a grupos del usuario no es compatible con un rol de Kubernetes y RoleBinding se conceden estos permisos, como se muestra en la salida del ejemplo siguiente:

```console
$ kubectl run --generator=run-pod/v1 nginx-dev --image=nginx --namespace sre

Error from server (Forbidden): pods is forbidden: User "aksdev@contoso.com" cannot create resource "pods" in API group "" in the namespace "sre"
```

### <a name="test-the-sre-access-to-the-aks-cluster-resources"></a>Probar el acceso SRE a los recursos de clúster AKS

Para confirmar que la pertenencia a grupos de Azure AD y Kubernetes RBAC funcionan correctamente entre distintos usuarios y grupos, pruebe los comandos anteriores cuando ha iniciado sesión como el *opssre* usuario.

Restablecer el *kubeconfig* contexto mediante la [az aks get-credentials] [ az-aks-get-credentials] comando que se borra el token de autenticación previamente almacenado en caché para el *aksdev*  usuario:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --overwrite-existing
```

Vuelva a programar y ver los pods en asignado *sre* espacio de nombres. Cuando se le solicite, inicie sesión con su propio `opssre@contoso.com` las credenciales que creó al principio del artículo:

```console
kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace sre
kubectl get pods --namespace sre
```

Como se muestra en la siguiente salida de ejemplo, puede crear y ver los pods correctamente:

```console
$ kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace sre

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BM4RHP3FD to authenticate.

pod/nginx-sre created

$ kubectl get pods --namespace sre

NAME        READY   STATUS    RESTARTS   AGE
nginx-sre   1/1     Running   0
```

Ahora, pruebe ver o programar los pods fuera del espacio de nombres SRE asignado:

```console
kubectl get pods --all-namespaces
kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace dev
```

Estos `kubectl` comandos generan un error, como se muestra en la siguiente salida de ejemplo. Pertenencia a grupos del usuario y el rol de Kubernetes y RoleBindings no conceder permisos para crear o administrador de recursos en otros espacios de nombres:

```console
$ kubectl get pods --all-namespaces
Error from server (Forbidden): pods is forbidden: User "akssre@contoso.com" cannot list pods at the cluster scope

$ kubectl run --generator=run-pod/v1 nginx-sre --image=nginx --namespace dev
Error from server (Forbidden): pods is forbidden: User "akssre@contoso.com" cannot create pods in the namespace "dev"
```

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, ha creado los recursos en el clúster de AKS y los usuarios y grupos de Azure AD. Para limpiar todos estos recursos, ejecute los siguientes comandos:

```azurecli-interactive
# Get the admin kubeconfig context to delete the necessary cluster resources
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin

# Delete the dev and sre namespaces. This also deletes the pods, Roles, and RoleBindings
kubectl delete namespace dev
kubectl delete namespace sre

# Delete the Azure AD user accounts for aksdev and akssre
az ad user delete --upn-or-object-id $AKSDEV_ID
az ad user delete --upn-or-object-id $AKSSRE_ID

# Delete the Azure AD groups for appdev and opssre. This also deletes the Azure role assignments.
az ad group delete --group appdev
az ad group delete --group opssre
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de cómo proteger los clústeres de Kubernetes, consulte [opciones de identidad y acceso para AKS)][rbac-authorization].

Para ver recomendaciones de control de identidades y recursos, consulte [procedimientos recomendados para la autenticación y autorización en AKS][operator-best-practices-identity].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-run]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[install-azure-cli]: /cli/azure/install-azure-cli
[azure-ad-aks-cli]: azure-ad-integration-cli.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-ad-group-create]: /cli/azure/ad/group#az-ad-group-create
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-ad-user-create]: /cli/azure/ad/user#az-ad-user-create
[az-ad-group-member-add]: /cli/azure/ad/group/member#az-ad-group-member-add
[az-ad-group-show]: /cli/azure/ad/group#az-ad-group-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
