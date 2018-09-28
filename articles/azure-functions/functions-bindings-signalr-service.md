---
title: Enlaces de SignalR Service con Azure Functions
description: Uso de enlaces de SignalR Service con Azure Functions.
services: functions
documentationcenter: na
author: anthonychu
manager: cfowler
editor: ''
tags: ''
keywords: azure functions, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/23/2018
ms.author: antchu
ms.openlocfilehash: 435a581b065b94399006e60fec8d007296734373
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993959"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Enlaces de SignalR Service para Azure Functions

En este artículo se explica cómo autenticar y enviar mensajes en tiempo real a los clientes conectados a [Azure SignalR Service](https://azure.microsoft.com/services/signalr-service/) mediante enlaces de SignalR Service en Azure Functions. Azure Functions admite enlaces de entrada y salida para SignalR Service.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

Los enlaces de SignalR Service se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.SignalRService](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService), versión 1.0.0-preview1-*. El código fuente del paquete está en el repositorio de GitHub [azure-functions-signalrservice-extension](https://github.com/Azure/azure-functions-signalrservice-extension).

> [!NOTE]
> Azure SignalR Service está disponible con carácter general. Sin embargo, los enlaces de SignalR Service para Azure Functions están actualmente en versión preliminar.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]


## <a name="signalr-connection-info-input-binding"></a>Enlace de entrada de información de conexión de SignalR

Para que un cliente pueda conectarse a Azure SignalR Service, antes debe recuperar la dirección URL del punto de conexión del servicio y un token de acceso válido. El enlace de entrada *SignalRConnectionInfo* genera la dirección URL del punto de conexión de SignalR Service y un token válido que se usan para conectarse al servicio. Dado que el token tiene una temporal y que se puede usar para autenticar un usuario concreto en una conexión, no debe almacenarlo en la caché ni compartirlo con los clientes. Los clientes pueden usar un desencadenador HTTP que use este enlace para recuperar la información de conexión.

Vea el ejemplo específico del lenguaje:

* [C# 2.x](#2x-c-input-example)
* [JavaScript 2.x](#2x-javascript-input-example)

### <a name="2x-c-input-example"></a>Ejemplo de entrada de C# 2.x

En el siguiente ejemplo se muestra una [función de C#](functions-dotnet-class-library.md) que adquiere la información de la conexión de SignalR mediante el enlace de entrada y la devuelve a través de HTTP.

```cs
[FunctionName("GetSignalRInfo")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Tokens autenticados

Si la función la desencadena un cliente autenticado, puede agregar una notificación del identificador de usuario al token generado. Puede agregar fácilmente la autenticación a una aplicación de función mediante [Autenticación de App Service] (../app-service/app-service-authentication-overview.md).

Autenticación de App Service establece encabezados HTTP denominados `x-ms-client-principal-id` y `x-ms-client-principal-name` que contienen el identificador y el nombre de la entidad de seguridad de cliente del usuario autenticado, respectivamente. Puede establecer la propiedad `UserId` del enlace en el valor de cualquiera de los encabezados mediante una [expresión de enlace](functions-triggers-bindings.md#binding-expressions-and-patterns): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`. 

```cs
[FunctionName("GetSignalRInfo")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

### <a name="2x-javascript-input-example"></a>Ejemplo de entrada de JavaScript 2.x

En el ejemplo siguiente se muestra un enlace de entrada de la información de la conexión de SignalR en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace para devolver la información de la conexión.

Estos son los datos de enlace del archivo *function.json*:

Function.json de ejemplo:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Este es el código de JavaScript:

```javascript
module.exports = function (context, req, connectionInfo) {
    context.res = { body: connectionInfo };
    context.done();
};
```

#### <a name="authenticated-tokens"></a>Tokens autenticados

Si la función la desencadena un cliente autenticado, puede agregar una notificación del identificador de usuario al token generado. Puede agregar fácilmente la autenticación a una aplicación de función mediante [Autenticación de App Service] (../app-service/app-service-authentication-overview.md).

Autenticación de App Service establece encabezados HTTP denominados `x-ms-client-principal-id` y `x-ms-client-principal-name` que contienen el identificador y el nombre de la entidad de seguridad de cliente del usuario autenticado, respectivamente. Puede establecer la propiedad `userId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](functions-triggers-bindings.md#binding-expressions-and-patterns): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`. 

Function.json de ejemplo:

```json
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "chat",
    "userId": "{headers.x-ms-client-principal-id}",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "direction": "in"
}
```

Este es el código de JavaScript:

```javascript
module.exports = function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier 
    // claim set to the authenticated user
    context.res = { body: connectionInfo };
    context.done();
};
```

## <a name="signalr-output-binding"></a>Enlace de salida de SignalR

Use el enlace de la salida de *SignalR* para enviar uno o varios mensajes mediante Azure SignalR Service. Puede difundir un mensaje a todos los clientes conectados, o bien puede difundirlo solo a los clientes conectados que se han autenticado en un usuario dado.

Vea el ejemplo específico del lenguaje:

* [C# 2.x](#2x-c-output-example)
* [JavaScript 2.x](#2x-javascript-output-example)

### <a name="2x-c-output-example"></a>Ejemplo de salida de C# 2.x

#### <a name="broadcast-to-all-clients"></a>Difusión a todos los clientes

En el siguiente ejemplo se muestra una [función de C#](functions-dotnet-class-library.md) que envía un mensaje mediante el enlace de salida a todos los clientes conectados. `Target` es el nombre del método que se va a invocar en cada cliente. La propiedad `Arguments` es una matriz de cero o más objetos que se pasarán al método del cliente.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

#### <a name="send-to-a-user"></a>Envío a un usuario

Solo se pueden enviar mensajes a las conexiones que se hayan autenticado en un usuario estableciendo la propiedad `UserId` del mensaje de SignalR.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message, 
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            // the message will only be sent to these user IDs
            UserId = "userId1",
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

### <a name="2x-javascript-output-example"></a>Ejemplo de salida de JavaScript 2.x

#### <a name="broadcast-to-all-clients"></a>Difusión a todos los clientes

En el ejemplo siguiente se muestra un enlace de salida de SignalR en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace para enviar in mensaje con Azure SignalR Service. Establezca el enlace de salida en una matriz de uno o varios mensajes de SignalR. Un mensaje de SignalR consta de una propiedad `target` que especifica el nombre del método que se invoca en cada cliente y una propiedad `arguments` que es una matriz de objetos que se usan en el método del cliente como argumentos.

Estos son los datos de enlace del archivo *function.json*:

Function.json de ejemplo:

```json
{
  "type": "signalR",
  "name": "signalRMessages",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Este es el código de JavaScript:

```javascript
module.exports = function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
    context.done();
};
```

#### <a name="send-to-a-user"></a>Envío a un usuario

Solo se pueden enviar mensajes a las conexiones que se hayan autenticado en un usuario estableciendo la propiedad `userId` del mensaje de SignalR.

*function.json* no cambia. Este es el código de JavaScript:

```javascript
module.exports = function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to these user IDs
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
    context.done();
};
```

## <a name="configuration"></a>Configuración

### <a name="signalrconnectioninfo"></a>SignalRConnectionInfo

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `SignalRConnectionInfo`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type**|| Se debe establecer en `signalRConnectionInfo`.|
|**dirección**|| Se debe establecer en `in`.|
|**name**|| Nombre de la variable que se utiliza en el código de función para el objeto de información de conexión. |
|**hubName**|**HubName**| Este valor se debe establecer en el nombre del concentrador SignalR para el que se genera la información de conexión.|
|**userId**|**UserId**| Opcional: el valor de la notificación del identificador de usuario que se establece en el token de la clave de acceso. |
|**connectionStringSetting**|**ConnectionStringSetting**| El nombre de la configuración de la aplicación que contiene la cadena de conexión de SignalR Service (el valor predeterminado es "AzureSignalRConnectionString") |

### <a name="signalr"></a>SignalR

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `SignalR`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type**|| Se debe establecer en `signalR`.|
|**dirección**|| Se debe establecer en `out`.|
|**name**|| Nombre de la variable que se utiliza en el código de función para el objeto de información de conexión. |
|**hubName**|**HubName**| Este valor se debe establecer en el nombre del concentrador SignalR para el que se genera la información de conexión.|
|**connectionStringSetting**|**ConnectionStringSetting**| El nombre de la configuración de la aplicación que contiene la cadena de conexión de SignalR Service (el valor predeterminado es "AzureSignalRConnectionString") |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)

