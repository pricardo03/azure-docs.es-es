---
title: Filtros de conexión IP de Azure IoT Hub | Microsoft Docs
description: Describe cómo usar el filtrado de IP para bloquear las conexiones de direcciones IP específicas de su instancia de Azure IoT Hub. Puede bloquear conexiones de direcciones IP concretas o de intervalos.
author: rezasherafat
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: rezas
ms.openlocfilehash: 903f8284327d3d5b9ef386305a436ce44a8a11b2
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378109"
---
# <a name="use-ip-filters"></a>Uso de filtros IP

La seguridad es un aspecto importante de cualquier solución de IoT basada en Azure IoT Hub. En ciertas ocasiones necesitará especificar explícitamente las direcciones IP desde las que se pueden conectar los dispositivos como parte de la configuración de seguridad. La característica de *filtro IP* le permite configurar reglas para rechazar o aceptar tráfico de direcciones IPv4 específicas.

## <a name="when-to-use"></a>Cuándo se deben usar

Hay dos casos específicos cuando resulta útil bloquear los puntos de conexión de IoT Hub para determinadas direcciones IP:

* IoT Hub debe recibir tráfico solo de un intervalo concreto de direcciones IP y rechazar todo lo demás. Por ejemplo, cuando se usa IoT Hub con [Azure ExpressRoute](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) para crear conexiones privadas entre una instancia de IoT Hub y la infraestructura local.

* Cuando necesite rechazar el tráfico de direcciones IP que el administrador de IoT Hub haya identificado como sospechosas.

## <a name="how-filter-rules-are-applied"></a>Cómo se aplican las reglas de filtro

Las reglas de filtro IP se aplican en el nivel de servicio de IoT Hub. Por lo tanto, las reglas de filtro IP se aplican a todas las conexiones de los dispositivos y aplicaciones de back-end mediante un protocolo admitido.

Cualquier intento de conexión desde una dirección IP que coincida con una regla IP de rechazo en su centro de IoT recibe un código de estado 401 no autorizado y la descripción. El mensaje de respuesta no menciona la regla IP.

## <a name="default-setting"></a>Configuración predeterminada

De forma predeterminada, la cuadrícula de **filtro IP** del portal para un centro de IoT está vacía. Este ajuste predeterminado indica que el centro acepta conexiones de cualquier dirección IP. Esta configuración predeterminada es equivalente a una regla que acepta el intervalo de direcciones IP 0.0.0.0/0.

![Configuración predeterminada de filtro de direcciones IP de IoT Hub](./media/iot-hub-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>Incorporación o edición de una regla de filtro IP

Al agregar una regla de filtro IP, le pediremos los siguientes valores:

* Un **nombre de regla de filtro IP** que debe ser una cadena única, que distinta mayúsculas de minúsculas y alfanumérica de hasta 128 caracteres. Solo se aceptan los caracteres alfanuméricos de 7 bits ASCII más `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}`.

* Seleccione un **Rechazar** o **Aceptar** como **acción** para la regla de filtro IP.

* Proporcione una única dirección IPv4 o un bloque de direcciones IP en la notación CIDR. Por ejemplo, en notación CIDR, 192.168.100.0/22 representa las direcciones IPv4 de 1024 de 192.168.100.0 a 192.168.103.255.

![Incorporación de una regla de filtro IP a una instancia de IoT Hub](./media/iot-hub-ip-filtering/ip-filter-add-rule.png)

Tras guardar la regla, se mostrará una alerta que le informará de que la actualización está en curso.

![Notificación acerca de cómo guardar una regla de filtro IP](./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png)

La opción **Agregar** está deshabilitada cuando se alcanza el máximo de 10 reglas de filtro IP.

Puede editar una regla existente al hacer doble clic en la fila que la contiene.

> [!NOTE]
> Rechazar direcciones de IP puede evitar que otros servicios de Azure (por ejemplo, Azure Stream Analytics, Azure Virtual Machines o el Explorador de dispositivos del portal) interactúen con el centro de IoT.

> [!WARNING]
> Si usa Azure Stream Analytics (ASA) para leer mensajes de una instancia de IoT Hub con el filtrado IP habilitado, use el punto de conexión y el nombre compatibles con Event Hub de su instancia de IoT Hub en la cadena de conexión de ASA.

## <a name="delete-an-ip-filter-rule"></a>Eliminación de una regla de filtro IP

Para eliminar una regla de filtro IP, seleccione una o varias reglas en la cuadrícula y haga clic en **Eliminar**.

![Eliminación de una regla de filtro IP de IoT Hub](./media/iot-hub-ip-filtering/ip-filter-delete-rule.png)

## <a name="ip-filter-rule-evaluation"></a>Evaluación de las reglas de filtro IP

Las reglas de filtro IP se aplican en orden y la primera regla que coincida con la dirección IP determina la acción de aceptar o rechazar.

Por ejemplo, si desea aceptar las direcciones del intervalo 192.168.100.0/22 y rechazar todas las demás, la primera regla de la cuadrícula debe aceptar el intervalo de direcciones 192.168.100.0/22. La siguiente regla debe rechazar todas las direcciones mediante el intervalo 0.0.0.0/0.

Puede cambiar el orden de las reglas de filtro IP en la cuadrícula al hacer clic en los tres puntos verticales situados al principio de las filas y mediante el método arrastrar y colocar.

Para guardar el nuevo orden de reglas de filtro IP, haga clic en **Guardar**.

![Cambiar el orden de las reglas de filtro IP de IoT Hub](./media/iot-hub-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Pasos siguientes

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Supervisión de operaciones](iot-hub-operations-monitoring.md)
* [Métricas de IoT Hub](iot-hub-metrics.md)