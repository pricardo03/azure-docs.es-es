---
title: Configurar el panel de la aplicación de Azure IoT Central | Microsoft Docs
description: Como un generador, obtenga información sobre cómo configurar el panel de aplicación de Azure IoT Central de forma predeterminada.
author: dominicbetts
ms.author: dobett
ms.date: 02/13/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 3168bbbf70c1ffeb3827482459febbcea256eda6
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57773153"
---
# <a name="configure-the-application-dashboard"></a>Configurar el panel de la aplicación

El **panel** es la página que se carga cuando los usuarios que tienen acceso a la aplicación navegan a la dirección URL de la aplicación. Si ha seleccionado ya sea el **ejemplo Contoso** o **Devkits ejemplo** plantilla de aplicación para crear la aplicación, la aplicación tiene un panel predefinido. Si eligió el **Custom Application** plantilla de aplicación, el panel está en blanco.

> [!NOTE]
> Los usuarios también pueden [crear sus propios paneles personales](howto-personalize-dashboard.md) para usar en lugar del panel de la aplicación predeterminada.

## <a name="add-tiles"></a>Agregar iconos

Captura de pantalla siguiente muestra el panel en una aplicación creada a partir del **ejemplo Contoso** plantilla. Para personalizar el panel predeterminado para la aplicación, seleccione **editar** en la parte superior derecha de la página.

![Panel para aplicaciones basadas en la plantilla "Ejemplo de Contoso"](media/howto-configure-homepage/image1.png)

Seleccionar **editar**, se abre el panel de la biblioteca. La biblioteca contiene los iconos y primitivas de panel que puede usar para personalizar el panel.

![Biblioteca de paneles](media/howto-configure-homepage/image2.png)

Por ejemplo, puede agregar un **propiedades y configuración del dispositivo** icono para mostrar una selección de los valores actuales de configuración y las propiedades de un dispositivo. Para ello, seleccione primero una **plantilla de dispositivo** y, a continuación, una **instancia de dispositivo**. Después, asígnele un título al icono y seleccione una **configuración** o una **propiedad** para mostrar. Captura de pantalla siguiente muestra la configuración y las propiedades que se van a agregar al icono. Seleccione **realiza** para guardar el cambio en el panel.

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](media/howto-configure-homepage/image3.png)

Ahora cuando un operador ve el panel de la aplicación de forma predeterminada, verán el icono de nuevo con el **temperatura establecido** establecer para el dispositivo:

![Pestaña "Dashboard" (Panel) en la que se muestran la configuración y las propiedades del icono](media/howto-configure-homepage/image4.png)

Puede explorar otros tipos de iconos en la biblioteca para descubrir cómo personalizar aún más el panel de la aplicación predeterminada.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar el panel de la aplicación de forma predeterminada Azure IoT Central, puede:

> [!div class="nextstepaction"]
> [Aprenda a preparar y cargar imágenes](howto-prepare-images.md)
