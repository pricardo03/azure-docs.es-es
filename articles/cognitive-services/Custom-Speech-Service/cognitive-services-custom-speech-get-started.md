---
title: Introducción a Custom Speech Service en Azure | Microsoft Docs
description: Suscríbase a Custom Speech Service y vincule las actividades de servicio a una suscripción de Azure para entrenar un modelo y realizar una implementación.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: bf674261a58aab4fee37920d12ce6a2ac54b58b8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975790"
---
# <a name="get-started-with-custom-speech-service"></a>Introducción a Custom Speech Service

Explore las características principales de Custom Speech Service y obtenga información sobre cómo crear, implementar y usar modelos acústicos y de lenguaje para las necesidades de su aplicación. Puede encontrar documentación más extensa e instrucciones paso a paso después de suscribirse en el portal de Custom Speech Service.

## <a name="samples"></a>Ejemplos  
Se proporciona un ejemplo para comenzar, que encontrará [aquí](https://github.com/Microsoft/Cognitive-Custom-Speech-Service).

## <a name="prerequisites"></a>Requisitos previos  

### <a name="subscribe-to-custom-speech-service-and-get-a-subscription-key"></a>Suscribirse a Custom Speech Service y obtener una clave de suscripción
Antes de divertirse con el ejemplo anterior, debe suscribirse a Custom Speech Service y obtener una clave, consulte [Suscripciones](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/CustomSpeech) o siga las explicaciones que encontrará [aquí](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-subscribe.md). En este tutorial, puede usarse tanto la clave principal como la secundaria. Asegúrese de seguir los procedimientos recomendados para mantener en secreto y protegida la clave de API.

### <a name="get-the-client-library-and-example"></a>Obtener la biblioteca cliente y ejemplo
Puede descargar una biblioteca cliente y un ejemplo a través del [SDK](https://www.microsoft.com/cognitive-services/en-us/SDK-Sample?api=bing%20speech&category=sdk). Debe extraer el archivo ZIP descargado en una carpeta de su elección, muchos usuarios eligen la carpeta de Visual Studio 2015.

## <a name="creating-a-custom-acoustic-model"></a>Crear un modelo acústico personalizado
Para personalizar el modelo acústico para un dominio en particular, se requiere una colección de datos de voz. Esta colección se compone de un conjunto de archivos de audio de los datos de voz y un archivo de texto de transcripciones de cada archivo de audio. Los datos de audio deben ser representativos del escenario en el que quiere usar el reconocedor.

Por ejemplo: si quiere reconocer mejor la voz en el entorno de una fábrica con ruido, los archivos de audio deben consistir en personas que hablan en una fábrica con ruido.
Si está interesado en optimizar el rendimiento de un solo hablante, por ejemplo, quiere transcribir todas las Charlas junto al fuego de FDR, los archivos de audio deben consistir en muchos ejemplos de ese único hablante.

Puede encontrar una descripción detallada sobre cómo crear un modelo acústico personalizado [aquí](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-acoustic-model.md).

## <a name="creating-a-custom-language-model"></a>Crear un modelo de lenguaje personalizado
El procedimiento para crear un modelo de lenguaje personalizado es similar a crear un modelo acústico excepto en que no hay ningún dato de audio, solo texto. El texto debe consistir en numerosos ejemplos de consultas o expresiones que se espera que digan (o escriban) los usuarios registrados en la aplicación.

Puede encontrar una descripción detallada sobre cómo crear un modelo de lenguaje personalizado [aquí](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-language-model.md).

## <a name="creating-a-custom-speech-to-text-endpoint"></a>Crear un punto de conexión personalizado de voz a texto
Cuando haya creado los modelos acústicos y los modelos de lenguaje personalizados, se pueden implementar en un punto de conexión personalizado de voz a texto. Para crear un nuevo punto de conexión personalizado, haga clic en "Implementaciones" del menú "CRIS" en la parte superior de la página. Esto le llevará a una tabla denominada "Implementaciones" de los puntos de conexión personalizados actuales. Si aún no ha creado ningún punto de conexión, la tabla estará vacía. La configuración regional actual se refleja en el título de la tabla. Si desea crear una implementación para un lenguaje diferente, haga clic en "Change Locale" (Cambiar configuración regional). Se puede encontrar más información sobre los lenguajes admitidos en la sección sobre el cambio de la configuración regional.

Puede encontrar una descripción detallada sobre cómo crear un punto de conexión personalizado de voz a texto [aquí](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md).

## <a name="using-a-custom-speech-endpoint"></a>Uso de un punto de conexión de voz personalizado
Las solicitudes pueden enviarse a un punto de conexión de voz a texto CRIS de forma muy similar que el punto de conexión de voz de Microsoft Cognitive Services. Tenga en cuenta que estos puntos de conexión son funcionalmente idénticos a los puntos de conexión predeterminados de Speech API. Por lo tanto, la misma funcionalidad disponible a través de la biblioteca cliente o la API de REST para Speech API también está disponible para el punto de conexión personalizado.

Puede encontrar una descripción detallada sobre cómo usar un punto de conexión personalizado de voz a texto [aquí](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-use-endpoint.md).


Tenga en cuenta que los puntos de conexión creados a través de CRIS pueden procesar un número diferente de solicitudes simultáneas según el plan al que está asociada la suscripción. Si se solicitan más reconocimientos, devolverá el código de error 429 (Demasiadas solicitudes). Para obtener más información, visite la [información sobre precios](https://www.microsoft.com/cognitive-services/en-us/pricing). Además, existe una cuota mensual de solicitudes para el plan gratuito. En los casos en los que acceda al punto de conexión del plan gratuito por encima de la cuota mensual, el servicio devolverá el código de error 403 Prohibido.

El servicio supone que el audio se transmite en tiempo real. Si se envía con mayor rapidez, la solicitud se considerará en ejecución hasta que transcurra su duración en tiempo real.

* [Información general](cognitive-services-custom-speech-home.md)
* [P+F](cognitive-services-custom-speech-faq.md)
* [Glosario](cognitive-services-custom-speech-glossary.md)
