---
title: 'Códigos de respuesta HTTP de API: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los códigos de respuesta HTTP que devuelve QnA Maker API. Esto ayudará a resolver los errores.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 9eb5f1aefc8a36727e837af79f553108f624fc70
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207979"
---
# <a name="qna-maker-api-http-response-codes"></a>Códigos de respuesta HTTP de QnA Maker API
Las API de [administración](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) de predicción devuelven códigos de respuesta HTTP. Mientras que los mensajes de respuesta incluyen información específica de una solicitud, el código de estado de respuesta HTTP es general. 

### <a name="management"></a>Administración

|Código|Explicación|
|:--|--|
|2xx|Correcto|
|400|Los parámetros de la solicitud no son correctos, lo que significa que faltan parámetros necesarios, o bien que tienen un formato incorrecto o son demasiado grandes|
|400|El cuerpo de la solicitud es incorrecto, lo que significa que falta el código JSON, o bien que tiene un formato incorrecto o es demasiado grande|
|401|Clave no válida|
|403|Prohibido: no tiene los permisos correctos|
|404|KB no existe|
