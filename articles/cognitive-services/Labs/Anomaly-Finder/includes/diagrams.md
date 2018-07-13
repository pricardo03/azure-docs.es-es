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
ms.openlocfilehash: df7326cb8e671d0f71924e813a1354dfef1e20c7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381291"
---
Los datos devueltos con el valor esperado y los márgenes superior e inferior predeterminados. En la práctica, puede definir un parámetro [sensibilidad] y luego usar (ExpectedValue + sensibilidad * UpperMargin) como el límite superior y (ExpectedValue - sensibilidad * LowerMargin) como el límite inferior para poder ajustar por su cuenta el punto de anomalía. El valor de [sensibilidad] debe ser mayor que 1. A continuación, se presentan algunos diagramas de ajuste.

> [!NOTE]
> Los diagramas no se generan mediante la aplicación de ejemplo. Se crean mediante una herramienta independiente con la aplicación de ejemplo.

![Ajuste: sensibilidad = 1,0](../media/sensitivity_1.png)
![Ajuste: sensibilidad = 1,5](../media/sensitivity_1.5.png)
![Ajuste: sensibilidad = 2](../media/sensitivity_2.png)
![Ajuste: sensibilidad = 3,5](../media/sensitivity_3.5.png)