---
title: 'Referencia de Azure Container Registry Tasks: YAML'
description: Referencia para definir tareas en YAML para ACR Tasks, como propiedades de tareas, tipos de pasos, propiedades de pasos y variables integradas.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 11/13/2018
ms.author: danlep
ms.openlocfilehash: e91b4e881c0f39304e3042d556f111db2089f7de
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334489"
---
# <a name="acr-tasks-reference-yaml"></a>Referencia de ACR Tasks: YAML

La definición de tareas de varios pasos en ACR Tasks proporciona un primitivo de proceso centrado en el contenedor enfocado a compilar y probar contenedores, y aplicarles revisiones. En este artículo se tratan los comandos, parámetros, propiedades y sintaxis de los archivos YAML que definen las tareas de varios pasos.

Este artículo contiene la referencia para crear archivos YAML de tarea de varios pasos para ACR Tasks. Para una introducción a ACR Tasks, consulte la [introducción a ACR Tasks](container-registry-tasks-overview.md).

> [!IMPORTANT]
> La característica de tareas de varios pasos de ACR Tasks está actualmente en versión preliminar. Las versiones preliminares están a su disposición a condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="acr-taskyaml-file-format"></a>formato de archivo acr-task.yaml

ACR Tasks admite la declaración de tareas de varios pasos en la sintaxis estándar de YAML. Definirá los pasos de una tarea en un archivo YAML que luego puede ejecutar manualmente, o que se ha desencadenado automáticamente tras la confirmación de Git o la actualización de la imagen base. Aunque en este artículo se hace referencia a `acr-task.yaml` como el archivo que contiene los pasos, ACR Tasks admite cualquier nombre de archivo válido con una [extensión admitida](#supported-task-filename-extensions).

Los primitivos `acr-task.yaml` de nivel superior son **propiedades de tareas**, **tipos de pasos** y **propiedades de pasos**:

* Las [propiedades de tareas](#task-properties) se aplican a todos los pasos a lo largo de la ejecución de tareas. Hay tres propiedades de tareas globales:
  * version
  * stepTimeout
  * totalTimeout
* Los [tipos de pasos de tareas](#task-step-types) representan los tipos de acciones que se pueden realizar en una tarea. Hay tres tipos de pasos:
  * build
  * push
  * cmd
* Las [propiedades de pasos de tareas](#task-step-properties) son parámetros que se aplican a un paso individual. Hay varias propiedades de pasos, como:
  * startDelay
  * timeout
  * when
  * y muchas más.

Le sigue el formato base de un archivo `acr-task.yaml`, incluidas algunas propiedades de pasos comunes. Aunque no es una representación exhaustiva de todo el uso de propiedades de pasos o tipos de pasos, proporciona una información general rápida del formato de archivo básico.

```yaml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
totalTimeout: # Total seconds allowed for all steps to complete.
steps: # A collection of image or container actions.
    build: # Equivalent to "docker build," but in a multi-tenant environment
    push: # Push a newly built or retagged image to a registry.
      when: # Step property that defines either parallel or dependent step execution.
    cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
      startDelay: # Step property that specifies the number of seconds to wait before starting execution.
```

### <a name="supported-task-filename-extensions"></a>Extensiones de nombre de archivo de tareas admitidas

ACR Tasks ha reservado varias extensiones de nombre de archivo, como `.yaml`, que se procesarán como un archivo de tarea. Cualquier extensión que *no* se encuentre en la siguiente lista se considera un archivo Dockerfile en ACR Tasks: .yaml, .yml, .toml, .json, .sh, .bash, .zsh,. ps1, .ps, .cmd, .bat, TS, .js, .php, .py,. RB, .lua.

YAML es el único formato de archivo compatible actualmente con ACR Tasks. Las demás extensiones de nombre de archivo están reservadas para posible compatibilidad futura.

## <a name="run-the-sample-tasks"></a>Ejecución de las tareas de ejemplo

Son varios los archivos de tareas de ejemplo a los que se hace referencia en las siguientes secciones de este artículo. Las tareas de ejemplo están en un repositorio público de GitHub, [Azure-Samples/acr-tasks][acr-tasks]. Puede ejecutarlas con el comando [az acr run][az-acr-run] de la CLI de Azure. Los comandos de ejemplo son parecidos a:

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

En el formato de los comandos de ejemplo se da por hecho que ha configurado un registro de forma predeterminada en la CLI de Azure, por lo que se omite el parámetro `--registry`. Para configurar un registro predeterminado, use el comando [az configure][az-configure] con el parámetro `--defaults`, que acepta un valor `acr=REGISTRY_NAME`.

Por ejemplo, para configurar la CLI de Azure con un registro predeterminado llamado "myregistry":

```azurecli
az configure --defaults acr=myregistry
```

## <a name="task-properties"></a>Propiedades de tareas

Las propiedades de tareas suelen aparecer en la parte superior de un archivo `acr-task.yaml` y son propiedades globales que se aplican a lo largo de la ejecución completa de la tarea. Algunas de estas propiedades globales se pueden reemplazar dentro de un paso individual.

| Propiedad | Escriba | Opcional | DESCRIPCIÓN | Invalidación admitida | Valor predeterminado |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | string | Sin  | La versión del archivo `acr-task.yaml` analizada por el servicio ACR Tasks. Si bien ACR Tasks se esfuerza por mantener la compatibilidad con versiones anteriores, este valor permite que ACR Tasks mantenga la compatibilidad dentro de una versión definida. | Sin  | None |
| `stepTimeout` | int (segundos) | SÍ | El número máximo de segundos que se puede ejecutar un paso. Esta propiedad se puede invalidar en un paso si se establece la propiedad [timeout](#timeout) del paso. | SÍ | 600 (10 minutos) |
| `totalTimeout` | int (segundos) | SÍ | El número máximo de segundos que se puede ejecutar una tarea. Una "ejecución" incluye la ejecución y la finalización de todos los pasos de la tarea, ya sea de forma correcta o con errores. También incluye la impresión de la salida de la tarea, como las dependencias de imagen detectadas y el estado de ejecución de la tarea. | Sin  | 3600 (1 hora) |

## <a name="task-step-types"></a>Tipos de pasos de tareas

ACR Tasks admite tres tipos de pasos. Cada tipo de paso admite varias propiedades, que se detallan en la sección para cada tipo de paso.

| Tipo de paso | DESCRIPCIÓN |
| --------- | ----------- |
| [`build`](#build) | Compila una imagen de contenedor mediante la conocida sintaxis `docker build`. |
| [`push`](#push) | Ejecuta una acción `docker push` de imágenes recién compiladas o etiquetadas de nuevo en un registro de contenedor. Se admite Azure Container Registry, otros registros privados y Docker Hub público.
| [`cmd`](#cmd) | Ejecuta un contenedor como un comando, con parámetros que se pasan al elemento `[ENTRYPOINT]` del contenedor. El tipo de paso `cmd` admite parámetros como env, detach y otras opciones de comando `docker run` conocidas, que permiten pruebas unitarias y funcionales con la ejecución simultánea del contenedor. |

## <a name="build"></a>build

Compila una imagen de contenedor. El tipo de paso `build` representa un medio multiinquilino seguro de ejecutar `docker build` en la nube como un primitivo de primera clase.

### <a name="syntax-build"></a>Sintaxis: build

```yaml
version: 1.0-preview-1
steps:
    - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
      [property]: [value]
```

El tipo de paso `build` admite los parámetros de la tabla siguiente. El tipo de paso `build` también es compatible con todas las opciones del comando [compilación de docker](https://docs.docker.com/engine/reference/commandline/build/), como `--build-arg` para establecer las variables en tiempo de compilación.

| Parámetro | DESCRIPCIÓN | Opcional |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | Define el elemento completo `image:tag` de la imagen compilada.<br /><br />Como se pueden usar imágenes para las validaciones de tareas internas, no todas las imágenes requieren la acción `push` en un registro. Sin embargo, para crear una instancia de una imagen dentro de una ejecución de tareas, la imagen necesita un nombre al que hacer referencia.<br /><br />A diferencia de `az acr build`, la ejecución de ACR Tasks no proporciona un comportamiento de inserción predeterminado. Con ACR Tasks, en el escenario predeterminado se da por hecho la posibilidad de compilar, validar y luego insertar una imagen. Consulte [push](#push) para ver como insertar opcionalmente imágenes compiladas. | SÍ |
| `-f` &#124; `--file` | Especifica el archivo Dockerfile que se pasa a `docker build`. Si no se especifica, se da por hecho el archivo Dockerfile predeterminado en la raíz del contexto. Para especificar un archivo Dockerfile alternativo, pase el nombre de archivo relativo a la raíz del contexto. | SÍ |
| `context` | El directorio raíz pasado a `docker build`. El directorio raíz de cada tarea se establece en un directorio [workingDirectory](#task-step-properties) compartido, e incluye la raíz del directorio clonado de Git asociado. | Sin  |

### <a name="properties-build"></a>Propiedades: build

El tipo de paso `build` admite las siguientes propiedades. Puede encontrar detalles de estas propiedades en la sección [Propiedades de pasos de tareas](#task-step-properties) de este artículo.

| | | |
| -------- | ---- | -------- |
| `detach` | booleano | Opcional |
| `entryPoint` | string | Opcional |
| `env` | [string, string, ...] | Opcional |
| `id` | string | Opcional |
| `ignoreErrors` | booleano | Opcional |
| `keep` | booleano | Opcional |
| `startDelay` | int (segundos) | Opcional |
| `timeout` | int (segundos) | Opcional |
| `when` | [string, string, ...] | Opcional |
| `workingDirectory` | string | Opcional |

### <a name="examples-build"></a>Ejemplos: build

#### <a name="build-image---context-in-root"></a>Imagen de compilación: contexto en raíz

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Imagen de compilación: contexto en el subdirectorio

```yaml
version: 1.0-preview-1
steps:
- build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>push

Inserta una o varias imágenes compiladas o etiquetadas de nuevo en un registro de contenedor. Admite la inserción en registros privados como Azure Container Registry o Docker Hub público.

### <a name="syntax-push"></a>Sintaxis: push

El tipo de paso `push` admite una colección de imágenes. La sintaxis de la colección YAML admite formatos alineados y anidados. La inserción de una sola imagen se representa normalmente mediante sintaxis alineada:

```yaml
version: 1.0-preview-1
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Para una mejor legibilidad, use sintaxis anidada al insertar varias imágenes:

```yaml
version: 1.0-preview-1
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>Propiedades: push

El tipo de paso `push` admite las siguientes propiedades. Puede encontrar detalles de estas propiedades en la sección [Propiedades de pasos de tareas](#task-step-properties) de este artículo.

| | | |
| -------- | ---- | -------- |
| `env` | [string, string, ...] | Opcional |
| `id` | string | Opcional |
| `ignoreErrors` | booleano | Opcional |
| `startDelay` | int (segundos) | Opcional |
| `timeout` | int (segundos) | Opcional |
| `when` | [string, string, ...] | Opcional |

### <a name="examples-push"></a>Ejemplos: push

#### <a name="push-multiple-images"></a>Insertar varias imágenes

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Compilar, insertar y ejecutar

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

El tipo de paso `cmd` ejecuta un contenedor.

### <a name="syntax-cmd"></a>Sintaxis: cmd

```yaml
version: 1.0-preview-1
steps:
    - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Propiedades: cmd

El tipo de paso `cmd` admite las siguientes propiedades:

| | | |
| -------- | ---- | -------- |
| `detach` | booleano | Opcional |
| `entryPoint` | string | Opcional |
| `env` | [string, string, ...] | Opcional |
| `id` | string | Opcional |
| `ignoreErrors` | booleano | Opcional |
| `keep` | booleano | Opcional |
| `startDelay` | int (segundos) | Opcional |
| `timeout` | int (segundos) | Opcional |
| `when` | [string, string, ...] | Opcional |
| `workingDirectory` | string | Opcional |

Puede encontrar detalles de estas propiedades en la sección [Propiedades de pasos de tareas](#task-step-properties) de este artículo.

### <a name="examples-cmd"></a>Ejemplos: cmd

#### <a name="run-hello-world-image"></a>Ejecutar la imagen hello world

Este comando ejecuta el archivo de tarea `hello-world.yaml`, que hace referencia a la imagen [hello world](https://hub.docker.com/_/hello-world/) de Docker Hub.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml --> [!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Ejecutar imagen de Bash y repetir "hello world"

Este comando ejecuta el archivo de tarea `bash-echo.yaml`, que hace referencia a la imagen [bash](https://hub.docker.com/_/bash/) de Docker Hub.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml --> [!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Ejecutar etiqueta de imagen de Bash específica

Para ejecutar una versión de imagen concreta, especifique la etiqueta en `cmd`.

Este comando ejecuta el archivo de tarea `bash-echo-3.yaml`, que hace referencia a la imagen [bash:3.0](https://hub.docker.com/_/bash/) de Docker Hub.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml --> [!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Ejecutar imágenes personalizadas

El tipo de paso `cmd` hace referencia a las imágenes mediante el formato estándar `docker run`. Se da por hecho que las imágenes que no comienzan con un registro se originan en docker.io. El ejemplo anterior se podría representar igualmente como:

```yaml
version: 1.0-preview-1
steps:
    - cmd: docker.io/bash:3.0 echo hello world
```

Mediante la convención de referencia de imágenes `docker run` estándar, `cmd` puede ejecutar imágenes que residan en cualquier registro privado o en Docker Hub público. Si va a hacer referencia a imágenes que se encuentran en el mismo registro donde se ejecutan las tareas de ACR, no es necesario especificar credenciales de registro.

* Ejecutar una imagen que reside en un registro de contenedor de Azure

    Reemplace `[myregistry]` por el nombre del registro.

    ```yaml
    version: 1.0-preview-1
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* Generalizar la referencia del registro con una variable de ejecución

    En lugar de codificar de forma rígida el nombre del registro en un archivo `acr-task.yaml`, puede hacer que sea más fácil de trasladar mediante una [variable de ejecución](#run-variables). La variable `Run.Registry` se expande en tiempo de ejecución al nombre del registro en el que se ejecuta la tarea.

    Para generalizar la tarea anterior de modo que funcione en cualquier registro de contenedor de Azure, haga referencia a la variable [Run.Registry](#runregistry) en el nombre de imagen:

    ```yaml
    version: 1.0-preview-1
    steps:
        - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>Propiedades de pasos de tareas

Cada tipo de paso admite varias propiedades adecuadas para su tipo. En la tabla siguiente se definen todas las propiedades de pasos disponibles. No todos los tipos de pasos admiten todas las propiedades. Para ver cuál de estas propiedades está disponible para cada tipo de paso, consulte las acciones de referencia de tipos de pasos [cmd](#cmd), [build](#build) y [push](#push).

| Propiedad | Escriba | Opcional | DESCRIPCIÓN |
| -------- | ---- | -------- | ----------- |
| `detach` | booleano | SÍ | Si el contenedor se debe desasociar cuando se ejecuta. |
| `entryPoint` | string | SÍ | Invalida el elemento `[ENTRYPOINT]` del contenedor de un paso. |
| `env` | [string, string, ...] | SÍ | Matriz de cadenas en formato `key=value` que define las variables de entorno del paso. |
| [`id`](#example-id) | string | SÍ | Identifica el paso de forma única dentro de la tarea. Otros pasos de la tarea pueden hacer referencia al elemento `id` del paso, por ejemplo, para la comprobación de dependencias con `when`.<br /><br />`id` también es el nombre del contenedor en ejecución. Los procesos que se ejecutan en otros contenedores de la tarea pueden hacer referencia a `id` como su nombre de host DNS, o para acceder a él con registros de docker [id], por ejemplo. |
| `ignoreErrors` | booleano | SÍ | Cuando se establece en `true`, el paso se marca como completado, con independencia de si se produjo un error durante su ejecución. Valor predeterminado: `false`. |
| `keep` | booleano | SÍ | Si se debe mantener el contenedor del paso después de la ejecución. |
| `startDelay` | int (segundos) | SÍ | Número de segundos para retrasar la ejecución de un paso. |
| `timeout` | int (segundos) | SÍ | Número máximo de segundos que se puede ejecutar un paso antes de terminar. |
| [`when`](#example-when) | [string, string, ...] | SÍ | Configura la dependencia de un paso de uno o varios pasos dentro de la tarea. |
| `workingDirectory` | string | SÍ | Establece el directorio de trabajo de un paso. De forma predeterminada, ACR Tasks crea un directorio raíz como directorio de trabajo. Sin embargo, si la compilación tiene varios pasos, los pasos anteriores pueden compartir artefactos con los pasos posteriores mediante la especificación del mismo directorio de trabajo. |

### <a name="examples-task-step-properties"></a>Ejemplos: Propiedades de pasos de tareas

#### <a name="example-id"></a>Ejemplo: id

Compila dos imágenes mediante la creación de una instancia de una imagen de prueba funcional. Cada paso se identifica mediante un valor de `id` único al que otros pasos de la tarea hacen referencia en su propiedad `when`.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml --> [!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Ejemplo: when

La propiedad `when` especifica la dependencia de un paso de otros pasos dentro de la tarea. Admite dos valores de parámetro:

* `when: ["-"]`: no indica ninguna dependencia de otros pasos. Un paso que especifica `when: ["-"]` comenzará inmediatamente la ejecución y permitirá la ejecución de pasos simultáneos.
* `when: ["id1", "id2"]`: indica que el paso depende de los pasos con `id` "id1" e `id` "id2". Este paso no se ejecuta hasta que los pasos "id1" e "id2" se han completado.

Si `when` no se especifica en un paso, este paso depende de la finalización del paso anterior en el archivo `acr-task.yaml`.

Ejecución secuencial de pasos sin `when`:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml --> [!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

Ejecución secuencial de pasos con `when`:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml --> [!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Compilación de imágenes paralelas:

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml --> [!code-yml[task](~/acr-tasks/when-parallel.yaml)]

Compilación de imágenes paralelas y pruebas de dependencia:

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml --> [!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Variables de ejecución

ACR Tasks incluye un conjunto predeterminado de variables que están disponibles para los pasos de la tarea cuando se ejecutan. Se puede acceder a estas variables con el formato `{{.Run.VariableName}}`, donde `VariableName` es uno de los siguientes:

* `Run.ID`
* `Run.Registry`
* `Run.Date`

### <a name="run46id"></a>Run&#46;ID

Cada ejecución, mediante `az acr run`, o la ejecución basada en desencadenador de las tareas creadas por medio de `az acr task create`, tiene un identificador único. El identificador representa la ejecución actualmente en marcha.

Se usa normalmente para etiquetar de forma única una imagen:

```yaml
version: 1.0-preview-1
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Run.Registry

El nombre completo de servidor del registro. Se usa normalmente para hacer referencia genérica al registro donde se ejecuta la tarea.

```yaml
version: 1.0-preview-1
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="rundate"></a>Run.Date

La hora UTC actual a la que comenzó la ejecución.

## <a name="next-steps"></a>Pasos siguientes

Para información general sobre las tareas de varios pasos, consulte [Ejecución de tareas de varios pasos de compilación, prueba y aplicación de revisiones en ACR Tasks](container-registry-tasks-multi-step.md).

Para compilaciones paso a paso, consulte la [Introducción a ACR Tasks](container-registry-tasks-overview.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-configure]: /cli/azure/reference-index#az-configure