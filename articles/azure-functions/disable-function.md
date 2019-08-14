---
title: Deshabilitamiento de funciones en Azure Functions
description: Aprenda a deshabilitar y a habilitar las funciones de Azure Functions 1.x y 2.x.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: glenga
ms.openlocfilehash: 183056d01146194b2854a70df790802e1a0bb839
ms.sourcegitcommit: f7998db5e6ba35cbf2a133174027dc8ccf8ce957
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68782231"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Deshabilitamiento de funciones en Azure Functions

En este artículo se explica cómo deshabilitar una función de Azure Functions. *Deshabilitar* una función significa hacer que el entorno en tiempo de ejecución omita el desencadenador automático que se ha definido para esa función. La forma de hacerlo dependerá de la versión del entorno en tiempo de ejecución y del lenguaje de programación:

* Functions 2.x:
  * Una forma para todos los lenguajes
  * Forma opcional para las bibliotecas de clases de C#
* Functions 1.x:
  * Lenguajes de scripting
  * Biblioteca de clases de C#

## <a name="functions-2x---all-languages"></a>Functions 2.x: todos los lenguajes

En Functions 2.x puede deshabilitar una función mediante una configuración de aplicación en el formato `AzureWebJobs.<FUNCTION_NAME>.Disabled`. Puede crear y modificar esta configuración mediante programación con la CLI de Azure. También puede realizar esta acción desde la pestaña **Administrar** de la función en [Azure Portal](https://portal.azure.com). 

### <a name="azure-cli"></a>CLI de Azure

En el CLI de Azure, use el comando [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) para crear y modificar la configuración de la aplicación. En el siguiente comando se deshabilita una función denominada `QueueTrigger` al crear una configuración de aplicaciones denominada `AzureWebJobs.QueueTrigger.Disabled` y establecerla en `true`. 

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=true
```

Para volver a habilitar la función, vuelva a ejecutar el mismo comando con un valor `false`.

```azurecli-interactive
az functionapp config appsettings set --name <myFunctionApp> \
--resource-group <myResourceGroup> \
--settings AzureWebJobs.QueueTrigger.Disabled=false
```

### <a name="portal"></a>Portal

También puede usar el conmutador **Estado de la función** en la pestaña **Administrar** de la función. El conmutador funciona mediante la creación y eliminación de la configuración de la aplicación `AzureWebJobs.<FUNCTION_NAME>.Disabled`.

![Conmutador Estado de la función](media/disable-function/function-state-switch.png)

## <a name="functions-2x---c-class-libraries"></a>Functions 2.x: bibliotecas de clases de C#

En una biblioteca de clases de Functions 2.x, se recomienda que utilice el método adecuado para todos los lenguajes. Pero si lo prefiere, puede [usar el atributo Disable como en Functions 1.x](#functions-1x---c-class-libraries).

## <a name="functions-1x---scripting-languages"></a>Functions 1.x: lenguajes de scripting

Para los lenguajes de scripting como script de C# y JavaScript, puede usar la propiedad `disabled` del archivo *function.json* para indicar al entorno en tiempo de ejecución que no desencadene una función. Esta propiedad puede establecerse en `true` o en el nombre de una configuración de aplicación:

```json
{
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ],
    "disabled": true
}
```
o 

```json
    "bindings": [
        ...
    ],
    "disabled": "IS_DISABLED"
```

En el segundo ejemplo, la función se deshabilita cuando hay una configuración de aplicación que se denomina IS_DISABLED y se establece en `true` o 1.

Puede editar el archivo en Azure Portal o utilizar el conmutador **Estado de la función** en la pestaña **Administrar** de la función. El modificador del portal funciona modificando el archivo *function.json*.

![Conmutador Estado de la función](media/disable-function/function-state-switch.png)

## <a name="functions-1x---c-class-libraries"></a>Functions 1.x: bibliotecas de clases de C#

En una biblioteca de clases de Functions 1.x, puede usar un atributo `Disable` para impedir que una función se desencadene. Puede usar el atributo sin un parámetro de constructor, tal como se muestra en el ejemplo siguiente:

```csharp
public static class QueueFunctions
{
    [Disable]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

El atributo sin un parámetro de constructor requiere que vuelva a compilar y a implementar el proyecto para cambiar el estado deshabilitado de la función. Una forma más flexible de usar el atributo consiste en incluir un parámetro de constructor que hace referencia a una configuración de aplicación booleana como se indica en el siguiente ejemplo:

```csharp
public static class QueueFunctions
{
    [Disable("MY_TIMER_DISABLED")]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Este método le permite habilitar y deshabilitar la función cambiando la configuración de la aplicación, sin necesidad de volver a compilar y a implementar. Cambiar una configuración de aplicación hace que la aplicación de función se reinicie, por lo que el estado deshabilitado se reconocerá inmediatamente.

> [!IMPORTANT]
> El atributo `Disabled` es la única manera de deshabilitar una función de biblioteca de clases. El archivo *function.json* que se generó para una función de biblioteca de clases no está diseñado para editarse directamente. Si edita ese archivo, haga lo que haga con la propiedad `disabled` no tendrá ningún efecto.
>
> Lo mismo sucede con el conmutador **Estado de la función** de la pestaña **Administrar**, ya que funciona cambiando el archivo *function.json*.
>
> También, tenga en cuenta que el portal puede indicar que la función está deshabilitada cuando realmente no lo está.

## <a name="next-steps"></a>Pasos siguientes

Este artículo trata acerca de cómo deshabilitar los desencadenadores automáticos. Para más información sobre desencadenadores, consulte [Desencadenadores y enlaces](functions-triggers-bindings.md).
