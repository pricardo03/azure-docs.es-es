---
title: Introducción a las versiones de tiempo de ejecución de Azure Functions
description: Azure Functions admite varias versiones del runtime. Conozca las diferencias entre ellas y cómo elegir la más adecuada en su caso.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/03/2018
ms.author: glenga
ms.openlocfilehash: 6988fb547b07f81891efea3caad8bf34f4c8a476
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036321"
---
# <a name="azure-functions-runtime-versions-overview"></a>Introducción a las versiones de tiempo de ejecución de Azure Functions

 Hay dos versiones principales de Azure Functions runtime: 1.x y 2.x. La versión actual (donde se están realizando mejoras y trabajos en características nuevas) es la 2.x., aunque para los escenarios de producción se admiten las dos.  A continuación se detallan algunas de las diferencias entre ambas versiones y la manera de crear cada una de ellas y actualizar de 1.x a 2.x.

> [!NOTE]
> Este artículo se refiere al servicio en la nube de Azure Functions. Para información acerca del producto en versión preliminar que le permite ejecutar Azure Functions de forma local, consulte la [Introducción a Azure Functions Runtime](functions-runtime-overview.md).

## <a name="cross-platform-development"></a>Desarrollo multiplataforma

La versión 2.x del entorno de ejecución funciona en .NET Core 2, lo cual permite su ejecución en todas las plataformas que admita .NET Core, incluidos macOS y Linux. La ejecución en .NET Core hace posible escenarios de desarrollo y hospedaje multiplataforma.

En cambio, la versión 1.x del entorno de ejecución admite solo desarrollo y hospedaje en Azure Portal o en equipos Windows.

## <a name="languages"></a>Languages

La versión 2.x del entorno de ejecución emplea un nuevo modelo de extensibilidad de lenguajes. En la versión 2.x, todas las funciones de una aplicación de función deben compartir el mismo lenguaje. El lenguaje de las funciones en una aplicación de función se elige al crear la aplicación.

Los lenguajes experimentales de la versión 1.x de Azure Functions no se actualizarán para usar el nuevo modelo, así que no se admiten en 2.x. En la siguiente tabla se indican los lenguajes de programación que se admiten actualmente en cada versión del entorno de ejecución.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Para más información, consulte [Lenguajes admitidos](supported-languages.md).

## <a name="creating-1x-apps"></a>Ejecución en la versión 1.x

De forma predeterminada, las aplicaciones de función que se crean en Azure Portal se establecen en la versión 2.x. Cuando sea posible, debe usar esta versión del entorno de ejecución, ya que en ella se invierte en nuevas características. Si lo necesita, puede seguir ejecutando una aplicación de función en la versión 1.x del entorno de ejecución. Solo puede cambiar la versión del entorno de ejecución después de crear la aplicación de función, pero antes de agregar funciones. Para información sobre cómo anclar la versión 1.x del entorno de ejecución, consulte [Visualización y actualización de la versión actual del entorno de ejecución](set-runtime-version.md#view-and-update-the-current-runtime-version).

## <a name="migrating-from-1x-to-2x"></a>Migración de 1.x a 2.x

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

La versión 2.x del entorno de ejecución usa un nuevo [modelo de extensibilidad de enlaces](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) que ofrece estas ventajas:

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
