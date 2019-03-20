---
title: Enlaces de SendGrid de Azure Functions
description: Referencia de enlaces de SendGrid de Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/29/2017
ms.author: cshoe
ms.openlocfilehash: 663283c4dff16194dea78afbf5dd393f1fa833b1
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56823814"
---
# <a name="azure-functions-sendgrid-bindings"></a>Enlaces de SendGrid de Azure Functions

En este artículo se explica cómo enviar correos electrónicos mediante los enlaces de [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) en Azure Functions. Azure Functions admite un enlace de salida para SendGrid.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paquetes: Functions 1.x

Los enlaces de SendGrid se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid), versión 2.x. El código fuente del paquete está en el repositorio [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.SendGrid/) de GitHub.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

Los enlaces de SendGrid se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.SendGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid), versión 3.x. El código fuente del paquete está en el repositorio [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>Ejemplo

Vea el ejemplo específico del lenguaje:

* [C#](#c-example)
* [Script de C# (.csx)](#c-script-example)
* [JavaScript](#javascript-example)
* [Java](#java-example)

### <a name="c-example"></a>Ejemplo de C#

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que usa un desencadenador de cola de Service Bus y un enlace de salida de SendGrid.

#### <a name="synchronous-c-example"></a>Ejemplo de C# sincrónico:

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] Message email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
var emailObject = JsonConvert.DeserializeObject<OutgoingEmail>(Encoding.UTF8.GetString(email.Body));

message = new SendGridMessage();
message.AddTo(emailObject.To);
message.AddContent("text/html", emailObject.Body);
message.SetFrom(new EmailAddress(emailObject.From));
message.SetSubject(emailObject.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```
#### <a name="asynchronous-c-example"></a>Ejemplo de C# asincrónico:

```cs
[FunctionName("SendEmail")]
public static async void Run(
 [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] Message email,
 [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] IAsyncCollector<SendGridMessage> messageCollector)
{
 var emailObject = JsonConvert.DeserializeObject<OutgoingEmail>(Encoding.UTF8.GetString(email.Body));

 var message = new SendGridMessage();
 message.AddTo(emailObject.To);
 message.AddContent("text/html", emailObject.Body);
 message.SetFrom(new EmailAddress(emailObject.From));
 message.SetSubject(emailObject.Subject);
 
 await messageCollector.AddAsync(message);
}

public class OutgoingEmail
{
 public string To { get; set; }
 public string From { get; set; }
 public string Subject { get; set; }
 public string Body { get; set; }
}
```

Puede omitir la propiedad `ApiKey` del atributo si tiene la clave de API en una configuración de aplicación denominada "AzureWebJobsSendGridApiKey".

### <a name="c-script-example"></a>Ejemplo de script de C#

En el ejemplo siguiente se muestra un enlace de salida de SendGrid en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace.

Estos son los datos de enlace del archivo *function.json*:

```json 
{
    "bindings": [
        {
          "type": "queueTrigger",
          "name": "mymsg",
          "queueName": "myqueue",
          "connection": "AzureWebJobsStorage",
          "direction": "in"
        },
        {
          "type": "sendGrid",
          "name": "$return",
          "direction": "out",
          "apiKey": "SendGridAPIKeyAsAppSetting",
          "from": "{FromEmail}",
          "to": "{ToEmail}"
        }
    ]
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

Este es el código de script de C#:

```csharp
#r "SendGrid"

using System;
using SendGrid.Helpers.Mail;
using Microsoft.Azure.WebJobs.Host;

public static SendGridMessage Run(Message mymsg, ILogger log)
{
    SendGridMessage message = new SendGridMessage()
    {
        Subject = $"{mymsg.Subject}"
    };
    
    message.AddContent("text/plain", $"{mymsg.Content}");

    return message;
}
public class Message
{
    public string ToEmail { get; set; }
    public string FromEmail { get; set; }
    public string Subject { get; set; }
    public string Content { get; set; }
}
```

### <a name="java-example"></a>Ejemplo de Java

En el ejemplo siguiente se usa el `@SendGridOutput` anotación desde el [Java funciones de biblioteca en tiempo de ejecución](/java/api/overview/azure/functions/runtime) enlace de salida enviar un correo electrónico mediante SendGrid.

```java
@FunctionName("SendEmail")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request,
            @SendGridOutput(
                name = "email", dataType = "String", apiKey = "SendGridConnection", to = "test@example.com", from = "test@example.com",
                subject= "Sending with SendGrid", text = "Hello from Azure Functions"
                ) OutputBinding<String> email
            )
    {
        String name = request.getBody().orElse("World");

        final String emailBody = "{\"personalizations\":" +
                                    "[{\"to\":[{\"email\":\"test@example.com\"}]," +
                                    "\"subject\":\"Sending with SendGrid\"}]," +
                                    "\"from\":{\"email\":\"test@example.com\"}," +
                                    "\"content\":[{\"type\":\"text/plain\",\"value\": \"Hello" + name + "\"}]}";

        email.setValue(emailBody);
        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
    }
```

### <a name="javascript-example"></a>Ejemplo de JavaScript

En el ejemplo siguiente se muestra un enlace de salida de SendGrid en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace.

Estos son los datos de enlace del archivo *function.json*:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};
```

## <a name="attributes"></a>Atributos

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs).

Para información sobre las propiedades de atributo que puede configurar, consulte [Configuración](#configuration). A continuación, se muestra un ejemplo del atributo `SendGrid` en una signatura de método:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

Para obtener un ejemplo completo, consulte [Ejemplo de C#](#c-example).

## <a name="configuration"></a>Configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `SendGrid`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type**|| Requerida: se debe establecer en `sendGrid`.|
|**dirección**|| Requerida: se debe establecer en `out`.|
|**name**|| Requerida: nombre de variable que se usa en el código de la función para la solicitud o el cuerpo de la solicitud. Este valor es ```$return``` cuando hay solo un valor de devuelto. |
|**apiKey**|**ApiKey**| El nombre de una configuración de aplicación que contiene la clave de API. Si no está establecido, el nombre predeterminado de la configuración de aplicación es "AzureWebJobsSendGridApiKey".|
|**to**|**To**| Dirección de correo electrónico del destinatario. |
|**from**|**From**| Dirección de correo electrónico del remitente. |
|**subject**|**Asunto**| Asunto del correo electrónico. |
|**text**|**Texto**| Contenido del correo electrónico. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>configuración de host.json

En esta sección se describen las opciones de configuración globales disponibles para este enlace en la versión 2.x. El siguiente archivo host.json de ejemplo contiene solo la configuración de la versión 2.x para este enlace. Para obtener más información acerca de las opciones de configuración globales de la versión 2.x, consulte la [referencia de host.json para Azure Functions, versión 2.x](functions-host-json.md).

> [!NOTE]
> Para obtener una referencia de host.json en Functions 1.x, consulte la [referencia de host.json para Azure Functions, versión 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "sendGrid": {
            "from": "Azure Functions <samples@functions.com>"
        }
    }
}
```  

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|De|N/D|Dirección de correo electrónico del remitente en todas las funciones.| 


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)
