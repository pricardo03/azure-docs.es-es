---
title: Crear paneles personales Azure IoT Central | Microsoft Docs
description: Como usuario, obtenga información sobre cómo crear y administrar sus escritorios personales.
author: dominicbetts
ms.author: dobett
ms.date: 02/13/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: fb74669dcaa802ad06a9c4dff3ffdf25726f518c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60518525"
---
# <a name="create-and-manage-personal-dashboards"></a>Crear y administrar paneles personales

El **panel** es la página que se carga cuando la primera vez que navegue a la aplicación. Un **generador** en la aplicación define el panel de la aplicación predeterminada para todos los usuarios. Puede reemplazar este panel predeterminado con su propio panel de la aplicación personalizada. Puede tener varios paneles que muestran datos diferentes y cambiar entre ellas.

## <a name="create-dashboard"></a>Crear panel

Captura de pantalla siguiente muestra el panel en una aplicación creada a partir del **ejemplo Contoso** plantilla. Puede reemplazar el panel de la aplicación predeterminada con los paneles personales. Para ello, seleccione **+ nuevo** en la parte superior derecha de la página.

![Panel para aplicaciones basadas en la plantilla "Ejemplo de Contoso"](media/howto-personalize-dashboard/defaultdashboard.png)

Seleccionar **+ nuevo**, se abre el editor de panel. En el editor, puede asignarle un nombre de su panel y elija los elementos de la biblioteca. La biblioteca contiene los iconos y primitivas de panel que puede usar para personalizar el panel.

![Biblioteca de paneles](media/howto-personalize-dashboard/dashboardeditor.png)

Por ejemplo, puede agregar un **propiedades y configuración del dispositivo** icono para mostrar los valores de configuración y las propiedades para uno de los dispositivos que administra. Para ello, seleccione primero una **plantilla de dispositivo** y, a continuación, una **instancia de dispositivo**. A continuación, asigne el icono de un título y seleccione un **configuración** o un **propiedad** para mostrar. La siguiente captura de pantalla muestra la **velocidad** van a agregar al icono de configuración. Seleccione **realiza** para guardar el cambio en el panel.

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](media/howto-personalize-dashboard/dashboardsetting.png)

Ahora cuando ve el panel personal, verá el nuevo icono con la **velocidad** establecer para el dispositivo:

![Pestaña "Dashboard" (Panel) en la que se muestran la configuración y las propiedades del icono](media/howto-personalize-dashboard/personaldashboard.png)

Puede explorar otros tipos de iconos en la biblioteca para descubrir cómo personalizar aún más los paneles personales.

## <a name="manage-dashboards"></a>Administrar paneles

Puede tener varios paneles personales y cambiar entre ellos o elija el panel de la aplicación predeterminada:

![Panel de conmutador](media/howto-personalize-dashboard/switchdashboards.png)

Puede modificar los paneles personales y eliminarlos que ya no necesita:

![Eliminar panel](media/howto-personalize-dashboard/managedashboards.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear y administrar paneles personales, puede:

> [!div class="nextstepaction"]
> [Aprenda a administrar sus preferencias de aplicación](howto-manage-preferences.md)
