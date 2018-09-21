---
title: Configuración de la seguridad para acceder a Time Series Insights y administrarlo | Microsoft Docs
description: En este artículo se describe cómo configurar la seguridad y los permisos como directivas de acceso de administración y directivas de acceso a datos para proteger Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 7cb5dc5b170103f98d56abc920f36dd85f855961
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364829"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Concesión de acceso a los datos de un entorno de Time Series Insights mediante Azure Portal

Los entornos de Time Series Insights tienen dos tipos independientes de directivas de acceso:

* Directivas de acceso de administración
* Directivas de acceso a datos

Ambas directivas conceden a las entidades de Azure Active Directory (usuarios y aplicaciones) distintos permisos en un entorno concreto. Las entidades de seguridad (usuarios y aplicaciones) deben pertenecer al directorio activo (conocido como inquilino de Azure) asociado a la suscripción que contiene el entorno.

Las directivas de acceso de administración conceden permisos relacionados con la configuración del entorno, tales como:
*   La creación y eliminación del entorno, orígenes de eventos, conjuntos de datos de referencia.
*   Administración de las directivas de acceso a datos.

Las directivas de acceso a datos conceden permisos para emitir consultas de datos, manipular datos de referencia en el entorno y compartir consultas guardadas y perspectivas asociadas con el entorno.

Los dos tipos de directivas permiten una separación clara entre el acceso a la administración del entorno y el acceso a los datos dentro del entorno. Por ejemplo, es posible configurar un entorno de modo que su propietario o creador no disponga de acceso a los datos. Además, los usuarios y servicios que tienen permiso para leer los datos del entorno, podrían no tener acceso a la configuración del entorno.

[!INCLUDE [iot-tsi-data-access](../../includes/iot-tsi-data-access.md)]

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [agregar un origen del evento del centro de eventos al entorno de Azure Time Series Insights](time-series-insights-how-to-add-an-event-source-eventhub.md).
* Realice el [envío de eventos](time-series-insights-send-events.md) al origen del evento.
* Vea el entorno en el [explorador de Time Series Insights](https://insights.timeseries.azure.com).
