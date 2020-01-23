---
title: Restablecimiento de aptitudes (api-version=2019-050-06-Preview)
titleSuffix: Azure Cognitive Search
description: Obtenga una versión preliminar de la API REST que se usa para el enriquecimiento incremental cuando se necesita un reprocesamiento completo o parcial de un conjunto de aptitudes.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 4557e966c431eca31aa7577bebc75caae9365dc9
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75832154"
---
# <a name="reset-skills-api-version2019-05-06-preview"></a>Restablecimiento de aptitudes (api-version=2019-05-06-Preview)

> [!IMPORTANT] 
> El enriquecimiento incremental se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Por el momento, no hay compatibilidad con el portal ni con .NET SDK.

La solicitud **Restablecimiento de aptitudes** especifica qué aptitudes procesar en la siguiente ejecución del indexador. En el caso de los indexadores que tienen habilitado el almacenamiento en caché, puede solicitar explícitamente el procesamiento de las actualizaciones de aptitudes que el indexador no puede detectar. Por ejemplo, si hace cambios externos, como revisiones de una aptitud personalizada, puede usar esta API para volver a ejecutar la aptitud. Las salidas, como un almacén de información o un índice de búsqueda, se actualizan con datos reutilizables de la memoria caché y el nuevo contenido de acuerdo con las aptitudes actualizadas.

Puede restablecer un [conjunto de aptitudes](https://docs.microsoft.com/rest/api/searchservice/create-skillset) existente mediante HTTP PUT, especificando el nombre del conjunto de aptitudes que se va a actualizar en el URI de la solicitud. Debe usar **api-version=2019-05-06-Preview** en la solicitud.

```http  
PUT https://[servicename].search.windows.net/skillsets/[skillset name]/resetskills?api-version=2019-05-06-Preview
api-key: [admin key]
Content-Type: application/json
```  

También puede usar POST y colocar el nombre del indexador en el cuerpo de la solicitud:

```http
POST https://[service name].search.windows.net/indexers/resetskills?api-version=[api-version]  
api-key: [admin key]  
```  

## <a name="request-headers"></a>Encabezados de solicitud  

 En la siguiente tabla se describen los encabezados de solicitud obligatorios y opcionales.  

|Encabezado de solicitud|Descripción|  
|--------------------|-----------------|  
|*Content-Type:*|Necesario. Establézcalo en `application/json`|  
|*api-key:*|Necesario. `api-key` se usa para autenticar la solicitud en su servicio de búsqueda. Es un valor de cadena único para el servicio. La solicitud **Restablecer conjunto de aptitudes** debe incluir un encabezado `api-key` establecido en su clave de administración (en lugar de una clave de consulta).|  

También necesita el nombre del servicio para construir la dirección URL de la solicitud. Puede obtener el nombre de servicio y `api-key` desde el panel de información general del servicio de Azure Portal. Consulte [Creación de un servicio Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-create-service-portal) para ayuda sobre la navegación en páginas.  

## <a name="request-body-syntax"></a>Sintaxis del cuerpo de la solicitud  

El cuerpo de la solicitud es una matriz de nombres de aptitudes.

```json
{   
    "skillNames" : ["<SKILL-1>", "SKILL-n"]
}  
```

## <a name="response"></a>Response  

Código de estado: 204 No Content para una respuesta correcta. 

## <a name="see-also"></a>Consulte también

+ [Búsqueda de API REST](https://docs.microsoft.com/rest/api/searchservice)
+ [Códigos de estado HTTP](https://docs.microsoft.com/rest/api/searchservice/http-status-codes)  
+ [Información general sobre el enriquecimiento de IA](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro)
+ [Configuración del almacenamiento en caché y el enriquecimiento incremental](search-howto-incremental-index.md)
+ [Enriquecimiento incremental](cognitive-search-incremental-indexing-conceptual.md)