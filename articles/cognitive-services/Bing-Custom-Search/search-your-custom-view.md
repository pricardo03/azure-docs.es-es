---
title: 'Búsqueda de una vista personalizada: Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Describe cómo buscar una vista personalizada de la web.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: maheshb
ms.openlocfilehash: 7a60ea934c6bb9008889992726ddca5dad21a640
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595617"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Llamada a la instancia de Bing Custom Search desde el portal

Después de configurar su experiencia de búsqueda personalizada, puede probarla desde el [portal](https://customsearch.ai) de Bing Custom Search. 

![captura de pantalla del portal de búsqueda personalizada de Bing](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Creación de una consulta de búsqueda 

Una vez que haya iniciado sesión en el [portal](https://customsearch.ai) de Bing Custom Search, seleccione la instancia de búsqueda y haga clic en la pestaña **Producción**. En **Endpoints** (Puntos de conexión), seleccione un punto de conexión de API (por ejemplo, Web API). La suscripción determina qué puntos de conexión se muestran.

Para crear una consulta de búsqueda, escriba los valores de parámetro para el punto de conexión. Tenga en cuenta que los parámetros que aparecen en el portal pueden cambiar en función del punto de conexión que elija. Consulte la [referencia de API de búsqueda personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) para obtener más información. Para cambiar la suscripción que usa la instancia de la búsqueda, agregue la clave de suscripción adecuado y actualizar los parámetros adecuados del mercado o lenguaje.

A continuación, se muestran algunos parámetros importantes:


|Parámetro  |DESCRIPCIÓN  |
|---------|---------|
|Consultar     | El término de búsqueda para buscar. Solo está disponible para los puntos de conexión Web, Image, Video y Autosuggest. |
|Custom Configuration ID (Id. de configuración personalizada) | El identificador de configuración de la instancia de Custom Search seleccionada. Este campo es de solo lectura. |
|Market     | El mercado desde el que se originan los resultados. Solo está disponible para los puntos de conexión Web, Image, Video y Hosted UI.        |
|Clave de suscripción | La clave de suscripción con la que probar. Puede seleccionar una clave en la lista desplegable o escribirla manualmente.          |

Al hacer clic en **Additional Parameters** (Parámetros adicionales) aparecen los siguientes parámetros:  

|Parámetro  |DESCRIPCIÓN  |
|---------|---------|
|Safe Search     | Un filtro usado para filtrar las páginas web de contenido para adultos. Solo está disponible para los puntos de conexión Web, Image, Video y Hosted UI.        |
|Idioma de la interfaz de usuario    | Idioma que se usa para las cadenas de la interfaz de usuario. Por ejemplo, si habilita las imágenes y los vídeos en Hosted UI, las pestañas **Image** (Imagen) y **Video** (Vídeo) usan el lenguaje especificado.        |
|Count     | Número de resultados de búsqueda que se devolverán en la respuesta. Disponible solo para los puntos de conexión Web, Image y Video.         |
|Offset    | El número de resultados de búsqueda que se van a omitir antes de devolver los resultados. Disponible solo para los puntos de conexión Web, Image y Video.        |
    
Tras especificar todas las opciones necesarias, haga clic en **Call** (Llamar) para ver la respuesta de JSON en el panel derecho. Si selecciona el punto de conexión Hosted UI, puede probar la experiencia de búsqueda en el panel derecho.

## <a name="change-your-bing-custom-search-subscription"></a>Cambie la suscripción a Bing Custom Search

Puede cambiar la suscripción asociada con la instancia de Bing Custom Search sin necesidad de crear una nueva instancia. Para que las llamadas de API envía y se cobra a una nueva suscripción, cree un nuevo recurso de Bing Custom Search en Azure portal. Use la nueva clave de suscripción en las solicitudes de API, junto con el Id. de configuración personalizada. de la instancia

## <a name="next-steps"></a>Pasos siguientes

- [Call your custom view with C#](./call-endpoint-csharp.md) (Llamada a la vista personalizada con C#)
- [Call your custom view with Java](./call-endpoint-java.md) (Llamada a la vista personalizada con Java)
- [Call your custom view with NodeJs](./call-endpoint-nodejs.md) (Llamada a la vista personalizada con NodeJs)
- [Call your custom view with Python](./call-endpoint-python.md) (Llamada a la vista personalizada con Python)

- [Call your custom view withthe C# SDK](./sdk-csharp-quick-start.md) (Llamada a la vista personalizada con el SDK de C#)
