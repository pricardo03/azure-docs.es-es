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
ms.openlocfilehash: 443db1b4609e62fb7c57de417e42a2b4d0737ada
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554036"
---
Los límites siguientes se aplican a los temas de sistema de Azure Event Grid y temas personalizados, *no* dominios del evento.

| Recurso | Límite |
| --- | --- |
| Temas personalizados por suscripción de Azure | 100 |
| Suscripciones de eventos por tema | 500 |
| Velocidad de publicación de un tema personalizado (entrada) | 5000 eventos por segundo y tema |

Los límites siguientes se aplican a solo los dominios de eventos.

| Recurso | Límite |
| --- | --- |
| Temas por dominio de eventos | 1000 durante la versión preliminar pública |
| Suscripciones de eventos por tema dentro de un dominio | 50 durante la versión preliminar pública |
| Suscripciones de eventos de ámbito de dominio | 50 durante la versión preliminar pública |
| Publicación de velocidad para un dominio de eventos (entrada) | 5000 eventos por segundo durante la versión preliminar pública |