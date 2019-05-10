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
ms.openlocfilehash: 3265477108b7e74d65050408add6c5d5c94b4852
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233898"
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
|Calendario|Calendario es algo acerca de las reuniones personales y citas, _no_ eventos públicos (como las programaciones de copa del mundo, Seattle eventos calendarios o genérico (por ejemplo, qué día es hoy en día, ¿qué otoño empieza, cuando es el día de la mano de obra).|
|Comunicación|Las solicitudes para realizar llamadas, enviar mensajes o mensajes instantáneos, buscar y agregar contactos y varias otras solicitudes relacionadas con la comunicación (generalmente saliente). Nombre de contacto solo consultas no pertenecen al dominio de comunicación.|
|Email|Correo electrónico es un subdominio del dominio de comunicación. Principalmente contiene las solicitudes para enviar y recibir mensajes a través de mensajes de correo electrónico.|
|HomeAutomation|El dominio HomeAutomation proporciona intenciones y entidades relacionadas para controlar los dispositivos inteligentes de inicio. Es compatible principalmente con el comando de control relacionada con las luces y aire acondicionado, pero tiene algunas capacidades de generalización para otros aparatos eléctricos.|
|Notas|Tenga en cuenta de dominio proporciona intenciones y entidades para crear notas y anotar elementos para los usuarios.|
|Lugares|Lugares incluyen las empresas, instituciones, restaurantes, espacios públicos y las direcciones. El dominio es compatible con el lugar de buscar y le pregunta sobre la información de un lugar público como ubicación, horas y distancia operativos.|
|RestaurantReservation|Dominio de reserva de restaurante es compatible con las intenciones para controlar las reservas para restaurantes.|
|ToDo|Dominio de la lista de tareas proporciona tipos de listas de tareas para que los usuarios agregar, marcar y eliminar sus tareas pendientes.|
|Suministros|Utilidades es un dominio general entre LUIS todos los modelos creados previamente que contiene las intenciones y grabaciones de voz en escenarios de diferencia comunes.|
|Tiempo|Dominio de tiempo se centra en la comprobación condiciones meteorológicas y Asesorías con la ubicación y la hora o la comprobación de tiempo por las condiciones meteorológicas.|
|Web|El dominio de Web proporciona la intención y las entidades de búsqueda para un sitio Web.|
