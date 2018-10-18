---
title: Administración de IoT Central desde Azure Portal | Microsoft Docs
description: Administración de IoT Central desde Azure Portal.
services: iot-central
ms.service: iot-central
author: tbhagwat3
ms.author: tanmayb
ms.date: 08/30/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: ff6978ddbf8718ad45a4265898109b7ac799d70c
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46468618"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Administración de IoT Central desde Azure Portal 
Además de crear y administrar aplicaciones de IoT Central desde el sitio web de IoT Central, también se puede administrar IoT Central desde Azure Portal. Este artículo le guiará sobre qué se puede hacer y cómo hacerlo.

## <a name="create-iot-central-applications"></a>Creación de aplicaciones de IoT Central
Para crear una nueva aplicación, vaya a [Azure Portal](https://ms.portal.azure.com) y haga clic en "Crear un recurso" en el menú de navegación principal de la izquierda. 

![Portal de administración: menú de navegación](media\howto-manage-iot-central-from-portal\image0.png)

En la barra de búsqueda escriba el término "IoT Central".

![Portal de administración: buscar](media\howto-manage-iot-central-from-portal\image0a.png)

Haga clic en los elementos de línea de la aplicación de IoT Central en los resultados de la búsqueda.

![Portal de administración: resultados de la búsqueda](media\howto-manage-iot-central-from-portal\image0b.png)

Ahora, haga clic en el botón "Crear" para ver el formulario que debe rellenar.

![Portal de administración: recurso de IoT Central](media\howto-manage-iot-central-from-portal\image0c.png)

Rellene todos los campos en el formulario. Este formulario es similar al formulario que se debe rellenar para crear aplicaciones desde el sitio web de IoT Central. Para obtener más información sobre cómo rellenar cada campo, puede consultar el documento [Creación de una aplicación de Azure IoT Central](https://docs.microsoft.com/ azure/iot-central/howto-create-application). 

![Portal de administración: crear un recurso de IoT Central](media\howto-manage-iot-central-from-portal\image1.png)  

Después de rellenar todos los campos, haga clic en el botón "Crear".

## <a name="manage-existing-iot-central-applications"></a>Administrar las aplicaciones existentes de IoT Central
Si ya tiene una aplicación de Azure IoT Central, puede eliminarla o moverla a un grupo de recursos o suscripción diferente en Azure Portal. No puede ver las aplicaciones de prueba de 7 días en Azure Portal porque ninguna suscripción es compatible con esas pruebas.

Para empezar, haga clic en "Todos los recursos" en el menú de navegación principal de la izquierda. En el cuadro de búsqueda, escriba el nombre de la aplicación y búsquela en la lista de recursos. A continuación, haga clic en la aplicación de IoT Central que quiera administrar.

![Portal de administración: administración de recursos](media\howto-manage-iot-central-from-portal\image2.png)

Para navegar a la aplicación, haga clic en la URL de la aplicación de IoT Central.

![Portal de administración: administración de recursos](media\howto-manage-iot-central-from-portal\image3.png)

Para mover la aplicación a otro grupo de recursos, haga clic en el vínculo **Cambiar** al lado del grupo de recursos. Seleccione el grupo de recursos donde le gustaría migrar esta aplicación en el cuadro de diálogo que aparece.

![Portal de administración: administración de recursos](media\howto-manage-iot-central-from-portal\image4.png)

Para mover la aplicación a otra suscripción, haga clic en el vínculo **Cambiar** al lado de la suscripción. Seleccione la suscripción donde le gustaría migrar esta aplicación en el cuadro de diálogo que aparece.

![Portal de administración: administración de recursos](media\howto-manage-iot-central-from-portal\image5.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar aplicaciones de Azure IoT Central desde Azure Portal, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [Administrar su aplicación](howto-administer.md)