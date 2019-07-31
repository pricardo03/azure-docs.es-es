---
title: solución de problemas
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/11/2018
ms.topic: conceptual
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.openlocfilehash: 2434507ac89d631bb96ae9633403075801879a37
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277400"
---
# <a name="troubleshooting-guide"></a>Guía de solución de problemas

Esta guía contiene información sobre problemas habituales que pueden surgir al usar Azure Dev Spaces.

Si tiene algún problema al usar Azure Dev Spaces, cree un [problema en el repositorio de GitHub para Azure Dev Spaces](https://github.com/Azure/dev-spaces/issues).

## <a name="enabling-detailed-logging"></a>Habilitar el registro detallado

Para solucionar problemas de forma más eficaz, puede ser útil crear registros más detallados para su revisión.

Para la extensión de Visual Studio, establezca la variable de entorno `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` en 1. Asegúrese de reiniciar Visual Studio para que la variable de entorno surta efecto. Una vez habilitados, los registros detallados se escribirán en su directorio `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools`.

En la CLI, puede generar más información durante la ejecución del comando mediante el uso del conmutador `--verbose`. También puede examinar registros más detallados en `%TEMP%\Azure Dev Spaces`. En un equipo Mac, para encontrar el directorio TEMP hay que ejecutar `echo $TMPDIR` desde una ventana de terminal. En un equipo Linux, el directorio TEMP es normalmente `/tmp`.

## <a name="debugging-services-with-multiple-instances"></a>Servicios de depuración con varias instancias

En este momento, Azure Dev Spaces funciona mejor cuando se depura una sola instancia o pod. El archivo azds.yaml contiene un valor, *replicaCount*, que indica el número de pods que Kubernetes ejecuta para el servicio. Si cambia el valor de replicaCount para configurar la aplicación de forma que ejecute varios pods para un servicio determinado, el depurador se asocia al primer pod (cuando se muestran en orden alfabético). El depurador se asocia a un pod diferente cuando se recicla el pod original, lo que da lugar posiblemente a un comportamiento inesperado.

## <a name="error-failed-to-create-azure-dev-spaces-controller"></a>Error "Failed to create Azure Dev Spaces controller" (Error al crear el controlador de Azure Dev Spaces)

### <a name="reason"></a>Motivo
Es posible que vea este error cuando algo va mal con la creación del controlador. Si es un error transitorio, elimine el controlador y vuelva a crearlo para corregirlo.

### <a name="try"></a>Probar

Eliminación del controlador:

```bash
azds remove -g <resource group name> -n <cluster name>
```

Para eliminar un controlador, use la CLI de Azure Dev Spaces. No es posible eliminar un controlador desde Visual Studio. Tampoco puede instalar la CLI de Azure Dev Spaces en Azure Cloud Shell así que no podrá eliminar un controlador desde este servicio.

Si no tiene la CLI de Azure Dev Spaces instalada, puede instalarla primero mediante el comando siguiente y, a continuación, eliminar el controlador:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

La recreación del controlador se puede realizar desde la CLI o desde Visual Studio. Consulte los inicios rápidos [Desarrollo en equipo](quickstart-team-development.md) o [Desarrollo con .NET Core](quickstart-netcore-visualstudio.md) para obtener ejemplos.

## <a name="error-service-cannot-be-started"></a>Error "No se puede iniciar el servicio".

Es posible que vea este error cuando el código del servicio no se inicia. La causa suele estar en el código de usuario. Para obtener más información de diagnóstico, realice los siguientes cambios en los comandos y valores de configuración:

### <a name="try"></a>Pruebe lo siguiente:

En la línea de comandos:

Al usar _azds.exe_, emplee la opción de línea de comandos detallada, y utilice la opción de línea de comandos de salida para especificar el formato de salida.
 
```cmd
azds up --verbose --output json
```

En Visual Studio:

1. Abra **Herramientas > Opciones** y, en **Proyectos y soluciones**, elija **Build and Run** (Compilar y ejecutar).
2. Cambie la configuración de **Detalles de la salida de la compilación del proyecto de MSBuild** a **Detallado** o **Diagnóstico**.

    ![Captura de pantalla del cuadro de diálogo Opciones de herramientas](media/common/VerbositySetting.PNG)
    
### <a name="multi-stage-dockerfiles"></a>Dockerfile de varias fases:
Al usar un Dockerfile de varias fases, recibirá el error *El servicio no se puede iniciar*. En esta situación, la salida detallada contiene el texto siguiente:

```cmd
$ azds up -v
Using dev space 'default' with target 'AksClusterName'
Synchronizing files...6s
Installing Helm chart...2s
Waiting for container image build...10s
Building container image...
Step 1/12 : FROM [imagename:tag] AS base
Error parsing reference: "[imagename:tag] AS base" is not a valid repository/tag: invalid reference format
Failed to build container image.
Service cannot be started.
```

Este error se debe a que los nodos de AKS ejecutan una versión anterior de Docker que no es compatible con las compilaciones de varias fases. Para evitar compilaciones de varias fases, vuelva a escribir el Dockerfile.

### <a name="rerunning-a-service-after-controller-re-creation"></a>Nueva ejecución de un servicio después de volver a crear el controlador
Puede que reciba el error*El servicio no se puede iniciar* cuando intenta volver a ejecutar un servicio después de haber quitado y luego creado de nuevo el controlador de Azure Dev Spaces asociado a este clúster. En esta situación, la salida detallada contiene el texto siguiente:

```cmd
Installing Helm chart...
Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
Helm install failed with exit code '1': Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
```

Este error se debe a que al eliminar el controlador de Dev Spaces no se eliminan los servicios instalados anteriormente por ese controlador. Se produce un error al volver a crear el controlador y luego intentar ejecutar los servicios mediante el nuevo controlador porque los servicios antiguos todavía están en su lugar.

Para solucionar este problema, use el comando `kubectl delete` para quitar manualmente los servicios antiguos del clúster y, después, vuelva a ejecutar Dev Spaces para instalar los nuevos servicios.

## <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>Se produce un error en la resolución de nombres DNS para una dirección URL pública asociada con un servicio Dev Spaces

Puede configurar un punto de conexión de dirección URL pública para el servicio si especifica el modificador `--public` para el comando `azds prep` o si activa la casilla `Publicly Accessible` en Visual Studio. El nombre DNS público se registra automáticamente cuando el servicio se ejecuta en Dev Spaces. Si no está registrado, verá un error *No se puede mostrar la página* o *No se puede acceder a este sitio* en el explorador web cuando se conecte a la dirección URL pública.

### <a name="try"></a>Pruebe lo siguiente:

Puede usar el siguiente comando para enumerar todas las direcciones URL asociadas con los servicios Dev Spaces:

```cmd
azds list-uris
```

Si una dirección URL está en estado *Pendiente*, significa que Dev Spaces aún está esperando que se complete el registro DNS. A veces, el proceso de registro tarda unos minutos en completarse. Dev Spaces también abre un túnel de host local para cada servicio, que puede usar mientras espera el registro del DNS.

Si una dirección URL permanece en estado *Pendiente* durante más de 5 minutos, puede que indique un problema con el pod de DNS externo que crea al punto de conexión público o con el pod controlador de entrada nginx que adquiere el punto de conexión público. Puede usar los siguientes comandos para eliminar estos pods. AKS vuelve a crear automáticamente los pods eliminados.

```cmd
kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
```

## <a name="error-required-tools-and-configurations-are-missing"></a>Error que indica que faltan las herramientas y configuraciones necesarias

Este error puede producirse al iniciar VS Code: "[Azure Dev Spaces] Faltan las herramientas y configuraciones necesarias para compilar y depurar "[nombre de proyecto]"".
El error significa que azds.exe no está en la variable de entorno PATH, tal como se muestra en VS Code.

### <a name="try"></a>Pruebe lo siguiente:

Inicie VS Code desde un símbolo del sistema donde la variable de entorno PATH esté establecida correctamente.

## <a name="error-required-tools-to-build-and-debug-projectname-are-out-of-date"></a>Error "Las herramientas necesarias para compilar y depurar 'nombreDeProyecto' están desactualizadas".

Verá este error en Visual Studio Code si tiene una versión más reciente de la extensión de VS Code para Azure Dev Spaces, pero una versión más antigua de la CLI de Azure Dev Spaces.

### <a name="try"></a>Probar

Descargue e instale la versión más reciente de la CLI de Azure Dev Spaces:

* [Windows](https://aka.ms/get-azds-windows)
* [Mac](https://aka.ms/get-azds-mac)
* [Linux](https://aka.ms/get-azds-linux)

## <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>El error de "azds" no se reconoce como un comando interno o externo, programa operable o archivo por lotes
 
Es posible que vea este error si azds.exe no está instalado o configurado correctamente.

### <a name="try"></a>Pruebe lo siguiente:

1. Compruebe la ubicación %ProgramFiles%/Microsoft SDKs\Azure\Azure Dev Spaces CLI de azds.exe. Si está ahí, agregue esa ubicación a la variable de entorno PATH.
2. Si azds.exe no está instalado, ejecute el siguiente comando:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

## <a name="warning-dockerfile-could-not-be-generated-due-to-unsupported-language"></a>Advertencia: Dockerfile could not be generated due to unsupported language (No se puede generar un Dockerfile debido a un lenguaje no admitido)
Azure Dev Spaces proporciona compatibilidad nativa para C# y Node.js. Al ejecutar *azds prep* en un directorio que contiene código escrito en uno de estos lenguajes, Azure Dev Spaces creará automáticamente un Dockerfile adecuado automáticamente.

Puede seguir usando Azure Dev Spaces con código escrito en otros lenguajes, pero deberá crear el Dockerfile manualmente antes de ejecutar *azds up* por primera vez.

### <a name="try"></a>Pruebe lo siguiente:
Si la aplicación se escribe en un lenguaje que Azure Dev Spaces no admite de forma nativa, deberá proporcionar un Dockerfile apropiado para compilar una imagen de contenedor que ejecute el código. Docker ofrece una [lista de procedimientos recomendados para escribir un Dockerfile](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/), así como una [referencia sobre Dockerfile](https://docs.docker.com/engine/reference/builder/) con la que le resultará más fácil escribir un Dockerfile que se ajuste a sus necesidades.

Una vez que tenga un Dockerfile adecuado en su lugar, puede continuar ejecutando *azds up* para ejecutar la aplicación en Azure Dev Spaces.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Error "Error de conexión en dirección ascendente o desconexión o restablecimiento antes de los encabezados"
Puede ver este error al intentar acceder al servicio. Por ejemplo, cuando vaya a la dirección URL del servicio en un explorador. 

### <a name="reason"></a>Motivo 
El puerto de contenedor no está disponible. Las causas de este problema pueden ser: 
* El contenedor está todavía en proceso de creación e implementación. Este problema puede producirse si ejecuta `azds up` o inicia el depurador y luego intenta acceder al contenedor antes de que se haya implementado correctamente.
* La configuración del puerto no es coherente en _Dockerfile_, el gráfico de Helm y cualquier otro código de servidor que abre un puerto.

### <a name="try"></a>Pruebe lo siguiente:
1. Si el contenedor está en proceso de creación o implementación, puede esperar entre dos y tres segundos e intentar acceder al servicio de nuevo. 
1. Compruebe la configuración del puerto. Los números de puerto especificados deben ser **idénticos** en todos los recursos siguientes:
    * **Dockerfile:** especificado por la instrucción `EXPOSE`.
    * **[Gráfico de Helm](https://docs.helm.sh):** especificado por los valores `externalPort` y `internalPort` valores para un servicio (a menudo se encuentra en un archivo `values.yml`).
    * Los puertos abiertos en el código de aplicación, por ejemplo, en Node.js: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Archivo de configuración no encontrado
Ejecuta `azds up` y aparece el siguiente error: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Motivo
Debe ejecutar `azds up` desde el directorio raíz del código que desea ejecutar y debe inicializar la carpeta de código para que se ejecute con Azure Dev Spaces.

### <a name="try"></a>Pruebe lo siguiente:
1. Cambie el directorio actual a la carpeta raíz que contiene el código de servicio. 
1. Si no tiene un archivo _azds.yaml_ en la carpeta de código, ejecute `azds prep` para generar los recursos de Docker, Kubernetes y Azure Dev Spaces.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Error: "El programa de canalización "azds" terminó inesperadamente con el código 126".
El inicio del depurador de VS Code a veces produce este error.

### <a name="try"></a>Pruebe lo siguiente:
1. Cierre y vuelva a abrir VS Code.
2. Vuelva a presionar F5.

## <a name="debugging-error-failed-to-find-debugger-extension-for-typecoreclr"></a>Error de depuración “No se pudo encontrar la extensión del depurador para type:coreclr”
La ejecución del depurador de VS Code informa del error `Failed to find debugger extension for type:coreclr.`.

### <a name="reason"></a>Motivo
No tiene la extensión de VS Code para C# instalada en la máquina de desarrollo. La extensión de C# incluye compatibilidad de depuración para .Net Core (CoreCLR).

### <a name="try"></a>Pruebe lo siguiente:
Instale la [extensión de VS Code para C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Error de depuración "El tipo de depuración "coreclr" configurado no es compatible."
La ejecución del depurador de VS Code informa del error `Configured debug type 'coreclr' is not supported.`.

### <a name="reason"></a>Motivo
No tiene la extensión de VS Code para Azure Dev Spaces instalada en la máquina de desarrollo.

### <a name="try"></a>Pruebe lo siguiente:
Instale la [extensión de VS Code para Azure Dev Spaces](get-started-netcore.md).

## <a name="debugging-error-invalid-cwd-value-src-the-system-cannot-find-the-file-specified-or-launch-program-srcpath-to-project-binary-does-not-exist"></a>Error de depuración "Valor 'cwd' no válido '/src'. El sistema no encuentra el archivo especificado." o "launch: program '/src/[ruta de acceso al archivo binario del proyecto]' no existe"
La ejecución del depurador de VS Code informa del error `Invalid 'cwd' value '/src'. The system cannot find the file specified.` o `launch: program '/src/[path to project executable]' does not exist`.

### <a name="reason"></a>Motivo
De forma predeterminada, la extensión de VS Code utiliza `src` como directorio de trabajo para el proyecto en el contenedor. Si ha actualizado `Dockerfile` para especificar un directorio de trabajo diferente, puede que vea este error.

### <a name="try"></a>Pruebe lo siguiente:
Actualizar el archivo `launch.json` en el subdirectorio `.vscode` de su carpeta de proyecto. Cambie la directiva `configurations->cwd` para que apunte al mismo directorio que el `WORKDIR` definido en el `Dockerfile` del proyecto. También es posible que deba actualizar la directiva `configurations->program`.

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>No se encontró el tipo o el nombre de espacio de nombres "MyLibrary"

### <a name="reason"></a>Motivo 
El contexto de compilación está en el nivel de proyecto o servicio de forma predeterminada; por tanto, si busca un proyecto de biblioteca, no lo encontrará.

### <a name="try"></a>Pruebe lo siguiente:
¿Qué debe hacer?:
1. Modifique el archivo _azds.yaml_ para establecer el contexto de compilación en el nivel de la solución.
2. Modifique los archivos _Dockerfile_ y _Dockerfile.develop_ para hacer referencia a los archivos _.csproj_ correctamente, en relación con el nuevo contexto de compilación.
3. Coloque un archivo _.dockerignore_ junto al archivo .sln y realice las modificaciones según sea necesario.

Puede encontrar un ejemplo en https://github.com/sgreenmsft/buildcontextsample.

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>Error de autorización "Microsoft.DevSpaces/register/action"
Necesita acceso de *propietario* o *colaborador* en su suscripción de Azure para administrar Azure Dev Spaces. Puede ver este error si intenta administrar Dev Spaces y no tiene acceso de *propietario* o *colaborador* a la suscripción de Azure asociada.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Motivo
La suscripción de Azure seleccionada no ha registrado el espacio de nombres `Microsoft.DevSpaces`.

### <a name="try"></a>Pruebe lo siguiente:
Un usuario con acceso de tipo Propietario o Colaborador a la suscripción de Azure puede ejecutar el siguiente comando de la CLI de Azure para registrar manualmente el espacio de nombres `Microsoft.DevSpaces`:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="dev-spaces-times-out-at-waiting-for-container-image-build-step-with-aks-virtual-nodes"></a>Con los nodos virtuales de AKS, Dev Spaces agota el tiempo de espera en el paso *Esperando la compilación de la imagen de contenedor...* .

### <a name="reason"></a>Motivo
Este problema se produce cuando intenta usar Dev Spaces para ejecutar un servicio que está configurado para funcionar en un [nodo virtual de AKS](https://docs.microsoft.com/azure/aks/virtual-nodes-portal). Dev Spaces no admite actualmente la compilación o depuración de servicios en nodos virtuales.

Si ejecuta `azds up` con el modificador `--verbose` o habilita el registro detallado en Visual Studio, verá más detalles:

```cmd
$ azds up --verbose

Installed chart in 2s
Waiting for container image build...
pods/mywebapi-76cf5f69bb-lgprv: Scheduled: Successfully assigned default/mywebapi-76cf5f69bb-lgprv to virtual-node-aci-linux
Streaming build container logs for service 'mywebapi' failed with: Timed out after 601.3037572 seconds trying to start build logs streaming operation. 10m 1s
Container image build failed
```

El comando anterior muestra que el pod del servicio se ha asignado a *virtual-node-aci-linux*, que es un nodo virtual.

### <a name="try"></a>Pruebe lo siguiente:
Actualice el gráfico de Helm para el servicio a fin de quitar cualquier valor *nodeSelector* o *tolerations* que permita que el servicio se ejecute en un nodo virtual. Estos valores se definen normalmente en el archivo `values.yaml` del gráfico.

Puede seguir usando un clúster de AKS con la característica de nodos virtuales habilitada si el servicio que quiere compilar o depurar mediante Dev Spaces se ejecuta en un nodo de máquina virtual. Esta es la configuración predeterminada.

## <a name="error-could-not-find-a-ready-tiller-pod-when-launching-dev-spaces"></a>"Error: no se pudo encontrar un pod de Tiller listo" al iniciar Dev Spaces

### <a name="reason"></a>Motivo
Este error se produce si el cliente de Helm ya no puede comunicarse con el pod Tiller que se está ejecutando en el clúster.

### <a name="try"></a>Pruebe lo siguiente:
Reiniciar los nodos de agente en el clúster normalmente resuelve este problema.

## <a name="error-release-azds-identifier-spacename-servicename-failed-services-servicename-already-exists-or-pull-access-denied-for-servicename-repository-does-not-exist-or-may-require-docker-login"></a>"Error: release azds-\<identifier\>-\<spacename\>-\<servicename\> failed: services '\<servicename\>' already exists" or "Pull access denied for \<servicename\>, repository does not exist or may require 'docker login'" ("Error: error en la versión azds-\<identifier\>-\<spacename\>-\<servicename\>: los servicios '\<servicename\>' ya existen" o "Se ha denegado el acceso de inserción para \<servicename\>, el repositorio no existe o requiere iniciar sesión en docker")

### <a name="reason"></a>Motivo
Estos errores se pueden producir si combina comandos directos de Helm (como `helm install`, `helm upgrade` o `helm delete`) con comandos de Dev Spaces (como `azds up` y `azds down`) en el mismo espacio de desarrollo. Se producen porque Dev Spaces dispone de su propia instancia de Tiller que entrará en conflicto con su propia instancia de Tiller que se ejecuta en el mismo espacio de desarrollo.

### <a name="try"></a>Pruebe lo siguiente:
Se pueden usar comandos de Helm y comandos de Dev Spaces en el mismo clúster de AKS, pero cada espacio de nombres habilitado para Dev Spaces debería usar unos u otros.

Por ejemplo, suponga que utiliza un comando Helm para ejecutar toda la aplicación en un espacio de desarrollo primario. Puede crear espacios de desarrollo secundarios fuera de ese espacio primario, usar Dev Spaces para ejecutar servicios individuales en los espacios de desarrollo secundarios y probar los servicios juntos. Cuando esté listo para insertar los cambios, utilice un comando de Helm para implementar el código actualizado en el espacio de desarrollo primario. No use `azds up` para ejecutar el servicio actualizado en el espacio de desarrollo primario ya que entrará en conflicto con el servicio que se ejecutó inicialmente mediante Helm.

## <a name="azure-dev-spaces-proxy-can-interfere-with-other-pods-running-in-a-dev-space"></a>El proxy de Azure Dev Spaces puede interferir con otros pods que se ejecutan en un espacio de desarrollo

### <a name="reason"></a>Motivo
Cuando se habilita Dev Spaces en un espacio de nombres en el clúster de AKS, se instala un contenedor adicional denominado _mindaro-proxy_ en cada uno de los pods que se ejecutan dentro de ese espacio de nombres. Este contenedor intercepta las llamadas a los servicios del pod, lo que es parte integral de las funcionalidades de desarrollo del equipo de Dev Spaces; sin embargo, puede interferir con determinados servicios que se ejecutan en esos pods. Se sabe que interfiere con los pods que ejecutan Azure Cache for Redis, lo que provoca errores de conexión y de comunicación entre el servidor primario y el secundario.

### <a name="try"></a>Pruebe lo siguiente:
Puede mover los pods afectados a un espacio de nombres dentro del clúster que _no_ tenga habilitado Dev Spaces. El resto de la aplicación puede seguir ejecutándose dentro de un espacio de nombres habilitado para Dev Spaces. Dev Spaces no instala el contenedor _mindaro-proxy_ dentro de contenedores no habilitados para Dev Spaces.

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure Dev Spaces no parece usar mi Dockerfile existente para crear un contenedor

### <a name="reason"></a>Motivo
Azure Dev Spaces puede configurarse para que apunte a un _Dockerfile_ específico en el proyecto. Si cree que Azure Dev Spaces no usa el _Dockerfile_ esperado para compilar los contenedores, podría tener que indicar explícitamente a Azure Dev Spaces dónde se encuentra. 

### <a name="try"></a>Pruebe lo siguiente:
Abra el archivo _azds.yaml_ que Azure Dev Spaces generó en el proyecto. Use la directiva *configurations->develop->build->dockerfile* para apuntar al Dockerfile que quiere usar:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```

## <a name="error-internal-watch-failed-watch-enospc-when-attaching-debugging-to-a-nodejs-application"></a>Error "Internal watch failed: watch ENOSPC" (Error durante la inspección interna: inspeccione ENOSPC) al asociar depuración a una aplicación de Node.js

### <a name="reason"></a>Motivo

El nodo que ejecuta el pod con la aplicación de Node.js que está intentando asociar a un depurador ha superado el valor *fs.inotify.max_user_watches*. En algunos casos, [el valor predeterminado de *fs.inotify.max_user_watches* puede ser demasiado pequeño para controlar la asociación de un depurador directamente a un pod](https://github.com/Azure/AKS/issues/772).

### <a name="try"></a>Probar
Una solución temporal para resolver este problema consiste en aumentar el valor de *fs.inotify.max_user_watches* en cada nodo del clúster y reiniciar ese nodo para que los cambios surtan efecto.

## <a name="new-pods-are-not-starting"></a>No se inician nuevos pods

### <a name="reason"></a>Motivo

El inicializador de Kubernetes no puede aplicar PodSpec a los nuevos pods debido a cambios en los permisos de RBAC en el rol *cluster-admin* del clúster. El nuevo pod también puede tener un PodSpec no válido, por ejemplo si la cuenta de servicio asociada con el pod ya no existe. Para ver los pods que están en un estado *pendiente* debido a algún problema del inicializador, use el comando `kubectl get pods`:

```bash
kubectl get pods --all-namespaces --include-uninitialized
```

Este problema puede afectar a los pods de *todos los espacios de nombres* del clúster incluidos aquellos espacios en los que Azure Dev Spaces no está habilitado.

### <a name="try"></a>Probar

[Actualice la CLI de Dev Spaces a la versión más reciente](./how-to/upgrade-tools.md#update-the-dev-spaces-cli-extension-and-command-line-tools) y, a continuación, elimine *azds InitializerConfiguration* del controlador de Azure Dev Spaces:

```bash
az aks get-credentials --resource-group <resource group name> --name <cluster name>
kubectl delete InitializerConfiguration azds
```

Una vez que haya eliminado *azds InitializerConfiguration* del controlador de Azure Dev Spaces, use `kubectl delete` para eliminar todos los pods con estado *pendiente*. Una vez que se hayan eliminado todos los pods pendientes, vuelva a implementarlos.

Si los nuevos pods aún siguen en el estado *pendiente* después de una reimplementación, use `kubectl delete` para eliminar todos los pods con ese estado. Una vez que se hayan eliminado todos los pods pendientes, elimine el controlador del clúster y vuelva a instalarlo:

```bash
azds remove -g <resource group name> -n <cluster name>
azds controller create --name <cluster name> -g <resource group name> -tn <cluster name>
```

Una vez reinstalado el controlador, vuelva a implementar los pods.

## <a name="incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis"></a>Permisos incorrectos de RBAC para llamar al controlador y las API de Dev Spaces

### <a name="reason"></a>Motivo
El usuario que accede al controlador de Azure Dev Spaces debe tener acceso de lectura al archivo *kubeconfig* de administración en el clúster de AKS. Por ejemplo, este permiso está disponible en el [rol de administrador del clúster integrado de Azure Kubernetes Service](../aks/control-kubeconfig-access.md#available-cluster-roles-permissions). El usuario que accede al controlador de Azure Dev Spaces debe tener también los roles de RBAC *Colaborador* o *Propietario* del controlador.

### <a name="try"></a>Probar
Encontrará más detalles sobre cómo actualizar los permisos de un usuario para un clúster de AKS [aquí](../aks/control-kubeconfig-access.md#assign-role-permissions-to-a-user-or-group).

Para actualizar el rol de RBAC del usuario para el controlador:

1. Inicie sesión en Azure Portal en https://portal.azure.com.
1. Vaya al grupo de recursos que contiene el controlador, que suele ser el mismo que el del clúster de AKS.
1. Marque la casilla *Mostrar tipos ocultos*.
1. Haga clic en el controlador.
1. Abra el panel *Access Control (IAM)* .
1. Haga clic en la pestaña *Asignaciones de roles*.
1. Haga clic en *Agregar* y, a continuación, en *Agregar asignación de roles*.
    * Para *Rol* seleccione *Colaborador* o *Propietario*.
    * En *Asignar acceso a*, seleccione *Usuario, grupo o entidad de servicio de Azure AD*.
    * En *Seleccionar* busque el usuario al que desea conceder permisos.
1. Haga clic en *Save*(Guardar).

## <a name="controller-create-failing-due-to-controller-name-length"></a>Error al crear el controlador debido a la longitud del nombre del controlador

### <a name="reason"></a>Motivo
El nombre de un controlador de Azure Dev Spaces no puede tener más de 31 caracteres. Si el nombre supera los 31 caracteres cuando habilite Dev Spaces en un clúster de AKS o cree un controlador, recibirá un error parecido a este:

*Failed to create a Dev Spaces controller for cluster 'a-controller-name-that-is-way-too-long-aks-east-us' (No se pudo crear un controlador de Dev Spaces para el clúster "a-controller-name-that-is-way-too-long-aks-east-us"): Azure Dev Spaces Controller name 'a-controller-name-that-is-way-too-long-aks-east-us' is invalid (El nombre del controlador de Azure Dev Spaces "a-controller-name-that-is-way-too-long-aks-east-us" no es válido). Constraint(s) violated (Restricciones infringidas): Azure Dev Spaces Controller names can only be at most 31 characters long* (Los nombres de los controladores de Azure Dev Spaces solo pueden tener 31 caracteres como máximo)

### <a name="try"></a>Probar

Cree un controlador con un nombre alternativo:

```cmd
azds controller create --name my-controller --target-name MyAKS --resource-group MyResourceGroup
```

## <a name="enabling-dev-spaces-failing-when-windows-node-pools-are-added-to-an-aks-cluster"></a>No se puede habilitar Dev Spaces cuando se agregan grupos de nodos de Windows a un clúster de AKS

### <a name="reason"></a>Motivo
Actualmente, Azure Dev Spaces está diseñado para ejecutarse solo en pods y nodos de Linux. Cuando haya un clúster de AKS con un grupo de nodos de Windows, debe asegurarse de que los pods de Azure Dev Spaces solo se programan en nodos de Linux. Si un pod de Azure Dev Spaces está programado para ejecutarse en un nodo de Windows, ese pod no se iniciará y se producirá un error al habilitar Dev Spaces.

### <a name="try"></a>Probar
[Agregue un valor taint](../aks/operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations) al clúster deAKS para asegurarse de que los pods de Linux no están programados para ejecutarse en un nodo de Windows.

## <a name="error-found-no-untainted-linux-nodes-in-ready-state-on-the-cluster-there-needs-to-be-at-least-one-untainted-linux-node-in-ready-state-to-deploy-pods-in-azds-namespace"></a>Error "Found no untainted Linux nodes in Ready state on the cluster. There needs to be at least one untainted Linux node in Ready state to deploy pods in 'azds' namespace." (No se encontraron nodos de Linux sin valor taint en estado Listo en el clúster y se necesita al menos uno para implementar pods en el espacio de nombres "azds").

### <a name="reason"></a>Motivo

Azure Dev Spaces no pudo crear un controlador en el clúster de AKS porque no encontró un nodo sin valor taint con estado *Listo* donde programar pods. Azure Dev Spaces necesita al menos un nodo de Linux en estado *Listo* que permita programar pods sin especificar tolerations.

### <a name="try"></a>Probar
[Actualice la configuración de taint](../aks/operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations) en el clúster de AKS para asegurarse de que al menos un nodo de Linux permite programar pods sin especificar tolerations. Además, asegúrese de que al menos uno de los nodos de Linux que permitan programar pods sin especificar tolerations tenga el estado *Listo*. Si el nodo está tardando mucho tiempo en alcanzar el estado *Listo*, puede probar a reiniciarlo.

## <a name="error-azure-dev-spaces-cli-not-installed-properly-when-running-az-aks-use-dev-spaces"></a>Error "Azure Dev Spaces CLI not installed properly" (La CLI de Azure Dev Spaces no está instalada correctamente) al ejecutar `az aks use-dev-spaces`

### <a name="reason"></a>Motivo
Una actualización de la CLI de Azure Dev Spaces puede haber cambiado su ruta de acceso de instalación. Si usa una versión de la CLI de Azure anterior a la 2.0.63, verá este error. Para mostrar la versión de la CLI de Azure, use `az --version`.

```bash
$ az --version
azure-cli                         2.0.60 *
...
```

A pesar del mensaje de error al ejecutar `az aks use-dev-spaces` con una versión de la CLI de Azure anterior a la 2.0.63, la instalación se realiza correctamente. Puede seguir usando `azds` sin ningún problema.

### <a name="try"></a>Probar
Actualice la instalación de la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) a la versión 2.0.63 o posterior. Esto resolverá el mensaje de error que recibe al ejecutar `az aks use-dev-spaces`. Como alternativa, puede continuar usando la versión actual de la CLI y la CLI de Azure Dev Spaces.


## <a name="horizontal-pod-autoscaling-not-working-in-a-dev-space"></a>La escalabilidad horizontal del pod no funciona en un espacio de desarrollo

### <a name="reason"></a>Motivo

Al ejecutar un servicio en un espacio de desarrollo, el pod del servicio se [inserta con contenedores adicionales para la instrumentación](how-dev-spaces-works.md#prepare-your-aks-cluster). Esos contenedores no tienen límites ni solicitudes de recursos definidos, lo que causa que el escalador automático horizontal del pod se deshabilite.

### <a name="try"></a>Probar
Ejecute el escalador automático horizontal del podo en un espacio de nombres que no tenga Dev Spaces habilitado.
