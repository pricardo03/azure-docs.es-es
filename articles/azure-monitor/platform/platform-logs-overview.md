---
title: Introducción a los registros de plataforma de Azure | Microsoft Docs
description: Introducción a los registros de diagnóstico en Azure que proporciona datos exhaustivos y frecuentes acerca del funcionamiento de un recurso de Azure.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: c1602b9beb5e667caf1688901c7ae4e67e5f5839
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263043"
---
# <a name="overview-of-azure-platform-logs"></a>Introducción a los registros de plataforma Azure
Los registros de plataforma proporcionan información detallada de diagnóstico y auditoría para los recursos de Azure y la plataforma Azure de la que dependen. Se generan automáticamente, aunque es necesario configurar determinados registros de plataforma para que se reenvíen a uno o varios destinos para la conservación. En este artículo se proporciona información general sobre los registros de plataforma, incluida la información que proporcionan y cómo puede configurarlos para la recopilación y el análisis.

## <a name="types-of-platform-logs"></a>Tipos de registros de plataforma
En la tabla siguiente se enumeran los registros de plataforma específicos disponibles en diferentes capas de Azure.

| Nivel | Registros | DESCRIPCIÓN |
|:---|:---|:---|
| Recursos de Azure. | [Registros de recurso](resource-logs-overview.md) | Proporcionan una visión general de las operaciones realizadas dentro del mismo recurso de Azure (en el *plano de datos*), por ejemplo, obtener un secreto de un almacén de claves o realizar una solicitud en una base de datos. El contenido de estos registros de recurso varía según el servicio de Azure y el tipo de recurso.<br>*Los registros de recurso se conocían anteriormente como registros de diagnóstico*.  |
| Suscripción de Azure | [Registro de actividad](activity-logs-overview.md) | Proporciona una visión general de las operaciones de cada recurso de Azure de la suscripción desde fuera (en el *plano de administración*) y de las actualizaciones de los eventos de Service Health. Hay un único registro de actividad para cada suscripción de Azure.   |
| Inquilino de Azure | [Registros de Azure Active Directory](../../active-directory/reports-monitoring/overview-reports.md)  | Contienen el historial de la actividad de inicio de sesión y la traza de auditoría de los cambios realizados en Azure Active Directory para un inquilino determinado.   |


![Introducción a los registros de plataforma](media/platform-logs-overview/logs-overview.png)

## <a name="viewing-platform-logs"></a>Visualización de los registros de plataforma
Puede ver el [registro de actividad](activity-log-view.md) y los [registros de Azure Active Directory](../../active-directory/reports-monitoring/overview-reports.md) en Azure Portal. Debe enviar los registros de recurso a un [destino](#destinations) para verlos.


## <a name="destinations"></a>Destinations
Puede enviar registros de plataforma a uno o varios de los destinos de la tabla siguiente en función de sus requisitos de supervisión. 

| Destino | Escenario | Referencias |
|:---|:---|:---|:---|
| Área de trabajo de Log Analytics | Analice los registros con otros datos de supervisión y aproveche las características de Azure Monitor, como las consultas y las alertas de registro. | [Registros de recurso](resource-logs-collect-storage.md)<br>[Registro de actividad](activity-log-collect.md)<br>[Registros de Azure Active Directory](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Almacenamiento de Azure | Archive los registros para auditoría, análisis estático o copia de seguridad. |[Registros de recurso](archive-diagnostic-logs.md)<br>[Registro de actividad](activity-log-export.md)<br>[Registros de Azure Active Directory](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Centro de eventos | Transmita los registros a sistemas de registro y telemetría de terceros.  |[Registros de recurso](resource-logs-stream-event-hubs.md)<br>[Registro de actividad](activity-log-export.md)<br>[Registros de Azure Active Directory](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |


## <a name="diagnostic-settings-and-log-profiles"></a>Configuraciones de diagnóstico y perfiles de registro
Configure los destinos para los registros de recurso y los registros de Azure Active Directory mediante la [creación de una configuración de diagnóstico](diagnostic-settings.md). Configure los destinos del registro de actividad mediante la [creación de un perfil de registro](activity-log-export.md) o su [conexión a un área de trabajo de Log Analytics](activity-log-collect.md).

La configuración de diagnóstico y el perfil de registro definen lo siguiente:

- Uno o más destinos para enviar los registros y las métricas seleccionados.
- Qué categorías de registro y métricas del recurso se envían a los destinos.
- Si se selecciona una cuenta de almacenamiento como destino, el tiempo de retención de cada categoría de registro.



## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre el registro de actividad](activity-logs-overview.md)
* [Más información sobre los registros de recurso](resource-logs-overview.md)
* [Ver el esquema del registro de recurso para los diferentes servicios de Azure](diagnostic-logs-schema.md)
