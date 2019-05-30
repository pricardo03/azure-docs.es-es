---
author: robinsh
manager: philmea
ms.author: robinsh
ms.topic: include
ms.date: 05/20/2019
ms.openlocfilehash: c164433efc6a34a3a06676a3145feb18d3de80b9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249062"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>Adición de un grupo de consumidores a IoT Hub

[Grupos de consumidores](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) proporcionan vistas independientes de la secuencia de eventos que permiten a las aplicaciones y servicios de Azure para consumir datos desde el mismo extremo de centro de eventos de forma independiente. En esta sección, agrega un grupo de consumidores a punto de conexión integrado del su IoT hub que se utiliza más adelante en este tutorial para extraer datos desde el punto de conexión.

Para agregar un grupo de consumidores a su centro de IoT, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), abra su centro de IoT.

2. En el panel izquierdo, seleccione **puntos de conexión integrados**, seleccione **eventos** en el panel derecho y escriba un nombre en **grupos de consumidores**. Seleccione **Guardar**.

   ![Creación de un grupo de consumidores para el IoT Hub](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
