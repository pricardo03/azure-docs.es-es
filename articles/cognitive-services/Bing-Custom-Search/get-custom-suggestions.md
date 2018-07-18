---
title: 'Bing Custom Search: obtención de las sugerencias de Custom Autosuggest | Microsoft Docs'
description: Se describe cómo recuperar sugerencias de Custom Autosuggest.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 73059038018602f7aa76377bf7c98d4658576576
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380998"
---
# <a name="get-custom-suggestions"></a>Obtención de sugerencias personalizadas
Antes de enviar consultas a Bing Custom Search, llame a Custom Autosuggest API para realizar sugerencias de término de búsqueda y mejorar la experiencia de búsqueda. Custom Autosuggest API devuelve una lista de consultas sugeridas basándose en una cadena de consulta parcial proporcionada por el usuario. Los términos de consulta personalizados pertinentes que especifique aparecen antes de las sugerencias que genera Autosuggest. Para obtener más información, consulte [Define custom search suggestions](define-custom-suggestions.md) (Definición de sugerencias de búsqueda personalizadas).

## <a name="endpoint"></a>Punto de conexión
Para obtener consultas sugeridas mediante Bing Custom Search API, envíe una solicitud `GET` al punto de conexión siguiente.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

## <a name="response-json"></a>JSON de respuesta
La respuesta contiene una lista de objetos SearchAction que contienen los términos de consulta sugeridos.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Cada sugerencia incluye un campo `displayText` y `query`. El campo `displayText` contiene la consulta sugerida que usa para rellenar la lista desplegable del cuadro de búsqueda.

Si el usuario selecciona una consulta sugerida de la lista desplegable, use el término de consulta en el campo `query` al llamar a [Bing Custom Search API](overview.md).

## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Call your custom search](./search-your-custom-view.md) (Llamada a la búsqueda personalizada)
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)