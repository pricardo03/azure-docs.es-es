---
title: 'Tutorial: Automatización de compilaciones de imágenes de contenedor, Azure Container Registry Tasks'
description: En este tutorial, aprenderá a configurar una tarea de Azure Container Registry Tasks que desencadena automáticamente compilaciones de imágenes de contenedor en la nube cuando envía código fuente a un repositorio Git.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 763ff0d5f619d2808fb06c05d5b266160b3a7069
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868572"
---
# <a name="tutorial-automate-container-image-builds-in-the-cloud-when-you-commit-source-code"></a>Tutorial: Automatización de las compilaciones de imágenes de contenedor en la nube al confirmar código fuente

Además de una [tarea rápida](container-registry-tutorial-quick-task.md), ACR Tasks admite las compilaciones automatizadas de imágenes de contenedor de Docker con la *tarea de compilación*. En este tutorial va a usar la CLI de Azure para crear una tarea que desencadena automáticamente compilaciones de imágenes en la nube cuando confirma código fuente en un repositorio Git.

En este tutorial, segunda parte de la serie, se realizan las siguientes operaciones:

> [!div class="checklist"]
> * Crea una tarea.
> * Prueba de la tarea
> * Visualización del estado de la tarea
> * Desencadenamiento de la tarea con una confirmación de código

En este tutorial se da por hecho que ya ha realizado los pasos de los [tutoriales anteriores](container-registry-tutorial-quick-task.md). Si aún no lo ha hecho, complete los pasos de la sección [Requisitos previos](container-registry-tutorial-quick-task.md#prerequisites) del tutorial anterior antes de continuar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si quiere usar la CLI de Azure de forma local, debe tener la versión **2.0.46** de la CLI de Azure u otra posterior instalada y registrada con [az login][az-login]. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli].

## <a name="prerequisites"></a>Requisitos previos

### <a name="get-sample-code"></a>Obtención de código de ejemplo

En este tutorial se da por hecho que ya ha realizado los pasos de los [tutoriales anteriores](container-registry-tutorial-quick-task.md) y que ha bifurcado y clonado el repositorio de ejemplo. Si aún no lo ha hecho, complete los pasos de la sección [Requisitos previos](container-registry-tutorial-quick-task.md#prerequisites) del tutorial anterior antes de continuar.

### <a name="container-registry"></a>Registro de contenedor

Debe tener un registro de contenedor de Azure en la suscripción de Azure para completar este tutorial. Si necesita un registro, consulte el [tutorial anterior](container-registry-tutorial-quick-task.md), o [Inicio rápido: Creación de un registro de contenedor con la CLI de Azure](container-registry-get-started-azure-cli.md).

## <a name="overview-of-acr-tasks"></a>Información general sobre ACR Tasks

Una tarea define las propiedades de una compilación automatizada, incluida la ubicación del código fuente de la imagen de contenedor y el evento que desencadena la compilación. Cuando se produce un evento definido en la tarea como, por ejemplo una confirmación en un repositorio Git, ACR Tasks inicia una compilación de imagen de contenedor en la nube. De forma predeterminada, a continuación, se inserta una imagen compilada correctamente en el registro de contenedor de Azure especificado en la tarea.

ACR Tasks admite actualmente los siguientes desencadenadores:

* Confirmación en un repositorio Git
* Actualización de imagen base

En este tutorial, la tarea ACR compila e inserta una imagen de contenedor único especificada en un archivo Dockerfile. Azure Container Registry Tasks también puede ejecutar [tareas de varios pasos](container-registry-tasks-multi-step.md) (actualmente en versión preliminar) mediante un archivo YAML que define pasos para compilar, insertar y, opcionalmente, probar varios contenedores.

## <a name="create-a-build-task"></a>Creación de una tarea de compilación

En esta sección, creará primero un token de acceso personal de GitHub (PAT) para usarlo con ACR Tasks. A continuación, creará una tarea que desencadena una compilación cuando se confirma el código en su bifurcación del repositorio.

### <a name="create-a-github-personal-access-token"></a>Creación de un token de acceso personal de GitHub

Para desencadenar una compilación en una confirmación en un repositorio Git, ACR Tasks necesita un token de acceso personal (PAT) para acceder al repositorio. Siga estos pasos para generar un token de acceso personal en GitHub:

1. Vaya a la página de creación del token de acceso personal en GitHub en https://github.com/settings/tokens/new
1. Escriba una breve **descripción** para el token, por ejemplo, "ACR Tasks Demo"
1. En **repo**, active **repo:status** y **public_repo**

   ![Captura de pantalla de la página de generación de un token de acceso personal en GitHub][build-task-01-new-token]

1. Seleccione el botón **Generar token** (puede que se le pida que confirme la contraseña)
1. Copie y guarde el token generado en una **ubicación segura** (usará este token cuando defina una tarea en la siguiente sección)

   ![Captura de pantalla del token de acceso personal que se generó en GitHub][build-task-02-generated-token]

### <a name="create-the-build-task"></a>Creación de la tarea de compilación

Ahora que ha completado los pasos necesarios para habilitar ACR Tasks para que lea el estado de confirmación y cree webhooks en un repositorio, puede crear una tarea que desencadene una compilación de imagen de contenedor después de las confirmaciones en el repositorio.

Primero, rellene estas variables de entorno de shell con valores adecuados para el entorno. Este paso no es estrictamente necesario, pero hace que la ejecución de los comandos de varias líneas de la CLI de Azure en este tutorial sea un poco más fácil. Si no rellena estas variables de entorno, deberá reemplazar manualmente cada valor siempre que aparezcan en los comandos de ejemplo.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the previous section
```

Ahora, cree la tarea mediante la ejecución del siguiente comando [az acr task create][az-acr-task-create]:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --branch master \
    --file Dockerfile \
    --git-access-token $GIT_PAT
```

> [!IMPORTANT]
> Si ha creado anteriormente tareas durante la vista previa con el comando `az acr build-task`, deberá volver a crear esas tareas mediante el comando [az acr task][az-acr-task].

Esta tarea especifica que siempre que confirme código en la rama *maestra* del repositorio especificado por `--context`, ACR Tasks compilará la imagen de contenedor desde el código de esa rama. La instancia de Dockerfile que especifica `--file` en la raíz del repositorio se usa para compilar la imagen. El argumento `--image` especifica un valor parametrizado de `{{.Run.ID}}` para la porción de la versión de la etiqueta de la imagen, lo que garantiza que la imagen compilada es correlativa a una compilación específica y que está etiquetada de forma exclusiva.

La salida de un comando [az acr task create][az-acr-task-create] que se ha ejecutado correctamente debe ser parecida a la siguiente:

```console
$ az acr task create \
>     --registry $ACR_NAME \
>     --name taskhelloworld \
>     --image helloworld:{{.Run.ID}} \
>     --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
>     --branch master \
>     --file Dockerfile \
>     --git-access-token $GIT_PAT
{
  "agentConfiguration": {
    "cpu": 2
  },
  "creationDate": "2018-09-14T22:42:32.972298+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myregistry/providers/Microsoft.ContainerRegistry/registries/myregistry/tasks/taskhelloworld",
  "location": "westcentralus",
  "name": "taskhelloworld",
  "platform": {
    "architecture": "amd64",
    "os": "Linux",
    "variant": null
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "myregistry",
  "status": "Enabled",
  "step": {
    "arguments": [],
    "baseImageDependencies": null,
    "contextPath": "https://github.com/gituser/acr-build-helloworld-node",
    "dockerFilePath": "Dockerfile",
    "imageNames": [
      "helloworld:{{.Run.ID}}"
    ],
    "isPushEnabled": true,
    "noCache": false,
    "type": "Docker"
  },
  "tags": null,
  "timeout": 3600,
  "trigger": {
    "baseImageTrigger": {
      "baseImageTriggerType": "Runtime",
      "name": "defaultBaseimageTriggerName",
      "status": "Enabled"
    },
    "sourceTriggers": [
      {
        "name": "defaultSourceTriggerName",
        "sourceRepository": {
          "branch": "master",
          "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node",
          "sourceControlAuthProperties": null,
          "sourceControlType": "GitHub"
        },
        "sourceTriggerEvents": [
          "commit"
        ],
        "status": "Enabled"
      }
    ]
  },
  "type": "Microsoft.ContainerRegistry/registries/tasks"
}
```

## <a name="test-the-build-task"></a>Comprobación de la tarea de compilación

Ahora tiene una tarea que define la compilación. Para probar la canalización de compilación, desencadene una compilación manualmente mediante la ejecución del comando [az acr task run][az-acr-task-run]:

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

De forma predeterminada, el comando `az acr task run` hace streaming de la salida del registro a la consola cuando se ejecuta el comando.

```console
$ az acr task run --registry $ACR_NAME --name taskhelloworld

2018/09/17 22:51:00 Using acb_vol_9ee1f28c-4fd4-43c8-a651-f0ed027bbf0e as the home volume
2018/09/17 22:51:00 Setting up Docker configuration...
2018/09/17 22:51:02 Successfully set up Docker configuration
2018/09/17 22:51:02 Logging in to registry: myregistry.azurecr.io
2018/09/17 22:51:03 Successfully logged in
2018/09/17 22:51:03 Executing step: build
2018/09/17 22:51:03 Obtaining source code and scanning for dependencies...
2018/09/17 22:51:05 Successfully obtained source code and scanned for dependencies
Sending build context to Docker daemon  23.04kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
Status: Image is up to date for node:9-alpine
 ---> a56170f59699
Step 2/5 : COPY . /src
 ---> 5f574fcf5816
Step 3/5 : RUN cd /src && npm install
 ---> Running in b1bca3b5f4fc
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN helloworld@1.0.0 No repository field.

up to date in 0.078s
Removing intermediate container b1bca3b5f4fc
 ---> 44457db20dac
Step 4/5 : EXPOSE 80
 ---> Running in 9e6f63ec612f
Removing intermediate container 9e6f63ec612f
 ---> 74c3e8ea0d98
Step 5/5 : CMD ["node", "/src/server.js"]
 ---> Running in 7382eea2a56a
Removing intermediate container 7382eea2a56a
 ---> e33cd684027b
Successfully built e33cd684027b
Successfully tagged myregistry.azurecr.io/helloworld:da2
2018/09/17 22:51:11 Executing step: push
2018/09/17 22:51:11 Pushing image: myregistry.azurecr.io/helloworld:da2, attempt 1
The push refers to repository [myregistry.azurecr.io/helloworld]
4a853682c993: Preparing
[...]
4a853682c993: Pushed
[...]
da2: digest: sha256:c24e62fd848544a5a87f06ea60109dbef9624d03b1124bfe03e1d2c11fd62419 size: 1366
2018/09/17 22:51:21 Successfully pushed image: myregistry.azurecr.io/helloworld:da2
2018/09/17 22:51:21 Step id: build marked as successful (elapsed time in seconds: 7.198937)
2018/09/17 22:51:21 Populating digests for step id: build...
2018/09/17 22:51:22 Successfully populated digests for step id: build
2018/09/17 22:51:22 Step id: push marked as successful (elapsed time in seconds: 10.180456)
The following dependencies were found:
- image:
    registry: myregistry.azurecr.io
    repository: helloworld
    tag: da2
    digest: sha256:c24e62fd848544a5a87f06ea60109dbef9624d03b1124bfe03e1d2c11fd62419
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 68cdf2a37cdae0873b8e2f1c4d80ca60541029bf


Run ID: da2 was successful after 27s
```

## <a name="trigger-a-build-with-a-commit"></a>Desencadenamiento de una compilación con una confirmación

Ahora que ha probado la tarea ejecutándola manualmente, desencadénela automáticamente con un cambio en el código fuente.

En primer lugar, asegúrese de que se encuentra en el directorio que contiene el clon local del [repositorio][sample-repo]:

```azurecli-interactive
cd acr-build-helloworld-node
```

A continuación, ejecute los siguientes comandos para crear, confirmar e insertar un nuevo archivo en la bifurcación del repositorio en GitHub:

```azurecli-interactive
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Tasks"
git push origin master
```

Se le pedirá que proporcione sus credenciales de GitHub cuando ejecute el comando `git push`. Proporcione el nombre de usuario de GitHub y escriba el token de acceso personal (PAT) que creó anteriormente para la contraseña.

```console
$ git push origin master
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

Una vez que ha insertado una confirmación en el repositorio, el webhook que creó ACR Tasks activa e inicia una compilación en Azure Container Registry. Muestre los registros de la tarea que se está ejecutando actualmente para comprobar y supervisar el progreso de la compilación:

```azurecli-interactive
az acr task logs --registry $ACR_NAME
```

La salida es parecida a la siguiente, que muestra la tarea que se está ejecutando actualmente (o la última que se ejecutó):

```console
$ az acr task logs --registry $ACR_NAME
Showing logs of the last created run.
Run ID: da4

[...]

Run ID: da4 was successful after 38s
```

## <a name="list-builds"></a>Lista de compilaciones

Para ver una lista de las tareas que ACR Tasks ha completado para el registro, ejecute el comando [az acr task list-runs][az-acr-task-list-runs]:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

La salida del comando debe ser parecida a la siguiente. Se muestran las ejecuciones que ha ejecutado ACR Tasks y aparece "Confirmación de Git" en la columna DESENCADENADOR de la tarea más reciente:

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
--------  --------------  ----------  ---------  ----------  --------------------  ----------
da4       taskhelloworld  Linux       Succeeded  Git Commit  2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual      2018-09-17T22:29:59Z  00:00:57
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a usar una tarea que desencadena automáticamente compilaciones de imágenes de contenedor en Azure cuando confirma código fuente en un repositorio Git. Pase al siguiente tutorial para aprender a crear tareas que desencadenan compilaciones cuando se actualiza la imagen base de la imagen de contenedor.

> [!div class="nextstepaction"]
> [Automatización de compilaciones después de una actualización de imagen base](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task]: /cli/azure/acr
[az-acr-task-create]: /cli/azure/acr
[az-acr-task-run]: /cli/azure/acr
[az-acr-task-list-runs]: /cli/azure/acr
[az-login]: /cli/azure/reference-index#az-login

<!-- IMAGES -->
[build-task-01-new-token]: ./media/container-registry-tutorial-build-tasks/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-tutorial-build-tasks/build-task-02-generated-token.png
