---
title: Límites de servicio de la versión preliminar pública de Azure Digital Twins | Microsoft Docs
description: Descripción de los límites de servicio de la versión preliminar pública de Azure Digital Twins
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dwalthermsft
ms.openlocfilehash: 86ae75118dd1311ea2ae92fb718fe4c58b8e5673
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961762"
---
# <a name="public-preview-service-limits"></a>Límites de servicio de la versión preliminar pública

Durante la versión preliminar pública, Azure Digital Twins tendrá los siguientes límites temporales de suscripción, instancia y frecuencia.

Estas restricciones ayudan a simplificar el aprendizaje sobre el nuevo servicio y sus numerosas características.

> [!NOTE]
> Estos límites se pueden aumentar o eliminar con disponibilidad general (GA).

## <a name="per-subscription-limits"></a>Límites por suscripción

Durante la versión preliminar pública, cada suscripción de Azure puede crear o ejecutar solo una instancia de Azure Digital Twins a la vez.

> [!TIP]
> Si elimina la instancia, puede crear una nueva.

## <a name="per-instance-limits"></a>Límites por instancia

Por su parte, cada instancia de Azure Digital Twins puede tener:

- Un recurso **IoTHub**.
- Un punto de conexión **EventHub** para el tipo de evento **DeviceMessage**.
- Hasta tres puntos de conexión **EventHub**, **ServiceBus** o **EventGrid** del tipo de evento **SensorChange**, **SpaceChange**, **TopologyOperation** o **UdfCustom**.

## <a name="management-api-limits"></a>Límites de la API de administración

Los límites de frecuencia de las solicitudes a la API de administración son:

- 100 solicitudes por segundo a Management API.
- Una sola consulta a Management API puede devolver hasta 1000 objetos. 

> [!IMPORTANT]
> Si se supera el límite de 1000 objetos, se producirá un error y deberá simplificar la consulta.

## <a name="udf-rate-limits"></a>Límites de frecuencia de las funciones definidas por el usuario

Los siguientes límites establecen el total de llamadas de las funciones definidas por el usuario realizadas a la instancia de Azure Digital Twins:

- 400 llamadas de biblioteca cliente por segundo
- 100 llamadas **SendNotification** por segundo

> [!NOTE]
> Las siguientes acciones pueden provocar que se apliquen límites de frecuencia adicionales temporalmente:
> - Modificaciones realizadas en los metadatos del objeto de topología
> - Actualizaciones realizadas a las definiciones de funciones definidas por el usuario
> - Dispositivos que envían telemetría por primera vez

## <a name="device-telemetry-limits"></a>Límites de telemetría del dispositivo

Los siguientes límites suponen el máximo de mensajes que los dispositivos pueden enviar a la instancia de Azure Digital Twins:

- 100 mensajes por segundo

## <a name="next-steps"></a>Pasos siguientes

Para probar un ejemplo de Azure Digital Twins, vaya a la [guía de inicio rápido para buscar salas disponibles](./quickstart-view-occupancy-dotnet.md).