---
title: Actualización de Azure IoT Hub | Microsoft Docs
description: Cambie el nivel de precios y de escala para que IoT Hub obtenga más funcionalidades de administración de dispositivos y mensajería.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: 4acd4f3fd9c013d622a23853dc2f0d02b8b753d9
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985791"
---
# <a name="how-to-upgrade-your-iot-hub"></a>Cómo actualizar IoT Hub

A medida que crece la solución de IoT, Azure IoT Hub está listo para ayudarle a escalar verticalmente. Azure IoT Hub ofrece dos niveles, básico (B) y estándar (S) para adaptarse a los clientes que desean usar diferentes características. Dentro de cada nivel hay tres tamaños (1,2 y 3), que determinan el número de mensajes que se pueden enviar cada día. 

Cuando tiene más dispositivos y necesita más funcionalidades, hay tres formas de ajustar IoT Hub para satisfacer sus necesidades:

* Agregue unidades dentro de IoT Hub. Por ejemplo, cada una de las unidades adicionales en IoT Hub B1 permite 400 000 mensajes diarios adicionales. 
* Cambie el tamaño de IoT Hub. Por ejemplo, migre del nivel B1 al nivel B2 para aumentar el número de mensajes que puede admitir cada unidad al día.
* Actualice a un nivel superior. Por ejemplo, la actualización del nivel B1 al nivel S1 para tener acceso a características avanzadas con la misma capacidad de mensajería.

Todos estos cambios pueden producirse sin interrumpir las operaciones existentes.

Si desea cambiar IoT hub, puede quitar unidades y reducir el tamaño de IoT hub, pero no se puede cambiar a un nivel inferior. Por ejemplo, puede cambiar del nivel S2 al nivel S1, pero no puede cambiar del nivel S2 al nivel B1. Solo se puede elegir un tipo de [edición](https://azure.microsoft.com/pricing/details/iot-hub/) dentro de un nivel por cada instancia de IoT Hub. Por ejemplo, puede crear una instancia de IoT Hub con varias unidades de S1, pero no con una combinación de unidades de versiones distintas como, por ejemplo, S1 y B3, o S1 y S2.

Estos ejemplos están diseñados para ayudarle a entender cómo ajustar IoT Hub a medida que cambia su solución. Para obtener información específica sobre las capacidades de cada nivel, debe consultar siempre [precios de Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Actualizar su IoT Hub existente 

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya a IoT Hub. 
2. Seleccione **Precios y escala**. 

   ![Precios y escala](./media/iot-hub-upgrade/pricing-scale.png)

3. Para cambiar el nivel para su central, seleccione **Nivel de precios y de escala**. Elige el nuevo nivel y, a continuación, haga clic en **Seleccionar**.

   ![Plan de tarifa y escalado](./media/iot-hub-upgrade/select-tier.png)

4. Para cambiar el número de unidades en su central, escriba un nuevo valor en **Unidades de IoT Hub**. 
5. Haga clic en **Guardar** para guardar los cambios. 

IoT Hub se ajusta ahora y sus configuraciones permanecen sin cambios. 

El límite máximo de particiones de nivel básico de IOT y IoT Hub de nivel estándar es 32. La mayoría de instancias de IoT Hub solo necesitan 4 particiones. El límite de particiones se elige cuando se crea la instancia de IoT Hub y relaciona los mensajes del dispositivo a la nube con la cantidad de lectores simultáneos de estos mensajes. Este valor permanecerá invariable cuando migre de un nivel básico a un nivel estándar. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga más detalles sobre [cómo elegir el nivel de IoT Hub adecuado](iot-hub-scaling.md). 

