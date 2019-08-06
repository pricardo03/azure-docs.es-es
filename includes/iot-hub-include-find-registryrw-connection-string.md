---
title: archivo de inclusión
description: archivo de inclusión
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: a752427d1e5873f0a27fd231872e3a13b234d9ed
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68402336"
---
<!-- This tells how to get the connection string for the registryReadWrite shared access policy of your IoT hub -->

Para obtener la cadena de conexión de IoT Hub para la directiva **registryReadWrite**, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), abra IoT Hub.  La manera más fácil de acceder a IoT Hub es seleccionar **Grupos de recursos**, seleccionar el grupo de recursos donde se encuentra IoT Hub y después seleccionarlo en la lista de recursos.

2. En el panel de la izquierda de IoT Hub, seleccione **Directivas de acceso compartido**.

3. En la lista de directivas, seleccione la directiva **registryReadWrite**.

4. En **Claves de acceso compartido**, seleccione el icono de **Cadena de conexión: clave principal** y guarde el valor.

    ![Recuperación de la cadena de conexión](./media/iot-hub-include-find-registryrw-connection-string/iot-hub-get-connection-string.png)

Para obtener más información sobre las directivas de acceso compartido y los permisos de IoT Hub, consulte [Permisos y control del acceso](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions).
