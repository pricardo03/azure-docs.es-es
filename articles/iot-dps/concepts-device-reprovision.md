---
title: Conceptos del dispositivo sobre el reaprovisionamiento de Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: En este artículo se describe los conceptos sobre reaprovisionamiento de dispositivos para Azure IoT Hub Device Provisioning Service.
author: wesmc7777
ms.author: wesmc
ms.date: 08/15/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 391131df7726131865ab9d875e8fcde185b3d0a5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965581"
---
# <a name="iot-hub-device-reprovisioning-concepts"></a>Conceptos sobre el reaprovisionamiento de dispositivos de IoT Hub


Durante el ciclo de vida de una solución de IoT, es habitual mover los dispositivos entre centros de IoT. Las razones de este movimiento pueden incluir los siguientes escenarios:

* **Geolocalización o geolatencia**: puesto que un dispositivo se mueve entre ubicaciones, la latencia de red se mejora mediante la migración del dispositivo a un centro de IoT más cercano.

* **Servicios multiinquilino**: un dispositivo puede usarse dentro de la misma solución de IoT y reasignarse a un cliente nuevo o al sitio de un cliente. Este nuevo cliente se puede mantener mediante un centro de IoT diferente.

* **Cambio de la solución**: un dispositivo se podría mover a una solución de IoT nueva o actualizada. Esta reasignación puede requerir que el dispositivo se comunique con un nuevo centro de IoT que esté conectado a otros componentes back-end. 

* **Cuarentena**: es similar a un cambio de la solución. Un dispositivo que no funciona correctamente, que está en peligro o no actualizado se puede reasignar a un centro de IoT para actualizarse y volver a estar en condiciones. Una vez que el dispositivo está funcionando correctamente, se vuelve a migrar a su centro principal.

La compatibilidad del reaprovisionamiento en el Device Provisioning Service soluciona estas necesidades. Los dispositivos se pueden reasignar automáticamente a nuevos centros de IoT basados en la directiva de reaprovisionamiento configurada en la entrada de inscripción del dispositivo. 

## <a name="device-state-data"></a>Datos sobre el estado del dispositivo

Los datos sobre el estado del dispositivo están formados por las capacidades del dispositivo y el [dispositivo gemelo](../iot-hub/iot-hub-devguide-device-twins.md). Estos datos se almacenan en la instancia de Device Provisioning Service y el centro de IoT al cual está asignado un dispositivo. 

![Aprovisionamiento con Device Provisioning Service](./media/concepts-device-reprovisioning/dps-provisioning.png)

Cuando un dispositivo se aprovisiona inicialmente con una instancia de Device Provisioning Service, se llevan a cabo los pasos siguientes:

1. El dispositivo envía una solicitud de aprovisionamiento a una instancia de Device Provisioning Service. La instancia de servicio autentica la identidad del dispositivo en función de una entrada de inscripción y crea la configuración inicial de los datos de estado del dispositivo. La instancia de servicio asigna el dispositivo a un centro de IoT en función de la configuración de inscripción y devuelve esa asignación del centro de IoT al dispositivo.

2. La instancia de servicio de aprovisionamiento proporciona una copia de los datos sobre el estado inicial del dispositivo al centro de IoT asignado. El dispositivo se conecta al centro de IoT asignado y comienza a realizar las operaciones.


Con el tiempo, las [operaciones del dispositivo](../iot-hub/iot-hub-devguide-device-twins.md#device-operations) y las [operaciones de back-end](../iot-hub/iot-hub-devguide-device-twins.md#back-end-operations) pueden actualizar los datos sobre el estado del dispositivo en el centro de IoT. La información sobre el estado inicial del dispositivo almacenada en la instancia de Device Provisioning Service permanece sin cambios. Estos datos sobre el estado del dispositivo sin modificar conforman la configuración inicial.

![Aprovisionamiento con Device Provisioning Service](./media/concepts-device-reprovisioning/dps-provisioning-2.png)

En función del escenario, a medida que un dispositivo se mueve entre centros de IoT, también puede ser necesario migrar el estado del dispositivo actualizado en el anterior centro de IoT al nuevo centro de IoT. Esto está respaldado por las directivas de reaprovisionamiento de Device Provisioning Service. 


## <a name="reprovisioning-policies"></a>Directivas de reaprovisionamiento

Según el escenario, un dispositivo normalmente envía una solicitud de aprovisionamiento a una instancia de servicio de aprovisionamiento en el reinicio y admite un método para desencadenar manualmente el aprovisionamiento por petición. La directiva de reaprovisionamiento en una entrada de inscripción determina la forma en la que la instancia de Device Provisioning Service gestiona estas solicitudes de aprovisionamiento y si habría que llevar a cabo la migración de los datos sobre el estado del dispositivo durante el reaprovisionamiento. Las mismas directivas están disponibles para las inscripciones individuales y los grupos de inscripción:

* **Volver a aprovisionar y migrar datos**: esta directiva es el valor predeterminado para nuevas entradas de inscripción. Esta directiva toma medidas cuando los dispositivos asociados a la entrada de inscripción envían una nueva solicitud de aprovisionamiento (1). Según la configuración de la entrada de la inscripción, el dispositivo se puede reasignar a otro centro de IoT. Si el dispositivo cambia a los centros de IoT, se quitará el registro de dispositivos con el centro de IoT inicial. La información sobre el estado actualizado del dispositivo se migrará de este centro de IoT inicial al nuevo centro de IoT (2). Durante la migración, el estado del dispositivo será **Asignando**.

    ![Aprovisionamiento con Device Provisioning Service](./media/concepts-device-reprovisioning/dps-reprovisioning-migrate.png)


* **Reaprovisionar y restablecer a la configuración inicial**: esta directiva toma medidas cuando los dispositivos asociados a la entrada de inscripción envían una nueva solicitud de aprovisionamiento (1). Según la configuración de la entrada de la inscripción, el dispositivo se puede reasignar a otro centro de IoT. Si el dispositivo cambia a los centros de IoT, se quitará el registro de dispositivos con el centro de IoT inicial. Los datos de configuración inicial que recibió la instancia del servicio de aprovisionamiento al aprovisionar el dispositivo se proporcionan para el nuevo centro de IoT (2). Durante la migración, el estado del dispositivo será **Asignando**.

    Esta directiva se utiliza a menudo para llevar a cabo un restablecimiento de fábrica sin cambiar los centros de IoT. 

    ![Aprovisionamiento con Device Provisioning Service](./media/concepts-device-reprovisioning/dps-reprovisioning-reset.png)


* **No volver a aprovisionar nunca**: el dispositivo nunca se reasigna a un centro diferente. Esta directiva se proporciona para administrar la compatibilidad con versiones anteriores.

#### <a name="managing-backwards-compatibility"></a>Administración de la compatibilidad con versiones anteriores

Antes de septiembre de 2018, las asignaciones de dispositivos a centros de IoT tenían un comportamiento complicado. Cuando un dispositivo retrocedía mediante el proceso de aprovisionamiento, solo se podía asignar al mismo centro de IoT. 

Para las soluciones que dependen de este comportamiento, el servicio de aprovisionamiento incluye la compatibilidad con versiones anteriores. Este comportamiento se mantiene actualmente para los dispositivos según los criterios siguientes:

1. Los dispositivos se conectan con una versión de API anterior a la disponibilidad de la compatibilidad nativa con el reaprovisionamiento en Device Provisioning Service. Consulte la siguiente tabla de API.

2. La entrada de inscripción para los dispositivos no tiene ninguna directiva de reaprovisionamiento establecida. 

Esta compatibilidad garantiza que los dispositivos que ya se hayan implementado experimenten el mismo comportamiento presente durante las pruebas iniciales. Para conservar el comportamiento anterior, no guarde ninguna directiva de reaprovisionamiento en estas inscripciones. Si se establece una directiva de reaprovisionamiento, la directiva de reaprovisionamiento tiene prioridad sobre el comportamiento. Al permitir que la directiva de reaprovisionamiento tenga prioridad, los clientes pueden actualizar el comportamiento del dispositivo sin tener que restablecer la imagen inicial del dispositivo.

El siguiente gráfico de flujo ayuda a resumir los casos en los que el comportamiento está presente:

![gráfico de flujo de la compatibilidad con versiones anteriores](./media/concepts-device-reprovisioning/reprovisioning-compatibility-flow.png)

En la tabla siguiente se muestran las versiones de API anteriores a la disponibilidad de la compatibilidad nativa con el reaprovisionamiento en Device Provisioning Service:


| API DE REST | SDK DE C | SDK de Python |  SDK de Node | SDK de Java | .NET SDK |
| -------- | ----- | ---------- | --------- | -------- | -------- |
| [2018-04-01 y versiones anteriores](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/createorupdate#uri-parameters) | [1.2.8 y versiones anteriores](https://github.com/Azure/azure-iot-sdk-c/blob/master/version.txt) | [1.4.2 y versiones anteriores](https://github.com/Azure/azure-iot-sdk-python/blob/0a549f21f7f4fc24bc036c1d2d5614e9544a9667/device/iothub_client_python/src/iothub_client_python.cpp#L53) | [1.7.3 o versiones anteriores](https://github.com/Azure/azure-iot-sdk-node/blob/074c1ac135aebb520d401b942acfad2d58fdc07f/common/core/package.json#L3) | [1.13.0 o versiones anteriores](https://github.com/Azure/azure-iot-sdk-java/blob/794c128000358b8ed1c4cecfbf21734dd6824de9/device/iot-device-client/pom.xml#L7) | [1.1.0 o versiones anteriores](https://github.com/Azure/azure-iot-sdk-csharp/blob/9f7269f4f61cff3536708cf3dc412a7316ed6236/provisioning/device/src/Microsoft.Azure.Devices.Provisioning.Client.csproj#L20)

> [!NOTE]
> Es probable que estos valores y vínculos cambien. Solo se trata de un intento para determinar en qué punto un cliente puede determinar las versiones y cuáles son las versiones esperadas.




## <a name="next-steps"></a>Pasos siguientes

- [Cómo reaprovisionar los dispositivos](how-to-reprovision.md)







