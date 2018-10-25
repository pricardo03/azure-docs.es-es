---
title: 'Búsqueda de una vista personalizada: Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Describe cómo buscar una vista personalizada de la web.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: maheshb
ms.openlocfilehash: 15c5b3c58c4f3617111707ed82d031b67b6ad4c1
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465142"
---
# <a name="call-your-custom-search"></a>Llamada a la búsqueda personalizada

Antes de realizar la primera llamada a Custom Search API para obtener resultados de búsqueda para la instancia, debe obtener una clave de suscripción de Cognitive Services. Para obtener una clave de Custom Search API, consulte [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).


## <a name="try-it-out"></a>Prueba

Después de configurar su experiencia de búsqueda personalizada, puede probar la configuración desde el portal de Custom Search. 

1. Inicie sesión en [Custom Search](https://customsearch.ai).
2. Haga clic en una instancia de Custom Search de la lista.
3. Haga clic en la pestaña **Production** (Producción). 
4. En la pestaña **Endpoints** (Puntos de conexión), seleccione uno de ellos (por ejemplo, Web API). La suscripción determina los puntos de conexión que se muestran (vea los [precios](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) para conocer las opciones de suscripción). 
5. Especifique los valores de los parámetros. 

    Los siguientes son los parámetros que puede establecer (la lista real depende del punto de conexión seleccionado). Para más información acerca de estos parámetros, consulte la referencia de [Custom Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).

    - **Consulta**: el término de búsqueda para buscar. Solo está disponible para los puntos de conexión Web, Image, Video y Autosuggest.
    - **Custom Configuration ID** (Id. de configuración personalizada): el identificador de configuración de la instancia de Custom Search seleccionada. Este campo es de solo lectura.
    - **Mercado**: mercado de donde proceden los resultados. Solo está disponible para los puntos de conexión Web, Image, Video y Hosted UI.
    - **Clave de suscripción**: la clave de la suscripción con la que probar. Puede seleccionar una clave en la lista desplegable o escribirla manualmente.  
      
    Al hacer clic en **Additional Parameters** (Parámetros adicionales) aparecen los siguientes parámetros:  
      
    - **Búsqueda segura**: un filtro usado para filtrar las páginas web de contenido para adultos. Solo está disponible para los puntos de conexión Web, Image, Video y Hosted UI.
    - **User Interface Language** (Idioma de la interfaz de usuario): el idioma que se usa en las cadenas de la interfaz de usuario. Por ejemplo, si habilita las imágenes y los vídeos en Hosted UI, las pestañas **Image** (Imagen) y **Video** (Vídeo) usan el lenguaje especificado.
    - **Recuento**: número de resultados de la búsqueda que se devolverán en la respuesta. Disponible solo para los puntos de conexión Web, Image y Video.
    - **Offset** (Desplazamiento): el número de resultados de búsqueda que se van a omitir antes de devolver los resultados. Disponible solo para los puntos de conexión Web, Image y Video.

6. Tras especificar todas las opciones necesarias, haga clic en **Call** (Llamar) para ver la respuesta de JSON en el panel derecho. 

Si selecciona el punto de conexión Hosted UI, puede probar la experiencia de búsqueda en el panel derecho.

## <a name="next-steps"></a>Pasos siguientes

- [Call your custom view with C#](./call-endpoint-csharp.md) (Llamada a la vista personalizada con C#)
- [Call your custom view with Java](./call-endpoint-java.md) (Llamada a la vista personalizada con Java)
- [Call your custom view with NodeJs](./call-endpoint-nodejs.md) (Llamada a la vista personalizada con NodeJs)
- [Call your custom view with Python](./call-endpoint-python.md) (Llamada a la vista personalizada con Python)

- [Call your custom view withthe C# SDK](./sdk-csharp-quick-start.md) (Llamada a la vista personalizada con el SDK de C#)