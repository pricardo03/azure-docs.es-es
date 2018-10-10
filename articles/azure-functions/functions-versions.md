---
title: Introducción a las versiones de tiempo de ejecución de Azure Functions
description: Azure Functions admite varias versiones del runtime. Conozca las diferencias entre ellas y cómo elegir la más adecuada en su caso.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: glenga
ms.openlocfilehash: a601ea42549abad84d6cab5c429cf94147776436
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978631"
---
# <a name="azure-functions-runtime-versions-overview"></a>Introducción a las versiones de tiempo de ejecución de Azure Functions

 Hay dos versiones principales del entorno de ejecución de Azure Functions: 1.x y 2.x. La versión actual (donde se están realizando mejoras y trabajos en características nuevas) es la 2.x., aunque para los escenarios de producción se admiten las dos.  A continuación se detallan algunas de las diferencias entre ambas versiones y la manera de crear cada una de ellas y actualizar de 1.x a 2.x.

> [!NOTE] 
> Este artículo se refiere al servicio en la nube de Azure Functions. Para información acerca del producto en versión preliminar que le permite ejecutar Azure Functions de forma local, consulte la [Introducción a Azure Functions Runtime](functions-runtime-overview.md).

## <a name="creating-1x-apps"></a>Creación de aplicaciones en versión 1.x

Las aplicaciones nuevas se crean en Azure Portal con la versión 2.x de manera predeterminada, ya que es la más reciente y donde se están realizando inversiones en nuevas características.  Pero todavía puede crear aplicaciones v1.x haciendo lo siguiente.

1. Creación de una instancia de Azure Functions en Azure Portal
1. Abra la aplicación creada y, con ella en blanco, abra **Configuración de Functions**
1. Cambie la versión de ~2 a ~1.  *Esta opción se deshabilitará si tiene funciones en la aplicación*.
1. Haga clic en Guardar y reinicie la aplicación.  Todas las plantillas ahora se crearán y se ejecutarán en la versión 1.x.

## <a name="cross-platform-development"></a>Desarrollo multiplataforma

El tiempo de ejecución 1.x admite solo desarrollo y hospedaje en el portal o en Windows. Runtime 2.x funciona en .NET Core 2, lo que significa que se puede ejecutar en todas las plataformas admitidas por .NET Core, incluidos macOS y Linux. Esto hace posible escenarios de desarrollo y hospedaje multiplataforma.

## <a name="languages"></a>Lenguajes

El tiempo de ejecución 2.x emplea un nuevo modelo de extensibilidad de lenguajes. Además, para mejorar el rendimiento y las herramientas, las aplicaciones con versión 2.x limitan a un solo lenguaje las funciones. Los lenguajes admitidos actualmente en la versión 2.x son C#, F#, JavaScript y Java. Los lenguajes experimentales de 1.x de Azure Functions no se han actualizado para usar el nuevo modelo, así que no se admiten en 2.x. En la tabla siguiente se indican qué lenguajes de programación se admiten en cada versión del runtime.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Para más información, consulte [Lenguajes admitidos](supported-languages.md).

## <a name="migrating-from-1x-to-2x"></a>Migración de 1.x a 2.x

Puede que desee mover una aplicación existente escrita en la versión 1.x a 2.x.  La mayoría de las consideraciones necesarias para cambiar de una versión a otra tienen que ver con los cambios en el tiempo de ejecución del lenguaje enumerados anteriormente (por ejemplo el cambio de C# de .NET Framework 4.7 a .NET Core 2).  Deberá asegurarse de que el código y las bibliotecas son compatibles con los tiempos de ejecución del lenguaje que se utilice.  Además, asegúrese de anotar los cambios en el desencadenador, los enlaces y las características que se resaltan a continuación.

### <a name="changes-in-triggers-and-bindings"></a>Cambio en los desencadenadores y los enlaces

Aunque la mayoría de las configuraciones y propiedades de desencadenador y enlace son las mismas en las dos versiones, en 2.x deberá instalar cualquier desencadenador o enlace en la aplicación. La única excepción son los desencadenadores HTTP y el temporizador.  Consulte [Registro de extensiones de enlace](./functions-triggers-bindings.md#register-binding-extensions).  Tenga en cuenta que también puede haber cambios de una versión a otra en `function.json` o en atributos de la función (por ejemplo, la propiedad `connection` de CosmosDB ahora es `ConnectionStringSetting`).  Consulte la [tabla de enlaces existentes](#bindings) para vínculos a la documentación de cada enlace.

### <a name="changes-in-features-available"></a>Cambios en las características disponibles

Además de los cambios en los lenguajes y los enlaces, hay algunas características que se han eliminado, actualizado o reemplazado de una versión a otra.  A continuación se muestran algunas de las principales consideraciones al empezar con la versión 2.x después de usar 1.x.  En v2.x se han realizado los siguientes cambios:

* Las claves para llamar a una función siempre se almacenan en el almacenamiento de blobs cifrado. En la versión 1.x, de forma predeterminada se encontraban en el almacenamiento de archivos y se podían mover a blobs al habilitar características tipo ranura.  Si actualiza una aplicación de 1.x a 2.x y los secretos se encuentran en el almacenamiento de archivos, se restablecerán.
* Para mejorar el rendimiento, se quitan los desencadenadores del tipo webhook y se reemplazan por desencadenadores HTTP.
* La configuración de host (`host.json`) debe estar vacía o contener `version` de `2.0` para una de las propiedades.
* Para mejorar la supervisión y la capacidad de observación, el panel de WebJobs (`AzureWebJobsDashboard`) se reemplaza por [Azure Application Insights](functions-monitoring.md) (`APPINSIGHTS_INSTRUMENTATIONKEY`)
* La configuración de la aplicación (`local.settings.json`) requiere un valor para la propiedad `FUNCTIONS_WORKER_RUNTIME` que se asigne al lenguaje de la aplicación `dotnet | node | java | python`.
    * Para mejorar el tiempo de inicio y la superficie, las aplicaciones se limitan a un solo lenguaje. Puede publicar varias aplicaciones con funciones en distintos lenguajes para la misma solución.
* El tiempo de espera predeterminado para las funciones en un plan de App Service es de 30 minutos.  Aún se puede establecer manualmente en ilimitado.
* [Debido a limitaciones de .NET Core](https://github.com/Azure/azure-functions-host/issues/3414), los scripts `.fsx` para las funciones de F# se han eliminado. Todavía se admiten las funciones compiladas de F#.
* El formato de los desencadenadores basados en webhook (por ejemplo, Event Grid) ha cambiado a `https://{app}/runtime/webhooks/{triggerName}`

### <a name="upgrading-a-locally-developed-application"></a>Actualización de una aplicación desarrollada de forma local

Si la aplicación v1.x se desarrolló localmente, puede realizar cambios en la aplicación o el proyecto para que sea compatible con v2.  Se recomienda crear una nueva aplicación y el puerto en el código de la nueva aplicación.  Aunque hay cambios que podrían hacerse en una aplicación existente para realizar una actualización en el sitio, hay otras mejoras entre v1 y v2 que el código heredado probablemente no aprovecha (por ejemplo, en C#, el cambio de `TraceWriter` a `ILogger`).  

La ruta de acceso recomendada es empezar desde una de las plantillas de v2 y moverse por el código a una aplicación o un proyecto nuevos.

#### <a name="visual-studio-runtime-versions"></a>Versiones de Runtime en Visual Studio

En Visual Studio se selecciona la versión de Runtime al crear un proyecto.  Visual Studio tiene los bits de las dos versiones principales y dinámicamente utiliza la correcta para el proyecto.  Estos valores proceden del archivo `.csproj`.  Para las aplicaciones 1.x, el proyecto tiene las propiedades

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

En la versión 2 son las propiedades del proyecto son

```xml
<TargetFramework>netstandard2.0</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Al hacer clic en Depurar o Publicar se establece correctamente la versión para la configuración del proyecto.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code y Azure Functions Core Tools

Otras herramientas locales se encuentran en Azure Functions Core Tools.  Estas se instalan en la máquina; por otra parte, en general, en la máquina de desarrollo solo se instala una versión a la vez.  Consulte las [instrucciones de instalación de versiones específicas de Core Tools](./functions-run-local.md).

Para VS Code es posible que deba actualizar la configuración de usuario en `azureFunctions.projectRuntime` para que coincida con la versión de las herramientas instaladas.  Esto también actualizará las plantillas y los lenguajes que aparecen durante la creación de nuevas aplicaciones.

### <a name="changing-version-of-apps-in-azure"></a>Cambio de la versión de las aplicaciones en Azure

Las versiones de la aplicación publicada se establecen mediante la configuración de la aplicación `FUNCTIONS_RUNTIME_VERSION`.  Se establece en `~2` para las aplicaciones con v2 y en `~1` para las aplicaciones con v1.  Es muy recomendable cambiar la versión de Runtime de una aplicación que tiene funciones existentes publicadas sin cambiar también el código de esas funciones.  La ruta recomendada es crear una nueva aplicación de función y establecer la versión adecuada, probar los cambios y deshabilitar o eliminar la aplicación anterior.

## <a name="bindings"></a>Enlaces 

El tiempo de ejecución 2.x usa un nuevo [modelo de extensibilidad de enlaces](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) que ofrece estas ventajas:

* Compatibilidad con extensiones de enlace de terceros.
* Desacoplamiento de tiempo de ejecución y enlaces. Esto permite el control de versiones y la publicación de las extensiones de enlace de forma independiente. Por ejemplo, puede elegir actualizar a una versión de una extensión que se basa en una versión más reciente de un SDK subyacente.
* Un entorno de ejecución más ligero, donde solo se conocen y se cargan en tiempo de ejecución los enlaces que están en uso.

Todos los enlaces integrados de Azure Functions han adoptado este modelo y ya no se incluyen de forma predeterminada, excepto los desencadenadores Timer y HTTP. Esas extensiones se instalan automáticamente al crear las funciones a través de herramientas como Visual Studio o el portal.

En la tabla siguiente se indica qué enlaces se admiten en cada versión de tiempo de ejecución.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* [Codificación y comprobación de Azure Functions en un entorno local](functions-run-local.md)
* [Cómo elegir las versiones del entorno de ejecución de Azure Functions](set-runtime-version.md)
* [Notas de la versión](https://github.com/Azure/azure-functions-host/releases)
