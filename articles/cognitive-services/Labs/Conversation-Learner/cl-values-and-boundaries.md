---
title: 'Configuración predeterminada de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de la configuración predeterminada de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: aaef6f5498e5a8da65d1c829feae8b3e85dba0fd
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705291"
---
# <a name="default-values-and-boundaries"></a>Límites y valores predeterminados

En este documento se describe la configuración predeterminada de Conversation Learner y los límites clave del servicio.

## <a name="default-configuration"></a>Configuración predeterminada

Parámetro | Valor predeterminado
--- | --- 
Tiempo de espera predeterminado de la sesión | 30 minutos

## <a name="boundaries"></a>Límites

Parámetro | Límite
--- | --- 
API de creación, número máximo de llamadas HTTP al mes | 5M
API de creación, número máximo de llamadas HTTP por segundo | 25
API de sesión, número máximo de llamadas HTTP al mes | 500 KB
API de sesión, número máximo de llamadas HTTP por segundo | 10
Número máximo de entidades (sin programación) personalizadas por modelo | Consulte el [documento acerca de los límites de LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries); en la práctica, el número real puede ser algo menor
Número máximo de entidades precompiladas por modelo | Consulte el [documento acerca de los límites de LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries)
Número máximo de entidades (en total) por modelo | 100
Número máximo de acciones por modelo | 32
Número máximo de cuadros de diálogos de entrenamiento por modelo | 1000
El número máximo de turnos de usuario por cuadro de diálogo de entrenamiento | 100
Número máximo de cuadros de diálogo de registro por modelo | No hay límite preestablecido, pero los cuadros de diálogo de registro solo se conservan durante un período fijo antes de descartarse.  Además, la interfaz de usuario de Conversation Learner mostrará 100 cuadros de diálogo de registro a la vez. 
Número máximo de modelos por usuario | Sin límite preestablecido
Número máximo de acciones secuenciales de no espera | 5 (*)

(*) Después de 5 acciones secuenciales de no espera, las restantes acciones de este tipo se enmascararán y Conversation Learner elegirá entre las acciones de espera disponibles.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a Conversation Learner](./quickstart.md)
