---
title: Compatibilidad de localización en Azure Maps | Microsoft Docs
description: Conozca los idiomas admitidos en los servicios de Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 1928185521419006a487a933e2ecba79894a09d3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686781"
---
# <a name="localization-support-in-azure-maps"></a>Compatibilidad de localización en Azure Maps

Azure Maps es compatible con diversos lenguajes y las vistas basadas en el país o región. Este artículo proporciona los idiomas admitidos y las vistas para ayudarle a dirigir su implementación de Azure Maps.


## <a name="azure-maps-supported-languages"></a>Idiomas admitidos en Azure Maps

Los servicios de Azure Maps se han localizado en diversos idiomas. En la tabla siguiente se proporcionan los códigos de idioma admitidos para cada servicio.  
  

| ID         | NOMBRE                   |  Mapas | Search | Enrutamiento | Incidentes de tráfico | Control de mapa JS | Zona horaria |
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
| en-US      | Inglés (Estados Unidos)          |   ✓   |    ✓   |    ✓    |         ✓         |        ✓       |     ✓     |
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


## <a name="azure-maps-supported-views"></a>Vistas admitidos en Azure Maps

Azure asigna la vista parámetro (también denominado "parámetro de región del usuario") es un código de país de 2 letras ISO 3166 que se muestran las asignaciones correctas para ese país o región especifica qué conjunto de caso conflictivas bordes y las etiquetas se muestran en el mapa.  De forma predeterminada el parámetro View se establece en **"Unified"**.  Países o regiones que no están en la lista de vista predeterminado será la vista "Unified". Es su responsabilidad para determinar la ubicación de los usuarios y, a continuación, establezca el parámetro View correctamente para esa ubicación. Se debe usar el parámetro View en Azure Maps conforme a las leyes aplicables, incluidas aquellas está disponible en cuanto a asignación del país donde mapas, imágenes y otro contenido de datos y de terceros que está autorización para tener acceso a través de Azure Maps.

En la tabla siguiente proporciona vistas admitidas.

| Ver         | DESCRIPCIÓN                            |  Mapas | Search | Control de mapa JS |
|--------------|----------------------------------------|:-----:|:------:|:--------------:|
| AE           | Emiratos Árabes Unidos (vista árabe)    |   ✓   |        |     ✓          |
| AR           | Argentina (vista de Argentina)           |   ✓   |    ✓   |     ✓          |
| BH           | Baréin (vista árabe)                 |   ✓   |        |     ✓          |
| IN           | India (vista de la India)                    |   ✓   |   ✓     |     ✓          |
| IQ           | Irak (vista árabe)                    |   ✓   |        |     ✓          |
| JO           | Jordania (vista árabe)                  |   ✓   |        |     ✓          |
| KW           | Kuwait (vista árabe)                  |   ✓   |        |     ✓          |
| LB           | Líbano (vista árabe)                 |   ✓   |        |     ✓          |
| MA           | Marruecos (vista Maroko)                |   ✓   |   ✓     |     ✓          |
| OM           | Omán (vista árabe)                    |   ✓   |        |     ✓          |
| PK           | Pakistán (pakistaní vista)              |   ✓   |    ✓    |     ✓          |
| PS           | Autoridad Palestina (vista árabe)    |   ✓   |        |     ✓          |
| QA           | Qatar (vista árabe)                   |   ✓   |        |     ✓          |
| SA           | Arabia Saudí (vista árabe)            |   ✓   |        |     ✓          |
| SY           | Siria (vista árabe)                   |   ✓   |        |     ✓          |
| YE           | Yemen (vista árabe)                   |   ✓   |        |     ✓          |
| Unificada      | Vista unificada (otros)                  |   ✓   |   ✓     |     ✓          |
