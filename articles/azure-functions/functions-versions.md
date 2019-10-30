---
title: Introducción a las versiones de tiempo de ejecución de Azure Functions
description: Azure Functions admite varias versiones del runtime. Conozca las diferencias entre ellas y cómo elegir la más adecuada en su caso.
author: ggailey777
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: glenga
ms.openlocfilehash: 9ca7006bb842cbe235d2e982e611613e1fd74ed9
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72597433"
---
# <a name="azure-functions-runtime-versions-overview"></a>Introducción a las versiones de tiempo de ejecución de Azure Functions

Las versiones principales del entorno de ejecución de Azure Functions se relacionan con la versión de .NET en la que se basa ese entorno. En la tabla siguiente se indica la versión actual del entorno de ejecución, el nivel de versión y la versión de .NET relacionada. 

| Versión en tiempo de ejecución | Nivel de versión<sup>1</sup> | Versión de .NET | 
| --------------- | ------------- | ------------ |
| 3.x  | Vista previa | .NET Core 3.x | 
| 2.x | GA | .NET Core 2.2 |
| 1.x | GA<sup>2</sup> | .NET Framework 4.6<sup>3</sup> |

<sup>1</sup> Las versiones de disponibilidad general se admiten en escenarios de producción.   
<sup>2</sup> La versión 1. x está en modo de mantenimiento. Las mejoras solo se proporcionan en versiones posteriores.   
<sup>3</sup> Solo admite el desarrollo en Azure Portal o localmente en equipos Windows.

>[!NOTE]  
> La versión 3. x del entorno de ejecución de Functions está en versión preliminar y no se admite en entornos de producción. Para más información sobre cómo probar la versión 3.x, consulte [este anuncio](https://dev.to/azure/develop-azure-functions-using-net-core-3-0-gcm).

En este artículo se detallan algunas de las diferencias entre las distintas versiones, cómo se puede crear cada versión y cómo se cambian las versiones.

## <a name="languages"></a>Lenguajes

A partir de la versión 2.x, el entorno de ejecución usa un modelo de extensibilidad de lenguaje y todas las funciones de una aplicación de funciones deben compartir el mismo lenguaje. El lenguaje de las funciones de una aplicación de funciones se elige cuando se crea la aplicación y se mantiene en el valor [FUNCTIONS\_WORKER\_RUNTIME](functions-app-settings.md#functions_worker_runtime). 

Los lenguajes experimentales de la versión 1.x de Azure Functions no pueden utilizar el nuevo modelo, así que no se admiten en la versión 2.x. En la siguiente tabla se indican los lenguajes de programación que se admiten actualmente en cada versión del entorno de ejecución.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Para más información, consulte [Lenguajes admitidos](supported-languages.md).

## <a name="creating-1x-apps"></a>Ejecución en una versión específica

De forma predeterminada, las aplicaciones de funciones que se crean en Azure Portal y en la CLI de Azure se establecen en la versión 2.x. Siempre que sea posible, debe usar esta versión del entorno de ejecución. Si lo necesita, puede seguir ejecutando una aplicación de función en la versión 1.x del entorno de ejecución. Solo puede cambiar la versión del entorno de ejecución después de crear la aplicación de función, pero antes de agregar funciones. Para información sobre cómo anclar la versión 1.x del entorno de ejecución, consulte [Visualización y actualización de la versión actual del entorno de ejecución](set-runtime-version.md#view-and-update-the-current-runtime-version).

También puede actualizar a la versión 3.x del entorno de ejecución, que se encuentra en versión preliminar. Hágalo si necesita ejecutar las funciones en .NET Core 3.x. Para información sobre cómo actualizar a la versión 3.x, consulte [Visualización y actualización de la versión actual del entorno de ejecución](set-runtime-version.md#view-and-update-the-current-runtime-version).

## <a name="migrating-from-1x-to-later-versions"></a>Migración desde la versión 1.x a versiones posteriores

Puede migrar una aplicación existente escrita para usar la versión 1.x del entorno de ejecución para que use 2.x. La mayoría de los cambios que debe realizar están relacionados con cambios del entorno de ejecución del lenguaje, como en la API de C#, de .NET Framework 4.7 y .NET Core 2. Deberá asegurarse de que el código y las bibliotecas son compatibles con la versión del entorno de ejecución del lenguaje que utilice. Finalmente, asegúrese de anotar los cambios en el desencadenador, los enlaces y las características que se resaltan a continuación. Para los mejores resultados de migración, debe crear una nueva aplicación de función para la versión 2.x y enrutar el código existente en 1.x a esta nueva aplicación.  

### <a name="changes-in-triggers-and-bindings"></a>Cambio en los desencadenadores y los enlaces

La versión 2.x requiere la instalación de extensiones para desencadenadores y enlaces específicos que usan las funciones de la aplicación. La única excepción son los desencadenadores HTTP y el temporizador, que no requieren extensión.  Para más información, consulte la sección sobre [Registro e instalación de extensiones de enlace](./functions-bindings-register.md).

También ha habido algunos cambios en `function.json` o los atributos de la función de una versión a otra. Por ejemplo, la propiedad `path` del centro de eventos es ahora `eventHubName`. Consulte la [tabla de enlaces existentes](#bindings) para vínculos a la documentación de cada enlace.

### <a name="changes-in-features-and-functionality"></a>Cambios en las características y la funcionalidad

En la nueva versión también se han eliminado, actualizado o reemplazado algunas características. En esta sección se describen los cambios que se observan en la versión 2.x después de haber utilizado la versión 1.x.

En la versión 2.x se han realizado los siguientes cambios:

* Las claves para llamar a los puntos de conexión HTTP siempre se almacenan cifradas en Azure Blob Storage. En la versión 1.x, se almacenaban en Azure File Storage de manera predeterminada. Al actualizar una aplicación de la versión 1.x a la versión 2.x, los secretos existentes que se encuentran en el almacenamiento de archivos se restablecen.

* La versión 2.x del entorno de ejecución no incluye compatibilidad integrada con proveedores de webhooks. Este cambio se realizó para mejorar el rendimiento. Todavía puede usar desencadenadores HTTP como puntos de conexión para los webhooks.

* El archivo de configuración de host (host.json) debe estar vacío o tener la cadena `"version": "2.0"`.

* Para mejorar la supervisión, el panel de WebJobs del portal, que usaba la configuración [`AzureWebJobsDashboard`](functions-app-settings.md#azurewebjobsdashboard) se reemplaza por Azure Application Insights, que usa la configuración [`APPINSIGHTS_INSTRUMENTATIONKEY`](functions-app-settings.md#appinsights_instrumentationkey). Para más información, consulte [Supervisión de Azure Functions](functions-monitoring.md).

* Todas las funciones de una aplicación de función deben compartir el mismo lenguaje. Al crear una aplicación de función, debe elegir una pila del entorno de ejecución para la aplicación. La pila del entorno de ejecución se especifica con el valor [`FUNCTIONS_WORKER_RUNTIME`](functions-app-settings.md#functions_worker_runtime) en la configuración de la aplicación. Este requisito se agregó para mejorar el tiempo de inicio y la superficie de memoria. Al desarrollar localmente, también debe incluir esta configuración en el [archivo local.settings.json](functions-run-local.md#local-settings-file).

* El tiempo de espera predeterminado para las funciones en un plan de App Service ha cambiado a 30 minutos. Puede cambiar manualmente el tiempo de espera de vuelta a ilimitado mediante la configuración [functionTimeout](functions-host-json.md#functiontimeout) de host.json.

* Se implementan de forma predeterminada limitaciones de simultaneidad HTTP para las funciones del plan de consumo, cuyo valor predeterminado es de 100 solicitudes simultáneas por instancia. Puede cambiar esto en la configuración [`maxConcurrentRequests`](functions-host-json.md#http) del archivo host.json.

* A causa de las [limitaciones de .NET Core](https://github.com/Azure/azure-functions-host/issues/3414), se ha eliminado la compatibilidad con las funciones de script (.fsx) de F#. Todavía se admiten las funciones compiladas de F# (.fs).

* El formato de dirección URL de los webhooks de desencadenador de Event Grid ha cambiado a `https://{app}/runtime/webhooks/{triggerName}`.

### <a name="migrating-a-locally-developed-application"></a>Migración de una aplicación desarrollada de forma local

Puede tener proyectos de aplicación de función existentes que haya desarrollado de forma local con la versión 1.x. del entorno de ejecución. Para actualizar a la versión 2.x, debe crear un proyecto de aplicación de función local con la versión 2.x y enrutar el código existente a la nueva aplicación. Puede actualizar manualmente el proyecto y el código existente, una especie de actualización "local". Sin embargo, hay otras mejoras de la versión 1.x a la 2.x que todavía tendrá que hacer. Por ejemplo, en C# se modificó el objeto de depuración de `TraceWriter` a `ILogger`. Al crear un nuevo proyecto con la versión 2.x, se empieza por las funciones actualizadas en función de las plantillas más recientes de la versión 2.x.

#### <a name="visual-studio-runtime-versions"></a>Versiones de Runtime en Visual Studio

En Visual Studio se selecciona la versión del entorno de ejecución al crear un proyecto. Las herramientas de Azure Functions para Visual Studio son compatibles con las dos versiones principales del entorno de ejecución. Se usa la versión correcta al depurar y publicar en función de configuración del proyecto. La configuración de la versión se define en el archivo `.csproj` en las siguientes propiedades:

##### <a name="version-1x"></a>Versión 1.x

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

##### <a name="version-2x"></a>Versión 2.x

```xml
<TargetFramework>netcoreapp2.2</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Al depurar o publicar el proyecto, se usa la versión correcta del entorno de ejecución.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code y Azure Functions Core Tools

[Azure Functions Core Tools](functions-run-local.md) se usa para el desarrollo de la línea de comandos, pero también lo usa la [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code. Para desarrollar con la versión 2.x, instale la versión 2.x de Core Tools. El desarrollo con la versión 1.x requiere la versión 1.x de Core Tools. Para más información, consulte [Instalación de Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools).

Para el desarrollo en Visual Studio Code es posible que deba actualizar la configuración de usuario en `azureFunctions.projectRuntime` para que coincida con la versión de las herramientas instaladas.  Esta configuración también actualiza las plantillas y los lenguajes utilizados durante la creación de la aplicación de función.

### <a name="changing-version-of-apps-in-azure"></a>Cambio de la versión de las aplicaciones en Azure

La versión del sistema en ejecución de Functions que usan las aplicaciones publicadas en Azure viene determinada por la configuración de la aplicación [`FUNCTIONS_EXTENSION_VERSION`](functions-app-settings.md#functions_extension_version). El valor `~2` es para la versión 2.x del entorno de ejecución y `~1`, para 1.x. No cambie esta configuración sin motivo, ya que puede requerir otros cambios de configuración de la aplicación y de código en las funciones. Para información acerca de la manera recomendada de migrar la aplicación de función a una versión diferente del entorno de ejecución, consulte [Cómo seleccionar un destino para versiones de Azure Functions Runtime](set-runtime-version.md).

## <a name="bindings"></a>Enlaces

A partir de la versión 2.x, el entorno de ejecución usa un nuevo [modelo de extensibilidad de enlaces](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) que ofrece estas ventajas:

* Compatibilidad con extensiones de enlace de terceros.

* Desacoplamiento de tiempo de ejecución y enlaces. Este cambio permite el control de versiones y la publicación de las extensiones de enlace de forma independiente. Por ejemplo, puede elegir actualizar a una versión de una extensión que se basa en una versión más reciente de un SDK subyacente.

* Un entorno de ejecución más ligero, donde solo se conocen y se cargan en tiempo de ejecución los enlaces que están en uso.

A excepción de los desencadenadores HTTP y el temporizador, todos los enlaces deben agregarse explícitamente al proyecto de aplicación de función o registrarse en el portal. Para más información, consulte [Registro de extensiones de enlace](./functions-bindings-expressions-patterns.md).

En la siguiente tabla se indica qué enlaces se admiten en cada versión del entorno de ejecución.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* [Codificación y comprobación de Azure Functions en un entorno local](functions-run-local.md)
* [Cómo elegir las versiones del entorno de ejecución de Azure Functions](set-runtime-version.md)
* [Notas de la versión](https://github.com/Azure/azure-functions-host/releases)
