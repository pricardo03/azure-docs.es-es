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
ms.openlocfilehash: 55dc712d323c43849fa3de23c83e29fc348a0f1e
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68403998"
---
<!-- This tells how to create a custom shared access policy for your IoT hub and get the connection string for it-->

Para crear una directiva de acceso compartido que conceda los permisos **Conexión del servicio** y **Lectura del Registro**, y obtenga una cadena de conexión para esta directiva, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), abra el centro de IoT. La manera más fácil de acceder al centro de IoT es seleccionar **Grupos de recursos**, seleccionar el grupo de recursos donde se encuentra el centro de IoT y después seleccionarlo en la lista de recursos.

2. En el panel de la izquierda del centro de IoT, seleccione **Directivas de acceso compartido**.

3. En el menú superior situado encima de la lista de directivas, seleccione **Agregar**.

4. En el panel **Agregar una directiva de acceso compartida**, escriba un nombre descriptivo para la directiva, por ejemplo *serviceAndRegistryRead*. En **Permisos**, seleccione **Lectura del Registro** y **Conectar el servicio**, y después seleccione **Crear**.

    ![Cómo agregar una nueva directiva de acceso compartido](./media/iot-hub-include-find-custom-connection-string/iot-hub-add-custom-policy.png)

5. En el panel **Directivas de acceso compartido**, seleccione la nueva directiva en la lista de directivas.

6. En **Claves de acceso compartido**, seleccione el icono de **Cadena de conexión -- clave principal** y guarde el valor.

    ![Recuperación de la cadena de conexión](./media/iot-hub-include-find-custom-connection-string/iot-hub-get-connection-string.png)

Para más información sobre las directivas de acceso compartido y los permisos de IoT Hub, vea [Permisos y control del acceso](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions).
