---
title: Migración de Azure Video Indexer API v1 a v2
titlesuffix: Azure Cognitive Services
description: En este tema se explica cómo migrar de Azure Video Indexer API v1 a v2.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: juliako
ms.openlocfilehash: 3c70bbe11d94e6b03d615b8d1394ccdca6bd3790
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985635"
---
# <a name="migrate-from-the-video-indexer-api-v1-to-v2"></a>Migración de Video Indexer API v1 a v2

> [!Note]
> Video Indexer API V1 ha quedado en desuso el 1 de agosto de 2018. A partir de esa fecha, debe usar Video Indexer API V2. <br/>Para desarrollar con Video Indexer API V2, consulte las instrucciones que encontrará [aquí](https://api-portal.videoindexer.ai/). 

En este artículo se describen los cambios que se introdujeron en la versión 2.  

## <a name="api-changes"></a>Cambios de API

### <a name="authorization-and-operations"></a>Autorización y operaciones

En la versión v2, Video Indexer cambió el modelo de autenticación y autorización de la API. Hay dos conjuntos de API: 

* Autorización 
* Operaciones

**Authorization** API se usa para obtener tokens de acceso para llamar a **Operations** API. **Operations** API contiene todas las API de Video Indexer, como cargar vídeo, obtener información y otras operaciones.

Cuando [se suscriba](video-indexer-get-started.md)a **Authorization** API, podrá obtener tokens de acceso al pasar su clave de suscripción (tal y como lo hizo en la v1).

## <a name="getting-and-using-the-access-token-for-operations-apis"></a>Obtención y uso del token de acceso para las API de operaciones

Cuando se llama a **Operations** API, ya no se usa más la clave de suscripción. En su lugar, pasará los tokens de acceso obtenidos por **Authorization** API. 

Cada solicitud debe tener un token válido, que coincida con el nivel de acceso de la API a la que está llamando. Por ejemplo, las operaciones en el usuario, como la obtención de sus cuentas, requieren un token de acceso de usuario. Las operaciones en el nivel de la cuenta, como enumerar todos los vídeos, requieren un token de acceso a la cuenta. Las operaciones en vídeos, como el vídeo de reindexación, requieren un token de acceso a la cuenta o un token de acceso al vídeo.

Para facilitar las cosas, puede usar **Authorization** API > **GetAccounts** para obtener las cuentas sin obtener un token de usuario primero. También puede solicitar las cuentas con tokens válidos, para ahorrarse una llamada al obtener un token de cuenta.

Para más información sobre los diferentes tokens de acceso, consulte [Uso de Azure Video Indexer API](video-indexer-use-apis.md).

### <a name="locations"></a>Ubicaciones

Cada llamada a la API debe incluir la ubicación de la cuenta de Video Indexer. Las llamadas API sin la ubicación o con una ubicación incorrecta producirán un error.

Se aplican los valores que se describen en la tabla siguiente. El **valor del parámetro** es el que se pasa cuando se usa la API.

|**Nombre**|**Valor del parámetro**|**Descripción**|
|---|---|---|
|Versión de prueba|evaluación|Se usa para cuentas de evaluación gratuita. Por ejemplo: https://api.videoindexer.ai/trial/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Oeste de EE. UU.|westus2|Se usa para la región de Azure Oeste de EE. UU. 2.  Por ejemplo: https://api.videoindexer.ai/westus2/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Europa del Norte |northeurope|Se usa para la región de Azure Europa del Norte. Por ejemplo: https://api.videoindexer.ai/northeurope/Accounts/{accountId}/Videos/{videoId}/Index?language=English. |
|Asia oriental|eastasia|Se usa para la región de Azure Asia Oriental. Por ejemplo: https://api.videoindexer.ai/eastasia/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|

### <a name="data-model"></a>Modelo de datos

Video Indexer tiene un modelo de datos simplificado para ofrecer información detallada mucho más clara. Para obtener más información sobre la salida producida por la API v2, consulte [Examen de la salida de Video Indexer producida por la API v2](video-indexer-output-json-v2.md).

### <a name="swagger"></a>Swagger

Las definiciones de Video Indexer API se han actualizado en consecuencia y están disponibles para su descarga mediante el [portal para desarrolladores de Video Indexer](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).


### <a name="v1-vs-v2-examples"></a>Ejemplos de la versión 1 frente a la 2

Las nuevas API V2 implican tres parámetros principales:

1. [LOCATION]: como se describió anteriormente. Puede ser trial, westus2, northeurope o eastasia.
2. [YOUR_ACCOUNT_ID]: identificador de un Guid de la cuenta. Se recupera al obtener todas las cuentas (descritas a continuación).
3. [YOUR_VIDEO_ID]: el identificador del vídeo (por ejemplo, "d4fa369abc"). Se devuelve al cargar un vídeo o al buscar vídeos.

#### <a name="uploading-a-video-in-v1"></a>Carga de un vídeo en la versión 1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=some_name&description=some_description&privacy=private&videoUrl=http://URL_TO_YOUR_VIDEO
```

#### <a name="uploading-a-video-in-v2"></a>Carga de un vídeo en la versión 2:

1. Obtención de un token de acceso para la solicitud de carga:

  Puede obtener todas las cuentas y sus tokens de acceso:

    ```
    https://api.videoindexer.ai/auth/[LOCATION]/Accounts?generateAccessTokens=true&allowEdit=true
    ```

  O bien obtener el token de acceso de la cuenta específica:
  
  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/AccessToken?allowEdit=true
  ```
2. Carga de un vídeo:

  ```
  POST https://api.videoindexer.ai/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos?name=MySample&description=MySampleDescription&videoUrl=[URL_ENCODED_VIDEO_URL]&accessToken=eyJ0eXAiOiJ...
  ```

#### <a name="getting-insights-in-v1"></a>Obtención de información detallada en la versión 1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]
```
  
#### <a name="getting-insights-in-v2"></a>Obtención de información detallada en la versión 2:

1. Utilice el token de acceso de cuenta u obtenga un token de acceso de nivel de vídeo:

  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/AccessToken
  ```
  
2. Obtención de información:

  ```
  https://api.videoindexer.ai/[LOCATION]/Accounts[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

#### <a name="getting-video-processing-state-in-v1"></a>Obtención del estado de procesamiento de vídeo en la versión 1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]/State
```
  
#### <a name="getting-video-processing-state-in-v2"></a>Obtención del estado de procesamiento de vídeo en la versión 2:

En la API v2, se devuelve el estado de procesamiento como parte de Get Video Index API.

1. Utilice el token de acceso de cuenta u obtenga un token de acceso de nivel de vídeo:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```
  
2. Obtención de información:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

## <a name="next-steps"></a>Pasos siguientes

[Uso de Azure Video Indexer API](video-indexer-use-apis.md)

