---
title: 'Códigos de respuesta HTTP de Language Understanding (LUIS) API: Azure | Microsoft Docs'
titleSuffix: Azure
description: Comprender qué códigos de respuesta HTTP se devuelven de las API de punto de conexión y creación de LUIS
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/16/2018
ms.author: diberry
ms.openlocfilehash: f005c7f13cd05ce3ca6ce494c4a2670106cb3637
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900988"
---
# <a name="luis-api-http-response-codes"></a>Códigos de respuesta HTTP de LUIS API
Las API de [creación](https://aka.ms/luis-authoring-apis) y [punto de conexión](https://aka.ms/luis-endpoint-apis) devuelven códigos de respuesta HTTP. Mientras que los mensajes de respuesta incluyen información específica de una solicitud, el código de estado de respuesta HTTP es general. 

## <a name="common-status-codes"></a>Códigos de estado comunes
En la tabla siguiente se muestran algunos de los códigos de estado de respuesta HTTP más comunes para las API de [creación](https://aka.ms/luis-authoring-apis) y [punto de conexión](https://aka.ms/luis-endpoint-apis):

|Código|API|Explicación|
|:--|--|--|
|400|Creación, punto de conexión|Los parámetros de la solicitud no son correctos, lo que significa que faltan parámetros necesarios, o bien que tienen un formato incorrecto o son demasiado grandes.|
|400|Creación, punto de conexión|El cuerpo de la solicitud es incorrecto, lo que significa que falta el código JSON, o bien que tiene un formato incorrecto o es demasiado grande.|
|401|Creación|Se usó la clave de la suscripción de punto de conexión en lugar de la clave de creación.|
|401|Creación, punto de conexión|La clave no es válida, tiene un formato incorrecto o está vacía.|
|401|Creación, punto de conexión| La clave no coincide con la región.|
|401|Creación|No es el propietario o un colaborador.|
|401|Creación|El orden de las llamadas API no es válido.|
|403|Creación, punto de conexión|Se superó el límite de cuota de claves mensual total.|
|409|Punto de conexión|La aplicación aún se está cargando.|
|410|Punto de conexión|La aplicación se debe entrenar y publicar de nuevo.|
|414|Punto de conexión|La consulta supera el límite máximo de caracteres.|
|429|Creación, punto de conexión|Se superó el límite de velocidad (solicitudes/segundo)|