---
title: 'Tutorial: Automatización de compilaciones de imágenes de contenedor en la actualización de imagen base: Azure Container Registry Tasks'
description: En este tutorial, aprenderá a configurar una tarea de Azure Container Registry que desencadena automáticamente compilaciones de imágenes de contenedor en la nube cuando se actualiza una imagen base.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: c89a239cd3abbdd59813626f4b64596ee8a1fd7e
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55756808"
---
# <a name="tutorial-automate-container-image-builds-when-a-base-image-is-updated-in-an-azure-container-registry"></a>Tutorial: Automatización de compilaciones de imágenes de contenedor al actualizarse una imagen base en una instancia de Azure Container Registry 

ACR Tasks admite la ejecución automatizada de compilaciones cuando se actualiza una imagen base del contenedor, como cuando se revisa el sistema operativo o el marco de trabajo de la aplicación de una de las imágenes base. En este tutorial, aprenderá a crear una tarea en ACR Tasks que desencadena una compilación en la nube cuando se inserta una imagen base de contenedor en el registro.

En este tutorial, el último de la serie, se realizan las siguientes operaciones:

> [!div class="checklist"]
> * Compilación de la imagen base
> * Creación de una tarea de compilación de la imagen de aplicación
> * Actualización de la imagen base para que desencadene una tarea de imagen de aplicación
> * Mostrar la tarea desencadenada
> * Comprobación de la imagen de aplicación actualizada

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si quiere usar la CLI de Azure de forma local, debe tener la versión **2.0.46** de la CLI de Azure u otra posterior instalada. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli].

## <a name="prerequisites"></a>Requisitos previos

### <a name="complete-the-previous-tutorials"></a>Complete los tutoriales anteriores.

En este tutorial se da por supuesto que ya ha completado los pasos de los dos primeros tutoriales de la serie, en los cuales puede realizar la:

* Creación de una instancia de Azure Container Registry
* Bifurcación del repositorio de ejemplo
* Clonación del repositorio de ejemplo
* Creación de un token de acceso personal de GitHub

Si aún no lo ha hecho, complete los dos primeros tutoriales antes de continuar:

[Compilación de imágenes de contenedor en la nube con Azure Container Registry Tasks](container-registry-tutorial-quick-task.md)

[Automatización de compilaciones de imágenes de contenedor con Azure Container Registry Tasks](container-registry-tutorial-build-task.md)

### <a name="configure-the-environment"></a>Configuración del entorno

Rellene estas variables de entorno de shell con valores adecuados para el entorno. Este paso no es estrictamente necesario, pero hace que la ejecución de los comandos de varias líneas de la CLI de Azure en este tutorial sea un poco más fácil. Si no rellena estas variables de entorno, deberá reemplazar manualmente cada valor siempre que aparezcan en los comandos de ejemplo.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Imágenes base

Los archivos de Docker que definen la mayoría de las imágenes de contenedor especifican una imagen primaria en la que se basan y que se denomina su *imagen base*. Las imágenes base normalmente contienen el sistema operativo, por ejemplo [Alpine Linux][base-alpine] o [Windows Nano Server][base-windows], sobre el que se aplica el resto de capas del contenedor. También pueden incluir marcos de trabajo de la aplicación como [Node.js][base-node] o [.NET Core][base-dotnet].

### <a name="base-image-updates"></a>Actualizaciones de imagen base

El mantenedor de la imagen actualiza frecuentemente la imagen base para que incluya nuevas características o mejoras en el sistema operativo o plataforma de la imagen. Las revisiones de seguridad son otra causa habitual de una actualización de la imagen base.

Cuando se actualiza una imagen base, se le presenta la necesidad de recompilar todas las imágenes de contenedor del registro que se basan en ella para que incluya las nuevas características y correcciones. ACR Tasks incluye la posibilidad de compilar automáticamente las imágenes cuando se actualiza una imagen base del contenedor.

### <a name="base-image-update-scenario"></a>Escenario de actualización de imagen base

Este tutorial le guía a través de un escenario de actualización de imagen base. El [código de ejemplo][code-sample] incluye dos archivos de Docker: una imagen de aplicación y una imagen que especifica como su base. En las secciones siguientes creará una tarea de ACR Tasks que desencadena automáticamente una compilación de la imagen de aplicación cuando se inserta una nueva versión de la imagen base en el registro de contenedor.

[Dockerfile-app][dockerfile-app]: una pequeña aplicación web de Node.js que representa una página web estática que muestra la versión de Node.js en la que se basa. Se simula la cadena de versión: se muestra el contenido de una variable de entorno, `NODE_VERSION`, que se definió en la imagen base.

[Dockerfile-base][dockerfile-base]: la imagen que `Dockerfile-app` especifica como su base. Está basada en una imagen de [Node][base-node] e incluye la variable de entorno `NODE_VERSION`.

En las secciones siguientes, va a crear una tarea, actualizar el valor `NODE_VERSION` en el archivo de Docker de la imagen base y, finalmente, a usar ACR Tasks para compilar la imagen base. Cuando ACR Tasks inserta la nueva imagen base en el registro, este desencadena automáticamente una compilación de la imagen de aplicación. Si lo desea, puede ejecutar la imagen de contenedor de la aplicación localmente para ver las diferentes cadenas de versión de las imágenes compiladas.

## <a name="build-the-base-image"></a>Compilación de la imagen base

Empiece por crear la imagen base con una *tarea rápida* de ACR Tasks. Como se ha descrito en el [primer tutorial](container-registry-tutorial-quick-task.md) de la serie, este proceso no solo sirve para compilar la imagen, si no también para insertarla en el registro de contenedor si la compilación es correcta.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-a-task"></a>Crea una tarea.

A continuación, cree una tarea con [az acr task create][az-acr-task-create]:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

> [!IMPORTANT]
> Si ha creado anteriormente tareas durante la vista previa con el comando `az acr build-task`, deberá volver a crear esas tareas mediante el comando [az acr task][az-acr-task].

Esta tarea es similar a la tarea rápida creada en el [tutorial anterior](container-registry-tutorial-build-task.md). Indica a ACR Tasks que desencadene una compilación de la imagen cuando se inserten confirmaciones en el repositorio especificado por `--context`.

La diferencia estriba en su comportamiento, ya que también desencadena una compilación de la imagen cuando se actualiza su *imagen base*. El archivo de Docker especificado por el argumento `--file`, [Dockerfile-app][dockerfile-app], admite la especificación de una imagen del mismo registro que su imagen base:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Cuando se ejecuta una tarea, ACR Tasks detecta las dependencias de una imagen. Si la imagen base especificada en la instrucción `FROM` reside en el mismo registro o repositorio público de Docker Hub, esta agrega un enlace para asegurarse de que esta imagen se recompila siempre que se actualiza su base.

## <a name="build-the-application-container"></a>Compilación del contenedor de aplicación

Para habilitar ACR Tasks para que determine y realice el seguimiento de las dependencias de una imagen de contenedor, lo cual incluye su imagen base, **debe** primero desencadenar su tarea **al menos una vez**.

Use [az acr task run][az-acr-task-run] para desencadenar manualmente la tarea y compilar la imagen de aplicación:

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

Una vez finalizada la tarea, tome nota del **identificador de ejecución** (por ejemplo, "da6") si desea completar el siguiente paso opcional.

### <a name="optional-run-application-container-locally"></a>Opcional: Ejecutar contenedor de aplicación localmente

Si está trabajando de forma local (no en Cloud Shell), y tiene Docker instalado, ejecute el contenedor para ver la aplicación representada en un explorador web antes de recompilar su imagen base. Si va a usar Cloud Shell, omita esta sección (Cloud Shell no admite `az acr login` ni `docker run`).

En primer lugar, inicie sesión en el registro de contenedor con [az acr login][az-acr-login]:

```azurecli
az acr login --name $ACR_NAME
```

Ahora, ejecute el contenedor localmente con `docker run`. Reemplace **\<run-id\>** por el identificador de ejecución que se encuentra en la salida del paso anterior (por ejemplo, "da6").

```azurecli
docker run -d -p 8080:80 $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Vaya a http://localhost:8080 en el explorador, allí verá el número de versión de Node.js representado en la página web de manera parecida a la siguiente. En un paso posterior, cambiará la versión agregando una "a" a la cadena de versión.

![Captura de pantalla de una aplicación de ejemplo representada en el explorador][base-update-01]

## <a name="list-the-builds"></a>Lista de las compilaciones

A continuación, muestre las tareas que ACR Tasks ha completado para el registro mediante el comando [az acr task list-runs][az-acr-task-list-runs]:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Si completó el tutorial anterior (y no eliminó el registro), debería ver una salida parecida a la siguiente. Tome nota del número de ejecuciones de tareas y del identificador de ejecución más reciente, para que pueda comparar la salida después de actualizar la imagen base en la sección siguiente.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK            PLATFORM    STATUS     TRIGGER     STARTED               DURATION
--------  --------------  ----------  ---------  ----------  --------------------  ----------
da6       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual      2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit  2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual      2018-09-17T22:29:59Z  00:00:57
```

## <a name="update-the-base-image"></a>Actualización de la imagen base

Aquí puede simular una revisión de la plataforma en la imagen base. Edite **Dockerfile-base** y agregue una "a" después del número de versión definido en `NODE_VERSION`:

```Dockerfile
ENV NODE_VERSION 9.11.2a
```

Ejecute una tarea rápida para compilar la imagen base modificada. Tome nota del **identificador de ejecución** de la salida.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Una vez que haya finalizado la compilación y que ACR Tasks haya insertado la nueva imagen base en el registro, este desencadena una compilación de la imagen de aplicación. Es posible que la tarea que creó anteriormente tarde unos instantes en desencadenar la compilación de la imagen de aplicación, ya que debe detectar la imagen base recién compilada e insertada.

## <a name="list-updated-build"></a>Lista de compilaciones actualizadas

Ahora que ha actualizado la imagen base, vuelva a enumerar las ejecuciones de tareas para compararlas con la lista anterior. Si al principio la salida no es diferente, ejecute periódicamente el comando hasta ver cómo aparece la nueva ejecución de tarea en la lista.

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

La salida es similar a la siguiente. El DESENCADENADOR de la última compilación ejecutada debe ser "Image Update", lo cual indica que la tarea la inició la tarea rápida de la imagen base.

```console
$ az acr task list-builds --registry $ACR_NAME --output table

Run ID    TASK            PLATFORM    STATUS     TRIGGER       STARTED               DURATION
--------  --------------  ----------  ---------  ------------  --------------------  ----------
da8       taskhelloworld  Linux       Succeeded  Image Update  2018-09-17T23:11:50Z  00:00:33
da7                       Linux       Succeeded  Manual        2018-09-17T23:11:27Z  00:00:35
da6       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual        2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit    2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual        2018-09-17T22:29:59Z  00:00:57
```

Si quiere realizar el siguiente paso opcional y ejecutar el contenedor recién compilado para ver el número de versión actualizado, anote el valor del **identificador de ejecución** de la compilación que desencadenó "Image Update" (en la salida anterior es "da8").

### <a name="optional-run-newly-built-image"></a>Opcional: Ejecución de la imagen recién compilada

Si está trabajando de forma local (no en Cloud Shell), y tiene Docker instalado, ejecute la nueva imagen de aplicación después de terminar su compilación. Sustituya `<run-id>` por el identificador de ejecución que obtuvo en el paso anterior. Si va a usar Cloud Shell, omita esta sección (Cloud Shell no admite `docker run`).

```bash
docker run -d -p 8081:80 $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Vaya a http://localhost:8081 en el explorador, allí verá el número de versión de Node.js actualizado (con la "a") en la página web:

![Captura de pantalla de una aplicación de ejemplo representada en el explorador][base-update-02]

Es importante observar que ha actualizado la imagen **base** con un nuevo número de versión, pero que también la imagen de **aplicación** recién compilada muestra la nueva versión. ACR Tasks recopiló el cambio en la imagen base y recompiló automáticamente la imagen de aplicación.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar todos los recursos que ha creado en esta serie de tutoriales, incluido el registro de contenedor, la instancia del contenedor, el almacén de claves y la entidad de servicio, ejecute los comandos siguientes:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a usar una tarea para desencadenar automáticamente compilaciones de imágenes de contenedor cuando se actualiza la imagen base de la imagen. Ahora, continúe para aprender más información acerca de la autenticación del registro de contenedor.

> [!div class="nextstepaction"]
> [Autenticación en Azure Container Registry](container-registry-authentication.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-task-create]: /cli/azure/acr
[az-acr-task-run]: /cli/azure/acr-run
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-task-list-runs]: /cli/azure/acr
[az-acr-task]: /cli/azure/acr

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png
