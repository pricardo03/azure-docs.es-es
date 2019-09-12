---
title: Creación e implementación de Azure Functions en Python con Visual Studio Code
description: Cómo usar la extensión de Visual Studio Code para Azure Functions para crear funciones sin servidor en Python e implementarlas en Azure.
services: functions
author: ggailey777
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: glenga
ms.openlocfilehash: 43fee2ce25e358bbcff915d2fbef96bf4b7c1a0c
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70233107"
---
# <a name="deploy-python-to-azure-functions-with-visual-studio-code"></a>Implementación de Python en Azure Functions con Visual Studio Code

En este tutorial, usará Visual Studio Code y la extensión de Azure Functions para crear un punto de conexión HTTP sin servidor con Python y agregar también una conexión (o "enlace") al almacenamiento. Azure Functions ejecuta el código en un entorno sin servidor sin necesidad de aprovisionar una máquina virtual ni publicar una aplicación web. La extensión de Azure Functions para Visual Studio Code simplifica considerablemente el proceso de uso de Functions mediante la administración automática de muchos trabajos de configuración.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Instalación de la extensión de Azure Functions
> * Crear una función desencadenada mediante HTTP
> * Depuración local
> * Sincronizar la configuración de la aplicación
> * Ver los registros de streaming
> * Conectar a Azure Storage

Si tiene problemas con cualquiera de los pasos de este tutorial, nos encantaría conocer los detalles. Use el botón **He tenido un problema** que se encuentra al final de cada sección para enviar comentarios detallados.

## <a name="prerequisites"></a>Requisitos previos

- Una [suscripción de Azure](#azure-subscription).
- [Visual Studio Code con la extensión de Azure Functions](#visual-studio-code-python-and-the-azure-functions-extension).
- [Azure Functions Core Tools](#azure-functions-core-tools).

### <a name="azure-subscription"></a>Suscripción de Azure

Si no tiene una suscripción de Azure, [regístrese ahora](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension) para obtener una cuenta gratuita de 30 días con 200 $ en créditos de Azure para probar cualquier combinación de servicios.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code, Python y la extensión de Azure Functions

Instale el siguiente software:

- Python 3.6.x tal como requiere Azure Functions. [Python 3.6.8](https://www.python.org/downloads/release/python-368/) es la versión 3.6.x más reciente.
- [Visual Studio Code](https://code.visualstudio.com/)
- La [extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python), como se describe en [Tutorial de Python en Visual Studio Code: requisitos previos](https://code.visualstudio.com/docs/python/python-tutorial).
- La [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). Para información general, visite [el repositorio de GitHub vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Siga las instrucciones para su sistema operativo en [Trabajo con Azure Functions Core Tools](functions-run-local.md#v2). Las herramientas están escritas en .NET Core y el paquete Core Tools se instala mejor con el administrador de paquetes de Node.js, npm, que es el motivo por el que necesita instalar .NET Core y Node.js en la actualidad, incluso para el código de Python. Sin embargo, puede omitir el requisito de .NET Core con el uso de "conjuntos de extensión", tal y como se describe en la documentación mencionada anteriormente. Sea cual sea el caso, debe instalar estos componentes una sola vez, después de lo cual Visual Studio Code le pedirá automáticamente que instale las actualizaciones.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Una vez instalada la extensión de Functions, inicie sesión en su cuenta de Azure; para ello, vaya al explorador de **Azure: Functions**, seleccione **Iniciar sesión en Azure** y siga las indicaciones.

![Inicio de sesión en Azure mediante Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-sign-in.png)

Después de iniciar sesión, compruebe que la cuenta de correo electrónico de la suscripción de Azure aparece en la barra de estado:

![Barra de estado de Visual Studio Code que muestra la cuenta de Azure](media/tutorial-vs-code-serverless-python/azure-account-status-bar.png)

El nombre que ha asignado a la suscripción también aparece en el explorador de **Azure: Functions** ("Primary" en la imagen siguiente):

![Explorador de Azure App Service de Visual Studio Code que muestra las suscripciones](media/tutorial-vs-code-serverless-python/azure-subscription-view.png)

> [!NOTE]
> Si aparece el error **"No se encuentra la suscripción con el nombre [identificador de suscripción]"** , puede deberse a que está detrás de un servidor proxy y no puede acceder a la API de Azure. Configure las variables de entorno `HTTP_PROXY` y `HTTPS_PROXY` con la información del servidor proxy en el terminal:
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```ps
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```

### <a name="verify-prerequisites"></a>Verificar los requisitos previos

Para comprobar que están instaladas todas las herramientas de Azure Functions, abra la paleta de comandos de Visual Studio Code (F1), seleccione el comando **Terminal: crear nuevo terminal integrado** y, una vez que se abre el terminal, ejecute el comando `func`:

![Comprobación de los requisitos previos de Azure Functions Core Tools](media/tutorial-vs-code-serverless-python/check-prereqs.png)

La salida que empieza con el logotipo de Azure Functions (debe desplazarse hacia arriba en la salida) indica que Azure Functions Core Tools está presente.

Si no se reconoce el comando `func`, compruebe que la carpeta en la que instaló Azure Functions Core Tools está incluida en la variable de entorno PATH.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=01-verify-prerequisites)

## <a name="create-the-function"></a>Creación de la función

1. El código de Azure Functions se administra dentro de un _proyecto_ de Functions, que se crea antes de crear el código. En el explorador de **Azure: Functions** (se abre con el icono de Azure en el lado izquierdo), seleccione el icono del comando **Nuevo proyecto** o abra la paleta de comandos (F1 ) y seleccione **Azure Functions: Crear nuevo proyecto**.

    ![Botón Crear nuevo proyecto en el explorador de Functions](media/tutorial-vs-code-serverless-python/project-create-new.png)

1. En las peticiones siguientes:

    | Prompt | Valor | DESCRIPCIÓN | 
    | --- | --- | --- |
    | Especifique una carpeta para el proyecto | Carpeta abierta actual | Carpeta en la que se va a crear el proyecto. Puede que desee crear el proyecto en una subcarpeta. |
    | Seleccionar el lenguaje para el proyecto de la aplicación de funciones | **Python** | Lenguaje que se va a usar para la función, lo que determina la plantilla que se usa para el código. |
    | Seleccionar una plantilla para la primera función de su proyecto | **desencadenador HTTP** | Una función que usa un desencadenador HTTP se ejecuta cuando se realiza una solicitud HTTP al punto de conexión de la función. (Existen otros desencadenadores para Azure Functions. Para más información, consulte [¿Qué puedo hacer con las funciones?](functions-overview.md#what-can-i-do-with-functions)). |
    | Proporcionar un nombre de función | HttpExample | El nombre se usa para una subcarpeta que contiene el código de la función junto con los datos de configuración y también define el nombre del punto de conexión HTTP. Use "HttpExample" en lugar de aceptar el valor predeterminado "HTTPTrigger" para distinguir la propia función del desencadenador. |
    | Nivel de autorización | **Function** | Las llamadas realizadas al punto de conexión de la función requieren una [clave de función](functions-bindings-http-webhook.md#authorization-keys). |
    | Seleccionar cómo desea que se abra el proyecto | **Abrir en la ventana actual** | Abre el proyecto en la ventana de Visual Studio Code actual. |

1. Tras un breve período de tiempo, se muestra un mensaje para indicar que se ha creado el nuevo proyecto. En el **explorador**, encontrará la subcarpeta creada para la función. 

1. Si aún no está abierto, abra el archivo *\_\_init\_\_.py* que contiene el código de función predeterminado:

    [![Resultado de la creación de un nuevo proyecto de funciones de Python](media/tutorial-vs-code-serverless-python/project-create-results.png)](media/tutorial-vs-code-serverless-python/project-create-results.png)

    > [!NOTE]
    > Si Visual Studio Code le indica que no tiene un intérprete de Python seleccionado cuando abra *\_\_init\_\_.py*, abra la paleta de comandos (F1) y seleccione el comando **Python: seleccionar intérprete** y, a continuación, seleccione el entorno virtual en la carpeta `.env` local (que se creó como parte del proyecto). El entorno debe basarse en Python 3.6x específicamente, como se indicó anteriormente en [Requisitos previos](#prerequisites).
    >
    > ![Selección del entorno virtual creado con el proyecto](media/tutorial-vs-code-serverless-python/select-venv-interpreter.png)

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=02-create-function)

## <a name="examine-the-code-files"></a>Examen de los archivos de código

En la subcarpeta de la función recién creada _HttpExample_ hay tres archivos: *\_\_init\_\_.py* contiene el código de la función, *function.json* describe la función para Azure Functions y *sample.dat* es un archivo de datos de ejemplo. Puede eliminar *sample.dat* si lo desea, ya que solo existe para mostrar que puede agregar otros archivos en la subcarpeta.

Echemos un vistazo en primer lugar a *function.json* y luego al código de *\_\_init\_\_.py*.

### <a name="functionjson"></a>function.json

El archivo function.json proporciona la información de configuración necesaria para el punto de conexión de Azure Functions:

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

La propiedad `scriptFile` identifica el archivo de inicio del código y ese código debe contener una función de Python llamada `main`. Puede dividir el código en varios archivos, siempre y cuando el archivo especificado aquí contenga una función `main`.

El elemento `bindings` contiene dos objetos, uno para describir las solicitudes entrantes y otro para describir la respuesta HTTP. En el caso de las solicitudes entrantes (`"direction": "in"`), la función responde a las solicitudes de tipo HTTP GET o POST y requiere que se proporcione la clave de función. La respuesta (`"direction": "out"`) es una respuesta HTTP que devuelve el valor que se devuelve desde la función de Python `main`.

### <a name="__initpy__"></a>\_\_init.py\_\_

Cuando se crea una nueva función, Azure Functions proporciona el código de Python predeterminado en *\_\_init\_\_.py*:

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

Las partes importantes del código son las siguientes:

- Debe importar `func` desde `azure.functions`; la importación del módulo de registro es opcional, pero se recomienda.
- La función de Python obligatoria `main` recibe un objeto `func.request` llamado `req` y devuelve un valor de tipo `func.HttpResponse`. Puede encontrar más información sobre las funcionalidades de estos objetos en las referencias de [func.HttpRequest](/python/api/azure-functions/azure.functions.httprequest?view=azure-python) y [func.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse?view=azure-python).
- Después, el cuerpo de `main` procesa la solicitud y genera una respuesta. En este caso, el código busca un parámetro `name` en la dirección URL. Si no es así, comprueba si el cuerpo de la solicitud contiene un elemento JSON (mediante `func.HttpRequest.get_json`) y si el elemento JSON contiene un valor `name` (mediante el método `get` del objeto JSON devuelto por `get_json`).
- Si se encuentra un nombre, el código devuelve la cadena "Hello" con el nombre anexado; en caso contrario, devuelve un mensaje de error.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=03-examine-code-files)

## <a name="debug-locally"></a>Depuración local

1. Al crear el proyecto de Functions, la extensión de Visual Studio Code también crea una configuración de inicio en `.vscode/launch.json` que contiene una configuración única llamada **Conectar a funciones de Python**. Esta configuración significa que solo tiene que presionar **F5** o usar el explorador de depuración para iniciar el proyecto:

    ![Explorador de depuración que muestra la configuración de inicio de Functions](media/tutorial-vs-code-serverless-python/launch-configuration.png)

1. Al iniciar el depurador, se abre un terminal que muestra la salida de Azure Functions, incluido un resumen de los puntos de conexión disponibles. La dirección URL puede ser diferente si ha usado un nombre distinto de "HttpExample":

    ```output
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Use **Ctrl+clic** o **Cmd+clic** en la dirección URL de la ventana **Salida** de Visual Studio Code para abrir un explorador en esa dirección o bien inicie un explorador y pegue la misma dirección URL. En cualquier caso, el punto de conexión es `api/<function_name>`, en este caso será `api/HttpExample`. Sin embargo, dado que esa dirección URL no incluye un parámetro name, la ventana del explorador solo debe mostrar "Pase un nombre en la cadena de consulta o en el cuerpo de la solicitud" según corresponda para esa ruta en el código.

1. Ahora intente agregar un parámetro name, como `http://localhost:7071/api/HttpExample?name=VS%20Code` y la ventana del explorador debería mostrar el mensaje "Hello Visual Studio Code!", en el que se muestra que se ha ejecutado esa ruta de código.

1. Para pasar el valor de name en un cuerpo de solicitud JSON, puede usar una herramienta como curl con el elemento JSON insertado:

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data {"name":"Visual Studio Code"} http://localhost:7071/api/HttpExample
    ```

    ```ps
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    Como alternativa, puede crear un archivo como *data.json* que contenga `{"name":"Visual Studio Code"}` y usar el comando `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample`.

1. Para probar la función, establezca un punto de interrupción en la línea que lea `name = req.params.get('name')` y vuelva a realizar una solicitud a la dirección URL. El depurador de Visual Studio Code debe detenerse en esa línea, lo que le permite examinar las variables y recorrer el código. (Para un breve tutorial de depuración básica, consulte [Tutorial de Visual Studio Code: configurar y ejecutar el depurador](https://code.visualstudio.com/docs/python/python-tutorial.md#configure-and-run-the-debugger)).

1. Cuando esté seguro de que ha probado exhaustivamente la función de forma local, detenga el depurador (con el comando de menú **Depurar** > **Detener depuración** o el comando **Desconectar** de la barra de herramientas de depuración).

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=04-test-debug)

## <a name="deploy-to-azure-functions"></a>Implementación en Azure Functions

En estos pasos, se usa la extensión de Functions para crear una aplicación de función en Azure, junto con otros recursos de Azure necesarios. Una Function App permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos. También se requiere una cuenta de Azure Storage para los datos y un [plan de hospedaje](functions-scale.md#hosting-plan-support). Todos estos recursos están organizados dentro de un único grupo de recursos.

1. En el área **Azure: Functions**, seleccione el comando **Implementar en aplicación de funciones** o abra la paleta de comandos (F1) y seleccione el comando **Azure Functions: Implementar en aplicación de funciones**. De nuevo, la aplicación de funciones es donde se ejecuta el proyecto de Python en Azure.

    ![Comando Implementar en aplicación de funciones](media/tutorial-vs-code-serverless-python/deploy-command.png)

1. Cuando se le solicite, seleccione **Crear nueva aplicación de funciones en Azure** y proporcione un nombre que sea único en Azure (normalmente con el nombre personal o de la compañía junto con otros identificadores únicos; puede usar letras, números y guiones). Si creó anteriormente una aplicación de funciones, su nombre aparecerá en esta lista de opciones.

1. La extensión realiza las siguientes acciones, que puede observar en los mensajes emergentes de Visual Studio Code y la ventana **Salida** (el proceso tarda unos minutos):

    - Cree un grupo de recursos con el nombre que asignó (quitando los guiones).
    - En ese grupo de recursos, cree la cuenta de almacenamiento, el plan de hospedaje y la aplicación de funciones. De forma predeterminada, se crea un [Plan de consumo](functions-scale.md#consumption-plan). Para ejecutar las funciones en un plan dedicado, debe [habilitar la publicación con opciones avanzadas de creación](functions-develop-vs-code.md).
    - Implemente el código en la aplicación de funciones.

    El explorador de **Azure: Functions** también muestra el progreso:

    ![Indicador de progreso de la implementación en el explorador de Azure: Functions](media/tutorial-vs-code-serverless-python/deploy-progress.png)

1. Una vez completada la implementación, la extensión de Azure Functions muestra un mensaje con botones para tres acciones adicionales:

    ![Mensaje que indica una implementación correcta con acciones adicionales](media/tutorial-vs-code-serverless-python/deployment-popup.png)

    Para **Transmitir registros** y **Cargar configuración**, consulte las secciones siguientes. Para **Ver la salida**, consulte el paso 5 siguiente.

1. Después de la implementación, la ventana **Salida** también muestra el punto de conexión público en Azure:

    ```output
    HTTP Trigger Urls:
      HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    ```

    Use este punto de conexión para ejecutar las mismas pruebas que realizó localmente, con los parámetros de la dirección URL y/o solicitudes con datos JSON en el cuerpo de la solicitud. Los resultados del punto de conexión público deben coincidir con los resultados del punto de conexión local que probó anteriormente.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=05-deploy)

### <a name="stream-logs"></a>Transmisión de registros

La compatibilidad con la transmisión de registros está actualmente en desarrollo, como se describe en la documentación del [Problema 589](https://github.com/microsoft/vscode-azurefunctions/issues/589) de la extensión de Azure Functions. El botón **Transmitir registros** del mensaje emergente de la implementación conectará finalmente la salida del registro en Azure a Visual Studio Code. También podrá iniciar y detener la transmisión de registros en el explorador de **Azure Functions**; para ello, haga clic con el botón derecho en el proyecto de Functions y seleccione **Iniciar transmisión de registros**o**Detener transmisión de registros**.

En la actualidad, sin embargo, estos comandos todavía no están operativos. En su lugar, la transmisión de registros está disponible desde un explorador; para ello, ejecute el comando siguiente, reemplazando `<app_name>` por el nombre de la aplicación de funciones en Azure:

```bash
# Replace <app_name> with the name of your Functions app on Azure
func azure functionapp logstream <app_name> --browser
```

### <a name="sync-local-settings-to-azure"></a>Sincronización de la configuración local con Azure

El botón **Cargar configuración** del mensaje emergente de la implementación aplica los cambios realizados en el archivo *local.settings.json* en Azure. También puede invocar el comando en el explorador de **Azure Functions**; para ello, expanda el nodo del proyecto de Functions, haga clic con el botón derecho en **Configuración de la aplicación**y seleccione **Cargar configuración local...** . También puede usar la paleta de comandos para seleccionar el comando **Azure Functions: Cargar configuración local**.

Al cargar la configuración, se actualizan los valores existentes y se agregan las nuevas configuraciones definidas en *local.settings.json*. La carga no elimina ninguna configuración de Azure que no aparezca en el archivo local. Para eliminar esas configuraciones, expanda el nodo **Configuración de la aplicación** en el explorador de **Azure Functions**, haga clic con el botón derecho en la configuración y seleccione **Eliminar configuración...** . También puede editar la configuración directamente en Azure Portal.

Para aplicar los cambios que realice mediante el portal o el **explorador de Azure** al archivo *local.settings.json*, haga clic con el botón derecho en el nodo **Configuración de la aplicación** y seleccione el comando **Descargar configuración remota...** . También puede usar la paleta de comandos para seleccionar el comando **Azure Functions: Descargar configuración remota**.

## <a name="add-a-second-function"></a>Adición de una segunda función

Después de la primera implementación, puede realizar cambios en el código, como agregar funciones adicionales y volver a implementar en la misma aplicación de funciones.

1. En el área **Azure: Functions**, seleccione el comando **Crear función** o utilice **Azure Functions: Crear función** desde la paleta de comandos. Especifique los siguientes detalles para la función:

    - Plantilla: Desencadenador HTTP
    - Nombre: "DigitsOfPi"
    - Nivel de autorización: Anónimas

1. En el explorador de archivos de Visual Studio Code hay una subcarpeta con el nombre de la función que, de nuevo, contiene los archivos llamados *\_\_init\_\_.py*, *function.json* y *sample.dat*.

1. Reemplace el contenido de *\_\_init\_\_.py* para que coincida con el código siguiente, que genera una cadena que contiene el valor de PI en los dígitos especificados en la dirección URL (este código solo usa un parámetro de dirección URL).

    ```python
    import logging

    import azure.functions as func

    """ Adapted from the second, shorter solution at http://www.codecodex.com/wiki/Calculate_digits_of_pi#Python
    """

    def pi_digits_Python(digits):
        scale = 10000
        maxarr = int((digits / 4) * 14)
        arrinit = 2000
        carry = 0
        arr = [arrinit] * (maxarr + 1)
        output = ""

        for i in range(maxarr, 1, -14):
            total = 0
            for j in range(i, 0, -1):
                total = (total * j) + (scale * arr[j])
                arr[j] = total % ((j * 2) - 1)
                total = total / ((j * 2) - 1)

            output += "%04d" % (carry + (total / scale))
            carry = total % scale

        return output;

    def main(req: func.HttpRequest) -> func.HttpResponse:
        logging.info('DigitsOfPi HTTP trigger function processed a request.')

        digits_param = req.params.get('digits')

        if digits_param is not None:
            try:
                digits = int(digits_param)
            except ValueError:
                digits = 10   # A default

            if digits > 0:
                digit_string = pi_digits_Python(digits)

                # Insert a decimal point in the return value
                return func.HttpResponse(digit_string[:1] + '.' + digit_string[1:])

        return func.HttpResponse(
             "Please pass the URL parameter ?digits= to specify a positive number of digits.",
             status_code=400
        )
    ```

1. Dado que el código solo admite HTTP GET, modifique *function.json* para que la colección `"methods"` contenga solo `"get"` (es decir, elimine `"post"`). El archivo completo debe ser similar al siguiente:

    ```json
    {
      "scriptFile": "__init__.py",
      "bindings": [
        {
          "authLevel": "anonymous",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req",
          "methods": [
            "get"
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

1. Inicie el depurador; para ello, presione **F5** o seleccione el comando de menú **Depurar** > **Iniciar depuración**. La ventana **Salida** ahora debería mostrar ambos puntos de conexión en el proyecto:

    ```output
    Http Functions:

            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Desde un explorador o con curl, haga una solicitud a `http://localhost:7071/api/DigitsOfPi?digits=125` y observe la salida. (Es posible que observe que el algoritmo del código no es totalmente preciso, pero dejaremos las mejoras). Detenga el depurador cuando haya terminado.

1. Vuelva a implementar el código mediante la opción **Implementar en aplicación de funciones** en el explorador de **Azure: Functions**. Si se le solicita, seleccione la aplicación de funciones creada previamente.

1. Una vez finalizada la implementación (tarda unos minutos), la ventana **Salida** muestra los puntos de conexión públicos con los que puede repetir las pruebas.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=06-second-function)

## <a name="add-a-binding-to-write-messages-to-azure-storage"></a>Adición de un enlace para escribir mensajes en Azure Storage

Un _enlace_ le permite conectar el código de la función a los recursos, como Azure Storage, sin escribir ningún código de acceso a datos. Un enlace se define en el archivo *function.json* y puede representar tanto la entrada como la salida. Una función puede utilizar varios enlaces de entrada y de salida, pero solo un desencadenador. Para más información, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

En esta sección, agregará un enlace de almacenamiento a la función HttpExample que creó anteriormente en este tutorial. La función utiliza este enlace para escribir mensajes en el almacenamiento con cada solicitud. El almacenamiento en cuestión usa la misma cuenta de almacenamiento predeterminada que la aplicación de funciones. Sin embargo, si planea hacer un uso intensivo del almacenamiento, debería considerar la posibilidad de crear una cuenta independiente.

1. Sincronice la configuración remota del proyecto de Azure Functions con el archivo *local.settings.json*; para ello, abra la paleta de comandos y seleccione **Azure Functions: Descargar configuración remota**. Abra *local.settings.json* y compruebe que contiene un valor para `AzureWebJobsStorage`. Ese valor es la cadena de conexión de la cuenta de almacenamiento.

1. En la carpeta `HttpExample`, haga clic con el botón derecho en *function.json* y seleccione **Agregar enlace**:

    ![Comando Agregar enlace en el explorador de Visual Studio Code](media/tutorial-vs-code-serverless-python/add-binding-command.png)

1. En los mensajes que siguen en Visual Studio Code, seleccione o proporcione los siguientes valores:

    | Prompt | Valor que se va a proporcionar |
    | --- | --- |
    | Establecer dirección de enlace | out |
    | Seleccionar enlace con dirección de salida | Azure Queue Storage |
    | Nombre utilizado para identificar este enlace en el código | msg |
    | Cola a la que se enviará el mensaje | outqueue |
    | Seleccione la configuración de *local.settings.json* (para la conexión de almacenamiento) | AzureWebJobsStorage |

1. Después de hacer estas selecciones, compruebe que se agrega el siguiente enlace al archivo *function.json*:

    ```json
        {
          "type": "queue",
          "direction": "out",
          "name": "msg",
          "queueName": "outqueue",
          "connection": "AzureWebJobsStorage"
        }
    ```

1. Ahora que ha configurado el enlace, puede usarlo en el código de la función. De nuevo, el enlace recién definido aparece en el código como un argumento de la función `main` de *\_\_init\_\_.py*. Por ejemplo, puede modificar el archivo *\_\_init\_\_.py* de HttpExample para que coincida con lo siguiente, que muestra cómo usar el argumento `msg` para escribir un mensaje con marca de tiempo con el nombre usado en la solicitud. Los comentarios explican los cambios específicos:

    ```python
    import logging
    import datetime  # MODIFICATION: added import
    import azure.functions as func

    # MODIFICATION: the added binding appears as an argument; func.Out[func.QueueMessage]
    # is the appropriate type for an output binding with "type": "queue" (in function.json).
    def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> func.HttpResponse:
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
            # MODIFICATION: write the a message to the message queue, using msg.set
            msg.set(f"Request made for {name} at {datetime.datetime.now()}")

            return func.HttpResponse(f"Hello {name}!")
        else:
            return func.HttpResponse(
                 "Please pass a name on the query string or in the request body",
                 status_code=400
            )
    ```

1. Para probar estos cambios localmente, vuelva a iniciar el depurador en Visual Studio Code; para ello, presione **F5** o seleccione el comando de menú **Depurar** > **Iniciar depuración**. Como antes, la ventana **Salida** ahora debería mostrar los puntos de conexión del proyecto.

1. En un explorador, visite la dirección URL `http://localhost:7071/api/HttpExample?name=VS%20Code` para crear una solicitud al punto de conexión de HttpExample, que también debe escribir un mensaje en la cola.

1. Para comprobar que el mensaje se ha escrito en la cola "outqueue" (como se llama en el enlace), puede usar uno de los tres métodos siguientes:

    1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al grupo de recursos que contiene el proyecto de Functions. Dentro de ese grupo de recursos, busque y abra a la cuenta de almacenamiento del proyecto y, a continuación, vaya a **Colas**. En esa página, vaya a "outqueue", que debería mostrar todos los mensajes registrados.

    1. Abra y examine la cola con el Explorador de Azure Storage, que se integra con Visual Studio, tal y como se describe en [Conectar Functions a Azure Storage con Visual Studio Code](functions-add-output-binding-storage-queue-vs-code.md); preste especial atención a la sección [Examen de la cola de salida](functions-add-output-binding-storage-queue-vs-code.md#examine-the-output-queue).

    1. Use la CLI de Azure para consultar la cola de almacenamiento, tal como se describe en [Consulta de la cola de almacenamiento](functions-add-output-binding-storage-queue-python.md#query-the-storage-queue).
    
1. Para probar en a nube, vuelva a implementar el código mediante la opción **Implementar en aplicación de funciones** en el explorador de **Azure: Functions**. Si se le solicita, seleccione la aplicación de funciones creada previamente. Una vez finalizada la implementación (tarda unos minutos), la ventana **Salida** muestra de nuevo los puntos de conexión públicos con los que puede repetir las pruebas.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=07-storage-binding)

## <a name="clean-up-resources"></a>Limpieza de recursos

La aplicación de funciones que ha creado incluye recursos que pueden incurrir en costos mínimos (consulte [Precios de Functions](https://azure.microsoft.com/pricing/details/functions/)). Para limpiar los recursos, haga clic con el botón derecho en el explorador de **Azure: Functions** y seleccione **Eliminar aplicación de funciones**. También puede visitar [Azure Portal](https://portal.azure.com), seleccionar **Grupos de recursos** en el panel de navegación izquierdo, seleccionar el grupo de recursos que se creó en el proceso de este tutorial y, a continuación, usar el comando **Eliminar grupo de recursos**.

## <a name="next-steps"></a>Pasos siguientes

Enhorabuena por completar este tutorial de implementación de código de Python en Azure Functions. Ahora está listo para crear muchas más funciones sin servidor.

Como se indicó anteriormente, encontrará más información sobre la extensión de Functions en el repositorio de GitHub [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions). Los problemas detectados y las contribuciones también son bienvenidos.

Consulte la [Introducción a Azure Functions](functions-overview.md) para explorar los distintos desencadenadores que puede usar.

Para más información sobre los servicios de Azure que puede usar desde Python, incluido el almacenamiento de datos, junto con la inteligencia artificial y Machine Learning Service, visite el [Centro para desarrolladores de Python de Azure](/azure/python/?view=azure-python).

También hay otras extensiones de Azure para Visual Studio Code que pueden resultarle útiles. Solo tiene que buscar "Azure" en el explorador de extensiones:

![Extensiones de Azure para Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-extensions.png)

Algunas extensiones populares son:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)
- [Herramientas de la CLI de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
