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
ms.openlocfilehash: 89109dec83342a8f4b5962778b1803eb36656e42
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352226"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Administración de IoT Central desde Azure Portal

En lugar de crear y administrar aplicaciones de IoT Central desde la página [Application Manager](https://aka.ms/iotcentral) (Administrador de aplicaciones) de IoT Central, puede usar [Azure Portal](https://portal.azure.com) para administrar las aplicaciones.

## <a name="create-iot-central-applications"></a>Creación de aplicaciones de IoT Central

Para crear una nueva aplicación, vaya a [Azure Portal](https://ms.portal.azure.com) y haga clic en **Crear un recurso** en el menú de navegación principal de la izquierda.

![Portal de administración: menú de navegación](media/howto-manage-iot-central-from-portal/image0.png)

En la barra de búsqueda escriba el término **IoT Central**.

![Portal de administración: buscar](media/howto-manage-iot-central-from-portal/image0a.png)

Haga clic en el elemento de línea de la **aplicación de IoT Central** en los resultados de la búsqueda.

![Portal de administración: resultados de la búsqueda](media/howto-manage-iot-central-from-portal/image0b.png)

Ahora, haga clic en el botón **Create** (Crear).

![Portal de administración: Recurso de IoT Central](media/howto-manage-iot-central-from-portal/image0c.png)

Rellene todos los campos en el formulario. Este formulario es similar al que se debe rellenar para crear aplicaciones en la página del [Application Manager](https://aka.ms/iotcentral) (Administrador de aplicaciones) de IoT Central. Para más información, consulte la guía de inicio rápido para [Crear una aplicación de IoT Central](quick-deploy-iot-central.md).

![Portal de administración: crear un recurso de IoT Central](media/howto-manage-iot-central-from-portal/image1.png)  

Después de rellenar todos los campos, haga clic en el botón **Crear**.

## <a name="manage-existing-iot-central-applications"></a>Administrar las aplicaciones existentes de IoT Central

Si ya tiene una aplicación de Azure IoT Central, puede eliminarla o moverla a un grupo de recursos o suscripción diferente en Azure Portal.

> [!NOTE]
> No puede ver las aplicaciones de la versión de evaluación en Azure Portal porque no están asociadas con su suscripción.

Para empezar, haga clic en **Todos los recursos** en el menú de navegación principal de la izquierda. En el cuadro de búsqueda, escriba el nombre de la aplicación para buscarla en la lista de recursos. A continuación, haga clic en la aplicación de IoT Central que quiera administrar.

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image2.png)

Para navegar a la aplicación, haga clic en la URL de la aplicación de IoT Central.

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image3.png)

Para mover la aplicación a otro grupo de recursos, haga clic en **Cambiar** al lado del grupo de recursos. En la página **Mover recursos**, seleccione el grupo de recursos al que le gustaría migrar esta aplicación.

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image4.png)

Para mover la aplicación a otra suscripción, haga clic en el vínculo **Cambiar** al lado de la suscripción. Seleccione la suscripción a la que le gustaría migrar esta aplicación en el cuadro de diálogo que aparece.

![Portal de administración: administración de recursos](media/howto-manage-iot-central-from-portal/image5.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar aplicaciones de Azure IoT Central desde Azure Portal, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [Administrar su aplicación](howto-administer.md)