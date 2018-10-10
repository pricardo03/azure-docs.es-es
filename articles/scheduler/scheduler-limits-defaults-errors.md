---
title: Límites, cuotas y umbrales de Azure Scheduler
description: Obtenga información sobre los límites, cuotas, valores predeterminados y umbrales de limitación de Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 0c1e704a3bdec239c87d879ae1ef95e6e76d27fc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966929"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Límites, cuotas y umbrales de limitación de Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se va a retirar. Para programar trabajos, [pruebe Azure Logic Apps en su lugar](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

## <a name="limits-quotas-and-thresholds"></a>Límites, cuotas y umbrales

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>Encabezado x-ms-request-id

Cada solicitud realizada al servicio Scheduler devuelve un encabezado de respuesta llamado**x-ms-request-id**. Este encabezado contiene un valor opaco que identifica de forma única la solicitud. Por lo tanto, si una solicitud falla constantemente y ha confirmado que la solicitud tiene un formato apropiado, puede notificar el error a Microsoft y proporcionar el valor del encabezado de respuesta **x-ms-request-id** e incluir estos detalles: 

* El valor de **x-ms-request-id**
* La hora aproximada en la que se realizó la solicitud 
* Los identificadores de la suscripción de Azure, la colección de trabajos y el trabajo 
* El tipo de operación que intenta realizar la solicitud

## <a name="see-also"></a>Otras referencias

* [¿Qué es Azure Scheduler?](scheduler-intro.md)
* [Conceptos, terminología y jerarquía de entidades de Azure Scheduler](scheduler-concepts-terms.md)
