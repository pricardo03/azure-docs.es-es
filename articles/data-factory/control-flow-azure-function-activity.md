---
title: Actividad de las funciones de Azure en Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo usar la actividad de las funciones de Azure para ejecutar una función de Azure en una canalización de Data Factory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: douglasl
ms.openlocfilehash: 0fd9b679411dd06336fd65bcb400b152316146fa
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188647"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Actividad de las funciones de Azure en Azure Data Factory

La actividad de las funciones de Azure permite ejecutar [Azure Functions](../azure-functions/functions-overview.md) en una canalización de Data Factory. Para ejecutar una función de Azure, debe crear una conexión de servicio vinculado y una actividad que especifique la función de Azure que se va a ejecutar.

## <a name="azure-function-linked-service"></a>Servicio vinculado de la función de Azure

El tipo de valor devuelto de la función de Azure tiene que ser un elemento `JObject` válido. (Tenga en cuenta que [JArray](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JArray.htm) *no* es un `JObject`.) Ningún tipo de valor devuelto diferente de `JObject` produce un error y genera el error de usuario genérico *Error al llamar al punto de conexión*.

| **Propiedad** | **Descripción** | **Obligatorio** |
| --- | --- | --- |
| Tipo   | La propiedad type debe establecerse en: **AzureFunction** | Sí |
| Dirección URL de Function App | Dirección URL de la instancia de Azure Function App. El formato es `https://<accountname>.azurewebsites.net`. Esta dirección URL es el valor que aparece en la sección **URL** al visualizar la instancia de Function App en Azure Portal.  | Sí |
| Tecla de función | Tecla de acceso de la función de Azure. Haga clic en la sección **Administrar** de la función correspondiente y copie la **tecla de función** o la **tecla del host**. Obtenga más información aquí. [Enlaces y desencadenadores HTTP de Azure Functions](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | Sí |
|   |   |   |

## <a name="azure-function-activity"></a>Actividad de función de Azure

| **Propiedad**  | **Descripción** | **Valores permitidos** | **Obligatorio** |
| --- | --- | --- | --- |
| Nombre  | Nombre de la actividad en la canalización  | string | Sí |
| Tipo  | El tipo de actividad es “AzureFunctionActivity” | string | Sí |
| Servicio vinculado | El servicio vinculado de la función de Azure para la instancia correspondiente de Azure Function App  | Referencia al servicio vinculado | Sí |
| Nombre de la función  | Nombre de la función de la instancia de Azure Function App al que esta actividad llama | string | Sí |
| estático  | Método de API de REST para llamar a la función | Tipos de cadenas admitidos: "GET", "POST", "PUT"   | Sí |
| encabezado  | Encabezados que se envían a la solicitud. Por ejemplo, para establecer el idioma y el tipo en una solicitud: "headers": { "Accept-Language": "en-us", "Content-Type": "application/json" } | Cadena (o expresión con un valor resultType de cadena) | Sin  |
| Cuerpo  | Cuerpo que se envía junto con la solicitud al método de API de la función  | Cadena (o expresión con un valor resultType de cadena) u objeto.   | Necesario para los métodos POST o PUT |
|   |   |   | |

Vea el esquema de la carga de solicitud en la sección  [Solicitar un esquema de carga](control-flow-web-activity.md#request-payload-schema) .

## <a name="more-info"></a>Más información

La actividad de la función de Azure admite **enrutamiento**. Por ejemplo, si su aplicación usa el enrutamiento - `https://functionAPP.azurewebsites.net/api/functionName/{value}?code=<secret>` -, el `functionName` es `functionName/{value}`, lo cual se puede parametrizar para proporcionar el `functionName` deseado en tiempo de ejecución.

La actividad de la función de Azure admite **consultas**. Una consulta tiene que formar parte de la `functionName`, por ejemplo, `HttpTriggerCSharp2?name=hello` (donde el `function name` es `HttpTriggerCSharp2`).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las actividades en Data Factory, vea [Canalizaciones y actividades en Azure Data Factory](concepts-pipelines-activities.md).
