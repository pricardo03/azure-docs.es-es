---
title: Automatización de la compilación, prueba y aplicación de revisiones para imágenes con tareas de varios pasos de Azure Container Registry
description: Una introducción a las tareas de varios pasos, una característica de ACR Tasks de Azure Container Registry que proporciona flujos de trabajos basados en tareas para compilar y probar imágenes de contenedor en la nube, así como aplicarles revisiones.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 09/24/2018
ms.author: danlep
ms.openlocfilehash: cdabafc4f70b08076820e7e0d39300b3eb0bc1e7
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856725"
---
# <a name="run-multi-step-build-test-and-patch-tasks-in-acr-tasks"></a>Ejecución de tareas de varios pasos de compilación, prueba y aplicación de revisiones en ACR Tasks

Las tareas de varios pasos amplían la funcionalidad de inserción y compilación de una única imagen de ACR Tasks con flujos de trabajo basados en varios contenedores y varios pasos. Use tareas de varios pasos para compilar e insertar varias imágenes, en serie o en paralelo, y ejecutar esas imágenes como comandos dentro de la ejecución de una sola tarea. Cada paso define una operación de compilación o inserción de una imagen de contenedor, y también define la ejecución de un contenedor. Cada paso de una tarea con varios pasos usa un contenedor como entorno de ejecución.

> [!IMPORTANT]
> Si ha creado anteriormente tareas durante la vista previa con el comando `az acr build-task`, deberá volver a crear esas tareas mediante el comando [az acr task][az-acr-task].

Por ejemplo, puede ejecutar una tarea con pasos que automaticen estas acciones:

1. Crear una imagen de aplicación web
1. Ejecutar el contenedor de aplicaciones web
1. Crear una imagen de prueba de aplicación web
1. Ejecutar el contenedor de prueba de aplicaciones web que realiza pruebas en el contenedor de aplicaciones en ejecución
1. Si se superan las pruebas, compilar un paquete de archivo de gráfico de Helm
1. Realizar una acción `helm upgrade` con el nuevo paquete de archivo de gráfico de Helm

Todos los pasos se realizan dentro de Azure, mediante la descarga del trabajo en los recursos de proceso de Azure; de esta forma, ya no tiene que ocuparse de la administración de la infraestructura. Aparte de su registro de contenedor de Azure, solo paga por los recursos que usa. Para información sobre los precios, consulte la sección **Container Build** en [Precios de Azure Container Registry][pricing].

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición a condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="common-task-scenarios"></a>Escenarios de tareas comunes

Las tareas de varios pasos permiten escenarios como los siguientes:

* Compilar, etiquetar e insertar una o varias imágenes de contenedor, en serie o en paralelo
* Ejecutar y capturar resultados de cobertura de código y pruebas unitarias
* Ejecutar y capturar pruebas funcionales ACR Tasks admite la ejecución de varios contenedores, con la ejecución de una serie de solicitudes entre ellos.
* Realizar la ejecución basada en tareas, incluidos los pasos anteriores y posteriores de una compilación de imagen de contenedor
* Implementar uno o varios contenedores con el motor de implementación favorito en su entorno de destino

## <a name="multi-step-task-definition"></a>Definición de tareas de varios pasos

Una tarea de varios pasos de ACR Tasks se define como una serie de pasos dentro de un archivo YAML. Cada paso puede especificar dependencias tras la finalización correcta de uno o varios pasos anteriores. Están disponibles los siguientes tipos de pasos de tarea:

* [`build`](container-registry-tasks-reference-yaml.md#build): compila una o varias imágenes de contenedor mediante sintaxis `docker build` conocida, en serie o en paralelo.
* [`push`](container-registry-tasks-reference-yaml.md#push): inserta imágenes compiladas en un registro de contenedor. Se admiten registros privados como Azure Container Registry, dado que es el Docker Hub público.
* [`cmd`](container-registry-tasks-reference-yaml.md#cmd): ejecuta un contenedor, de forma que puede operar como una función dentro del contexto de la tarea en ejecución. Puede pasar parámetros al elemento `[ENTRYPOINT]` del contenedor y especificar propiedades como env, detach y otros parámetros conocidos de `docker run`. El tipo de paso `cmd` permite pruebas unitarias y funcionales, con la ejecución simultánea del contenedor.

Las tareas de varios pasos pueden ser tan simples como la compilación y la inserción de una sola imagen:

```yaml
version: 1.0-preview-1
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

O más complejas, como esta tarea, que incluye los pasos para las acciones build, test, helm package y helm deploy:

```yaml
version: 1.0-preview-1
steps:
  - id: build-web
    build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
    when: ["-"]
  - id: build-tests
    build -t {{.Run.Registry}}/hello-world-tests ./funcTests
    when: ["-"]
  - id: push
    push: ["{{.Run.Registry}}/helloworld:{{.Run.ID}}"]
    when: ["build-web", "build-tests"]
  - id: hello-world-web
    cmd: {{.Run.Registry}}/helloworld:{{.Run.ID}}
  - id: funcTests
    cmd: {{.Run.Registry}}/helloworld:{{.Run.ID}}
    env: ["host=helloworld:80"]
  - cmd: {{.Run.Registry}}/functions/helm package --app-version {{.Run.ID}} -d ./helm ./helm/helloworld/
  - cmd: {{.Run.Registry}}/functions/helm upgrade helloworld ./helm/helloworld/ --reuse-values --set helloworld.image={{.Run.Registry}}/helloworld:{{.Run.ID}}
```

## <a name="run-a-sample-task"></a>Ejecución de una tarea de ejemplo

Las tareas admiten ejecución manual, lo que se conoce como "ejecución rápida", y ejecución automatizada tras la confirmación de Git o la actualización de la imagen base.

Para ejecutar una tarea, primero defina los pasos de la tarea en un archivo YAML y luego ejecute el comando [az acr run][az-acr-run] de la CLI de Azure.

Este es un ejemplo de comando de la CLI de Azure que ejecuta una tarea mediante un archivo YAML de tarea de ejemplo. Los pasos son compilar y luego insertar una imagen. Antes de ejecutar este comando, actualice `\<acrName\>` con el nombre de su propio registro de contenedor de Azure.

```azurecli
az acr run --registry <acrName> -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

Al ejecutar la tarea, la salida debe mostrar el progreso de cada paso que se define en el archivo YAML. En la siguiente salida, los pasos aparecen como `acb_step_0` y `acb_step_1`.

```console
$ az acr run --registry myregistry -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
Sending context to registry: myregistry...
Queued a run with ID: yd14
Waiting for an agent...
2018/09/12 20:08:44 Using acb_vol_0467fe58-f6ab-4dbd-a022-1bb487366941 as the home volume
2018/09/12 20:08:44 Creating Docker network: acb_default_network
2018/09/12 20:08:44 Successfully set up Docker network: acb_default_network
2018/09/12 20:08:44 Setting up Docker configuration...
2018/09/12 20:08:45 Successfully set up Docker configuration
2018/09/12 20:08:45 Logging in to registry: myregistry.azurecr-test.io
2018/09/12 20:08:46 Successfully logged in
2018/09/12 20:08:46 Executing step: acb_step_0
2018/09/12 20:08:46 Obtaining source code and scanning for dependencies...
2018/09/12 20:08:47 Successfully obtained source code and scanned for dependencies
Sending build context to Docker daemon  109.6kB
Step 1/1 : FROM hello-world
 ---> 4ab4c602aa5e
Successfully built 4ab4c602aa5e
Successfully tagged myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:48 Executing step: acb_step_1
2018/09/12 20:08:48 Pushing image: myregistry.azurecr-test.io/hello-world:yd14, attempt 1
The push refers to repository [myregistry.azurecr-test.io/hello-world]
428c97da766c: Preparing
428c97da766c: Layer already exists
yd14: digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812 size: 524
2018/09/12 20:08:55 Successfully pushed image: myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:55 Step id: acb_step_0 marked as successful (elapsed time in seconds: 2.035049)
2018/09/12 20:08:55 Populating digests for step id: acb_step_0...
2018/09/12 20:08:57 Successfully populated digests for step id: acb_step_0
2018/09/12 20:08:57 Step id: acb_step_1 marked as successful (elapsed time in seconds: 6.832391)
The following dependencies were found:
- image:
    registry: myregistry.azurecr-test.io
    repository: hello-world
    tag: yd14
    digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
  git: {}


Run ID: yd14 was successful after 19s
```

Para más información sobre las compilaciones automatizadas tras la confirmación de Git o la actualización de la imagen base, consulte los tutoriales [Automatización de compilaciones de imágenes](container-registry-tutorial-build-task.md) y [Compilaciones de actualización de imágenes base](container-registry-tutorial-base-image-update.md).

## <a name="preview-feedback"></a>Comentarios sobre la versión preliminar

Aunque la característica de tareas de varios pasos de ACR Tasks está en versión preliminar, le invitamos a que nos proporcione sus comentarios. Existen varios canales de comentarios:

* [Issues](https://aka.ms/acr/issues): vea los problemas y errores existentes y registre otros nuevos.
* [UserVoice](https://aka.ms/acr/uservoice): vote por las solicitudes de características existentes o cree solicitudes.
* [Discuss](https://aka.ms/acr/feedback): entable debates sobre Azure Container Registry con la comunidad de Stack Overflow.

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar referencias y ejemplos de las tareas de varios pasos aquí:

* [Referencia de tareas](container-registry-tasks-reference-yaml.md): tipos de pasos de tareas, sus propiedades y uso.
* [Ejemplos de tareas][task-examples]: archivos `task.yaml` de ejemplo para varios escenarios, de sencillos a complejos.

<!-- IMAGES -->

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[task-examples]: https://github.com/Azure-Samples/acr-tasks
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-run]: /cli/azure/acr/run#az-acr-run
[az-acr-task]: /cli/azure/acr#az-acr-task