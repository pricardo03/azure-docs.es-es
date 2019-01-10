---
title: Simulación de dispositivos con la supervisión remota de IoT - Azure | Microsoft Docs
description: En esta guía paso a paso, se muestra cómo usar el simulador de dispositivos con el acelerador de soluciones de supervisión remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/28/2018
ms.topic: conceptual
ms.openlocfilehash: 06909c06087219cdfa6edccbfed70ea1694e5a03
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633738"
---
# <a name="create-and-test-a-new-simulated-device"></a>Crear y probar un nuevo dispositivo simulado

El acelerador de soluciones de supervisión remota le permite definir sus propios dispositivos simulados. En este artículo muestra cómo definir un nuevo dispositivo simulado de Bombilla y, a continuación, probarlo localmente. El acelerador de soluciones incluye dispositivos simulados, como refrigeradores y camiones. Sin embargo, puede definir sus propios dispositivos simulados para probar sus soluciones de IoT antes de implementar dispositivos reales.

> [!NOTE]
> En este artículo se describe cómo usar dispositivos simulados hospedados en el servicio de simulación del dispositivo. Si quiere crear un dispositivo físico, consulte [Conectar el dispositivo al acelerador de soluciones de supervisión remota](iot-accelerators-connecting-devices.md).

En esta guía se muestra cómo personalizar el microservicio de simulación de dispositivos. Este microservicio es parte del acelerador de soluciones de supervisión remota. Para mostrar las funcionalidades del simulador de dispositivos, en esta guía paso a paso se usan dos escenarios en la aplicación IoT de Contoso:

[!INCLUDE [iot-solution-accelerators-create-device](../../includes/iot-solution-accelerators-create-device.md)]

## <a name="next-steps"></a>Pasos siguientes

En esta guía se ha explicado cómo crear tipos de dispositivos simulados y personalizados y cómo probarlos mediante la ejecución local del microservicio de simulación de dispositivos.

El siguiente paso sugerido es obtener información sobre cómo implementar los tipos de dispositivos simulados y personalizados en el [Acelerador de soluciones supervisión remota](iot-accelerators-remote-monitoring-deploy-simulated-device.md).
