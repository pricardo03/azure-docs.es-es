---
title: Novedades de Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: Este artículo contiene las últimas novedades sobre Language Understanding.
author: diberry
manager: nitinme
ms.custom: experiment-luis-0519
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: diberry
ms.openlocfilehash: 3a32499a85b1cd782c808166c516be7ad9c78913
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2019
ms.locfileid: "70813286"
---
# <a name="whats-new-in-language-understanding"></a>Novedades de Language Understanding

Conozca las novedades del servicio. Estos elementos pueden ser notas de la versión, vídeos, entradas de blogs y otros tipos de información. Marque esta página para mantenerse actualizado con el servicio.  

## <a name="release-notes"></a>Notas de la versión 

### <a name="september-3-2019"></a>3 de septiembre de 2019

* Recurso de creación de Azure: [migrar ahora](luis-migration-authoring.md).
    * 500 aplicaciones por recurso de Azure
    * 100 versiones por aplicación
* Compatibilidad del turco con las entidades precompiladas
* Compatibilidad del italiano con datetimeV2

### <a name="july-23-2019"></a>23 de julio de 2019

* Actualización de [Recognizers-Text](https://github.com/microsoft/Recognizers-Text/releases/tag/dotnet-v1.2.3) a 1.2.3
    * Reconocedores de edad, temperatura, dimensión y moneda en italiano.
    * Mejora en el reconocimiento de festividades en inglés para calcular correctamente las fechas basadas en el Día de Acción de Gracias.
    * Mejoras en DateTime en francés para reducir los falsos positivos de entidades que no son de fecha y hora.
    * Compatibilidad con el año natural, escolar o fiscal y acrónimos en DateRange en inglés.
    * Reconocimiento de PhoneNumber mejorado en chino y japonés.
    * Compatibilidad mejorada con NumberRange en inglés.
    * Mejoras en el rendimiento.

### <a name="june-24-2019"></a>24 de junio de 2019

* [Entidad creada previamente OrdinalV2](luis-reference-prebuilt-ordinal-v2.md) para permitir la ordenación, como siguiente, anterior y último. Referencia cultural inglesa únicamente.

### <a name="may-6-2019---build-conference"></a>6 de mayo de 2019: conferencia Build

Las siguientes características se presentaron en el congreso Build 2019:

* [Versión preliminar de la guía de migración de la API v3](luis-migration-api-v3.md)
* [Panel de análisis mejorado](luis-how-to-use-dashboard.md)
* [Dominios creados previamente mejorados](luis-reference-prebuilt-domains.md) 
* [Entidades de lista dinámica](luis-migration-api-v3.md#dynamic-lists-passed-in-at-prediction-time)
* [Entidades externas](luis-migration-api-v3.md#external-entities-passed-in-at-prediction-time)

## <a name="blogs"></a>Blogs

[Bot Framework](https://blog.botframework.com/)

## <a name="videos"></a>Vídeos

### <a name="2019-build-videos"></a>Vídeos de Build 2019

[How to use Azure Conversational AI to scale your business for the next generation](https://www.youtube.com/watch?v=_k97jd-csuk&feature=youtu.be) (Cómo usar la inteligencia artificial de Azure Conversational para llevar su negocio a la próxima generación)

## <a name="service-updates"></a>Actualizaciones del servicio

[Anuncios de actualización de Azure para Cognitive Services](https://azure.microsoft.com/updates/?product=cognitive-services)
