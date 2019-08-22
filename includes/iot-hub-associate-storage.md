---
title: archivo de inclusión
description: archivo de inclusión
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/22/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: f88f6aa6daec342b24d165c4c4cc0a89a0805f7a
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69558490"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Asociación de una cuenta de Azure Storage a IoT Hub

Como la aplicación del dispositivo simulado carga un archivo en un blob, debe tener una cuenta de [Azure Storage](../articles/storage/common/storage-quickstart-create-account.md) asociada al centro de IoT. Al asociar una cuenta de Azure Storage a un centro de IoT Hub, el centro genera un URI de SAS. Un dispositivo puede usar este URI de SAS para cargar de forma segura un archivo en un contenedor de blobs. El servicio IoT Hub y los SDK de dispositivo coordinan el proceso que genera el URI de SAS y permite que esté disponible para un dispositivo para cargar un archivo.

Siga las instrucciones que se detallan en [Configure file uploads using the Azure portal](../articles/iot-hub/iot-hub-configure-file-upload.md) (Configuración de cargas de archivos mediante Azure Portal). Asegúrese de que hay un contenedor de blobs asociado a su centro de IoT Hub y que las notificaciones de archivo están habilitadas.

![Habilitación de las notificaciones de archivo en el Portal](./media/iot-hub-associate-storage/file-notifications-vs2019.png)
