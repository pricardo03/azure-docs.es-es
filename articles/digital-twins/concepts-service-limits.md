---
title: Límites de servicio de la versión preliminar pública de Azure Digital Twins | Microsoft Docs
description: Descripción de los límites de servicio de la versión preliminar pública de Azure Digital Twins
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: dwalthermsft
ms.openlocfilehash: aa5f6053bf1c98d2b84c02617da30f5d856ed3fc
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323917"
---
# <a name="public-preview-service-limits"></a>Límites de servicio de la versión preliminar pública

Durante la versión preliminar pública, Azure Digital Twins tendrá límites de suscripción, instancia y frecuencia temporales que se describen a continuación.

Estas restricciones ayudan a simplificar el aprendizaje sobre el nuevo servicio y sus numerosas características.

> [!NOTE]
> Estos límites se pueden aumentar o eliminar con disponibilidad general (GA).

## <a name="per-subscription-limits"></a>Límites por suscripción

Durante la versión preliminar pública, cada suscripción de Azure puede crear o ejecutar exactamente una instancia de Azure Digital Twins a la vez.

> [!TIP]
> La eliminación de la instancia permite crear una nueva.

## <a name="per-instance-limits"></a>Límites por instancia

Por su parte, cada instancia de Azure Digital Twins puede tener:

- Un recurso `IoTHub`
- Un punto de conexión `EventHub` para el tipo de evento DeviceMessage
- Hasta tres puntos de conexión `EventHub`, `ServiceBus` o `EventGrid` del tipo de evento `SensorChange`, `SpaceChange`, `TopologyOperation` o `UdfCustom`

## <a name="management-api-limits"></a>Límites de la API de administración

Los límites de frecuencia de las solicitudes a la API de administración son:

- 100 solicitudes por segundo a la API de administración
- Una sola consulta a la API de administración puede devolver hasta 1000 objetos

> [!IMPORTANT]
> Si se supera el límite de 1000 objetos, se producirá un error y deberá simplificar la consulta.

## <a name="udf-rate-limits"></a>Límites de frecuencia de las funciones definidas por el usuario

Los siguientes límites establecen el total de llamadas de las funciones definidas por el usuario realizadas a la instancia de Azure Digital Twins:

- 400 llamadas de biblioteca cliente por segundo
- 100 llamadas SendNotification por segundo

> [!NOTE]
> Las siguientes acciones pueden provocar que se apliquen límites de frecuencia adicionales temporalmente:
> - Ediciones en los metadatos del objeto de topología
> - Actualizaciones de las definiciones de las funciones definidas por el usuarios
> - Dispositivos que envían telemetría por primera vez

## <a name="device-telemetry-limits"></a>Límites de telemetría del dispositivo

Los siguientes límites suponen el máximo de mensajes que los dispositivos pueden enviar a la instancia de Azure Digital Twins:

- 100 mensajes por segundo

## <a name="next-steps"></a>Pasos siguientes

Para probar un ejemplo de Azure Digital Twins, vaya a la [guía de inicio rápido para buscar salas disponibles](./quickstart-view-occupancy-dotnet.md).