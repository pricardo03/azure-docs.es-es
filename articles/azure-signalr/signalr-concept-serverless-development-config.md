---
title: Desarrollar y configurar aplicaciones de funciones de Azure SignalR Service
description: Obtener más información sobre cómo desarrollar y configurar las aplicaciones sin servidor en tiempo real con Azure Functions y Azure SignalR Service
author: anthonychu
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: antchu
ms.openlocfilehash: 9b68b9d0bbac984c29759cf4b7b026a559a9d819
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60809006"
---
# <a name="azure-functions-development-and-configuration-with-azure-signalr-service"></a>Configuración con Azure SignalR Service y desarrollo de las funciones de Azure

Aplicaciones de Azure Functions pueden aprovechar la [enlaces de Azure SignalR Service](../azure-functions/functions-bindings-signalr-service.md) para agregar funciones en tiempo real. Las aplicaciones cliente utilizar el SDK de cliente disponibles en varios lenguajes para conectarse a Azure SignalR Service y recibir mensajes en tiempo real.

En este artículo se describe los conceptos para desarrollar y configurar una aplicación de función de Azure que se integra con SignalR Service.

## <a name="signalr-service-configuration"></a>Configuración de SignalR Service

Azure SignalR Service puede configurarse en distintos modos. Cuando se usa con Azure Functions, el servicio debe configurarse en *Serverless* modo.

En Azure portal, busque el *configuración* página del recurso de SignalR Service. Establecer el *modo de servicio* a *Serverless*.

![Modo SignalR Service](media/signalr-concept-azure-functions/signalr-service-mode.png)

## <a name="azure-functions-development"></a>Desarrollo de Azure Functions

Una aplicación en tiempo real sin servidor creada con Azure Functions y Azure SignalR Service normalmente requiere dos funciones de Azure:

* Una función "negotiate" que el cliente llama para obtener un válido de SignalR Service token de acceso y dirección URL del extremo de servicio
* Una o varias funciones que envían mensajes o administración la pertenencia a grupo

### <a name="negotiate-function"></a>Negotiate (función)

Una aplicación cliente requiere un token de acceso válida para conectarse a Azure SignalR Service. Puede ser un token de acceso anónimo o autenticado a un identificador de usuario determinado. Aplicaciones sin servidor de SignalR Service requieren que un extremo HTTP denominado "negotiate" para obtener un token y otra información de conexión, como la dirección URL de punto de conexión de SignalR Service.

Use HTTP desencadena la función de Azure y la *SignalRConnectionInfo* enlace para generar el objeto de información de conexión de entrada. La función debe tener una ruta HTTP que termina en `/negotiate`.

Para obtener más información sobre cómo crear la función negotiate, consulte el [ *SignalRConnectionInfo* referencia de enlace de entrada](../azure-functions/functions-bindings-signalr-service.md#signalr-connection-info-input-binding).

Para obtener información sobre cómo crear un token autenticado, consulte [utilizando la autenticación de App Service](#using-app-service-authentication).

### <a name="sending-messages-and-managing-group-membership"></a>Envío de mensajes y administrar la pertenencia a grupos

Use la *SignalR* enlace de salida para enviar mensajes a los clientes conectados a Azure SignalR Service. Puede difundir mensajes a todos los clientes o enviarlos a un subconjunto de los clientes se autentican con un identificador de usuario específico o que se han agregado a un grupo específico.

Se pueden agregar usuarios a uno o más grupos. También puede usar el *SignalR* enlace para agregar o quitar usuarios hacia y desde grupos de salida.

Para obtener más información, consulte el [ *SignalR* la referencia de enlace de salida](../azure-functions/functions-bindings-signalr-service.md#signalr-output-binding).

### <a name="signalr-hubs"></a>Concentradores de SignalR

SignalR tiene un concepto de "concentradores". Cada conexión de cliente y cada mensaje enviado desde Azure Functions se limita a un concentrador concreto. Puede usar concentradores como una manera de separar las conexiones y los mensajes en los espacios de nombres lógico.

## <a name="client-development"></a>Desarrollo de cliente

Las aplicaciones cliente de SignalR pueden aprovechar el SDK en uno de varios lenguajes para conectarse y recibir mensajes de Azure SignalR Service fácilmente de cliente de SignalR.

### <a name="configuring-a-client-connection"></a>Configurar una conexión de cliente

Para conectarse a SignalR Service, un cliente debe realizar una negociación correcta de conexión que consta de estos pasos:

1. Realice una solicitud a la *negociar* extremo HTTP mencionados anteriormente para obtener información de conexión válida
1. Conectarse a SignalR Service creados con la dirección URL de punto de conexión de servicio y el token de acceso obtenido a partir del *negociar* punto de conexión

SDK de cliente de SignalR ya contienen la lógica necesaria para realizar el enlace de negociación. Pasar la dirección URL del punto de conexión del negotiate, menos el `negotiate` segmento, el SDK `HubConnectionBuilder`. Este es un ejemplo en JavaScript:

```javascript
const connection = new signalR.HubConnectionBuilder()
  .withUrl('https://my-signalr-function-app.azurewebsites.net/api')
  .build()
```

Por convención, el SDK se anexa automáticamente `/negotiate` a la dirección URL y lo usa para comenzar la negociación.

> [!NOTE]
> Si usa el SDK de JavaScript/TypeScript en un explorador, necesitará [habilitar recursos entre orígenes (CORS) de uso compartido](#enabling-cors) en la aplicación de función.

Para obtener más información sobre cómo usar el SDK de cliente de SignalR, consulte la documentación para su idioma:

* [.NET Standard](https://docs.microsoft.com/aspnet/core/signalr/dotnet-client)
* [JavaScript](https://docs.microsoft.com/aspnet/core/signalr/javascript-client)
* [Java](https://docs.microsoft.com/aspnet/core/signalr/java-client)

### <a name="sending-messages-from-a-client-to-the-service"></a>Envío de mensajes desde un cliente al servicio

Aunque el SDK de SignalR permite a aplicaciones cliente invocar la lógica de back-end en un concentrador SignalR, esta funcionalidad no se admite aún cuando se usa SignalR Service con Azure Functions. Utilice HTTP solicitudes para invocar funciones de Azure.

## <a name="azure-functions-configuration"></a>Configuración de Azure Functions

Azure Function App que se integran con Azure SignalR Service puede implementarse como cualquier aplicación de función de Azure típico, mediante técnicas como [continuamente implementación](../azure-functions/functions-continuous-deployment.md), [zip implementación](../azure-functions/deployment-zip-push.md)y [ejecutar desde el paquete](../azure-functions/run-functions-from-deployment-package.md).

Sin embargo, hay algunas consideraciones especiales para las aplicaciones que usan los enlaces de SignalR Service. Si el cliente se ejecuta en un explorador, debe habilitarse la CORS. Y si la aplicación requiere autenticación, puede integrar el punto de conexión de negociar con autenticación de App Service.

### <a name="enabling-cors"></a>Habilitación de CORS

El cliente de JavaScript/TypeScript realiza las solicitudes HTTP a la función negotiate para iniciar la negociación de conexión. Cuando la aplicación cliente se hospeda en un dominio diferente de la aplicación de función de Azure, de recursos entre orígenes (CORS) de uso compartido deben estar habilitado en la aplicación de función o el explorador bloqueará las solicitudes.

#### <a name="localhost"></a>localhost

Cuando se ejecuta la aplicación de función en el equipo local, puede agregar un `Host` sección a *local.settings.json* para habilitar la CORS. En la `Host` sección, agregue dos propiedades:

* `CORS` -Escriba la dirección URL base que es el origen de la aplicación cliente
* `CORSCredentials` : establézcalo en `true` para permitir las solicitudes de "withCredentials"

Ejemplo:

```json
{
  "IsEncrypted": false,
  "Values": {
    // values
  },
  "Host": {
    "CORS": "http://localhost:8080",
    "CORSCredentials": true
  }
}
```

#### <a name="azure"></a>Azure

Para habilitar CORS en una aplicación de función de Azure, vaya a la pantalla de configuración de CORS en el *características de la plataforma* pestaña de la aplicación de función en Azure portal.

La CORS con Access-Control-Allow-Credentials debe estar habilitado para que el cliente de SignalR llamar a la función negotiate. Seleccione la casilla de verificación para habilitarlo.

En el *orígenes permitidos* sección, agregue una entrada con la dirección URL base de origen de la aplicación web.

![Configuración de CORS](media/signalr-concept-serverless-development-config/cors-settings.png)

### <a name="using-app-service-authentication"></a>Uso de autenticación de App Service

Azure Functions tiene la autenticación integrada, compatibilidad con proveedores conocidos, como Facebook, Twitter, Google, Microsoft Account y Azure Active Directory. Esta característica se puede integrar con el *SignalRConnectionInfo* enlace para crear conexiones a Azure SignalR Service que se han autenticado a un identificador de usuario. La aplicación puede enviar mensajes mediante el *SignalR* de salida de enlace que se destinan a ese identificador de usuario.

En el portal de Azure, en la aplicación de función *características de la plataforma* pestaña, abra el *autenticación/autorización* ventana de configuración. Siga la documentación de [autenticación de App Service](../app-service/overview-authentication-authorization.md) para configurar la autenticación mediante un proveedor de identidades de su elección.

Una vez configurado, solicitudes HTTP autenticadas incluirá `x-ms-client-principal-name` y `x-ms-client-principal-id` encabezados que contienen el nombre de usuario de la identidad autenticada y el Id. de usuario, respectivamente.

Puede usar estos encabezados en su *SignalRConnectionInfo* configuración de enlace para crear las conexiones autenticadas. Este es un ejemplo C# negociar la función que usa el `x-ms-client-principal-id` encabezado.

```csharp
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

A continuación, puede enviar mensajes a ese usuario estableciendo el `UserId` propiedad de un mensaje de SignalR.

```csharp
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

Para obtener información acerca de otros lenguajes, consulte el [enlaces de Azure SignalR Service](../azure-functions/functions-bindings-signalr-service.md) para hacer referencia a las funciones de Azure.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a desarrollar y configurar aplicaciones sin servidor de SignalR Service con Azure Functions. Intente crear una aplicación mediante una de las guías de inicio rápido o tutoriales en la [página de información general de SignalR Service](index.yml).