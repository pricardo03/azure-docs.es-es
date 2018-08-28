---
title: 'Tutorial de Kubernetes en Azure: Implementación de una aplicación'
description: En este tutorial de Azure Kubernetes Service (AKS), implemente una aplicación con varios contenedores en el clúster mediante una imagen personalizada que se almacena en Azure Container Registry.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bf817f553250ead449ec0d5db3d33acc2eff23f3
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2018
ms.locfileid: "41919553"
---
# <a name="tutorial-run-applications-in-azure-kubernetes-service-aks"></a>Tutorial: Ejecución de aplicaciones en Azure Kubernetes Service (AKS)

Kubernetes proporciona una plataforma distribuida para aplicaciones en contenedores. Compile e implemente sus propios servicios y aplicaciones en un clúster de Kubernetes, y deje que el clúster administre la disponibilidad y conectividad. En este tutorial, la cuarta parte de siete, se implementa una aplicación de ejemplo en un clúster de Kubernetes. Aprenderá a:

> [!div class="checklist"]
> * Actualizar los archivos de manifiesto de Kubernetes
> * Ejecutar una aplicación en Kubernetes
> * Prueba de la aplicación

En los tutoriales posteriores, esta aplicación se escala horizontalmente y se actualiza.

En este tutorial se da por hecho que tiene unos conocimientos básicos de los conceptos de Kubernetes. Para obtener información más detallada sobre esta aplicación, consulte la [documentación de Kubernetes][kubernetes-documentation].

## <a name="before-you-begin"></a>Antes de empezar

En tutoriales anteriores se empaquetó una aplicación en una imagen de contenedor, se cargó esta imagen en Azure Container Registry y se creó un clúster de Kubernetes.

Para completar este tutorial, necesita el archivo de manifiesto de Kubernetes `azure-vote-all-in-one-redis.yaml` creado previamente. Este archivo se descargó con el código fuente de la aplicación en un tutorial anterior. Confirme que ha clonado el repositorio y que ha cambiado los directorios en el repositorio clonado. Si no ha realizado estos pasos, pero desea continuar, vuelva al [tutorial 1: Creación de imágenes de contenedor][aks-tutorial-prepare-app].

Para realizar este tutorial es necesario disponer de la versión 2.0.44, o superior, de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="update-the-manifest-file"></a>Actualización del archivo de manifiesto

En estos tutoriales, una instancia de Azure Container Registry (ACR) almacena la imagen del contenedor de la aplicación de ejemplo. Para implementar la aplicación, debe actualizar el nombre de la imagen en el archivo de manifiesto de Kubernetes para incluir el nombre del servidor de inicio de sesión de ACR.

Para obtener el nombre del servidor de inicio de sesión de ACR se usa el comando [az acr list][az-acr-list], como se muestra a continuación:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

El archivo de manifiesto de ejemplo del repositorio git clonado en el primer tutorial usa el nombre de servidor de inicio de sesión *microsoft*. Abra este archivo de manifiesto con un editor de texto, como `vi`:

```console
vi azure-vote-all-in-one-redis.yaml
```

Reemplace *microsoft* por el nombre del servidor de inicio de sesión de ACR. El nombre de la imagen se encuentra en la línea 47 del archivo de manifiesto. En el ejemplo siguiente se muestra el nombre de imagen predeterminado:

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Especifique su propio nombre del servidor de inicio de sesión de ACR para que el archivo de manifiesto sea parecido al del ejemplo siguiente:

```yaml
containers:
- name: azure-vote-front
  image: <acrName>.azurecr.io/azure-vote-front:v1
```

Guarde y cierre el archivo.

## <a name="deploy-the-application"></a>Implementación de la aplicación

Para implementar la aplicación, use el comando [kubectl apply][kubectl-apply]. Este comando analiza el archivo de manifiesto y crea los objetos de Kubernetes definidos. Especifique el archivo de manifiesto de ejemplo, como se muestra en el ejemplo siguiente:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Los objetos de Kubernetes se crean en el clúster, como se muestra en el ejemplo siguiente:

```
$ kubectl apply -f azure-vote-all-in-one-redis.yaml

deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Prueba de la aplicación

Se crea un [servicio de Kubernetes][kubernetes-service] que expone la aplicación a Internet. Este proceso puede tardar unos minutos. Para supervisar el progreso, utilice el comando [kubectl get service][kubectl-get] con el argumento `--watch`:

```console
kubectl get service azure-vote-front --watch
```

La *IP EXTERNA* del servicio *azure-vote-front* aparece inicialmente como *pendiente*, como se muestra en el siguiente ejemplo:

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Cuando la dirección *IP EXTERNA* cambie de *pendiente* a una dirección IP pública real, use `CTRL-C` para detener el proceso de inspección de kubectl. En el siguiente ejemplo se muestra una dirección IP que está ya asignada:

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Para ver la aplicación en acción, abra un explorador web en la dirección IP externa.

![Imagen del clúster de Kubernetes en Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

Si no se cargó la aplicación, esto se puede deber a un problema de autorización con el registro de la imagen. Para ver el estado de los contenedores, use el comando `kubectl get pods`. Si no se pueden extraer las imágenes del contenedor, consulte [permitir el acceso a Container Registry con un secreto de Kubernetes](https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks#access-with-kubernetes-secret).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, se ha implementado la aplicación Azure Vote en un clúster de Kubernetes en AKS. Ha aprendido a:

> [!div class="checklist"]
> * Actualizar los archivos de manifiesto de Kubernetes
> * Ejecutar una aplicación en Kubernetes
> * Probar la aplicación

Pase al siguiente tutorial para aprender a escalar una aplicación Kubernetes y la infraestructura de Kubernetes subyacente.

> [!div class="nextstepaction"]
> [Escalar infraestructura y aplicación de Kubernetes][aks-tutorial-scale]

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list
[azure-cli-install]: /cli/azure/install-azure-cli
