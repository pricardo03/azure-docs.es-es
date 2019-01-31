---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 79ae38db73d55021572d04f693e5cb809e9bd056
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228974"
---
Los datos devueltos con el valor esperado y los márgenes superior e inferior predeterminados. En la práctica, puede definir un parámetro [sensibilidad] y luego usar (ExpectedValue + sensibilidad * UpperMargin) como el límite superior y (ExpectedValue - sensibilidad * LowerMargin) como el límite inferior para poder ajustar por su cuenta el punto de anomalía. El valor de [sensibilidad] debe ser mayor que 1. A continuación, se presentan algunos diagramas de ajuste.

> [!NOTE]
> Los diagramas no se generan mediante la aplicación de ejemplo. Se crean mediante una herramienta independiente con la aplicación de ejemplo.

![Ajuste: sensibilidad = 1,0](../media/sensitivity_1.png)
![Ajuste: sensibilidad = 1,5](../media/sensitivity_1.5.png)
![Ajuste: sensibilidad = 2](../media/sensitivity_2.png)
![Ajuste: sensibilidad = 3,5](../media/sensitivity_3.5.png)
