---
title: Tutorial sobre administración de dispositivos en una solución de supervisión remota basada en Azure | Microsoft Docs
description: En este tutorial se muestra cómo administrar los dispositivos conectados a un acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/19/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 6e92ef276d270fcc4c351fb89c699c8addd74632
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222156"
---
# <a name="tutorial-configure-and-manage-devices-connected-to-your-monitoring-solution"></a>Tutorial: Configuración y administración de dispositivos conectados a una solución de supervisión

En este tutorial, usará el acelerador de soluciones de supervisión remota para configurar y administrar los dispositivos de IoT conectados. Agregará un nuevo dispositivo al acelerador de soluciones, lo configurará y actualizará su firmware.

Contoso solicitó maquinaria nueva para expandir una de sus instalaciones. Mientras espera la entrega de la maquinaria nueva, desea ejecutar una simulación para probar el comportamiento de la solución. Para ejecutar la simulación, agregue un nuevo dispositivo de motor simulado al acelerador de soluciones de supervisión remota y compruebe que este dispositivo simulado responda correctamente a las acciones y a las actualizaciones de configuración.

Para proporcionar una manera extensible de configurar y administrar los dispositivos, el acelerador de soluciones de supervisión remota usa características de IoT Hub, como los [trabajos](../iot-hub/iot-hub-devguide-jobs.md) y los [métodos directos](../iot-hub/iot-hub-devguide-direct-methods.md). Aunque este tutorial utiliza dispositivos simulados, un desarrollador de dispositivos puede implementar métodos directos en un [dispositivo físico conectado al acelerador de soluciones de supervisión remota](iot-accelerators-connecting-devices.md).

En este tutorial, va a:

>[!div class="checklist"]
> * Aprovisionar un dispositivo simulado.
> * Probar un dispositivo simulado.
> * Actualizar el firmware de un dispositivo.
> * Volver a configurar un dispositivo.
> * Organizar los dispositivos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="add-a-simulated-device"></a>Adición de un dispositivo simulado

Vaya a la página **Dispositivos** en la solución y haga clic en **+ Nuevo dispositivo**:

[![Aprovisionamiento de un dispositivo simulado](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-expanded.png#lightbox)

En el panel **Nuevo dispositivo**, elija **Simulado**, deje el número de dispositivos que aprovisionar como **1**, elija el modelo de dispositivo **Faulty Engine** (Motor defectuoso) y, a continuación, elija **Aplicar** para crear el dispositivo simulado:

[![Aprovisionamiento de un dispositivo de motor simulado](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-expanded.png#lightbox)

## <a name="test-the-simulated-device"></a>Prueba del dispositivo simulado

Para probar que el dispositivo de motor simulado envía los valores de propiedad de informes y telemetría, selecciónelo en la lista de dispositivos de la página **Dispositivos**. La información sobre el motor se muestra en el panel **Detalles del dispositivo**:

[![Vista del nuevo dispositivo de motor simulado](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-expanded.png#lightbox)

En **Detalles del dispositivo**, compruebe que el dispositivo nuevo está enviando datos de telemetría. Para ver el flujo de telemetría de vibración desde el dispositivo, haga clic en **Vibración**:

[![Selección de un flujo de telemetría para observar](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-expanded.png#lightbox)

El panel **Detalles del dispositivo** muestra otra información sobre el dispositivo, como los valores de etiqueta, los métodos que admite y las propiedades de que informa.

Para ver los diagnósticos detallados, desplácese hacia abajo en el panel **Detalles del dispositivo** para ver la sección **Diagnósticos**.

## <a name="act-on-a-device"></a>Acción en un dispositivo

Para probar que el dispositivo de motor simulado responde correctamente a las acciones iniciadas desde el panel, ejecute el método **FirmwareUpdate**. Para actuar en un dispositivo ejecutando un método, seleccione el dispositivo en la lista de dispositivos y, a continuación, haga clic en **Trabajos**. Puede seleccionar más de un dispositivo, si desea actuar en varios. En el panel **Trabajos**, seleccione **Ejecutar método**. El modelo de dispositivo **Motor** especifica tres métodos: **FirmwareUpdate**, **FillTank** y **EmptyTank**:

[![Métodos de motor](./media/iot-accelerators-remote-monitoring-manage/devicesmethods-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmethods-expanded.png#lightbox)

Elija **FirmwareUpdate**, establezca el nombre del trabajo en **UpdateEngineFirmware**, establezca la versión de firmware en **2.0.0**, establezca el URI del firmware en **http://contoso.com/engine.bin** y, a continuación, haga clic en **Aplicar**:

[![Programación del método de actualización de firmware](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatejob-inline.png)](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatejob-expanded.png#lightbox)

Para realizar un seguimiento del estado del trabajo, haga clic en **View job status** (Ver estado del trabajo):

[![Supervisión del trabajo de actualización de firmware programado](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatestatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatestatus-expanded.png#lightbox)

Una vez completado el trabajo, vaya a la página **Dispositivos**. Se muestra la nueva versión de firmware para el dispositivo de motor.

Si selecciona varios dispositivos de diferentes tipos en la página **Dispositivos**, puede crear un trabajo para ejecutar un método en varios de esos dispositivos. El panel **Trabajos** solo muestra los métodos que son comunes a todos los dispositivos seleccionados.

## <a name="reconfigure-a-device"></a>Nueva configuración de un dispositivo

Para comprobar si puede actualizar las propiedades de configuración del motor, selecciónelo en la lista de dispositivos de la página **Dispositivos**. A continuación, haga clic en **Trabajos** y, a continuación, elija **Volver a configurar**. El panel de trabajos muestra los valores de propiedad del dispositivo seleccionado que se pueden actualizar:

[![Nueva configuración de un dispositivo](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-expanded.png#lightbox)

Para actualizar la ubicación del motor, establezca el nombre del trabajo en **UpdateEngineLocation**, establezca la longitud en **-122.15**, establezca la ubicación en **Factory 2**, establezca la latitud en **47.62** y haga clic en **Aplicar**:

[![Actualización de un valor de propiedad de dispositivo](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-expanded.png#lightbox)

Para realizar un seguimiento del estado del trabajo, haga clic en **View job status** (Ver estado del trabajo):

[![Actualización de un valor de propiedad de dispositivo](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-expanded.png#lightbox)

Una vez completado el trabajo, vaya a la página **Panel**. El dispositivo de motor se muestra en el mapa en su nueva ubicación:

[![Visualización de la ubicación del motor](./media/iot-accelerators-remote-monitoring-manage/enginelocation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/enginelocation-expanded.png#lightbox)

## <a name="organize-your-devices"></a>Organización de los dispositivos

Para facilitar que los operadores organicen y administren los dispositivos, es conveniente etiquetarlos con el nombre del equipo correspondiente. Contoso tiene dos equipos distintos para las actividades de servicio de campo:

* El equipo de Vehículo inteligente administra los camiones y los dispositivos de creación de prototipos.
* El equipo de Edificio inteligente administra los refrigeradores, los ascensores y los motores.

Para mostrar todos los dispositivos, vaya a la página **Dispositivos** y elija el filtro **Todos los dispositivos**:

[![Mostrar todos los dispositivos](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-expanded.png#lightbox)

### <a name="add-tags"></a>Incorporación de etiquetas

Seleccione todos los dispositivos de **camiones** y de **creación de prototipos**. A continuación, haga clic en **Trabajos**.

En el panel **Trabajos**, seleccione **Etiqueta**, establezca el nombre del trabajo en **AddConnectedVehicleTag** y, a continuación, agregue una etiqueta de texto llamada **FieldService** con el valor **ConnectedVehicle**. A continuación, haga clic en **Aplicar**:

[![Incorporación de una etiqueta a los dispositivos de prototipo y camión](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-expanded.png#lightbox)

En la página de dispositivos, seleccione todos los dispositivos **Refrigerador**, **Ascensor** y **Motor**. A continuación, haga clic en **Trabajos**.

En el panel **Trabajos**, seleccione **Etiqueta**, establezca el nombre del trabajo en **AddSmartBuildingTag** y, a continuación, agregue una etiqueta de texto llamada **FieldService** con el valor **SmartBuilding**. A continuación, haga clic en **Aplicar**:

[![Incorporación de una etiqueta a los dispositivos de refrigerador, ascensor y motor](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-expanded.png#lightbox)

### <a name="create-filters"></a>Creación de filtros

Ahora puede usar los valores de etiqueta para crear filtros. En la página **Dispositivos**, haga clic en **Administrar grupos de dispositivos**:

[![Administrar grupos de dispositivos](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-expanded.png#lightbox)

Cree un filtro de texto que use el nombre de etiqueta **FieldService** y el valor **SmartBuilding** en la condición. Guarde el filtro como **Edificio inteligente**:

[![Creación del filtro de edificio inteligente](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-expanded.png#lightbox)

Cree un filtro de texto que use el nombre de etiqueta **FieldService** y el valor **ConnectedVehicle** en la condición. Guarde el filtro como **Vehículo conectado**.

[![Creación del filtro de vehículo conectado](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-expanded.png#lightbox)

Ahora el operador de Contoso puede consultar los dispositivos en función del equipo operativo:

[![Creación del filtro de vehículo conectado](./media/iot-accelerators-remote-monitoring-manage/filterinaction-inline.png)](./media/iot-accelerators-remote-monitoring-manage/filterinaction-expanded.png#lightbox)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se muestra cómo configurar y administrar los dispositivos conectados a un acelerador de soluciones de supervisión remota. Para aprender a usar el acelerador de soluciones para el análisis de causas principales en una alerta inesperada, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Realización de un análisis de causas principales en una alerta](iot-accelerators-remote-monitoring-root-cause-analysis.md)
