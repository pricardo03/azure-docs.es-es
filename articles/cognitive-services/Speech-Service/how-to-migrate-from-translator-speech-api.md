---
title: Migrar de Translator Speech API al servicio Voz
titleSuffix: Azure Cognitive Services
description: Aprenda a migrar sus aplicaciones desde Translator Speech API hasta los servicios de Voz.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: 1ed494cea1ccf8845a25a3ab49d3194cc6a55509
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65785676"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Migrar de Translator Speech API al servicio Voz

Use este artículo para obtener información sobre cómo migrar sus aplicaciones de Translator Speech API de Microsoft al [servicio Voz](index.yml). Esta guía describe las diferencias entre Translator Speech API y el servicio Voz, y sugiere estrategias para migrar sus aplicaciones.

> [!NOTE]
> El servicio Voz aceptará su clave de suscripción de Translator Speech API. Deberá crear una suscripción de servicios de Voz.

## <a name="comparison-of-features"></a>Comparación de características

| Característica                                           | Translator Speech API                                  | Speech Services | Detalles                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Traducción a texto                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Traducción a voz                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Punto de conexión global                                   | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Los servicios de Voz no ofrecen un punto de conexión global. Un punto de conexión global puede dirigir automáticamente el tráfico al punto de conexión regional más cercano, reduciendo la latencia en la aplicación.                                                    |
| Puntos de conexión regionales                                | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Límite de tiempo de conexión                             | 90 minutos                                               | Ilimitado con SDK. 10 minutos con una conexión WebSockets                                                                                                                                                                                                                                                                                   |
| Clave de autenticación de encabezado                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Varios idiomas que se traducen en una sola solicitud | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDK disponibles                                    | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Para ver los SDK disponibles, consulte la [documentación de servicio de Voz](index.yml).                                                                                                                                                    |
| Conexiones de WebSockets                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| API Languages                                     | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Los servicios de Voz son compatibles con el mismo conjunto de idiomas que se describe en el artículo de [referencia de idiomas de API Translator](../translator-speech/languages-reference.md). |
| Marcador y filtro de obscenidad                       | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| .WAV/PCM como entrada                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Otros tipos de archivo como entrada                         | :heavy_minus_sign:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Resultados parciales                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Información de control de tiempo                                       | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                 |
| Id. de correlación                                    | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Modelos de voz personalizados                              | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Los servicios de Voz ofrecen modelos de voz personalizados que le permiten personalizar el reconocimiento de voz con el vocabulario único de su organización.                                                                                                                                           |
| Modelos de traducción personalizados                         | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Si se suscribe a Microsoft Translator Text API, podrá utilizar [Traductor personalizado](https://www.microsoft.com/translator/business/customization/), lo que le permitirá emplear sus propios datos para conseguir traducciones más precisas.                                                 |

## <a name="migration-strategies"></a>Estrategias de migración

Si su organización o usted tienen aplicaciones en desarrollo o producción que usan la API Translator Speech, deben actualizarlas para que usen el servicio Voz. Vea la documentación del [servicio Voz](index.yml) para obtener información sobre SDK disponibles, ejemplos de código y tutoriales. Tenga en cuenta lo siguiente cuando esté migrando:

* Los servicios de Voz no ofrecen un punto de conexión global. Determine si la aplicación funciona eficazmente con un único punto de conexión regional para todo su tráfico. Si no lo hace, use la ubicación geográfica para determinar el punto de conexión más eficaz.

* Si la aplicación usa conexiones de larga duración y no puede usar un SDK disponible, puede usar una conexión de WebSockets. Administre el límite de tiempo de espera de 10 minutos volviendo a conectarse en los momentos adecuados.

* Si la aplicación utiliza las API Translator Text y Translator Speech para habilitar los modelos de traducción personalizada, puede agregar los identificadores "Categoría" directamente mediante el servicio de voz.

* A diferencia de Translator Speech API, los servicios de Voz pueden realizar traducciones en varios idiomas en una sola solicitud.

## <a name="next-steps"></a>Pasos siguientes

* [Prueba gratuita de los servicios de Voz](get-started.md)
* [Inicio rápido: Reconocimiento de voz en una aplicación de UWP mediante el SDK de Voz](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Otras referencias

* [¿Qué es el servicio Voz?](overview.md)
* [Documentación de los servicios de Voz y Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
