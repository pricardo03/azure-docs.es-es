---
title: Uso de Azure Portal para configurar la carga de archivos | Microsoft Docs
description: Describe cómo usar Azure Portal para configurar el centro de IoT Hub con el fin de habilitar las cargas de archivo desde dispositivos conectados. Incluye información sobre cómo configurar la cuenta de Azure Storage.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: robinsh
ms.openlocfilehash: bd7cc37b8fc81fc9d4109826743f2243913d0604
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59047286"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Configuración de cargas de archivos de IoT Hub mediante Azure Portal

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Carga de archivos

Para utilizar [la funcionalidad de carga de archivos en IoT Hub](iot-hub-devguide-file-upload.md), primero debe asociar una cuenta de Azure Storage con su centro. Seleccione **Carga de archivos** para mostrar una lista de las propiedades de carga de archivos para el IoT Hub que se está modificando.

![Visualización de la configuración de carga de archivos de IoT Hub en el portal](./media/iot-hub-configure-file-upload/file-upload-settings.png)

* **Contenedor de almacenamiento**: Use el portal de Azure para seleccionar un contenedor de blobs en una cuenta de almacenamiento de Azure en su suscripción actual de Azure para asociar con IoT Hub. Si es necesario, puede crear una cuenta de Azure Storage en la hoja **Cuentas de almacenamiento** y el contenedor de blobs en la hoja **Contenedores**. IoT Hub genera automáticamente identificadores URI de SAS con permisos de escritura en este contenedor de blobs para los dispositivos que se utilizarán cuando se carguen archivos.

   ![Visualización de contenedores de almacenamiento para la carga de archivos en el portal](./media/iot-hub-configure-file-upload/file-upload-container-selection.png)

* **Recibir notificaciones de archivos cargados**: Habilitar o deshabilitar las notificaciones de carga de archivo mediante el botón de alternancia.

* **SAS TTL**(TTL SAS): este valor es el periodo de vida de los URI de SAS que Azure IoT Hub devuelve al dispositivo. Se establece en una hora de forma predeterminada, pero se puede personalizar con otros valores mediante el control deslizante.

* **File notification settings default TTL**(TTL predeterminado de configuración de notificación de archivos): el periodo de vida de una notificación de carga de archivos antes de que caduque. Se establece en un día de forma predeterminada, pero se puede personalizar con otros valores mediante el control deslizante.

* **File notification maximum delivery count**(Número máximo de entregas de notificaciones de archivo): el número de veces que Azure IoT Hub tratará de entregar una notificación de carga de archivos. Se establece en 10 días de forma predeterminada, pero se puede personalizar con otros valores mediante el control deslizante.

   ![Configuración de la carga de archivos de IoT Hub en el portal](./media/iot-hub-configure-file-upload/file-upload-selected-container.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las funcionalidades de carga archivos de IoT Hub, consulte [Carga de archivos con IoT Hub](iot-hub-devguide-file-upload.md) en la guía para desarrolladores.

Siga estos vínculos para más información sobre la administración de Azure IoT Hub:

* [Administración de identidades de dispositivos de Centro de IoT de forma masiva](iot-hub-bulk-identity-mgmt.md)
* [Métricas de IoT Hub](iot-hub-metrics.md)
* [Supervisión de operaciones](iot-hub-operations-monitoring.md)

Para explorar aún más las funcionalidades de IoT Hub, consulte:

* [Guía para desarrolladores de IoT Hub](iot-hub-devguide.md)
* [Implementación de IA en dispositivos perimetrales con Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
* [Protección total de la solución de IoT](../iot-fundamentals/iot-security-ground-up.md)
