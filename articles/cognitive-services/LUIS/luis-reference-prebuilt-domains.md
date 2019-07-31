---
title: Referencia de dominio creado previamente
titleSuffix: Azure
description: La referencia de dominios creados previamente, que son colecciones creadas previamente de intenciones y entidades de Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: b615185472ede25fd6ceef9cc3e7325375e16ac7
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846678"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Referencia de dominio creado previamente para la aplicación de LUIS
Esta referencia ofrece información sobre los [dominios creados previamente](luis-how-to-use-prebuilt-domains.md), que son colecciones creadas previamente de intenciones y entidades que LUIS ofrece.

Los [dominios personalizados](luis-how-to-start-new-app.md), por el contrario, se inician sin intenciones ni modelos. Puede agregar cualquier intención y entidad de dominios creados previamente a un modelo personalizado.

# <a name="supported-domains-across-cultures"></a>Dominios admitidos en distintas referencias culturales

La única referencia cultural compatible es el inglés. 

<!--

The table below summarizes the currently supported domains. Support for English is usually more complete than others.


| Entity Type       | EN-US      | ZH-CN   | DE    | FR     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| Calendar    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -      | -    | -    | -     | -  |
| Communication   | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Email           | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| HomeAutomation           | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Notes      | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Places    | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| RestaurantReservation   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo_IPA        | ✓    | ✓       | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Utilities          | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Weather        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Web    | ✓    | -        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
||||||||||||| 

-->

<br><br>

|Tipo de entidad|description|
|--|--|
|Calendario|Calendario no va sobre reuniones personales y citas, _ni_ sobre eventos públicos (como programaciones de la copa del mundo, calendarios de eventos de Seattle) o calendarios genéricos (por ejemplo, qué día es hoy, cuándo comienza el otoño o cuándo es el día del trabajo).|
|Comunicación|Solicitudes para realizar llamadas, enviar mensajes de texto o mensajes instantáneos, buscar y agregar contactos y otras diversas solicitudes relacionadas con la comunicación (generalmente saliente). Las consultas de solo nombres de contacto no pertenecen al dominio de comunicación.|
|Email|Correo electrónico es un subdominio del dominio Comunicación. Contiene principalmente solicitudes para enviar y recibir mensajes por correo electrónico.|
|HomeAutomation|El dominio HomeAutomation ofrece intenciones y entidades relacionadas con el control de dispositivos domésticos inteligentes. Admite principalmente el comando de control relacionado con las luces y el aire acondicionado, pero tiene algunas capacidades de generalización para otros aparatos eléctricos.|
|Notas|El dominio Notas proporciona intenciones y entidades para crear notas y escribir elementos para los usuarios.|
|Lugares|los lugares incluyen empresas, instituciones, restaurantes, espacios públicos y direcciones. El dominio admite la búsqueda de lugares y la solicitud de información sobre un lugar público, como la ubicación, las horas de servicio y la distancia.|
|RestaurantReservation|El dominio de reserva de restaurante admite intenciones para controlar las reservas de restaurantes.|
|ToDo|El dominio ToDo proporciona listas de tipos de tareas para que los usuarios agreguen, marquen y eliminen sus elementos de tareas pendientes.|
|Sectores públicos|El dominio Sectores públicos es un dominio general entre todos los modelos precompilados de LUIS que contiene intenciones y expresiones comunes en diferentes escenarios.|
|Tiempo|El dominio Tiempo se centra en la comprobación y las advertencias de las condiciones meteorológicas con la ubicación y la hora, o en la comprobación de la hora por las condiciones meteorológicas.|
|Web|El dominio Web proporciona la intención y las entidades para la búsqueda de un sitio web.|
