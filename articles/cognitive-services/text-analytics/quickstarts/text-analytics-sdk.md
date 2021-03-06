---
title: 'Inicio rápido: Biblioteca cliente v3 de Text Analytics | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Use este inicio rápido para conectar las aplicaciones a API Text Analytics desde Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 02/24/2020
ms.author: aahi
zone_pivot_groups: programming-languages-text-analytics
ms.openlocfilehash: c6883b9b79adfc4f91af354147d45ea27d806d78
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78396111"
---
# <a name="quickstart-use-the-text-analytics-client-library"></a>Inicio rápido: Uso de la biblioteca cliente de Text Analytics

Comience a trabajar con la biblioteca cliente de Text Analytics. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas.

La biblioteca cliente de Text Analytics se puede usar para realizar las siguientes operaciones:

* análisis de opiniones
* Detección de idiomas
* Reconocimiento de entidades
* Extracción de la frase clave

::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * La versión preliminar más reciente de Text Analytics API es `3.0-preview`, la cual incluye una versión preliminar pública de las funcionalidades mejoradas [Análisis de sentimiento](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) y [Reconocimiento de entidades con nombre (NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features). La versión estable más reciente es `2.1`.
>    * Asegúrese de seguir únicamente las instrucciones de la versión que está usando.
> * Por motivos de simplicidad, en el código de este artículo se usan métodos sincrónicos y almacenamiento de credenciales no protegidas. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Para más información, consulte la siguiente documentación de referencia.

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * Este inicio rápido es solo para la versión `3.0-preview` de la biblioteca cliente de Text Analytics, que incluye una versión preliminar pública de las funcionalidades mejoradas [Análisis de sentimiento](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) y [Reconocimiento de entidades con nombre (NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features).
>    * Asegúrese de seguir únicamente las instrucciones de la versión que está usando.
> * Por motivos de simplicidad, en el código de este artículo se usan métodos sincrónicos y almacenamiento de credenciales no protegidas. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Para más información, consulte la siguiente documentación de referencia.

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * La versión preliminar más reciente de Text Analytics API es `3.0-preview`, la cual incluye una versión preliminar pública de las funcionalidades mejoradas [Análisis de sentimiento](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) y [Reconocimiento de entidades con nombre (NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features). La versión estable más reciente es `2.1`.
>    * Asegúrese de seguir únicamente las instrucciones de la versión que está usando.
> * Por motivos de simplicidad, en el código de este artículo se usan métodos sincrónicos y almacenamiento de credenciales no protegidas. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Para más información, consulte la siguiente documentación de referencia.

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * La versión preliminar más reciente de Text Analytics API es `3.0-preview`, la cual incluye una versión preliminar pública de las funcionalidades mejoradas [Análisis de sentimiento](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) y [Reconocimiento de entidades con nombre (NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features). La versión estable más reciente es `2.1`.
>    * Asegúrese de seguir únicamente las instrucciones de la versión que está usando.
> * Por motivos de simplicidad, en el código de este artículo se usan métodos sincrónicos y almacenamiento de credenciales no protegidas. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Para más información, consulte la siguiente documentación de referencia. 

[!INCLUDE [Python quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-other"

## <a name="additional-language-support"></a>Compatibilidad para lenguajes adicionales

Si ha hecho clic en esta pestaña, es probable que no vea un inicio rápido en su lenguaje de programación favorito. No se preocupe, tenemos inicios rápidos adicionales disponibles. Use la tabla para encontrar el ejemplo correcto para su lenguaje de programación.

| Idioma | Versión disponible | 
|----------|------------------------|
| Ruby     | [Versión 2.1](ruby-sdk.md) | 
| Go       | [Versión 2.1](go-sdk.md) | 

::: zone-end

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de una solución](../text-analytics-user-scenarios.md#analyze-recorded-inbound-customer-calls)

* [Información general de Text Analytics](../overview.md)
* [Análisis de opiniones](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Reconocimiento de entidades](../how-tos/text-analytics-how-to-entity-linking.md)
* [Detección de idioma](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Reconocimiento de idioma](../how-tos/text-analytics-how-to-language-detection.md)
