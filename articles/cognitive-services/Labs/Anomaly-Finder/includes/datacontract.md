---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: e37d3ef5b6f65ad31bc19f9f8c15350014d1c9ad
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381327"
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

