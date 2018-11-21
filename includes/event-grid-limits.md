---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 04/30/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2425d9455606f5eabba4b89cfa238b7a928be30e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285683"
---
####

Los límites siguientes se aplican a los temas de Event Grid del sistema y los temas personalizados, *no* a los dominios de eventos.

| Recurso | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado (entrada) | 5000 eventos por segundo y tema |

####

Los límites siguientes corresponden a Dominios de eventos únicamente.

| Recurso | Límite |
| --- | --- |
| Temas por dominio de eventos | 1000 durante la versión preliminar pública |
| Suscripciones de eventos por tema dentro de un dominio | 50 durante la versión preliminar pública |
| Suscripciones de eventos del ámbito de dominio | 50 durante la versión preliminar pública |
| Tasa de publicación para un dominio de eventos (entrada) | 5000 eventos por segundo durante la versión preliminar pública |