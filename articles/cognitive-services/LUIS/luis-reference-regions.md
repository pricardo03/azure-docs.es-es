---
title: Regiones de publicación y puntos de conexión
titleSuffix: Azure Cognitive Services
description: La región en la que publique la aplicación de LUIS corresponde a la región o ubicación que especifique en Azure Portal al crear una clave de punto de conexión de LUIS para Azure. Al publicar una aplicación, LUIS genera automáticamente una dirección URL de punto de conexión para la región asociada a la clave.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/10/2019
ms.author: diberry
ms.openlocfilehash: 3182ef14388d52a9223c00c013640795f07ddbaa
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214201"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Creación y publicación de regiones y las claves asociadas

La región en la que publique la aplicación de LUIS corresponde a la región o ubicación que especifique en Azure Portal al crear una clave de punto de conexión de LUIS para Azure. Al [publicar una aplicación](./luis-how-to-publish-app.md), LUIS genera automáticamente una dirección URL de punto de conexión para la región asociada a la clave. Para publicar una aplicación de LUIS en más de una región, necesitará al menos una clave por región. 

## <a name="luis-website"></a>Sitio web de LUIS
Hay tres sitios web de LUIS, en función de la región. Debe crear y publicar el contenido en la misma región. 

|LUIS|Region|
|--|--|
|[www.luis.ai][www.luis.ai]|Datos de agencias y<br>Fuera de Europa<br>Fuera de Australia|
|[au.luis.ai][au.luis.ai]|Australia|
|[eu.luis.ai][eu.luis.ai]|Europa|

## <a name="regions-and-azure-resources"></a>Regiones y recursos de Azure
La aplicación se publica en todas las regiones asociadas a los recursos de LUIS agregados en el portal de LUIS. Por ejemplo, para una aplicación creada en [www.luis.ai][www.luis.ai], si crea un recurso de LUIS en **westus** y lo agrega a la aplicación como un recurso, la aplicación se publicará en dicha región. 

## <a name="public-apps"></a>Aplicaciones públicas
Se publica una aplicación pública en todas las regiones para que un usuario con una clave de recurso de LUIS basada en regiones pueda tener acceso a la aplicación en cualquier región que esté asociado a su clave de recurso.

## <a name="publishing-regions"></a>Regiones de publicación

Las aplicaciones de LUIS creadas en https://www.luis.ai se pueden publicar en todos los puntos de conexión, salvo en [Europa](#publishing-to-europe) y [Australia](#publishing-to-australia). 

La aplicación de regiones de creación solo se puede publicar en la región de publicación correspondiente. Si la aplicación está en la región de creación incorrecta, expórtela e impórtela en la región de creación correcta para su región de publicación. 

 Región global | Región de la API de creación y sitio web de creación| Región de publicación y de consulta<br>`API region name`   |  Formato de dirección URL de punto de conexión   |
|-----|------|------|------|
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| India Central<br>`centralindia` |  https://centralindia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Asia oriental<br>`eastasia`     |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Este de Japón<br>`japaneast`     |   https://japaneast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Oeste de Japón<br>`japanwest`     |   https://japanwest.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Corea Central<br>`koreacentral`     |   https://koreacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asia | `westus`<br>[www.luis.ai][www.luis.ai]| Sudeste asiático<br>`southeastasia`     |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Australia](#publishing-to-australia) | `australiaeast`<br>[au.luis.ai][au.luis.ai]| Este de Australia<br>`australiaeast`     |  https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Europa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Centro de Francia<br>`francecentral`     | https://francecentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Europa del Norte<br>`northeurope`     | https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Europa occidental<br>`westeurope`    |  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Sur de Reino Unido 2<br>`uksouth`    |  https://uksouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Centro de Canadá<br>`canadacentral`     |   https://canadacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Centro de EE. UU.<br>`centralus`     |   https://centralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Este de EE. UU<br>`eastus`      |  https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Norteamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Este de EE. UU. 2<br>`eastus2`     |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Norteamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Centro-Norte de EE. UU<br>`northcentralus`  |  https://northcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Norteamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Centro-Sur de EE. UU<br>`southcentralus`  |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Centro occidental de EE.UU.<br>`westcentralus`    |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Norteamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Oeste de EE. UU.<br>`westus`  |   https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Norteamérica |`westus`<br>[www.luis.ai][www.luis.ai] | Oeste de EE. UU. 2<br>`westus2`    |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Sudamérica | `westus`<br>[www.luis.ai][www.luis.ai] | Sur de Brasil<br>`brazilsouth`    |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |




## <a name="publishing-to-europe"></a>Publicación en Europa

Para publicar en las regiones europeas, solo debe crear aplicaciones de LUIS en https://eu.luis.ai. Si intenta publicar en cualquier otro lugar con una clave en la región europea, LUIS mostrará un mensaje de advertencia. En su lugar, use https://eu.luis.ai. Las aplicaciones de LUIS creadas en [https://eu.luis.ai][eu.luis.ai] no se migran automáticamente a otras regiones. Exporte e importe la aplicación de LUIS para poder migrarla.

## <a name="publishing-to-australia"></a>Publicación en Australia

Para publicar en las regiones australianas, solo debe crear aplicaciones de LUIS en https://au.luis.ai. Si intenta publicar en cualquier otro lugar con una clave en la región australiana, LUIS mostrará un mensaje de advertencia. En su lugar, use https://au.luis.ai. Las aplicaciones de LUIS creadas en [https://au.luis.ai][au.luis.ai] no se migran automáticamente a otras regiones. Exporte e importe la aplicación de LUIS para poder migrarla.

## <a name="endpoints"></a>Puntos de conexión

LUIS actualmente tiene dos puntos de conexión: uno para la creación y otro para el análisis de texto.

|Propósito|URL|
|--|--|
|Creación|`https://{region}.api.cognitive.microsoft.com/luis/api/v2.0/apps/{appID}/`|
|Análisis de texto (predicción de consultas)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]`|

En la tabla siguiente se explican los parámetros que se muestran entre llaves `{}` en la tabla anterior.

|Parámetro|Propósito|
|--|--|
|region|Región de Azure: la creación y publicación tienen diferentes regiones|
|appID|Identificador de la aplicación de LUIS utilizado en la ruta de la dirección URL que se encuentra en el panel de la aplicación|
|q|Texto de expresión enviado desde la aplicación cliente, como un bot de chat|


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de entidades creadas previamente](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
