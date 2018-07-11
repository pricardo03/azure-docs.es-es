---
title: Uso de alertas y resolución de problemas de los dispositivos en la solución de supervisión remota de Azure | Microsoft Docs
description: En este tutorial, se explica cómo puede utilizar las alertas para identificar y solucionar problemas relacionados con los dispositivos conectados al acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/18/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 9607705220450b30d2ffaf0f2be9fa2a5664b879
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081795"
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Solución de problemas de los dispositivos

En este tutorial, utilizará el acelerador de soluciones de supervisión remota para identificar y solucionar problemas relacionados con los dispositivos de IoT conectados. Utilizará las alertas del panel del acelerador de soluciones para identificar los problemas y después ejecutará trabajos remotos para resolverlos.

Contoso está probando un tipo nuevo de **prototipo** en el campo. Como operador de Contoso, durante las pruebas observa que el dispositivo **Prototipo** desencadena inesperadamente una alerta en el panel. Ahora, debe investigar el comportamiento de este dispositivo **prototipo** defectuoso y resolver el problema.

En este tutorial, hizo lo siguiente:

>[!div class="checklist"]
> * Investigación de una alerta procedente de un dispositivo
> * Resolución del problema relacionado con el dispositivo

## <a name="prerequisites"></a>requisitos previos

Para seguir este tutorial, necesita una instancia implementada de la solución de supervisión remota en la suscripción de Azure.

Si aún no ha implementado el acelerador de soluciones de supervisión remota, debe completar la guía de inicio rápido [Implementación de una solución de supervisión remota basada en la nube](quickstart-remote-monitoring-deploy.md).

## <a name="investigate-an-alert"></a>Investigación de una alerta

En la página **Panel** observará que hay alertas de temperatura inesperadas que provienen de la regla asociada con los dispositivos **Prototipo**:

[![Alertas que aparecen en el panel](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-expanded.png#lightbox)

Para investigar aún más el problema, elija la opción **Explorar alerta** junto a la alerta:

[![Exploración de la alerta desde el panel](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-expanded.png#lightbox)

Aparece la vista de detalles de la alerta:

* Cuándo se desencadenó la alerta
* La información de estado sobre los dispositivos asociados con la alerta
* Telemetría desde los dispositivos asociados con la alerta

[![Detalles de alertas](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-expanded.png#lightbox)

Para confirmar la alerta, seleccione todas las opciones de **Alert occurrences** (Repeticiones de alertas) y elija **Acknowledge** (Confirmación). Esta acción permite que otros operadores sepan que ha visto la alerta y que está trabajando en ella:

[![Confirmación de alertas](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-expanded.png#lightbox)

Al confirmar la alerta, el estado de la repetición cambia a **Confirmado**.

En la lista, puede ver el dispositivo **Prototipo** responsable de activar la alerta de temperatura:

[![Lista de los dispositivos que causan la alerta](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-expanded.png#lightbox)

## <a name="resolve-the-issue"></a>Resolución del problema

Para solucionar el problema con el dispositivo **prototipo**, debe llamar al método **DecreaseTemperature** del dispositivo.

Para realizar alguna acción en un dispositivo, selecciónelo en la lista de dispositivos y, luego, elija **Trabajos**. El modelo de dispositivo **Prototipo** especifica los seis métodos que debe admitir un dispositivo:

[![Vista de los métodos que admite el dispositivo](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-expanded.png#lightbox)

Elija **DecreaseTemperature** y establezca el nombre del trabajo en **DecreaseTemperature**. Luego elija **Aplicar**:

[![Creación del trabajo para disminuir la temperatura](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-expanded.png#lightbox)

Para realizar un seguimiento del estado del trabajo, haga clic en **View job status** (Ver estado del trabajo). Use la vista **Trabajos** para realizar un seguimiento de todos los trabajos y las llamadas de método en la solución:

[![Supervisión del trabajo para disminuir la temperatura](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-expanded.png#lightbox)

Puede comprobar si la temperatura del dispositivo ha disminuido consultando los datos de telemetría de la página **Dashboard** (Panel):

[![Vista de la disminución de temperatura](./media/iot-accelerators-remote-monitoring-maintain/jobresult-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/jobresult-expanded.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a utilizar alertas para identificar problemas con los dispositivos y a solucionarlos realizando las acciones oportunas. Para aprender a conectar un dispositivo físico al acelerador de soluciones, consulte los artículos de procedimientos.

Ahora que sabe cómo solucionar los problemas con los dispositivos, le sugerimos que aprenda a [conectar un dispositivo al acelerador de soluciones de supervisión remota](iot-accelerators-connecting-devices.md).
