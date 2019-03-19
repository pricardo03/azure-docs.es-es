---
title: Migración de Azure IoT Hub a la configuración de diagnóstico | Microsoft Docs
description: Se describe cómo actualizar Azure IoT Hub para usar la configuración de diagnóstico de Azure en lugar de la supervisión de operaciones para controlar el estado de las operaciones de IoT Hub en tiempo real.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: kgremban
ms.openlocfilehash: b6cde8402c699a7477cd0efc79a44b3f5e150ad0
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792657"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migración de la supervisión de operaciones de IoT Hub a la configuración de diagnóstico

Los clientes que usan la [supervisión de operaciones](iot-hub-operations-monitoring.md) para realizar un seguimiento del estado de las operaciones en IoT Hub pueden migrar dicho flujo de trabajo a la [configuración de diagnóstico de Azure](../azure-monitor/platform/diagnostic-logs-overview.md), una característica de Azure Monitor. La configuración de diagnóstico proporciona información de diagnóstico a nivel de recurso para muchos servicios de Azure.

**Las operaciones de funcionalidad de supervisión de IoT Hub está en desuso**y se ha quitado desde el portal. En este artículo se explican los pasos para migrar las cargas de trabajo de la supervisión de operaciones a la configuración de diagnóstico. Para más información sobre la escala de tiempo de desuso, consulte [Monitor your Azure IoT solutions with Azure Monitor and Azure Resource Health](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/) (Supervisión de las soluciones de Azure IoT con Azure Monitor y Azure Resource Health).

## <a name="update-iot-hub"></a>Actualización de IoT Hub

Para actualizar IoT Hub en Azure Portal, primero active la configuración de diagnóstico y luego desactive la supervisión de operaciones.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Desactivación de la supervisión de operaciones

> [!NOTE]
> Igual que del 11 de marzo de 2019, las operaciones de característica de supervisión se quita de la interfaz del portal de Azure IoT Hub. Ya no se aplican los pasos siguientes. Para migrar, asegúrese de que las categorías adecuadas estén activadas en la configuración de diagnóstico de Azure Monitor anterior.

Una vez que prueba la nueva configuración de diagnóstico del flujo de trabajo, puede desactivar la característica de supervisión de operaciones. 

1. En el menú de IoT Hub, seleccione **Supervisión de operaciones**.

2. En cada categoría de supervisión, seleccione **Ninguno**.

3. Guarde los cambios de la supervisión de operaciones.

## <a name="update-applications-that-use-operations-monitoring"></a>Actualización de las aplicaciones que usan la supervisión de operaciones

Los esquemas para la supervisión de operaciones y la configuración de diagnóstico varían ligeramente. Es importante que actualice las aplicaciones que utilizan la supervisión de operaciones actualmente para asignarlas al esquema que la configuración de diagnóstico utiliza. 

Además, la configuración de diagnóstico ofrece cinco categorías nuevas para el seguimiento. Después de actualizar las aplicaciones para el esquema existente, agregue también las nuevas categorías:

* Operaciones gemelas de la nube al dispositivo
* Operaciones gemelas del dispositivo a la nube
* Consultas gemelas
* Operaciones de trabajos
* Métodos directos

Para las estructuras de esquema específicas, consulte la [información sobre el esquema de la configuración de diagnóstico](iot-hub-monitor-resource-health.md#understand-the-logs).

## <a name="monitoring-device-connect-and-disconnect-events-with-low-latency"></a>Supervisión de los eventos de conexión y desconexión del dispositivo con baja latencia

Para supervisar dispositivos conectar y desconectar eventos en producción, recomendamos la suscripción a la [ **dispositivo desconectado** evento](iot-hub-event-grid.md#event-types) en Event Grid para recibir alertas y supervisar el estado de conexión del dispositivo. Siga este [tutorial](iot-hub-how-to-order-connection-state-events.md) para aprender a integrar eventos de dispositivo conectado y dispositivo desconectado de IoT Hub en la solución de IoT.

## <a name="next-steps"></a>Pasos siguientes

[Supervisión del mantenimiento de Azure IoT Hub y diagnóstico de problemas rápidamente](iot-hub-monitor-resource-health.md)
