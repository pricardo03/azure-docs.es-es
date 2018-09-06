---
title: 'Introducción a Text Analytics: Azure Cognitive Services | Microsoft Docs'
description: Text Analytics en Azure Cognitive Services para el análisis de opiniones, la extracción de frases clave, la detección del idioma y la vinculación de entidades.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 8/30/2018
ms.author: ashmaka
ms.openlocfilehash: 71af2bcbf58279681bbea66d4f76e951a2efce59
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341493"
---
# <a name="what-is-text-analytics"></a>¿Qué es Text Analytics?

El servicio Text Analytics proporciona un procesamiento avanzado del lenguaje natural en texto sin formato ni estructura. Incluye cuatro funciones principales: análisis de opiniones, extracción de frases clave, detección del idioma y vinculación de entidades.

## <a name="analyze-sentiment"></a>Análisis de opinión

[Averigüe](how-tos/text-analytics-how-to-sentiment-analysis.md) qué opinan los clientes de su marca o de un tema concreto mediante el análisis de texto sin formato, con el fin de obtener pistas acerca de si sus opiniones son positivas o negativas. Esta API devuelve una puntuación de la opción, que oscila entre 0 y 1, con respecto a cada documento, donde 1 es la más positiva.<br />
Los modelos de análisis se entrenan previamente con una gran cantidad de cuerpo de texto y tecnologías de idioma natural de Microsoft. Para los [idiomas seleccionados](text-analytics-supported-languages.md), la API puede analizar y puntuar cualquier texto sin formato que se proporcione.

## <a name="extract-key-phrases"></a>Extracción de frases clave

[Extracción de las frases clave](how-tos/text-analytics-how-to-keyword-extraction.md) para identificar rápidamente los puntos principales. Por ejemplo, si el texto de entrada es "La comida estaba deliciosa y el personal era maravilloso", Text Analytics devuelve los principales puntos de conversación: "comida" y "personal maravilloso".

## <a name="detect-language"></a>Detectar idioma

[Se detecta](how-tos/text-analytics-how-to-language-detection.md) el idioma (120 como máximo) en que está escrito el texto de entrada y se usa un código de idioma único para informar acerca de cada documento enviado en la solicitud. El código de idioma se empareja con una puntuación que indica la intensidad de esta.

## <a name="identify-linked-entities-preview"></a>Identificación de entidades vinculadas (versión preliminar)

[Identificación](how-tos/text-analytics-how-to-entity-linking.md) de las entidades conocidas en el texto y vinculación a más información que pueda haber en la web. La vinculación de entidad reconoce un término y elimina la ambigüedad que se produce cuando se usa como entidad, verbo u otra forma de palabra que se distingue por separado.

## <a name="typical-workflow"></a>Flujo de trabajo típico

El flujo de trabajo es sencillo: se envían datos para su análisis y se controla el resultado en el código. Los analizadores se consumen tal cual, sin configuración ni personalización adicionales.

1. [Regístrese](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para obtener una [clave de acceso](how-tos/text-analytics-how-to-access-key.md). La clave se debe usar en cada solicitud.

2. [Cree una solicitud](how-tos/text-analytics-how-to-call-api.md#json-schema) en JSON que contenga los datos como texto sin formato ni estructura.

3. Publique la solicitud en el punto de conexión establecido durante el registro y anexe la API a la que desee llamar: análisis de opiniones, extracción de frases clave, detección de idioma o identificación de entidades.

4. Transmita la respuesta en secuencias o almacénela localmente. En función de cuál sea la solicitud, los resultados son una puntuación de opinión, una colección de frases clave extraídas o un código de idioma.

La salida se devuelve como documento JSON individual, con los resultados de cada documento de texto que ha publicado, en función del identificador. Posteriormente puede analizar, visualizar o clasificar los resultados hasta convertirlos en conclusiones que requieran alguna acción.

Las operaciones realizadas por el servicio Text Analytics no tienen estado. No se almacenan datos en su cuenta.

<a name="data-limits"></a>

## <a name="specifications"></a>Especificaciones

### <a name="supported-languages"></a>Idiomas admitidos

Consulte [Idiomas admitidos en Text Analytics](text-analytics-supported-languages.md).

### <a name="data-limits"></a>Límites de datos

Todos los puntos de conexión del servicio Text Analytics aceptan datos de texto sin formato. El límite actual es de 5000 caracteres para cada documento; si necesita analizar documentos mayores, puede dividirlos en fragmentos más pequeños. Si necesita un límite mayor, [póngase en contacto con nosotros](https://azure.microsoft.com/overview/sales-number/) para poder analizar sus requisitos.

| Límite | Valor |
|------------------------|---------------|
| Tamaño máximo de un documento individual | 5000 caracteres, medidos por `String.Length`. |
| Tamaño máximo de la solicitud completa | 1 MB |
| Número máximo de documentos de una solicitud | 1000 documentos |

El límite de la tarifa es 100 llamadas por minuto. Tenga en cuenta que puede enviar una gran cantidad de documentos en una sola llamada (hasta 1000).

### <a name="unicode-encoding"></a>Codificación Unicode

El servicio Text Analytics utiliza la codificación Unicode para la representación del texto y los cálculos del número de caracteres. Las solicitudes se pueden enviar tanto en UTF-8 como en UTF-16 sin que se observen diferencias apreciables en el número de caracteres. Si utiliza `String.Length` para obtener el número de caracteres, está empleando el mismo método que nosotros para medir el tamaño de los datos.

## <a name="next-steps"></a>Pasos siguientes

En primer lugar, pruebe con la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/text-analytics/). Puede pegar una entrada de texto (de 5000 caracteres como máximo) para detectar el idioma (hasta 120), calcular una puntuación de opinión, extraer frases clave o identificar entidades vinculadas. No es preciso registrarse.

Cuando esté listo para llamar al servicio Text Analytics directamente:

+ [Regístrese](how-tos/text-analytics-how-to-signup.md) para obtener una clave de acceso y consulte los pasos necesarios para [llamar a la API](how-tos/text-analytics-how-to-call-api.md).

+ [Inicio rápido](quickstarts/csharp.md) es un tutorial escrito en C# de las llamadas a API REST. Aprenda a enviar texto, elegir un análisis y ver los resultados con una cantidad mínima de código.

+ [Documentación de referencia de API](//go.microsoft.com/fwlink/?LinkID=759346) proporciona la documentación técnica de las API REST. La documentación admite llamadas insertadas para que pueda llamar a la API desde todas las páginas de la documentación.

+ [Contenido externo y de la comunidad](text-analytics-resource-external-community.md) proporciona una lista de entradas de blog y vídeos que muestran cómo usar Text Analytics con otras herramientas y tecnologías.

## <a name="see-also"></a>Otras referencias

 [Página de documentación de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)
