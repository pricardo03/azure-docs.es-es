---
title: archivo de inclusión
description: archivo de inclusión
services: iot-hub
ms.service: iot-hub
author: dominicbetts
ms.topic: include
ms.date: 02/17/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 69fdc6cf678107ef64ea1fe7b819738fd4a4ff4f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60346260"
---
## <a name="customize-and-extend-the-device-management-actions"></a>Personalizar y ampliar el dispositivo las acciones de administración del dispositivo

Las soluciones de IoT pueden expandir el conjunto definido de patrones de administración de dispositivos o permitir modelos personalizados mediante el uso de los primitivos de método de nube a dispositivo y dispositivos gemelos. Otros ejemplos de acciones de administración de dispositivos son el restablecimiento de fábrica, la actualización de firmware, la actualización de software, la administración de energía, la administración de conectividad y red, y el cifrado de datos.

## <a name="device-maintenance-windows"></a>Ventanas de mantenimiento del dispositivo

Normalmente, puede dispositivos para llevar a cabo acciones a la vez que minimiza las interrupciones y el tiempo de inactividad. Las ventanas de mantenimiento dle dispositivo son un patrón que se utiliza habitualmente para definir la hora en la que un dispositivo debe actualizar su configuración. Las soluciones de back-end pueden utilizar las propiedades deseadas del dispositivo gemelo para definir y activar una directiva en el dispositivo que permita una ventana de mantenimiento. Cuando un dispositivo recibe la directiva de la ventana de mantenimiento, puede usar la propiedad notificada del dispositivo gemelo para informar del estado de la directiva. La aplicación de back-end puede usar luego consultas de dispositivos gemelos para dar testimonio de cumplimiento de dispositivos y cada directiva.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se usó un método directo para desencadenar un reinicio remoto en un dispositivo. Se usaron las propiedades notificadas para notificar la última hora de reinicio del dispositivo y se consultó el dispositivo gemelo para detectar la última hora de reinicio del dispositivo desde la nube.

Para continuar la introducción a IoT Hub y los patrones de administración de dispositivos como remoto a través de la actualización de firmware de aire, consulte [cómo realizar una actualización de firmware](../articles/iot-hub/tutorial-firmware-update.md)

Para obtener información sobre cómo ampliar la solución y programar llamadas de método en varios dispositivos de IoT, consulte [programación y difusión de trabajos](../articles/iot-hub/iot-hub-node-node-schedule-jobs.md).