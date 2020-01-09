---
title: Creación de funciones de Azure en Linux mediante una imagen personalizada
description: Aprenda a crear funciones de Azure que se ejecutan en una imagen de Linux personalizada.
ms.date: 09/27/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 5a7fbecca2dc7585ff7110d53deccbbbbf23087c
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2019
ms.locfileid: "75551495"
---
# <a name="create-a-function-on-linux-using-a-custom-image"></a>Creación de una función en Linux con una imagen personalizada

Azure Functions le permite hospedar sus funciones en Linux en su propio contenedor personalizado. También puede [hospedarlas en un contenedor de Azure App Service predeterminado](functions-create-first-azure-function-azure-cli-linux.md). Esta funcionalidad requiere [el entorno de ejecución de Functions 2.x](functions-versions.md).

En este tutorial, aprenderá a implementar funciones en Azure como una imagen de Docker personalizada. Este patrón es útil cuando es necesario personalizar la imagen de contenedor integrada. Es posible que quiera usar una imagen personalizada cuando las funciones necesiten una versión de idioma determinada o requieran una dependencia o configuración específica que no se proporciona en la imagen integrada. Se pueden encontrar imágenes base admitidas para Azure Functions en el [repositorio de imágenes base de Azure Functions](https://hub.docker.com/_/microsoft-azure-functions-base). 

En este tutorial se explica cómo usar Azure Functions Core Tools para crear una función en una imagen de Linux personalizada. Esta imagen se publica en una aplicación de función de Azure, que se creó mediante la CLI de Azure. Más adelante, actualizará la función para conectarse a Azure Queue Storage. También la habilitará.  

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una aplicación de función y un archivo Dockerfile mediante Core Tools.
> * Crear una imagen personalizada con Docker
> * Publicar una imagen personalizada en un registro de contenedor
> * Cree una cuenta de Azure Storage.
> * Cree un plan de hospedaje premium.
> * Implementar una aplicación de función desde Docker Hub
> * Agregar la configuración de aplicación a la aplicación de función
> * Habilite la implementación continua.
> * Habilitar las conexiones SSH al contenedor.
> * Agregar un enlace de salida de Queue Storage. 

Los pasos siguientes se admiten en equipos Mac, Windows o Linux. 

## <a name="prerequisites"></a>Prerequisites

Antes de ejecutar este ejemplo, debe tener lo siguiente:

* Instale [la versión 2.x de Azure Core Tools](functions-run-local.md#v2).

* Instale la [CLI de Azure]( /cli/azure/install-azure-cli). En este artículo se necesita la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene.  
También puede usar [Azure Cloud Shell](https://shell.azure.com/bash).

* Una suscripción de Azure activa.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-cloud-shell-note](../../includes/functions-cloud-shell-note.md)]

## <a name="create-the-local-project"></a>Creación del proyecto local

Ejecute el comando siguiente desde la línea de comandos para crear un proyecto de aplicación de función en la carpeta `MyFunctionProj` del directorio local actual. Para un proyecto de Python, [la ejecución se debe realizar en un entorno virtual](functions-create-first-function-python.md#create-and-activate-a-virtual-environment).

```bash
func init MyFunctionProj --docker
```

Al incluir la opción `--docker`, se genera un archivo Dockerfile para el proyecto. Dicho archivo se usa para crear un contenedor personalizado en el que se ejecuta el proyecto. La imagen base que se usa depende del lenguaje del runtime de trabajo que se elija.  

Cuando se le solicite, elija un runtime de trabajo en los siguientes lenguajes:

* `dotnet`: crea un proyecto de biblioteca de clases .NET (.csproj).
* `node`: crea un proyecto JavaScript.
* `python`: crea un proyecto de Python.  

Use el siguiente comando para ir a la nueva carpeta del proyecto `MyFunctionProj`.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

## <a name="build-from-the-docker-file"></a>Compilación desde el archivo de Docker

Eche un vistazo al archivo _Dockerfile_ en la carpeta raíz del proyecto. Este archivo describe el entorno que es necesario para ejecutar la aplicación de función en Linux. El ejemplo siguiente es un archivo Dockerfile que crea un contenedor que ejecuta una aplicación de función en el runtime de trabajo de JavaScript (Node.js): 

```Dockerfile
FROM mcr.microsoft.com/azure-functions/node:2.0

ENV AzureWebJobsScriptRoot=/home/site/wwwroot
COPY . /home/site/wwwroot
```

> [!NOTE]
> La lista completa de imágenes base admitidas para Azure Functions se puede encontrar en la [página de imágenes base de Azure Functions](https://hub.docker.com/_/microsoft-azure-functions-base).

### <a name="run-the-build-command"></a>Ejecute el comando `build`.

En la carpeta raíz, ejecute el comando [docker build](https://docs.docker.com/engine/reference/commandline/build/) y especifique un nombre, `mydockerimage`, y una etiqueta, `v1.0.0`. Reemplace `<docker-id>` por el identificador de su cuenta de Docker Hub. Este comando compila la imagen de Docker del contenedor.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Cuando el comando se complete, podrá ejecutar el nuevo contenedor de forma local.

### <a name="run-the-image-locally"></a>Ejecución de la imagen de manera local
Compruebe que la imagen que ha creado funciona mediante la ejecución de la imagen de Docker en un contenedor local. Emita el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) y transmítale el nombre y la etiqueta de la imagen. Asegúrese de especificar el puerto con el argumento `-p`.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Una vez que la imagen personalizada ya se ejecuta en un contenedor de Docker local, compruebe que la aplicación de función y el contenedor funcionan correctamente; para ello, vaya a <http://localhost:8080>.

![Ejecute la aplicación de función de forma local.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

> [!NOTE]
> En este momento, cuando intente llamar a su función HTTP específica, obtendrá una respuesta de error HTTP 401. Esto se debe a que la función se ejecuta en el contenedor local como lo haría en Azure, lo que significa que se requiere la clave de función. Dado que el contenedor todavía no se ha publicado en una aplicación de función, no hay ninguna clave de función disponible. Más adelante verá que, al usar las herramientas principales para publicar el contenedor, se le mostrarán las teclas de función. Si desea probar la función que se ejecuta en el contenedor local, puede cambiar la [clave de autorización](functions-bindings-http-webhook.md#authorization-keys) a `anonymous`. 

Después de haber comprobado la aplicación de función en el contenedor, detenga la ejecución. Ahora, puede insertar la imagen personalizada en su cuenta de Docker Hub.

## <a name="push-to-docker-hub"></a>Inserción en Docker Hub

Un registro es una aplicación que hospeda imágenes y proporciona servicios de imágenes y servicios de contenedores. Para compartir la imagen, debe insertarla en un registro. Docker Hub es un registro para imágenes de Docker que le permite hospedar sus propios repositorios, ya sean públicos o privados.

Antes de poder insertar una imagen, debe iniciar sesión en Docker Hub mediante el comando [docker login](https://docs.docker.com/engine/reference/commandline/login/). Reemplace `<docker-id>` por el nombre de cuenta y escriba la contraseña en la consola en el símbolo del sistema. Para ver otras opciones de contraseña de Docker Hub, consulte la [documentación del comando docker login](https://docs.docker.com/engine/reference/commandline/login/).

```bash
docker login --username <docker-id>
```

Un mensaje de inicio de sesión correcto confirmará que inició sesión. Cuando haya iniciado sesión, inserte la imagen en Docker Hub mediante el comando [docker push](https://docs.docker.com/engine/reference/commandline/push/).

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Una vez completada correctamente la inserción, puede usar esta imagen como origen de implementación para una nueva aplicación de función en Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-premium-plan"></a>Creación de un plan Premium

Hospedaje de Linux para contenedores de Functions personalizados admitidos en [planes dedicados (App Service)](functions-scale.md#app-service-plan) y [planes premium](functions-premium-plan.md#features). En este tutorial se usa un plan premium, que se puede ampliar según sea necesario. Para obtener más información sobre el hospedaje, consulte [Comparación de los planes de hospedaje de Azure Functions](functions-scale.md).

En el ejemplo siguiente se crea un plan premium llamado `myPremiumPlan` en el plan de tarifa **Elastic Premium 1** (`--sku EP1`), en la región de Oeste de EE. UU. (`-location WestUS`), y en un contenedor de Linux (`--is-linux`).

```azurecli-interactive
az functionapp plan create --resource-group myResourceGroup --name myPremiumPlan \
--location WestUS --number-of-workers 1 --sku EP1 --is-linux
```

## <a name="create-an-app-from-the-image"></a>Creación de una aplicación a partir de la imagen

La aplicación de función administra la ejecución de las funciones en el plan de hospedaje. Cree una aplicación de función a partir de una imagen de Docker Hub mediante el comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create).

En el siguiente comando, sustituya un nombre de aplicación de función único donde vea el marcador de posición `<app_name>` y el nombre de la cuenta de almacenamiento para `<storage_name>`. El `<app_name>` se usa como el dominio DNS predeterminado para la Function App y, por ello, el nombre debe ser único en todas las aplicaciones de Azure. Al igual que antes, `<docker-id>` es el nombre de cuenta de Docker.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myPremiumPlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0
```

El parámetro _deployment-container-image-name_ indica la imagen hospedada en Docker Hub que se usará para crear la aplicación de función. Use el comando [az functionapp config container show](/cli/azure/functionapp/config/container#az-functionapp-config-container-show) para ver información sobre la imagen usada en la implementación. Use el comando [az functionapp config container set](/cli/azure/functionapp/config/container#az-functionapp-config-container-set) para realizar la implementación desde una imagen diferente.

## <a name="configure-the-function-app"></a>Configuración de la Function App

La función necesita la cadena de conexión para conectarse a la cuenta de almacenamiento predeterminada. Al publicar la imagen personalizada en una cuenta de contenedor privada, debe establecer los valores de la aplicación como variables de entorno en Dockerfile mediante la [instrucción ENV](https://docs.docker.com/engine/reference/builder/#env), o algo similar.

En este caso, `<storage_name>` es el nombre de la cuenta de almacenamiento que ha creado. Obtenga la cadena de conexión con el comando [az storage account show-connection-string](/cli/azure/storage/account). Agregue esta configuración de aplicación a la Function App con el comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set).

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_name> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings AzureWebJobsStorage=$storageConnectionString
```

> [!NOTE]
> Si el contenedor es privado, tendría que establecer también la siguiente configuración de aplicación  
> - DOCKER_REGISTRY_SERVER_USERNAME  
> - DOCKER_REGISTRY_SERVER_PASSWORD  
>
> Tendrá que detener y, a continuación, iniciar la aplicación de función para que se tomen estos valores

## <a name="verify-your-functions"></a>Comprobación de las funciones

<!-- we should replace this with a CLI or API-based approach, when we get something better than REST -->

La función desencadenada por HTTP que ha creado requiere una [clave de función](functions-bindings-http-webhook.md#authorization-keys) al llamar al punto de conexión. En este momento, la forma más fácil de obtener la dirección URL de la función, incluida la clave, es desde [Azure Portal]. 

> [!TIP]
> También puede obtener las claves de función mediante las [API de administración de claves](https://github.com/Azure/azure-functions-host/wiki/Key-management-API), que hacen que sea necesario presentar un [token de portador para la autenticación](/cli/azure/account#az-account-get-access-token).

Busque la nueva aplicación de función en [Azure Portal] escribiendo el nombre de la aplicación de función en el cuadro **Buscar** en la parte superior de la página y seleccionando el recurso **App Service**.

Seleccione la función **MyHttpTrigger**, seleccione **</> Obtener la dirección URL de la función** > **predeterminado (clave de función)**  > **Copiar**.

![Copiar la dirección URL de la función desde Azure Portal](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key.png)

En esta dirección URL, la clave de función es el parámetro de consulta `code`. 

> [!NOTE]  
> Dado que la aplicación de función se implementa como un contenedor, no se pueden realizar cambios en el código de la función en el portal. En su lugar, debe actualizar el proyecto en el contenedor local y volver a publicarlo en Azure.

Pegue la dirección URL de la función en la barra de direcciones de su explorador. Anexe el valor `&name=<yourname>` de la cadena de consulta al final de esta dirección URL y presione la tecla `Enter` en el teclado para ejecutar la solicitud. Debería ver la respuesta devuelta por la función mostrada en el explorador.

El ejemplo siguiente muestra la respuesta en el explorador:

![Respuesta de la función en el explorador.](./media/functions-create-function-linux-custom-image/function-app-browser-testing.png)

La dirección URL de la solicitud incluye una clave que, de forma predeterminada, es necesaria para tener acceso a la función a través de HTTP. 

## <a name="enable-continuous-deployment"></a>Habilitación de la implementación continua

Una de las ventajas de usar contenedores es la compatibilidad con la implementación continua. Las funciones le permiten implementar automáticamente las actualizaciones cuando el contenedor se actualiza en el registro. Habilite la implementación continua con el comando [az functionapp deployment container config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config).

```azurecli-interactive
az functionapp deployment container config --enable-cd \
--query CI_CD_URL --output tsv \
--name <app_name> --resource-group myResourceGroup
```

Después de habilitar la implementación continua, este comando devuelve la dirección URL del webhook de implementación. También puede usar el comando [az functionapp deployment container show-cd-url](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-show-cd-url) para devolver esta dirección URL. 

Copie la dirección URL de implementación y vaya a su repositorio de DockerHub, elija la pestaña **Webhooks**, escriba el **nombre del webhook**, pegue la dirección URL en **Dirección URL de webhook** y, luego, elija el signo más ( **+** ).

![Agregar el webhook al repositorio de DockerHub](./media/functions-create-function-linux-custom-image/dockerhub-set-continuous-webhook.png)  

Una vez definido el webhook, cuando se actualiza la imagen vinculada en DockerHub, la aplicación de función descarga e instala la imagen más reciente.

## <a name="enable-ssh-connections"></a>Habilitación de las conexiones SSH

SSH habilita la comunicación segura entre un contenedor y un cliente. Con SSH habilitado, puede conectarse al contenedor mediante las herramientas avanzadas de App Service (Kudu). Para facilitar la conexión al contenedor mediante SSH, las funciones proporcionan una imagen base que ya tiene SSH habilitado. 

### <a name="change-the-base-image"></a>Modificación de la imagen base

En el archivo dockerfile, anexe la cadena `-appservice` a la imagen base de la instrucción `FROM`, que para un proyecto de JavaScript tiene un aspecto parecido al siguiente.

```docker
FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
```

Las diferencias en las dos imágenes base permiten las conexiones SSH en el contenedor. Estas diferencias se detallan en [tutorial de App Services](../app-service/containers/tutorial-custom-docker-image.md#enable-ssh-connections).

### <a name="rebuild-and-redeploy-the-image"></a>Recompilación y reimplementación de la imagen

En la carpeta raíz, vuelva a ejecutar el comando [docker build](https://docs.docker.com/engine/reference/commandline/build/), como antes, y reemplace `<docker-id>` por el identificador de la cuenta de Docker Hub. 

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Inserte la imagen actualizada de nuevo en Docker Hub.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

La imagen actualizada se vuelve a implementar en la aplicación de funciones.

### <a name="connect-to-your-container-in-azure"></a>Conexión con su contenedor en Azure

En el explorador, vaya al siguiente punto de conexión `scm.` de las herramientas avanzadas (Kudu) del contenedor de aplicaciones de funciones y reemplace `<app_name>` por el nombre de la aplicación de funciones.

```
https://<app_name>.scm.azurewebsites.net/
```

Inicie sesión en su cuenta de Azure y, después, seleccione la pestaña **SSH** para crear una conexión SSH al contenedor.

Una vez establecida la conexión, ejecute el comando `top` para ver los procesos que se están ejecutando actualmente. 

![Comando top de Linux en ejecución en una sesión SSH.](media/functions-create-function-linux-custom-image/linux-custom-kudu-ssh-top.png)

## <a name="write-to-queue-storage"></a>Escritura en Queue Storage

Functions le permite conectar servicios de Azure y otros recursos a funciones sin tener que escribir su propio código de integración. Estos *enlaces*, que representan la entrada y la salida, se declaran dentro de la definición de función. Los datos de los enlaces se proporcionan a la función como parámetros. Un *desencadenador* es un tipo especial de enlace de entrada. Si bien una función tiene un único desencadenador, puede tener varios enlaces de entrada y salida. Para más información, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

En esta sección se muestra cómo integrar la función con una cola de Azure Storage. El enlace de salida que se agrega a esta función escribe datos de una solicitud HTTP en un mensaje de la cola.

### <a name="download-the-function-app-settings"></a>Descarga de la configuración de la aplicación de función

[!INCLUDE [functions-app-settings-download-local-cli](../../includes/functions-app-settings-download-local-cli.md)]

### <a name="enable-extension-bundles"></a>Habilitación de conjuntos de extensiones

Dado que está utilizando un enlace de salida de Queue Storage, debe tener la extensión de enlaces de Storage instalada antes de ejecutar el proyecto. 


# <a name="javascript--pythontabnodejspython"></a>[JavaScript/Python](#tab/nodejs+python)

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

A excepción de los desencadenadores HTTP y el temporizador, los enlaces se implementan como paquetes de extensión. Ejecute el siguiente comando [dotnet add package](/dotnet/core/tools/dotnet-add-package) en la ventana Terminal para agregar el paquete de extensión de Storage al proyecto.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

> [!TIP]
> Si utiliza Visual Studio, también puede usar el administrador de paquetes NuGet para agregar este paquete.

---

Ahora podrá agregar el enlace de salida de Storage al proyecto.

### <a name="add-an-output-binding"></a>Adición de un enlace de salida

En Functions, para cada tipo de enlace es necesario definir los elementos `direction`, `type` y un valor único de `name` en el archivo function.json. La manera de definir estos atributos depende del lenguaje de la aplicación de funciones.

# <a name="javascript--pythontabnodejspython"></a>[JavaScript/Python](#tab/nodejs+python)

[!INCLUDE [functions-add-output-binding-json](../../includes/functions-add-output-binding-json.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]

---

### <a name="add-code-that-uses-the-output-binding"></a>Adición de código que utilice el enlace de salida

Una vez definido el enlace, podrá usar el valor de `name` de este para acceder a él como atributo en la firma de función. Con un enlace de salida, no tiene que usar el código del SDK de Azure Storage para autenticarse, obtener una referencia de cola o escribir datos. El sistema en tiempo de ejecución de Functions y el enlace de salida de cola realizan esas tareas automáticamente.

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

[!INCLUDE [functions-add-output-binding-js](../../includes/functions-add-output-binding-js.md)]

# <a name="pythontabpython"></a>[Python](#tab/python)

[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]

---

### <a name="update-the-hosted-container"></a>Actualización del contenedor hospedado

En la carpeta raíz, ejecute de nuevo el comando [docker build](https://docs.docker.com/engine/reference/commandline/build/) y, en esta ocasión, actualice la versión de la etiqueta a `v1.0.2`. Igual que antes, reemplace `<docker-id>` por el identificador de la cuenta de Docker Hub. 

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Inserte la imagen actualizada de nuevo en el repositorio.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

### <a name="verify-the-updates-in-azure"></a>Comprobación de las actualizaciones en Azure

Use la misma dirección URL que antes desde el explorador para desencadenar la función. Debería ver la misma respuesta. Sin embargo, esta vez, la cadena que se pasa como parámetro `name` se escribe en la cola de almacenamiento `outqueue`.

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado correctamente el contenedor personalizado en una aplicación de funciones en Azure, considere la posibilidad de conocer más información sobre los temas siguientes:

+ [Supervisión de funciones](functions-monitoring.md)
+ [Opciones de escalado y hospedaje](functions-scale.md)
+ [Hospedaje sin servidor basado en Kubernetes](functions-kubernetes-keda.md)

[Azure Portal]: https://portal.azure.com
