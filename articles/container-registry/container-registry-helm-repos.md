---
title: Usar repositorios de Helm en Azure Container Registry
description: Obtenga información sobre cómo usar un repositorio de Helm con Azure Container Registry para almacenar los gráficos de las aplicaciones
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 09/24/2018
ms.author: iainfou
ms.openlocfilehash: 2135a3a5a8f14cf6c2e7fd2984d9b221e2445c1d
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68309509"
---
# <a name="use-azure-container-registry-as-a-helm-repository-for-your-application-charts"></a>Usar Azure Container Registry como un repositorio de Helm para los gráficos de la aplicación

Para administrar e implementar rápidamente aplicaciones de Kubernetes, puede usar el [administrador de paquetes de código abierto de Helm][helm]. Con Helm, las aplicaciones se definen como *gráficos* que se almacenan en un repositorio de gráficos de Helm. Estos gráficos definen configuraciones y dependencias, y pueden tener varias versiones en todo el ciclo de vida de la aplicación. Azure Container Registry puede usarse como el host para repositorios de gráficos de Helm.

Con Azure Container Registry, tiene un repositorio gráfico de Helm seguro y privado que puede integrarse con las canalizaciones de compilación u otros servicios de Azure. Los repositorios de gráficos de Helm en Azure Container Registry incluyen características de replicación geográfica para mantener los gráficos cerca de las implementaciones y por motivos de redundancia. Solo paga por el almacenamiento de los gráficos usados y están disponibles en todos los niveles de precios de Azure Container Registry.

Este artículo muestra cómo usar un repositorio de gráficos de Helm almacenado en Azure Container Registry.

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos de este artículo, deben satisfacerse los siguientes requisitos previos:

- **Azure Container Registry**: cree un registro de contenedor en la suscripción de Azure. Por ejemplo, use [Azure Portal](container-registry-get-started-portal.md) o la [CLI de Azure](container-registry-get-started-azure-cli.md).
- **Versión de cliente Helm 2.11.0 (no una versión RC) o versión posterior**: ejecute `helm version` para buscar la versión actual. También necesitará un servidor de Helm (Tiller) inicializado dentro de un clúster de Kubernetes. Si es necesario, puede [crear un clúster de Azure Kubernetes Service][aks-quickstart]. Para más información sobre cómo instalar y usar Helm, consulte [Instalación de Helm][helm-install].
- **CLI de Azure versión 2.0.46 o posterior**: ejecute `az --version` para buscar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

## <a name="add-a-repository-to-helm-client"></a>Agregar un repositorio al cliente de Helm

Un repositorio de Helm es un servidor HTTP que puede almacenar gráficos de Helm. Azure Container Registry puede proporcionar este almacenamiento para gráficos de Helm y administrar la definición del índice a medida que agrega y quita gráficos en el repositorio.

Use la CLI de Azure para agregar Azure Container Registry como un repositorio de gráficos de Helm. Con este enfoque, el cliente de Helm se actualiza con el URI y las credenciales para el repositorio respaldado por Azure Container Registry. No es necesario especificar manualmente esta información del repositorio, por lo que las credenciales no se exponen en el historial de comandos, por ejemplo.

Si es necesario, inicie sesión en la CLI de Azure y siga las indicaciones:

```azurecli
az login
```

Configure los valores predeterminados de la CLI de Azure con el nombre de su Azure Container Registry mediante el comando [az configure][az-configure]. En el siguiente ejemplo, reemplace `<acrName>` con el nombre del registro:

```azurecli
az configure --defaults acr=<acrName>
```

Ahora agregue el repositorio de gráficos de Helm de Azure Container Registry al cliente de Helm mediante el comando [az acr helm repo add][az-acr-helm-repo-add]. Este comando obtiene un token de autenticación del Azure Container Registry que usa el cliente de Helm. El token de autenticación tiene una validez de 1 hora. De forma similar a `docker login`, puede ejecutar este comando en sesiones futuras de la CLI para autenticar el cliente de Helm con el repositorio de gráficos de Helm de Azure Container Registry:

```azurecli
az acr helm repo add
```

## <a name="add-a-chart-to-the-repository"></a>Agregar un gráfico al repositorio

En este artículo, vamos a obtener un gráfico de Helm existente desde el repositorio *stable* público de Helm. El repositorio *stable* es un repositorio público mantenido que incluye gráficos de aplicación comunes. Los mantenedores de paquetes pueden enviar sus gráficos al repositorio *stable*, de la misma manera que Docker Hub proporciona un registro público de imágenes de contenedores comunes. El gráfico descargado del repositorio *stable* público puede insertarse en el repositorio de Azure Container Registry privado. En la mayoría de los escenarios, creará y cargará sus propios gráficos para las aplicaciones que desarrolla. Para obtener más información sobre cómo crear sus propios gráficos de Helm, consulte cómo [desarrollar gráficos de Helm][develop-helm-charts].

En primer lugar, cree un directorio en *~/acr-helm*, después descargue el gráfico *stable/wordpress* existente:

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm fetch stable/wordpress
```

Muestre el gráfico descargado y anote la versión de Wordpress que se incluye en el nombre de archivo. El comando `helm fetch stable/wordpress` no ha especificado una versión determinada, por lo que se capturó la versión *más reciente*. Todos los gráficos de Helm incluyen un número de versión en el nombre de archivo que sigue el estándar [SemVer 2][semver2]. En la siguiente salida de ejemplo, el gráfico de Wordpress es la versión *2.1.10*:

```
$ ls

wordpress-2.1.10.tgz
```

Ahora inserte el gráfico en el repositorio de gráficos de Helm en Azure Container Registry mediante el comando [az acr helm push][az-acr-helm-push] de la CLI de Azure. Especifique el nombre de su gráfico de Helm descargado en el paso anterior, como *wordpress-2.1.10.tgz*:

```azurecli
az acr helm push wordpress-2.1.10.tgz
```

Transcurridos unos instantes, la CLI de Azure notifica que se ha guardado el gráfico, como se muestra en la siguiente salida de ejemplo:

```
$ az acr helm push wordpress-2.1.10.tgz

{
  "saved": true
}
```

## <a name="list-charts-in-the-repository"></a>Lista de gráficos en el repositorio

El cliente Helm mantiene una copia en caché local del contenido de los repositorios remotos. Los cambios realizados en un repositorio remoto no actualizan automáticamente la lista de gráficos disponibles conocidos localmente por el cliente de Helm. Al realizar una búsqueda de gráficos en los repositorios, Helm usa su índice almacenado en caché local. Para usar el gráfico cargado en el paso anterior, se debe actualizar el índice de repositorios de Helm local. Puede volver a indexar los repositorios en el cliente Helm o usar la CLI de Azure para actualizar el índice de repositorios. Este paso debe completarse cada vez que agregue un gráfico al repositorio:

```azurecli
az acr helm repo add
```

Con un gráfico almacenado en el repositorio y el índice actualizado disponible localmente, puede usar los comandos de cliente de Helm regulares para buscar o instalar. Para ver todos los gráficos en el repositorio, use `helm search <acrName>`. Especifique el nombre de su Azure Container Registry:

```console
helm search <acrName>
```

Se muestra el gráfico de Wordpress que se insertó en el paso anterior, como se muestra en la siguiente salida de ejemplo:

```
$ helm search myacrhelm

NAME                CHART VERSION   APP VERSION DESCRIPTION
helmdocs/wordpress  2.1.10          4.9.8       Web publishing platform for building blogs and websites.
```

También puede enumerar los gráficos con la CLI de Azure mediante [az acr helm list][az-acr-helm-list]:

```azurecli
az acr helm list
```

## <a name="show-information-for-a-helm-chart"></a>Mostrar la información de un gráfico de Helm

Para ver información de un gráfico específico en el repositorio, puede usar nuevamente el cliente de Helm normal. Para ver información para el gráfico denominado *wordpress*, utilice `helm inspect`.

```console
helm inspect <acrName>/wordpress
```

Cuando no se proporciona ningún número de versión, se usa la versión *más reciente*. Helm devuelve información detallada sobre el gráfico, tal como se muestra en la siguiente salida de ejemplo condensada:

```
$ helm inspect myacrhelm/wordpress

appVersion: 4.9.8
description: Web publishing platform for building blogs and websites.
engine: gotpl
home: https://www.wordpress.com/
icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
keywords:
- wordpress
- cms
- blog
- http
- web
- application
- php
maintainers:
- email: containers@bitnami.com
  name: bitnami-bot
name: wordpress
sources:
- https://github.com/bitnami/bitnami-docker-wordpress
version: 2.1.10
[...]
```

También puede mostrar la información de un gráfico con el comando [az acr helm show][az-acr-helm-show] de la CLI de Azure. Nuevamente, de forma predeterminada se devuelve la versión *más reciente* de un gráfico. Puede anexar `--version` para mostrar una versión específica de un gráfico, como *2.1.10*:

```azurecli
az acr helm show wordpress
```

## <a name="install-a-helm-chart-from-the-repository"></a>Instalar un gráfico de Helm desde el repositorio

El gráfico de Helm en el repositorio se instala especificando el nombre del repositorio y después el nombre del gráfico. Use el cliente de Helm para instalar el gráfico de Wordpress:

```console
helm install <acrName>/wordpress
```

> [!TIP]
> Si inserta en el repositorio de gráfico de Helm de Azure Container Registry y regresa posteriormente en una nueva sesión CLI, el cliente de Helm local necesita un token de autenticación actualizado. Para obtener un nuevo token de autenticación, use el comando [az acr helm repo add][az-acr-helm-repo-add].

Durante el proceso de instalación deben completarse los siguientes pasos:

- El cliente de Helm busca el índice del repositorio local.
- El gráfico correspondiente se descarga desde el repositorio de Azure Container Registry.
- El gráfico se implementa mediante el Tiller en el clúster de Kubernetes.

En la siguiente salida de ejemplo condensada se muestran los recursos de Kubernetes implementados mediante el gráfico de Helm:

```
$ helm install myacrhelm/wordpress

NAME:   irreverent-jaguar
LAST DEPLOYED: Thu Sep 13 21:44:20 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                          READY  STATUS   RESTARTS  AGE
irreverent-jaguar-wordpress-7ff46d9b8c-b7v6m  0/1    Pending  0         1s
irreverent-jaguar-mariadb-0                   0/1    Pending  0         1s
[...]
```

## <a name="delete-a-helm-chart-from-the-repository"></a>Eliminar un gráfico de Helm desde el repositorio

Para eliminar un gráfico desde el repositorio, use el comando [az acr helm delete][az-acr-helm-delete]. Especifique el nombre del gráfico, como *wordpress*, y la versión que desea eliminar, como *2.1.10*.

```azurecli
az acr helm delete wordpress --version 2.1.10
```

Si desea eliminar todas las versiones del gráfico con nombre, omita el parámetro `--version`.

Continúa el gráfico sigue devolviéndose en `helm search <acrName>`. Nuevamente, el cliente de Helm no actualiza automáticamente la lista de gráficos disponibles en un repositorio. Para actualizar el índice de repositorio del cliente de Helm, use de nuevo el comando [az acr helm repo add][az-acr-helm-repo-add]:

```azurecli
az acr helm repo add
```

## <a name="next-steps"></a>Pasos siguientes

Este artículo usó un gráfico de Helm existente del repositorio *stable* público. Para obtener más información sobre cómo crear e implementar sus propios gráficos de Helm, consulte cómo [desarrollar gráficos de Helm][develop-helm-charts].

Los gráficos de helm pueden usarse como parte del proceso de compilación de contenedor. Para más información, consulte cómo [usar Azure Container Registry Tasks][acr-tasks].

Para más información sobre cómo usar y administrar Azure Container Registry, consulte los [procedimientos recomendados][acr-bestpractices].

<!-- LINKS - external -->
[helm]: https://helm.sh/
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[develop-helm-charts]: https://docs.helm.sh/developing_charts/
[semver2]: https://semver.org/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-quickstart]: ../aks/kubernetes-walkthrough.md
[acr-bestpractices]: container-registry-best-practices.md
[az-configure]: /cli/azure/reference-index#az-configure
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-helm-repo-add]: /cli/azure/acr/helm/repo#az-acr-helm-repo-add
[az-acr-helm-push]: /cli/azure/acr/helm#az-acr-helm-push
[az-acr-helm-list]: /cli/azure/acr/helm#az-acr-helm-list
[az-acr-helm-show]: /cli/azure/acr/helm#az-acr-helm-show
[az-acr-helm-delete]: /cli/azure/acr/helm#az-acr-helm-delete
[acr-tasks]: container-registry-tasks-overview.md
