---
title: 'Características de HTTP en Durable Functions: Azure Functions'
description: Aprenda a implementar las características de HTTP integradas en la extensión Durable Functions para Azure Functions.
author: cgillum
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: b909918ff4f9abc1dd64d4c7e5ccb35954b233f7
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935549"
---
# <a name="http-features"></a>Características de HTTP

Durable Functions tiene varias características que facilitan la incorporación de orquestaciones y entidades duraderas en flujos de trabajo HTTP. En este artículo se profundiza en los detalles sobre algunas de esas características.

## <a name="exposing-http-apis"></a>Exposición de las API HTTP

Las orquestaciones y entidades se pueden invocar y administrar mediante solicitudes HTTP. La extensión Durable Functions expone las API HTTP integradas y también proporciona API para interactuar con las orquestaciones y entidades desde las funciones desencadenadas por HTTP.

### <a name="built-in-http-apis"></a>API HTTP integradas

La extensión de Durable Functions agrega automáticamente un conjunto de API HTTP al host de Azure Functions. Estas API permiten interactuar con las orquestaciones y entidades, y administrarlas sin necesidad de escribir ningún código.

Se admiten los siguientes tipos de API HTTP integradas.

* [Iniciar nueva orquestación](durable-functions-http-api.md#start-orchestration)
* [Consultar instancia de orquestación](durable-functions-http-api.md#get-instance-status)
* [Terminar instancia de orquestación](durable-functions-http-api.md#terminate-instance)
* [Enviar un evento externo a una orquestación](durable-functions-http-api.md#raise-event)
* [Purgar el historial de orquestación](durable-functions-http-api.md#purge-single-instance-history)
* [Enviar un evento de operación](durable-functions-http-api.md#signal-entity)
* [Consultar el estado de una entidad](durable-functions-http-api.md#query-entity)

Consulte las [API HTTP](durable-functions-http-api.md) para obtener una descripción completa de todas las API HTTP integradas que expone la extensión de Durable Functions.

### <a name="http-api-url-discovery"></a>Detección de la dirección URL de la API de HTTP

El [enlace del cliente de orquestación](durable-functions-bindings.md#orchestration-client) expone las API que se pueden utilizar para generar las cargas de respuesta HTTP útiles. Por ejemplo, puede crear una respuesta que contenga vínculos a las API de administración de una instancia de orquestación específica. Este ejemplo de función que se desencadena mediante HTTP muestra cómo utilizar la API para una nueva instancia de orquestación:

#### <a name="precompiled-c"></a>C# precompilado

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

#### <a name="c-script"></a>Script de C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

#### <a name="functionjson"></a>Function.json

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/function.json)]

El inicio de una función de orquestador mediante las funciones de desencadenador HTTP mostradas anteriormente se puede realizar con cualquier cliente HTTP. El siguiente comando de cURL inicia una función de orquestador denominada `DoWork`.

```bash
curl -X POST https://localhost:7071/orchestrators/DoWork -H "Content-Length: 0" -i
```

Esta es una respuesta de ejemplo para una orquestación con `abc123` como identificador (se han quitado algunos detalles para mayor claridad):

```http
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX
Retry-After: 10

{
    "id": "abc123",
    "purgeHistoryDeleteUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "sendEventPostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/raiseEvent/{eventName}?code=XXX",
    "statusQueryGetUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "terminatePostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/terminate?reason={text}&code=XXX"
}
```

Cada uno de los campos `~Uri` del ejemplo anterior corresponde a las API HTTP integradas. Estas API se pueden usar para administrar la instancia de orquestación de destino.

> [!NOTE]
> El formato de las direcciones URL del webhook puede diferir en función de la versión del host de Azure Functions en ejecución. El ejemplo anterior es para el host de Azure Functions 2.0.

Para una descripción de todas las API HTTP integradas, consulte la documentación de [referencia de la API HTTP](durable-functions-http-api.md).

### <a name="async-operation-tracking"></a>Seguimiento de operaciones asincrónicas

La respuesta HTTP que se ha mencionado anteriormente está diseñada para ayudar a implementar API de HTTP asincrónicas de ejecución prolongada con Durable Functions. Este patrón se conoce también como *Patrón de sondeo del consumidor*. El flujo cliente/servidor funciona del siguiente modo:

1. El cliente emite una solicitud HTTP para iniciar un proceso de ejecución prolongada, como una función de orquestador.
2. El desencadenador HTTP de destino devuelve una respuesta HTTP 202 con un encabezado `Location` con el valor `statusQueryGetUri`.
3. El cliente sondea la dirección URL en el encabezado `Location`. Continúa viendo las respuestas HTTP 202 con encabezado `Location`.
4. Cuando la instancia termina (o se produce un error), el punto de conexión en el encabezado `Location` devuelve HTTP 200.

Este protocolo permite la coordinación de procesos de ejecución prolongada con clientes o servicios externos que admitan el sondeo de puntos de conexión HTTP y sigan al encabezado `Location`. Las implementaciones de cliente y de servidor de este patrón están integradas en las API HTTP de Durable Functions.

> [!NOTE]
> De forma predeterminada, todas las acciones basadas en HTTP proporcionadas por [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) admiten el patrón estándar de operación asincrónica. Esta funcionalidad permite insertar una función durable de ejecución prolongada como parte del flujo de trabajo de Logic Apps. Más detalles sobre la compatibilidad de Logic Apps con los patrones asincrónicos de HTTP en la [documentación de los desencadenadores y las acciones del flujo de trabajo de Azure Logic Apps](../../logic-apps/logic-apps-workflow-actions-triggers.md).

> [!NOTE]
> La interacción con las orquestaciones se puede realizar desde cualquier tipo de función, no solo con las funciones desencadenadas por HTTP.

Para más información acerca de la administración de orquestaciones y entidades mediante las API cliente, consulte el artículo sobre [administración de instancias](durable-functions-instance-management.md).

## <a name="consuming-http-apis"></a>Consumo de API HTTP

No se permite la E/S directa de las funciones de orquestador, según se describe en las [restricciones de código de las funciones de orquestador](durable-functions-code-constraints.md). En su lugar, las funciones de orquestador normalmente llaman a [funciones de actividad](durable-functions-types-features-overview.md#activity-functions) que realizan operaciones de E/S.

A partir de Durable Functions 2.0, las orquestaciones pueden consumir de forma nativa las API HTTP mediante el [enlace de desencadenador de orquestación](durable-functions-bindings.md#orchestration-trigger).

> [!NOTE]
> La funcionalidad para llamar a los puntos de conexión HTTP directamente desde las funciones de orquestador no está disponible todavía en JavaScript.

En el ejemplo de código siguiente se muestra una función de orquestador C# que realiza una solicitud HTTP saliente mediante la API de .NET `CallHttpAsync`:

```csharp
[FunctionName("CheckSiteAvailable")]
public static async Task CheckSiteAvailable(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    Uri url = context.GetInput<Uri>();

    // Makes an HTTP GET request to the specified endpoint
    DurableHttpResponse response = 
        await context.CallHttpAsync(HttpMethod.Get, url);

    if (response.StatusCode >= 400)
    {
        // handling of error codes goes here
    }
}
```

La acción "llamar a HTTP" le permite realizar las siguientes acciones en las funciones de orquestador:

* Llamar directamente a las API HTTP desde las funciones de orquestador (con algunas limitaciones mencionadas más adelante).
* Compatibilidad automática con los patrones de sondeo del estado HTTP 202 en el lado cliente.
* Use [Identidades administradas de Azure](../../active-directory/managed-identities-azure-resources/overview.md) para realizar llamadas HTTP autorizadas a otros puntos de conexión de Azure.

La capacidad de usar API HTTP directamente desde las funciones de orquestador resulta conveniente en un determinado conjunto de escenarios comunes. Es posible implementar todas estas características mediante las funciones de actividad. En muchos casos, las funciones de actividad pueden proporcionar más flexibilidad.

### <a name="http-202-handling"></a>Control de HTTP 202

La API "llamar a HTTP" puede implementar automáticamente el lado cliente del *patrón de sondeo del consumidor*. Si una API llamada devuelve una respuesta HTTP 202 con un encabezado `Location`, la función de orquestador sondeará automáticamente el recurso `Location` hasta que vuelva una respuesta que no sea 202. La respuesta que no sea 202 será la que se devuelva al código de la función de orquestador.

> [!NOTE]
> Las funciones de orquestador también admiten de forma nativa el *patrón de sondeo del consumidor* en el lado servidor, tal y como se describe en [Seguimiento de operaciones asincrónicas](#async-operation-tracking). Esto significa que las orquestaciones en una aplicación de función pueden coordinar fácilmente las funciones del orquestador en las demás aplicaciones de función. Esto es similar al concepto de [suborquestación](durable-functions-sub-orchestrations.md), pero con compatibilidad para la comunicación entre aplicaciones. Esto es especialmente útil para el desarrollo de aplicaciones del estilo microservicio.

### <a name="managed-identities"></a>Identidades administradas

Durable Functions admite de forma nativa llamadas a las API que aceptan tokens de Azure AD para la autorización. Esta compatibilidad usa [identidades administradas de Azure](../../active-directory/managed-identities-azure-resources/overview.md) para adquirir estos tokens.

El código siguiente es un ejemplo de una función de orquestador de .NET que realiza llamadas autenticadas para reiniciar una máquina virtual mediante la [API REST de Virtual Machines](https://docs.microsoft.com/rest/api/compute/virtualmachines) en Azure Resource Manager.

```csharp
[FunctionName("RestartVm")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string subscriptionId = "mySubId";
    string resourceGroup = "myRG";
    string vmName = "myVM";
    
    // Automatically fetches an Azure AD token for resource = https://management.core.windows.net
    // and attaches it to the outgoing Azure Resource Manager API call.
    var restartRequest = new DurableHttpRequest(
        HttpMethod.Post, 
        new Uri($"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Compute/virtualMachines/{vmName}/restart?api-version={apiVersion}"),
        tokenSource: new ManagedIdentityTokenSource("https://management.core.windows.net"));
    DurableHttpResponse restartResponse = await context.CallHttpAsync(restartRequest);
    if (restartResponse.StatusCode != HttpStatusCode.OK)
    {
        throw new ArgumentException($"Failed to restart VM: {restartResponse.StatusCode}: {restartResponse.Content}");
    }
}
```

En el ejemplo anterior, el parámetro `tokenSource` se configura para adquirir tokens de Azure AD para [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) (identificados por el "URI de recurso" `https://management.core.windows.net`). En el ejemplo se da por supuesto que la aplicación de función actual se está ejecutando localmente o se ha implementado como una aplicación de Azure Functions con una identidad administrada. Se supone que la identidad local o la identidad administrada tienen permiso para administrar las máquinas virtuales en el grupo de recursos especificado, `myRG`.

En tiempo de ejecución, el origen de token configurado devuelve automáticamente un token de acceso de OAuth 2.0 y lo agrega como un token de portador `Authorization` al encabezado de la solicitud de salida. Este modelo es una mejora respecto a la adición manual de encabezados de autorización a las solicitudes HTTP porque:

* La actualización de tokens se controla automáticamente. No tendrá que preocuparse por los tokens expirados.
* Los tokens nunca se almacenan en el estado de orquestación duradero.
* No tiene que escribir código para tratar la adquisición de tokens.

Puede encontrar un ejemplo más completo en los [ejemplos de C# "RestartVMs" precompilados](https://github.com/Azure/azure-functions-durable-extension/blob/v2/samples/v2/precompiled/RestartVMs.cs).

Las identidades administradas no se limitan a la administración de recursos de Azure. Las identidades administradas se pueden usar para tener acceso a cualquier API que acepte tokens de portador de Azure AD, incluidos los servicios de Azure de terceros o aplicaciones Web de terceros. La aplicación web de terceros podría incluso ser otra aplicación de función. Para ver la lista de servicios de Azure de terceros que admiten la autenticación con Azure AD, consulte [Servicios de Azure que admiten la autenticación de Azure AD](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

### <a name="limitations"></a>Limitaciones

La compatibilidad integrada para llamar a las API HTTP es una característica útil y no es adecuada para todos los escenarios. Las solicitudes HTTP enviadas por las funciones de orquestador y sus respuestas se serializan y se conservan como mensajes en cola. Este comportamiento de puesta en cola garantiza que las llamadas HTTP sean [confiables y seguras para la reproducción de orquestación](durable-functions-orchestrations.md#reliability). Sin embargo, también significa que:

* Cada solicitud HTTP implica una latencia adicional en comparación con un cliente HTTP nativo.
* Los mensajes de solicitud o respuesta de gran tamaño que no caben en un mensaje de cola pueden degradar significativamente el rendimiento de la orquestación. La posible degradación del rendimiento se debe a la sobrecarga que supone descargar cargas de mensajes en el almacenamiento en blobs.
* No se admiten las cargas de streaming, fragmentadas ni binarias.
* La capacidad de personalizar el comportamiento del cliente HTTP es limitada.

Si alguna de estas limitaciones puede afectar a su caso de uso, considere la posibilidad de usar en su lugar las funciones de actividad y las bibliotecas de cliente HTTP específicas del lenguaje para realizar llamadas HTTP salientes.

> [!NOTE]
> Si es programador de .NET, es posible que se pregunte por qué esta característica usa los tipos `DurableHttpRequest` y `DurableHttpResponse` en lugar de los tipos integrados `HttpRequestMessage` y `HttpResponseMessage` de .NET. Esta opción de diseño fue premeditada. La razón principal es que los tipos personalizados ayudan a garantizar que los usuarios no realicen suposiciones incorrectas sobre los comportamientos admitidos del cliente HTTP interno. Los tipos específicos de Durable Functions también permiten simplificar el diseño de la API y aclarar con mayor facilidad las características especiales, como son la [integración de identidades administradas](#managed-identities) y el [patrón de sondeo del consumidor](#http-202-handling).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre las entidades duraderas](durable-functions-entities.md)