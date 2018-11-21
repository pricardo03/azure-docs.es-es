---
title: 'Control de errores y excepciones: Azure Logic Apps | Microsoft Docs'
description: Obtenga información acerca de los patrones para el control de errores y excepciones en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: dereklee
ms.author: deli
manager: jeconnoc
ms.date: 01/31/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 19a715812f1250523fd050ac8b80dee9ec664be4
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686269"
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Control de errores y excepciones en Azure Logic Apps

La manera en la que una arquitectura de integración controla correctamente el tiempo de inactividad o los problemas causados por sistemas dependientes puede plantear un desafío. Para ayudarle a crear integraciones sólidas y resistentes que controlen correctamente los problemas y los errores, Logic Apps proporciona una experiencia de primera clase para el control de errores y excepciones. 

<a name="retry-policies"></a>

## <a name="retry-policies"></a>Directivas de reintentos

Para el control de errores y excepciones más básico, puede usar una *directiva de reintentos* en cualquier acción o desencadenador compatible. Una directiva de reintentos especifica cuándo y cómo la acción o desencadenador vuelve a intentar una solicitud cuando se agota el tiempo de espera de la solicitud original o se produce un error, como cualquier solicitud que devuelva una respuesta 408, 429 o 5xx. Si no se utiliza ninguna otra directiva de reintentos, se usará la directiva predeterminada. 

Estos son los tipos de directivas de reintentos: 

| Escriba | DESCRIPCIÓN | 
|------|-------------| 
| [**Predeterminada**](#default-retry) | Esta directiva envía hasta cuatro reintentos en intervalos de [*aumento exponencial*](#exponential-retry) con una escala de 7,5 segundos, pero que están limitados entre 5 y 45 segundos. | 
| [**Intervalo exponencial**](#exponential-retry)  | Esta directiva espera un intervalo aleatorio seleccionado de un intervalo exponencialmente creciente antes de enviar la solicitud siguiente. | 
| [**Intervalo fijo**](#fixed-retry)  | Esta directiva espera el intervalo especificado antes de enviar la solicitud siguiente. | 
| [**Ninguna**](#no-retry)  | No volver a enviar la solicitud. | 
||| 

Para más información acerca de los límites de las directivas de reintentos, consulte [Límites y configuración de Logic Apps](../logic-apps/logic-apps-limits-and-config.md#request-limits). 

### <a name="change-retry-policy"></a>Cambio de la directiva de reintentos

Para seleccionar una directiva de reintentos diferente, siga estos pasos: 

1. Abra la aplicación lógica en el Diseñador de aplicaciones lógicas. 

2. Abra el elemento **Configuración** de una acción o un desencadenador.

3. Si la acción o el desencadenador admiten directivas de reintentos, en **Directiva de reintentos**, seleccione el tipo que desee. 

O bien, puede especificar manualmente la directiva de reintentos en la sección `inputs` de una acción o un desencadenador que admitan directivas de reintentos. Si no se especifica una directiva de reintentos, la acción utiliza la directiva predeterminada.

```json
"<action-name>": {
   "type": "<action-type>", 
   "inputs": {
      "<action-specific-inputs>",
      "retryPolicy": {
         "type": "<retry-policy-type>",
         "interval": "<retry-interval>",
         "count": <retry-attempts>,
         "minimumInterval": "<minimum-interval>",
         "maximumInterval": "<maximun-interval>"
      },
      "<other-action-specific-inputs>"
   },
   "runAfter": {}
}
```

*Obligatorio*

| Valor | Escriba | DESCRIPCIÓN |
|-------|------|-------------|
| <*retry-policy-type*> | string | El tipo de directiva de reintentos que quiere usar: `default`, `none`, `fixed` o `exponential`. | 
| <*retry-interval*> | string | El intervalo de reintento, donde se debe usar el [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) para el valor. El intervalo mínimo predeterminado es `PT5S` y el intervalo máximo es `PT1D`. Cuando se usa la directiva de intervalo exponencial, puede especificar diferentes valores mínimos y máximos. | 
| <*retry-attempts*> | Entero | El número de reintentos, que debe estar comprendido entre 1 y 90 | 
||||

*Opcional*

| Valor | Escriba | DESCRIPCIÓN |
|-------|------|-------------|
| <*minimum-interval*> | string | Para la directiva de intervalo exponencial, el intervalo más pequeño para el intervalo seleccionado aleatoriamente en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) | 
| <*maximum-interval*> | string | Para la directiva de intervalo exponencial, el intervalo más grande para el intervalo seleccionado aleatoriamente en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) | 
|||| 

Aquí tiene más información acerca de los tipos de directivas diferentes.

<a name="default-retry"></a>

### <a name="default"></a>Valor predeterminado

Si no define una directiva de reintentos, la acción utiliza la directiva predeterminada, que es una [directiva de intervalo exponencial](#exponential-interval) que envía hasta cuatro reintentos en intervalos crecientes exponencialmente que aumentan 7,5 segundos cada vez. El intervalo se demora entre 5 y 45 segundos. 

Aunque no definida explícitamente en la acción o el desencadenador, le mostramos cómo se comporta la directiva predeterminada en una acción HTTP de ejemplo:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "http://myAPIendpoint/api/action",
      "retryPolicy" : {
         "type": "exponential",
         "interval": "PT7S",
         "count": 4,
         "minimumInterval": "PT5S",
         "maximumInterval": "PT1H"
      }
   },
   "runAfter": {}
}
```

### <a name="none"></a>None

Para especificar que la acción o el desencadenador no vuelvan a intentar las solicitudes con error, establezca el elemento <*retry-policy-type*> en `none`.

### <a name="fixed-interval"></a>Intervalo fijo

Para especificar que la acción o el desencadenador esperen el intervalo especificado antes de enviar la solicitud siguiente, establezca el elemento <*retry-policy-type*> en `fixed`.

*Ejemplo*

Esta directiva de reintentos intenta obtener las últimas noticias dos veces más tras la primera solicitud con error con un retraso de 30 segundos entre cada intento:

```json
"Get_latest_news": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://mynews.example.com/latest",
      "retryPolicy": {
         "type": "fixed",
         "interval": "PT30S",
         "count": 2
      }
   }
}
```

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Intervalo exponencial

Para especificar que la acción o el desencadenador esperen un intervalo aleatorio antes de enviar la solicitud siguiente, establezca el elemento <*retry-policy-type*> en `exponential`. El intervalo aleatorio se selecciona de un intervalo exponencialmente creciente. Si lo desea, también puede invalidar los intervalos mínimo y máximo predeterminados especificando sus propios intervalos mínimo y máximo.

**Intervalos variables aleatorios**

Esta tabla muestra cómo Logic Apps genera una variable aleatoria uniforme dentro del intervalo especificado para cada reintento hasta el número de reintentos, este incluido:

| Número de reintentos | Intervalo mínimo | Intervalo máximo |
|--------------|------------------|------------------|
| 1 | max(0, <*minimum-interval*>) | min(interval, <*maximum-interval*>) |
| 2 | max(interval, <*minimum-interval*>) | min(2 * interval, <*maximum-interval*>) |
| 3 | max(2 * interval, <*minimum-interval*>) | min(4 * interval, <*maximum-interval*>) |
| 4 | max(4 * interval, <*minimum-interval*>) | min(8 * interval, <*maximum-interval*>) |
| .... | .... | .... | 
|||| 

## <a name="catch-and-handle-failures-with-the-runafter-property"></a>Detección y control de errores con la propiedad RunAfter

Cada acción de aplicación lógica declara las acciones que deben finalizar antes de que dicha acción se inicie, de manera similar a la especificación del orden de los pasos en el flujo de trabajo. En la definición de acción, la propiedad **runAfter** define este orden y es un objeto que describe qué acciones y estados de acción ejecutan la acción.

De forma predeterminada, todas las acciones que se agregan en el Diseñador de aplicación lógica están configuradas para que se ejecuten después del paso anterior, si es el resultado del paso anterior es **Succeeded**. Sin embargo, puede personalizar el valor **runAfter** para que las acciones se activen cuando las acciones anteriores den como resultado **Failed**, **Skipped** o alguna combinación de estos valores. Por ejemplo, para agregar un elemento a un tema de Service Bus específico después de que se produzca un error en una determinada acción **Insert_Row**, podría utilizar esta definición **runAfter** de ejemplo:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

La propiedad **runAfter** está configurada para ejecutarse cuando el estado de la acción **Insert_Row** es **Failed**. Para ejecutar la acción si el estado de la acción es **Correcta**, **Errónea** u **Omitida**, use esta sintaxis:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Las acciones que se ejecutan y completan correctamente después de que una acción anterior haya producido un error presentan la marca **Succeeded**. Este comportamiento significa que, si detecta correctamente todos los errores de un flujo de trabajo, la propia ejecución se marca como **Succeeded**.

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Evaluación de las acciones con ámbitos y sus resultados

De forma similar a la ejecución de pasos tras acciones individuales con la propiedad **runAfter**, puede agrupar acciones dentro de un [ámbito](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md). Puede usar ámbitos si desea agrupar acciones lógicamente, evaluar el estado global del ámbito y realizar acciones basadas en ese estado. Una vez que todas las acciones de un ámbito acaben de ejecutarse, el propio ámbito obtiene su estado. 

Para comprobar el estado del ámbito, puede usar los mismos criterios que usa para comprobar el estado de ejecución de la aplicación lógica, como **Succeeded**, **Failed**, etc. 

De forma predeterminada, cuando las acciones del ámbito se ejecutan correctamente, el estado del ámbito se marca como **Succeeded**. Si la acción final de un ámbito da como resultado **Failed** o **Aborted**, el estado del ámbito se marca como **Failed**. 

Para detectar las excepciones en un ámbito **Failed** y ejecutar acciones que controlen estos errores, puede usar la propiedad **runAfter** para ese ámbito **Failed**. De ese modo, si *cualquier* acción del ámbito produce un error, y usa la propiedad **runAfter** para ese ámbito, puede crear una única acción para detectar errores.

Para conocer los límites en los ámbitos, consulte [Límites y configuración](../logic-apps/logic-apps-limits-and-config.md).

### <a name="get-context-and-results-for-failures"></a>Obtención del contexto y resultados de errores

Aunque la detección de errores desde un ámbito es útil, quizás también necesite el contexto como ayuda para comprender exactamente qué acciones han producido un error y los errores o códigos de estado que se hayan devuelto. La expresión `@result()` proporciona contexto sobre el resultado de todas las acciones de un ámbito.

La expresión `@result()` acepta un único parámetro (el nombre del ámbito) y devuelve una matriz de todos los resultados de acción desde dentro de ese ámbito. Estos objetos de acción incluyen los mismos atributos que el objeto  **@actions()**, como la hora de inicio, la hora de finalización, el estado, las entradas, los identificadores de correlación y las salidas de la acción. Para enviar el contexto de las acciones que produjeron un error dentro de un ámbito, puede emparejar fácilmente una función **@result()** con una propiedad **runAfter**.

Para ejecutar una acción para cada una de las acciones de un ámbito que tenga un resultado **Failed** y filtrar la matriz de resultados para las acciones que produjeron un error, puede emparejar **@result()** con una acción **[Filtrar matriz](../connectors/connectors-native-query.md)** y un bucle [**For each**](../logic-apps/logic-apps-control-flow-loops.md). Puede tomar la matriz de resultados filtrada y realizar una acción para cada error mediante el bucle **For each**. 

En este ejemplo, seguido de una explicación detallada, se envía una solicitud HTTP POST con el cuerpo de respuesta de todas las acciones que produjeron un error dentro del ámbito "My_Scope":

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": "@result('My_Scope')",
      "where": "@equals(item()['status'], 'Failed')"
   },
   "runAfter": {
      "My_Scope": [
         "Failed"
      ]
    }
},
"For_each": {
   "type": "foreach",
   "actions": {
      "Log_exception": {
         "type": "Http",
         "inputs": {
            "method": "POST",
            "body": "@item()['outputs']['body']",
            "headers": {
               "x-failed-action-name": "@item()['name']",
               "x-failed-tracking-id": "@item()['clientTrackingId']"
            },
            "uri": "http://requestb.in/"
         },
         "runAfter": {}
      }
   },
   "foreach": "@body('Filter_array')",
   "runAfter": {
      "Filter_array": [
         "Succeeded"
      ]
   }
}
```

Este es un tutorial detallado que describe lo que sucede en este ejemplo:

1. Para obtener el resultado de todas las acciones de "My_Scope", la acción **Filter Array** usa esta expresión de filtro: `@result('My_Scope')`.

2. La condición de **Filter Array** es cualquier elemento `@result()` que tenga el estado **Failed** (Erróneo). Esta condición filtra la matriz que contiene todos los resultados de acción de "My_Scope" a una matriz con solo los resultados de acción que hayan producido un error.

3. Realiza una acción de bucle **For each** en las salidas de la *matriz filtrada*. Este paso realiza una acción para cada resultado de acción con errores que se filtrara antes.

   Si una sola acción del ámbito produjo un error, las acciones del bucle **For each** se ejecutan solo una vez. 
   Muchas acciones con error causan una acción por cada error.

4. Envía una solicitud HTTP POST en el cuerpo de respuesta del elemento **For each**, que es la expresión `@item()['outputs']['body']`. 

   La forma del elemento `@result()` es la misma que la forma de `@actions()` y se puede analizar del mismo modo.

5. Incluya dos encabezados personalizados con el nombre de la acción con errores (`@item()['name']`) y el id. de seguimiento de cliente de la ejecución con errores `@item()['clientTrackingId']`.

Como referencia, este es un ejemplo de un único elemento `@result()`, que muestra las propiedades **name**, **body** y **clientTrackingId** que se analizan en el ejemplo anterior. Fuera de una acción **For each**, `@result()` devuelve una matriz de estos objetos.

```json
{
   "name": "Example_Action_That_Failed",
   "inputs": {
      "uri": "https://myfailedaction.azurewebsites.net",
      "method": "POST"
   },
   "outputs": {
      "statusCode": 404,
      "headers": {
         "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
         "Server": "Microsoft-IIS/8.0",
         "X-Powered-By": "ASP.NET",
         "Content-Length": "68",
         "Content-Type": "application/json"
      },
      "body": {
         "code": "ResourceNotFound",
         "message": "/docs/folder-name/resource-name does not exist"
      }
   },
   "startTime": "2016-08-11T03:18:19.7755341Z",
   "endTime": "2016-08-11T03:18:20.2598835Z",
   "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
   "clientTrackingId": "08587307213861835591296330354",
   "code": "NotFound",
   "status": "Failed"
}
```

Para poner en práctica diferentes patrones de control de excepciones, puede usar las expresiones descritas anteriormente en este artículo. Puede optar por ejecutar una única acción de control de excepciones fuera del ámbito, que acepte toda la matriz filtrada de errores y eliminar la acción **For each**. También puede incluir otras propiedades útiles de la respuesta **@result()**, tal como se mostró anteriormente.

## <a name="azure-diagnostics-and-metrics"></a>Azure Diagnostics y métricas

Los patrones anteriores son una manera excelente de controlar errores y excepciones dentro de una ejecución, pero también puede identificar y responder a los errores con independencia de la ejecución en sí. 
[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) ofrece un método sencillo de enviar todos los eventos del flujo de trabajo (incluidos todos los estados de ejecución y acción) a una cuenta de Azure Storage o a un centro de eventos creado con Azure Event Hubs. 

Para evaluar los estados de ejecución, puede supervisar los registros y las métricas, o publicarlos en la herramienta de supervisión que prefiera. Una posible opción es transmitir todos los eventos mediante Event Hubs a [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). En Stream Analytics, puede escribir consultas en directo de anomalías, promedios o errores de los registros de diagnóstico. Asimismo, puede usar Stream Analytics para enviar información a otros orígenes de datos, como colas, temas, SQL, Azure Cosmos DB o Power BI.

## <a name="next-steps"></a>Pasos siguientes

* [Vea cómo un cliente crea el control de errores con Azure Logic Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Encuentre más escenarios y ejemplos de Logic Apps](../logic-apps/logic-apps-examples-and-scenarios.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
