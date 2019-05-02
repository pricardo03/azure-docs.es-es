---
title: Referencia del programador de PowerShell para Azure Functions
description: Descubra cómo desarrollar funciones mediante el uso de PowerShell.
services: functions
documentationcenter: na
author: tylerleonhardt
manager: jeconnoc
ms.service: azure-functions
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha, glenga
ms.openlocfilehash: 71ac525e2af7473ca9ce0a8f60268e76eccd1a9a
ms.sourcegitcommit: 111a7b3e19d5515ce7036287cea00a7204ca8b56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64530389"
---
# <a name="azure-functions-powershell-developer-guide"></a>Guía del desarrollador de Azure PowerShell de funciones

En este artículo proporciona detalles sobre cómo escribir funciones de Azure con PowerShell.

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

Una función de PowerShell se representa como un script de PowerShell que se ejecuta cuando se desencadena. Cada secuencia de comandos de función tiene un relacionados function.json que define cómo se comporta la función, por ejemplo, cómo se activa y los parámetros de entrada y salidos. Para obtener más información, consulte el [desencadenadores y artículo enlace](functions-triggers-bindings.md). 

Al igual que otros tipos de funciones, el script de PowerShell toma parámetros que coinciden con los nombres de todos los enlaces de entrada definidos en function.json. Un `TriggerMetadata` también se pasa parámetro que contiene información adicional sobre el desencadenador que inicia la función.

En este artículo se supone que ya ha leído [Referencia para desarrolladores de Azure Functions](functions-reference.md). Debe haber completado también el [inicio rápido de Functions para PowerShell](functions-create-first-function-powershell.md) para crear su primera función de PowerShell.

## <a name="folder-structure"></a>Estructura de carpetas

La estructura de carpetas necesarios para un proyecto de PowerShell tiene el siguiente aspecto. Este valor predeterminado se puede cambiar. Para más información, consulte la sección [scriptFile](#configure-function-scriptfile) a continuación.

```
PSFunctionApp
 | - MyFirstFunction
 | | - run.ps1
 | | - function.json
 | - MySecondFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - myFirstHelperModule
 | | | - myFirstHelperModule.psd1
 | | | - myFirstHelperModule.psm1
 | | - mySecondHelperModule
 | | | - mySecondHelperModule.psd1
 | | | - mySecondHelperModule.psm1
 | - local.settings.json
 | - host.json
 | - requirements.psd1
 | - profile.ps1
 | - extensions.csproj
 | - bin
```

En la raíz del proyecto, hay un archivo [host.json](functions-host-json.md) compartido que se puede usar para configurar la aplicación de función. Cada función tiene una carpeta con su propio archivo de código (. ps1) y el archivo de configuración de enlace (function.json). El nombre del directorio primario de `function.json` siempre es el nombre de la función.

Determinados enlaces requieren la presencia de un `extensions.csproj`. Enlace las extensiones necesarias en [versión 2.x](functions-versions.md) del tiempo de ejecución de funciones, se definen en el `extensions.csproj` archivo, con los archivos de biblioteca real en el `bin` carpeta. Al desarrollar de forma local, debe [registrar las extensiones de enlace](functions-bindings-register.md#local-development-with-azure-functions-core-tools-and-extension-bundles). Al desarrollar funciones en Azure Portal, este registro se realiza automáticamente.

En las aplicaciones de función de PowerShell, podría tener opcionalmente un `profile.ps1` que se ejecuta cuando se inicia una aplicación de función para ejecutarse (en caso contrario, se conoce como un  *[frío](#cold-start)*. Para obtener más información, consulte [perfil PowerShell](#powershell-profile).

## <a name="defining-a-powershell-script-as-a-function"></a>Definición de un script de PowerShell como una función

De forma predeterminada, el tiempo de ejecución de Functions busca la función en `run.ps1`, donde `run.ps1` comparte el mismo directorio primario que su `function.json` correspondiente.

La secuencia de comandos se pasa un número de argumentos en ejecución. Para controlar estos parámetros, agregue un `param` bloque a la parte superior de la secuencia de comandos como en el ejemplo siguiente:

```powershell
# $TriggerMetadata is optional here. If you don't need it, you can safely remove it from the param block
param($MyFirstInputBinding, $MySecondInputBinding, $TriggerMetadata)
```

### <a name="triggermetadata-parameter"></a>Parámetro TriggerMetadata

El `TriggerMetadata` parámetro se usa para proporcionar información adicional acerca del desencadenador. Los metadatos adicionales varía en función de enlace al enlace, pero todos ellos contienen un `sys` propiedad que contiene los datos siguientes:

```powershell
$TriggerMetadata.sys
```

| Propiedad   | DESCRIPCIÓN                                     | Type     |
|------------|-------------------------------------------------|----------|
| UtcNow     | Cuando, en UTC, se desencadenó la función        | DateTime |
| MethodName | El nombre de la función que se desencadenó     | string   |
| RandGuid   | un guid único para esta ejecución de la función | string   |

Cada tipo de desencadenador tiene un conjunto diferente de metadatos. Por ejemplo, el `$TriggerMetadata` para `QueueTrigger` contiene el `InsertionTime`, `Id`, `DequeueCount`, entre otras cosas. Para obtener más información sobre los metadatos del desencadenador de cola, vaya a la [documentación oficial para los desencadenadores de colas](functions-bindings-storage-queue.md#trigger---message-metadata). Consulte la documentación en el [desencadenadores](functions-triggers-bindings.md) que está trabajando para ver lo que está incluido en los metadatos de desencadenador.

## <a name="bindings"></a>Enlaces

En PowerShell, [enlaces](functions-triggers-bindings.md) se configuran y se define en function.json de la función. Las funciones interactúan con los enlaces de varias maneras.

### <a name="reading-trigger-and-input-data"></a>Lectura de datos del desencadenador y de entrada

Desencadenador y enlaces de entrada se leen como parámetros pasados a la función. Los enlaces de entrada tienen un `direction` establecido en `in` en function.json. El `name` propiedad definida en `function.json` es el nombre del parámetro, en el `param` bloque. Dado que PowerShell utiliza parámetros con nombre para el enlace, no importa el orden de los parámetros. Sin embargo, es una práctica recomendada para seguir el orden de los enlaces definidos en el `function.json`.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)
```

### <a name="writing-output-data"></a>Escribir datos de salida

En las funciones, un enlace de salida tiene un `direction` establecido en `out` en el archivo function.json. Puede escribir en un enlace de salida mediante el uso de la `Push-OutputBinding` cmdlet, que está disponible para el tiempo de ejecución de funciones. En todos los casos, el `name` propiedad del enlace tal como se define en `function.json` corresponde a la `Name` parámetro de la `Push-OutputBinding` cmdlet.

La siguiente muestra cómo llamar a `Push-OutputBinding` en el script de la función:

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Push-OutputBinding -Name myQueue -Value $myValue
```

También puede pasar un valor para un enlace específico a través de la canalización.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Produce-MyOutputValue | Push-OutputBinding -Name myQueue
```

`Push-OutputBinding` se comporta de forma distinta según el valor especificado para `-Name`:

* Cuando no se puede resolver el nombre especificado en un enlace de salida válido, se produce un error.

* Cuando el enlace de salida acepta una colección de valores, puede llamar a `Push-OutputBinding` varias veces para insertar varios valores.

* Cuando el enlace de salida solo acepta un valor singleton, una llamada a `Push-OutputBinding` una segunda vez genera un error.

#### <a name="push-outputbinding-syntax"></a>`Push-OutputBinding` Sintaxis

Los siguientes son parámetros válidos para llamar a `Push-OutputBinding`:

| NOMBRE | Type | Posición | DESCRIPCIÓN |
| ---- | ---- |  -------- | ----------- |
| **`-Name`** | string | 1 | El nombre del enlace de salida que desea establecer. |
| **`-Value`** | Object | 2 | El valor del enlace de salida que desea establecer, que se acepta desde la canalización ByValue. |
| **`-Clobber`** | SwitchParameter | con nombre | (Opcional) Cuando se especifica, se fuerza el valor debe establecerse para un enlace de salida especificado. | 

También se admiten los parámetros comunes siguientes: 
* `Verbose`
* `Debug`
* `ErrorAction`
* `ErrorVariable`
* `WarningAction`
* `WarningVariable`
* `OutBuffer`
* `PipelineVariable`
* `OutVariable` 

Para obtener más información, consulte [acerca de CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

#### <a name="push-outputbinding-example-http-responses"></a>Ejemplo de inserción OutputBinding: Respuestas HTTP

Un desencadenador HTTP devuelve una respuesta con un enlace de salida denominado `response`. En el ejemplo siguiente, el enlace de salida de `response` tiene el valor de "salida #1":

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #1"
})
```

Dado que la salida es HTTP, que acepta sólo un valor singleton, se produce un error cuando `Push-OutputBinding` se llama a una segunda vez.

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #2"
})
```

Los resultados que sólo acepte valores singleton, puede usar el `-Clobber` parámetro para invalidar el valor anterior en lugar de intentar agregar a una colección. En el siguiente ejemplo se da por supuesto que ya ha agregado un valor. Mediante el uso de `-Clobber`, la respuesta de ejemplo siguiente invalida el valor existente para devolver un valor de "salida #3":

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #3"
}) -Clobber
```

#### <a name="push-outputbinding-example-queue-output-binding"></a>Ejemplo de inserción OutputBinding: Enlace de salida de cola

`Push-OutputBinding` se usa para enviar datos a los enlaces de salida, como un [enlace de salida Azure Queue storage](functions-bindings-storage-queue.md#output). En el ejemplo siguiente, el mensaje que se escriben en la cola tiene un valor de "salida #1":

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #1"
```

El enlace de salida para una cola de almacenamiento acepta varios valores de salida. En este caso, una llamada en el ejemplo siguiente después de la primera escribe en la cola de una lista con dos elementos: "output #1" y "salida de #2".

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #2"
```

El ejemplo siguiente, cuando se llama después de los dos anteriores, agrega dos o más valores a la colección de salida:

```powershell
PS >Push-OutputBinding -Name outQueue -Value @("output #3", "output #4")
```

Cuando se escribe en la cola, el mensaje contiene estos cuatro valores: "output #1", "salida de #2", "salida #3" y "salida #4".

#### <a name="get-outputbinding-cmdlet"></a>`Get-OutputBinding` cmdlet

Puede usar el `Get-OutputBinding` para recuperar los valores establecidos actualmente para los enlaces de salida. Este cmdlet recupera una tabla hash que contiene los nombres de los enlaces de salida con los valores respectivos. 

El siguiente es un ejemplo del uso de `Get-OutputBinding` para devolver valores de enlace actual:

```powershell
Get-OutputBinding
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
MyOtherQueue                   myData
```

`Get-OutputBinding` También contiene un parámetro denominado `-Name`, que se puede usar para filtrar el enlace devuelto, como en el ejemplo siguiente:

```powershell
Get-OutputBinding -Name MyQ*
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
```

Se admiten caracteres comodín (*) en `Get-OutputBinding`.

## <a name="logging"></a>Registro

Iniciar sesión en las funciones de PowerShell funciona como registro de PowerShell regular. Puede usar los cmdlets de registro para escribir en cada flujo de salida. Cada cmdlet se asigna a un nivel de registro utilizado por las funciones.

| Funciones de nivel de registro | Cmdlet de registro |
| ------------- | -------------- |
| Error | **`Write-Error`** |
| Advertencia | **`Write-Warning`**  | 
| Información | **`Write-Information`** <br/> **`Write-Host`** <br /> **`Write-Output`**      | Información | Escribe en _información_ registro en el nivel. |
| Depurar | **`Write-Debug`** |
| Seguimiento | **`Write-Progress`** <br /> **`Write-Verbose`** |

Además de estos cmdlets, todo lo escrito en la canalización se redirige a la `Information` registro nivel y se muestran con el formato predeterminado de PowerShell.

> [!IMPORTANT]
> Mediante el `Write-Verbose` o `Write-Debug` cmdlets no es suficiente para ver detallado y el registro de nivel de depuración. También debe configurar el umbral de nivel de registro, que declara qué nivel de los registros de verdad te preocupas. Para obtener más información, consulte [configurar el nivel de registro de aplicación de función](#configure-the-function-app-log-level).

### <a name="configure-the-function-app-log-level"></a>Configurar el nivel de registro de aplicación de función

Functions permite definir el nivel de umbral para facilitar a las funciones de manera que se escribe en los registros de control. Para establecer el umbral para todos los seguimientos que se escriben en la consola, use el `logging.logLevel.default` propiedad en el [ `host.json` archivo][referencia sobre host.json]. Esta configuración se aplica a todas las funciones de Function App.

El ejemplo siguiente se establece el umbral para habilitar el registro detallado para todas las funciones, pero establece el umbral para habilitar el registro de depuración para una función denominada `MyFunction`:

```json
{
    "logging": {
        "logLevel": {
            "Function.MyFunction": "Debug",
            "default": "Trace"
        }
    }
}  
```

Para más información, consulte la [referencia sobre host.json].

### <a name="viewing-the-logs"></a>Visualización de los registros

Si la aplicación de función se ejecuta en Azure, puede usar Application Insights para supervisarlo. Consulte cómo [supervisar Azure Functions](functions-monitoring.md) para obtener más información sobre cómo ver y consultar registros de la función.

Si está ejecutando la aplicación de función localmente para el desarrollo, registra de forma predeterminada en el sistema de archivos. Para ver los registros en la consola, establezca el `AZURE_FUNCTIONS_ENVIRONMENT` variable de entorno `Development` antes de iniciar la aplicación de función.

## <a name="triggers-and-bindings-types"></a>Tipos de enlaces y desencadenadores

Hay un número de enlaces y desencadenadores disponibles para su uso con la aplicación de función. La lista completa de los desencadenadores y enlaces [puede encontrarse aquí](functions-triggers-bindings.md#supported-bindings).

Todos los desencadenadores y enlaces se representan en el código como algunos tipos de datos reales:

* Hashtable
* string
* byte[]
* int
* double
* HttpRequestContext
* HttpResponseContext

Los cinco primeros tipos de esta lista son tipos de .NET estándar. Los dos últimos se utilizan únicamente por la [HttpTrigger desencadenador](#http-triggers-and-bindings).

Cada parámetro de enlace en las funciones debe ser uno de estos tipos.

### <a name="http-triggers-and-bindings"></a>Desencadenadores y enlaces HTTP

Los desencadenadores HTTP y de webhook trigger y los enlaces de salida HTTP usan objetos de solicitud y respuesta para representar la mensajería HTTP.

#### <a name="request-object"></a>Objeto de solicitud

El objeto de solicitud que se pasa a la secuencia de comandos es del tipo `HttpRequestContext`, que tiene las siguientes propiedades:

| Propiedad  | DESCRIPCIÓN                                                    | Type                      |
|-----------|----------------------------------------------------------------|---------------------------|
| **`Body`**    | Objeto que contiene el cuerpo de la solicitud. `Body` se serializa en el mejor tipo basándose en los datos. Por ejemplo, si los datos JSON, se pasa en como una tabla hash. Si los datos están una cadena, se pasa en forma de cadena. | objeto |
| **`Headers`** | Un diccionario que contiene los encabezados de solicitud.                | Diccionario < cadena, cadena ><sup>*</sup> |
| **`Method`** | Método HTTP de la solicitud.                                | string                    |
| **`Params`**  | Objeto que contiene los parámetros de enrutamiento de la solicitud. | Diccionario < cadena, cadena ><sup>*</sup> |
| **`Query`** | Objeto que contiene los parámetros de consulta.                  | Diccionario < cadena, cadena ><sup>*</sup> |
| **`Url`** | Dirección URL de la solicitud.                                        | string                    |

<sup>*</sup> Todos los `Dictionary<string,string>` claves distinguen mayúsculas de minúsculas.

#### <a name="response-object"></a>Objeto de respuesta

El objeto de respuesta que debe enviar atrás es del tipo `HttpResponseContext`, que tiene las siguientes propiedades:

| Propiedad      | DESCRIPCIÓN                                                 | Type                      |
|---------------|-------------------------------------------------------------|---------------------------|
| **`Body`**  | Objeto que contiene el cuerpo de la respuesta.           | objeto                    |
| **`ContentType`** | Una mano corto para establecer el tipo de contenido para la respuesta. | string                    |
| **`Headers`** | Objeto que contiene los encabezados de la respuesta.               | Diccionario o tabla hash   |
| **`StatusCode`**  | Código de estado HTTP de la respuesta.                       | cadena o entero             |

#### <a name="accessing-the-request-and-response"></a>Acceso a solicitudes y respuestas

Cuando se trabaja con desencadenadores HTTP, puede acceder a la solicitud HTTP del mismo modo que lo haría con cualquier otro enlace de entrada. Se encuentra en la `param` bloque.

Use un `HttpResponseContext` objeto para devolver una respuesta, tal como se muestra en la siguiente:

`function.json`

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "anonymous"
    },
    {
      "type": "http",
      "direction": "out"
    }
  ]
}
```

`run.ps1`

```powershell
param($req, $TriggerMetadata)

$name = $req.Query.Name

Push-OutputBinding -Name res -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "Hello $name!"
})
```

El resultado de invocar esta función sería:

```
PS > irm http://localhost:5001?Name=Functions
Hello Functions!
```

### <a name="type-casting-for-triggers-and-bindings"></a>Conversión de tipos de desencadenadores y enlaces

Para determinados enlaces como el enlace de blob, podrá especificar el tipo del parámetro.

Por ejemplo, para que los datos del almacenamiento de blobs proporcionado como una cadena, agregue el siguiente tipo de conversión a mi `param` bloque:

```powershell
param([string] $myBlob)
```

## <a name="powershell-profile"></a>Perfil de PowerShell

En PowerShell, hay el concepto de un perfil de PowerShell. Si no está familiarizado con los perfiles de PowerShell, consulte [acerca de los perfiles](/powershell/module/microsoft.powershell.core/about/about_profiles).

En las funciones de PowerShell, se ejecuta el script de perfil cuando se inicia la aplicación de función. Inician de aplicaciones de función cuando se implementó por primera vez y después de que se está inactivo ([frío](#cold-start)).

Cuando se crea una aplicación de función con herramientas como Visual Studio Code y Azure Functions Core Tools, valor predeterminado es `profile.ps1` se crea automáticamente. El perfil predeterminado se mantiene [en el repositorio de GitHub de Core Tools](https://github.com/Azure/azure-functions-core-tools/blob/dev/src/Azure.Functions.Cli/StaticResources/profile.ps1) y contiene:

* Autenticación automática de MSI en Azure.
* La capacidad de activar Azure PowerShell `AzureRM` PowerShell alias si lo desea.

## <a name="powershell-version"></a>Versión de PowerShell

En la tabla siguiente se muestra la versión de PowerShell usada por cada versión principal del tiempo de ejecución de funciones:

| Versión de Functions | Versión de PowerShell                             |
|-------------------|------------------------------------------------|
| 1.x               | Windows PowerShell 5.1 (bloqueado por el tiempo de ejecución) |
| 2.x               | PowerShell Core 6                              |

Puede ver la versión actual mediante la impresión `$PSVersionTable` desde cualquier función.

## <a name="dependency-management"></a>Administración de dependencias

Las funciones de PowerShell admiten la administración de módulos de Azure por el servicio. Modificando el host.json y establecer la propiedad managedDependency habilitada en true, se procesará el archivo requirements.psd1. Los módulos de Azure más recientes se descargará automáticamente y disponible para la función.

host.json
```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

requirements.psd1

```powershell
@{
    Az = '1.*'
}
```

Aprovechamiento de su propio custom módulos o los módulos de la [Galería de PowerShell](https://powershellgallery.com) es un poco diferente de cómo lo haría normalmente.

Cuando se instala el módulo en el equipo local, se incluye en una de las carpetas disponibles globalmente en su `$env:PSModulePath`. Dado que la función se ejecuta en Azure, no tendrá acceso a los módulos instalados en el equipo. Esto requiere que el `$env:PSModulePath` para una función de PowerShell difiere aplicación `$env:PSModulePath` en un script de PowerShell normal.

En las funciones, `PSModulePath` contiene dos rutas de acceso:

* Un `Modules` carpeta que existe en la raíz de la aplicación de función.
* Una ruta de acceso a un `Modules` carpeta que se encuentra en el trabajo de lenguaje de PowerShell.

### <a name="function-app-level-modules-folder"></a>Nivel de aplicación de función `Modules` carpeta

Para usar los módulos personalizados o los módulos de PowerShell desde la Galería de PowerShell, puede colocar los módulos que dependen las funciones en un `Modules` carpeta. Desde esta carpeta, módulos están automáticamente disponibles para el tiempo de ejecución de funciones. Cualquier función de la aplicación de función puede usar estos módulos.

Para aprovechar las ventajas de esta característica, cree un `Modules` carpeta en la raíz de la aplicación de función. Guarde los módulos que desea usar en las funciones en esta ubicación.

```powershell
mkdir ./Modules
Save-Module MyGalleryModule -Path ./Modules
```

Usar `Save-Module` para guardar todos los módulos que utilizan las funciones o copiar sus propios módulos personalizados para el `Modules` carpeta. Con una carpeta de módulos, la aplicación de función debe tener la estructura de carpetas siguiente:

```
PSFunctionApp
 | - MyFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - MyGalleryModule
 | | - MyOtherGalleryModule
 | | - MyCustomModule.psm1
 | - local.settings.json
 | - host.json
```

Cuando se inicia la aplicación de función, el trabajador de lenguaje de PowerShell agrega este `Modules` carpeta a la `$env:PSModulePath` por lo que puede confiar en el módulo carga automática tal como haría en un script de PowerShell normal.

### <a name="language-worker-level-modules-folder"></a>Nivel de trabajo del lenguaje `Modules` carpeta

Varios módulos se usan normalmente el trabajador de lenguaje de PowerShell. Estos módulos se definen en la última posición de `PSModulePath`. 

La lista actual de los módulos es como sigue:

* [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive): módulo usadas para trabajar con archivos, como `.zip`, `.nupkg`y otros.
* **ThreadJob**: Una implementación basada en subprocesos de la API de trabajo de PowerShell.

La versión más reciente de estos módulos se usa las funciones. Para usar una versión específica de estos módulos, puede colocar la versión específica en el `Modules` carpeta de la aplicación de función.

## <a name="environment-variables"></a>Variables de entorno

En Functions, [la configuración de la aplicación](functions-app-settings.md), como las cadenas de conexión del servicio, se exponen como variables de entorno durante la ejecución. Puede tener acceso a estas opciones mediante `$env:NAME_OF_ENV_VAR`, como se muestra en el ejemplo siguiente:

```powershell
param($myTimer)

Write-Host "PowerShell timer trigger function ran! $(Get-Date)"
Write-Host $env:AzureWebJobsStorage
Write-Host $env:WEBSITE_SITE_NAME
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Cuando se ejecuta localmente, la configuración de la aplicación se lee desde el archivo del proyecto [local.settings.json](functions-run-local.md#local-settings-file).

## <a name="concurrency"></a>Simultaneidad

De forma predeterminada, el tiempo de ejecución de PowerShell de funciones sólo puede procesar una invocación de una función a la vez. Sin embargo, este nivel de simultaneidad podría no ser suficiente en las situaciones siguientes:

* Cuando intenta administrar un gran número de llamadas al mismo tiempo.
* Cuando dispone de funciones que llaman a otras funciones dentro de la misma aplicación de función.

Puede cambiar este comportamiento estableciendo la variable de entorno siguiente en un valor entero:

```
PSWorkerInProcConcurrencyUpperBound
```

Establezca esta variable de entorno el [configuración de la aplicación](functions-app-settings.md) de la aplicación de función.

### <a name="considerations-for-using-concurrency"></a>Consideraciones sobre el uso de simultaneidad

PowerShell es un _un único subproceso_ lenguaje de scripting de forma predeterminada. Sin embargo, la simultaneidad puede agregarse mediante el uso de varios espacios de ejecución de PowerShell en el mismo proceso. Esta característica es cómo funciona el tiempo de ejecución de funciones de Azure PowerShell.

Existen algunos inconvenientes con este enfoque.

#### <a name="concurrency-is-only-as-good-as-the-machine-its-running-on"></a>Simultaneidad es tan buena como el equipo en que se está ejecutando

Si la aplicación de función se ejecuta en un [plan de App Service](functions-scale.md#app-service-plan) que solo admite un único núcleo, a continuación, simultaneidad no ayuda demasiado. Eso es porque no hay ningún núcleos adicionales para ayudar a equilibrar la carga. En este caso, el rendimiento puede variar cuando el núcleo único tiene a cambio de contexto entre los espacios de ejecución.

El [plan de consumo](functions-scale.md#consumption-plan) se ejecuta con un solo núcleo, lo que no puede aprovechar la simultaneidad. Si desea aprovechar al máximo de simultaneidad, en su lugar, implemente las funciones a una aplicación de función que se ejecuta en un plan de App Service dedicado con suficientes núcleos.

#### <a name="azure-powershell-state"></a>Estado de Azure PowerShell

Azure PowerShell usa algunos _nivel de proceso_ contextos y estado para ayudar a ahorrar de exceso de escribir. Sin embargo, si activa la simultaneidad en la aplicación de función e invocar acciones que cambian el estado, puede acabar con las condiciones de carrera. Estas condiciones de carrera son difíciles de depurar porque una invocación se basa en un estado determinado y la otra llamada ha cambiado el estado.

Hay gran valor en la simultaneidad con Azure PowerShell, ya que algunas operaciones pueden tardar una cantidad considerable de tiempo. Sin embargo, debe proceder con precaución. Si sospecha que tiene una condición de carrera, establecer la simultaneidad al `1` e inténtelo de nuevo la solicitud.

## <a name="configure-function-scriptfile"></a>Configurar la función `scriptFile`

De forma predeterminada, se ejecuta una función de PowerShell desde `run.ps1`, un archivo que comparte el mismo directorio primario correspondiente `function.json`.

El `scriptFile` propiedad en el `function.json` puede usarse para obtener una estructura de carpetas es similar al ejemplo siguiente:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.ps1
```

En este caso, el `function.json` para `myFunction` incluye un `scriptFile` propiedad hace referencia al archivo con la función exportada para ejecutar.

```json
{
  "scriptFile": "../lib/PSFunction.ps1",
  "bindings": [
    // ...
  ]
}
```

## <a name="use-powershell-modules-by-configuring-an-entrypoint"></a>Usar módulos de PowerShell mediante la configuración de un punto de entrada

Este artículo le haya mostrado las funciones de PowerShell en el valor predeterminado `run.ps1` generado por las plantillas de archivo de script.
Sin embargo, también puede incluir las funciones en módulos de PowerShell. Puede hacer referencia el código de función específica en el módulo mediante el `scriptFile` y `entryPoint` campos en el archivo function.json "archivo de configuración.

En este caso, `entryPoint` es el nombre de una función o cmdlet del módulo de PowerShell que se hace referenciado en `scriptFile`.

Tenga en cuenta la siguiente estructura de carpetas:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.psm1
```

Donde `PSFunction.psm1` contiene:

```powershell
function Invoke-PSTestFunc {
    param($InputBinding, $TriggerMetadata)

    Push-OutputBinding -Name OutputBinding -Value "output"
}

Export-ModuleMember -Function "Invoke-PSTestFunc"
```

En este ejemplo, la configuración de `myFunction` incluye un `scriptFile` propiedad que hace referencia a `PSFunction.psm1`, que es un módulo de PowerShell en otra carpeta.  El `entryPoint` referencias de propiedad el `Invoke-PSTestFunc` función, que es el punto de entrada en el módulo.

```json
{
  "scriptFile": "../lib/PSFunction.psm1",
  "entryPoint": "Invoke-PSTestFunc",
  "bindings": [
    // ...
  ]
}
```

Con esta configuración, el `Invoke-PSTestFunc` se ejecuta exactamente como un `run.ps1` haría.

## <a name="considerations-for-powershell-functions"></a>Consideraciones para las funciones de PowerShell

Cuando se trabaja con las funciones de PowerShell, tenga en cuenta las consideraciones de las secciones siguientes.

### <a name="cold-start"></a>Arranque en frío

Al desarrollar Azure Functions en el [modelo de hospedaje sin servidor](functions-scale.md#consumption-plan)en frío se inicia es una realidad. *Arranque en frío* se refiere al período de tiempo tarda la aplicación de función empezar a ejecutar para procesar una solicitud. Arranque en frío se produce con mayor frecuencia en el consumo previsto porque la aplicación de función se cierra durante períodos de inactividad.

### <a name="bundle-modules-instead-of-using-install-module"></a>Módulos de agrupación en lugar de usar `Install-Module`

El script se ejecuta en cada invocación. Evite el uso de `Install-Module` en el script. En su lugar use `Save-Module` antes de publicar para que la función no tiene que perder tiempo descargando el módulo. Si los arranques en frío que afectan a las funciones, considere la posibilidad de implementar la aplicación de función a un [plan de App Service](functions-scale.md#app-service-plan) establecido en *AlwaysOn* o a un [plan Premium](functions-scale.md#premium-plan-public-preview).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
* [Referencia para desarrolladores de Azure Functions](functions-reference.md)
* [Enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)

[referencia sobre host.json]: functions-host-json.md
