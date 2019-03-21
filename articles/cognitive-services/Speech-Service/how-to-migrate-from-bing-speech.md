---
title: Migrar de Bing Speech a servicios de voz de Azure
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo migrar de una suscripción existente de Bing Speech a los servicios de voz de Azure.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: 6324da55c8af4934185fa39a106939844788adba
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57848973"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migrar de Bing Speech al servicio Voz

Use este artículo para obtener información sobre cómo migrar sus aplicaciones de Bing Speech API al servicio Voz.

En este artículo se describe las diferencias entre las Bing Speech API y los servicios de voz y sugiere las estrategias para migrar sus aplicaciones. La clave de Bing Speech API de suscripción no funcionará con el servicio de voz. necesitará una nueva suscripción de servicios de voz.

Una única clave de suscripción de servicios de voz concede acceso a las siguientes características. Cada una se mide por separado, por lo que se le cobrará solo por las características que use.

* [Voz a texto](speech-to-text.md)
* [Conversión de voz a texto personalizada](https://cris.ai)
* [Texto a voz](text-to-speech.md)
* [Conversión de texto a voz personalizada](how-to-customize-voice-font.md)
* [Traducción de voz](speech-translation.md) (no incluye [traducción de texto](../translator/translator-info-overview.md))

El [Speech SDK](speech-sdk.md) es un reemplazo funcional para las bibliotecas de cliente de Bing Speech, pero usa una API diferente.

## <a name="comparison-of-features"></a>Comparación de características

Los servicios de voz son muy similares a Bing Speech, con las siguientes diferencias.

Característica | Bing Speech | Servicios de voz | Detalles
-|-|-|-
SDK de C++ | :heavy_minus_sign: | :heavy_check_mark: | Servicios de voz es compatible con Windows y Linux.
SDK de Java | :heavy_check_mark: | :heavy_check_mark: | Servicios de voz es compatible con dispositivos Android y voz.
SDK DE C# | :heavy_check_mark: | :heavy_check_mark: | Servicios de voz es compatible con Windows 10, plataforma Universal de Windows (UWP) y .NET Standard 2.0.
Reconocimiento de voz continua | 10 minutos | Ilimitado (con SDK) | Bing Speech y protocolos de WebSockets de servicios de voz admiten hasta 10 minutos por llamada. Sin embargo, el Speech SDK se vuelve a conectar automáticamente en tiempo de expiración o desconexiones.
Resultados intermedios o parciales | :heavy_check_mark: | :heavy_check_mark: | Con el protocolo WebSockets o el SDK.
Modelos de voz personalizados | :heavy_check_mark: | :heavy_check_mark: | Bing Speech requiere una suscripción independiente de Custom Speech.
Cuentes de voz personalizadas | :heavy_check_mark: | :heavy_check_mark: | Bing Speech requiere una suscripción independiente de Custom Voice.
Voces de 24 kHz | :heavy_minus_sign: | :heavy_check_mark:
Reconocimiento de intenciones mediante voz | Requiere una llamada API de LUIS independiente | Integrada (con el SDK) |  Puede usar una clave de LUIS con el servicio Voz.
Reconocimiento de intenciones simple | :heavy_minus_sign: | :heavy_check_mark:
Transcripción de lotes de archivos de sonido de larga duración | :heavy_minus_sign: | :heavy_check_mark:
Modo de reconocimiento | Manual a través de URI de punto de conexión | Automático | El modo de reconocimiento no está disponible en el servicio Voz.
Localidad de punto de conexión | Global | Regional | Los puntos de conexión regionales mejoran la latencia.
API de REST | :heavy_check_mark: | :heavy_check_mark: | Las API de REST de servicios de voz son compatibles con Bing Speech (punto de conexión diferente). Las API de REST admiten la funcionalidad de texto a voz y la funcionalidad de voz a texto de forma limitada.
Protocolos de WebSockets | :heavy_check_mark: | :heavy_check_mark: | La API de WebSockets de servicios de voz es compatible con Bing Speech (punto de conexión diferente). Migre a Speech SDK si es posible para simplificar el código.
Llamadas API de servicio a servicio | :heavy_check_mark: | :heavy_minus_sign: | Proporcionado en Bing Speech a través de la biblioteca de servicio de C#.
SDK de código abierto | :heavy_check_mark: | :heavy_minus_sign: |

Los servicios de voz utilizan un modelo de precios basado en tiempo (en lugar de un modelo basado en transacciones). Consulte [precios de servicios de voz](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para obtener más información.

## <a name="migration-strategies"></a>Estrategias de migración

Si usted o su organización tiene aplicaciones en desarrollo o producción que usan un Bing Speech API, debe actualizar para que usen los servicios de voz tan pronto como sea posible. Consulte la [documentación de servicios de voz](index.yml) para SDK disponibles, ejemplos de código y tutoriales.

Los servicios de voz [las API de REST](rest-apis.md) son compatibles con las API de Bing Speech. Si actualmente usa las API de REST de Bing Speech, debe cambiar solo el punto de conexión REST y cambie a una clave de suscripción de servicios de voz.

Los protocolos de WebSockets de servicios de voz también son compatibles con las utilizadas por Bing Speech. Para nuevos desarrollos, se recomienda usar el SDK de Voz en lugar de WebSockets. También es una buena idea migrar el código existente al SDK. No obstante, como con las API de REST, el código existente que utiliza Bing Speech a través de WebSockets requiere solo un cambio en el punto de conexión y una clave actualizada.

Si usa una biblioteca de cliente de Bing Speech para un lenguaje de programación específico, migrar al [Speech SDK](speech-sdk.md) requerirá cambios en la aplicación porque la API es diferente. El Speech SDK puede simplificar el código al mismo tiempo que le proporciona acceso a nuevas características.

Actualmente, Speech SDK es compatible con C# (Windows 10, UWP, .NET Standard), Java (dispositivos Android y personalizados), Objective C (iOS), C++ (Windows y Linux) y JavaScript. Las API en todas las plataformas son similares, lo que acelera el desarrollo multiplataforma.

Los servicios de voz no ofrecen un punto de conexión global. Determine si la aplicación funciona eficazmente con un único punto de conexión regional para todo su tráfico. Si no lo hace, use la ubicación geográfica para determinar el punto de conexión más eficaz. Necesita una suscripción de servicios de voz independiente en cada región que use.

Si la aplicación usa conexiones de larga duración y no puede usar un SDK disponible, puede usar una conexión de WebSockets. Administre el límite de tiempo de espera de 10 minutos volviendo a conectarse en los momentos adecuados.

Para empezar a usar el Speech SDK:

1. Descargue el [SDK de Voz](speech-sdk.md).
1. Trabajo a través de los servicios de voz [guías de inicio rápido](quickstart-csharp-dotnet-windows.md) y [tutoriales](how-to-recognize-intents-from-speech-csharp.md). Examine también los [ejemplos de código](samples.md) para obtener experiencia con las nuevas API.
1. Actualizar la aplicación para usar los servicios de voz.

## <a name="support"></a>Soporte técnico

Los clientes de Bing Speech deben ponerse en contacto con el soporte al cliente mediante una [incidencia de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). También puede ponerse contacto con nosotros si sus necesidades de soporte técnico requieren un [Plan de soporte técnico](https://azure.microsoft.com/support/plans/).

Compatibilidad con el servicio de voz, SDK y API, visite los servicios de voz [página de soporte técnico](support.md).

## <a name="next-steps"></a>Pasos siguientes

* [Pruebe los servicios de voz de forma gratuita](get-started.md)
* [Inicio rápido: Reconocimiento de voz en una aplicación de UWP mediante el SDK de Voz](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Vea también
* [Notas de la versión de servicios de voz](releasenotes.md)
* [¿Qué es el servicio Voz?](overview.md)
* [Documentación de servicios de voz y Speech SDK](speech-sdk.md#get-the-sdk)
