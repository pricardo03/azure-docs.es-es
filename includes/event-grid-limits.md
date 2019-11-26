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
ms.openlocfilehash: 890095718833c90a6764d0799d2081b393b1d0bd
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72887842"
---
Los límites siguientes se aplican a temas del sistema y temas personalizados de Azure Event Grid, *no* a dominios de eventos.

| Resource | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado (entrada) | 5000 eventos por segundo y tema |
| Solicitudes de publicación | 250 por segundo |
| Tamaño del evento | Compatibilidad con 64 KB en disponibilidad general (GA). La compatibilidad con 1 MB se encuentra actualmente en versión preliminar. |

Los límites siguientes se aplican solo a dominios de eventos.

| Resource | Límite |
| --- | --- |
| Temas por dominio de eventos | 100 000 |
| Suscripciones de eventos por tema dentro de un dominio | 500 |
| Suscripciones de eventos del ámbito de dominio | 50 |
| Tasa de publicación para un dominio de eventos (entrada) | 5000 eventos por segundo |
| Solicitudes de publicación | 250 por segundo |
| Dominios de eventos por suscripción de Azure | 100 |