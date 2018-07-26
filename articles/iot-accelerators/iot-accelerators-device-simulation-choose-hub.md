---
title: Uso de un centro de IoT existente con un acelerador de soluciones Simulación de dispositivo en Azure | Microsoft Docs
description: En este artículo se describe cómo configurar un acelerador de soluciones Simulación de dispositivo para usar una instancia de IoT Hub existente.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: ee96173ca5f36dee0f08c38e4b6e29da6fee804e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967516"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Uso de una instancia de IoT Hub existente con el acelerador de soluciones Simulación de dispositivo

Al aprovisionar el acelerador de soluciones Simulación de dispositivo, puede elegir implementar una instancia de IoT Hub en el grupo de recursos del acelerador de soluciones para usar en la simulación.

Si decide no implementar la instancia de IoT Hub opcional, debe usar su propio centro para cualquier simulación que ejecute. Si decide implementar la instancia de IoT Hub opcional, puede utilizar su propio centro o este opcional.

Si no dispone de ninguna instancia de IoT, siempre puede crear una nueva desde [Azure Portal](https://portal.azure.com).

Para utilizar una instancia de IoT Hub existente, necesita una cadena de conexión para la directiva de acceso compartido **iothubowner**. Puede obtener la cadena de conexión de [Azure Portal](https://portal.azure.com):

1. En la página de configuración del centro en el portal, haga clic en **Directivas de acceso compartido**.
1. Haga clic en **iothubowner**.
1. Copie la cadena de conexión principal o la secundaria.

[![Obtención de la cadena de conexión](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Use la cadena de conexión que copió al configurar la simulación:

[![Configuración de la simulación](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-expanded.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido cómo usar una instancia de IoT Hub existente en una simulación. Después, puede que desee aprender a [configurar un modelo de dispositivo personalizado](iot-accelerators-device-simulation-custom-model.md) para una simulación.
