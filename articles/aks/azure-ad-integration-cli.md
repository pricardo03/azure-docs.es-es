---
title: Integración de Azure Active Directory con Azure Kubernetes Service
description: Aprenda a usar la CLI de Azure para crear y el clúster de Azure Active Directory habilitado Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/16/2019
ms.author: iainfou
ms.openlocfilehash: 0216a8c7d4e52e89098979223e9b792398e25038
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64920179"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service-using-the-azure-cli"></a>Integrar Azure Active Directory con Azure Kubernetes Service mediante la CLI de Azure

Es posible configurar Azure Kubernetes Service (AKS) para que utilice Azure Active Directory (AD) para la autenticación de usuarios. En esta configuración, puede iniciar sesión en un clúster de AKS mediante un token de autenticación de Azure AD. Los operadores del clúster también pueden configurar Kubernetes control de acceso basado en roles (RBAC) en función de una identidad o el directorio de pertenencia a grupos.

Este artículo muestra cómo crear los componentes de Azure AD, a continuación, implementar un clúster de Azure habilitado para AD y cree un rol RBAC básico en el clúster de AKS. También puede [completar estos pasos con Azure portal][azure-ad-portal].

Para el script de ejemplo completo que se usa en este artículo, consulte [ejemplos de la CLI de Azure: integración de AKS con Azure AD][complete-script].

Se aplican las siguientes limitaciones:

- Azure AD solo puede habilitarse cuando se crea un clúster habilitado para RBAC nuevo. No se puede habilitar Azure AD en un clúster AKS existente.
- *Invitado* a los usuarios en Azure AD, tal como si usa un inicio de sesión federado desde un directorio diferente, no se admiten.

## <a name="before-you-begin"></a>Antes de empezar

Necesita la CLI de Azure versión 2.0.61 o posterior instalado y configurado. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][install-azure-cli].

Para mantener la coherencia y ayudar a ejecutar los comandos en este artículo, cree una variable para el nombre deseado del clúster AKS. En el ejemplo siguiente se usa el nombre *myakscluster*:

```azurecli-interactive
aksname="myakscluster"
```

## <a name="azure-ad-authentication-overview"></a>Introducción a la autenticación de Azure AD

La autenticación de Azure AD se proporciona a los clústeres de AKS con OpenID Connect. OpenID Connect es una capa de identidad creada basándose en el protocolo OAuth 2.0. Puede encontrar más información sobre OpenID Connect en la [documentación de OpenID Connect][open-id-connect].

Dentro del clúster de Kubernetes, se usa la autenticación de token de webhook para verificar los tokens de autenticación. La autenticación de token de webhook se configura y administra como parte del clúster de AKS. Puede encontrar más información sobre la autenticación de token de webhook en la [documentación de autenticación de webhook][kubernetes-webhook].

> [!NOTE]
> Al configurar Azure AD para la autenticación de AKS, se configuran dos aplicaciones de Azure AD. Un administrador del inquilino de Azure debe realizar esta operación.

## <a name="create-azure-ad-server-component"></a>Crear el componente de servidor de Azure AD

Para integrar con AKS, crear y usar una aplicación de Azure AD que actúa como un punto de conexión para las solicitudes de identidad. La primera aplicación de Azure AD que necesita Obtiene la pertenencia a grupos de Azure AD para un usuario.

Crear el componente de aplicación de servidor mediante el [crear az ad app] [ az-ad-app-create] comando y, a continuación, actualizar la pertenencia al grupo de notificaciones mediante el [actualización de az ad app] [ az-ad-app-update] comando. En el ejemplo siguiente se usa el *aksname* variable definida en el [antes de comenzar](#before-you-begin) sección y crea una variable

```azurecli-interactive
# Create the Azure AD application
serverApplicationId=$(az ad app create \
    --display-name "${aksname}Server" \
    --identifier-uris "https://${aksname}Server" \
    --query appId -o tsv)

# Update the application group memebership claims
az ad app update --id $serverApplicationId --set groupMembershipClaims=All
```

Ahora cree una entidad de servicio para la aplicación de servidor con el [az ad sp crear] [ az-ad-sp-create] comando. Esta entidad de servicio se usa para autenticarse a sí mismo dentro de la plataforma Azure. A continuación, obtenga la entidad de servicio secreto mediante la [Restablecer credenciales de az ad sp] [ az-ad-sp-credential-reset] de comandos y asigne a la variable denominada *serverApplicationSecret* para su uso en uno de los pasos siguientes:

```azurecli-interactive
# Create a service principal for the Azure AD application
az ad sp create --id $serverApplicationId

# Get the service principal secret
serverApplicationSecret=$(az ad sp credential reset \
    --name $serverApplicationId \
    --credential-description "AKSPassword" \
    --query password -o tsv)
```

Azure AD necesita permisos para realizar las siguientes acciones:

* Leer datos de directorio
* Iniciar sesión y leer el perfil de usuario

Asigne estos permisos mediante la [agregar permiso de az ad app] [ az-ad-app-permission-add] comando:

```azurecli-interactive
az ad app permission add \
    --id $serverApplicationId \
    --api 00000003-0000-0000-c000-000000000000 \
    --api-permissions e1fe6dd8-ba31-4d61-89e7-88639da4683d=Scope 06da0dbc-49e2-44d2-8312-53f166ab848a=Scope 7ab1d382-f21e-4acd-a863-ba3e13f7da61=Role
```

Por último, conceder los permisos asignados en el paso anterior para el servidor de aplicaciones mediante el [concesión de permisos de az ad app] [ az-ad-app-permission-grant] comando. Este paso se produce un error si la cuenta actual no es un administrador de inquilinos. También deberá agregar permisos para la aplicación de Azure AD solicitar información que en caso contrario, puede requerir el uso de consentimiento del administrador del [az ad app permiso-consentimiento del administrador][az-ad-app-permission-admin-consent]:

```azurecli-interactive
az ad app permission grant --id $serverApplicationId --api 00000003-0000-0000-c000-000000000000
az ad app permission admin-consent --id  $serverApplicationId
```

## <a name="create-azure-ad-client-component"></a>Crear el componente de cliente de Azure AD

La segunda aplicación de Azure AD se usa cuando un usuario inicia sesión el clúster de AKS con la CLI de Kubernetes (`kubectl`). Esta aplicación cliente recibe la solicitud de autenticación del usuario y comprueba sus credenciales y permisos. Crear la aplicación de Azure AD para el componente de cliente mediante el [crear az ad app] [ az-ad-app-create] comando:

```azurecli-interactive
clientApplicationId=$(az ad app create \
    --display-name "${aksname}Client" \
    --native-app \
    --reply-urls "https://${aksname}Client" \
    --query appId -o tsv)
```

Crear una entidad de servicio para la aplicación cliente mediante el [az ad sp crear] [ az-ad-sp-create] comando:

```azurecli-interactive
az ad sp create --id $clientApplicationId
```

Obtener el identificador de oAuth2 para la aplicación de servidor para permitir que el flujo de autenticación entre los componentes de dos aplicaciones mediante el [show de az ad app] [ az-ad-app-show] comando. Este identificador de oAuth2 se usa en el paso siguiente.

```azurecli-interactive
oAuthPermissionId=$(az ad app show --id $serverApplicationId --query "oauth2Permissions[0].id" -o tsv)
```

Agregue los permisos para la aplicación cliente y componentes de aplicación de servidor para usar la comunicación de oAuth2 fluyen utilizando la [agregar permiso de az ad app] [ az-ad-app-permission-add] comando. A continuación, conceda permisos para la aplicación cliente para la comunicación con el servidor de aplicaciones mediante el [concesión de permisos de az ad app] [ az-ad-app-permission-grant] comando:

```azurecli-interactive
az ad app permission add --id $clientApplicationId --api $serverApplicationId --api-permissions $oAuthPermissionId=Scope
az ad app permission grant --id $clientApplicationId --api $serverApplicationId
```

## <a name="deploy-the-cluster"></a>Implementar el clúster

Con las dos aplicaciones de Azure AD creadas, cree ahora el propio clúster AKS. En primer lugar, cree un grupo de recursos mediante el [crear grupo az] [ az-group-create] comando. En el ejemplo siguiente se crea el grupo de recursos en el *EastUS* región:

Cree un grupo de recursos para el clúster:

```azurecli-interactive
az group create --name myResourceGroup --location EastUS
```

Obtener el identificador del inquilino de su suscripción de Azure con el [mostrar de la cuenta de az] [ az-account-show] comando. A continuación, cree el clúster AKS con la [crear az aks] [ az-aks-create] comando. El comando para crear el clúster de AKS proporciona el servidor y cliente, identificadores de aplicación, el secreto principal de servicio de aplicación de servidor y el identificador de inquilino:

```azurecli-interactive
tenantId=$(az account show --query tenantId -o tsv)

az aks create \
    --resource-group myResourceGroup \
    --name $aksname \
    --node-count 1 \
    --generate-ssh-keys \
    --aad-server-app-id $serverApplicationId \
    --aad-server-app-secret $serverApplicationSecret \
    --aad-client-app-id $clientApplicationId \
    --aad-tenant-id $tenantId
```

Por último, obtener el clúster de credenciales de administrador mediante el [az aks get-credentials] [ az-aks-get-credentials] comando. En uno de los pasos siguientes, obtendrá las tarifas *usuario* las credenciales para ver la autenticación de Azure AD del clúster flujo en acción.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name $aksname --admin
```

## <a name="create-rbac-binding"></a>Creación del enlace de RBAC

Para poder usar una cuenta de Azure Active Directory con el clúster AKS, debe crear un enlace de rol o un enlace de rol del clúster. *Roles* define los permisos para conceder, y *enlaces* los aplica a los usuarios deseados. Estas asignaciones se pueden aplicar a un espacio de nombres especificado o a todo el clúster. Para más información, consulte [Uso de la autorización de RBAC][rbac-authorization].

Obtener el nombre principal de usuario (UPN) para el usuario conectado actualmente uso la [Mostrar usuarios inicien sesión de az ad] [ az-ad-signed-in-user-show] comando. Esta cuenta de usuario está habilitada para la integración de Azure AD en el paso siguiente.

```azurecli-interactive
az ad signed-in-user show --query userPrincipalName -o tsv
```

> [!IMPORTANT]
> Si es el usuario que conceda el enlace de RBAC en el mismo inquilino de Azure AD, asignar permisos basados en la *userPrincipalName*. Si el usuario está en un Azure AD diferente de inquilinos, consultar y usar el *objectId* propiedad en su lugar.

Crear un manifiesto YAML denominado `basic-azure-ad-binding.yaml` y pegue el siguiente contenido. En la última línea, reemplace *userPrincipalName_or_objectId* con el resultado de Id. de UPN o el objeto del comando anterior:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: userPrincipalName_or_objectId
```

Cree la ClusterRoleBinding mediante el [kubectl aplicar] [ kubectl-apply] comando y especifique el nombre de archivo de su manifiesto de YAML:

```console
kubectl apply -f basic-azure-ad-binding.yaml
```

## <a name="access-cluster-with-azure-ad"></a>Acceso al clúster con Azure AD

Ahora vamos a probar la integración de autenticación de Azure AD para el clúster de AKS. Establecer el `kubectl` contexto de configuración para usar credenciales de usuario normal. Este contexto pasa todas las solicitudes de autenticación a través de Azure AD.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name $aksname --overwrite-existing
```

Ahora utilice el [kubectl get pods] [ kubectl-get] comando para ver los pods entre todos los espacios de nombres:

```console
kubectl get pods --all-namespaces
```

Recibir un inicio de sesión en el símbolo del sistema para autenticarse con credenciales de Azure AD mediante un explorador web. Una vez se haya autenticado correctamente, el `kubectl` comando muestra los pods del clúster de AKS, como se muestra en la salida del ejemplo siguiente:

```console
$ kubectl get pods --all-namespaces

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BYMK7UXVD to authenticate.

NAMESPACE     NAME                                    READY   STATUS    RESTARTS   AGE
kube-system   coredns-754f947b4-2v75r                 1/1     Running   0          23h
kube-system   coredns-754f947b4-tghwh                 1/1     Running   0          23h
kube-system   coredns-autoscaler-6fcdb7d64-4wkvp      1/1     Running   0          23h
kube-system   heapster-5fb7488d97-t5wzk               2/2     Running   0          23h
kube-system   kube-proxy-2nd5m                        1/1     Running   0          23h
kube-system   kube-svc-redirect-swp9r                 2/2     Running   0          23h
kube-system   kubernetes-dashboard-847bb4ddc6-trt7m   1/1     Running   0          23h
kube-system   metrics-server-7b97f9cd9-btxzz          1/1     Running   0          23h
kube-system   tunnelfront-6ff887cffb-xkfmq            1/1     Running   0          23h
```

El token de autenticación recibido para `kubectl` se almacena en caché. Solo introducir de nuevo para iniciar sesión cuando el token ha expirado o se vuelve a crear el archivo de configuración de Kubernetes.

Si ve un mensaje de error de autorización una vez registrado correctamente en un explorador web, como se muestra en la siguiente salida de ejemplo, compruebe los siguientes posibles problemas:

```console
error: You must be logged in to the server (Unauthorized)
```

* El usuario haya iniciado sesión en como no es un *invitado* en la instancia de Azure AD (éste suele ser el caso si utiliza un inicio de sesión federada desde un directorio diferente).
* El usuario no es miembro de más de 200 grupos.

## <a name="next-steps"></a>Pasos siguientes

Para el script completo que contiene los comandos que aparecen en este artículo, consulte el [repositorio de ejemplos de script de integración de Azure AD en el AKS][complete-script].

Para usar Azure AD usuarios y grupos para controlar el acceso a los recursos de clúster, consulte [controlar el acceso a recursos de clúster mediante el control de acceso basado en roles y las identidades de Azure AD en AKS][azure-ad-rbac].

Para obtener más información acerca de cómo proteger los clústeres de Kubernetes, consulte [opciones de identidad y acceso para AKS)][rbac-authorization].

Para ver recomendaciones de control de identidades y recursos, consulte [procedimientos recomendados para la autenticación y autorización en AKS][operator-best-practices-identity].

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[complete-script]: https://github.com/Azure-Samples/azure-cli-samples/tree/master/aks/azure-ad-integration/azure-ad-integration.sh

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-ad-app-create]: /cli/azure/ad/app#az-ad-app-create
[az-ad-app-update]: /cli/azure/ad/app#az-ad-app-update
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create
[az-ad-app-permission-add]: /cli/azure/ad/app/permission#az-ad-app-permission-add
[az-ad-app-permission-grant]: /cli/azure/ad/app/permission#az-ad-app-permission-grant
[az-ad-app-permission-admin-consent]: /cli/azure/ad/app/permission#az-ad-app-permission-admin-consent
[az-ad-app-show]: /cli/azure/ad/app#az-ad-app-show
[az-group-create]: /cli/azure/group#az-group-create
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-signed-in-user-show]: /cli/azure/ad/signed-in-user#az-ad-signed-in-user-show
[azure-ad-portal]: azure-ad-integration.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
