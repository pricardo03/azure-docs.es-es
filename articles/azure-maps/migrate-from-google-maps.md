---
title: 'Tutorial: Migración de Google Maps a Azure Maps | Microsoft Azure Maps'
description: Tutorial sobre la migración de Google Maps a Microsoft Azure Maps. Este documento le guiará por los pasos para cambiar a las API y los SDK de Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 1f6f282406c6813b2b126c300f21bda21e8f9464
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76988981"
---
# <a name="migrate-from-google-maps-to-azure-maps"></a>Migración de Google Maps a Azure Maps

En este tutorial se proporciona información sobre cómo migrar aplicaciones web, para dispositivos móviles y basadas en servidor desde Google Maps a la plataforma Microsoft Azure Maps. En este tutorial se incluyen ejemplos de código comparativos, sugerencias de migración y procedimientos recomendados para migrar a Azure Maps.

## <a name="azure-maps-platform-overview"></a>Introducción a la plataforma Azure Maps

Azure Maps ofrece a los desarrolladores de todos los sectores potentes funcionalidades geoespaciales, integradas con datos de mapas actualizados de forma periódica para proporcionar contexto geográfico a aplicaciones web y para dispositivos móviles. Azure Maps tiene un conjunto de API REST compatible con Azure One API. Esas API REST ofrecen mapas, búsqueda, enrutamiento, tráfico, zonas horarias, geolocalización, geovallas, datos de mapas, climatología, movilidad y operaciones espaciales. Las operaciones vienen acompañadas de los SDK web y para Android para hacer el desarrollo sencillo, flexible y portátil entre varias plataformas.

## <a name="high-level-platform-comparison"></a>Comparación general de las plataformas

En la tabla siguiente se proporciona una lista general de las características de Azure Maps, que se corresponden con las características de Azure Maps. Esta lista no muestra todas las características de Azure Maps. Azure Maps incluye características adicionales como accesibilidad, API de geovallas, isocronas, operaciones espaciales, acceso directo a iconos de mapa, servicios de lotes y comparaciones de cobertura de datos (es decir, cobertura de imágenes).

| Característica de Google Maps         | Compatibilidad con Azure Maps                     |
|-----------------------------|:--------------------------------------:|
| SDK web                     | ✓                                      |
| SDK de Android                 | ✓                                      |
| SDK de iOS                     | Planeado                                |
| API REST del servicio           | ✓                                      |
| Direcciones (cálculo de ruta)        | ✓                                      |
| Matriz de distancia             | ✓                                      |
| Elevation                   | Planeado                                |
| Geocodificación (directa o inversa) | ✓                                      |
| Geolocalización                 | N/D                                    |
| Búsqueda de lugares               | ✓                                      |
| Detalles de lugares              | N/D: sitio web y número de teléfono disponible |
| Fotos de lugares               | N/D                                    |
| Autocompletar para lugares          | ✓                                      |
| Mapas estáticos                 | ✓                                      |
| Vista de calle estática          | N/D                                    |
| Zona horaria                   | ✓                                      |
| API insertada de mapas           | N/D                                    |
| Direcciones URL de mapas                    | N/D                                    |

Google Maps proporciona una autenticación básica basada en claves. Azure Maps proporciona autenticación básica basada en claves y la autenticación de alta seguridad de Azure Active Directory.

## <a name="licensing-considerations"></a>Consideraciones acerca de las licencias

Al migrar a Azure Maps desde Google Maps, se deben tener en cuenta los aspectos siguientes relacionados con las licencias.

- Azure Maps cobra por el uso de mapas interactivos en función del número de mosaicos de mapa cargados, mientras que Google Maps cobra por la carga del control de mapa. En los SDK interactivos de Azure Maps, los iconos de mapa se almacenan automáticamente en caché para reducir el costo para el desarrollador. Se genera una transacción de Azure Maps por cada 15 iconos de mapa que se cargan. Los SDK interactivos de Azure Maps usan iconos de 512 píxeles y, de media, generan una o menos transacciones por cada vista de página.
- A menudo es mucho más rentable reemplazar las imágenes de mapas estáticas de los servicios web de Google Maps por el SDK web de Azure Maps. El SDK web de Azure Maps usa mosaicos de mapa y, a menos que el usuario desplace lateralmente el mapa y lo acerque, con frecuencia solo se generará una fracción de una transacción por carga del mapa. El SDK web de Azure Maps tiene opciones para deshabilitar el movimiento panorámico y el zoom. Además, el SDK web de Azure Maps ofrece muchas más opciones de visualización que las de un servicio web de mapas estáticos.
- Azure Maps permite almacenar los datos de su plataforma en Azure. También se pueden almacenar en caché en otro lugar durante un máximo de seis meses, según las [condiciones de uso](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46).

Estos son algunos recursos relacionados con Azure Maps:

- [Página de precios de Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/)
- [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/?service=azure-maps)
- [Condiciones de uso de Azure Maps](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) (incluido en los términos de Microsoft Online Services)
- [Elección del plan de tarifa adecuado de Azure Maps](https://docs.microsoft.com/azure/azure-maps/choose-pricing-tier)

## <a name="suggested-migration-plan"></a>Plan de migración sugerido

A continuación se describe un plan de migración general.

1. Recopile los SDK y servicios de Google Maps que se usan en la aplicación y compruebe que Azure Maps proporciona SDK y servicios alternativos a los que puede realizar la migración.
2. Cree una suscripción de Azure (si todavía no tiene una) en [https://azure.com](https://azure.com).
3. Cree una cuenta de Azure Maps ([documentación](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys))y una clave de autenticación o de Azure Active Directory ([documentación](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)).
4. Migre el código de la aplicación.
5. Pruebe la aplicación migrada.
6. Implemente la aplicación migrada en producción.

## <a name="azure-maps-technical-resources"></a>Recursos técnicos para Azure Maps

Esta es una lista de recursos técnicos útiles para Azure Maps.

- Información general: [https://azure.com/maps](https://azure.com/maps)
- Documentación: [https://aka.ms/AzureMapsDocs](https://aka.ms/AzureMapsDocs)
- Ejemplos de código del SDK web: [https://aka.ms/AzureMapsSamples](https://aka.ms/AzureMapsSamples)
- Foros para desarrolladores: [https://aka.ms/AzureMapsForums](https://aka.ms/AzureMapsForums)
- Vídeos: [https://aka.ms/AzureMapsVideos](https://aka.ms/AzureMapsVideos)
- Blog: [https://aka.ms/AzureMapsBlog](https://aka.ms/AzureMapsBlog)
- Comentarios de Azure Maps (UserVoice): [https://aka.ms/AzureMapsFeedback](https://aka.ms/AzureMapsFeedback)

## <a name="migration-support"></a>Compatibilidad con la migración

Los desarrolladores pueden buscar compatibilidad con la migración a través de los [foros](https://aka.ms/AzureMapsForums) o de una de las muchas opciones de soporte técnico de Azure: [https://azure.microsoft.com/support/options](https://azure.microsoft.com/support/options)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo migrar la aplicación de Google Maps con estos artículos:

> [!div class="nextstepaction"]
> [Migración de una aplicación web](migrate-from-google-maps-web-app.md)

> [!div class="nextstepaction"]
> [Migración de una aplicación de Android](migrate-from-google-maps-android-app.md)

> [!div class="nextstepaction"]
> [Migración de un servicio web](migrate-from-google-maps-web-services.md)
