---
title: Cómo usar un repositorio de Helm privado en Azure Dev Spaces
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 08/22/2019
ms.topic: conceptual
description: Use un repositorio de Helm privado en una instancia de Azure Dev Spaces.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, contenedores, Helm
manager: gwallace
ms.openlocfilehash: d0f2cb739a502d614ac329eef1fc57b2fb15cb38
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70014279"
---
# <a name="use-a-private-helm-repository-in-azure-dev-spaces"></a>Uso de un repositorio de Helm privado en Azure Dev Spaces

[Helm][helm] es un administrador de paquetes para Kuberentes. Helm usa un formato de [gráfico][helm-chart] para empaquetar las dependencias. Los gráficos de Helm se almacenan en un repositorio, que puede ser público o privado. Azure Dev Spaces solo recupera los gráficos de Helm de los repositorios públicos cuando se ejecuta la aplicación. En los casos en los que el repositorio Helm sea privado o Azure Dev Spaces no pueda acceder a él, puede agregar un gráfico desde ese repositorio directamente a la aplicación. Agregar directamente el gráfico permite a Azure Dev Spaces ejecutar la aplicación sin tener que acceder al repositorio privado de Helm.

## <a name="add-the-private-helm-repository-to-your-local-machine"></a>Incorporación del repositorio de Helm privado al equipo local

Utilice [helm repo add][helm-repo-add] y [helm repo update][helm-repo-update] para acceder al repositorio privado de Helm desde el equipo local.

```cmd
helm repo add privateRepoName http://example.com/helm/v1/repo --username user --password 5tr0ng_P@ssw0rd!
helm repo update
```

## <a name="add-the-chart-to-your-application"></a>Incorporación del carro a la aplicación

Vaya al directorio del proyecto y ejecute `azds prep`.

```cmd
azds prep --public
```

Cree un archivo [requirements.yaml][helm-requirements] con el gráfico en el directorio del gráfico de la aplicación. Por ejemplo, si la aplicación se denomina *app1*, crearía *charts/app1/requirements.yaml*.

```yaml
dependencies:
    - name: mychart
      version: 0.1.0
      repository:  http://example.com/helm/v1/repo
```

Navegue al directorio del gráfico de la aplicación y use [helm dependency update][helm-dependency-update] para actualizar las dependencias de Helm para la aplicación y descargar el gráfico desde el repositorio privado.

```cmd
helm dependency update
```

Compruebe que se ha agregado un subdirectorio *charts* con un archivo *tgz* al directorio de gráficos de la aplicación. Por ejemplo, *charts/app1/charts/mychart-0.1.0.tgz*.

El gráfico del repositorio privado de Helm se ha descargado y agregado al proyecto. Quite el archivo *requirements.yaml* para que Dev Spaces no intente actualizar esta dependencia.

## <a name="run-your-application"></a>Ejecución de la aplicación

Vaya al directorio raíz del proyecto y ejecute `azds up` para comprobar que la aplicación se ejecuta correctamente en el espacio de desarrollo.

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...2s
Installing Helm chart...2s
Waiting for container image build...2m 25s
Building container image...
...
Service 'app1' port 'http' is available at http://app1.1234567890abcdef1234.eus.azds.io/
Service 'app1' port 80 (http) is available at http://localhost:54256
...
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [Helm y cómo funciona][helm].

[helm]: https://docs.helm.sh
[helm-chart]: https://helm.sh/docs/developing_charts/
[helm-dependency-update]: https://helm.sh/docs/helm/#helm-dependency-update
[helm-repo-add]: https://helm.sh/docs/helm/#helm-repo-add
[helm-repo-update]: https://helm.sh/docs/helm/#helm-repo-update
[helm-requirements]: https://helm.sh/docs/developing_charts/#chart-dependencies