---
title: Claves de suscripción
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS utiliza dos claves, la clave de creación gratuita para crear el modelo y la clave del punto de conexión de uso medido para consultar el punto de conexión de predicción con expresiones del usuario.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: diberry
ms.openlocfilehash: 1f5aab607c5046df0dee4db5caf36b0b7de53c4d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57998749"
---
# <a name="authoring-and-query-prediction-endpoint-keys-in-luis"></a>Claves de creación y del punto de conexión de consulta de predicciones en LUIS
LUIS usa dos claves: de [creación](#programmatic-key) y [punto de conexión](#endpoint-key). La clave de creación se crea automáticamente al crear la cuenta de LUIS. Cuando esté listo para publicar la aplicación de LUIS, tiene que [crear la clave de punto de conexión](luis-how-to-azure-subscription.md), [asignarla](luis-how-to-azure-subscription.md) a la aplicación de LUIS y [usarla con la consulta de punto de conexión](#use-endpoint-key-in-query). 

|Clave|Propósito|
|--|--|
|[Clave de creación](#programmatic-key)|Creación, publicación, administración de colaboradores, control de versiones|
|[Clave de punto de conexión](#endpoint-key)| Consultas|

Es importante crear las aplicaciones de LUIS en [regiones](luis-reference-regions.md#publishing-regions) en las que también quiera publicar y consultar.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Clave de creación

Una clave de creación, también conocida como clave de inicio, se crea automáticamente al crear una cuenta de LUIS y es gratuita. Tiene una clave de creación en todas las aplicaciones de LUIS para cada [región](luis-reference-regions.md) de creación. La clave de creación se proporciona para crear la aplicación de LUIS o para probar las consultas de punto de conexión. 

Para encontrar la clave de creación, inicie sesión en [LUIS](luis-reference-regions.md#luis-website) y haga clic en el nombre de cuenta en la barra de navegación superior derecha para abrir **Configuración de la cuenta**.

![Clave de creación](./media/luis-concept-keys/programatic-key.png)

Si quiere hacer **consultas de punto de conexión de producción**, cree la [suscripción de LUIS](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) de Azure. 

> [!CAUTION]
> Para mayor comodidad, en muchos de los ejemplos se usa la clave de creación, ya que ofrece algunas llamadas de punto de conexión en su [cuota](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>Clave de punto de conexión
Cuando necesite **consultas de puntos de conexión de producción**, cree un recurso de Azure y, después, asígnelo a la aplicación de LUIS. 

[!INCLUDE [Azure resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

Cuando finalice el proceso de creación del recurso de Azure, [asigne la clave](luis-how-to-azure-subscription.md) a la aplicación. 

    * La clave de punto de conexión permite una cuota de visitas de punto de conexión basada en el plan de uso que ha especificado al crear la clave. Vea [Precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) para obtener información sobre los precios.

    * La clave de punto de conexión se puede usar para todas las aplicaciones de LUIS o para aplicaciones de LUIS específicas. 

    * No use la clave de punto de conexión para crear aplicaciones de LUIS. 

## <a name="use-endpoint-key-in-query"></a>Usar la clave de punto de conexión en consultas
El punto de conexión de LUIS acepta dos estilos de consulta; ambos usan el punto de conexión, pero en distintos lugares:

|Verbo|Ejemplo de URL y ubicación de la clave|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>Valor de cadena de consulta de `subscription-key`<br><br>Cambie el valor de la consulta de punto de conexión de `subscription-key` de la clave de creación (inicio) a la nueva clave de punto de conexión para usar la tasa de cuota de la clave de punto de conexión de LUIS. Si crea la clave y la asigna, pero no cambia el valor de la consulta de punto de conexión de `subscription-key`, no está usando la cuota de la clave de punto de conexión.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> Valor de encabezado de `Ocp-Apim-Subscription-Key`<br><br>Cambie el valor de la consulta de punto de conexión de `Ocp-Apim-Subscription-Key` de la clave de creación (inicio) a la nueva clave de punto de conexión para usar la tasa de cuota de la clave de punto de conexión de LUIS. Si crea la clave y la asigna, pero no cambia el valor de la consulta de punto de conexión de `Ocp-Apim-Subscription-Key`, no está usando la cuota de la clave de punto de conexión.|

El identificador de la aplicación que se usó en las direcciones URL anteriores, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, es la aplicación de IoT pública que se usa para la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Uso de API de Ocp-Apim-Subscription-Key
Las API de LUIS usan el encabezado `Ocp-Apim-Subscription-Key`. El nombre del encabezado no cambia en función de qué clave y conjunto de API está usando. Establezca el encabezado en la clave de creación para las API de creación. Si usa el punto de conexión, establezca el encabezado en la clave de punto de conexión. 

No puede pasar la clave de punto de conexión para crear API. Si lo hace, recibirá el error 401 "Acceso denegado debido a una clave de punto de conexión no válida". 

## <a name="key-limits"></a>Límites de la clave
Vea [Key Limits](luis-boundaries.md#key-limits) (Límites de la clave) y [Azure Regions](luis-reference-regions.md) (Regiones de Azure). La clave de creación es gratis y se usa para crear. La clave de punto de conexión de LUIS tiene un nivel gratis, pero debe crearla usted y asociarla a la aplicación de LUIS en la página **Publicar**. No se puede usar para crear, solo para las consultas de punto de conexión.

Las regiones de publicación son diferentes de las regiones de creación. Asegúrese de que crea una aplicación en la región de creación correspondiente a la región de publicación que quiera.

## <a name="key-limit-errors"></a>Errores del límite de la clave
Si supera su cuota por segundo, recibirá un error HTTP 429. Si supera su cuota por mes, recibirá un error HTTP 403. Para solucionar estos errores, obtenga una clave de [punto de conexión](#endpoint-key) de LUIS y [asígnela](luis-how-to-azure-subscription.md) a la aplicación en la página **Publicar** del sitio web de [LUIS](luis-reference-regions.md#luis-website).

## <a name="assignment-of-the-endpoint-key"></a>Asignación de la clave de punto de conexión

Puede [asignar](luis-how-to-azure-subscription.md) la clave de punto de conexión en el [portal de LUIS](https://www.luis.ai) o a través de las API correspondientes. 


## <a name="next-steps"></a>Pasos siguientes

* Descubra [conceptos](luis-how-to-azure-subscription.md) sobre las claves de creación y punto de conexión.
