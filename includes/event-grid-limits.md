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
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376925"
---
Los límites siguientes se aplican a los temas de sistema de Azure Event Grid y temas personalizados, *no* dominios del evento.

| Recurso | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado (entrada) | 5000 eventos por segundo y tema |
| Solicitudes de publicación | 250 por segundo |
| Tamaño del evento | Compatibilidad con 64 KB en General (GA) de disponibilidad. Compatibilidad con 1 MB está actualmente en versión preliminar. |

Los límites siguientes se aplican a solo los dominios de eventos.

| Recurso | Límite |
| --- | --- |
| Temas por dominio de eventos | 1000 durante la versión preliminar pública |
| Suscripciones de eventos por tema dentro de un dominio | 50 durante la versión preliminar pública |
| Suscripciones de eventos de ámbito de dominio | 50 durante la versión preliminar pública |
| Publicación de velocidad para un dominio de eventos (entrada) | 5000 eventos por segundo durante la versión preliminar pública |
| Solicitudes de publicación | 250 por segundo |