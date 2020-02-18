---
title: Creación de una función de Python sin servidor para solicitudes HTTP en Azure Functions
description: Cree e implemente código Python sin servidor en la nube mediante Azure Functions.
ms.date: 02/11/2020
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: a781e10cee4cf433de5e837490d901020a875205
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77157897"
---
# <a name="quickstart-create-a-python-function-in-azure-that-responds-to-http-requests"></a>Inicio rápido: Creación de una función de Python en Azure que responda a solicitudes HTTP

En este artículo se usan herramientas de línea de comandos para crear una función de Python que responda a solicitudes HTTP. Después de probar el código localmente, se implementa en el entorno sin servidor de Azure Functions. Este inicio rápido supone un pequeño costo en su cuenta de Azure.

También hay una [versión basada en Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-python) de este artículo.

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure Functions Core Tools](./functions-run-local.md#v2), versión 2.7.1846, o cualquier versión posterior.
- La [CLI de Azure](/cli/azure/install-azure-cli), versión 2.0.76, o cualquier versión posterior. 
- [Python 3.7.4 de 64 bits](https://www.python.org/downloads/release/python-374/). (Python 3.7.4 se ha comprobado con Azure Functions; Python 3.8 y las versiones posteriores aún no son compatibles).

### <a name="prerequisite-check"></a>Comprobación de requisitos previos

1. En una ventana de terminal o de comandos, ejecute `func --version` para comprobar que la versión de Azure Functions Core Tools es 2.7.1846 u otra posterior.
1. Ejecute `az --version` para comprobar que la versión de la CLI de Azure es 2.0.76 o posterior.
1. Ejecute `az login` para iniciar sesión en Azure y comprobar una suscripción activa.
1. Ejecute `python --version` (Linux/MacOS) o `py --version` (Windows) para comprobar los informes de la versión de Python 3.7.x.

## <a name="create-and-activate-a-virtual-environment"></a>Creación y activación de un entorno virtual

En una carpeta adecuada, ejecute los comandos siguientes para crear y activar un entorno virtual denominado `.venv`. Asegúrese de usar Python 3.7, que es compatible con Azure Functions.


# <a name="bashtabbash"></a>[bash](#tab/bash)

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Si Python no instaló el paquete venv en la distribución de Linux, ejecute el siguiente comando:

```bash
sudo apt-get install python3-venv
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)

```cmd
py -m venv .venv
```

```cmd
.venv\scripts\activate
```

---

Ejecute todos los comandos siguientes en este entorno virtual activado. (Para salir del entorno virtual, ejecute `deactivate`).

## <a name="create-a-local-function-project"></a>Creación de un proyecto de función local

En Azure Functions, un proyecto de función es un contenedor para una o varias funciones individuales que responden a un desencadenador específico. Todas las funciones de un proyecto comparten las mismas configuraciones locales y de hospedaje. En esta sección, se crea un proyecto de función que contiene una sola función.

1. En el entorno virtual, ejecute el comando `func init` para crear un proyecto de funciones en una carpeta llamada *LocalFunctionProj* con el runtime especificado:

    ```
    func init LocalFunctionProj --python
    ```
    
    Esta carpeta contiene varios archivos del proyecto, incluidos los archivos de configuración [local.settings.json](functions-run-local.md#local-settings-file) y [host.json](functions-host-json.md). Como *local.settings.json* puede contener secretos descargados de Azure, el archivo se excluye del control de código fuente de forma predeterminada en el archivo *.gitignore*.

    > [!TIP]
    > Como un proyecto de función está asociado a un entorno de ejecución específico, todas las funciones del proyecto deben estar escritas con el mismo lenguaje.

1. Vaya a la carpeta del proyecto:

    ```
    cd LocalFunctionProj
    ```
    
1. Agregue una función al proyecto mediante el comando siguiente, donde el argumento `--name` es el nombre único de la función y el argumento `--template` especifica el desencadenador de esta. `func new` crea una subcarpeta que coincide con el nombre de la función y que contiene un archivo de código apropiado para el lenguaje elegido del proyecto y un archivo de configuración denominado *function.json*.

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

### <a name="optional-examine-the-file-contents"></a>(Opcional) Examen del contenido del archivo

Si lo desea, puede ir a [Ejecución local de la función](#run-the-function-locally) y examine el contenido del archivo posteriormente.

#### <a name="__init__py"></a>\_\_init\_\_.py

*\_\_init\_\_.py* contiene una función `main()` de Python que se desencadena en función de la configuración de *function.json*.

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

En el caso del desencadenador HTTP, la función recibe los datos de la solicitud en la variable `req` tal como se definen en *function.json*. `req` es una instancia de la [clase azure.functions.HttpRequest](/python/api/azure-functions/azure.functions.httprequest). El objeto devuelto, definido como `$return` en *function.json*, es una instancia de la [clase azure.functions.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse). Para más información, vea [Enlaces y desencadenadores HTTP de Azure Functions](functions-bindings-http-webhook.md).

#### <a name="functionjson"></a>function.json

*function.json* es un archivo de configuración que define la entrada y salida `bindings` de la función, lo que incluye el tipo de desencadenador. Si lo desea, puede cambiar `scriptFile` para invocar otro un archivo de Python.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

Cada enlace requiere una dirección, un tipo y un nombre único. El desencadenador HTTP tiene un enlace de entrada de tipo [`httpTrigger`](functions-bindings-http-webhook.md#trigger) y un enlace de salida de tipo [`http`](functions-bindings-http-webhook.md#output).


## <a name="run-the-function-locally"></a>Ejecución local de la función

Inicie la función, iniciando para ello el host del entorno de ejecución de Azure Functions local en la carpeta *LocalFunctionProj*:

```
func start
```

Debería aparecer la siguiente salida (si HttpExample no aparece como se muestra a continuación, es probable que haya iniciado el host desde la carpeta *HttpExample*. En ese caso, use **Ctrl**+**C** para detener el host, vaya a la carpeta *LocalFunctionProj* primaria y vuelva a ejecutar `func start`).

```output
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
```

Copie la dirección URL de la función `HttpExample` de esta salida en un explorador y anexe la cadena de consulta `?name=<your-name>`, lo que hará que la dirección URL completa sea `http://localhost:7071/api/HttpExample?name=Functions`. El explorador debe mostrar un mensaje como este `Hello Functions`:

![Resultado de la ejecución de la función localmente en el explorador](./media/functions-create-first-function-python/function-test-local-browser.png)

El terminal en el que ejecutó `func start` también muestra la salida del registro cuando realiza solicitudes.

Cuando esté listo, presione **Ctrl**+**C** para detener el host de Functions.

## <a name="create-supporting-azure-resources-for-your-function"></a>Creación de recursos auxiliares de Azure para la función

Antes de poder implementar el código de la función en Azure, debe crear tres recursos:

- Un grupo de recursos, que es un contenedor lógico de recursos relacionados.
- Una cuenta de almacenamiento, que mantiene el estado e información adicional sobre los proyectos.
- Una aplicación de funciones de Azure, que proporciona el entorno para ejecutar el código de las funciones. Una aplicación de funciones se asigna al proyecto de funciones y le permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos.

Puede utilizar comandos de la CLI de Azure para crear estos elementos. Cada comando proporciona una salida JSON al finalizar.

1. Si aún no lo ha hecho, inicie sesión en Azure con el comando [az login](/cli/azure/reference-index#az-login):

    ```azurecli
    az login
    ```
    
1. Para crear un grupo de recursos, use el comando [az group create](/cli/azure/group#az-group-create). En el ejemplo siguiente se crea un grupo de recursos denominado `AzureFunctionsQuickstart-rg` en la región `westeurope`. (Por lo general, tanto los grupos de recursos como los recursos se crean en una región cercana y se utiliza alguna de las regiones disponibles del comando `az account list-locations`).

    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location westeurope
    ```
    
    > [!NOTE]
    > No se pueden hospedar aplicaciones de Windows y Linux en el mismo grupo de recursos. Si tiene un grupo de recursos existente llamado `AzureFunctionsQuickstart-rg` con una aplicación web o aplicación de función de Windows, debe usar un grupo de recursos diferente.
    
1. Cree una cuenta de almacenamiento de uso general en el grupo de recursos con el comando [az storage account create](/cli/azure/storage/account#az-storage-account-create). En el siguiente ejemplo, reemplace `<storage_name>` por un nombre único global que sea adecuado. Los nombres deben contener entre 3 y 24 caracteres y solo letras minúsculas. `Standard_LRS` especifica una cuenta típica de uso general.

    ```azurecli
    az storage account create --name <storage_name> --location westeurope --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```
    
    La cuenta de almacenamiento incurrirá solo en un costo insignificante para este inicio rápido.
    
1. Cree la aplicación de funciones con el comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create). En el ejemplo siguiente, reemplace `<storage_name>` por el nombre de la cuenta que usó en el paso anterior y `<app_name>` por un nombre único global que le resulte adecuado. `<app_name>` también es el dominio DNS predeterminado de la aplicación de función.

    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --os-type Linux --consumption-plan-location westeurope --runtime python --name <app_name> --storage-account <storage_name>
    ```
    
    Este comando crea una aplicación de funciones que ejecuta el runtime del lenguaje especificado en el [plan de consumo de Azure Functions](functions-scale.md#consumption-plan), que es gratuito para la cantidad de uso que se realiza aquí. El comando también aprovisiona una instancia asociada de Azure Application Insights en el mismo grupo de recursos con la que puede supervisar la aplicación de funciones y ver registros. Para más información, consulte [Supervisión de Azure Functions](functions-monitoring.md). La instancia no incurrirá en ningún costo hasta que se active.
    
## <a name="deploy-the-function-project-to-azure"></a>Implementación del proyecto de función en Azure

Con los recursos necesarios en vigor, ya está listo para implementar el proyecto de funciones local en la aplicación de funciones de Azure mediante el comando [func azure functionapp publish](functions-run-local.md#project-file-deployment). En el ejemplo siguiente, reemplace `<app_name>` por el nombre de su aplicación.

```
func azure functionapp publish <app_name>
```

Si aparece el error "Can't find app with name..." (No se encuentra la aplicación...), espere unos segundos e inténtelo de nuevo, ya que es posible que Azure no haya inicializado completamente la aplicación después del comando `az functionapp create` anterior.

El comando de publicación muestra un resultado similar a lo que se muestra a continuación (se ha truncado para que resulte más simple):

```output
Getting site publishing info...
Creating archive for current directory...
Performing remote build for functions project.

...

Deployment successful.
Remote build succeeded!
Syncing triggers...
Functions in msdocs-azurefunctions-qs:
    HttpExample - [httpTrigger]
        Invoke url: https://msdocs-azurefunctions-qs.azurewebsites.net/api/httpexample?code=KYHrydo4GFe9y0000000qRgRJ8NdLFKpkakGJQfC3izYVidzzDN4gQ==
```

## <a name="invoke-the-function-on-azure"></a>Invocación de la función en Azure

Como la función usa un desencadenador HTTP, para invocarla es preciso realizar una solicitud HTTP a su dirección URL en el explorador o con una herramienta como CURL. En ambas instancias, el parámetro de dirección URL `code` es la clave de función exclusiva única que autoriza la invocación con el punto de conexión de la función.

# <a name="browsertabbrowser"></a>[Browser](#tab/browser)

Copie el valor completo de **Invoke URL** que se muestra en la salida del comando de publicación en una barra de direcciones del explorador, anexando el parámetro de consulta `&name=Azure`. El explorador debe mostrar una salida similar a cuando ejecutó la función localmente.

![La salida de la función ejecutada en Azure en un explorador](./media/functions-create-first-function-python/function-test-cloud-browser.png)


# <a name="curltabcurl"></a>[curl](#tab/curl)

Ejecute [curl](https://curl.haxx.se/) con la **dirección URL de invocación**  y anexe el parámetro `&name=Azure`. El resultado del comando debería ser el texto "Hello Azure".

![La salida de la función ejecutada en Azure en un mediante curl](./media/functions-create-first-function-python/function-test-cloud-curl.png)

---

> [!TIP]
> Para ver los registros casi en tiempo real de una aplicación de Python publicada, utilice [Live Metrics Stream de Application Insights](functions-monitoring.md#streaming-logs).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si continúa con el paso siguiente, [Adición de un enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-python.md), conserve todos los recursos intactos, ya que va a crear a partir de lo que ya ha hecho.

De lo contrario, use el siguiente comando para eliminar el grupo de recursos y todos los recursos que contiene para evitar incurrir en costos adicionales.

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de un enlace de salida de la cola de Azure Storage](functions-add-output-binding-storage-queue-python.md)
