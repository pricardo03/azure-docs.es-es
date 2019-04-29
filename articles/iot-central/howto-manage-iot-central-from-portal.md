---
title: Administración de IoT Central desde Azure Portal | Microsoft Docs
description: Administración de IoT Central desde Azure Portal.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 01/14/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: f58d06cd191166f47d864241564bc57019b59132
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707433"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Administración de IoT Central desde Azure Portal

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

En lugar de crear y administrar aplicaciones de IoT Central desde la página [Application Manager](https://aka.ms/iotcentral) (Administrador de aplicaciones) de IoT Central, puede usar [Azure Portal](https://portal.azure.com) para administrar las aplicaciones.

## <a name="create-iot-central-applications"></a>Creación de aplicaciones de IoT Central

Para crear una aplicación, vaya a la [portal Azure](https://ms.portal.azure.com) y seleccione **crear un recurso** en el menú de navegación principal de la izquierda.

![Portal de administración: menú de navegación](media/howto-manage-iot-central-from-portal/image0.png)

En la barra de búsqueda escriba el término **IoT Central**.

![Portal de administración: buscar](media/howto-manage-iot-central-from-portal/image0a.png)

Seleccione el **IoT Central Application** elementos de línea en los resultados de búsqueda.

![Portal de administración: resultados de la búsqueda](media/howto-manage-iot-central-from-portal/image0b.png)

Ahora, seleccione **Crear**.

![Portal de administración: Recurso de IoT Central](media/howto-manage-iot-central-from-portal/image0c.png)

Rellene todos los campos en el formulario. Este formulario es similar al que se debe rellenar para crear aplicaciones en la página del [Application Manager](https://aka.ms/iotcentral) (Administrador de aplicaciones) de IoT Central. Para más información, consulte la guía de inicio rápido para [Crear una aplicación de IoT Central](quick-deploy-iot-central.md).

![Portal de administración: crear un recurso de IoT Central](media/howto-manage-iot-central-from-portal/image1.png)  

Después de rellenar todos los campos, seleccione **crear**.

## <a name="manage-existing-iot-central-applications"></a>Administrar las aplicaciones existentes de IoT Central

Si ya tiene una aplicación de Azure IoT Central, puede eliminarla o moverla a un grupo de recursos o suscripción diferente en Azure Portal.

> [!NOTE]
> No puede ver las aplicaciones de la versión de evaluación en Azure Portal porque no están asociadas con su suscripción.

Para empezar, seleccione **todos los recursos** en el menú de navegación principal de la izquierda. En el cuadro de búsqueda, escriba el nombre de la aplicación para buscarla en la lista de recursos. A continuación, seleccione la aplicación de IoT Central que desea administrar.

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image2.png)

Para navegar a la aplicación, seleccione la dirección URL de aplicación Central de IoT.

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image3.png)

Para mover la aplicación a otro grupo de recursos, seleccione **cambiar** al lado del grupo de recursos. En la página **Mover recursos**, seleccione el grupo de recursos al que le gustaría migrar esta aplicación.

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image4.png)

Para mover la aplicación a otra suscripción, seleccione la **cambiar** vínculo junto a la suscripción. Seleccione la suscripción a la que le gustaría migrar esta aplicación en el cuadro de diálogo que aparece.

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image5.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar aplicaciones de Azure IoT Central desde Azure Portal, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [Administrar su aplicación](howto-administer.md)