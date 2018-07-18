---
title: 'Introducción a Text Analytics API: Azure Cognitive Services | Microsoft Docs'
description: Text Analytics API en Microsoft Cognitive Services para el análisis de sentimiento, la extracción de frases clave y la detección de lenguaje.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: db5ea943f270aa512afb508668aec90cc4c90df4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382551"
---
# <a name="what-is-text-analytics-api-version-20"></a>¿Qué es Text Analytics API, versión 2.0?

Text Analytics API es un servicio basado en la nube que proporciona un procesamiento avanzado de idioma natural sobre texto sin formato, e incluye cuatro funciones principales: análisis de sentimiento, extracción de frases clave y detección de lenguaje y vinculación de entidad.

La API tiene el respaldo de los recursos de [Microsoft Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/), una colección de algoritmos de aprendizaje automático y de inteligencia artificial en la nube, que se pueden consumir fácilmente en los proyectos de desarrollo.

## <a name="capabilities-in-text-analytics"></a>Funcionalidades de Text Analytics

El análisis de texto puede significar diferentes cosas, pero en Cognitive Services, Text Analytics API proporciona cuatro tipos de análisis, que se describen en la tabla siguiente.

| Operaciones| DESCRIPCIÓN | API existentes |
|-----------|-------------|------|
|[**Análisis de sentimiento**](how-tos/text-analytics-how-to-sentiment-analysis.md) | Averigüe qué opinan los clientes de su marca o de un tema concreto mediante el análisis de texto sin formato, con el fin de obtener pistas acerca de si sus opiniones son positivas o negativas. Esta API devuelve una puntuación de la opción, que oscila entre 0 y 1, con respecto a cada documento, donde 1 es la más positiva.<br /> Los modelos de análisis se entrenan previamente con una gran cantidad de cuerpo de texto y tecnologías de idioma natural de Microsoft. Para los [idiomas seleccionados](text-analytics-supported-languages.md), la API puede analizar y puntuar cualquier texto sin formato que se proporcione, y devolver los resultados directamente a la aplicación que realiza la llamada. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) <br /> [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package)  |
|[**Extracción de frases clave**](how-tos/text-analytics-how-to-keyword-extraction.md) | Extracción de las frases clave para identificar rápidamente los puntos principales. Por ejemplo, si el texto de entrada es "La comida estaba deliciosa y el personal era maravilloso", la API devuelve los principales puntos de conversación: "comida" y "personal maravilloso".  | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) <br /> [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) |
|[**Detección de idioma**](how-tos/text-analytics-how-to-language-detection.md) | Hasta un máximo de 120 idiomas, se detecta el idioma en que está escrito el texto de entrada y usa un código de idioma único para informar acerca de cada documento enviado en la solicitud. El código de idioma se empareja con una puntuación que indica la intensidad de esta. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) <br />  [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) | 
|[**Vinculación de entidad (versión preliminar)**](how-tos/text-analytics-how-to-entity-linking.md) | Identificación de las entidades conocidas en el texto y vinculación a más información que pueda haber en la Web. La vinculación de entidad reconoce un término y elimina la ambigüedad que se produce cuando se usa como entidad, verbo u otra forma de palabra que se distingue por separado. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) | 

## <a name="typical-workflow"></a>Flujo de trabajo típico

El flujo de trabajo es sencillo: se envían datos para su análisis y se controla el resultado en el código. Los analizadores se consumen tal cual, sin configuración ni personalización adicionales.

1. [Regístrese](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para obtener una [clave de acceso](how-tos/text-analytics-how-to-access-key.md). La clave se debe usar en cada solicitud.

2. [Formule una solicitud](how-tos/text-analytics-how-to-call-api.md#json-schema) en JSON que contenga los datos como texto sin estructura.

3. Publique la solicitud en el punto de conexión establecido durante el registro y anexe el recurso deseado: análisis de sentimiento, extracción de frases clave, detección de idioma o identificación de entidad.

4. Transmita la respuesta en secuencias o almacénela localmente. En función de cuál sea la solicitud, los resultados son una puntuación de opinión, una colección de frases clave extraídas o un código de idioma.

La salida se devuelve como documento JSON individual, con los resultados de cada documento de texto que ha publicado, en función del identificador. Posteriormente puede analizar, visualizar o clasificar los resultados hasta convertirlos en conclusiones que requieren que se realice alguna acción.

No se almacenan datos en su cuenta. Las operaciones que realiza Text Analytics API no tienen estado, lo que significa que se procesa el texto que se proporciona y los resultados se devuelven de inmediato.

<a name="supported-languages"></a>

## <a name="supported-languages"></a>Idiomas admitidos

Esta sección se ha movido a otro artículo para facilitar su detección. Para ver este contenido, consulte [Idiomas que se admiten en Text Analytics API](text-analytics-supported-languages.md).

<a name="data-limits"></a>

## <a name="data-limits"></a>Límites de datos

Todos los puntos de conexión de Text Analytics API aceptan datos de texto sin formato. El límite actual es de 5000 caracteres para cada documento; si necesita analizar documentos mayores, puede dividirlos en fragmentos más pequeños. Si necesita un límite mayor, [póngase en contacto con nosotros](https://azure.microsoft.com/overview/sales-number/) para poder analizar sus requisitos.

| Límite | Valor |
|------------------------|---------------|
| Tamaño máximo de un documento individual | 5000 caracteres, medidos por `String.Length`. |
| Tamaño máximo de la solicitud completa | 1 MB |
| Número máximo de documentos de una solicitud | 1000 documentos |

El límite de la tarifa es 100 llamadas por minuto. Tenga en cuenta que puede enviar una gran cantidad de documentos en una sola llamada (hasta 1000 documentos).

## <a name="unicode-encoding"></a>Codificación Unicode

Text Analytics API utiliza la codificación Unicode para la representación del texto y los cálculos del número de caracteres. Las solicitudes se pueden enviar tanto en UTF-8 como en UTF-16 sin que haya diferencias apreciables en el número de caracteres. Los puntos de código Unicode se utilizan como heurística de longitud de caracteres y se consideran equivalentes a efectos de los límites de datos del análisis de texto. Si utiliza `String.Length` para obtener el número de caracteres, está empleando el mismo método que nosotros para medir el tamaño de los datos.

## <a name="next-steps"></a>Pasos siguientes

En primer lugar, pruebe con la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/text-analytics/). Puede pegar una entrada de texto (un máximo de 5000 caracteres) para detectar el idioma (hasta 120), calcular una puntuación de opinión o extraer frases clave. No es preciso registrarse.

Cuando esté listo para llamar directamente a la API:

+ [Regístrese](how-tos/text-analytics-how-to-signup.md) para obtener una clave de acceso y consulte los pasos necesarios para [llamar a la API](how-tos/text-analytics-how-to-call-api.md).

+ [Inicio rápido](quickstarts/csharp.md) es un tutorial escrito en C# de las llamadas a API REST. Aprenda a enviar texto, elegir un análisis y ver los resultados con una cantidad mínima de código.

+ [Documentación de referencia de API](//go.microsoft.com/fwlink/?LinkID=759346) proporciona la documentación técnica de las API. La documentación admite llamadas insertadas para que pueda llamar a la API desde todas las páginas de la documentación.

+ [Contenido externo y de la comunidad](text-analytics-resource-external-community.md) proporciona una lista de entradas de blog y vídeos que muestran cómo usar Text Analytics con otras herramientas y tecnologías.

## <a name="see-also"></a>Otras referencias

 [Página de documentación de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)
