---
title: Enlaces de SignalR Service con Azure Functions
description: Uso de enlaces de SignalR Service con Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
editor: ''
tags: ''
keywords: azure functions, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: f0d4a607676285ed4f0f91d8ce8c83ddf1313b89
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306838"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Enlaces de SignalR Service para Azure Functions

En este artículo se explica cómo autenticar y enviar mensajes en tiempo real a los clientes conectados a [Azure SignalR Service](https://azure.microsoft.com/services/signalr-service/) mediante enlaces de SignalR Service en Azure Functions. Azure Functions admite enlaces de entrada y salida para SignalR Service.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

Los enlaces de SignalR Service se proporcionan en el [Microsoft.Azure.WebJobs.Extensions.SignalRService](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService) paquete de NuGet, versión 1.*. El código fuente del paquete está en el repositorio de GitHub [azure-functions-signalrservice-extension](https://github.com/Azure/azure-functions-signalrservice-extension).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]


### <a name="java-annotations"></a>Anotaciones de Java

Para utilizar las anotaciones de SignalR Service en las funciones de Java, deberá agregar una dependencia a la *azure funciones java biblioteca signalr* artefactos (versión 1.0 o superior) en pom.xml.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

> [!NOTE]
> Para utilizar los enlaces de SignalR Service en Java, asegúrese de que usa la versión 2.4.419 o posterior de Azure Functions Core Tools (versión de host 2.0.12332).

## <a name="using-signalr-service-with-azure-functions"></a>Usar SignalR Service con Azure Functions

Para obtener más información sobre cómo configurar y usar SignalR Service y Azure Functions juntos, consulte [configuración con Azure SignalR Service y desarrollo de Azure Functions](../azure-signalr/signalr-concept-serverless-development-config.md).

## <a name="signalr-connection-info-input-binding"></a>Enlace de entrada de información de conexión de SignalR

Para que un cliente pueda conectarse a Azure SignalR Service, antes debe recuperar la dirección URL del punto de conexión del servicio y un token de acceso válido. El enlace de entrada *SignalRConnectionInfo* genera la dirección URL del punto de conexión de SignalR Service y un token válido que se usan para conectarse al servicio. Dado que el token tiene una temporal y que se puede usar para autenticar un usuario concreto en una conexión, no debe almacenarlo en la caché ni compartirlo con los clientes. Los clientes pueden usar un desencadenador HTTP que use este enlace para recuperar la información de conexión.

Vea el ejemplo específico del lenguaje:

* [C# 2.x](#2x-c-input-examples)
* [JavaScript 2.x](#2x-javascript-input-examples)
* [2.x Java](#2x-java-input-examples)

Para obtener más información sobre cómo se utiliza este enlace para crear una función "negotiate" que puede consumir un SDK de cliente de SignalR, consulte el [artículo de desarrollo y la configuración de Azure Functions](../azure-signalr/signalr-concept-serverless-development-config.md) en los conceptos de SignalR Service documentación.

### <a name="2x-c-input-examples"></a>2.x C# ejemplos de entrada

En el siguiente ejemplo se muestra una [función de C#](functions-dotnet-class-library.md) que adquiere la información de la conexión de SignalR mediante el enlace de entrada y la devuelve a través de HTTP.

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req,
    [SignalRConnectionInfo(HubName = "chat")]SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Tokens autenticados

Si la función la desencadena un cliente autenticado, puede agregar una notificación del identificador de usuario al token generado. Puede agregar fácilmente la autenticación a una aplicación de función mediante [autenticación de App Service](../app-service/overview-authentication-authorization.md).

Autenticación de App Service establece encabezados HTTP denominados `x-ms-client-principal-id` y `x-ms-client-principal-name` que contienen el identificador y el nombre de la entidad de seguridad de cliente del usuario autenticado, respectivamente. Puede establecer la propiedad `UserId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`. 

```cs
[FunctionName("negotiate")]
public static SignalRConnectionInfo Negotiate(
    [HttpTrigger(AuthorizationLevel.Anonymous)]HttpRequest req, 
    [SignalRConnectionInfo
        (HubName = "chat", UserId = "{headers.x-ms-client-principal-id}")]
        SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier claim set to the authenticated user
    return connectionInfo;
}
```

### <a name="2x-javascript-input-examples"></a>ejemplos de entrada de JavaScript de 2.x

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
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

#### <a name="authenticated-tokens"></a>Tokens autenticados

Si la función la desencadena un cliente autenticado, puede agregar una notificación del identificador de usuario al token generado. Puede agregar fácilmente la autenticación a una aplicación de función mediante [autenticación de App Service](../app-service/overview-authentication-authorization.md).

Autenticación de App Service establece encabezados HTTP denominados `x-ms-client-principal-id` y `x-ms-client-principal-name` que contienen el identificador y el nombre de la entidad de seguridad de cliente del usuario autenticado, respectivamente. Puede establecer la propiedad `userId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`. 

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
module.exports = async function (context, req, connectionInfo) {
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    context.res.body = connectionInfo;
};
```

### <a name="2x-java-input-examples"></a>ejemplos de entrada de Java de 2.x

El ejemplo siguiente se muestra un [función Java](functions-reference-java.md) que adquiere la información de conexión de SignalR con el enlace de entrada y lo devuelve a través de HTTP.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

#### <a name="authenticated-tokens"></a>Tokens autenticados

Si la función la desencadena un cliente autenticado, puede agregar una notificación del identificador de usuario al token generado. Puede agregar fácilmente la autenticación a una aplicación de función mediante [autenticación de App Service](../app-service/overview-authentication-authorization.md).

Autenticación de App Service establece encabezados HTTP denominados `x-ms-client-principal-id` y `x-ms-client-principal-name` que contienen el identificador y el nombre de la entidad de seguridad de cliente del usuario autenticado, respectivamente. Puede establecer la propiedad `UserId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`.

```java
@FunctionName("negotiate")
public SignalRConnectionInfo negotiate(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> req,
        @SignalRConnectionInfoInput(
            name = "connectionInfo",
            hubName = "chat",
            userId = "{headers.x-ms-client-principal-id}") SignalRConnectionInfo connectionInfo) {
    return connectionInfo;
}
```

## <a name="signalr-output-binding"></a>Enlace de salida de SignalR

Use el enlace de la salida de *SignalR* para enviar uno o varios mensajes mediante Azure SignalR Service. Puede difundir un mensaje a todos los clientes conectados, o bien puede difundirlo solo a los clientes conectados que se han autenticado en un usuario dado.

También puede usarlo para administrar los grupos a los que pertenece un usuario.

Vea el ejemplo específico del lenguaje:

* [C# 2.x](#2x-c-send-message-output-examples)
* [JavaScript 2.x](#2x-javascript-send-message-output-examples)
* [2.x Java](#2x-java-send-message-output-examples)

### <a name="2x-c-send-message-output-examples"></a>2.x C# enviar ejemplos de mensajes de salida

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
            // the message will only be sent to this user ID
            UserId = "userId1",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

#### <a name="send-to-a-group"></a>Enviar a un grupo

Puede enviar un mensaje sólo a las conexiones que se han agregado a un grupo mediante el establecimiento del `GroupName` propiedad del mensaje de SignalR.

```cs
[FunctionName("SendMessage")]
public static Task SendMessage(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]object message,
    [SignalR(HubName = "chat")]IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage
        {
            // the message will be sent to the group with this name
            GroupName = "myGroup",
            Target = "newMessage",
            Arguments = new [] { message }
        });
}
```

### <a name="2x-c-group-management-output-examples"></a>2.x C# ejemplos del resultado de administración de grupos

SignalR Service permite a los usuarios pueden agregarse a grupos. A continuación, se pueden enviar mensajes a un grupo. Puede usar el `SignalRGroupAction` clase con el `SignalR` enlace para administrar la pertenencia a grupos de un usuario de salida.

#### <a name="add-user-to-a-group"></a>Agregar usuario a un grupo

El ejemplo siguiente agrega un usuario a un grupo.

```csharp
[FunctionName("addToGroup")]
public static Task AddToGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    string userId,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userId,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Quitar usuario de un grupo

El ejemplo siguiente quita un usuario de un grupo.

```csharp
[FunctionName("removeFromGroup")]
public static Task RemoveFromGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    string userId,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userId,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

### <a name="2x-javascript-send-message-output-examples"></a>ejemplos del resultado de mensaje de envío de JavaScript de 2.x

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
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

#### <a name="send-to-a-user"></a>Envío a un usuario

Solo se pueden enviar mensajes a las conexiones que se hayan autenticado en un usuario estableciendo la propiedad `userId` del mensaje de SignalR.

*function.json* no cambia. Este es el código de JavaScript:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this user ID
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

#### <a name="send-to-a-group"></a>Enviar a un grupo

Puede enviar un mensaje sólo a las conexiones que se han agregado a un grupo mediante el establecimiento del `groupName` propiedad del mensaje de SignalR.

*function.json* no cambia. Este es el código de JavaScript:

```javascript
module.exports = async function (context, req) {
    context.bindings.signalRMessages = [{
        // message will only be sent to this group
        "groupName": "myGroup",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

### <a name="2x-javascript-group-management-output-examples"></a>ejemplos del resultado de 2.x de administración de grupos de JavaScript

SignalR Service permite a los usuarios pueden agregarse a grupos. A continuación, se pueden enviar mensajes a un grupo. Puede usar el `SignalR` enlace para administrar la pertenencia a grupos de un usuario de salida.

#### <a name="add-user-to-a-group"></a>Agregar usuario a un grupo

El ejemplo siguiente agrega un usuario a un grupo.

*function.json*

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "signalR",
      "name": "signalRGroupActions",
      "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
      "hubName": "chat",
      "direction": "out"
    }
  ]
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "add"
  }];
};
```

#### <a name="remove-user-from-a-group"></a>Quitar usuario de un grupo

El ejemplo siguiente quita un usuario de un grupo.

*function.json*

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "signalR",
      "name": "signalRGroupActions",
      "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
      "hubName": "chat",
      "direction": "out"
    }
  ]
}
```

*index.js*

```javascript
module.exports = async function (context, req) {
  context.bindings.signalRGroupActions = [{
    "userId": req.query.userId,
    "groupName": "myGroup",
    "action": "remove"
  }];
};
```

### <a name="2x-java-send-message-output-examples"></a>ejemplos del resultado de mensaje de envío de Java de 2.x

#### <a name="broadcast-to-all-clients"></a>Difusión a todos los clientes

El ejemplo siguiente se muestra un [función Java](functions-reference-java.md) que envía un mensaje utilizando el enlace de salida para todos los clientes conectados. `target` es el nombre del método que se va a invocar en cada cliente. La propiedad `arguments` es una matriz de cero o más objetos que se pasarán al método del cliente.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

#### <a name="send-to-a-user"></a>Envío a un usuario

Solo se pueden enviar mensajes a las conexiones que se hayan autenticado en un usuario estableciendo la propiedad `userId` del mensaje de SignalR.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.userId = "userId1";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

#### <a name="send-to-a-group"></a>Enviar a un grupo

Puede enviar un mensaje sólo a las conexiones que se han agregado a un grupo mediante el establecimiento del `groupName` propiedad del mensaje de SignalR.

```java
@FunctionName("sendMessage")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRMessage sendMessage(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req) {

    SignalRMessage message = new SignalRMessage();
    message.groupName = "myGroup";
    message.target = "newMessage";
    message.arguments.add(req.getBody());
    return message;
}
```

### <a name="2x-java-group-management-output-examples"></a>ejemplos del resultado de 2.x de administración de grupos de Java

SignalR Service permite a los usuarios pueden agregarse a grupos. A continuación, se pueden enviar mensajes a un grupo. Puede usar el `SignalRGroupAction` clase con el `SignalROutput` enlace para administrar la pertenencia a grupos de un usuario de salida.

#### <a name="add-user-to-a-group"></a>Agregar usuario a un grupo

El ejemplo siguiente agrega un usuario a un grupo.

```java
@FunctionName("addToGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction addToGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "add";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
```

#### <a name="remove-user-from-a-group"></a>Quitar usuario de un grupo

El ejemplo siguiente quita un usuario de un grupo.

```java
@FunctionName("removeFromGroup")
@SignalROutput(name = "$return", hubName = "chat")
public SignalRGroupAction removeFromGroup(
        @HttpTrigger(
            name = "req",
            methods = { HttpMethod.POST },
            authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Object> req,
        @BindingName("userId") String userId) {

    SignalRGroupAction groupAction = new SignalRGroupAction();
    groupAction.action = "remove";
    groupAction.userId = userId;
    groupAction.groupName = "myGroup";
    return action;
}
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
|**userId**|**UserId**| Opcional: Opcional: valor de la notificación del identificador de usuario que se va a establecer en el token de la clave de acceso. |
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

> [!div class="nextstepaction"]
> [Configuración con Azure SignalR Service y desarrollo de las funciones de Azure](../azure-signalr/signalr-concept-serverless-development-config.md)
