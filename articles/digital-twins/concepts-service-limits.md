---
title: Límites de servicio de la versión preliminar pública de Azure Digital Twins | Microsoft Docs
description: Descripción de los límites de servicio de la versión preliminar pública de Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/03/2019
ms.openlocfilehash: 88a66506b5102a4efaeefe1c7033b337dff9889a
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949240"
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

- Exactamente un recurso de **IoTHub** incrustado que se crea automáticamente durante el aprovisionamiento del servicio.
- Exactamente un punto de conexión de **EventHub** para el tipo de evento **DeviceMessage**.
- Hasta tres puntos de conexión **EventHub**, **ServiceBus** o **EventGrid** del tipo de evento **SensorChange**, **SpaceChange**, **TopologyOperation** o **UdfCustom**.

> [!NOTE]
> Algunos parámetros que normalmente se definen al crear las entidades de Azure IoT anteriores no son necesarios durante la versión preliminar pública.
> - Consulte la [documentación de referencia de Swagger](./how-to-use-swagger.md) para conocer las especificaciones de API más recientes.

## <a name="azure-digital-twins-management-api-limits"></a>Límites de API de administración de Azure Digital Twins

Los límites de frecuencia de solicitud de la API de administración de Azure Digital Twins son:

- 100 solicitudes por segundo a la API de administración de Azure Digital Twins.
- Hasta 1000 objetos devueltos por una sola consulta de API de administración de Azure Digital Twins.

> [!IMPORTANT]
> Si se supera el límite de 1000 objetos, se producirá un error y deberá simplificar la consulta.

## <a name="user-defined-functions-rate-limits"></a>Límites de frecuencia de las funciones definidas por el usuario

Los siguientes límites establecen el total de llamadas de las funciones definidas por el usuario realizadas a la instancia de Azure Digital Twins:

- 400 llamadas de biblioteca cliente por segundo
- 100 llamadas **SendNotification** por segundo

> [!NOTE]
> Las siguientes acciones pueden provocar que se apliquen límites de frecuencia adicionales temporalmente:
> - Modificaciones realizadas en los metadatos del objeto de topología
> - Actualizaciones realizadas en la definición de función definida por el usuario
> - Dispositivos que envían telemetría por primera vez

## <a name="device-telemetry-limits"></a>Límites de telemetría del dispositivo

Los siguientes límites suponen el máximo de mensajes que los dispositivos pueden enviar a la instancia de Azure Digital Twins:

- 100 mensajes por segundo

## <a name="next-steps"></a>Pasos siguientes

- Para probar un ejemplo de Azure Digital Twins, vaya a la [guía de inicio rápido para buscar salas disponibles](./quickstart-view-occupancy-dotnet.md).