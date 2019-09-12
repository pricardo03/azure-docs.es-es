---
title: Integración de Azure Container Registry con Azure Kubernetes Service
description: Obtenga información sobre cómo integrar Azure Kubernetes Service (AKS) con Azure Container Registry (ACR).
services: container-service
author: mlearned
manager: gwallace
ms.service: container-service
ms.topic: article
ms.date: 08/15/2018
ms.author: mlearned
ms.openlocfilehash: 3c11367945b74db9be20ade86c7bc26901440e4d
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70305165"
---
# <a name="preview---authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Versión preliminar: autenticación con Azure Container Registry desde Azure Kubernetes Service

Cuando se usa Azure Container Registry (ACR) con Azure Kubernetes Service (AKS), es preciso establecer un mecanismo de autenticación. En este documento se detallan las configuraciones recomendadas para la autenticación entre estos dos servicios de Azure.

Puede configurar la integración de AKS en ACR con unos pocos comandos sencillos con la CLI de Azure.

> [!IMPORTANT]
> Las características en vista previa de AKS son de autoservicio y se tienen que habilitar. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los contratos de nivel de servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para obtener información adicional, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS](support-policies.md)
> * [Preguntas más frecuentes de soporte técnico de Azure](faq.md)

## <a name="before-you-begin"></a>Antes de empezar

Debe tener lo siguiente:

* Rol de **propietario** o **administrador de cuenta de Azure** en la **suscripción de Azure**.
* También necesita la versión 2.0.70 de la CLI de Azure u otra versión posterior y la extensión 0.4.8 de la versión preliminar de AKS.
* Necesita tener [Docker instalado](https://docs.docker.com/install/) en el cliente y disponer de acceso a [Docker Hub](https://hub.docker.com/).

## <a name="install-latest-aks-cli-preview-extension"></a>Instalación de la extensión de la versión preliminar de la CLI de AKS más reciente

Necesita la extensión **0.4.13 de la versión preliminar de AKS** o posterior.

```azurecli
az extension remove --name aks-preview 
az extension add -y --name aks-preview
```

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>Creación de un nuevo clúster de AKS con integración de ACR

Puede configurar la integración de AKS y ACR durante la creación inicial del clúster de AKS.  Para permitir que un clúster de AKS interactúe con ACR, se usa una **entidad de servicio** de Azure Active Directory. El siguiente comando CLI permite autorizar un ACR existente en su suscripción y configura el rol **ACRPull** adecuado para la entidad de servicio. Proporcione valores válidos para los parámetros siguientes.  Los parámetros entre corchetes son opcionales.
```azurecli
az login
az acr create -n myContainerRegistry -g myContainerRegistryResourceGroup --sku basic [in case you do not have an existing ACR]
az aks create -n myAKSCluster -g myResourceGroup --attach-acr <acr-name-or-resource-id>
```
** Un identificador de recursos de ACR tiene el formato siguiente: 

/subscriptions/<subscription-d>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/{name} 
  
Este paso puede tardar varios minutos en completarse.

## <a name="configure-acr-integration-for-existing-aks-clusters"></a>Configuración de la integración de ACR para clústeres de AKS existentes

Para integrar un ACR existente con clústeres de AKS existentes, proporcione valores válidos para **acr-name** o **acr-resource-id**, como se indica a continuación.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

También puede quitar la integración entre un grupo de ACR y un clúster de AKS con lo siguiente
```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```


## <a name="log-in-to-your-acr"></a>Inicio de sesión en ACR

Para iniciar sesión en ACR, use el comando siguiente.  Reemplace el parámetro <acrname> por el nombre de ACR.  Por ejemplo, el valor predeterminado es **aks<suGrupoDeRecursos>acr**.

```azurecli
az acr login -n <acrName>
```

## <a name="pull-an-image-from-docker-hub-and-push-to-your-acr"></a>Extracción de una imagen de Docker Hub e inserción en ACR

Extraiga una imagen de Docker Hub, etiquete la imagen e insértela en ACR.

```console
acrloginservername=$(az acr show -n <acrname> -g <myResourceGroup> --query loginServer -o tsv)
docker pull nginx
```

```
$ docker tag nginx $acrloginservername/nginx:v1
$ docker push $acrloginservername/nginx:v1

The push refers to repository [someacr1.azurecr.io/nginx]
fe6a7a3b3f27: Pushed
d0673244f7d4: Pushed
d8a33133e477: Pushed
v1: digest: sha256:dc85890ba9763fe38b178b337d4ccc802874afe3c02e6c98c304f65b08af958f size: 948
```

## <a name="update-the-state-and-verify-pods"></a>Actualización del estado y comprobación de los pods

Siga estos pasos para comprobar la implementación.

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

Vea el archivo YAML y edite la propiedad de imagen reemplazando el valor por el servidor de inicio de sesión, la imagen y la etiqueta de ACR.

```
$ cat acr-nginx.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx0-deployment
  labels:
    app: nginx0-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx0
  template:
    metadata:
      labels:
        app: nginx0
    spec:
      containers:
      - name: nginx
        image: <replace this image property with you acr login server, image and tag>
        ports:
        - containerPort: 80

$ kubectl apply -f acr-nginx.yaml
$ kubectl get pods

You should have two running pods.

NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

<!-- LINKS - external -->
[AKS AKS CLI]:  https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
