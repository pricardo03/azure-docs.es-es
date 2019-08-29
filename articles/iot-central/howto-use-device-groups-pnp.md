---
title: Uso de grupos de dispositivos en una aplicación de Azure IoT Central | Microsoft Docs
description: Como operador, aprenda a usar grupos de dispositivos en la aplicación de Azure IoT Central.
author: ellenfosborne
ms.author: elfarber
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 07f1df101442e8ae43b26cebc60c8452d475d0f3
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878984"
---
# <a name="use-device-groups-in-your-azure-iot-central-application-preview-features"></a>Uso de grupos de dispositivos en una aplicación de Azure IoT Central (características de versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este artículo se describe cómo usar grupos de dispositivos en la aplicación de Azure IoT Central como operador.

Un grupo de dispositivos es una lista de dispositivos que se agrupan entre sí porque cumplen algunos criterios especificados. Los grupos de dispositivos ayudan a administrar, visualizar y analizar los dispositivos a escala, agrupando dichos dispositivos en grupos lógicos más pequeños. Por ejemplo, puede crear un grupo de dispositivos para enumerar todos los dispositivos de aire acondicionado de Seattle para que, así, un técnico de Seattle pueda encontrar los dispositivos de los que es responsable. En este artículo se muestra cómo crear y configurar grupos de dispositivos.

## <a name="create-a-device-group"></a>Creación de un grupo de dispositivos

Para crear un grupo de dispositivos:

1. Elija **Grupos de dispositivos** en el menú de navegación de la izquierda.

1. Seleccione **+ Nuevo**.

    ![Grupo de dispositivos nuevo](media/howto-use-device-groups-pnp/image1.png)

1. Asigne al grupo de dispositivos un nombre que sea único en toda la aplicación. También puede agregar una descripción. Un grupo de dispositivos solo puede contener los dispositivos de una sola plantilla de dispositivo. Elija la plantilla de dispositivo que se usará con este grupo.

1. Cree la consulta para identificar los dispositivos del grupo de dispositivos, seleccionando para ello una propiedad, un operador de comparación y un valor. Puede agregar varias consultas y dispositivos que cumplan **todos** los criterios establecidos en torno al grupo de dispositivos. El grupo de dispositivos que cree será accesible para todas aquellas personas que tengan acceso a la aplicación, por lo que cualquier usuario podrá ver, modificar o eliminar el grupo de dispositivos.

    ![Consulta del grupo de dispositivos](media/howto-use-device-groups-pnp/image2.png)

    > [!NOTE]
    > El grupo de dispositivos es una consulta dinámica. Cada vez que vea la lista de dispositivos, puede haber diferentes dispositivos en ella. La lista depende de qué dispositivos cumplen actualmente los criterios de la consulta.

1. Elija **Guardar**.

## <a name="analytics"></a>Análisis

Los análisis de los grupos de dispositivos son lo mismo que la pestaña principal Análisis del menú de navegación izquierdo. Puede obtener más información sobre los análisis en el artículo sobre [creación de análisis](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar grupos de dispositivos en la aplicación de Azure IoT Central, este es el paso siguiente sugerido:

> [!div class="nextstepaction"]
> [Creación de reglas de telemetría](howto-create-telemetry-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
