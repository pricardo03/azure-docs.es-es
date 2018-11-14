---
title: Configuración de la página principal de la aplicación de Azure IoT Central | Microsoft Docs
description: Como generador, aprenda a configurar la página principal de la aplicación de Azure IoT Central.
author: tbhagwat3
ms.author: tanmayb
ms.date: 07/10/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 8e79d05147cae5bf9c46fc1f7c0e7e98574383d5
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007627"
---
## <a name="configuring-homepage"></a>Configuración de la página principal

La página principal es la página que se carga cuando los usuarios que tiene acceso a la aplicación van a la dirección URL de dicha aplicación. Si ha seleccionado las plantillas de aplicación "Sample Contoso" o "Sample Devkits" durante la creación de la aplicación, esta tendrá páginas principales predefinidas. Si, por el contrario, ha seleccionado la plantilla de aplicación "Custom Application", la página principal estará en blanco.

Por ejemplo, aquí está la página principal para aplicaciones basadas en la plantilla "Sample Contoso". Para personalizar la página principal de la aplicación, primero seleccione **Editar** en la parte superior derecha. 

![Página principal para aplicaciones basadas en la plantilla "Sample Contoso"](media\howto-configure-homepage\image1.png)

Al seleccionar **Editar**, se abre la biblioteca de paneles en un panel a la izquierda. Hay muchos tipos de iconos y paneles primitivos que se pueden agregar para personalizar la página principal.

![Biblioteca de paneles](media\howto-configure-homepage\image2.png)

Por ejemplo, puede agregar un icono **Settings and Properties** (Configuración y propiedades) para mostrar una selección de los valores actuales de la configuración y las propiedades. Para ello, seleccione primero una **plantilla de dispositivo** y, a continuación, una **instancia de dispositivo**. Después, asígnele un título al icono y seleccione una **configuración** o una **propiedad** para mostrar. En este caso, hemos seleccionado **Set Temperature** (Establecer temperatura). Al hacer clic en **Guardar**, este icono aparecerá en la página principal.

![El formulario "Configure Device Details" (Configurar detalles del dispositivo) con detalles de la configuración y las propiedades](media\howto-configure-homepage\image3.png)

Ahora, cuando un operador vea la página principal, podrá ver este icono que muestra las propiedades y la configuración del dispositivo:

![Pestaña "Dashboard" (Panel) en la que se muestran la configuración y las propiedades del icono](media\howto-configure-homepage\image4.png)

Practique con los otros tipos de iconos en la biblioteca para descubrir cómo puede personalizar la página principal de la aplicación aún más.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a configurar la página principal de Azure IoT Central, puede:

> [!div class="nextstepaction"]
> [Aprenda a preparar y cargar imágenes](howto-prepare-images.md)
