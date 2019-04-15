---
title: Simulación de dispositivos con la supervisión remota de IoT - Azure | Microsoft Docs
description: En esta guía paso a paso, se muestra cómo usar el simulador de dispositivos con el acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: 81efd9dc5d33ed23574b1cb66f26ccb444d5a3ff
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58180541"
---
# <a name="create-and-test-a-new-simulated-device"></a>Crear y probar un nuevo dispositivo simulado

El acelerador de soluciones de supervisión remota le permite definir sus propios dispositivos simulados. En este artículo muestra cómo definir un nuevo dispositivo simulado de Bombilla y, a continuación, probarlo localmente. El acelerador de soluciones incluye dispositivos simulados, como refrigeradores y camiones. Sin embargo, puede definir sus propios dispositivos simulados para probar sus soluciones de IoT antes de implementar dispositivos reales.

> [!NOTE]
> En este artículo se describe cómo usar dispositivos simulados hospedados en el servicio de simulación del dispositivo. Si desea crear un dispositivo real, consulte [Conexión del dispositivo al acelerador de soluciones de supervisión remota](iot-accelerators-connecting-devices.md).

En esta guía se muestra cómo personalizar el microservicio de simulación de dispositivos. Este microservicio es parte del acelerador de soluciones de supervisión remota. Para mostrar las funcionalidades del simulador de dispositivos, en esta guía paso a paso se usan dos escenarios en la aplicación IoT de Contoso:

[!INCLUDE [iot-solution-accelerators-create-device](../../includes/iot-solution-accelerators-create-device.md)]

## <a name="next-steps"></a>Pasos siguientes

En esta guía se ha explicado cómo crear tipos de dispositivos simulados y personalizados y cómo probarlos mediante la ejecución local del microservicio de simulación de dispositivos.

El siguiente paso sugerido es obtener información sobre cómo implementar los tipos de dispositivos simulados y personalizados en el [Acelerador de soluciones de supervisión remota](iot-accelerators-remote-monitoring-deploy-simulated-device.md).
