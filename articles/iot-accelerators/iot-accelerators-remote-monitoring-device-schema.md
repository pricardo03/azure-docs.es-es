---
title: 'Esquema de dispositivos en una solución de supervisión remota: Azure | Microsoft Docs'
description: En este artículo se describe el esquema JSON que define un dispositivo simulado en la solución de supervisión remota.
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 12/18/2018
ms.topic: conceptual
ms.openlocfilehash: 3c2bca45a7927689992cf44fb70199e246641760
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54445429"
---
# <a name="understand-the-device-model-schema"></a>Descripción del esquema de modelo del dispositivo

Puede usar dispositivos simulados en la solución de supervisión remota para probar su comportamiento. La solución de supervisión remota incluye un servicio de simulación de dispositivos para ejecutar dispositivos simulados. Al implementar la solución de supervisión remota, se aprovisiona automáticamente una colección de dispositivos simulados. Puede personalizar los dispositivos simulados existentes o crear los suyos propios.

En este artículo se describe el esquema de modelo del dispositivo que especifica las características y el comportamiento de un dispositivo simulado. El modelo del dispositivo se almacena en un archivo JSON.

> [!NOTE]
> El esquema de este modelo de dispositivo es solo para dispositivos simulados que se hospedan en el servicio de simulación del dispositivo. Si desea crear un dispositivo real, consulte [Conexión del dispositivo al acelerador de soluciones de supervisión remota](iot-accelerators-connecting-devices.md).

Los siguientes artículos están relacionadas con el artículo actual:

* [Implement the device model behavior](iot-accelerators-remote-monitoring-device-behavior.md) (Implementar el comportamiento de modelo del dispositivo): describe los archivos de JavaScript que se usan para implementar el comportamiento de un dispositivo simulado.
* [Create a new simulated device](iot-accelerators-remote-monitoring-create-simulated-device.md) (Crear un nuevo dispositivo simulado): reúne todos los elementos necesarios y le muestra cómo implementar un nuevo tipo de dispositivo simulado en la solución.

En este artículo, aprenderá a:

>[!div class="checklist"]
> * Usar un archivo JSON para definir un modelo de dispositivo simulado.
> * Especificar las propiedades del dispositivo simulado.
> * Especificar la telemetría que envía el dispositivo simulado.
> * Especificar los métodos de tipo "de la nube al dispositivo" a los que el dispositivo responde.

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Pasos siguientes

En este artículo se describe la manera de crear su propio modelo de dispositivo simulado personalizado. En este artículo le mostramos cómo:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Usar un archivo JSON para definir un modelo de dispositivo simulado.
> * Especificar las propiedades del dispositivo simulado.
> * Especificar la telemetría que envía el dispositivo simulado.
> * Especificar los métodos de tipo "de la nube al dispositivo" a los que el dispositivo responde.

Ahora que tiene información sobre el esquema JSON, el siguiente paso propuesto aprender a [implementar el comportamiento del dispositivo simulado](iot-accelerators-remote-monitoring-device-behavior.md).

Para más información sobre la solución de supervisión remota para los desarrolladores, consulte:

* [Guía de referencia para desarrolladores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Guía de solución de problemas para desarrolladores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)
