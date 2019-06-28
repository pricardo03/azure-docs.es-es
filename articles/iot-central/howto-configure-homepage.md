---
title: Configuración del panel de la aplicación de Azure IoT Central | Microsoft Docs
description: Aprenda como generador a configurar el panel de la aplicación predeterminado de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 02/13/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: b1edd4777839c947cf8f60bb6857f939553e2603
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65467608"
---
# <a name="configure-the-application-dashboard"></a>Configuración del panel de la aplicación

El **panel** es la página que se carga cuando los usuarios que tienen acceso a la aplicación van a la dirección URL de la aplicación. Si seleccionó la plantilla de aplicación **Contoso de ejemplo** o **Kits de desarrollo de ejemplo** para crear la aplicación, esta tendrá un panel predefinido. Si eligió la plantilla **Aplicación personalizada**, el panel estará en blanco.

> [!NOTE]
> Los usuarios también pueden [crear sus propios paneles personales](howto-personalize-dashboard.md) para usarlos en lugar del panel de la aplicación predeterminado.

## <a name="add-tiles"></a>Agregar iconos

En la captura de pantalla siguiente se muestra el panel en una aplicación creada a partir de la plantilla **Contoso de ejemplo**. Para personalizar el panel predeterminado para la aplicación, seleccione **Editar** en la parte superior derecha de la página.

![Panel para aplicaciones basadas en la plantilla "Contoso de ejemplo"](media/howto-configure-homepage/image1a.png)

Si selecciona **Editar**, se abrirá el panel Biblioteca. La biblioteca contiene iconos y paneles primitivos que puede usar para personalizar el panel.

![Biblioteca de paneles](media/howto-configure-homepage/image2a.png)

Por ejemplo, puede agregar un icono **Propiedades y configuración del dispositivo** para mostrar una selección de los valores actuales de las propiedades y la configuración de un dispositivo. Para ello, seleccione primero una **plantilla de dispositivo** y, a continuación, una **instancia de dispositivo**. Después, asígnele un título al icono y seleccione una **configuración** o una **propiedad** para mostrar. En la captura de pantalla siguiente se muestran las propiedades y la configuración seleccionadas que se van a agregar al icono. Seleccione **Listo** para guardar el cambio en el panel.

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](media/howto-configure-homepage/image3a.png)

Ahora, cuando un operador visualice el panel de la aplicación predeterminado, verá el nuevo icono con una opción para **establecer la temperatura** del dispositivo:

![Pestaña "Dashboard" (Panel) en la que se muestran la configuración y las propiedades del icono](media/howto-configure-homepage/image4a.png)

Puede explorar otros tipos de iconos en la biblioteca para descubrir hasta qué punto es posible personalizar el panel de la aplicación predeterminado.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar el panel de la aplicación predeterminado de Azure IoT Central, puede hacer lo siguiente:

> [!div class="nextstepaction"]
> [Aprenda a preparar y cargar imágenes](howto-prepare-images.md)
