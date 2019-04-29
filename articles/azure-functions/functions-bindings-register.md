---
title: Registrar las extensiones de enlace de Azure Functions
description: Aprenda a registrar una extensión de enlace de Azure Functions en función de su entorno.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
origin.date: 02/18/2019
ms.date: 04/26/2019
ms.author: v-junlch
ms.openlocfilehash: 5534086d5754691f650370e465fa2c63210e0dc7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61437861"
---
# <a name="register-azure-functions-binding-extensions"></a>Registrar las extensiones de enlace de Azure Functions

Azure Functions admite HTTP y el temporizador de fábrica. Para trabajar con otros servicios, deberá instalar o *registrar* un [enlace](./functions-triggers-bindings.md) extensión. Las extensiones de enlace se proporcionan a través de paquetes de NuGet o de Azure Core Tools. 

En la tabla siguiente indica cómo y cuándo registrar los enlaces.

| Entorno de desarrollo |Registro<br/> en Functions 1.x  |Registro<br/> en Functions 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure Portal|Automático|[Automático con símbolo del sistema](#azure-portal-development)|
|Lenguajes que no sean de. NET o desarrollo de herramientas de Azure Core local|Automático|[Uso de comandos de la CLI de Core Tools](#local-development-azure-functions-core-tools)|
|Biblioteca de clases de C# con Visual Studio 2017|[Uso de herramientas NuGet](#c-class-library-with-visual-studio-2017)|[Uso de herramientas NuGet](#c-class-library-with-visual-studio-2017)|
|Biblioteca de clases de C# con Visual Studio Code|N/D|[Uso de la CLI de .NET Core](#c-class-library-with-visual-studio-code)|

Los siguientes tipos de enlace son excepciones que no requieren ningún registro explícito porque se registran automáticamente en todos los entornos y versiones: HTTP y temporizador.

> [!IMPORTANT]
> Este contenido para el resto de este artículo se aplica únicamente a Functions 2.x. Las extensiones de enlace explícitamente no están registradas en Functions 1.x, excepto cuando [creando un C# biblioteca de clases mediante Visual Studio 2017](#local-csharp).

## <a name="azure-portal-development"></a>Desarrollo con Azure Portal

Al crear una función o agregar un enlace, se le avisa cuando hay que registrar la extensión para el desencadenador o el enlace que requieren el registro. Responda a la pregunta haciendo clic en **Instalar** para registrar la extensión. La instalación puede tardar hasta 10 minutos en un plan de consumo. 

Solo necesita instalar cada extensión una vez para una aplicación de función determinada. Para los enlaces admitidos que no están disponibles en el portal o para actualizar una extensión instalada, también puede [instalar o actualizar manualmente las extensiones de enlace de Azure Functions desde el portal](install-update-binding-extensions-manual.md).  

## <a name="local-development-azure-functions-core-tools"></a>Desarrollo local con Azure Functions Core Tools

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
## <a name="c-class-library-with-visual-studio-2017"></a>Biblioteca de clases de C# con Visual Studio 2017

En **Visual Studio 2017**, puede instalar paquetes desde la consola del Administrador de paquetes mediante el comando [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package), tal como se muestra en el ejemplo siguiente:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <target_version>
```

El nombre del paquete que se usará para un enlace determinado se proporciona en el artículo de referencia de ese enlace. Para obtener un ejemplo, consulte la [sección de paquetes del artículo de referencia de enlace de Service Bus](functions-bindings-service-bus.md#packages---functions-1x).

Reemplace `<target_version>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

## <a name="c-class-library-with-visual-studio-code"></a>Biblioteca de clases de C# con Visual Studio Code

En **Visual Studio Code**, puede instalar paquetes desde el símbolo de sistema mediante el comando [dotnet add package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) de la CLI de .NET Core, tal como se muestra en el ejemplo siguiente:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <target_version>
```

La CLI de .NET Core solo puede utilizarse para el desarrollo de Azure Functions 2.x.

El nombre del paquete que se usará para un enlace determinado se proporciona en el artículo de referencia de ese enlace. Para obtener un ejemplo, consulte la [sección de paquetes del artículo de referencia de enlace de Service Bus](functions-bindings-service-bus.md#packages---functions-1x).

Reemplace `<target_version>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Ejemplo de desencadenador y enlace de Azure (función)](./functions-bindings-example.md)


