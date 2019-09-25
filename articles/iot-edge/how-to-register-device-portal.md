---
title: 'Registro de un dispositivo nuevo desde Azure Portal: Azure IoT Edge | Microsoft Docs'
description: Uso de Azure Portal para registrar un dispositivo IoT Edge nuevo y recuperar la cadena de conexión
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 7f3d0037bcf0fd33ae23c298679e3157046247cb
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70983539"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Registro de un nuevo dispositivo Azure IoT Edge desde Azure Portal

Para poder usar los dispositivos IoT con Azure IoT Edge, debe registrarlos con IoT Hub. Una vez que registre un dispositivo, recibirá una cadena de conexión que se puede usar para configurar el dispositivo para cargas de trabajo de IoT Edge.

En este artículo se muestra cómo registrar un nuevo dispositivo IoT Edge mediante Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Un [centro de IoT](../iot-hub/iot-hub-create-through-portal.md) gratuito o estándar en la suscripción de Azure.

## <a name="create-a-device"></a>Crear un dispositivo

En Azure Portal, los dispositivos IoT Edge se crean y administran por separado de los dispositivos que se conectan al centro de IoT pero que no están habilitados para Edge.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.
2. Seleccione **IoT Edge** en el menú.
3. Seleccione **Agregar un dispositivo IoT Edge**.
4. Proporcione un identificador de dispositivo descriptivo. Use la configuración predeterminada para la generación automática de claves de autenticación y para conectar el dispositivo nuevo a su centro.
5. Seleccione **Guardar**.

## <a name="view-all-devices"></a>Ver todos los dispositivos

Todos los dispositivos habilitados para Edge que se conectan al centro de IoT se enumeran en la página **IoT Edge**.

## <a name="retrieve-the-connection-string"></a>Recuperación de la cadena de conexión

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT.

1. En la página **IoT Edge** del portal, haga clic en el identificador de dispositivo en la lista de dispositivos de IoT Edge.
2. Copie el valor de **Cadena de conexión (clave principal)** o **Cadena de conexión (clave secundaria)** .

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar módulos en un dispositivo con Azure Portal](how-to-deploy-modules-portal.md).
