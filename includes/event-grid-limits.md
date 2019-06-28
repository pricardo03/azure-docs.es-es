---
title: archivo de inclusión
description: archivo de inclusión
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 05/22/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 3f94481e6a8550479788d92c744327e1dc3b58c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66376925"
---
Los límites siguientes se aplican a temas del sistema y temas personalizados de Azure Event Grid, *no* a dominios de eventos.

| Recurso | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado (entrada) | 5000 eventos por segundo y tema |
| Solicitudes de publicación | 250 por segundo |
| Tamaño del evento | Compatibilidad con 64 KB en disponibilidad general (GA). La compatibilidad con 1 MB se encuentra actualmente en versión preliminar. |

Los límites siguientes se aplican solo a dominios de eventos.

| Recurso | Límite |
| --- | --- |
| Temas por dominio de eventos | 1000 durante la versión preliminar pública |
| Suscripciones de eventos por tema dentro de un dominio | 50 durante la versión preliminar pública |
| Suscripciones de eventos del ámbito de dominio | 50 durante la versión preliminar pública |
| Tasa de publicación para un dominio de eventos (entrada) | 5000 eventos por segundo durante la versión preliminar pública |
| Solicitudes de publicación | 250 por segundo |