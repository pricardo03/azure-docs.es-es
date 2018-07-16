---
title: 'Descripción de las claves de LUIS: Azure | Microsoft Docs'
description: Use claves de Language Understanding (LUIS) para crear la aplicación y consultar su punto de conexión.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/23/2018
ms.author: v-geberr
ms.openlocfilehash: 70bca3b181e02f42da50e827154193936544131a
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263825"
---
# <a name="keys-in-luis"></a>Claves en LUIS
LUIS usa dos claves: de [creación](#programmatic-key) y [punto de conexión](#endpoint-key). La clave de creación se crea automáticamente al crear la cuenta de LUIS. Cuando esté listo para publicar la aplicación de LUIS, tiene que [crear la clave de punto de conexión](luis-how-to-azure-subscription.md#create-luis-endpoint-key), [asignarla](Manage-keys.md#assign-endpoint-key) a la aplicación de LUIS y [usarla con la consulta de punto de conexión](#use-endpoint-key-in-query). 

|Clave|Propósito|
|--|--|
|[Clave de creación](#programmatic-key)|Creación, publicación, administración de colaboradores, control de versiones|
|[Clave de punto de conexión](#endpoint-key)| Consultas|

Es importante crear las aplicaciones de LUIS en [regiones](luis-reference-regions.md#publishing-regions) en las que también quiera publicar y consultar.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Clave de creación

Una clave de creación, también conocida como clave de inicio, se crea automáticamente al crear una cuenta de LUIS y es gratuita. Tiene una clave de creación en todas las aplicaciones de LUIS para cada [región](luis-reference-regions.md) de creación. La clave de creación se proporciona para crear la aplicación de LUIS o para probar las consultas de punto de conexión. 

Para encontrar la clave de creación, inicie sesión en [LUIS][LUIS] y haga clic en el nombre de cuenta en la barra de navegación superior derecha para abrir **Configuración de la cuenta**.

![Clave de creación](./media/luis-concept-keys/programatic-key.png)

Si quiere hacer **consultas de punto de conexión de producción**, cree una [suscripción a LUIS](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) de Azure. 

> [!CAUTION]
> Para mayor comodidad, en muchos de los ejemplos se usa la clave de creación, ya que ofrece algunas llamadas de punto de conexión en su [cuota](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>Clave de punto de conexión
 Si necesita hacer **consultas de punto de conexión de producción**, cree una [clave de LUIS](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) en Azure Portal. Recuerde el nombre que ha usado para crear la clave, lo necesitará cuando la agregue a la aplicación.

Cuando finalice el proceso de suscripción a LUIS, [agregue la clave](Manage-keys.md#assign-endpoint-key) a la aplicación en la página **Publicar**. 

La clave de punto de conexión permite una cuota de visitas de punto de conexión basada en el plan de uso que ha especificado al crear la clave. Vea [Precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) para obtener información sobre los precios.

La clave de punto de conexión se puede usar para todas las aplicaciones de LUIS o para aplicaciones de LUIS específicas. 

No use la clave de punto de conexión para crear aplicaciones de LUIS. 

## <a name="use-endpoint-key-in-query"></a>Usar la clave de punto de conexión en consultas
El punto de conexión de LUIS acepta dos estilos de consulta; ambos usan el punto de conexión, pero en distintos lugares:

|Verbo|Ejemplo de URL y ubicación de la clave|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/98998dcf-66d2-468e-840a-7c7c57549b5a?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn on the lights<br><br>Valor de cadena de consulta de `subscription-key`<br><br>Cambie el valor de la consulta de punto de conexión de `subscription-key` de la clave de creación (inicio) a la nueva clave de punto de conexión para usar la tasa de cuota de la clave de punto de conexión de LUIS. Si crea la clave y la asigna, pero no cambia el valor de la consulta de punto de conexión de "subscription-key", no está usando la cuota de la clave de punto de conexión.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/98998dcf-66d2-468e-840a-7c7c57549b5a<br><br> Valor de encabezado de `Ocp-Apim-Subscription-Key`<br><br>Cambie el valor de la consulta de punto de conexión de `Ocp-Apim-Subscription-Key` de la clave de creación (inicio) a la nueva clave de punto de conexión para usar la tasa de cuota de la clave de punto de conexión de LUIS. Si crea la clave y la asigna, pero no cambia el valor de la consulta de punto de conexión de `Ocp-Apim-Subscription-Key`, no está usando la cuota de la clave de punto de conexión.|

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Uso de API de Ocp-Apim-Subscription-Key
Las API de LUIS usan el encabezado `Ocp-Apim-Subscription-Key`. El nombre del encabezado no cambia en función de qué clave y conjunto de API está usando. Establezca el encabezado en la clave de creación para las API de creación. Si usa el punto de conexión, establezca el encabezado en la clave de punto de conexión. 

No puede pasar la clave de punto de conexión para crear API. Si lo hace, recibirá el error 401 "Acceso denegado debido a una clave de suscripción no válida". 

## <a name="key-limits"></a>Límites de la clave
Vea [Key Limits](luis-boundaries.md#key-limits) (Límites de la clave) y [Azure Regions](luis-reference-regions.md) (Regiones de Azure). La clave de creación es gratis y se usa para crear. La clave de suscripción de LUIS tiene un nivel gratis, pero debe crearla usted y asociarla a la aplicación de LUIS en la página **Publicar**. No se puede usar para crear, solo para las consultas de punto de conexión.

Las regiones de publicación son diferentes de las regiones de creación. Asegúrese de que crea una aplicación en la región de creación correspondiente a la región de publicación que quiera.

## <a name="key-limit-errors"></a>Errores del límite de la clave
Si supera su cuota por segundo, recibirá un error HTTP 429. Si supera su cuota por mes, recibirá un error HTTP 403. Para solucionar estos errores, obtenga una clave de [punto de conexión](#endpoint-key) de LUIS y [asígnela](Manage-keys.md#assign-endpoint-key) a la aplicación en la página **Publicar** del sitio web de [LUIS][LUIS].

## <a name="next-steps"></a>Pasos siguientes

* Descubra [conceptos](Manage-Keys.md#assign-endpoint-key) sobre las claves de creación y punto de conexión.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
