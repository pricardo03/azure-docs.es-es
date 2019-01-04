---
title: Migrar de Translator Speech API al servicio Voz
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo migrar sus aplicaciones de Translator Speech API al servicio Voz.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: dec903d8557145701e06ddd588a58eb11fef96f5
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879321"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Migrar de Translator Speech API al servicio Voz

Use este artículo para obtener información sobre cómo migrar sus aplicaciones de Translator Speech API de Microsoft al [servicio Voz](index.yml). Esta guía describe las diferencias entre Translator Speech API y el servicio Voz, y sugiere estrategias para migrar sus aplicaciones.

> [!NOTE]
> El servicio Voz aceptará su clave de suscripción de Translator Speech API. Deberá iniciar una nueva suscripción del servicio Voz.

## <a name="comparison-of-features"></a>Comparación de características

| Característica                                           | Translator Speech API                                  | Speech Service | Detalles                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Traducción a texto                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Traducción a voz                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Punto de conexión global                                   | :heavy_check_mark:                                              | :heavy_minus_sign:                 | El servicio Voz no ofrece actualmente un punto de conexión global. Un punto de conexión global puede dirigir automáticamente el tráfico al punto de conexión regional más cercano, reduciendo la latencia en la aplicación.                                                    |
| Puntos de conexión regionales                                | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Límite de tiempo de conexión                             | 90 minutos                                               | Ilimitado con SDK. 10 minutos con una conexión WebSockets                                                                                                                                                                                                                                                                                   |
| Clave de autenticación de encabezado                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Varios idiomas que se traducen en una sola solicitud | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDK disponibles                                    | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Vea la [Documentación del SDK del servicio Voz](index.yml) para obtener información de SDK disponibles.                                                                                                                                                    |
| Conexiones de WebSockets                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| API Languages                                     | :heavy_check_mark:                                              | :heavy_minus_sign:                 | El servicio Voz es compatible con el mismo conjunto de idiomas que se describe en el artículo de [referencia de idiomas de la API Translator](../translator-speech/languages-reference.md). |
| Marcador y filtro de obscenidad                       | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| .WAV/PCM como entrada                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Otros tipos de archivo como entrada                         | :heavy_minus_sign:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Resultados parciales                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Información de control de tiempo                                       | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                 |
| Id. de correlación                                    | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Modelos de voz personalizados                              | :heavy_minus_sign:                                              | :heavy_check_mark:                 | El servicio Voz ofrece modelos de voz personalizados que le permiten personalizar el reconocimiento de voz con el vocabulario único de su organización.                                                                                                                                           |
| Modelos de traducción personalizados                         | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Si se suscribe a Microsoft Translator Text API, podrá utilizar [Traductor personalizado](https://www.microsoft.com/translator/business/customization/), lo que le permitirá emplear sus propios datos para conseguir traducciones más precisas.                                                 |

## <a name="migration-strategies"></a>Estrategias de migración

Si su organización o usted tienen aplicaciones en desarrollo o producción que usan la API Translator Speech, deben actualizarlas para que usen el servicio Voz. Vea la documentación del [servicio Voz](index.yml) para obtener información sobre SDK disponibles, ejemplos de código y tutoriales. Tenga en cuenta lo siguiente cuando esté migrando:

* El servicio Voz no ofrece actualmente un punto de conexión global. Determine si la aplicación funciona eficazmente con un único punto de conexión regional para todo su tráfico. Si no lo hace, use la ubicación geográfica para determinar el punto de conexión más eficaz.

* Si la aplicación usa conexiones de larga duración y no puede usar un SDK disponible, puede usar una conexión de WebSockets. Administre el límite de tiempo de espera de 10 minutos volviendo a conectarse en los momentos adecuados.

* Si la aplicación utiliza las API Translator Text y Translator Speech para habilitar los modelos de traducción personalizada, puede agregar los identificadores "Categoría" directamente mediante el servicio de voz.

* A diferencia de Translator Speech API, el servicio Voz puede completar las traducciones en varios idiomas en una sola solicitud.

## <a name="next-steps"></a>Pasos siguientes

* [Prueba gratuita del servicio Voz](get-started.md)
* [Inicio rápido: Reconocimiento de voz en una aplicación de UWP mediante el SDK de Voz](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Otras referencias

* [¿Qué es el servicio Voz?](overview.md)
* [Documentación del servicio Voz y del SDK correspondiente](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
