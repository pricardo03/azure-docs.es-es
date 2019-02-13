---
title: Configuración de la página principal de la aplicación de Azure IoT Central | Microsoft Docs
description: Como generador, aprenda a configurar la página principal de la aplicación de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 599616f046b2eccee009e8a08e8a2f51cf968644
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55772768"
---
# <a name="configuring-homepage"></a>Configuración de la página principal

La página principal es la página que se carga cuando los usuarios que tiene acceso a la aplicación van a la dirección URL de dicha aplicación. Si ha seleccionado las plantillas de aplicación "Sample Contoso" o "Sample Devkits" durante la creación de la aplicación, esta tendrá páginas principales predefinidas. Si, por el contrario, ha seleccionado la plantilla de aplicación "Custom Application", la página principal estará en blanco.

## <a name="add-tiles"></a>Agregar iconos

Por ejemplo, aquí está la página principal para aplicaciones basadas en la plantilla "Sample Contoso". Para personalizar la página principal de la aplicación, primero seleccione **Editar** en la parte superior derecha. 

![Página principal para aplicaciones basadas en la plantilla "Sample Contoso"](media/howto-configure-homepage-experimental/image1.png)

Al seleccionar **Editar**, se abre la biblioteca de paneles en un panel a la izquierda. Hay muchos tipos de iconos y paneles primitivos que se pueden agregar para personalizar la página principal.

![Biblioteca de paneles](media/howto-configure-homepage-experimental/image2.png)

Por ejemplo, puede agregar un icono **Settings and Properties** (Configuración y propiedades) para mostrar una selección de los valores actuales de la configuración y las propiedades. Para ello, seleccione primero una **plantilla de dispositivo** y, a continuación, una **instancia de dispositivo**. Después, asígnele un título al icono y seleccione una **configuración** o una **propiedad** para mostrar. En este caso, hemos seleccionado **Set Temperature** (Establecer temperatura). Al hacer clic en **Guardar**, este icono aparecerá en la página principal.

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](media/howto-configure-homepage-experimental/image3.png)

Ahora, cuando un operador vea la página principal, podrá ver este icono que muestra las propiedades y la configuración del dispositivo:

![Pestaña "Dashboard" (Panel) en la que se muestran la configuración y las propiedades del icono](media/howto-configure-homepage-experimental/image4.png)

Practique con los otros tipos de iconos en la biblioteca para descubrir cómo puede personalizar la página principal de la aplicación aún más.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar la página principal de Azure IoT Central, puede:

> [!div class="nextstepaction"]
> [Aprenda a preparar y cargar imágenes](howto-prepare-images-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
