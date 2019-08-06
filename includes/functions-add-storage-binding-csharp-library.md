---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 4db460a5dcefb49de3ad2b594d3957cbcf7445c3
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592819"
---
En un proyecto de biblioteca de clases de C#, los enlaces se definen como atributos de enlace en el método de función. Después se genera automáticamente el archivo *function.json* en función de estos atributos.

Abra el archivo de proyecto *HttpTrigger.cs* y agregue la siguiente instrucción `using`:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

Agregue el siguiente parámetro a la definición de método `Run`:

```cs
[Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg
```

El parámetro `msg` es de tipo `ICollector<T>`, que representa una colección de mensajes escritos en un enlace de salida cuando se completa la función. En este caso, la salida es una cola de almacenamiento denominada `outqueue`. La cadena de conexión de la cuenta de Storage la establece `StorageAccountAttribute`. Este atributo indica la configuración que contiene la cadena de conexión de la cuenta de Storage y se puede aplicar en el nivel de clase, método o parámetro. En este caso, puede omitir `StorageAccountAttribute`, puesto que ya está usando la cuenta de almacenamiento predeterminada.

La definición del método Run debe ahora parecerse a la siguiente:  

```cs
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
```
