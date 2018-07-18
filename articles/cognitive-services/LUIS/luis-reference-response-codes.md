---
title: 'Códigos de respuesta HTTP de Language Understanding (LUIS) API: Azure | Microsoft Docs'
titleSuffix: Azure
description: Comprender qué códigos de respuesta HTTP se devuelven de las API de punto de conexión y creación de LUIS
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 04/16/2018
ms.author: v-geberr
ms.openlocfilehash: 9c7381d9dc2ecf302c85c6b4f1f24b24a28d9ebe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380894"
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