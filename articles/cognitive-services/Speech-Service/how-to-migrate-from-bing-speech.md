---
title: Migración de Bing Speech a Speech Service
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo migrar de una suscripción existente de Bing Speech a Speech Services desde Azure Cognitive Services.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: nitinme
ms.openlocfilehash: 5694894a78a46ad658ec18f210c6a82fb82df23f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559607"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migrar de Bing Speech al servicio Voz

Use este artículo para obtener información sobre cómo migrar sus aplicaciones de Bing Speech API al servicio Voz.

Este artículo describe las diferencias entre Bing Speech API y los servicios de voz, y sugiere estrategias para migrar sus aplicaciones. El servicio Voz no funcionará con su clave de suscripción de Bing Speech API, por lo que necesitará una nueva suscripción a los servicios de voz.

Una única clave de suscripción a los servicios de voz concede acceso a las siguientes características. Cada una se mide por separado, por lo que se le cobrará solo por las características que use.

* [Voz a texto](speech-to-text.md)
* [Conversión de voz a texto personalizada](https://cris.ai)
* [Texto a voz](text-to-speech.md)
* [Conversión de texto a voz personalizada](how-to-customize-voice-font.md)
* [Traducción de voz](speech-translation.md) (no incluye [traducción de texto](../translator/translator-info-overview.md))

El [Speech SDK](speech-sdk.md) es un reemplazo funcional para las bibliotecas de cliente de Bing Speech, pero usa una API diferente.

## <a name="comparison-of-features"></a>Comparación de características

Los servicios de voz son similares en gran medida a Bing Speech, con las siguientes diferencias.

Característica | Bing Speech | Speech Services | Detalles
-|-|-|-
SDK de C++ | :heavy_minus_sign: | :heavy_check_mark: | Los servicios de voz son compatibles con Windows y Linux.
SDK de Java | :heavy_check_mark: | :heavy_check_mark: | Los servicios de voz son compatibles con dispositivos de voz y Android.
SDK DE C# | :heavy_check_mark: | :heavy_check_mark: | Los servicios de voz son compatibles con Windows 10, Plataforma universal de Windows (UWP) y .NET Standard 2.0.
Reconocimiento de voz continua | 10 minutos | Ilimitado (con SDK) | Bing Speech y los protocolos de WebSockets de los servicios de voz admiten hasta 10 minutos por llamada. Sin embargo, el Speech SDK se vuelve a conectar automáticamente en tiempo de expiración o desconexiones.
Resultados intermedios o parciales | :heavy_check_mark: | :heavy_check_mark: | Con el protocolo WebSockets o el SDK.
Modelos de voz personalizados | :heavy_check_mark: | :heavy_check_mark: | Bing Speech requiere una suscripción independiente de Custom Speech.
Cuentes de voz personalizadas | :heavy_check_mark: | :heavy_check_mark: | Bing Speech requiere una suscripción independiente de Custom Voice.
Voces de 24 kHz | :heavy_minus_sign: | :heavy_check_mark:
Reconocimiento de intenciones mediante voz | Requiere una llamada API de LUIS independiente | Integrada (con el SDK) |  Puede usar una clave de LUIS con el servicio Voz.
Reconocimiento de intenciones simple | :heavy_minus_sign: | :heavy_check_mark:
Transcripción de lotes de archivos de sonido de larga duración | :heavy_minus_sign: | :heavy_check_mark:
Modo de reconocimiento | Manual a través de URI de punto de conexión | Automático | El modo de reconocimiento no está disponible en el servicio Voz.
Localidad de punto de conexión | Global | Regional | Los puntos de conexión regionales mejoran la latencia.
API de REST | :heavy_check_mark: | :heavy_check_mark: | Las API REST de los servicios de voz son compatibles con Bing Speech (punto de conexión diferente). Las API de REST admiten la funcionalidad de texto a voz y la funcionalidad de voz a texto de forma limitada.
Protocolos de WebSockets | :heavy_check_mark: | :heavy_check_mark: | La API WebSockets de los servicios de voz es compatible con Bing Speech (punto de conexión diferente). Migre a Speech SDK si es posible para simplificar el código.
Llamadas API de servicio a servicio | :heavy_check_mark: | :heavy_minus_sign: | Proporcionado en Bing Speech a través de la biblioteca de servicio de C#.
SDK de código abierto | :heavy_check_mark: | :heavy_minus_sign: |

Los servicios de voz usan un modelo de precios basado en tiempo (en lugar de un modelo basado en transacciones). Consulte los [precios de los servicios de voz](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para obtener más información.

## <a name="migration-strategies"></a>Estrategias de migración

Si su organización o usted tienen aplicaciones en desarrollo o producción que usan Bing Speech API, deben actualizarlas para que usen los servicios de voz tan pronto como sea posible. Vea la [documentación de los servicios de voz](index.yml) para obtener información sobre SDK disponibles, ejemplos de código y tutoriales.

Las [API REST](rest-apis.md) de los servicios de voz son compatibles con las Bing Speech API. Si actualmente usa las API de REST de Bing Speech, solo debe cambiar el punto de conexión de REST y cambiar a una clave de suscripción de los servicios de voz.

Los protocolos de WebSockets de los servicios de voz también son compatibles con los utilizados por Bing Speech. Para nuevos desarrollos, se recomienda usar el SDK de Voz en lugar de WebSockets. También es una buena idea migrar el código existente al SDK. No obstante, como con las API de REST, el código existente que utiliza Bing Speech a través de WebSockets requiere solo un cambio en el punto de conexión y una clave actualizada.

Si usa una biblioteca de cliente de Bing Speech para un lenguaje de programación específico, migrar al [Speech SDK](speech-sdk.md) requerirá cambios en la aplicación porque la API es diferente. El Speech SDK puede simplificar el código al mismo tiempo que le proporciona acceso a nuevas características.

Actualmente, el SDK de Voz es compatible con C# ([puede ver detalles aquí](https://aka.ms/csspeech)), Java (dispositivos Android y personalizados), Objective C (iOS), C++ (Windows y Linux), y JavaScript. Las API en todas las plataformas son similares, lo que acelera el desarrollo multiplataforma.

Los servicios de voz no ofrecen un punto de conexión global. Determine si la aplicación funciona eficazmente con un único punto de conexión regional para todo su tráfico. Si no lo hace, use la ubicación geográfica para determinar el punto de conexión más eficaz. Necesitará una suscripción de los servicios de voz independiente en cada región que use.

Si la aplicación usa conexiones de larga duración y no puede usar un SDK disponible, puede usar una conexión de WebSockets. Administre el límite de tiempo de espera de 10 minutos volviendo a conectarse en los momentos adecuados.

Para empezar a usar el Speech SDK:

1. Descargue el [SDK de Voz](speech-sdk.md).
1. Utilice las [guías de inicio rápido](quickstart-csharp-dotnet-windows.md) y [tutoriales](how-to-recognize-intents-from-speech-csharp.md) de los servicios de voz. Examine también los [ejemplos de código](samples.md) para obtener experiencia con las nuevas API.
1. Actualice la aplicación para usar los servicios de voz.

## <a name="support"></a>Soporte técnico

Los clientes de Bing Speech deben ponerse en contacto con el soporte al cliente mediante una [incidencia de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). También puede ponerse contacto con nosotros si sus necesidades de soporte técnico requieren un [Plan de soporte técnico](https://azure.microsoft.com/support/plans/).

Para obtener información sobre compatibilidad de API, SDK y los servicios de voz, visite la [página de soporte técnico](support.md) de los servicios de voz.

## <a name="next-steps"></a>Pasos siguientes

* [Prueba gratuita de los servicios de voz](get-started.md)
* [Inicio rápido: Reconocimiento de voz en una aplicación de UWP mediante el SDK de Voz](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Otras referencias
* [Notas de la versión de los servicios de voz](releasenotes.md)
* [¿Qué es el servicio Voz?](overview.md)
* [Documentación de los servicios de voz y Speech SDK](speech-sdk.md#get-the-sdk)
