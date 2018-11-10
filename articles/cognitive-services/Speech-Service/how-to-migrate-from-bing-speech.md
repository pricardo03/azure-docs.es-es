---
title: Migrar de Bing Speech al servicio Voz
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre las diferencias entre Bing Speech y el servicio Voz desde la perspectiva del desarrollador y migre la aplicación para usar el servicio Voz.
services: cognitive-services
author: wsturman
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: fdd22e14e0b7636dbc337a20dd69bf93696bb924
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416287"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migrar de Bing Speech al servicio Voz

Use este artículo para obtener información sobre cómo migrar sus aplicaciones de Bing Speech API al servicio Voz.

Este artículo describe las diferencias entre Bing Speech API y el servicio Voz, y sugiere estrategias para migrar sus aplicaciones. El servicio Voz no acepta su clave de suscripción de Bing Speech API, por lo que necesitará una nueva suscripción al servicio Voz.

Una única clave de suscripción al servicio Voz concede acceso a las siguientes características. Cada una se mide por separado, por lo que se le cobrará solo por las características que use.

* [Voz a texto](speech-to-text.md)
* [Conversión de voz a texto personalizada](https://cris.ai)
* [Texto a voz](text-to-speech.md)
* [Conversión de texto a voz personalizada](how-to-customize-voice-font.md)
* [Traducción de voz](speech-translation.md) (no incluye [traducción de texto](../translator/translator-info-overview.md))

El [Speech SDK](speech-sdk.md) es un reemplazo funcional para las bibliotecas de cliente de Bing Speech, pero usa una API diferente.

## <a name="comparison-of-features"></a>Comparación de características

El servicio Voz es similar en gran medida a Bing Speech, con las siguientes diferencias.

Característica | Bing Speech | Speech Service | Detalles
-|-|-|-
SDK de C++ | :heavy_minus_sign: | :heavy_check_mark: | El servicio Voz es compatible Windows y Linux.
SDK de Java | :heavy_check_mark: | :heavy_check_mark: | El servicio Voz es compatible con dispositivos de voz y Android.
SDK DE C# | :heavy_check_mark: | :heavy_check_mark: | El servicio Voz es compatible con Windows 10, Plataforma universal de Windows (UWP) y .NET Standard 2.0.
Reconocimiento de voz continua | 10 minutos | Ilimitado (con SDK) | Bing Speech y los protocolos de WebSockets del servicio Voz admiten hasta 10 minutos por llamada. Sin embargo, el Speech SDK se vuelve a conectar automáticamente en tiempo de expiración o desconexiones.
Resultados intermedios o parciales | :heavy_check_mark: | :heavy_check_mark: | Con el protocolo WebSockets o el SDK.
Modelos de voz personalizados | :heavy_check_mark: | :heavy_check_mark: | Bing Speech requiere una suscripción independiente de Custom Speech.
Cuentes de voz personalizadas | :heavy_check_mark: | :heavy_check_mark: | Bing Speech requiere una suscripción independiente de Custom Voice.
Voces de 24 kHz | :heavy_minus_sign: | :heavy_check_mark: 
Reconocimiento de intenciones mediante voz | Requiere una llamada API de LUIS independiente | Integrada (con el SDK) |  Puede usar una clave de LUIS con el servicio Voz.
Reconocimiento de intenciones simple | :heavy_minus_sign: | :heavy_check_mark: 
Transcripción de lotes de archivos de sonido de larga duración | :heavy_minus_sign: | :heavy_check_mark:
Modo de reconocimiento | Manual a través de URI de punto de conexión | Automático | El modo de reconocimiento no está disponible en el servicio Voz.
Localidad de punto de conexión | Global | Regional | Los puntos de conexión regionales mejoran la latencia.
API de REST | :heavy_check_mark: | :heavy_check_mark: | La API de REST del servicio Voz es compatible con Bing Speech (punto de conexión diferente). Las API de REST admiten la funcionalidad de texto a voz y la funcionalidad de voz a texto de forma limitada.
Protocolos de WebSockets | :heavy_check_mark: | :heavy_check_mark: | La API de WebSockets del servicio Voz es compatible con Bing Speech (punto de conexión diferente). Migre a Speech SDK si es posible para simplificar el código.
Llamadas API de servicio a servicio | :heavy_check_mark: | :heavy_minus_sign: | Proporcionado en Bing Speech a través de la biblioteca de servicio de C#. 
SDK de código abierto | :heavy_check_mark: | :heavy_minus_sign: |

El servicio Voz usa un modelo de precios basado en tiempo (en lugar de un modelo basado en transacciones). Consulte los [precios del servicio Voz](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para más información.

## <a name="migration-strategies"></a>Estrategias de migración

Si su organización o usted tienen aplicaciones en desarrollo o producción que usan Bing Speech API, deben actualizarlas para que usen el servicio Voz tan pronto como sea posible. Vea la [documentación del servicio Voz](index.yml) para obtener información sobre SDK disponibles, ejemplos de código y tutoriales.

Las [API de REST](rest-apis.md) del servicio Voz son compatibles con las Bing Speech API. Si actualmente usa las API de REST de Bing Speech, solo debe cambiar el punto de conexión de REST y cambiar a una clave de suscripción del servicio Voz.

Los protocolos de WebSockets del servicio Voz también son compatibles con los utilizados por Bing Speech. Para nuevos desarrollos, se recomienda usar el SDK del servicio Voz en lugar de WebSockets. También es una buena idea migrar el código existente al SDK. No obstante, como con las API de REST, el código existente que utiliza Bing Speech a través de WebSockets requiere solo un cambio en el punto de conexión y una clave actualizada.

Si usa una biblioteca de cliente de Bing Speech para un lenguaje de programación específico, migrar al [Speech SDK](speech-sdk.md) requerirá cambios en la aplicación porque la API es diferente. El Speech SDK puede simplificar el código al mismo tiempo que le proporciona acceso a nuevas características.

Actualmente, Speech SDK es compatible con C# (Windows 10, UWP, .NET Standard), Java (dispositivos Android y personalizados), Objective C (iOS), C++ (Windows y Linux) y JavaScript. Las API en todas las plataformas son similares, lo que acelera el desarrollo multiplataforma.

El servicio Voz no ofrece actualmente un punto de conexión global. Determine si la aplicación funciona eficazmente con un único punto de conexión regional para todo su tráfico. Si no lo hace, use la ubicación geográfica para determinar el punto de conexión más eficaz. Necesitará una suscripción del servicio Voz independiente en cada región que use.

Si la aplicación usa conexiones de larga duración y no puede usar un SDK disponible, puede usar una conexión de WebSockets. Administre el límite de tiempo de espera de 10 minutos volviendo a conectarse en los momentos adecuados.

Para empezar a usar el Speech SDK:

1. Descargue el [SDK de Voz](speech-sdk.md).
1. Utilice las [guías de inicio rápido](quickstart-csharp-dotnet-windows.md) y [tutoriales](how-to-recognize-intents-from-speech-csharp.md) del servicio Voz. Examine también los [ejemplos de código](samples.md) para obtener experiencia con las nuevas API.
1. Actualice la aplicación para usar las API y el servicio Voz.

## <a name="support"></a>Soporte técnico

Los clientes de Bing Speech deben ponerse en contacto con el soporte al cliente mediante una [incidencia de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). También puede ponerse contacto con nosotros si sus necesidades de soporte técnico requieren un [Plan de soporte técnico](https://azure.microsoft.com/support/plans/).

Para obtener información sobre compatibilidad de API, SDK y el servicio Voz, visite [página de soporte técnico](support.md) del servicio Voz.

## <a name="next-steps"></a>Pasos siguientes

* [Prueba gratuita del servicio Voz](get-started.md)
* [Inicio rápido: Reconocimiento de voz en una aplicación para UWP mediante el SDK de Voz](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Otras referencias
* [Notas de la versión del servicio Voz](releasenotes.md)
* [¿Qué es el servicio Voz?](overview.md)
* [Documentación del servicio Voz y del SDK correspondiente](speech-sdk.md#get-the-sdk)
