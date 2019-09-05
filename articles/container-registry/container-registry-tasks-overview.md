---
title: Automatización de la compilación y aplicación de revisiones de imágenes de contenedor con Azure Container Registry Tasks (ACR Tasks)
description: Una introducción a ACR Tasks, un conjunto de características de Azure Container Registry que proporciona compilaciones de imágenes de contenedor, administración y aplicación de revisiones automatizadas y seguras en la nube.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 1459b6fc45bb3d875b4869d1dcb4302dec21eb96
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114800"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>Automatización de compilaciones y mantenimiento de imágenes de contenedor con ACR Tasks

Los contenedores proporcionan nuevos niveles de virtualización, que aíslan las dependencias de aplicaciones y desarrolladores de los requisitos operativos y la infraestructura. Sin embargo, lo que permanece es la necesidad de abordar cómo se administran y aplican las revisiones de esta virtualización de aplicaciones en el ciclo de vida del contenedor.

## <a name="what-is-acr-tasks"></a>¿Qué es ACR Tasks?

**ACR Tasks** es un conjunto de características que se encuentran en Azure Container Registry. Proporciona creación de imágenes de contenedores basados en la nube para Linux, Windows y ARM, y puede automatizar [las revisiones de plataforma y sistema operativo](#automate-os-and-framework-patching) para los contenedores Docker. ACR Tasks no solo amplía el ciclo de desarrollo del "bucle interno" a la nube con compilaciones de imágenes de contenedor a petición, sino que también permite compilaciones automatizadas tras la confirmación del código fuente o cuando se actualiza la imagen base de un contenedor. Con los desencadenadores de actualización de imágenes base, puede automatizar el flujo de trabajo de aplicación de revisiones de sistema operativo y marco de aplicación, y mantener los entornos seguros al tiempo que se respetan los principios de los contenedores inmutables.

Compile y pruebe imágenes de contenedor con ACR Tasks de cuatro maneras:

* [Tarea rápida](#quick-task): Compile e inserte imágenes de contenedor a petición en Azure, sin necesidad de una instalación local del motor de Docker. Considere que `docker build`, es `docker push` en la nube. Compile desde el código fuente local o un repositorio de Git.
* [Compilación al confirmar el código fuente](#automatic-build-on-source-code-commit): Desencadene una compilación de imágenes de contenedor automáticamente cuando el código se confirme en un repositorio de Git.
* [Compilación al actualizar la imagen base](#automate-os-and-framework-patching): Desencadene una compilación de imágenes de contenedor cuando se ha actualizado la imagen base de la imagen.
* [Tareas de varios pasos](#multi-step-tasks): Defina tareas de varios pasos que compilan imágenes, ejecutan contenedores como comandos e insertan imágenes en un registro. Esta característica de ACR Tasks admite la ejecución de tareas a petición y operaciones en paralelo de compilación, prueba e inserción de imágenes.

## <a name="quick-task"></a>Tarea rápida

El ciclo de desarrollo de bucle interno, el proceso iterativo de escribir código, compilar y probar la aplicación antes de confirmar el control de código fuente, es realmente el comienzo de la administración del ciclo de vida del contenedor.

Antes de confirmar la primera línea de código, la característica de [tareas rápidas](container-registry-tutorial-quick-task.md) de ACR Tasks puede proporcionar una experiencia de desarrollo integrado mediante la descarga de compilaciones de imágenes de contenedor en Azure. Con tareas rápidas, puede comprobar sus definiciones de compilación automatizadas y detectar posibles problemas antes de confirmar el código.

Mediante el conocido formato `docker build`, el comando [az acr build][az-acr-build] de la CLI de Azure toma un *contexto* (el conjunto de archivos que se van a compilar), lo envía a ACR Tasks y, de forma predeterminada, inserta la imagen compilada en su registro tras completarse.

A modo de introducción, vea el inicio rápido de [compilación y ejecución de una imagen de contenedor](container-registry-quickstart-task-cli.md) en Azure Container Registry.  

En la tabla siguiente se muestran algunos ejemplos de ubicaciones de contexto admitidas en ACR Tasks:

| Ubicación de contexto | DESCRIPCIÓN | Ejemplo |
| ---------------- | ----------- | ------- |
| Sistema de archivos local | Archivos en un directorio en el sistema de archivos local. | `/home/user/projects/myapp` |
| Rama maestra de GitHub | Archivos dentro de la rama maestra (u otra predeterminada) de un repositorio de GitHub.  | `https://github.com/gituser/myapp-repo.git` |
| Rama de GitHub | Rama específica de un repositorio de GitHub.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| Subcarpeta de GitHub | Archivos en una subcarpeta en un repositorio de GitHub. En el ejemplo se muestra la combinación de una rama y una especificación de subcarpeta. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| Tarball remoto | Archivos en un archivo comprimido en un servidor web remoto. | `http://remoteserver/myapp.tar.gz` |

ACR Tasks está diseñado como un primitivo de ciclo de vida de contenedor. Por ejemplo, puede integrar ACR Tasks en su solución de CI/CD. Al ejecutar [az login][az-login] con una [entidad de servicio][az-login-service-principal], la solución de CI/CD podría emitir entonces comandos [az acr build][az-acr-build] para iniciar las compilaciones de imágenes.

Aprenda a usar tareas rápidas en el primer tutorial de ACR Tasks: [Compilación de imágenes de contenedor en la nube con Azure Container Registry Tasks](container-registry-tutorial-quick-task.md).

## <a name="automatic-build-on-source-code-commit"></a>Compilación automática al confirmar el código fuente

Use ACR Tasks para desencadenar automáticamente una compilación de imágenes de contenedor cuando el código se confirme en un repositorio de Git en GitHub o Azure DevOps. Las tareas de compilación, configurables con el comando [az acr-task][az-acr-task] de la CLI de Azure, le permiten especificar un repositorio de Git y, opcionalmente, una rama y un archivo Dockerfile. Cuando el equipo confirma el código en el repositorio, un webhook creado por ACR Tasks desencadena una compilación de la imagen de contenedor definida en el repositorio.

> [!IMPORTANT]
> Si ha creado anteriormente tareas durante la versión preliminar con el comando `az acr build-task`, deberá volver a crear esas tareas mediante el comando [az acr task][az-acr-task].

Aprenda a desencadenar compilaciones tras la confirmación del código fuente en el segundo tutorial de ACR Tasks: [Automatización de compilaciones de imágenes de contenedor con Azure Container Registry Tasks](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatización de aplicación de revisiones de sistema operativo y marco

La eficacia de ACR Tasks para mejorar verdaderamente el flujo de trabajo de compilación del contenedor procede de su capacidad para detectar una actualización de una imagen base. Cuando la imagen base actualizada se inserta en el registro, o una imagen base se actualiza en un repositorio público, como en Docker Hub, ACR Tasks puede compilar automáticamente cualquier imagen de aplicación basada en ella.

Las imágenes de contenedor pueden clasificarse a grandes rasgos en imágenes *base* e imágenes de *aplicación*. Normalmente, las imágenes base incluyen el sistema operativo y los marcos de aplicaciones sobre los que se compila la aplicación, junto con otras personalizaciones. Estas imágenes base se basan normalmente en imágenes ascendentes públicas, por ejemplo: [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet] o [Node.js][base-node]. Algunas de las imágenes de aplicación podrían compartir una imagen base común.

Cuando el mantenedor ascendente actualiza una imagen de sistema operativo o marco de aplicación, por ejemplo, con una revisión de seguridad crítica del sistema operativo, también debe actualizar las imágenes base para incluir la corrección crítica. Cada imagen de aplicación se debe entonces volver a compilar para incluir estas correcciones ascendentes ahora incluidas en la imagen base.

Como ACR Tasks detecta dinámicamente las dependencias de la imagen base cuando compila una imagen de contenedor, puede detectar cuándo la imagen base de una aplicación está actualizada. ACR Tasks se sirve de una [tarea de compilación](container-registry-tutorial-base-image-update.md#create-a-task) preconfigurada para **recompilar automáticamente cada imagen de aplicación**. Gracias a esta detección y recompilación automáticas, ACR Tasks le ahorra tiempo y esfuerzo que normalmente son necesarios para actualizar y realizar el seguimiento de forma manual de cada una de las imágenes de aplicación que hacen referencia a la imagen base actualizada.

Una instancia de ACR Tasks realiza un seguimiento de una actualización de la imagen base cuando esta se encuentra en una de las siguientes ubicaciones:

* La misma instancia de Azure Container Registry en la que se ejecuta la tarea
* Otro registro de Azure Container Registry de la misma región 
* Un repositorio público de Docker Hub
* Un repositorio público de Microsoft Container Registry

Aprenda más sobre la aplicación de revisiones de sistema operativo y plataforma en el tercer tutorial de ACR Tasks, [Automatización de compilaciones de imágenes en la actualización de imagen base con Azure Container Registry Tasks](container-registry-tutorial-base-image-update.md).

## <a name="multi-step-tasks"></a>Tareas de varios pasos

Las tareas con varios pasos proporcionan definición y ejecución de tareas basadas en pasos para compilar y probar imágenes de contenedor en la nube, y aplicarles revisiones. Los pasos de las tareas definen las operaciones de creación e inserción de imágenes de contenedor individuales. También pueden definir la ejecución de uno o más contenedores, donde cada paso utiliza el contenedor como su entorno de ejecución.

Por ejemplo, puede crear una tarea de varios pasos que automatice lo siguiente:

1. Crear una imagen de aplicación web
1. Ejecutar el contenedor de aplicaciones web
1. Crear una imagen de prueba de aplicación web
1. Ejecutar el contenedor de prueba de aplicaciones web que realiza pruebas en el contenedor de aplicaciones en ejecución
1. Si se superan las pruebas, compilar un paquete de archivo de gráfico de Helm
1. Realizar una acción `helm upgrade` con el nuevo paquete de archivo de gráfico de Helm

Las tareas de varios pasos permiten dividir la compilación, la ejecución y la prueba de una imagen en más pasos que admiten composición, con compatibilidad con dependencias entre pasos. Con las tareas de varios pasos de ACR Tasks, tiene control más pormenorizado sobre los flujos de trabajo de compilación, prueba y aplicación de revisiones para imágenes de sistema operativo y plataforma.

Aprenda más sobre las tareas de varios pasos en [Ejecución de tareas de varios pasos de compilación, prueba y aplicación de revisiones en ACR Tasks](container-registry-tasks-multi-step.md).

## <a name="view-task-logs"></a>Vista de los registros de tareas

Las ejecuciones de las tareas generan un resultado de registro que se puede examinar para determinar si los pasos de dicha tarea se han ejecutado correctamente. Si usa los comandos [az acr build](/cli/azure/acr#az-acr-build), [az acr run](/cli/azure/acr#az-acr-run) o [az acr task run](/cli/azure/acr/task#az-acr-task-run) para desencadenar la tarea, la salida de registro para la ejecución de la tarea se transmite a la consola y también se almacena para su posterior recuperación. Vea los registros de la ejecución de una tarea en Azure Portal o use el comando [az acr task logs](/cli/azure/acr/task#az-acr-task-logs).

A partir de julio de 2019, los datos y registros de las ejecuciones de las tareas en un registro se conservarán de manera predeterminada durante 30 días y, luego, se purgarán automáticamente. Si quiere archivar los datos de la ejecución de una tarea, habilite el archivado con el comando [az acr task update-run](/cli/azure/acr/task#az-acr-task-update-run). En el ejemplo siguiente se habilita el archivado para la ejecución de la tarea *cf11* en el registro *myregistry*.

```azurecli
az acr task update-run --registry myregistry --run-id cf11 --no-archive false
```

## <a name="next-steps"></a>Pasos siguientes

Cuando esté listo para automatizar la aplicación de revisiones de sistema operativo y plataforma mediante la compilación de imágenes de contenedor en la nube, vea la [serie de tutoriales de ACR Tasks](container-registry-tutorial-quick-task.md) en tres partes.

Opcionalmente, instale la [extensión de Docker para Visual Studio Code](https://code.visualstudio.com/docs/azure/docker) y la extensión de la [cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) para trabajar con los registros de contenedor de Azure. Extraiga e inserte imágenes en un registro de contenedor de Azure o ejecute ACR Tasks y, todo ello, en Visual Studio Code.

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task]: /cli/azure/acr
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
