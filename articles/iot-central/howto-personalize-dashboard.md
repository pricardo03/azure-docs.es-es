---
title: Creación de paneles personales de Azure IoT Central | Microsoft Docs
description: Como usuario, obtenga información sobre cómo crear y administrar sus paneles personales.
author: dominicbetts
ms.author: dobett
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: c048ae8c0daba0e467a9243f4dd83f8d95921e10
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502653"
---
# <a name="create-and-manage-personal-dashboards"></a>Creación y administración de paneles personales

El **panel** es la página que se carga cuando navega por primera vez a la aplicación. Un **generador** en la aplicación define el panel de la aplicación predeterminado para todos los usuarios. Puede reemplazar este panel predeterminado por su propio panel de aplicación personalizado. Puede tener varios paneles que muestren datos diferentes y cambiar entre ellos.

## <a name="create-dashboard"></a>Crear panel

En la captura de pantalla siguiente se muestra el panel en una aplicación creada a partir de la plantilla **Contoso de ejemplo**. Puede reemplazar el panel de aplicación predeterminado por un panel personal. Para ello, seleccione **+ Nuevo** en la parte superior derecha de la página.

![Panel para aplicaciones basadas en la plantilla "Contoso de ejemplo"](media/howto-personalize-dashboard/defaultdashboard.png)

Al seleccionar **+ Nuevo** se abre el editor de panel. En el editor, puede asignarle un nombre al panel y elegir los elementos de la biblioteca. La biblioteca contiene iconos y paneles primitivos que puede usar para personalizar el panel.

![Biblioteca de paneles](media/howto-personalize-dashboard/dashboardeditor.png)

Por ejemplo, puede agregar un icono **Propiedades y configuración del dispositivo** para mostrar la configuración y los valores de las propiedades de un dispositivo que administre. Para ello, seleccione primero una **plantilla de dispositivo** y, a continuación, una **instancia de dispositivo**. Después, asígnele un título al icono y seleccione una **configuración** o una **propiedad** para mostrar. En la captura de pantalla siguiente se muestra el elemento de configuración **Velocidad del ventilador** seleccionado que se va a agregar al icono. Seleccione **Listo** para guardar el cambio en el panel.

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](media/howto-personalize-dashboard/dashboardsetting.png)

Ahora, cuando vea el panel personal, verá el nuevo icono con el elemento de configuración **Velocidad del ventilador** para el dispositivo:

![Pestaña "Dashboard" (Panel) en la que se muestran la configuración y las propiedades del icono](media/howto-personalize-dashboard/personaldashboard.png)

Puede explorar otros tipos de iconos en la biblioteca para descubrir hasta qué punto es posible personalizar los paneles personales.

Para más información sobre cómo usar los iconos en Azure IoT Central, consulte [Uso de iconos de panel](howto-use-tiles.md).

## <a name="manage-dashboards"></a>Administración de paneles

Puede tener varios paneles personales y cambiar entre ellos o elegir el panel de la aplicación predeterminado:

![Cambio de panel](media/howto-personalize-dashboard/switchdashboards.png)

Puede editar los paneles personales y eliminar los que ya no necesite:

![Eliminación de un panel](media/howto-personalize-dashboard/managedashboards.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear y administrar paneles personales, puede:

> [!div class="nextstepaction"]
> [Aprender a administrar las preferencias de las aplicaciones](howto-manage-preferences.md)
