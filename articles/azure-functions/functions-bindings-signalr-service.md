---
title: Enlaces de SignalR Service con Azure Functions
description: Uso de enlaces de SignalR Service con Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: a1de59ebb5ef0d7f5522a388aa9a2f5818495a9f
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74786340"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Enlaces de SignalR Service para Azure Functions

En este artículo se explica cómo autenticar y enviar mensajes en tiempo real a los clientes conectados a [Azure SignalR Service](https://azure.microsoft.com/services/signalr-service/) mediante enlaces de SignalR Service en Azure Functions. Azure Functions admite enlaces de entrada y salida para SignalR Service.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

Los enlaces de SignalR Service se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.SignalRService](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService), versión 1.*. El código fuente del paquete está en el repositorio de GitHub [azure-functions-signalrservice-extension](https://github.com/Azure/azure-functions-signalrservice-extension).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2-manual-portal.md)]

Para más información sobre cómo configurar y usar SignalR Service y Azure Functions juntos, consulte [Desarrollo y configuración de Azure Functions con Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md).

### <a name="annotations-library-java-only"></a>Biblioteca de anotaciones (solo Java)

Para utilizar las anotaciones de SignalR Service en las funciones de Java, deberá agregar una dependencia al artefacto *azure-functions-java-library-signalr* (versión 1.0 o superior) en el archivo pom.xml.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="input"></a>Entrada

Para que un cliente pueda conectarse a Azure SignalR Service, antes debe recuperar la dirección URL del punto de conexión del servicio y un token de acceso válido. El enlace de entrada *SignalRConnectionInfo* genera la dirección URL del punto de conexión de SignalR Service y un token válido que se usan para conectarse al servicio. Dado que el token tiene una temporal y que se puede usar para autenticar un usuario concreto en una conexión, no debe almacenarlo en la caché ni compartirlo con los clientes. Los clientes pueden usar un desencadenador HTTP que use este enlace para recuperar la información de conexión.

Para más información sobre cómo se utiliza este enlace para crear una función "negotiate" que pueda consumir un SDK de cliente de SignalR, consulte el [artículo Desarrollo y configuración de Azure Functions](../azure-signalr/signalr-concept-serverless-development-config.md) en la documentación sobre conceptos de SignalR Service.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

En el ejemplo siguiente se muestra un enlace de entrada de la información de la conexión de SignalR en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace para devolver la información de conexión.

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

Este es el código de script de C#:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

# <a name="pythontabpython"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra un enlace de entrada de la información de la conexión de SignalR en un archivo *function.json* y una [función de Python](functions-reference-python.md) que usa el enlace para devolver la información de conexión.

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

Este es el código de Python:

```python
def main(req: func.HttpRequest, connectionInfoJson: str) -> func.HttpResponse:
    return func.HttpResponse(
        connectionInfoJson,
        status_code=200,
        headers={
            'Content-type': 'application/json'
        }
    )
```

# <a name="javatabjava"></a>[Java](#tab/java)

En el siguiente ejemplo se muestra una [función de Java](functions-reference-java.md) que adquiere la información de la conexión de SignalR mediante el enlace de entrada y la devuelve a través de HTTP.

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

---

### <a name="authenticated-tokens"></a>Tokens autenticados

Si la función la desencadena un cliente autenticado, puede agregar una notificación del identificador de usuario al token generado. Puede agregar fácilmente la autenticación a una aplicación de función mediante [Autenticación de App Service](../app-service/overview-authentication-authorization.md).

Autenticación de App Service establece encabezados HTTP denominados `x-ms-client-principal-id` y `x-ms-client-principal-name` que contienen el identificador y el nombre de la entidad de seguridad de cliente del usuario autenticado, respectivamente.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Puede establecer la propiedad `UserId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`.

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

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

Puede establecer la propiedad `userId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`.

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

Este es el código de script de C#:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static SignalRConnectionInfo Run(HttpRequest req, SignalRConnectionInfo connectionInfo)
{
    // connectionInfo contains an access key token with a name identifier
    // claim set to the authenticated user
    return connectionInfo;
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Puede establecer la propiedad `userId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`.

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

# <a name="pythontabpython"></a>[Python](#tab/python)

Puede establecer la propiedad `userId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`.

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

Este es el código de Python:

```python
def main(req: func.HttpRequest, connectionInfoJson: str) -> func.HttpResponse:
    # connectionInfo contains an access key token with a name identifier
    # claim set to the authenticated user
    return func.HttpResponse(
        connectionInfoJson,
        status_code=200,
        headers={
            'Content-type': 'application/json'
        }
    )
```

# <a name="javatabjava"></a>[Java](#tab/java)

Puede establecer la propiedad `userId` del enlace en el valor de cualquier encabezado mediante una [expresión de enlace](./functions-bindings-expressions-patterns.md): `{headers.x-ms-client-principal-id}` o `{headers.x-ms-client-principal-name}`.

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

---

## <a name="output"></a>Output

Use el enlace de la salida de *SignalR* para enviar uno o varios mensajes mediante Azure SignalR Service. Puede difundir un mensaje a todos los clientes conectados, o bien puede difundirlo solo a los clientes conectados que se han autenticado en un usuario dado.

También puede usarlo para administrar los grupos a los que pertenece un usuario.

### <a name="broadcast-to-all-clients"></a>Difusión a todos los clientes

En el siguiente ejemplo se muestra una función que envía un mensaje mediante el enlace de salida a todos los clientes conectados. *Target* es el nombre del método que se va a invocar en cada cliente. *Arguments* es una matriz de cero o más objetos que se pasarán al método del cliente.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

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

Este es el código de script de C#:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
{
    return signalRMessages.AddAsync(
        new SignalRMessage 
        {
            Target = "newMessage", 
            Arguments = new [] { message } 
        });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

# <a name="pythontabpython"></a>[Python](#tab/python)

Estos son los datos de enlace del archivo *function.json*:

Function.json de ejemplo:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Este es el código de Python:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

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

---

### <a name="send-to-a-user"></a>Envío a un usuario

Solo se pueden enviar mensajes a las conexiones que se hayan autenticado en un usuario mediante el establecimiento de *UserID* en el mensaje de SignalR.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

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

Este es el código de script de C#:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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
        // message will only be sent to this user ID
        "userId": "userId1",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Estos son los datos de enlace del archivo *function.json*:

Function.json de ejemplo:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Este es el código de Python:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        #message will only be sent to this user ID
        'userId': 'userId1',
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

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

---

### <a name="send-to-a-group"></a>Envío a un grupo

Solo se pueden enviar mensajes a las conexiones que se hayan agregado a un grupo mediante el establecimiento de *GroupName* en el mensaje de SignalR.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

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

Este es el código de script de C#:

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    object message,
    IAsyncCollector<SignalRMessage> signalRMessages)
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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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
        // message will only be sent to this group
        "groupName": "myGroup",
        "target": "newMessage",
        "arguments": [ req.body ]
    }];
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Estos son los datos de enlace del archivo *function.json*:

Function.json de ejemplo:

```json
{
  "type": "signalR",
  "name": "out_message",
  "hubName": "<hub_name>",
  "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
  "direction": "out"
}
```

Este es el código de Python:

```python
def main(req: func.HttpRequest, out_message: func.Out[str]) -> func.HttpResponse:
    message = req.get_json()
    out_message.set(json.dumps({
        #message will only be sent to this group
        'groupName': 'myGroup',
        'target': 'newMessage',
        'arguments': [ message ]
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

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

---

### <a name="group-management"></a>Administración de grupos

SignalR Service permite que se puedan agregar usuarios a grupos. A continuación, se pueden enviar mensajes a un grupo. Puede usar el enlace de salida `SignalR` para administrar la pertenencia a un grupo de usuarios.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

#### <a name="add-user-to-a-group"></a>Adición de un usuario a un grupo

En el ejemplo siguiente se agrega un usuario a un grupo.

```csharp
[FunctionName("addToGroup")]
public static Task AddToGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Eliminación de un usuario de un grupo

En el ejemplo siguiente se quita a un usuario de un grupo.

```csharp
[FunctionName("removeFromGroup")]
public static Task RemoveFromGroup(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    [SignalR(HubName = "chat")]
        IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

> [!NOTE]
> Para obtener `ClaimsPrincipal` correctamente enlazado, debe haber definido la configuración de autenticación en Azure Functions.

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

#### <a name="add-user-to-a-group"></a>Adición de un usuario a un grupo

En el ejemplo siguiente se agrega un usuario a un grupo.

*function.json* de ejemplo

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*Run.csx*

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Add
        });
}
```

#### <a name="remove-user-from-a-group"></a>Eliminación de un usuario de un grupo

En el ejemplo siguiente se quita a un usuario de un grupo.

*function.json* de ejemplo

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*Run.csx*

```cs
#r "Microsoft.Azure.WebJobs.Extensions.SignalRService"
using Microsoft.Azure.WebJobs.Extensions.SignalRService;

public static Task Run(
    HttpRequest req,
    ClaimsPrincipal claimsPrincipal,
    IAsyncCollector<SignalRGroupAction> signalRGroupActions)
{
    var userIdClaim = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier);
    return signalRGroupActions.AddAsync(
        new SignalRGroupAction
        {
            UserId = userIdClaim.Value,
            GroupName = "myGroup",
            Action = GroupAction.Remove
        });
}
```

> [!NOTE]
> Para obtener `ClaimsPrincipal` correctamente enlazado, debe haber definido la configuración de autenticación en Azure Functions.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

#### <a name="add-user-to-a-group"></a>Adición de un usuario a un grupo

En el ejemplo siguiente se agrega un usuario a un grupo.

*function.json* de ejemplo

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
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

#### <a name="remove-user-from-a-group"></a>Eliminación de un usuario de un grupo

En el ejemplo siguiente se quita a un usuario de un grupo.

*function.json* de ejemplo

```json
{
    "type": "signalR",
    "name": "signalRGroupActions",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
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

# <a name="pythontabpython"></a>[Python](#tab/python)

#### <a name="add-user-to-a-group"></a>Adición de un usuario a un grupo

En el ejemplo siguiente se agrega un usuario a un grupo.

*function.json* de ejemplo

```json
{
    "type": "signalR",
    "name": "action",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*\_\_init.py__*

```python
def main(req: func.HttpRequest, action: func.Out[str]) -> func.HttpResponse:
    action.set(json.dumps({
        'userId': 'userId1',
        'groupName': 'myGroup',
        'action': 'add'
    }))
```

#### <a name="remove-user-from-a-group"></a>Eliminación de un usuario de un grupo

En el ejemplo siguiente se quita a un usuario de un grupo.

*function.json* de ejemplo

```json
{
    "type": "signalR",
    "name": "action",
    "connectionStringSetting": "<name of setting containing SignalR Service connection string>",
    "hubName": "chat",
    "direction": "out"
}
```

*\_\_init.py__*

```python
def main(req: func.HttpRequest, action: func.Out[str]) -> func.HttpResponse:
    action.set(json.dumps({
        'userId': 'userId1',
        'groupName': 'myGroup',
        'action': 'remove'
    }))
```

# <a name="javatabjava"></a>[Java](#tab/java)

#### <a name="add-user-to-a-group"></a>Adición de un usuario a un grupo

En el ejemplo siguiente se agrega un usuario a un grupo.

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

#### <a name="remove-user-from-a-group"></a>Eliminación de un usuario de un grupo

En el ejemplo siguiente se quita a un usuario de un grupo.

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

---

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
> [Desarrollo y configuración de Azure Functions con Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md)
