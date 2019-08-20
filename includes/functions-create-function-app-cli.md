---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/05/2019
ms.author: glenga
ms.openlocfilehash: c90587ebd7b0a608e41f19ab600f82dc484bad0a
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949932"
---
## <a name="create-the-local-function-app-project"></a>Creación del proyecto de aplicación de función local

Ejecute el comando siguiente desde la línea de comandos para crear un proyecto de aplicación de función en la carpeta `MyFunctionProj` del directorio local actual. También se crea un repositorio GitHub en `MyFunctionProj`.

```command
func init MyFunctionProj
```

Cuando se le pida, seleccione un tiempo de ejecución del trabajo de entre los siguientes lenguajes:

+ `dotnet`: crea un proyecto de biblioteca de clases .NET (.csproj).
+ `node`: crea un proyecto basado en Node.js. Elija `javascript` o `typescript`. 
+ `python`: para un proyecto de Python, complete en su lugar [Creación de una función desencadenada mediante HTTP en Azure](../articles/azure-functions/functions-create-first-function-python.md).
+ `powershell`: para un proyecto de PowerShell, complete en su lugar [Creación de su primera función de PowerShell en Azure](../articles/azure-functions/functions-create-first-function-powershell.md). 


Una vez que haya creado el proyecto, use el siguiente comando para ir a la nueva carpeta del proyecto `MyFunctionProj`.

```command
cd MyFunctionProj
```
