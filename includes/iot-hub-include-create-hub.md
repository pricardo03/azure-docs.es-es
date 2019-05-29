---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: cc83d94acd25914ee57473de53afbc018f310887
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66117434"
---
En esta sección se describe cómo crear un centro de IoT mediante [Azure Portal](https://portal.azure.com).

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 

2. Elija +**Crear un recurso** y en *Buscar en Marketplace*, escriba **IoT Hub**.

3. Seleccione **Centro de IoT** y haga clic en el botón **Crear**. Verá la primera pantalla para crear un centro de IoT.

   ![Creación de un centro mediante Azure Portal](./media/iot-hub-include-create-hub/iot-hub-create-screen-basics.png)

   Rellene los campos.

   **Suscripción**: seleccione la suscripción que se usará con su centro de IoT.

   **Grupo de recursos**: puede crear un grupo de recursos o use uno existente. Para crear uno, haga clic en **Create new** (Crear) y escriba el nombre que quiere usar. Para usar un grupo de recursos existente, haga clic en **Use existing** (Usar existente) y seleccione el grupo de recursos de la lista desplegable. Para más información, consulte [Administración de grupos de recursos de Azure Resource Manager](../articles/azure-resource-manager/manage-resource-groups-portal.md).

   **Región**: es la región en la que quiere ubicar el centro. Seleccione la ubicación más cercana a usted en la lista desplegable.

   **Nombre de la instancia de IoT Hub**: escriba el nombre de su instancia de IoT Hub. Este nombre debe ser único globalmente. Si el nombre que escribe está disponible, aparece una marca de verificación verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

4. Haga clic en **Siguiente: Size and scale** (Siguiente: tamaño y escala) para seguir con la creación del centro de IoT.

   ![Configuración del tamaño y la escala de un nuevo centro de IoT mediante Azure Portal](./media/iot-hub-include-create-hub/iot-hub-create-screen-size-scale.png)

   En esta pantalla, puede aceptar los valores predeterminados y hacer clic en **Review + create** (Revisar y crear) en la parte inferior. 

   **Plan de tarifa y escala**: Puede elegir entre varios niveles, en función del número de características que desee y del número de mensajes que envíe a través de su solución al día. El nivel gratis está pensado para la prueba y evaluación. Permite la conexión de 500 dispositivos con el centro de IoT y hasta 8000 mensajes al día. Cada suscripción a Azure puede crear una instancia de IoT Hub en el nivel gratis. 

   **Unidades de IoT Hub**: El número de mensajes que se permiten por unidad al día depende del plan de tarifa del centro. Por ejemplo, si desea que el Centro de IoT admita la entrada de 700 000 mensajes, elija dos unidades del nivel de S1.

   Para más información sobre las demás opciones del nivel, consulte la sección [Elección del nivel correcto de IoT Hub](../articles/iot-hub/iot-hub-scaling.md).

   **Avanzada/Particiones del dispositivo a la nube** : esta propiedad relaciona los mensajes del dispositivo a la nube con el número de lectores simultáneos de los mensajes. La mayoría de los centros de IoT solo necesitan cuatro particiones. 

5. Haga clic en **Review + create** (Revisar y crear) para revisar sus selecciones. Verá algo parecido a esta pantalla.

   ![Revisión de la información para crear el centro de IoT](./media/iot-hub-include-create-hub/iot-hub-create-review.png)

6. Haga clic en **Create** (Crear) para crear el centro de IoT. Esta operación tarda unos minutos.
