---
title: 'Códigos de respuesta HTTP de API: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los códigos de respuesta HTTP que devuelve QnA Maker API. Esto ayudará a resolver los errores.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/20/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 0416b282a869536b0cea591ba39d7a3804697bac
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446535"
---
# <a name="qna-maker-api-http-response-codes"></a>Códigos de respuesta HTTP de QnA Maker API
Las API de [administración](https://go.microsoft.com/fwlink/?linkid=2092179) de predicción devuelven códigos de respuesta HTTP. Mientras que los mensajes de respuesta incluyen información específica de una solicitud, el código de estado de respuesta HTTP es general. 

### <a name="management"></a>Administración

|Código|Explicación|
|:--|--|
|2xx|Correcto|
|400|Los parámetros de la solicitud no son correctos, lo que significa que faltan parámetros necesarios, o bien que tienen un formato incorrecto o son demasiado grandes|
|400|El cuerpo de la solicitud es incorrecto, lo que significa que falta el código JSON, o bien que tiene un formato incorrecto o es demasiado grande|
|401|Clave no válida|
|403|Prohibido: no tiene los permisos correctos|
|404|KB no existe|
|410|Esta API está en desuso y ya no está disponible|
