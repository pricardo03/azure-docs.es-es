---
title: Reaprovisionamiento de dispositivos en el servicio Azure IoT Hub Device Provisioning
description: Obtenga información sobre cómo reaprovisionar dispositivos con la instancia de Device Provisioning Service (DPS) y por qué es posible que tenga que hacerlo.
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 0ded494debab19daa15a953715b1ab7b0b10ad18
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974911"
---
# <a name="how-to-reprovision-devices"></a>Reaprovisionamiento de dispositivos

Durante el ciclo de vida de una solución de IoT, es habitual mover los dispositivos entre centros de IoT. Las razones de este movimiento pueden incluir los siguientes escenarios:

* **Geolocalización**: puesto que un dispositivo se mueve entre ubicaciones, la latencia de red se mejora mediante la migración del dispositivo a un centro de IoT Hub más cercano a cada ubicación.

* **Servicios multiinquilino**: un dispositivo puede usarse dentro de la misma solución de IoT y reasignarse o alquilarse a un nuevo cliente o al sitio de un cliente. Este nuevo cliente se puede atender mediante un centro de IoT diferente.

* **Cambio de la solución**: un dispositivo se podría mover a una solución de IoT nueva o actualizada. Esta reasignación puede requerir que el dispositivo se comunique con un nuevo centro de IoT que esté conectado a otros componentes del back-end. 

* **Cuarentena**: es similar a un cambio de la solución. Un dispositivo que no funciona correctamente, que está en peligro o no actualizado se puede reasignar a un centro de IoT para actualizarse y volver a estar en condiciones. Una vez que el dispositivo está funcionando correctamente, se vuelve a migrar a su centro principal.

Para una descripción más detallada del reaprovisionamiento, consulte [Conceptos sobre el reaprovisionamiento de dispositivos de IoT Hub](concepts-device-reprovision.md).


## <a name="configure-the-enrollment-allocation-policy"></a>Configuración de la directiva de asignación de inscripciones

La directiva de asignación determina cómo se asignarán los dispositivos asociados con la inscripción a un centro de IoT, una vez aprovisionados.

En los pasos siguientes se configura la directiva de asignación para la inscripción de un dispositivo:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la instancia de Device Provisioning Service.

2. Haga clic en **Administrar inscripciones** y haga clic en el grupo de inscripciones o en una inscripción individual que quiera configurar para el reaprovisionamiento. 

3. En **Select how you want to assign devices to hubs** (Seleccionar cómo asignar los dispositivos a los centros), seleccione las siguientes directivas de asignación:

    * **Latencia más baja**: esta directiva asigna dispositivos al centro de IoT Hub vinculado que dará lugar a comunicaciones de latencia más baja entre el dispositivo e IoT Hub. Esta opción permite que el dispositivo se comunique con el centro de IoT más cercano en función de la ubicación. 
    
    * **Distribución ponderada uniformemente**: esta directiva distribuye los dispositivos entre los centros de IoT Hub vinculados en función del peso de asignación asignado a cada centro de IoT Hub vinculado. Esta directiva permite equilibrar la carga de los dispositivos entre un grupo de centros vinculados en función de los pesos de asignación establecidos sobre esos centros. Si va a aprovisionar dispositivos para un único centro de IoT, se recomienda esta configuración. Esta es la configuración predeterminada. 
    
    * **Configuración estática**: esta directiva requiere que el centro de IoT Hub deseado se muestre en la entrada de inscripción del dispositivo que se va a aprovisionar. Esta directiva permite designar un único centro de IoT al que quiere asignar los dispositivos.

4. En **Select the IoT hubs this group can be assigned to** (Seleccionar los centros de IoT a los que se puede asignar este grupo), seleccione los centros de IoT vinculados que quiere incluir con la directiva de asignación. También puede agregar un nuevo centro de IoT vinculado mediante el botón **Link a new IoT Hub** (Vincular a un nuevo centro de IoT).

    Con la directiva de asignación **Lowest latency** (Latencia más baja), los centros que seleccione se incluirán en la evaluación de la latencia para determinar el centro más próximo para la asignación de dispositivos.

    Con la directiva de asignación **Evenly weighted distribution** (Distribución ponderada uniformemente), se equilibra la carga de los dispositivos entre los centros que seleccione en función de los pesos de asignación configurados y su carga actual de dispositivos.

    Con la directiva de asignación **Static configuration** (Configuración estática), seleccione el centro de IoT al que quiere que se asignen los dispositivos.

4. Haga clic en **Save** (Guardar), o continúe con la sección siguiente para establecer la directiva de reaprovisionamiento.

    ![Selección de la directiva de asignación de inscripciones](./media/how-to-reprovision/enrollment-allocation-policy.png)



## <a name="set-the-reprovisioning-policy"></a>Establecimiento de la directiva de reaprovisionamiento

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la instancia de Device Provisioning Service.

2. Haga clic en **Administrar inscripciones** y haga clic en el grupo de inscripciones o en una inscripción individual que quiera configurar para el reaprovisionamiento.

3. En **Seleccione cómo desea que los datos del dispositivo para administrarse en reaprovisionamiento a un centro de IoT diferentes**, elija una de las siguientes directivas de reaprovisionamiento:

    * **Volver a aprovisionar y migrar datos**: esta directiva toma medidas cuando los dispositivos asociados a la entrada de inscripción envían una nueva solicitud de aprovisionamiento. Según la configuración de la entrada de inscripción, el dispositivo se puede reasignar a otro centro de IoT. Si el dispositivo cambia a los centros de IoT, se quitará el registro de dispositivos con el centro de IoT inicial. Toda la información sobre el estado del dispositivo de ese centro de IoT inicial se migrará al nuevo centro de IoT. Durante la migración, el estado del dispositivo será **Assigning** (Asignando).

    * **Volver a aprovisionar y restablecer a la configuración inicial**: esta directiva toma medidas cuando los dispositivos asociados a la entrada de inscripción envían una nueva solicitud de aprovisionamiento. Según la configuración de la entrada de inscripción, el dispositivo se puede reasignar a otro centro de IoT. Si el dispositivo cambia a los centros de IoT, se quitará el registro de dispositivos con el centro de IoT inicial. Se proporcionan al nuevo centro de IoT los datos de configuración iniciales que recibió la instancia del servicio de aprovisionamiento al aprovisionar el dispositivo. Durante la migración, el estado del dispositivo será **Assigning** (Asignando).

4. Haga clic en **Save** (Guardar) para permitir el reaprovisionamiento del dispositivo en función de los cambios.

    ![Selección de la directiva de asignación de inscripciones](./media/how-to-reprovision/reprovisioning-policy.png)



## <a name="send-a-provisioning-request-from-the-device"></a>Envío de una solicitud de aprovisionamiento desde el dispositivo

Para que los dispositivos se reaprovisionen según los cambios de configuración realizados en las secciones anteriores, estos dispositivos deben solicitar el reaprovisionamiento. 

La frecuencia con la que un dispositivo envía una solicitud de aprovisionamiento depende del escenario. Sin embargo, se aconseja programar los dispositivos para que envíen una solicitud de aprovisionamiento a una instancia del servicio de aprovisionamiento al reiniciarse, y que admitan un [método](../iot-hub/iot-hub-devguide-direct-methods.md) para desencadenar manualmente el aprovisionamiento a petición. El aprovisionamiento también podría desencadenarse mediante el establecimiento de una [propiedad deseada](../iot-hub/iot-hub-devguide-device-twins.md#desired-property-example). 

La directiva de reaprovisionamiento en una entrada de inscripción determina la forma en la que la instancia de Device Provisioning Service administra estas solicitudes de aprovisionamiento y si habría que llevar a cabo la migración de los datos sobre el estado del dispositivo durante el reaprovisionamiento. Las mismas directivas están disponibles para las inscripciones individuales y los grupos de inscripción:

Si quiere ver un ejemplo de código para enviar solicitudes de aprovisionamiento desde un dispositivo durante una secuencia de arranque, consulte [Aprovisionamiento automático de un dispositivo simulado](quick-create-simulated-device.md).


## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre el reaprovisionamiento, consulte [Conceptos sobre el reaprovisionamiento de dispositivos de IoT Hub](concepts-device-reprovision.md). 
- Para obtener más información sobre el desaprovisionamiento, consulte [Desaprovisionamiento de dispositivos aprovisionados automáticamente](how-to-unprovision-devices.md). 











