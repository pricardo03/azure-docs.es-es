---
title: Migración desde Google Maps | Microsoft Docs
description: Un tutorial sobre cómo migrar de Google Maps a Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: ea9afc413a7fb187042c9d832fa1c16802993eed
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475370"
---
# <a name="migrate-from-google-maps"></a>Migración desde Google Maps

En este tutorial se proporciona información sobre cómo migrar aplicaciones web, para dispositivos móviles y basadas en servidor desde Google Maps a la plataforma Microsoft Azure Maps. En este tutorial se incluyen ejemplos de código comparativos, sugerencias de migración y procedimientos recomendados para migrar a Azure Maps.

## <a name="azure-maps-platform-overview"></a>Introducción a la plataforma Azure Maps

Azure Maps ofrece a los desarrolladores de todos los sectores potentes funcionalidades geoespaciales, integradas con datos de mapas actualizados de forma periódica para proporcionar contexto geográfico a aplicaciones web y para dispositivos móviles. Azure Maps tiene un conjunto de API REST compatibles con Azure One API para mapas, búsqueda, rutas, tráfico, zonas horarias, geolocalización, geovalla, datos de mapas y operaciones espaciales, que incluye los SDK web y Android que hacen que el desarrollo sea sencillo, flexible y portable entre varias plataformas.

## <a name="high-level-platform-comparison"></a>Comparación general de las plataformas

En la tabla siguiente se proporciona una lista general de las características de Google Maps y la compatibilidad relativa con esas características en Azure Maps. Esta lista no incluye características adicionales de Azure Maps como accesibilidad, API de geovallas, isocronas, operaciones espaciales, acceso directo a iconos de mapa, servicios de lote y comparaciones de cobertura de datos (es decir, cobertura de imágenes).

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

Google Maps proporciona autenticación básica basada en claves. Azure Maps proporciona autenticación básica basada en claves, así como autenticación de alta seguridad de Azure Active Directory.

## <a name="licensing-considerations"></a>Consideraciones acerca de las licencias

Al migrar a Azure Maps desde Google Maps, se deben tener en cuenta los aspectos siguientes relacionados con las licencias.

- Azure Maps cobra por el uso de mapas interactivos en función del número de iconos de mapa cargados, mientras que Google Maps cobra por la carga del control de mapa. En los SDK interactivos de Azure Maps, los iconos de mapa se almacenan automáticamente en caché para reducir el costo para el desarrollador. Se genera una transacción de Azure Maps por cada 15 iconos de mapa que se cargan. Los SDK interactivos de Azure Maps usan iconos de 512 píxeles y, de media, generan una o menos transacciones por cada vista de página.
- Por lo general resulta mucho más rentable reemplazar las imágenes de mapas estáticos de los servicios web de Google Maps con el SDK web de Azure Maps, ya que usa iconos de mapa y, a menos que el usuario aumente y acerque el mapa, solo generarán una fracción de una transacción por carga de mapa. El SDK web de Azure Maps tiene opciones para deshabilitar el movimiento panorámico y el zoom. Además, el SDK web de Azure Maps ofrece muchas más opciones de visualización que las de un servicio web de mapas estáticos.
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
