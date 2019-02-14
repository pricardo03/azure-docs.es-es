---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2019
ms.openlocfilehash: 5228e4986993f792dceb7324bb6ba9f836f29aea
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56213199"
---
Este contenedor tiene las siguientes opciones de configuración:

|Obligatorio|Configuración|Propósito|
|--|--|--|
|Sí|[ApiKey](#apikey-configuration-setting)|Se usa para realizar un seguimiento de la información de facturación.|
|Sin |[Application Insights](#applicationinsights-setting)|Le permite agregar compatibilidad con los datos de telemetría de [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) al contenedor.|
|Sí|[Facturación](#billing-configuration-setting)|Especifica el URI del punto de conexión del recurso de servicio en Azure.|
|Sí|[Eula](#eula-setting)| Indica que ha aceptado la licencia del contenedor.|
|Sin |[Fluentd](#fluentd-settings)|Escribe el registro y, opcionalmente, los datos de métricas en un servidor de Fluentd.|
|Sin |[Proxy Http](#http-proxy-credentials-settings)|Configure un proxy HTTP para hacer solicitudes salientes.|
|Sin |[Logging](#logging-settings)|Proporciona compatibilidad con el registro de ASP.NET Core al contenedor. |
|Sin |[Mounts](#mount-settings)|Leer y escribir datos desde el equipo host al contenedor y del contenedor al equipo host.|
