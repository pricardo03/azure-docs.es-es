---
title: archivo de inclusión (versión preliminar de los flujos de dispositivo)
description: archivo de inclusión (versión preliminar de los flujos de dispositivo)
author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/14/2019
ms.author: rezas
ms.custom: include file
ms.openlocfilehash: 93c71fa8b0c39cc16d2a8e24472e8d68717a6c32
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733236"
---
En esta sección se describe cómo crear un centro de IoT mediante [Azure Portal](https://portal.azure.com).

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Haga clic en **Crear un recurso** y, a continuación, seleccione **Internet de las cosas**.

1. En la lista de la derecha, seleccione **Iot Hub**. Se abre la primera página para crear un centro de IoT.

   ![Creación de un centro de IoT mediante Azure Portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-01.png)

   Rellene los campos:

   a. En la lista desplegable **Suscripción**, seleccione la suscripción que usará con su centro de IoT.

   b. Para el **grupo de recursos**, realice uno de los siguientes pasos: 
      * Para crear un grupo de recursos, seleccione **Crear nuevo** y escriba el nombre que desee usar. 
      * Para usar un grupo de recursos existente, seleccione **Usar existente** y, en la lista desplegable, seleccione el grupo de recursos. 
      
        Para más información, consulte [Administración de grupos de recursos de Azure Resource Manager](../articles/azure-resource-manager/manage-resource-groups-portal.md).

   c. En la lista desplegable **Región**, seleccione la región en la que quiere ubicar su centro. Seleccione una región que admita la versión preliminar de los flujos de dispositivo de IoT Hub, bien **Centro de EE. UU.** o **EUAP de centro de EE. UU.** .

   d. En el cuadro **Nombre de IoT Hub**, escriba el nombre del centro de IoT. El nombre debe ser único globalmente. Si el nombre que escribe está disponible, aparece una marca de verificación verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

1. Para continuar con la creación del centro de IoT, seleccione **Siguiente: Escala y tamaño**.

   ![Configuración del tamaño y la escala de un nuevo centro de IoT mediante Azure Portal](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-02.png)

   En este panel, puede aceptar la configuración predeterminada y seleccionar **Revisar y crear** en la parte inferior. Considere las opciones siguientes:

   * En la lista desplegable **Nivel de precios y de escala** seleccione uno de los niveles estándar (**S1**, **S2** o **S3**), o **F1: nivel Gratis**. Esta elección también puede determinarse por el tamaño de la flota y las cargas de trabajo que no sean de streaming que espera que haya en el centro (por ejemplo, mensajes de telemetría). Por ejemplo, el nivel gratis está pensado para la prueba y evaluación. Permite la conexión de 500 dispositivos con el centro de IoT y hasta 8000 mensajes al día. Cada suscripción a Azure puede crear un centro de IoT en el nivel gratis. 

   * **Número de unidades de IoT Hub**: esta opción depende de la carga de trabajo que no sea de streaming que espera en el centro. Puede seleccionar 1 por ahora.

   Para más información acerca de las opciones del nivel, consulte [Elección del nivel adecuado de IoT Hub](../articles/iot-hub/iot-hub-scaling.md).

1. Para revisar sus selecciones, haga clic en la pestaña **Revisar y crear**. El panel que se abre es similar al siguiente:

   ![Información para crear el centro de IoT](./media/iot-hub-include-create-hub-device-streams/iot-hub-creation-03.png)

1. Para crear el centro de IoT, seleccione **Crear**. El proceso tarda unos minutos.
