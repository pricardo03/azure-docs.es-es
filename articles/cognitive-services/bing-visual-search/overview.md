---
title: ¿Qué es Bing Visual Search?
titleSuffix: Azure Cognitive Services
description: Bing Visual Search muestra cómo obtener detalles o conclusiones sobre una imagen, como imágenes similares u orígenes de compras.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 9e323e9ec928d9f8b0592850adcd088a589ebf28
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863098"
---
# <a name="what-is-the-bing-visual-search-api"></a>¿Qué es Bing Visual Search API?

Bing Visual Search API ofrece detalles de imagen similares a los que se muestran en Bing.com/images. Al cargar una imagen o proporcionar una dirección URL a una, esta API puede identificar diversos datos sobre ella, como, por ejemplo, imágenes similares visualmente, orígenes de compra, páginas web que incluyen la imagen y mucho más. Si usa la [Bing Image Search API](../bing-image-search/overview.md), puede utilizar tokens de información detallada asociados a los resultados de búsqueda de la API en lugar de cargar una imagen.

## <a name="insights"></a>Información detallada

Esta es la información detallada que Visual Search le permite detectar:

| Conclusión                              | DESCRIPCIÓN |
|--------------------------------------|-------------|
| Imágenes visualmente similares              | Lista de imágenes que son visualmente similares a la imagen de entrada. |
| Productos visualmente similares            | Productos que son visualmente similares al producto mostrado.            |
| Orígenes de compra                     | Lugares en los que puede comprar el artículo que se muestra en la imagen de entrada.            |
| Búsquedas relacionadas                     | Búsquedas relacionadas realizadas por otros usuarios o que se basan en el contenido de la imagen.            |
| Páginas web que incluyen la imagen     | Páginas web que incluyen la imagen de entrada.            |
| Recetas                              | Páginas web que incluyen recetas para hacer el plato que se muestra en la imagen de entrada.            |

Además de estas conclusiones, Visual Search también devuelve un conjunto de términos variados (etiquetas) derivados de la imagen de entrada. Estas etiquetas permiten a los usuarios explorar los conceptos que se encuentran en la imagen. Por ejemplo, si la imagen de entrada es un atleta famoso, una de las etiquetas podría ser el nombre del atleta y otra, deportes. O bien, si la imagen de entrada es un pastel de manzana, las etiquetas podrían ser pastel de manzana, pasteles o postres, para que los usuarios puedan explorar conceptos relacionados.

Los resultados de Visual Search también incluyen rectángulos de selección para las regiones de interés de la imagen. Por ejemplo, si la imagen contiene varias celebridades, los resultados pueden incluir rectángulos de selección para cada una de las celebridades reconocidas en la imagen. O bien, si Bing reconoce un producto o prenda en la imagen, el resultado puede incluir un rectángulo de selección para el producto o prenda que se ha reconocido.

> [!IMPORTANT]
> Si obtiene información detallada de imágenes con Bing Image Search API, considere la posibilidad de cambiar a Bing Visual Search API, que proporciona información más completa.

## <a name="workflow"></a>Flujo de trabajo

Bing Visual Search API es un servicio web RESTful, por lo que resulta muy fácil llamarlo desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar código JSON. Puede usar el servicio con la API REST o el SDK.

1. Cree una cuenta de API de Cognitive Services con acceso a Bing Search APIs. Si no tiene una suscripción de Azure, puede crear una cuenta gratuita.
2. Envíe una solicitud a la API con una consulta de búsqueda válida.
3. Analice el mensaje JSON devuelto para procesar la respuesta de API.


## <a name="next-steps"></a>Pasos siguientes

Primero, pruebe la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/) de Bing Image Search API.
Esta demostración muestra cómo puede personalizar rápidamente una consulta de búsqueda y explorar la Web en busca de imágenes.

Cuando esté listo para llamar a la API, cree una [cuenta de API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) gratuita.

Para empezar a trabajar rápidamente con la primera solicitud, consulte los artículos de inicio rápido: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).


## <a name="see-also"></a>Otras referencias

* La documentación de [referencia de Bing Visual Search](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) contiene definiciones e información sobre los puntos de conexión, los encabezados, las respuestas de API y los parámetros de consulta que puede usar para solicitar resultados de búsqueda basados en imágenes.

* En los [requisitos de uso y visualización de Bing](./use-and-display-requirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.
