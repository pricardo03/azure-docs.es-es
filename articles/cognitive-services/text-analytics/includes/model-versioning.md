---
title: Control de versiones de los modelos
titleSuffix: Azure Cognitive Services
description: Especificar versiones del modelo en los puntos de conexión V3
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/25/2019
ms.author: aahi
ms.openlocfilehash: 5a06e26e5f1640024e343c714db3df134422115c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488656"
---
La versión 3 de Text Analytics API le permite elegir el modelo de Text Analytics que se usa en los datos. Use el parámetro opcional `model-version` para seleccionar una versión del modelo en sus solicitudes. Si no se especifica este parámetro, la API usará de forma predeterminada `latest`, la versión de modelo estable más reciente.

Versiones disponibles del modelo:
* `2019-10-01` (`latest`)

Cada respuesta de los puntos de conexión V3 incluye un campo `model-version` que especifica la versión del modelo que se usó.

```json
{
    “documents”: […]
    “errors”: []
    “model-version”: “2019-10-01”
}
```
