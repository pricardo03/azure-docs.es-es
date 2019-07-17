---
title: Implementación de una aplicación con un archivo Dockerfile en Kubernetes mediante Azure Dev Spaces
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: quickstart
description: Implementación de un microservicio con un archivo Dockerfile en AKS con Azure Dev Spaces
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s
manager: gwallace
ms.openlocfilehash: a272047955564ba745d4cf521e560af9d24cfb9e
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710698"
---
# <a name="quickstart-develop-an-application-with-a-dockerfile-on-kubernetes-using-azure-dev-spaces"></a>Inicio rápido: Desarrollo de una aplicación con un archivo Dockerfile en Kubernetes mediante Azure Dev Spaces
En esta guía, aprenderá a:

- Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
- Desarrollar y ejecutar código de contenedores mediante la línea de comandos.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).
- [La CLI de Azure instalada](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Creación de un clúster de Azure Kubernetes Service

Debe crear un clúster de AKS en una [región admitida][supported-regions]. Los siguientes comandos permiten crear un grupo de recursos llamado *MyResourceGroup* y un clúster de AKS denominado *MyAKS*.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --disable-rbac --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Habilitar Azure Dev Spaces en el clúster de AKS

Use el comando `use-dev-spaces` para habilitar Dev Spaces en el clúster de AKS y siga las indicaciones. El siguiente comando permite habilitar Dev Spaces en el clúster *MyAKS* del grupo *MyResourceGroup* y crea un espacio de desarrollo *predeterminado*.

```cmd
$ az aks use-dev-spaces -g MyResourceGroup -n MyAKS

'An Azure Dev Spaces Controller' will be created that targets resource 'MyAKS' in resource group 'MyResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'MyAKS' in resource group 'MyResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

## <a name="get-sample-application-code"></a>Obtención del código de la aplicación de ejemplo

En este artículo, puede usar la [aplicación de ejemplo de Azure Dev Spaces](https://github.com/Azure/dev-spaces) para demostrar el uso de Azure Dev Spaces. La aplicación de ejemplo está escrita en Go, pero puede usar el lenguaje que desee siempre y cuando proporcione un archivo Dockerfile válido para incluir la aplicación en un contenedor y ejecutarla en AKS.

Clone la aplicación desde GitHub y vaya al directorio *dev-spaces/samples/golang/getting-started/webfrontend*:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/golang/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Preparar la aplicación

Para ejecutar la aplicación en Azure Dev Spaces, necesita un archivo Dockerfile y un gráfico de Helm. Para algunos lenguajes, como [Java][java-quickstart], [.NET core][netcore-quickstart] y [Node.js][nodejs-quickstart], las herramientas de cliente de Azure Dev Spaces pueden generar todos los recursos que necesita. Para muchos otros lenguajes como Go, PHP y Python, las herramientas de cliente podrán generar el gráfico de Helm siempre y cuando pueda proporcionar un archivo Dockerfile válido.

La aplicación de ejemplo incluye un archivo Dockerfile válido. Para generar los recursos del gráfico de Helm para ejecutar la aplicación en Kubernetes, use el comando `azds prep`:

```cmd
azds prep --public
```

Debe ejecutar el comando `prep` desde el directorio *dev-spaces/samples/golang/getting-started/webfrontend* para generar correctamente el gráfico de Helm.

# <a name="build-and-run-code-in-kubernetes"></a>Compilación y ejecución de código en Kubernetes

Compile y ejecute el código en AKS mediante el comando `azds up`:

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...3s
Installing Helm chart...2s
Waiting for container image build...39s
Building container image...
Step 1/7 : FROM golang:1.10
Step 2/7 : EXPOSE 80
Step 3/7 : WORKDIR /go/src
Step 4/7 : COPY src .
Step 5/7 : WORKDIR /go/src/app
Step 6/7 : RUN go install app
Step 7/7 : ENTRYPOINT /go/bin/app
Built container image in 41s
Waiting for container...13s
Service 'webfrontend' port 'http' is available at http://default.webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available via port forwarding at http://localhost:50199
...
```

Para ver el servicio en ejecución, abra la dirección URL pública y vaya a la ruta de acceso `/api`. La dirección URL pública aparece en la salida del comando `azds up`. En este ejemplo, la dirección URL pública es `http://default.webfrontend.1234567890abcdef1234.eus.azds.io/` y deberá ir a `http://default.webfrontend.1234567890abcdef1234.eus.azds.io/api`.

Si detiene el comando `azds up` mediante *Ctrl + c*, el servicio continuará ejecutándose en AKS y la dirección URL pública seguirá estando disponible.

## <a name="update-code"></a>Actualización del código

Para implementar una versión actualizada del servicio, puede actualizar cualquier archivo del proyecto y volver a ejecutar el comando `azds up`. Por ejemplo:

1. Si `azds up` se está todavía ejecutando, presione *Ctrl + c*.
1. Actualice la [línea 29 en `src/app/main.go`](https://github.com/Azure/dev-spaces/blob/master/samples/golang/getting-started/webfrontend/src/app/main.go#L29) a:
    
    ```golang
        fmt.Fprintf(w, "Hello from webfrontend in Azure")
    ```

1. Guarde los cambios.
1. Vuelva a ejecutar el comando `azds up`:

    ```cmd
    $ azds up
    Using dev space 'default' with target 'MyAKS'
    Synchronizing files...1s
    Installing Helm chart...3s
    Waiting for container image build...
    ...    
    ```

1. Vaya al servicio en ejecución y observe los cambios.
1. Presione *Ctrl + C* para detener el comando `azds up`.

## <a name="clean-up-your-azure-resources"></a>Limpieza de los recursos de Azure

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de la forma en que Azure Dev Spaces le ayuda a desarrollar aplicaciones más complejas en varios contenedores y cómo puede simplificar el desarrollo en colaboración mediante el uso de distintas versiones o bifurcaciones del código en distintos espacios.

> [!div class="nextstepaction"]
> [Desarrollo en equipo en Azure Dev Spaces][team-quickstart]


[java-quickstart]: quickstart-java.md
[nodejs-quickstart]: quickstart-nodejs.md
[netcore-quickstart]: quickstart-netcore.md
[team-quickstart]: quickstart-team-development.md
[supported-regions]: about.md#supported-regions-and-configurations