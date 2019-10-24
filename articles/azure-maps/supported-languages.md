---
title: Soporte de localización en Azure Maps | Microsoft Docs
description: Conozca los idiomas admitidos en los servicios de Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: b3382cffdc41685f8329a640aaf6c6c526375a83
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299843"
---
# <a name="localization-support-in-azure-maps"></a>Soporte de localización en Azure Maps

Azure Maps admite varios idiomas y vistas según el país o región. En este artículo se proporcionan los idiomas y las vistas compatibles para que pueda realizar su implementación de Azure Maps.


## <a name="azure-maps-supported-languages"></a>Idiomas admitidos en Azure Maps

Los servicios de Azure Maps se han localizado en diversos idiomas. En la tabla siguiente se proporcionan los códigos de idioma admitidos para cada servicio.  
  

| id         | NOMBRE                   |  Mapas | Search | Enrutamiento | Incidentes de tráfico | Control de mapa JS | Zona horaria |
|------------|------------------------|:-----:|:------:|:-------:|:-----------------:|:--------------:|:---------:|
| af-ZA      | Afrikáans              |       |    ✓   |    ✓    |                   |                |     ✓     |
| ar-SA      | Árabe                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| eu-ES      | Vasco                 |       |    ✓   |         |                   |                |     ✓     |
| bg-BG      | Búlgaro              |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| ca-ES      | Catalán                |       |    ✓   |         |                   |                |     ✓     |
| zh-HanS    | Chino (simplificado)   |       |  zh-CN |         |                   |                |     ✓     |
| zh-HanT    | Chino (tradicional)  | zh-TW |  zh-TW |  zh-TW  |                   |      Zh-TW     |     ✓     |
| hr-HR      | Croata               |       |    ✓   |         |                   |                |     ✓     |
| cs-CZ      | Checo                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| da-DK      | Danés                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| nl-NL      | Neerlandés                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| nl-BE      | Neerlandés (belga)        |       |    ✓   |         |                   |                |     ✓     |
| en-AU      | Inglés (Australia)    |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| en-NZ      | Inglés (Nueva Zelanda)  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| en-GB      | Inglés (Gran Bretaña) |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| en-US      | Inglés (EE. UU.)          |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| et-EE      | Estonio               |       |    ✓   |         |         ✓         |                |     ✓     |
| fi-FI      | Finés                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| fr-FR      | Francés                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| fr-CA      | Francés (canadiense)      |       |    ✓   |         |                   |                |     ✓     |
| gl-ES      | Gallego               |       |    ✓   |         |                   |                |     ✓     |
| de-DE      | Alemán                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| el-GR      | Griego                  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| he-IL      | Hebreo                 |       |    ✓   |         |         ✓         |                |     ✓     |
| hi-IN      | Hindi                  |       |        |         |                   |                |     ✓     |
| hu-HU      | Húngaro              |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| id-ID      | Indonesio             |   ✓   |    ✓    |    ✓    |         ✓         |        ✓       |     ✓     |
| it-IT      | Italiano                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ja-JP      | Japonés               |       |        |         |                   |                |     ✓     |
| kk-KZ      | Kazajo                 |       |    ✓   |         |                   |                |     ✓     |
| ko-KR      | Coreano                 |   ✓   |        |    ✓    |                   |        ✓       |     ✓     |
| es-419     | Español (América latina) |       |    ✓   |         |                   |                |     ✓     |
| lv-LV      | Letón                |       |    ✓   |         |         ✓         |                |     ✓     |
| lt-LT      | Lituano             |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ms-MY      | Malayo (latino)          |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| nb-NO      | Noruego bokmal       |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| NGT        | Neutral Ground Truth, idiomas oficiales para todas las regiones en alfabetos locales si están disponibles |   ✓     |        |         |                   |      ✓          |         |
| NGT-Latn   | Neutral Ground Truth, exónimos latinos. Si está disponible, se usará el alfabeto latino |   ✓     |        |         |                   |        ✓         |          |
| pl-PL      | Polaco                 |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| pt-BR      | Portugués (Brasil)    |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| pt-PT      | Portugués (Portugal)  |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| ro-RO      | Rumano               |       |    ✓    |         |         ✓         |                |     ✓     |
| ru-RU      | Ruso                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| sr-Cyrl-RS | Serbio (cirílico)     |       |    Serbio (cirílico) (sr-RS)   |         |                   |                |     ✓     |
| sr-Latn-RS | Serbio (latino)        |       |        |         |                   |                |     ✓     |
| sk-SK      | Eslovaco              |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| sl-SL      | Esloveno              |   ✓   |    ✓   |    ✓    |                   |        ✓       |     ✓     |
| es-ES      | Español                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| es-MX      | Español (México)       |   ✓   |        |    ✓    |                   |        ✓       |     ✓     |
| sv -SE     | Sueco                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| th-TH      | Tailandés                   |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| tr-TR      | Turco                |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
| uk-UA      | Ucraniano               |       |    ✓   |         |                   |                |     ✓     |
| vi-VN      | Vietnamita             |       |    ✓   |         |                   |                |     ✓     |


## <a name="azure-maps-supported-views"></a>Vistas admitidas en Azure Maps

> [!Note]
> El 1 de agosto de 2019 se lanzó Azure Maps en los países o regiones siguientes:
>  * Argentina
>  * India
>  * Marruecos
>  * Pakistán
>
> Después del 1 de agosto de 2019, la configuración del parámetro **Vista** definirá el contenido del mapa devuelto para las nuevas regiones o países enumerados anteriormente. Le recomendamos que se asegure de configurar el parámetro Vista para las API de REST y los SDK que usan sus servicios.
>  
>
>  **API de REST:**
>  
>  Asegúrese de haber configurado el parámetro Vista tal como se ha especificado. El parámetro Vista especifica qué conjunto de contenido con conflictos geopolíticos se devuelve a través de los servicios de Azure Maps. 
>
>  Servicios REST de Azure Maps afectados:
>    
>    * Obtener icono del mapa
>    * Obtener imagen de mapa 
>    * Obtener búsqueda aproximada
>    * Obtener búsqueda de POI
>    * Obtener búsqueda de POI por categoría
>    * Obtener búsqueda cercana
>    * Obtener dirección de búsqueda
>    * Obtener la dirección de búsqueda estructurada
>    * Obtener la dirección de búsqueda inversa
>    * Obtener la dirección de búsqueda inversa entre calles
>    * Búsqueda de POST dentro de geometría
>    * Búsqueda de POST de la vista previa del lote de direcciones
>    * Búsqueda de POST de la vista previa del lote de direcciones inversas
>    * Búsqueda de POST a lo largo de la ruta
>    * Búsqueda de POST aproximada de la vista previa del lote
>
>    
>  **SDK**:
>
>  Asegúrese de haber configurado el parámetro Vista según sea necesario y de tener la última versión de los SDK de Web y Android. SDK afectados:
>
>    * SDK web de Azure Maps
>    * Android SDK para Azure Maps


El parámetro **Vista** de Azure Maps (también denominado "parámetro de región de usuario") es un código de país ISO-3166 de dos letras que mostrará los mapas correctos para ese país o región, especificando qué conjunto de contenido disputado geopolíticamente se devuelve a través de los servicios de Azure Maps, incluidas las fronteras y las etiquetas que se muestran en el mapa. 

De forma predeterminada, el parámetro Vista está establecido en **Unificado**, incluso si no lo ha definido en la solicitud. Es su responsabilidad determinar la ubicación de sus usuarios y luego establecer el parámetro Vista correctamente para esa ubicación. Como alternativa, tiene la opción de establecer "View=Auto", que devolverá los datos del mapa según la dirección IP de la solicitud.  El parámetro Vista en Azure Maps se debe usar de conformidad con las leyes aplicables, incluidas aquellas relacionadas con la asignación del país donde están disponibles los mapas, imágenes y otros datos y contenido de terceros a los que está autorizado a acceder a través de Azure Maps.


La siguiente tabla proporciona vistas compatibles.

| Ver         | DESCRIPCIÓN                            |  Mapas | Search | Control de mapa JS |
|--------------|----------------------------------------|:-----:|:------:|:--------------:|
| AE           | Emiratos Árabes Unidos (vista árabe)    |   ✓   |        |     ✓          |
| AR           | Argentina (vista argentina)           |   ✓   |    ✓   |     ✓          |
| BH           | Baréin (vista árabe)                 |   ✓   |        |     ✓          |
| IN           | India (vista india)                    |   ✓   |   ✓     |     ✓          |
| IQ           | Irak (vista árabe)                    |   ✓   |        |     ✓          |
| JO           | Jordania (vista árabe)                  |   ✓   |        |     ✓          |
| KW           | Kuwait (vista árabe)                  |   ✓   |        |     ✓          |
| LB           | Líbano (vista árabe)                 |   ✓   |        |     ✓          |
| MA           | Marruecos (vista marroquí)                |   ✓   |   ✓     |     ✓          |
| OM           | Omán (vista árabe)                    |   ✓   |        |     ✓          |
| PK           | Pakistán (vista pakistaní)              |   ✓   |    ✓    |     ✓          |
| PS           | Autoridad Nacional Palestina (vista árabe)    |   ✓   |        |     ✓          |
| QA           | Catar (vista árabe)                   |   ✓   |        |     ✓          |
| SA           | Arabia Saudí (vista árabe)            |   ✓   |        |     ✓          |
| SY           | Siria (vista árabe)                   |   ✓   |        |     ✓          |
| YE           | Yemen (vista árabe)                   |   ✓   |        |     ✓          |
| Automático         | Devuelva los datos del mapa según la dirección IP de la solicitud.|   ✓   |    ✓   |     ✓          |
| Unificado      | Vista unificada (otros)                  |   ✓   |   ✓     |     ✓          |
