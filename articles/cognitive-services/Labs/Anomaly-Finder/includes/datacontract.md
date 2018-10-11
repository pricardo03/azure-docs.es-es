---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 9280790f6692096a0b3909c9d1dfab2e94a8c0d7
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48904502"
---
Con [Anomaly Finder API](https://labs.cognitive.microsoft.com/en-us/project-anomaly-finder), puede cargar datos de serie temporal en formato JSON en el punto de conexión de API y luego leer el resultado de la respuesta de la API. Puede cargar los datos de serie temporal, y cada punto de datos incluye:  
* Marca de tiempo: la marca de tiempo del punto de datos. Asegúrese de que usa una cadena de tiempo de fecha UTC; por ejemplo, "2017-08-01T00:00:00Z".
* Valor: la medición de ese punto de datos.

Los resultados se componen de:
* Período: la periodicidad con que se usa la API para detectar anomalías.
* WarningText: información de advertencia posible.
* ExpectedValue: valor de predicción del modelo basado en el aprendizaje.
* IsAnomaly: el resultado que indica si los puntos de datos son anomalías o no en ambas direcciones (picos o DIP).
* IsAnomaly_Neg: el resultado que indica si los puntos de datos son anomalías en una dirección negativa (DIP).
* IsAnomaly_Pos: el resultado que indica si los puntos de datos son anomalías en una dirección positiva (picos).
* UpperMargin: la suma de ExpectedValue y UpperMargin determina el límite superior en que el punto de datos aún se considera normal.
* LowerMargin: (ExpectedValue - LowerMargin) determina el límite inferior en que el punto de datos aún se considera normal.

Los detalles del contrato de datos se pueden encontrar [aquí](../apiref.md).

