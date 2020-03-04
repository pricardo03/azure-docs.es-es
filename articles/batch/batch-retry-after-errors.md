---
title: Reintento de tareas después de un error
description: Compruebe si hay errores y vuelva a intentarlo.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: azure-batch
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/15/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: eda567fda13d6caca679d0ce4947e042eca9530d
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77651934"
---
# <a name="detecting-and-handling-batch-service-errors"></a>Detección y control de errores en el servicio Batch

Es importante recordar comprobar si hay errores al trabajar con una API del servicio REST. No es raro que se produzcan errores al ejecutar trabajos por lotes.

## <a name="common-errors"></a>Errores comunes 

- Errores de redes: se trata de solicitudes que nunca llegaron a Batch o que la respuesta de Batch no llegó al cliente a tiempo.
- Errores internos del servidor: son las respuestas con código de estado HTTP 5xx estándar.
- La limitación puede producir errores como respuestas con código de estado HTTP 429 o 503 con el encabezado Retry-after.
- Los errores 4xx que incluyen errores como AlreadyExists e InvalidOperation. Esto significa que el recurso no está en el estado correcto para la transición de estado.

## <a name="when-to-retry"></a>Cuándo se debe reintentar

Las API de Batch le notificarán si se produce un error. Todos pueden volver a intentarse y todos incluyen un controlador de reintentos global para ese propósito. Es mejor utilizar este mecanismo integrado.

Después de un error, debe esperar un poco (varios segundos entre reintentos) antes de volver a intentarlo. Si realiza reintentos con demasiada frecuencia o demasiado rápido, el controlador de reintentos lo limitará.


Para obtener información detallada sobre cada API y sus directivas de reintento predeterminadas, consulte [Códigos de estado y de error de Batch](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).
