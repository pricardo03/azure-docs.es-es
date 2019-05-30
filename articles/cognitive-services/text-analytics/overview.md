---
title: ¿Qué es Text Analytics API? - Funcionalidades -
titleSuffix: Azure Cognitive Services
description: Use Text Analytics API de Azure Cognitive Services para el análisis de sentimiento, la extracción de frases clave, la detección del idioma y el reconocimiento de entidades.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 04/03/2019
ms.author: aahi
ms.openlocfilehash: ae1d8f18705b1058c10d4720c64f624ed0ef4a3d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "60829538"
---
# <a name="what-is-text-analytics-api"></a>¿Qué es Text Analytics API?

Text Analytics API es un servicio basado en la nube que proporciona un procesamiento avanzado de idioma natural sobre texto sin formato e incluye cuatro funciones principales: análisis de sentimiento, extracción de frases clave, detección de lenguaje y reconocimiento de entidades.

La API forma parte de [Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/), una colección de algoritmos de aprendizaje automático y de inteligencia artificial en la nube para los proyectos de desarrollo.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Understanding-Text-using-Cognitive-Services/player]

El análisis de texto puede significar diferentes cosas, pero en Cognitive Services, Text Analytics API proporciona cuatro tipos de análisis, que se describen a continuación.

## <a name="sentiment-analysis"></a>Análisis de sentimiento
Use el [análisis de sentimiento](how-tos/text-analytics-how-to-sentiment-analysis.md) para averiguar qué piensan los clientes de su marca o de un tema concreto mediante el análisis de texto sin formato, con el fin de obtener pistas acerca de si sus opiniones son positivas o negativas. Esta API devuelve una puntuación de la opción, que oscila entre 0 y 1, con respecto a cada documento, donde 1 es la más positiva.<br /> Los modelos de análisis se entrenan previamente con una gran cantidad de cuerpo de texto y tecnologías de idioma natural de Microsoft. Para los [idiomas seleccionados](text-analytics-supported-languages.md), la API puede analizar y puntuar cualquier texto sin formato que se proporcione, y devolver los resultados directamente a la aplicación que realiza la llamada. Puede usar la API de [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c9) o el SDK de [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package).

## <a name="key-phrase-extraction"></a>Extracción de frases clave
[Extracción de las frases clave](how-tos/text-analytics-how-to-keyword-extraction.md) para identificar rápidamente los puntos principales. Por ejemplo, si el texto de entrada es "La comida estaba deliciosa y el personal era maravilloso", la API devuelve los principales puntos de conversación: "comida" y "personal maravilloso". Puede usar la API de [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c6) aquí o el SDK de [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package).

## <a name="language-detection"></a>Detección de idiomas
Puede [detectar en qué idioma está escrito el texto de entrada](how-tos/text-analytics-how-to-language-detection.md) y utilizar un código de idioma único para informar acerca de cada documento enviado en la solicitud para un máximo de 120 idiomas. El código de idioma se empareja con una puntuación que indica la intensidad de esta. Puede usar la API de [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/56f30ceeeda5650db055a3c7) o el SDK de [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package).

## <a name="named-entity-recognition"></a>Reconocimiento de entidades con nombre
[Identifique y clasifique las entidades](how-tos/text-analytics-how-to-entity-linking.md) en el texto como personas, lugares, organizaciones, fecha y hora, cantidades, porcentajes, divisas, etc. Las entidades más conocidas también se reconocen y vinculan a más información en la web. Puede usar la API de [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/5ac4251d5b4ccd1554da7634).

## <a name="use-containers"></a>Uso de contenedores

[Use los contenedores de Text Analytics](how-tos/text-analytics-how-to-install-containers.md) para extraer frases clave, detectar el idioma y analizar opiniones localmente, mediante la instalación de contenedores de Docker estándar más cercanos a los datos.

## <a name="typical-workflow"></a>Flujo de trabajo típico

El flujo de trabajo es sencillo: se envían datos para su análisis y se controla el resultado en el código. Los analizadores se consumen tal cual, sin configuración ni personalización adicionales.

1. [Regístrese](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para obtener una [clave de acceso](how-tos/text-analytics-how-to-access-key.md). La clave se debe usar en cada solicitud.

2. [Formule una solicitud](how-tos/text-analytics-how-to-call-api.md#json-schema) en JSON que contenga los datos como texto sin estructura.

3. Publique la solicitud en el punto de conexión establecido durante el registro y anexe el recurso deseado: análisis de sentimiento, extracción de frases clave, detección de idioma o identificación de entidad.

4. Transmita la respuesta en secuencias o almacénela localmente. En función de cuál sea la solicitud, los resultados son una puntuación de opinión, una colección de frases clave extraídas o un código de idioma.

La salida se devuelve como documento JSON individual, con los resultados de cada documento de texto que ha publicado, en función del identificador. Posteriormente puede analizar, visualizar o clasificar los resultados hasta convertirlos en conclusiones que requieren que se realice alguna acción.

No se almacenan datos en su cuenta. Las operaciones que realiza Text Analytics API no tienen estado, lo que significa que se procesa el texto que se proporciona y los resultados se devuelven de inmediato.

## <a name="text-analytics-for-multiple-programming-experience-levels"></a>Text Analytics para varios niveles de experiencia de programación

Puede empezar a usar Text Analytics API en sus procesos, aunque no tenga mucha experiencia en programación. Use estos tutoriales para averiguar cómo puede usar la API para analizar texto de distintas formas que se ajusten a su nivel de experiencia. 

* Conocimientos mínimos de programación necesarios:
    * [Uso de Text Analytics API y MS Flow para identificar el sentimiento de los comentarios en un grupo de Yammer](https://docs.microsoft.com/Yammer/integrate-yammer-with-other-apps/sentiment-analysis-flow-azure?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Integración de Power BI con Text Analytics API para analizar los comentarios de clientes](tutorials/tutorial-power-bi-key-phrases.md)
* Experiencia de programación recomendada:
    * [Análisis de sentimiento sobre los datos de streaming con Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)
    * [Compilación de una aplicación de Flask para traducir texto, analizar el sentimiento y sintetizar la voz](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2F%2Fazure%2Fcognitive-services%2Ftext-analytics%2Ftoc.json&bc=%2F%2Fazure%2Fbread%2Ftoc.json)


<a name="supported-languages"></a>

## <a name="supported-languages"></a>Idiomas admitidos

Esta sección se ha movido a otro artículo para facilitar su detección. Para ver este contenido, consulte [Idiomas que se admiten en Text Analytics API](text-analytics-supported-languages.md).

<a name="data-limits"></a>

## <a name="data-limits"></a>Límites de datos

Todos los puntos de conexión de Text Analytics API aceptan datos de texto sin formato. El límite actual es de 5120 caracteres para cada documento; si necesita analizar documentos mayores, puede dividirlos en fragmentos más pequeños. Si necesita un límite mayor, [póngase en contacto con nosotros](https://azure.microsoft.com/overview/sales-number/) para poder analizar sus requisitos.

| Límite | Valor |
|------------------------|---------------|
| Tamaño máximo de un documento individual | 5120 caracteres, medidos por [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements). |
| Tamaño máximo de la solicitud completa | 1 MB |
| Número máximo de documentos de una solicitud | 1000 documentos |

El límite de velocidad es 100 solicitudes por segunda y 1000 solicitudes por minuto. Puede enviar una gran cantidad de documentos en una sola llamada (hasta 1000).

## <a name="unicode-encoding"></a>Codificación Unicode

Text Analytics API utiliza la codificación Unicode para la representación del texto y los cálculos del número de caracteres. Las solicitudes se pueden enviar tanto en UTF-8 como en UTF-16 sin que haya diferencias apreciables en el número de caracteres. Los puntos de código Unicode se utilizan como heurística de longitud de caracteres y se consideran equivalentes a efectos de los límites de datos del análisis de texto. Si usa [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements) para obtener el número de caracteres, está empleando el mismo método que nosotros para medir el tamaño de los datos.

## <a name="next-steps"></a>Pasos siguientes

+ [Regístrese](how-tos/text-analytics-how-to-signup.md) para obtener una clave de acceso y consulte los pasos necesarios para [llamar a la API](how-tos/text-analytics-how-to-call-api.md).

+ [Inicio rápido](quickstarts/csharp.md) es un tutorial escrito en C# de las llamadas a API REST. Aprenda a enviar texto, elegir un análisis y ver los resultados con una cantidad mínima de código. Si lo prefiere, en su lugar, puede empezar con el [inicio rápido de Python](quickstarts/python.md).

+ Profundice un poco más con este [tutorial sobre análisis de sentimiento](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services) mediante Azure Databricks.

+ Consulte nuestra lista de entradas de blog y más vídeos sobre cómo usar Text Analytics API con otras herramientas y tecnologías en nuestra [página de contenido externo y de la comunidad](text-analytics-resource-external-community.md).
