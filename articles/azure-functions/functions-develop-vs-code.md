---
title: Desarrollo de Azure Functions con Visual Studio Code | Microsoft Docs
description: Aprenda a desarrollar y probar Azure Functions mediante la extensión de Azure Functions para Visual Studio Code.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: glenga
ms.openlocfilehash: 17550e148ea61eb69a20fc6a3215dfb63b65f18e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454243"
---
# <a name="develop-azure-functions-using-visual-studio-code"></a>Desarrollo de Azure Functions con Visual Studio Code

La [Extensión de Azure Functions para Visual Studio Code] permite desarrollar localmente e implementar las funciones en Azure. Si esta es su primera experiencia con Azure Functions, puede obtener más información en [Introducción a Azure Functions](functions-overview.md).

La extensión de Azure Functions proporciona los siguientes beneficios: 

* Editar, compilar y ejecutar funciones en el equipo de desarrollo local. 
* Publicar su proyecto de Azure Functions directamente en Azure. 
* Escribir las funciones en varios lenguajes a la vez que se tienen todos los beneficios de Visual Studio Code. 

Puede utilizar la extensión con los siguientes lenguajes compatibles con el tiempo de ejecución de Azure Functions versión 2.x: 

* [C# compilado](functions-dotnet-class-library.md) 
* [Script de C#](functions-reference-csharp.md)<sup>*</sup>
* [JavaScript](functions-reference-node.md)
* [Java](functions-reference-java.md)
* [PowerShell](functions-reference-powershell.md)
* [Python](functions-reference-python.md)

<sup>*</sup>Requiere el [establecimiento de script de C# como el lenguaje de proyecto predeterminado](#c-script-projects).

En este artículo, solo están disponibles en la actualidad ejemplos para JavaScript (Node.js) y biblioteca de clases de C#.  

En este artículo se dan detalles sobre cómo usar la extensión de Azure Functions para desarrollar funciones y publicarlas en Azure. Antes de leer este artículo, debería realizar la [Creación de la primera función mediante Visual Studio Code](functions-create-first-function-vs-code.md).

> [!IMPORTANT]
> No mezcle el desarrollo local con el desarrollo del portal en la misma aplicación de función. Al publicar desde un proyecto local en una aplicación de la función, el proceso de implementación sobrescribe todas las funciones que ha desarrollado en el portal.

## <a name="prerequisites"></a>Requisitos previos

Antes de instalar y ejecutar la [extensión de Azure Functions][extensión de azure functions para visual studio code], es necesario cumplir los siguientes requisitos:

* Tener instalado [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas compatibles](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* Una suscripción de Azure activa.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Otros recursos que necesita, como una cuenta de Azure Storage, se crean en su suscripción cuando [publica con Visual Studio Code](#publish-to-azure).

> [!IMPORTANT]
> Puede desarrollar localmente funciones y publicar en Azure sin tener que iniciar y ejecutarlas localmente. Existen requisitos adicionales para ejecutar su funcional localmente, incluida la descarga automática de Azure Functions Core Tools. Para más información, consulte [Requisitos adicionales para la ejecución en el entorno local](#additional-requirements-to-run-locally). 

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

## <a name="create-an-azure-functions-project"></a>Creación de un proyecto de Azure Functions

La extensión de Functions le permite crear un proyecto de aplicación de funciones, junto con su primera función. Los pasos a continuación muestran cómo crear una función desencadenada por HTTP en un nuevo proyecto de funciones. [Desencadenador HTTP](functions-bindings-http-webhook.md) es la plantilla de desencadenador de función más sencilla de mostrar.

1. En **Azure: Functions**, elija el icono Crear función.

    ![Creación de una función](./media/functions-develop-vs-code/create-function.png)

1. Seleccione la carpeta para el proyecto de aplicación de funciones y, a continuación, **seleccione un lenguaje para el proyecto de función**. 

1. Seleccione la plantilla de función del **desencadenador HTTP**, también puede elegir **Omitir por ahora** para crear un proyecto sin una función. Siempre puede [agregar una función al proyecto](#add-a-function-to-your-project) en otro momento. 

    ![Elegir la plantilla del desencadenador HTTP](./media/functions-develop-vs-code/create-function-choose-template.png)

1. Escriba `HTTPTrigger` como nombre de la función y presione ENTRAR y, a continuación, seleccione la autorización de **Function**. Este nivel de autorización requiere que le proporcione un valor para la [clave de función](functions-bindings-http-webhook.md#authorization-keys) al llamar al punto de conexión de la función.

    ![Elección de la autenticación de función](./media/functions-develop-vs-code/create-function-auth.png)

    Se crea una función en el lenguaje elegido mediante la plantilla para una función desencadenada por HTTP.

    ![Plantilla de función desencadenada por HTTP en Visual Studio Code](./media/functions-develop-vs-code/new-function-full.png)

La plantilla de proyecto crea un proyecto en el lenguaje elegido e instala las dependencias necesarias. Para cualquier lenguaje, el proyecto nuevo contiene los archivos siguientes:

* **host.json**: permite configurar el host de Functions. Esta configuración se aplica tanto cuando se ejecuta localmente como en Azure. Para más información, consulte la [referencia sobre host.json](functions-host-json.md).

* **local.settings.json**: mantiene la configuración que se usa cuando se ejecutan localmente las funciones. Estos valores solo se usan cuando se ejecuta localmente. Para más información, consulte [Archivo de configuración local](#local-settings-file).

    >[!IMPORTANT]
    >Debido a que el archivo local.settings.json puede contener secretos, debe excluirlo del control de origen del proyecto.

En este momento, puede agregar los enlaces de entrada y de salida a la función mediante la [modificación del archivo de function.json](#javascript-2) o [la incorporación de un parámetro a una función de biblioteca de clases de C#](#c-class-library-2).

También puede [agregar una nueva función al proyecto](#add-a-function-to-your-project).

## <a name="install-binding-extensions"></a>Instalación de extensiones de enlace

Excepto los desencadenadores HTTP y del temporizador, los enlaces se implementan como paquetes de extensión. Tiene que instalar los paquetes de extensión para los desencadenadores y enlaces que los necesiten. La forma de instalar las extensiones de enlace depende del lenguaje del proyecto.

### <a name="javascript"></a>JavaScript

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

### <a name="c-class-library"></a>Biblioteca de clases de C\#

Ejecute el comando [dotnet add package](/dotnet/core/tools/dotnet-add-package) en la ventana del terminal para instalar los paquetes de extensión que necesita en el proyecto. El siguiente ejemplo instala la extensión de Azure Storage, que implementa los enlaces para el almacenamiento de Blob, Cola y Tabla.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

## <a name="add-a-function-to-your-project"></a>Incorporación de una función al proyecto

Puede agregar una nueva función a un proyecto existente mediante una de las plantillas de desencadenador de funciones predefinidas. Para agregar un nuevo desencadenador de función, presione la tecla F1 para abrir la paleta de comandos, a continuación, busque y ejecute el comando **Azure Functions: Create Function...** (Crear función...). Siga las indicaciones para elegir el tipo de desencadenador y definir los atributos necesarios del desencadenador. Si el desencadenador requiere una clave de acceso o cadena de conexión para conectarse a un servicio, prepárela antes de crear el desencadenador de la función. 

Los resultados de esta operación dependen del lenguaje del proyecto:

### <a name="javascript"></a>JavaScript

Se crea una nueva carpeta en el proyecto que contiene un nuevo archivo de function.json y el nuevo archivo de código de JavaScript.

### <a name="c-class-library"></a>Biblioteca de clases de C\#

Un nuevo archivo de biblioteca de clases (. cs) de C# se agrega al proyecto.

## <a name="add-input-and-output-bindings"></a>Incorporación de enlaces de entrada y de salida

Puede expandir la función mediante la incorporación de enlaces de entrada y de salida. La manera de hacer esto depende del lenguaje del proyecto. Para más información acerca de los enlaces, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md). 

Los ejemplos siguientes se conectan a una cola de almacenamiento denominada `outqueue`, en la que está establecida la cadena de conexión para la cuenta de almacenamiento en valor de aplicación `MyStorageConnection` en local.settings.json. 

### <a name="javascript"></a>JavaScript

Visual Studio Code le permite agregar enlaces a su archivo function.json siguiendo un conjunto práctico de mensajes. Para crear un enlace, haga clic con el botón derecho (Ctrl + clic en macOS) en el archivo `function.json` de la carpeta de función y elija **Agregar enlace...** 

![Incorporación de un enlace a una función de JavaScript existente ](media/functions-develop-vs-code/function-add-binding.png)

Los siguientes son mensajes de ejemplo para definir un nuevo enlace de salida de almacenamiento:

| Prompt | Valor | DESCRIPCIÓN |
| -------- | ----- | ----------- |
| **Select binding direction** (Seleccionar dirección de enlace) | `out` | El enlace es un enlace de salida. |
| **Select binding with direction...** (Seleccionar enlace con dirección...) | `Azure Queue Storage` | El enlace es un enlace de cola de Azure Storage. |
| **The name used to identify this binding in your code** (Nombre identificativo del enlace en el código) | `msg` | Nombre que identifica el parámetro de enlace al que se hace referencia en el código. |
| **The queue to which the message will be sent** (Cola donde se enviará el mensaje) | `outqueue` | El nombre de la cola en la que escribe el enlace. Cuando no existe *queueName*, el enlace lo crea durante el primer uso. |
| **Select setting from "local.setting.json"** (Seleccionar configuración de "local.setting.json") | `MyStorageConnection` | El nombre de una configuración de la aplicación que contiene la cadena de conexión de la cuenta de Storage. El valor `AzureWebJobsStorage` contiene la cadena de conexión de la cuenta de almacenamiento que creó con la aplicación de función. |

En este ejemplo, se agrega el siguiente enlace a la matriz `bindings` en el archivo function.json:

```javascript
{
    "type": "queue",
    "direction": "out",
    "name": "msg",
    "queueName": "outqueue",
    "connection": "MyStorageConnection"
}
```

También puede agregar la misma definición de enlace directamente a function.json.

En el código de función, se obtiene acceso al enlace `msg` desde el `context`, como en el ejemplo siguiente:

```javascript
context.bindings.msg = "Name passed to the function: " req.query.name;
```

Para más información, consulte la referencia sobre [enlace de salida de Queue Storage](functions-bindings-storage-queue.md#output---javascript-example).

### <a name="c-class-library"></a>Biblioteca de clases de C\#

Actualice el método de función para agregar el siguiente parámetro a la definición de método `Run`:

```cs
[Queue("outqueue"),StorageAccount("MyStorageConnection")] ICollector<string> msg
```

Este código requiere que agregue la siguiente instrucción `using`:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

El parámetro `msg` es de tipo `ICollector<T>`, que representa una colección de mensajes escritos en un enlace de salida cuando se completa la función. Agregue uno o más mensajes a la colección, que se envían a la cola cuando se completa la función.

Para más información, consulte la referencia sobre [enlace de salida de Queue Storage](functions-bindings-storage-queue.md#output---c-example).

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="publish-to-azure"></a>Publicación en Azure

Visual Studio Code le permite publicar el proyecto de Functions directamente en Azure. En el proceso, puede crear una aplicación de función y los recursos relacionados en su suscripción de Azure. La aplicación de función proporciona un contexto de ejecución para sus funciones. El proyecto se empaqueta e implementa en la nueva aplicación de función en su suscripción de Azure.

Al publicar desde Visual Studio Code, se usan uno de dos métodos de implementación:

* [Implementación desde archivo zip con la ejecución desde un archivo de paquete habilitada](functions-deployment-technologies.md#zip-deploy): utilizado para la mayoría de las implementaciones de Azure Functions.
* [Dirección URL de paquete externo](functions-deployment-technologies.md#external-package-url): utilizado para la implementación de aplicaciones de Linux en un [Plan de consumo](functions-scale.md#consumption-plan).

### <a name="quick-function-app-creation"></a>Creación rápida de aplicación de funciones

De forma predeterminada, Visual Studio Code genera automáticamente valores para los recursos de Azure necesarios para la aplicación de funciones. Estos valores se basan en el nombre de la aplicación de funciones que elija. Para obtener un ejemplo del uso de los valores predeterminados para publicar el proyecto en una nueva aplicación de funciones en Azure, consulte el [artículo de inicio rápido de Visual Studio Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure).

Si desea proporcionar nombres explícitos para los recursos creados, tiene que habilitar la publicación con las opciones avanzadas.

### <a name="enabled-publishing-with-advanced-create-options"></a>Habilitación de publicación con opciones de creación avanzadas

Para proporcionarle control sobre la configuración asociada con la creación de aplicaciones de Azure Functions, actualice la extensión de Azure Functions para habilitar la configuración avanzada.

1. Haga clic en **Archivo > Preferencias > Configuración**

1. Navegue por **Configuración de usuario > Extensiones > Azure Functions**

1. Active la casilla para **Función de Azure: Creación avanzada**

### <a name="publish-to-a-new-function-app-in-azure-with-advanced-creation"></a>Publicar en una aplicación de funciones en Azure con creación avanzada

Los pasos siguientes publican el proyecto en una nueva aplicación de funciones que se creó con opciones de creación avanzadas.

1. En el área **Azure: Functions**, seleccione el icono de implementación en Function App.

    ![Configuración de la aplicación de funciones](./media/functions-develop-vs-code/function-app-publish-project.png)

1. Se le pedirá que **inicie sesión en Azure** si todavía no lo ha hecho. También puede **crear una cuenta de Azure gratis**. Una vez que inicie sesión correctamente en el explorador, vuelva a Visual Studio Code.

1. Si tiene varias suscripciones, **seleccione una suscripción** para la aplicación de función y, luego, elija **+ Create New Function App in Azure** (+ Crear nueva aplicación de función en Azure).

1. Siguiendo las indicaciones, proporcione la información siguiente:

    | Prompt | Valor | DESCRIPCIÓN |
    | ------ | ----- | ----------- |
    | Selección de una aplicación de funciones en Azure | + Creación de una aplicación de funciones en Azure | En el siguiente aviso, escriba un nombre único global que identifique la nueva aplicación de funciones y presione ENTRAR. Los siguientes son caracteres válidos para un nombre de aplicación de función: `a-z`, `0-9` y `-`. |
    | Seleccione un sistema operativo | Windows | La aplicación de funciones se ejecuta en Windows |
    | Seleccione un plan de hospedaje | Plan de consumo | Se utiliza un [hospedaje del plan de consumo](functions-scale.md#consumption-plan) sin servidor. |
    | Seleccione un tiempo de ejecución para la nueva aplicación | El lenguaje del proyecto | El tiempo de ejecución tiene que coincidir con el proyecto que va a publicar. |
    | Seleccionar un grupo de recursos para los nuevos recursos | Crear un nuevo grupo de recursos | En el siguiente aviso, escriba un nombre de grupo de recursos, como `myResourceGroup`, y presione ENTRAR. También puede usar un grupo de recursos existente. |
    | Seleccione una cuenta de almacenamiento | Creación de una nueva cuenta de almacenamiento | En el siguiente aviso, escriba un nombre único global para la nueva cuenta de almacenamiento que va a usar la aplicación de funciones y presione ENTRAR. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede elegir una cuenta existente. |
    | Seleccione una ubicación para los nuevos recursos | region | Elija una ubicación en una [región](https://azure.microsoft.com/regions/) cercana a usted o a otros servicios a los que accedan las funciones. |

    Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación. Seleccione **View Output** (Ver salida) en esta notificación para ver la creación y los resultados de la implementación, incluidos los recursos de Azure que ha creado.

## <a name="republish-project-files"></a>Volver a publicar los archivos de proyecto

Al configurar [implementación continua](functions-continuous-deployment.md), la aplicación de función en Azure se actualiza cada vez que se actualizan los archivos de origen en la ubicación de origen conectada. Aunque se recomienda esta práctica de desarrollo, también puede volver a publicar las actualizaciones del archivo de proyecto desde Visual Studio Code. 

> [!IMPORTANT]
> La publicación en una aplicación de función existente sobrescribe el contenido de esa aplicación en Azure.

1. En Visual Studio Code, presione F1 para abrir la paleta de comandos. En la paleta de comandos, busque y seleccione `Azure Functions: Deploy to function app...`.

1. Se le pedirá que **inicie sesión en Azure** si todavía no lo ha hecho. Una vez que inicie sesión correctamente en el explorador, vuelva a Visual Studio Code. Si tiene varias suscripciones, **seleccione una suscripción** que contenga la aplicación de funciones.

1. Elija la aplicación de funciones existente en Azure. Cuando se le presente una advertencia sobre la sobrescritura de todos los archivos de la aplicación de funciones, elija **Implementar** para confirmar la advertencia y continuar. 

El proyecto se vuelve a compilar y empaquetar, y se carga en Azure. El proyecto existente se reemplaza por el nuevo paquete y se reinicia la aplicación de funciones.

## <a name="get-deployed-function-url"></a>Obtención de la dirección URL de la función implementada

Para poder llamar a una función desencadenada por HTTP, necesitará la dirección URL de la función cuando se implementa en la aplicación de funciones. Esta dirección URL incluye todas las [teclas de función](functions-bindings-http-webhook.md#authorization-keys) necesarias. Puede usar la extensión para obtener estas direcciones URL para las funciones implementadas.

1. Presione la tecla F1 para abrir la paleta de comandos, y busque y ejecute el comando **Azure Functions: Copy Function URL** (Copiar la dirección URL de la función).

1. Siga las indicaciones para elegir la aplicación de funciones en Azure y, a continuación, el desencadenador HTTP específico que desea invocar. 

La dirección URL de la función se copia en el Portapapeles, junto con las claves necesarias pasadas mediante el parámetro de consulta `code`. Use una herramienta de HTTP para enviar las solicitudes POST, o un explorador para las solicitudes GET a la función remota.  

## <a name="run-functions-locally"></a>Ejecución local de funciones

La extensión de Azure Functions le permite ejecutar un proyecto de funciones en el equipo de desarrollo local. El tiempo de ejecución local es el mismo tiempo de ejecución que hospeda la aplicación de funciones en Azure. La configuración local se lee desde el [archivo local.settings.json](#local-settings-file).

### <a name="additional-requirements-to-run-locally"></a>Requisitos adicionales para la ejecución en el entorno local

Para poder ejecutar el proyecto de Functions localmente, tiene que cumplir estos requisitos adicionales:

* Instale la versión 2.x de [Azure Functions Core Tools](functions-run-local.md#v2). El paquete de Core Tools se descarga e instala automáticamente al iniciar el proyecto de forma local. Core Tools incluye el tiempo de ejecución completo de Azure Functions, por lo que descarga e instalación pueden tardar algún tiempo.

* Instale los requisitos específicos para el lenguaje elegido:

    | Idioma | Requisito |
    | -------- | --------- |
    | **C#** | [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)<br/>[Herramientas de la CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)   |
    | **Java** | [Depurador para la extensión de Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)<br/>[Java 8](https://aka.ms/azure-jdks)<br/>[Maven 3+](https://maven.apache.org/) |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> |  
    | **Python** | [Extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)<br/>[Python 3.6 o posterior](https://www.python.org/downloads/)|

    <sup>*</sup>Versiones Active LTS y Maintenance LTS (se recomiendan 8.11.1 y 10.14.1).

### <a name="configure-the-project-to-run-locally"></a>Configuración del proyecto para que se ejecute localmente

El tiempo de ejecución de Functions usa una cuenta de Azure Storage internamente para todos los tipos de desencadenadores distintos de HTTP y webhooks. Esto quiere decir que tiene que establecer la clave **Values.AzureWebJobsStorage** en una cadena de conexión de cuenta de Azure Storage válida.

Esta sección usa la [extensión de Azure Storage para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) con el [Explorador de Microsoft Azure Storage](https://storageexplorer.com/) para conectarse y recuperar la cadena de conexión de almacenamiento.   

Para establecer la cadena de conexión de cuenta de almacenamiento:

1. En Visual Studio, abra **Cloud Explorer**, expanda **Cuenta de almacenamiento** > **Su cuenta de almacenamiento** y seleccione **Propiedades** y copie el valor **Cadena de conexión principal**.

2. En el proyecto, abra el archivo local.settings.json y establezca el valor de la clave **AzureWebJobsStorage** en la cadena de conexión que copió.

3. Repita el paso anterior para agregar claves únicas a la matriz de **valores** para cualquier otra conexión que requieran sus funciones.

Para más información, consulte [Archivo de configuración local](#local-settings-file).

### <a name="debugging-functions-locally"></a>Depuración local de funciones  

Para depurar las funciones, presione F5. Si aún no ha descargado [Core Tools][Azure Functions Core Tools], se le pedirá que lo haga. Una vez que Core Tools esté instalado y en ejecución, la salida se muestra en el Terminal. Esto equivale a ejecutar el comando `func host start` de Core Tools desde el Terminal, pero con tareas de compilación adicionales y un depurador asociado.  

Con el proyecto en ejecución, puede desencadenar las funciones como lo haría al implementar en Azure. Cuando se ejecuta en modo de depuración, los puntos de interrupción se alcanzan en Visual Studio Code tal como se esperaba.

La dirección URL de solicitud para los desencadenadores HTTP se muestra en la salida en el terminal. Las teclas de función para los desencadenadores HTTP no se utilizan cuando se ejecuta localmente. Para más información, consulte [Estrategias para probar el código en Azure Functions](functions-test-a-function.md).  

Para más información, consulte [Uso de Azure Functions Core Tools][Azure Functions Core Tools].

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

De manera predeterminada, estas opciones de configuración no se migran automáticamente cuando el proyecto se publica en Azure. Una vez completada la publicación, tiene la opción de publicar la configuración en local.settings.json en Azure. Para más información, consulte [Publicación de la configuración de la aplicación](#publish-application-settings).

Los valores de **ConnectionStrings** nunca se publican.

Los valores de la configuración de la aplicación de funciones también se pueden leer en el código como variables de entorno. Para más información, consulte la sección Variables de entorno de estos artículos de referencia específicos de cada lenguaje:

* [C# precompilado](functions-dotnet-class-library.md#environment-variables)
* [Script de C# (.csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [JavaScript](functions-reference-node.md#environment-variables)

## <a name="application-settings-in-azure"></a>Configuración de la aplicación en Azure

La configuración en el archivo local.settings.json en el proyecto debe ser la misma que la configuración de la aplicación en la aplicación de funciones en Azure. Cualquier configuración que agregue en local.settings.json tiene que agregarse a la aplicación de funciones en Azure. Esta configuración no se carga automáticamente cuando publica el proyecto. Del mismo modo, cualquier configuración que se crea en la aplicación de funciones [en el portal](functions-how-to-use-azure-function-app-settings.md#settings) tiene que descargarse para el proyecto local.

### <a name="publish-application-settings"></a>Publicación de la configuración de la aplicación

La manera más sencilla de publicar la configuración requerida en la aplicación de funciones en Azure es usar el vínculo **Configuración de carga** que aparece una vez que publica correctamente el proyecto.

![Implementación completa configuración de carga de la aplicación](./media/functions-develop-vs-code/upload-app-settings.png)

También puede publicar la configuración con el comando `Azure Functions: Upload Local Setting` en la paleta de comandos. Los valores individuales se agregan a la configuración de la aplicación en Azure mediante el uso del comando `Azure Functions: Add New Setting...`. 

> [!TIP]
> Asegúrese de guardar el archivo local.settings.json antes de publicarlo.

Si el archivo local está cifrado, se descifra, publica y cifra de nuevo. Si existen configuraciones con diferentes valores en ambas ubicaciones, se le pedirá que elija cómo continuar.

Visualice la configuración de la aplicación en el área **Azure: Funciones** expandiendo su suscripción, la aplicación de funciones, y **Configuración de aplicaciones**.

![Visualización de la configuración de aplicación de funciones en Visual Studio Code](./media/functions-develop-vs-code/view-app-settings.png)

### <a name="download-settings-from-azure"></a>Descarga de configuración desde Azure

Si ha creado la configuración de la aplicación en Azure, puede descargarla en el archivo local.settings.json. mediante el uso del comando `Azure Functions: Download Remote Settings...`. 

Como en el caso de las cargas, si el archivo local está cifrado, se descifra, actualiza y cifra de nuevo. Si existen configuraciones con diferentes valores en ambas ubicaciones, se le pedirá que elija cómo continuar.

## <a name="monitoring-functions"></a>Supervisión de funciones

Cuando se realice la [ejecución localmente](#run-functions-locally), los datos de registro se transmiten a la consola del Terminal. También puede obtener datos de registro cuando se ejecuta el proyecto de funciones en una aplicación de funciones en Azure. Puede conectarse a los registros de streaming en Azure para ver los datos de registro prácticamente en tiempo real, o puede habilitar Application Insights para un reconocimiento más completo de cómo se comporta la aplicación de funciones.

### <a name="streaming-logs"></a>Registros de streaming

Al implementar una aplicación, suele resultar útil ver la información de registro casi en tiempo real. Puede ver una secuencia de archivos de registro que generan las funciones. La salida siguiente es un ejemplo de registros de streaming para una solicitud a una función desencadenada HTTP:

![Salida de registros de streaming para el desencadenador HTTP](media/functions-develop-vs-code/streaming-logs-vscode-console.png) 

Para más información, consulte [Registros de streaming](functions-monitoring.md#streaming-logs). 

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

> [!NOTE]
> Los registros de secuencias admiten una sola instancia del host de Functions. Cuando la función se escala a varias instancias, no se muestran los datos de otras instancias en la secuencia de registro. [Live Metrics Stream](../azure-monitor/app/live-stream.md) en Application Insights admite varias instancias. Si bien también funciona casi en tiempo real, el análisis de secuencias también se basa en [datos muestreados](functions-monitoring.md#configure-sampling).

### <a name="application-insights"></a>Application Insights

La forma recomendada de supervisar la ejecución de las funciones es mediante la aplicación de función con Azure Application Insights. De forma predeterminada, cuando crea una aplicación de función en Azure Portal, esta integración se realiza automáticamente. Sin embargo, cuando se crea la aplicación de función durante la publicación de Visual Studio, no se lleva a cabo su integración en Azure.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Para más información, consulte [Supervisión de Azure Functions](functions-monitoring.md).

## <a name="c-script-projects"></a>Proyectos de script C\#

De forma predeterminada, todos los proyectos C# se crean como [proyectos de biblioteca de clases de C# compilados](functions-dotnet-class-library.md). Si prefiere en su lugar trabajar con proyectos de script de C#, tiene que seleccionar script de C# como el lenguaje predeterminado en la configuración de la extensión de Azure Functions.

1. Haga clic en **Archivo > Preferencias > Configuración**.

1. Navegue por **Configuración de usuario > Extensiones > Azure Functions**.

1. Elija **Script de C#** desde **Función de Azure: Lenguaje del proyecto**.

En este momento, las llamadas realizadas a la instancia subyacente de Core Tools incluyen la opción `--csx`, que genera y publica archivos de proyecto de script de C# (.csx). Con un lenguaje predeterminado especificado, todos los proyectos creados son de forma predeterminada proyectos de script de C#. Cuando hay establecido un lenguaje predeterminado, no se le pide que elija un lenguaje de proyecto. Para crear proyectos con otro lenguaje, tiene que cambiar esta configuración o quitarla del archivo de usuario settings.json. Después de quitar esta configuración, al crear un proyecto se le vuelve a pedir que elija su lenguaje.

## <a name="command-palette-reference"></a>Referencia de la paleta de comandos

La extensión de Azure Functions proporciona una interfaz gráfica útil en el área de Azure para interactuar con las aplicación de funciones en Azure. La misma funcionalidad también está disponible como comandos en la paleta de comandos (F1). Los siguientes comandos específicos de Azure Functions están disponibles:

|Comando de Azure Functions  | DESCRIPCIÓN  |
|---------|---------|
|**Add new settings...** (Agregar nueva configuración)  |  Crea una nueva configuración de aplicación en Azure. Para más información, consulte [Publicación de la configuración de la aplicación](#publish-application-settings). También es posible que tenga que [descargar esta configuración en la configuración local](#download-settings-from-azure). |
| **Configure Deployment Source...** (Configurar origen de implementación...) | Conecte la aplicación de funciones en Azure a un repositorio Git local. Para más información consulte [Implementación continua para Azure Functions](functions-continuous-deployment.md). |
| **Connect to GitHub Repository...** (Conectar al repositorio de GitHub) | Conecte la aplicación de funciones a un repositorio de GitHub. |
| **Copy Function URL** (Copiar la dirección URL de la función) | Obtiene la dirección URL remota de una función desencadenada por HTTP que se ejecuta en Azure. Para más información, consulte cómo [obtener la dirección URL de la función implementada](#get-deployed-function-url). |
| **Create function app in Azure...** (Creación de una aplicación de funciones en Azure...) | Crea una nueva aplicación de funciones en su suscripción de Azure. Para más información, consulte cómo [publicar una nueva aplicación de funciones en Azure](#publish-to-azure).        |
| **Decrypt Settings** (Descifrar la configuración) | Se utiliza para descifrar la [configuración local](#local-settings-file) que ha sido cifrada mediante el uso de `Azure Functions: Encrypt Settings`.  |
| **Delete Function App...** (Eliminar aplicación de funciones...) | Quita una aplicación de funciones existente de su suscripción en Azure. Cuando no hay ninguna otra aplicación en el plan de App Service, se le da la opción de eliminar este también. Otros recursos, como las cuentas de almacenamiento y grupos de recursos, no se eliminan. Para quitar todos los recursos, en su lugar debe [eliminar el grupo de recursos](functions-add-output-binding-storage-queue-vs-code.md#clean-up-resources). El proyecto local no se verá afectado. |
|**Delete Function...** (Eliminar función...)  | Quita una función existente de una aplicación de funciones en Azure. Dado que esta eliminación no afecta al proyecto local, en su lugar, considere la posibilidad de quitar la función localmente y después [volver a publicar el proyecto](#republish-project-files). |
| **Delete Proxy...** (Eliminar a Proxy...) | Quita a un servidor proxy de Azure Functions desde la aplicación de funciones en Azure. Para más información sobre servidores proxy, consulte [Uso de Azure Functions Proxies](functions-proxies.md). |
| **Delete Setting...** (Eliminar opción...) | Elimina una configuración de aplicación de funciones en Azure. No afecta a la configuración en el archivo local.settings.json. |
| **Disconnect from Repo...** (Desconectar del repositorio...)  | Quita la conexión de [implementación continua](functions-continuous-deployment.md) entre una aplicación de funciones en Azure y un repositorio de control de código fuente. |
| **Download Remote Settings...** (Descargar configuración remota...) | Descarga la configuración de la aplicación de funciones elegida en Azure en el archivo local.settings.json. Si el archivo local está cifrado, se descifra, actualiza y cifra de nuevo. Si existen configuraciones con diferentes valores en ambas ubicaciones, se le pedirá que elija cómo continuar. Asegúrese de que ha guardado los cambios en el archivo local.settings.json antes de ejecutar este comando. |
| **Edit Settings...** (Editar configuración...) | Cambia el valor de una configuración de aplicación de funciones existente en Azure. No afecta a la configuración en el archivo local.settings.json.  |
| **Encrypt settings** (Cifrar configuración) | Cifra los elementos individuales de la matriz de `Values` en la [configuración local](#local-settings-file). En este archivo, `IsEncrypted` también se establece en `true`, lo que indica al tiempo de ejecución local que descifre la configuración antes de usarla. Cifre la configuración local para reducir el riesgo de pérdida de información valiosa. En Azure, las configuraciones de aplicación siempre se almacenan cifradas. |
| **Execute Function Now** (Ejecutar la función ahora) | Inicia manualmente una [función desencadenada por temporizador](functions-bindings-timer.md) en Azure con fines de prueba. Para más información acerca de cómo desencadenar funciones ajenas a HTTP en Azure, consulte [Ejecución manual de una función no desencadenada por HTTP](functions-manually-run-non-http.md). |
| **Initialize Project for Use with VS Code...** (Inicializar el proyecto para su uso con VS Code...) | Agrega los archivos de proyecto de Visual Studio Code necesarios a un proyecto existente de Functions. Use este comando para trabajar con un proyecto creado mediante Core Tools. |
| **Install of Update Azure Functions Core Tools** (Instalación de actualización de Azure Functions Core Tools) | Instala o actualiza [Azure Functions Core Tools] que se utilizan para ejecutar localmente. |
| **Redeploy** (Volver a implementar)  | Le permite volver a implementar archivos de proyecto desde un repositorio de Git conectado a una implementación específica de Azure. Para volver a publicar actualizaciones locales de Visual Studio Code, [vuelva a publicar el proyecto](#republish-project-files). |
| **Rename Settings...** (Cambiar el nombre de configuración...) | Cambia el nombre clave de una configuración de aplicación de funciones existente en Azure. No afecta a la configuración en el archivo local.settings.json. Después de cambiar el nombre de configuración en Azure, debe [descargar esos cambios en el proyecto local](#download-settings-from-azure). |
| **Restart** (Reiniciar) | Reinicia la aplicación de funciones en Azure. La implementación de actualizaciones, también reinicia la aplicación de funciones. |
| **Set AzureWebJobStorage...** (Establecer AzureWebJobStorage...)| Establece el valor de la configuración de la aplicación`AzureWebJobStorage`. Esta configuración es necesaria para las funciones de Azure y se establece cuando se crea la aplicación de funciones en Azure. |
| **Iniciar** | Inicia una aplicación de funciones detenida en Azure. | 
| **Start Streaming Logs** (Iniciar registros de streaming) | Inicia la transmisión de registros para la aplicación de funciones en Azure. Use los registros de streaming durante la solución de problemas remota en Azure si tiene que ver esta información en tiempo casi real. Para más información, consulte [Registros de streaming](#streaming-logs). |
| **Detención** | Apaga una aplicación de funciones que se esté ejecutando en Azure. |
| **Stop Streaming Logs** (Detener registros de streaming) | Detiene la transmisión de registros para la aplicación de funciones en Azure. |
| **Toggle as Slot Setting** (Alternar como configuración de ranura) | Cuando está habilitado, se asegura de que se conserve una configuración de aplicación para una ranura de implementación determinada. |
| **Uninstall Azure Functions Core Tools** (Desinstalar Azure Functions Core Tools) | Quita Azure Functions Core Tools, que es necesaria para la extensión. |
| **Upload local settings...** (Cargar configuración local) | Carga la configuración del archivo local.settings.json a la aplicación de funciones elegida en Azure. Si el archivo local está cifrado, se descifra, carga y cifra de nuevo. Si existen configuraciones con diferentes valores en ambas ubicaciones, se le pedirá que elija cómo continuar. Asegúrese de que ha guardado los cambios en el archivo local.settings.json antes de ejecutar este comando. |
| **View Commit in GitHub** (Ver confirmación en GitHub) | Muestra la última confirmación en una implementación específica cuando la aplicación de funciones está conectada a un repositorio. |
| **View Deployment Logs** (Ver registros de implementación) | Muestra los registros para una implementación específica de la aplicación de funciones en Azure. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Functions Core Tools, consulte [Codificación y comprobación de las funciones de Azure en un entorno local](functions-run-local.md).

Para más información sobre el desarrollo de funciones como las bibliotecas de clases de .NET, consulte [Referencia para desarrolladores de C# de Azure Functions](functions-dotnet-class-library.md). Este artículo también tiene vínculos a ejemplos de cómo usar atributos para declarar los distintos tipos de vínculos compatibles con Azure Functions.    

[Extensión de Azure Functions para Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
[Azure Functions Core Tools]: functions-run-local.md