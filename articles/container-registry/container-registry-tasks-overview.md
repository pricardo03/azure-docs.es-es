---
title: Introducción a ACR Tasks
description: Una introducción a ACR Tasks, un conjunto de características de Azure Container Registry que proporciona compilaciones de imágenes de contenedor, administración y aplicación de revisiones automatizadas y seguras en la nube.
ms.topic: article
ms.date: 09/05/2019
ms.openlocfilehash: f8ab3c3bd259f83a61d0b030a49e158ccd6e2a69
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76938877"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>Automatización de compilaciones y mantenimiento de imágenes de contenedor con ACR Tasks

Los contenedores proporcionan nuevos niveles de virtualización, que aíslan las dependencias de aplicaciones y desarrolladores de los requisitos operativos y la infraestructura. Sin embargo, lo que permanece es la necesidad de abordar cómo se administran y aplican las revisiones de esta virtualización de aplicaciones en el ciclo de vida del contenedor.

## <a name="what-is-acr-tasks"></a>¿Qué es ACR Tasks?

**ACR Tasks** es un conjunto de características que se encuentran en Azure Container Registry. Proporciona la compilación de imágenes de contenedor basadas en la nube para [plataformas](#image-platforms), como Linux, Windows y ARM, y puede automatizar las [revisiones de marco y sistema operativo](#automate-os-and-framework-patching) para los contenedores de Docker. ACR Tasks no solo amplía el ciclo de desarrollo del "bucle interno" a la nube con compilaciones de imágenes de contenedor a petición, sino que también permite compilaciones automatizadas desencadenadas por actualizaciones del código fuente, actualizaciones en una imagen base del contenedor o temporizadores. Por ejemplo, con los desencadenadores de actualización de imágenes base, puede automatizar el flujo de trabajo de aplicación de revisiones de sistema operativo y de marco de trabajo de la aplicación, y mantener los entornos seguros al tiempo que se respetan los principios de los contenedores inmutables.

## <a name="task-scenarios"></a>Escenarios de tareas

ACR Tasks admite varios escenarios para compilar y mantener imágenes de contenedor y otros artefactos. Para obtener más información, consulte las siguientes secciones de este artículo.

* **[Tarea rápida](#quick-task)** : compile e inserte una sola imagen de contenedor en un registro de contenedor a petición en Azure, sin tener que realizar una instalación local del motor de Docker. Considere que `docker build`, es `docker push` en la nube.
* **Tareas desencadenadas automáticamente**: habilite uno o varios *desencadenadores* para compilar una imagen:
  * **[Desencadenar al actualizar el código fuente](#trigger-task-on-source-code-update)** 
  * **[Desencadenar al actualizar la imagen base](#automate-os-and-framework-patching)** 
  * **[Desencadenar de acuerdo con una programación](#schedule-a-task)** 
* **[Tarea de varios pasos](#multi-step-tasks)** : amplíe la funcionalidad de inserción y compilación de una única imagen de ACR Tasks con flujos de trabajo basados en varios contenedores y varios pasos. 

Cada tarea de ACR tiene un [contexto de código fuente](#context-locations) asociado (la ubicación de un conjunto de archivos de código fuente que se usa para compilar una imagen de contenedor u otro artefacto). Los ejemplos de contextos incluyen un repositorio GIT o un sistema de archivos local.

Las tareas también pueden sacar provecho de las [variables de ejecución](container-registry-tasks-reference-yaml.md#run-variables), de modo que puede volver a usar las definiciones de tareas y estandarizar las etiquetas para las imágenes y los artefactos.

## <a name="quick-task"></a>Tarea rápida

El ciclo de desarrollo de bucle interno, el proceso iterativo de escribir código, compilar y probar la aplicación antes de confirmar el control de código fuente, es realmente el comienzo de la administración del ciclo de vida del contenedor.

Antes de confirmar la primera línea de código, la característica de [tareas rápidas](container-registry-tutorial-quick-task.md) de ACR Tasks puede proporcionar una experiencia de desarrollo integrado mediante la descarga de compilaciones de imágenes de contenedor en Azure. Con tareas rápidas, puede comprobar sus definiciones de compilación automatizadas y detectar posibles problemas antes de confirmar el código.

Mediante el conocido formato `docker build`, el comando [az acr build][az-acr-build] de la CLI de Azure toma un [contexto](#context-locations) (el conjunto de archivos que se van a compilar), lo envía a ACR Tasks y, de forma predeterminada, inserta la imagen compilada en su registro tras completarse.

A modo de introducción, vea el inicio rápido de [compilación y ejecución de una imagen de contenedor](container-registry-quickstart-task-cli.md) en Azure Container Registry.  

ACR Tasks está diseñado como un primitivo de ciclo de vida de contenedor. Por ejemplo, puede integrar ACR Tasks en su solución de CI/CD. Al ejecutar [az login][az-login] con una [entidad de servicio][az-login-service-principal], la solución de CI/CD podría emitir entonces comandos [az acr build][az-acr-build] para iniciar las compilaciones de imágenes.

Aprenda a usar tareas rápidas en el primer tutorial de ACR Tasks: [Compilación de imágenes de contenedor en la nube con Azure Container Registry Tasks](container-registry-tutorial-quick-task.md).

> [!TIP]
> Si quiere compilar e insertar una imagen directamente desde el código fuente, sin ningún archivo Dockerfile, Azure Container Registry proporciona el comando [az acr pack build][az-acr-pack-build] (versión preliminar). Esta herramienta compila e inserta una imagen del código fuente de la aplicación mediante [Cloud Native Buildpacks](https://buildpacks.io/).

## <a name="trigger-task-on-source-code-update"></a>Desencadenamiento de una tarea al actualizar el código fuente

Desencadene una compilación de imágenes de contenedor o una tarea de varios pasos cuando se confirme el código o bien se actualice o realice una solicitud de incorporación de cambios, en un repositorio de Git público o privado en GitHub o Azure DevOps. Por ejemplo, configure una tarea de compilación con el comando [az acr task create][az-acr-task-create] de la CLI de Azure. Para ello, especifique un repositorio de GIT y, si quiere, una rama y un archivo Dockerfile. Cuando el equipo actualiza el código en el repositorio, un webhook creado por ACR Tasks desencadena una compilación de la imagen de contenedor definida en el repositorio. 

ACR Tasks admite los siguientes desencadenadores cuando establece un repositorio de GIT como el contexto de la tarea:

| Desencadenador | Habilitado de forma predeterminada |
| ------- | ------------------ |
| Commit | Sí |
| Solicitud de incorporación de cambios | No |

Para configurar el desencadenador de actualización de código fuente, debe proporcionar un token de acceso personal (PAT) a la tarea para establecer el webhook en el repositorio de GitHub público o privado o el repositorio de Azure DevOps.

> [!NOTE]
> Actualmente, ACR Tasks no admite desencadenadores de solicitud de confirmación o de extracción en repositorios de GitHub Enterprise.

Aprenda a desencadenar compilaciones tras la confirmación del código fuente en el segundo tutorial de ACR Tasks: [Automatización de compilaciones de imágenes de contenedor con Azure Container Registry Tasks](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatización de aplicación de revisiones de sistema operativo y marco

La eficacia de ACR Tasks para mejorar verdaderamente el flujo de trabajo de compilación del contenedor procede de su capacidad para detectar una actualización de una imagen base. Cuando la imagen base actualizada se inserta en el registro, o una imagen base se actualiza en un repositorio público, como en Docker Hub, ACR Tasks puede compilar automáticamente cualquier imagen de aplicación basada en ella.

Las imágenes de contenedor pueden clasificarse a grandes rasgos en imágenes *base* e imágenes de *aplicación*. Normalmente, las imágenes base incluyen el sistema operativo y los marcos de aplicaciones sobre los que se compila la aplicación, junto con otras personalizaciones. Estas imágenes base se basan normalmente en imágenes ascendentes públicas, por ejemplo: [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet] o [Node.js][base-node]. Algunas de las imágenes de aplicación podrían compartir una imagen base común.

Cuando el mantenedor ascendente actualiza una imagen de sistema operativo o marco de aplicación, por ejemplo, con una revisión de seguridad crítica del sistema operativo, también debe actualizar las imágenes base para incluir la corrección crítica. Cada imagen de aplicación se debe entonces volver a compilar para incluir estas correcciones ascendentes ahora incluidas en la imagen base.

Como ACR Tasks detecta dinámicamente las dependencias de la imagen base cuando compila una imagen de contenedor, puede detectar cuándo la imagen base de una aplicación está actualizada. ACR Tasks se sirve de una [tarea de compilación](container-registry-tutorial-base-image-update.md#create-a-task) preconfigurada para **recompilar automáticamente cada imagen de aplicación**. Gracias a esta detección y recompilación automáticas, ACR Tasks le ahorra tiempo y esfuerzo que normalmente son necesarios para actualizar y realizar el seguimiento de forma manual de cada una de las imágenes de aplicación que hacen referencia a la imagen base actualizada.

En el caso de las compilaciones de imágenes de un archivo Dockerfile, una tarea de ACR realiza un seguimiento de una actualización de la imagen base cuando esta se encuentra en una de las ubicaciones siguientes:

* La misma instancia de Azure Container Registry en la que se ejecuta la tarea
* Otro registro de Azure Container Registry de la misma región 
* Un repositorio público de Docker Hub
* Un repositorio público de Microsoft Container Registry

> [!NOTE]
> * El desencadenador de actualización de la imagen base está habilitado de forma predeterminada en una tarea de ACR. 
> * Actualmente, ACR Tasks solo realiza un seguimiento de las actualizaciones de las imágenes base de las imágenes de las aplicaciones (*tiempo de ejecución*). No realiza ningún seguimiento de las actualizaciones de las imágenes base de las imágenes intermedias (*tiempo de compilación*) que se usan en los archivos Dockerfile de varias fases. 

Aprenda más sobre la aplicación de revisiones de sistema operativo y plataforma en el tercer tutorial de ACR Tasks, [Automatización de compilaciones de imágenes en la actualización de imagen base con Azure Container Registry Tasks](container-registry-tutorial-base-image-update.md).

## <a name="schedule-a-task"></a>Programación de una tarea

Puede programar una tarea mediante la configuración de uno o más *desencadenadores de temporizador* al crear o actualizar la tarea. La programación de una tarea resulta útil para ejecutar cargas de trabajo de contenedor según una programación definida o para ejecutar operaciones de mantenimiento o pruebas en imágenes insertadas periódicamente en el registro. Para obtener detalles, consulte [Ejecución de una tarea de ACR según una programación definida](container-registry-tasks-scheduled.md).

## <a name="multi-step-tasks"></a>Tareas de varios pasos

Las tareas con varios pasos proporcionan definición y ejecución de tareas basadas en pasos para compilar y probar imágenes de contenedor en la nube, y aplicarles revisiones. Los pasos de la tarea definidos en un [archivo YAML](container-registry-tasks-reference-yaml.md) especifican operaciones de compilación e inserción individuales para imágenes de contenedor u otros artefactos. También pueden definir la ejecución de uno o más contenedores, donde cada paso utiliza el contenedor como su entorno de ejecución.

Por ejemplo, puede crear una tarea de varios pasos que automatice lo siguiente:

1. Crear una imagen de aplicación web
1. Ejecutar el contenedor de aplicaciones web
1. Crear una imagen de prueba de aplicación web
1. Ejecutar el contenedor de prueba de aplicaciones web, que realiza pruebas en el contenedor de aplicaciones en ejecución
1. Si se superan las pruebas, compilar un paquete de archivo de gráfico de Helm
1. Realizar una acción `helm upgrade` con el nuevo paquete de archivo de gráfico de Helm

Las tareas de varios pasos permiten dividir la compilación, la ejecución y la prueba de una imagen en más pasos que admiten composición, con compatibilidad con dependencias entre pasos. Con las tareas de varios pasos de ACR Tasks, tiene control más pormenorizado sobre los flujos de trabajo de compilación, prueba y aplicación de revisiones para imágenes de sistema operativo y plataforma.

Aprenda más sobre las tareas de varios pasos en [Ejecución de tareas de varios pasos de compilación, prueba y aplicación de revisiones en ACR Tasks](container-registry-tasks-multi-step.md).

## <a name="context-locations"></a>Ubicaciones de contexto

En la tabla siguiente se muestran algunos ejemplos de ubicaciones de contexto admitidas en ACR Tasks:

| Ubicación de contexto | Descripción | Ejemplo |
| ---------------- | ----------- | ------- |
| Sistema de archivos local | Archivos en un directorio en el sistema de archivos local. | `/home/user/projects/myapp` |
| Rama maestra de GitHub | Archivos dentro de la rama maestra (u otra predeterminada) de un repositorio de GitHub público o privado.  | `https://github.com/gituser/myapp-repo.git` |
| Rama de GitHub | Rama específica de un repositorio de GitHub público o privado.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| Subcarpeta de GitHub | Archivos en una subcarpeta en un repositorio de GitHub público o privado. En el ejemplo se muestra la combinación de una rama y una especificación de subcarpeta. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| Confirmación de GitHub | Confirmación específica en un repositorio de GitHub público o privado. En el ejemplo se muestra la combinación de un valor hash de confirmación (SHA) y una especificación de subcarpeta. | `https://github.com/gituser/myapp-repo.git#git-commit-hash:myfolder` |
| Subcarpeta de Azure DevOps | Archivos en una subcarpeta en un repositorio de Azure público o privado. En el ejemplo se muestra la combinación de una rama y una especificación de subcarpeta. | `https://dev.azure.com/user/myproject/_git/myapp-repo#mybranch:myfolder` |
| Tarball remoto | Archivos en un archivo comprimido en un servidor web remoto. | `http://remoteserver/myapp.tar.gz` |

> [!NOTE]
> Al usar un repositorio de Git privado como contexto para una tarea, debe proporcionar un token de acceso personal (PAT).

## <a name="image-platforms"></a>Plataformas de imagen

De forma predeterminada, ACR Tasks compila imágenes para el sistema operativo Linux y la arquitectura AMD64. Especifique la etiqueta `--platform` para compilar imágenes de Windows o imágenes de Linux para otras arquitecturas. Especifique el sistema operativo y, opcionalmente, una arquitectura admitida en formato de arquitectura o sistema operativo (por ejemplo,`--platform Linux/arm`). En el caso de las arquitecturas ARM, especifique opcionalmente una variante en formato de sistema operativo,arquitectura o variante (por ejemplo,`--platform Linux/arm64/v8`):

| SO | Architecture|
| --- | ------- | 
| Linux | AMD64<br/>ARM<br/>ARM64<br/>386 |
| Windows | AMD64 |

## <a name="view-task-logs"></a>Vista de los registros de tareas

Las ejecuciones de las tareas generan un resultado de registro que se puede examinar para determinar si los pasos de dicha tarea se han ejecutado correctamente. Si usa los comandos [az acr build](/cli/azure/acr#az-acr-build), [az acr run](/cli/azure/acr#az-acr-run) o [az acr task run](/cli/azure/acr/task#az-acr-task-run) para desencadenar la tarea, la salida de registro para la ejecución de la tarea se transmite a la consola y también se almacena para su posterior recuperación. Cuando una tarea se desencadena automáticamente, por ejemplo, mediante una confirmación de código fuente o una actualización de imagen base, los registros de tareas solo se almacenan. Vea los registros de la ejecución de una tarea en Azure Portal o use el comando [az acr task logs](/cli/azure/acr/task#az-acr-task-logs).

De manera predeterminada, los datos y registros de las ejecuciones de las tareas en un registro se conservan durante 30 días y, luego, se purgan automáticamente. Si quiere archivar los datos de la ejecución de una tarea, habilite el archivado con el comando [az acr task update-run](/cli/azure/acr/task#az-acr-task-update-run). En el ejemplo siguiente se habilita el archivado para la ejecución de la tarea *cf11* en el registro *myregistry*.

```azurecli
az acr task update-run --registry myregistry --run-id cf11 --no-archive false
```

## <a name="next-steps"></a>Pasos siguientes

Cuando esté listo para automatizar las compilaciones y el mantenimiento de imágenes de contenedor en la nube, consulte la [serie de tutoriales de ACR Tasks](container-registry-tutorial-quick-task.md).

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
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
