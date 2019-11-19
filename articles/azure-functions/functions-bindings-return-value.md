---
title: Uso del valor devuelto de una función de Azure
description: Aprenda a administrar valores devueltos de Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: reference
ms.date: 01/14/2019
ms.author: cshoe
ms.openlocfilehash: 8dd5a4d9d869c879ed402c5450690f0a691e1d2c
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74074394"
---
# <a name="using-the-azure-function-return-value"></a>Uso del valor devuelto de una función de Azure

En este artículo se explica cómo funcionan los valores devueltos dentro de una función.

En los lenguajes que tienen un valor devuelto, puede enlazar el [enlace de salida](./functions-triggers-bindings.md#binding-direction) de una función al valor devuelto:

* En una biblioteca de clases C#, aplique el atributo de enlace de salida para el valor devuelto del método.
* En otros lenguajes, establezca la propiedad `name` de *function.json* en `$return`.

Si hay varios enlaces de salida, use el valor devuelto para solo uno de ellos.

En C# y script de C#, los parámetros `out` y los [objetos de recopilador](functions-reference-csharp.md#writing-multiple-output-values) son formas alternativas de enviar datos a un enlace de salida.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Este es el código de C# que utiliza el valor devuelto de un enlace de salida, seguido de un ejemplo asincrónico:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

Este es el enlace de salida del archivo *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Este es el código de script de C#, seguido de un ejemplo asincrónico:

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

# <a name="ftabfsharp"></a>[F#](#tab/fsharp)

Este es el enlace de salida del archivo *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Este es el código de F#:

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Este es el enlace de salida del archivo *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

En JavaScript, el valor devuelto va en el segundo parámetro para `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Este es el enlace de salida del archivo *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Este es el código de Python:

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

---

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Control de errores de enlace de Azure Functions](./functions-bindings-errors.md)
