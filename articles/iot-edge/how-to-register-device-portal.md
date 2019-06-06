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
ms.openlocfilehash: 16660fbed465cc70f16cde430024f33b8aa4350e
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66495353"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Registro de un nuevo dispositivo Azure IoT Edge desde Azure Portal

Para poder usar los dispositivos IoT con Azure IoT Edge, debe registrarlos con IoT Hub. Después de registrar un dispositivo, recibirá una cadena de conexión que puede usarse para configurar el dispositivo para cargas de trabajo de IoT Edge.

En este artículo se muestra cómo registrar un nuevo dispositivo IoT Edge mediante Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Una segunda oportunidad para estándar [IoT hub](../iot-hub/iot-hub-create-through-portal.md) en su suscripción de Azure.

## <a name="create-a-device"></a>Crear un dispositivo

En Azure Portal, los dispositivos IoT Edge se crean y administran por separado de los dispositivos que se conectan al centro de IoT pero que no están habilitados para Edge.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.
2. Seleccione **IoT Edge** en el menú.
3. Seleccione **Agregar un dispositivo IoT Edge**.
4. Proporcione un identificador de dispositivo descriptivo. Use la configuración predeterminada para generar automáticamente las claves de autenticación y conecte el dispositivo nuevo a su centro.
5. Seleccione **Guardar**.

## <a name="view-all-devices"></a>Ver todos los dispositivos

Todos los dispositivos habilitados para Edge que se conectan al centro de IoT se enumeran en la página **IoT Edge**.

## <a name="retrieve-the-connection-string"></a>Recuperación de la cadena de conexión

Cuando esté listo para configurar el dispositivo, necesitará la cadena de conexión que vincula el dispositivo físico con su identidad en el centro de IoT.

1. Desde el **IoT Edge** página en el portal, haga clic en el identificador de dispositivo en la lista de dispositivos IoT Edge.
2. Copie el valor de **Cadena de conexión (clave principal)** o **Cadena de conexión (clave secundaria)** .

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [implementar módulos en un dispositivo con Azure portal](how-to-deploy-modules-portal.md).
