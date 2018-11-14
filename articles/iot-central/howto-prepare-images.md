---
title: Carga de imágenes a una aplicación de Azure IoT Central | Microsoft Docs
description: Como generador, aprenda a preparar y cargar imágenes para su aplicación de Azure IoT Central.
author: tbhagwat3
ms.author: tanmayb
ms.date: 10/31/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: b55b9929129b882f893b1fea58b7061131daffb8
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51005145"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>Preparación y carga de imágenes a una aplicación de Azure IoT Central

En este artículo se describe cómo puede, en tanto que generador, personalizar la aplicación de Azure IoT Central mediante la carga de imágenes personalizadas. Por ejemplo, puede personalizar un panel de dispositivo con una imagen del dispositivo.

## <a name="before-you-begin"></a>Antes de empezar

Necesitará lo siguiente para completar los pasos de este artículo:

1. Una aplicación de Azure IoT Central. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
1. Una herramienta para ajustar la escala y el tamaño de los archivos de imágenes.

## <a name="choose-where-to-use-custom-images"></a>Elija dónde quiere utilizar imágenes personalizadas.

Puede agregar imágenes personalizadas a las siguientes ubicaciones en una aplicación de Azure IoT Central:

* La página **Application Manager** (Administrador de aplicaciones)

    ![Imagen en la página del administrador de aplicaciones](media/howto-prepare-images/applicationmanager.png)

* La página principal

    ![Imagen en la página principal](media/howto-prepare-images/homepage.png)

* Una plantilla de dispositivo

    ![Imagen en una plantilla de dispositivo](media/howto-prepare-images/devicetemplate.png)

* Un icono en un panel de dispositivo

    ![Imagen en un icono de dispositivo](media/howto-prepare-images/devicetile.png)

* Un icono en el panel de un conjunto de dispositivos

    ![Imagen en un icono de conjunto de dispositivos](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>Preparación de las imágenes

En las cuatro ubicaciones, puede usar imágenes PNG, GIF o JPEG.

En la tabla siguiente se resumen los tamaños de imagen que puede usar:

| Ubicación | Tamaños |
| -------- | ------ |
| Application Manager (Administrador de aplicaciones) | 268x160 px |
| Plantilla de dispositivo | 64x64 px |
| Página principal e iconos del panel | El icono de tamaño más pequeño es 200 x 200 px, los iconos más grandes pueden ser múltiplos cuadrados o rectangulares de los iconos pequeños. Por ejemplo, 200 x 400 px, 400 x 200 px o 400 x 400 px. |

Para obtener la mejor visualización en la aplicación, debe crear imágenes que coincidan con las dimensiones que se muestran en la tabla anterior.

## <a name="upload-the-images"></a>Carga de las imágenes

En las secciones siguientes se describe cómo cargar las imágenes que se usan en las diferentes ubicaciones:

### <a name="application-manager"></a>Administrador de aplicaciones

Para cargar una imagen en **Application Manager** (Administrador de aplicaciones), vaya a la página **Application Settings** (Configuración de la aplicación) de la sección **Administration** (Administración). Debe ser un administrador para completar esta tarea:

![Carga de imagen en la aplicación](media/howto-prepare-images/uploadapplicationmanager.png)

Haga clic en el icono de imagen de la aplicación para cargar la imagen preparada (268 x 160 píxeles) desde la máquina local.

### <a name="home-page"></a>Página de inicio

Para cargar una imagen en la página principal, vaya a la **página principal** de su aplicación y haga clic en **Edit** (Editar). Debe ser un generador para completar esta tarea:

![Carga de imagen de la página principal](media/howto-prepare-images/uploadhomepage.png)

En Configure Image (Configurar imagen), haga clic en el icono de imagen para cargar la imagen preparada desde su máquina local. El icono de tamaño más pequeño es 200 x 200 px, los iconos más grandes pueden ser múltiplos cuadrados o rectangulares de los iconos pequeños. Por ejemplo, 200 x 400 px, 400 x 200 px o 400 x 400 px.

**Guarde** la imagen cargada. Puede cambiar su tamaño mientras está en modo de edición. Cuando haya terminado, haga clic en **Listo**. 

### <a name="device-template"></a>Plantilla de dispositivo

Para cargar una imagen en una plantilla de dispositivo, vaya a **Device Explorer**, elija la plantilla de dispositivo y luego un dispositivo. Debe ser un generador para completar esta tarea:

![Carga de la imagen de plantilla de dispositivo](media/howto-prepare-images/uploaddevicetemplate.png)

Haga clic en el icono de imagen para cargar la imagen preparada (64 x 64 píxeles) desde la máquina local. 

### <a name="device-dashboard"></a>Panel del dispositivo

Para cargar una imagen en un panel de dispositivo, vaya a **Device Explorer**, elija la plantilla de dispositivo y luego un dispositivo. A continuación, elija la página **Dashboard** (Panel) y haga clic en **Edit Template** (Editar plantilla). Debe ser un generador para completar esta tarea:

![Carga de imagen del panel de dispositivo](media/howto-prepare-images/uploaddevicedashboard.png)

En Configure Image (Configurar imagen), haga clic en el icono de imagen y, a continuación, elija el archivo para cargar desde la máquina local. El icono de tamaño más pequeño es 200 x 200 px, los iconos más grandes pueden ser múltiplos cuadrados o rectangulares de los iconos pequeños. Por ejemplo, 200 x 400 px, 400 x 200 px o 400 x 400 px.

**Guarde** la imagen cargada. Puede cambiar el tamaño y la posición mientras está en modo de edición. Cuando haya terminado, haga clic en **Listo**.

### <a name="device-set-dashboard"></a>Panel de conjunto de dispositivos

Para cargar una imagen en un panel de conjunto de dispositivos, vaya a **Device Sets** (Conjuntos de dispositivos), elija el conjunto de dispositivos y luego un dispositivo. A continuación, elija la página **Dashboard** (Panel) y haga clic en **Edit** (Editar).

![Carga de imagen del panel de conjunto de dispositivos](media/howto-prepare-images/uploaddevicesetdashboard.png)

En Configure Image (Configurar imagen), haga clic en el icono de imagen para cargar la imagen preparada desde su máquina local. El icono de tamaño más pequeño es 200 x 200 px, los iconos más grandes pueden ser múltiplos cuadrados o rectangulares de los iconos pequeños. Por ejemplo, 200 x 400 px, 400 x 200 px o 400 x 400 px.

**Guarde** la imagen cargada. Puede cambiar el tamaño y la posición mientras está en modo de edición. Cuando haya terminado, haga clic en **Listo**.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a preparar y cargar imágenes en la aplicación de Azure IoT Central, le sugerimos que siga con lo siguiente:

> [!div class="nextstepaction"]
> [Manage devices in your Azure IoT Central application](howto-manage-devices.md) (Administración de dispositivos en la aplicación de Azure IoT Central)