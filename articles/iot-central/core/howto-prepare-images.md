---
title: Carga de imágenes a una aplicación de Azure IoT Central | Microsoft Docs
description: Como generador, aprenda a preparar y cargar imágenes para su aplicación de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 07/11/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: d8a863ee971fa154740f3726034fc4604b02fd34
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72941405"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Preparación y carga de imágenes a una aplicación de Azure IoT Central

En este artículo se describe cómo puede, en tanto que generador, personalizar la aplicación de Azure IoT Central mediante la carga de imágenes personalizadas. Por ejemplo, puede personalizar un panel de dispositivo con una imagen del dispositivo.

## <a name="before-you-begin"></a>Antes de empezar

Necesitará lo siguiente para completar los pasos de este artículo:

1. Una aplicación de Azure IoT Central. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
1. Una herramienta para ajustar la escala y el tamaño de los archivos de imágenes.

## <a name="choose-where-to-use-custom-images"></a>Elija dónde quiere utilizar imágenes personalizadas.

Puede agregar imágenes personalizadas a las siguientes ubicaciones en una aplicación de Azure IoT Central:

* Página **Mis aplicaciones**

    ![Imagen en la página del administrador de aplicaciones](media/howto-prepare-images/applicationmanager.png)

* Panel de la aplicación

    ![Imagen en el panel de la aplicación](media/howto-prepare-images/homepage.png)

* Una plantilla de dispositivo

    ![Imagen en una plantilla de dispositivo](media/howto-prepare-images/devicetemplate.png)

* Un icono en un panel de dispositivo

    ![Imagen en un icono de dispositivo](media/howto-prepare-images/devicetile.png)

* Un icono en el panel de un conjunto de dispositivos

    ![Imagen en un icono de conjunto de dispositivos](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>Preparación de las imágenes

En las cuatro ubicaciones, puede usar imágenes PNG, GIF o JPEG.

En la tabla siguiente se resumen los tamaños de imagen que puede usar:

| Location | Tamaños |
| -------- | ------ |
| Application Manager (Administrador de aplicaciones) | 268x160 px |
| Plantilla de dispositivo | 64x64 px |
| Iconos de panel | El icono de tamaño más pequeño es 200 x 200 px, los iconos más grandes pueden ser múltiplos cuadrados o rectangulares de los iconos pequeños. Por ejemplo, 200 x 400 px, 400 x 200 px o 400 x 400 px. |

Para obtener la mejor visualización en la aplicación, debe crear imágenes que coincidan con las dimensiones que se muestran en la tabla anterior.

## <a name="upload-the-images"></a>Carga de las imágenes

En las secciones siguientes se describe cómo cargar las imágenes que se usan en las diferentes ubicaciones:

### <a name="application-manager"></a>Administrador de aplicaciones

Para cargar una imagen que se vaya a usar en la página **Mis aplicaciones**, vaya a la página **Configuración de la aplicación** de la sección **Administración**. Debe ser un administrador para completar esta tarea:

![Carga de imagen en la aplicación](media/howto-prepare-images/uploadapplicationmanager.png)

Seleccione el icono **Imagen de aplicación** para cargar una imagen (268 x 160 píxeles) desde el equipo local.

### <a name="application-dashboard"></a>Panel de la aplicación

Para cargar una imagen en el panel de la aplicación, vaya a la página **Panel** de la aplicación y seleccione **Editar**. Debe ser un generador para completar esta tarea:

![Carga de imagen del panel](media/howto-prepare-images/uploadhomepage.png)

En **Configurar la imagen**, seleccione el icono **Imagen** para cargar una imagen desde el equipo local. El icono de tamaño más pequeño es 200 x 200 px, los iconos más grandes pueden ser múltiplos cuadrados o rectangulares de los iconos pequeños. Por ejemplo, 200 x 400 px, 400 x 200 px o 400 x 400 px.

**Guarde** la imagen cargada. Puede cambiar su tamaño mientras está en modo de edición. Cuando haya terminado, seleccione **Listo**.

### <a name="device-template"></a>Plantilla de dispositivo

Para cargar una imagen en una plantilla de dispositivo, vaya a **Plantillas de dispositivo** y elija la plantilla de dispositivo. Debe ser un generador para completar esta tarea:

![Carga de la imagen de plantilla de dispositivo](media/howto-prepare-images/uploaddevicetemplate.png)

Seleccione el icono de imagen para cargar una imagen (64 x 64 píxeles) desde el equipo local.

### <a name="device-dashboard"></a>Panel del dispositivo

Para cargar una imagen en un panel de dispositivo, vaya a **Plantillas de dispositivo** y elija la plantilla de dispositivo. Luego elija la pestaña **Panel**. Debe ser un generador para completar esta tarea:

![Carga de imagen del panel de dispositivo](media/howto-prepare-images/uploaddevicedashboard.png)

En **Configurar la imagen**, seleccione el icono **Imagen** y luego el archivo que se va a cargar desde el equipo local. El icono de tamaño más pequeño es 200 x 200 px, los iconos más grandes pueden ser múltiplos cuadrados o rectangulares de los iconos pequeños. Por ejemplo, 200 x 400 px, 400 x 200 px o 400 x 400 px.

**Guarde** la imagen cargada. Puede cambiar el tamaño y la posición mientras está en modo de edición. Cuando haya terminado, seleccione **Listo**.

### <a name="device-set-dashboard"></a>Panel de conjunto de dispositivos

Para cargar una imagen en un panel de conjunto de dispositivos, vaya a **Device Sets** (Conjuntos de dispositivos), elija el conjunto de dispositivos y luego un dispositivo. Luego seleccione la página **Panel** y **Editar**:

![Carga de imagen del panel de conjunto de dispositivos](media/howto-prepare-images/uploaddevicesetdashboard.png)

En **Configurar la imagen**, seleccione el icono **Imagen** para cargar una imagen desde el equipo local. El icono de tamaño más pequeño es 200 x 200 px, los iconos más grandes pueden ser múltiplos cuadrados o rectangulares de los iconos pequeños. Por ejemplo, 200 x 400 px, 400 x 200 px o 400 x 400 px.

**Guarde** la imagen cargada. Puede cambiar el tamaño y la posición mientras está en modo de edición. Cuando haya terminado, seleccione **Listo**.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a preparar y cargar imágenes en la aplicación de Azure IoT Central, estos son los siguientes pasos que se sugieren:

* [Personalizar la interfaz de usuario de Azure IoT Central](./howto-customize-ui.md)
* [Adición de iconos al panel](./howto-add-tiles-to-your-dashboard.md)
* [Manage devices in your Azure IoT Central application](howto-manage-devices.md) (Administración de dispositivos en la aplicación de Azure IoT Central)
