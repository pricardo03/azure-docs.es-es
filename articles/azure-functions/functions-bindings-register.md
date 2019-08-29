---
title: Registro de las extensiones de enlace de Azure Functions
description: Aprenda a registrar una extensión de enlace de Azure Functions basada en su entorno.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 93ced443a73d5499d8b305770c3c866c26d540f0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70086463"
---
# <a name="register-azure-functions-binding-extensions"></a>Registro de las extensiones de enlace de Azure Functions

En la versión de Azure Functions 2.x, los [enlaces](./functions-triggers-bindings.md) están disponibles como paquetes independientes de Functions Runtime. Aunque las funciones de .NET acceden a los enlaces a través de paquetes NuGet, los grupos de extensiones permiten a otras funciones acceder a todos los enlaces mediante una opción de configuración.

Tenga en cuenta los siguientes elementos relacionados con las extensiones de enlaces:

- Las extensiones de enlaces no están registradas explícitamente en Functions 1.x, excepto cuando [se crea una biblioteca de clases en C# mediante Visual Studio](#local-csharp).

- De forma predeterminada, se admiten los desencadenadores de HTTP y de temporizador y no requieren una extensión.

En la tabla siguiente se indica cuándo y cómo registrar los enlaces.

| Entorno de desarrollo |Registro<br/> en Functions 1.x  |Registro<br/> en Functions 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Portal de Azure|Automático|Automático|
|Desarrollo en lenguajes que no son .NET o en Azure Functions Core Tools local|Automático|[Uso de Azure Functions Core Tools y agrupaciones de extensiones](#extension-bundles)|
|Biblioteca de clases de C# con Visual Studio|[Uso de herramientas NuGet](#vs)|[Uso de herramientas NuGet](#vs)|
|Biblioteca de clases de C# con Visual Studio Code|N/D|[Uso de la CLI de .NET Core](#vs-code)|

## <a name="extension-bundles"></a>Agrupaciones de extensiones para el desarrollo local

Las agrupaciones de extensiones son una tecnología de desarrollo de la versión 2.x del runtime que le permite agregar un conjunto compatible de extensiones de enlaces de Functions al proyecto de aplicación de funciones. Estos paquetes de extensiones se incluyen en el paquete de implementación al implementar en Azure. Las agrupaciones hacen que todos los enlaces publicados por Microsoft estén disponibles mediante un valor del archivo *host.json*. Los paquetes de extensiones definidos en una agrupación son compatibles entre sí, lo cual le ayuda a evitar conflictos entre ellos. Cuando desarrolle localmente, asegúrese de que está utilizando la versión más reciente de [Azure Functions Core Tools](functions-run-local.md#v2).

Use agrupaciones de extensiones para todo el desarrollo local con Azure Functions Core Tools o Visual Studio Code.

Si no usa agrupaciones de extensiones, deberá instalar el SDK de .NET Core 2.x en el equipo local antes de instalar las extensiones de enlaces. Las agrupaciones permiten eliminar este requisito para el desarrollo local. 

Para usar agrupaciones de extensiones, actualice el archivo *host.json* para que incluya la siguiente entrada para `extensionBundle`:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

Las siguientes propiedades están disponibles en `extensionBundle`:

| Propiedad | DESCRIPCIÓN |
| -------- | ----------- |
| **`id`** | Espacio de nombres de las agrupaciones de extensiones de Microsoft Azure Functions. |
| **`version`** | Versión de la agrupación que va a instalar. Functions Runtime siempre elige la máxima versión permitida definida por el rango o intervalo de versiones. El valor de versión anterior permite todas las versiones de agrupaciones desde la 1.0.0 en adelante pero sin incluir la 2.0.0. Para más información, consulte la [notación de intervalo para especificar intervalos de versiones](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges-and-wildcards). |

Las versiones de las agrupaciones aumentan a medida que cambian los paquetes de la agrupación. Los cambios de versión principal se producen cuando los paquetes de la agrupación aumentan debido a una versión principal, lo cual normalmente coincide con un cambio en la versión principal de Functions Runtime.  

El conjunto actual de extensiones instaladas mediante la agrupación predeterminada se enumera en este [archivo extensions.json](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json).

<a name="local-csharp"></a>

## <a name="vs"></a> Biblioteca de clases de C\# con Visual Studio

En **Visual Studio**, puede instalar paquetes desde la Consola del Administrador de paquetes mediante el comando [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package), tal como se muestra en el ejemplo siguiente:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

El nombre del paquete que se usa para un enlace determinado se proporciona en el artículo de referencia de ese enlace. Para obtener un ejemplo, consulte la [sección de paquetes del artículo de referencia de enlace de Service Bus](functions-bindings-service-bus.md#packages---functions-1x).

Reemplace `<TARGET_VERSION>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

Si usa `Install-Package` para hacer referencia a un enlace, no es necesario utilizar [paquetes de extensión](#extension-bundles). Este enfoque es específico de las bibliotecas de clases compiladas en Visual Studio.

## <a name="vs-code"></a> Biblioteca de clases de C# con Visual Studio Code

> [!NOTE]
> Se recomienda usar [agrupaciones de extensiones](#extension-bundles) para que Functions instale automáticamente un conjunto compatible de paquetes de extensiones de enlace.

En **Visual Studio Code**, instale paquetes para un proyecto de biblioteca de clases de C# desde el símbolo del sistema mediante el comando [dotnet add package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) de la CLI de .NET Core. En el ejemplo siguiente se muestra cómo agregar un enlace:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

La CLI de .NET Core solo puede utilizarse para el desarrollo de Azure Functions 2.x.

Reemplace `<BINDING_TYPE_NAME>` por el nombre del paquete proporcionado en el artículo de referencia para el enlace que desee. Puede encontrar el artículo de referencia de enlace deseado en la [lista de enlaces admitidos](./functions-triggers-bindings.md#supported-bindings).

Reemplace `<TARGET_VERSION>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Ejemplo de desencadenador y enlace de Azure Functions](./functions-bindings-example.md)
