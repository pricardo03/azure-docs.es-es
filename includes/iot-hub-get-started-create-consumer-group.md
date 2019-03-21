---
author: robinsh
manager: philmea
ms.author: robin.shahan
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 242f601ced4838a1b4e559774c25d05de04ddb77
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2019
ms.locfileid: "57016330"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>Adición de un grupo de consumidores a IoT Hub

Las aplicaciones usan grupos de consumidores para extraer datos de Azure IoT Hub. En este tutorial, se va a crear un grupo de consumidores que un próximo servicio de Azure utilizará para leer los datos de IoT Hub.

Para agregar un grupo de consumidores a su centro de IoT, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), abra su centro de IoT.

2. En el panel izquierdo, haga clic en **Puntos de conexión integrados**, seleccione **Eventos** en el panel superior y escriba un nombre en **Grupos de consumidores** en el panel derecho. Haga clic en **Guardar** después de cambiar el valor **TTL predeterminado** y devolverlo al valor original.

   ![Creación de un grupo de consumidores para el IoT Hub](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
